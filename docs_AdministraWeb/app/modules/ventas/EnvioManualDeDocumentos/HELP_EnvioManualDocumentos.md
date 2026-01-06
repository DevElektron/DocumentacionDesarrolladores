# 📦 Módulo: Envío Manual de Documentos

#### 📁 **Código Frontend:** `app/modules/ventas/envio-manual-documentos`

#### 📁 **Código Backend:** `BackVentas/Modulo/EnvioManualDeDocumentos`

#### 💻 **Menú:** Ventas > Envío Manual de Documentos [Ver en QA](http://192.168.2.16:1089/app/ventas/enviomanualdedocumentos)

## 📝 Descripción

Este módulo permite la gestión y envío masivo de comprobantes fiscales (Facturas, Notas de Abono y Complementos de Pago) vía correo electrónico. El sistema valida la existencia y el timbrado de los documentos antes de agregarlos a una cola de envío, genera los archivos PDF/XML correspondientes y los despacha en un único proceso hacia el destinatario final.

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
| --- | --- | --- | --- |
| Botón | Agregar Documento | Ejecuta la validación de existencia y timbrado en el backend | Ventas / Gerente |
| Botón | Procesar Envío | Dispara el orquestador de exportación y envío de correo | Ventas / Gerente |
| API | `x-nven` / `x-idRol` | Se utilizan los headers propagados para validar la sesión del usuario | Todos los roles |

## 💼 Políticas Generales

1. **Validación Previa:** No se permite agregar documentos a la lista si no existen en el ERP, no son CFDI o no han sido timbrados exitosamente.
2. **Restricción de Notas:** Las Notas de Abono con `Tmov = 5` (Eliminación de saldos pequeños) están restringidas para envío manual.
3. **Integridad de Archivos:** El sistema intenta recuperar el XML primero desde la base de datos (`ELFACF`, `ELFNAF`, `ELCHCF`). Si no se encuentra, busca en el directorio histórico de archivos del servidor según el año y tipo de ingreso/egreso.
4. **Manejo de Prefijos:** Los archivos generados en la carpeta `Envios` utilizan prefijos estándar: `FD_` (Facturas), `NC_` (Notas de Abono) y `PG_` (Pagos).
5. **Formato de Correo:** Es obligatorio capturar un correo electrónico válido para habilitar el proceso de envío.

## 🧪 Casos de Prueba

### 1. Validación e Inserción en Lista

#### 💼 Operación

* [ ] Capturar un Tipo (Factura), Serie (FMF) y Folio (21357).
* [ ] El sistema llama al endpoint `/validar-documento`.

#### 🛡️ Validaciones

* [ ] **Duplicidad:** Si el documento ya está en el grid, mostrar alerta: `"Este documento ya se encuentra en la lista de envío."`.
* [ ] **Timbrado:** Si el documento no está timbrado, el backend debe responder con error y el mensaje configurado en el DTO de validación.


### 2. Proceso de Exportación y Envío (Orquestación)

#### 💼 Operación

* [ ] Con una lista de documentos válida, capturar el correo destino y dar clic en "Procesar Envío".
* [ ] El backend debe invocar al `IPdfOrchestratorService` para generar cada PDF.

#### 🛡️ Validaciones

* [ ] **Generación de Archivos:** Se debe verificar que el PDF y el XML se depositen correctamente en la ruta `.../Envios/`.
* [ ] **Adjuntos:** El correo enviado debe contener exactamente el número de archivos (PDF + XML) correspondientes a los documentos procesados con éxito.
* [ ] **Mensaje Global:** Al finalizar, el sistema debe mostrar el resumen: `"Enviados OK: X, Errores: Y"`.

### 3. Recuperación de XML desde Histórico

#### 💼 Operación

* [ ] Procesar un pago cuyo XML no esté en el campo `Xml` de la tabla `ELCHCF`.

#### 🛡️ Validaciones

* [ ] **Búsqueda en Disco:** El sistema debe navegar a la carpeta `[DirectorioCFD]/[RFC]/[Año]/Pagos/` y copiar el archivo a la carpeta `Envios` para poder adjuntarlo.

## 📎 Observaciones adicionales

* El frontend utiliza `AG-Grid` para la gestión de la cola de documentos con eliminación reactiva.
* El servicio de backend `EnvioManualRepository` utiliza `ICorreoService` (fachada de `IEmailService`) para el despacho de correos.



### 4. Envío de Prueba

#### 💼 Operación

* [ ] 1. Ejecutar la función de prueba (botón demo, esta oculto en produccion) para precargar datos globales.
* [ ] 2. Verificar que el campo de correo se asigne automáticamente a: `ssanchez@elektron.com.mx`.
* [ ] 3. Validar que la tabla (AG-Grid) se cargue con los siguientes 3 registros de prueba:
* **Factura:** Serie `FMF`, Folio `21357`, Versión `4.0`.
* **Nota de Crédito:** Serie `HAN`, Folio `3331`, Versión `4.0`.
* **Pagos:** Serie `PC2`, Folio `432114`, Versión `4.0`.


* [ ] 4. Presionar el botón de **Procesar Envío**.

#### 🛡️ Validaciones

* [ ] **Mapeo de Tipos:** El sistema debe mapear correctamente la "Nota de Crédito" hacia el tipo de backend `Notaabono`.
* [ ] **Generación de Archivos:** Se debe verificar en el servidor la existencia de los archivos en la carpeta `Envios` con sus prefijos correspondientes:
* `FD_FMF021357.pdf` / `.xml`
* `NC_HAN003331.pdf` / `.xml`
* `PG_PC2432114.pdf` / `.xml`


* [ ] **Resumen Final:** Al terminar el proceso, el mensaje de alerta debe indicar: `Enviados OK: 3` y `Errores: 0`.
* [ ] **Estado de Interfaz:** La variable `procesoActual` debe transitar de `ENVIANDO...` a `FINALIZADO`.


# 📦 Plantilla para Pull Request (Frontend)

## [AdministraWeb Front]
# [FEATURE: Interfaz de Envío Manual de Documentos]

[Motivo del PR]:
- **[ADD]**: Pantalla de gestión de cola de envíos con formulario reactivo.
- **[ADD]**: Integración con AG-Grid para visualización de documentos a procesar.
- **[FIX]**: Validación asíncrona de documentos antes de su inserción en la lista local.

[Breve descripción de la funcionalidad]:
**Interfaz de usuario que permite capturar comprobantes fiscales por folio/serie, validarlos en tiempo real contra el backend y gestionar su envío por correo electrónico.**

### Puntos clave
- **AG-Grid**: Uso del tema `ag-theme-quartz` con acciones de eliminación por fila.
- **Validaciones UX**: Implementación de loaders durante la validación y el envío masivo para prevenir bloqueos de UI.
- **Integración API**: Consumo de endpoints de validación y exportación mediante `genericPostToken`.


# 🔧 Plantilla para Pull Request (API - Backend)


## [PROYECTO MIGRACIÓN ERP]
# [FEATURE: Módulo de Envío Manual de Documentos CFDI]

[Motivo del PR]:
- **[ADD]**: Implementación de `EnvioManualDeDocumentosController` para validación y procesamiento de envíos.
- **[ADD]**: Lógica de recuperación de XML histórica (DB -> Disco) en `EnvioManualRepository`.
- **[IMP]**: Integración con el orquestador de impresión para generar adjuntos dinámicos en el correo.

[Breve descripción de la funcionalidad]:
**Módulo que permite la validación, generación y envío masivo por correo electrónico de facturas, notas de abono y complementos de pago, automatizando la búsqueda de archivos XML y la unión con PDFs generados al vuelo.**

### Puntos clave
- **Validación Robusta**: Implementación de la lógica `Sr_Validacion` de Clarion para asegurar que solo documentos timbrados entren a la cola.
- **Orquestación de Adjuntos**: El sistema mapea automáticamente el tipo de documento para llamar al proveedor de datos (DataProvider) correcto del módulo de Impresión.
- **Fallback de XML**: Mecanismo de seguridad que busca el XML en disco si no existe en la base de datos para evitar envíos incompletos.

- Ruta principal: **_`api/ventas/enviomanualdedocumentos`_**
- Endpoints:
1. **[POST][validar-documento]**: Valida existencia, tipo de CFDI y estatus de timbrado.
2. **[POST][exportar-y-enviar]**: Genera archivos y despacha correo mediante SMTP.

### Nuevos elementos de ayuda
**_Helpers_**
- **BackVentas.Modulo.EnvioManualDeDocumentos.Application.Services.CorreoService**: Fachada para el envío de correos con múltiples adjuntos.


> 🗓️ **Fecha de última modificación:** 2025-12-29
> 👤 **Samuel Valles Sanchez**
> 🏷️ **Versión:** 1








