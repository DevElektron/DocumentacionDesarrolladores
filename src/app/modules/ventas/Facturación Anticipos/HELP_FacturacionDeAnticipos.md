# 📦 Módulo: 
#### 📁 **Código:** `Modules/Ventas/facturacionAnticipos`
#### 💻 **Menú:** Ventas > Facturación de Anticipos  [Ver en QA](http://192.168.2.16:1089/app/ventas/facturacionanticipos)

## 📝 Descripción
Éste módulo permite la consulta de las facturas de anticipos así como sus detalles y observaciones.
También, permite la captura de anticipos tanto para cliente mostrador y cliente normal.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                         | Rol permitido |
|---------|-------------------|-------------------------------------|----------------|
| Botón   | Añadir anticipo   | Abre ventana de captura de anticipo |                |

## 💼 Políticas Generales
-  Para poder capturar, el usuario debe tener un almacén asignado
-  El almacén se asignará automáticamente dependiendo el almacén asignado al usuario y no se podrá modificar
-  La fecha será la actual y no se podrá modificar
-  La caja se asignará automáticamente dependiendo el almacén asignado al usuario y no se podrá modificar
-  Uso de Cfdi, forma de pago, método de pago y tipo de moneda se deben llenar con información del cliente seleccionado
-  Los campos con * son obligatorios y de no tener el valor necesario, no se podrá presionar `Guardar`

## 🧪 Casos de Prueba

### Capturar Factura de Anticipo (Normal)
#### 💼 Operación
- [ ] No se permite guardar folios duplicados
- [ ] No se permiten guardar facturas con datos inválidos
#### 🛡️ Validaciones
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Almacén
    - Fecha documento
    - Caja
    - Cliente
    - Uso de Cfdi
    - Forma de pago
    - Método de pago
    - Tipo de moneda
    - Vendedor
    - Importe
    - Descripción (Debe tener un minimo de 10 caracteres y válida que no sea solo simbolos al azar)
- [ ] Se debe seleccionar un cliente válido, ya sea dando clic en el registro o haciendo tabulador al ingresar el número de cliente
- [ ] Se debe seleccionar un vendedor válido, ya sea dando clic en el registro o haciendo tabulador al ingresar el número de vendedor

### Capturar Factura de Anticipo Cliente Mostrador
#### 💼 Operación
- [ ] Mismas operaciones que en la captura normal
- [ ] Al ingresar un cliente mostrador (CTE:MODIF_VENTA == 1) se deberá abrir un modal donde se pueda modificar la información del cliente
- [ ] Al ingresar un cliente, aparecerá un botón azul en la parte inferior izquierda del modal, al dar clic también se habrirá el modal.
- [ ] En el nuevo modal, al seleccionar la opción de "Seleccionar datos de registro previo" deberá abrir un nuevo modal donde se podrá seleccionar información previamente registrada para el cliente actual.
#### 🛡️ Validaciones
- [ ] Mismas válidaciones que captura normal
- [ ] Ventana "Captura los datos del cliente para la impresión de la factura" se deben ingresar por lo menos los siguientes datos
    - Nombre
    - Nombre Cliente SAT
    - Domicilio
    - Colonia
    - Ciudad
    - Municipio
    - Estado
    - Código Postal
    - Método de pago
    - Régimen Fiscal
- [ ] Ventana "Seleccionar cliente de mostrador"
    - Se debe seleccionar un registro de la tabla si es que existe        
  

## 📎 Observaciones adicionales
- Al momento de guardar la factura de anticipos, el sistema deberá preguntar:
    - ¿Desea imprimir la factura del anticipo?
- Deberá abrir un modal donde muestre las cuentas de correo a donde será enviado el documento y dará la opción de añadir uno o más correos.
    - Validará que sean correos válidos y que esten separados por coma en caso de que sea más de uno
- Deberá mostrar un modal con el folio generado y el número de caja donde se creo
- En caso de que en el proceso de guardado de la factura existieron errores o advertencias, el sistema deberá mostrarlos al usuario
- Deberá preguntar: ¿Éste Anticipo es Para un Pedido en Específico?
    - En caso de decir que sí, se abrirá un modal donde se podrán vincular uno o varios pedidos a la facura
- Deberá mostrar si el proceso fue terminado con éxito o no     

> 🗓️ **Fecha de última modificación:** 2025-08-05
> 👤 **Luis Guillermo Pérez Fuentes**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/02 | GP |Entrega|







