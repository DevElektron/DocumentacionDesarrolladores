# 📦 Módulo: Timbrador
#### 📁 **Código:** `Modules/Timbrador/Timbrador`
#### 💻 **Menú:** Ventas > Facturación de anticipos > Botón "Timbrar el documento seleccionado" [Ver en QA](http://192.168.2.16:1089/app/ventas/facturacionanticipos)
---

## Datos Generales
<details>
<summary> Ver aquí la documentación </summary>

## 📝 Descripción
Backend del timbrado de documentos CFDi en el SAT, a través del API Rest de CEPDI.

## 🔐 Seguridad
|Tipo UI|Elemento|Descripción|Rol permitido|
|:---|:---|:---|:---|
|Botón|Timbrar el documento seleccionado|Ejecuta el timbrado del documento seleccionado|Heredado|

## 💼 Políticas Generales
- El documento a timbrar, NO debe estar timbrado y estar dentro de las 72 horas para facturas y notas de abono y 10 días del mes siguiente para pagos de cliente.
- No se permite timbrar documentos de clientes sin RFC, éstos se incluyen en una factura global.
- Para el timbrado a través del API Rest de CEPDI, es el PAC el que incluye el sello del emisor del documento. Para la configuración de la bóveda del PAC, consultar: [**Manual de carga de CSD (PDF)**](./PRD_Carga_de_CSD_portal_facturación_CEPDI_Multi_RFC.pdf)

## 🧪 Casos de Prueba

#### 💼 Operación
- [x] No se permite timbrar un documento previamente timbrado.
- [x] Para timbrar un documento, primero se elabora éste como XML y se codifica en base 64, para ésto, se procesa el documento con el servicio del módulo del generador de XMLs (GeneradorXML).
- [x] Existe un registro de timbrado en curso para procesos iniciados de timbrado en diferentes estaciones, éste se encuentra en la bitácora de mensajes (Bitacora_Mensa) y se libera 2 horas después de iniciado el timbrado en la primera instancia.
- [x] Existe un candado para la validación del ambiente de timbrado contra la base de datos productiva, en donde, si se detecta una anomalía, se envía una alerta de ajuste de datos para el timbrado real. Hay que ajustar los datos en el registro de iniciales para liberarlo.

#### 🛡️ Validaciones
- [ ] Los datos requeridos para ejecutar cualquier timbrado de documento en el SAT, versión CFDi 4.0, son:
	- RFC del receptor (puede ser genérico)
	- Nombre fiscal del receptor (para el genérico, se usa la leyenda PÚBLICO GENERAL)
	- Domicilio fiscal del receptor (es el código postal del cliente)

---

#### Timbrar la factura de venta
1. Se captura una factura de venta en el menú Ventas > Facturación > Botón "Nueva factura".
2. El proceso se encarga de timbrar los documentos en segundo plano.

#### Timbrar la factura de anticipo
1. Entrar en menú Ventas > Facturación de anticipos.
2. Seleccionar un documento del listado.
3. Dar clic en el botón "Timbrar el documento seleccionado".

#### Timbrar la factura de activo

---

#### Timbrar nota de abono (1) por cancelación de factura de venta

#### Timbrar nota de abono (2) por cancelación de factura de anticipo / activo

#### Timbrar nota de abono (3) por devolución de mercancía

#### Timbrar nota de abono (4) por descuento
##### &rarr; Descuento especial directo en detalle de la factura de venta
1. Se captura una factura de venta en el menú Ventas > Facturación > Botón "Nueva factura".
2. Se define uno o varios descuentos en los detalles de la factura de venta.
3. El proceso se encarga de timbrar los documentos en segundo plano.

#### Timbrar nota de abono (6) por aplicación de factura de anticipo a factura de venta
1. Se captura una factura de venta en el menú Ventas > Facturación > Botón "Nueva factura".
2. Se captura la cantidad en moneda de uno o más anticipos fiscales ligados al cliente.
3. El proceso se encarga de timbrar los documentos en segundo plano.

#### :fa-info-circle: Los documentos sin definición de ruta de ejecución, están listos en BackEnd y pendientes de implementación en FrontEnd.

---

## 📎 Observaciones adicionales
- Existen 2 ambientes proporcionados por CEPDI, uno demo y otro productivo.

- Existe un manual de carga de CSD para la generación del sello de parte de CEPDI en el presente repositorio, en la siguiente liga: [PRD_Carga_de_CSD_portal_facturación_CEPDI_Multi_RFC.pdf](./PRD_Carga_de_CSD_portal_facturación_CEPDI_Multi_RFC.pdf)


> 🗓️ **Fecha de última modificación:** 2026-02-20
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

</details>



## Pruebas
<!--
Para mantener desplegada una sección en la vista previa, agregar open a la etiqueta de apertura de detalles.
Ej.: <details open>...</details>
-->
<details> <summary> Revisión 202512281104-IC </summary> 

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Completa | Timbrado de factura de venta en ambiente de pruebas | 202602150830-IC | |

</details>
<details> <summary> Acotaciones </summary>

| Estatus | Descripción |
|:---|:---:|
| Pendiente | No se ha iniciado la revisión |
| Iniciado | Revisión Iniciada |
| Error | Errores en la revisión (Revisar la sección de errores) |
| Completa | Revisión completada satisfactoriamente |

</details>

## Errores
<details> <summary> Revisión 202602170800-IC </summary>
	
| Error | Severidad | Pasos para reproducir | Comportamiento actual | Comportamiento esperado | Evidencia | Usuario contacto | Fecha corrección | Notas / comentarios |
|:---|:---:|:---|:---|:---|:---:|:---|:---:|:---|
|Error al propagar variable de endpoint|Grave|Se descarga la rama a local y al compilar docker, manda error|||||20260218||

</details>
