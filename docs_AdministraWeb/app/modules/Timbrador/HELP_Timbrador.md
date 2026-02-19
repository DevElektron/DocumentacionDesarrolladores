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

## 🧪 Casos de Prueba

#### 💼 Operación
- [ ] No se permite cancelar un documento previamente cancelado.

#### 🛡️ Validaciones
- [ ] Los datos requeridos para ejecutar cualquier cancelación en el SAT, son:
	- UUID
	- RFC del receptor (puede ser genérico)
	- Total del documento

#### Cancelar nota de abono por aplicación de factura de anticipo a factura de venta.
1. Se hace factura de ANTICIPO
2. Se hace factura de VENTA y se relaciona el anticipo anterior
	Ésta operación, genera una nota de abono relacionada a la factura de VENTA para equilibrar los movimientos
3. Se cancela la factura de VENTA
   ---> Aquí estamos, al cancelar la fac. de venta, se debe cancelar la NA relacionada (cancelación del presente documento)

#### Cancelación de factura de activo
1. Se elabora una factura de activo.
2. Se cancela la factura de activo.

#### Cancelación de pago de cliente
1. Se captura un pago de cliente.
2. Se cancela el pago de cliente.

## 📎 Observaciones adicionales
- Existen 2 ambientes proporcionados por CEPDI, uno demo y otro productivo.
	- El ambiente demo, responde siempre lo mismo, para cambiar el tipo de respuesta recibida, hay que contactar al personal de CEPDI para solicitarlo.

- Existe un manual de consumo del API Rest en el presente repositorio, en la siguiente liga: [WSE_Manual_CancelaCFDi_API_Rest.pdf](./WSE_Manual_CancelaCFDi_API_Rest.pdf)

> 🗓️ **Fecha de última modificación:** 2026-02-19
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

</details>


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
