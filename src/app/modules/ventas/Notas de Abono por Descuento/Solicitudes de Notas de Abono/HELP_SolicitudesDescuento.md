# 📦 Módulo: 
#### 📁 **Código:** `Modules/Ventas/NotasAbonoDescuento/SolicitudesDescuento`
#### 💻 **Menú:** Menú > Ventas > Notas de Abono por Descuento > Solicitudes de Notas de Abono  [Ver en QA](http://192.168.2.16:1089/app/ventas/notasabonodescuento/solicitudesdescuento)

## 📝 Descripción
Éste módulo permite consultar las solicitudes de descuento. Adicionalmente permite crear, modificar y eliminar los registros del mismo modulo, así como también permite autorizar las solicitudes de notas de abono pendientes de autorización. 

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|---------------|
| Botón   | Añadir      | Permite capturar una solicitud de descuento |        |
| Botón   | Modificar   | Permite modificar una solicitud de descuento |        |
| Botón   | Eliminar    | Permite eliminar una solicitud de descuento |        |
| Botón   | Notas de Abono <br> por Autorizar | Permite visualizar una pantalla con las <br> solicitudes pendientes de autorización |        |

## 💼 Políticas Generales
- El Estado de la solicitud debe de ser distinto a "Aplicada" para poder modificarlo o eliminarlo

## 🧪 Casos de Prueba

### Modal Notas de Abono por autorizar
#### 💼 Operación
- A partir de los almacenes asociados al gerente del vendedor actualmente logueado en el sistema, se obtienen las facturas pendientes de autorización. Para ser consideradas, estas facturas deben encontrarse en estado "No Autorizada".


### Modal Act de Solicitus
#### 🛡️ Validaciones
- Se debe de seleccionar una Factura Origen para poder seleccionar la Factura Destino
- El subtotal capturado  debe ser menor que el de la Factura Origen.
- El importe total debe ser mayor a cero.
- El importe total capturado debe ser menor que el de la Factura Origen.
- No se puede seleccionar una Factura Origen que se encuentra "Cancelada"
- Si la Factura Origen es un Anticipo Facturado, la Factura Destino será la misma

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-07-09
> 👤 **Eduardo Navarro**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩| 2025/07/31 | EN |Entrega del modulo|
|⏪|  |  |  |
