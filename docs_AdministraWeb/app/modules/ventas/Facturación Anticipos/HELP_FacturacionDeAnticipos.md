# 📦 Módulo: Facturación de Anticipos
#### 📁 **Código:** `Modules/Ventas/facturacionAnticipos`
#### 💻 **Menú:** Ventas > Facturación de Anticipos  [Ver en QA](http://192.168.2.16:1089/app/ventas/facturacionanticipos)

## 📝 Descripción  
Este módulo permite consultar las facturas de anticipos, incluyendo sus detalles y observaciones.  
También permite capturar anticipos tanto para clientes de mostrador como para clientes regulares.

## 🔐 Seguridad  
| Tipo UI | Elemento                                      | Descripción                            | Rol permitido                   |
|---------|-----------------------------------------------|----------------------------------------|---------------------------------|
| Botón   | Añadir anticipo                               | Abre la ventana de captura de anticipo |                                 |
| Botón   | Desvincular Pedidos del Anticipo Seleccionado | Desvincula pedidos del anticipo        | Vendedor / Vendedor Mostrador * |

> * Se podrá ver el botón, pero si el rol no es cualquiera de los indicados producirá error al realizar la acción: _**Acceso Denegado**_.

## 💼 Políticas Generales  
1. Para poder capturar, el usuario debe tener un almacén asignado.  
2. El almacén se asignará automáticamente con base en el almacén asociado al usuario y no podrá ser modificado.  
3. La fecha será la actual y no podrá ser modificada.  
4. La caja se asignará automáticamente de acuerdo con el almacén del usuario y no podrá ser modificada.  
5. El uso de CFDI, la forma de pago, el método de pago y el tipo de moneda deben llenarse con la información del cliente seleccionado.  
6. Los campos marcados con * son obligatorios. Si no se capturan correctamente, no se podrá presionar el botón `Guardar`.

## 🧪 Casos de Prueba  

### 1. Captura de Factura de Anticipo (Cliente Regular)  

#### 💼 Operación  

- [ ] No se permite guardar folios duplicados.  
- [ ] No se permite guardar facturas con datos inválidos.

#### 🛡️ Validaciones  

- [ ] Se debe capturar al menos la siguiente información obligatoria:  
  - Almacén  
  - Fecha del documento  
  - Caja  
  - Cliente  
  - Uso de CFDI  
  - Forma de pago  
  - Método de pago  
  - Tipo de moneda  
  - Vendedor  
  - Importe  
  - Descripción (mínimo 10 caracteres; no debe contener únicamente símbolos aleatorios)  
- [ ] Se debe seleccionar un cliente válido, ya sea haciendo clic en el registro o presionando Tab al ingresar el número de cliente.  
- [ ] Se debe seleccionar un vendedor válido, ya sea haciendo clic en el registro o presionando Tab al ingresar el número de vendedor.

### 2. Captura de Factura de Anticipo (Cliente Mostrador)  

#### 💼 Operación  

- [ ] Se aplican las mismas operaciones que en la captura de cliente regular.  
- [ ] Al ingresar un cliente mostrador (`CTE:MODIF_VENTA == 1`), deberá abrirse un modal para modificar los datos del cliente.  
- [ ] Al seleccionar un cliente, aparecerá un botón azul en la parte inferior izquierda del modal; al hacer clic en él, también se abrirá el mismo modal.  
- [ ] En el nuevo modal, al seleccionar la opción "Seleccionar datos de registro previo", deberá abrirse un segundo modal donde se pueda elegir información previamente registrada del cliente.

#### 🛡️ Validaciones  

- [ ] Se aplican las mismas validaciones que en la captura de cliente regular.  
- [ ] En la ventana “Captura los datos del cliente para la impresión de la factura”, deberán capturarse al menos los siguientes campos:  
  - Nombre  
  - Nombre del Cliente SAT  
  - Domicilio  
  - Colonia  
  - Ciudad  
  - Municipio  
  - Estado  
  - Código Postal  
  - Método de pago  
  - Régimen Fiscal  
- [ ] En la ventana "Seleccionar cliente de mostrador", se debe seleccionar un registro de la tabla (si existen registros).

### 3. Desvincular pedidos de un anticipo

#### 💼 Operación  

- [ ] 1. Entra al sistema con un usuario que no tenga el rol de `Vendedor` o `Vendedor Mostrador`.
- [ ] 2. Entra a la ruta de Factura de Anticipos.
- [ ] 3. En la parte superior derecha, aparecerá los botones de acción, ubica el botón amarillo _Desvincular Pedidos del Anticipo Seleccionado_.
- [ ] 4. Selecciona un anticipo de la tabla principal, da clic en _Desvincular Pedidos del Anticipo Seleccionado_ y te mostrará un mensaje de error: _Acceso Denegado_.
- [ ] 5. Cierra sesión con ese usuario, y entra con otro que tenga los roles permitidos (`Vendedor` o `Vendedor Mostrador`).
- [ ] 6. Repite los paso del 3 al 4, y esta vez te mostra uno de 2 mensajes:
      - **Desvinculación exitosa**: _El Anticipo {anticipo lFolio}-{anticipo nFolio} ha sido desvinculado de los Pedidos exitosamente_.
      - **Desvinculación no realizada**: _No existen anticipos relacionados por Pedido de la Factura {anticipo lFolio}-{anticipo nFolio}_.
- [ ] 7. Actualiza la pestaña del navegador, y _sin seleccionar un anticipo_, intenta dar clic en el botón _Desvincular Pedidos del Anticipo Seleccionado_; no podrás dar clic, ya que el botón estará bloqueado hasta que hayas seleccionado un anticipo.

#### 🛡️ Validaciones  

- [ ] Botón _Desvincular Pedidos del Anticipo Seleccionado_ visible para todos los roles que tengan acceso a _Facturación de Anticipo_.  
- [ ] Mensaje de acceso denegado cuando el usuario tenga un rol no permitido.
- [ ] Mensaje de éxito cuando un anticipo haya tenido pedidos.
- [ ] Mensaje de error cuando un anticipo no haya tenido pedidos.
- [ ] Botón _Desvincular Pedidos del Anticipo Seleccionado_ inhabilitado de clics cuando no se haya seleccionado un anticipo.

## 📎 Observaciones adicionales  
- Al guardar la factura de anticipo, el sistema deberá preguntar:  
  **¿Desea imprimir la factura del anticipo?**  
- Se deberá mostrar un modal con las cuentas de correo a las que se enviará el documento, permitiendo añadir una o más direcciones.  
  - Se validará que los correos sean válidos y estén separados por comas en caso de haber varios.  
- Se mostrará un modal con el folio generado y el número de caja donde se creó.  
- En caso de errores o advertencias durante el proceso de guardado, el sistema deberá mostrarlos al usuario.  
- Se deberá preguntar: **¿Este anticipo es para un pedido en específico?**  
  - Si la respuesta es afirmativa, se abrirá un modal que permitirá vincular uno o más pedidos a la factura.  
- Se deberá notificar si el proceso se completó con éxito o si hubo algún fallo.

> 🗓️ **Fecha de última modificación:** 2025-08-05  
> 👤 **Luis Guillermo Pérez Fuentes, Sergio Ernesto Tostado Sánchez**  
> 🏷️ **Versión:** 2

---

# Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪ | 2025/07/02 | GP  |Entrega                       |
|⏪ | 2026/05/15 | ST  |Desvinculación de pedidos     |
