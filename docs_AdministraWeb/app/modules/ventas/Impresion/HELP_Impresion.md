# 📦 Módulo: Impresión (Orquestador de PDFs)

#### 📁 **Código:** `BackVentas/Modulo/Impresion`

#### 💻 **Menú:** Acceso transversal desde Ventas, Pagos y Notas de Abono

## 📝 Descripción

Este módulo centraliza la lógica de generación de documentos PDF para el ecosistema de **BackVentas**. Utiliza un patrón de **Factory** y **Proveedores de Datos (DataProviders)** para extraer información de la base de datos (SQL Server) y archivos XML fiscales, permitiendo generar copias múltiples (Original, Copia, etc.) y unirlas en un solo archivo mediante `PdfSharp`.

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
| --- | --- | --- | --- |
| API Endpoint | `pdf/generar` | Genera y retorna el documento PDF solicitado | Usuarios con acceso a módulos de origen |
| Lógica Interna | Headers Propagados | Se utilizan para la trazabilidad y obtención de datos del usuario | N/A |

## 💼 Políticas Generales

* **Manejo de Copias:** El sistema permite solicitar múltiples tipos de copias (ej: "Original", "Copia Archivo, etc"). Si no se especifica ninguna, el sistema genera por defecto la copia "Original".
* **Integridad Fiscal:** Para documentos timbrados (Pagos y Notas de Abono), la información de saldos y UUIDs se extrae prioritariamente del **XML** para garantizar precisión fiscal. Si no existe XML, se recurre a los datos de la base de datos (ELDCHC/ELCHC).
* **Unión de Documentos:** Cuando se solicitan múltiples copias, el servicio utiliza `PdfSharp` para realizar un *merge* de los bytes de cada página y entregar un único archivo descargable.
* **Localización:** Los cálculos de tiempo (como en manifiestos) utilizan el timezone `America/Mexico_City`.

## 🧪 Casos de Prueba

### 1. Generación de PDF de Pagos (CFDI 4.0 / Pagos 2.0)

#### 💼 Operación

* [ ] Solicitar impresión de un pago con folio específico (Serie/Folio).
* [ ] El sistema debe recuperar el encabezado de `Elchcs` y los datos fiscales de `Elchcfs`.
* [ ] Si el pago está timbrado, debe parsear el XML para obtener los `DoctoRelacionado`.

#### 🛡️ Validaciones

* [ ] **Error de existencia:** Si el pago no existe, debe retornar: `"No se encontró el pago {lfolio}-{nfolio}"`.
* [ ] **Cadena Original:** Debe construirse siguiendo el estándar: `||Version|UUID|FechaTimbrado|RfcProvCertif|SelloCFD|NoCertificadoSAT||`.
* [ ] **QR:** El string del QR debe incluir el enlace de validación del SAT con el UUID, RFC Emisor/Receptor, Total y los últimos 8 dígitos del Sello.

### 2. Generación de PDF de Ventas y Activos

#### 💼 Operación

* [ ] Solicitar impresión de una factura de venta o un detalle de activos.
* [ ] Para activos, el sistema debe realizar un JOIN entre `Elfacas` y `Elfacs`.
* [ ] El sistema debe proyectar los datos al DTO `ActivoPdfItemDto` ordenados por `Npartida`.

#### 🛡️ Validaciones

* [ ] **Mapeo de Detalles:** Se debe validar que el nodo JSON "Detalles" se deserialice correctamente hacia `List<VentaPdfPartidaDto>`.
* [ ] **Datos de Control:** Si no se encuentran los datos de la empresa (`Elctrl`), el sistema lanza: `"Datos de control ('Elctrl') no encontrados"`.

#### 🛡️ Validaciones

* [ ] **Cálculo de Precios:** Se debe validar que el precio de lista incluya el descuento aplicado: `d.Prclista - (d.Prclista * (d.Prdescuento / 100))`.

## 📎 Observaciones adicionales

* El módulo utiliza un `PdfServiceFactory` para determinar dinámicamente qué servicio de impresión utilizar según el `tipoDocumento` enviado en la petición.
* Los archivos PDF generados se entregan como un objeto `AttachmentDto` que contiene el contenido en bytes y el `ContentType` correspondiente.




# [FEATURE: Módulo de Impresión Centralizada (PDF Orchestrator)]

[Motivo del PR]:

* **[ADD]**: Implementación del controlador `ImpresionController` para centralizar la generación de documentos PDF del área de Ventas.
* **[ADD]**: Creación de la arquitectura de `DataProviders` para separar la lógica de extracción de datos de la generación del documento.
* **[IMP]**: Integración de `PdfSharp` para permitir la unión de múltiples copias (Original/Copias) en un solo archivo descargable.

[Breve descripción de la funcionalidad de Módulo / Bug / Fix / Feature]:
**Servicio orquestador encargado de la generación y entrega de archivos PDF para los módulos de Ventas, Anticipos, Activos, Notas de Abono y Cotizaciones, garantizando precisión fiscal mediante el consumo de datos SQL y archivos XML.**

### Puntos clave

* **Arquitectura Extensible**: Se implementó `PdfServiceFactory` para seleccionar dinámicamente el servicio de generación según el tipo de documento solicitado (venta, pago, activo, etc.).
* **Lógica Fiscal**: Para los módulos de Pagos y Notas de Abono, el sistema parsea directamente el XML timbrado para asegurar que los UUIDs y saldos coincidan con el SAT.
* **Gestión de Copias**: El orquestador procesa una lista de títulos de copia, genera cada una de forma independiente y las fusiona en un solo stream de bytes para el usuario final.
* **Desacoplamiento**: El uso de `IPdfDataProvider` permite que cada módulo defina sus propias reglas de obtención de datos sin afectar al motor de impresión principal.
* Ruta principal: ***`api/impresion`***
* Endpoints:

1. **[POST][pdf/generar]**: Genera el documento basado en el tipo de factura, serie/folio y lista de copias solicitadas.

### Nuevos elementos de ayuda del proyecto

***Helpers***

* **BackVentas.Modulo.Impresion.Application.Services.VentaCommonData**: Centraliza la obtención de información común entre facturas de venta y anticipos.
* **BackVentas.Infrastructure.Repositories.ActivoRepository**: Provee acceso a los detalles de activos asociados a folios de factura.

***Otro recurso de ayuda***

* **IPdfGeneratorService**: Interfaz estándar para la implementación de nuevos generadores de documentos.

> NOTA: Es indispensable contar con la librería `PdfSharp` en el microservicio de Ventas para que la unión de páginas PDF funcione correctamente.



> 🗓️ **Fecha de última modificación:** 2025-12-29
> 👤 **Samuel Valles Sanchez**
> 🏷️ **Versión:** 1




