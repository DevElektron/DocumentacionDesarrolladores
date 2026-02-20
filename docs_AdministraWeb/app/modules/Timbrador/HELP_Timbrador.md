# 📦 Módulo: Timbrador
#### 📁 **Código:** `Modules/Timbrador/Timbrador`
#### 💻 **Menú:** Ventas > Facturación de anticipos > Botón "Timbrar el documento seleccionado" [Ver en QA](http://192.168.2.16:1089/app/ventas/facturacionanticipos)
---

#### Datos Generales
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

</details>


#### Timbrar factura de venta
1. Se captura una factura de venta en el menú Ventas > Facturación > Botón "Nueva factura".
2. El proceso se encarga de timbrar la factura en segundo plano.

#### Timbrar factura de anticipo
1. Se elabora una factura de anticipo en el menú Ventas > Facturación de anticipos > Botón "Timbrar el documento seleccionado"
2. Actualmente, NO está implementado el timbrado al finalizar la captura de éste documento.

#### Timbrar factura de activo

#### Timbrar nota de abono por descuento directo en detalle de la factura de venta
1. Se captura una factura de venta en el menú Ventas > Facturación > Botón "Nueva factura".
2. Se define uno o varios descuentos en los detalles de la factura de venta.
3. El proceso se encarga de timbrar las notas de abono en segundo plano.

# #### AQUÍ VOY # # # # #


#### Los documentos sin definición de ruta de ejecución, están listos en BackEnd y pendientes de implementación en FrontEnd.



## 📎 Observaciones adicionales
- Existen 2 ambientes proporcionados por CEPDI, uno demo y otro productivo.
	- El ambiente demo, responde siempre lo mismo, para cambiar el tipo de respuesta recibida, hay que contactar al personal de CEPDI para solicitarlo.

- Existe un manual de consumo del API Rest en el presente repositorio, en la siguiente liga: [WSE_Manual_CancelaCFDi_API_Rest.pdf](./WSE_Manual_CancelaCFDi_API_Rest.pdf)

> 🗓️ **Fecha de última modificación:** 2026-02-19
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1



#### Pruebas
<!-- Para mantener desplegada una sección en la vista previa, 
	agregar open a la etiqueta de apertura de detalles.
	Ej.: <details open>...</details>
-->
<details> <summary> Revisión 202602101600-IC </summary> 

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Completa | Cancelación de nota de abono por aplicación de anticipo en RespDiario | 202602101038-IC | |

</details>
<details> <summary> Revisión 202602151250-IC </summary> 

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Completa | Cancelación de factura de activo en RespDiario | 202602150830-IC | |

</details>
<details> <summary> Revisión 202602191629-IC </summary> 

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Completa | Cancelación de pago en RespDiario | 202602181235-IC | |

</details>
<details> <summary> Acotaciones </summary>

| Estatus | Descripción |
|:---|:---:|
| Pendiente | No se ha iniciado la revisión |
| Iniciado | Revisión Iniciada |
| Error | Errores en la revisión (Revisar la sección de errores) |
| Completa | Revisión completada satisfactoriamente |

</details>

### Errores
<details> <summary> Revisión 202602170800-IC </summary>
	
| Error | Severidad | Pasos para reproducir | Comportamiento actual | Comportamiento esperado | Evidencia | Usuario contacto | Fecha corrección | Notas / comentarios |
|:---|:---:|:---|:---|:---|:---:|:---|:---:|:---|
|Error al propagar variable de endpoint|Grave|Se descarga la rama a local y al compilar docker, manda error|||||20260218||

</details>

---
#### Otros ejemplos de control de QA
<details> <summary> Otros ejemplos de control de QA </summary>

## Para agregar a documentación: extender uso

## Checklist de Migración
- [x] Funcionalidades principales migradas
- [x] Datos persistentes correctos
- [ ] APIs responden igual que EXE
- [x] UI/UX similar
- [ ] Rendimiento aceptable
- [ ] Documentación actualizada

</details>
