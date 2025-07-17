# 📦 Módulo: Cancelar Facturas
#### 📁 **Código:** `Modules/Ventas/cancelarFacturas`
#### 💻 **Menú:** Ventas > Cancelar facturas  [Ver en QA](http://192.168.2.16:1089/app////)

## 📝 Descripción
Éste módulo permite la cancelacion de facturas y anticipos. Permite la modificacion de tramos y poner los prodcutos o detalles en observacion.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Cancelar factura      | Permite cancelar facturas      |   Ventas / Gerente     |
| Botón   | Cancelar anticipo   | Permite cancelar los anticipos    |     Ventas / Gerente   |

## 💼 Políticas Generales
- No se permiten cancelar facturas de venta que no correspondan al dia actual
- No se permite cancelar facturas de anticipo que no correspondan al corte actual
- Una vez cancelada una factura esta no se debe de poder cancelar
- El numero de tramos debe de coincidir con el numero de tramos en factura al momento de hacer modificaciones
- Al dar clic en la tecla F8 del teclado se debe de poner el detalle seleccionado en observacion

## 🧪 Casos de Prueba

### Eliminar ventas o anticipos
#### 💼 Operación
- [ ] No se permite cancelar facturas ya canceladas
- [ ] No se permite cancelar facturas de venta de dias anteriores
- [ ] No se permite cancelar facturas de anticipo de cortes anteriores
- [ ] Al momento de cancelar ventas o anticipos solo se puede modificar los conceptos de cancelación 
#### 🛡️ Validaciones
- [ ] Los tramos deben de coincidir con el total de tramos en detalles
- [ ] Se debe seleccionar un concepto de cancelacion para poder continuar
- [ ] En caso de que el motivo de cancelacion sea por falta de existencias, se debe indicar al menos un detalle en observacion 

## 📎 Observaciones adicionales
- N.A.
> 🗓️ **Fecha de última modificación:** 2025-07-15
> 👤 **Rodrigo Rangel Martinez**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
