# 📦 Módulo: Registro Ventas
#### 📁 **Código:** `modules/ventas/ventas/components/act-ventas`
#### 💻 **Menú:** Ventas > Consulta de Facturas  [Ver en QA](http://192.168.2.16:1089/app/ventas/consultadefacturas)

## 📝 Descripción  
Este módulo permite realizar el registro de nuevas ventas a clientes, puede importar pedidos y facturas existentes, timbra, envía por correo electronico y descarga documentos.

## 🔐 Seguridad  
| Tipo UI | Elemento        | Descripción                          | Rol permitido |
|---------|-----------------|--------------------------------------|----------------|
| Botón   | Añadir venta    | Abre la ventana de captura de ventas |                |

## 💼 Políticas Generales  
1. Para poder capturar, el usuario debe tener un almacén asignado.  
2. El almacén de facturación se asignará automáticamente con base en el almacén asociado al usuario y no podrá ser modificado.  
3. El almacén de salida podrá ser modificada por el usuario pero se asigna automáticamente con base a la información del usuario.
4. La fecha será la actual y no podrá ser modificada.  
5. Cuando se capture un correo electronico en el campo recibe, el sistema deberá buscar si este existe, de ser así sustituira el correo por el nombre del recibidor, de no encontrarlo pedirá ingresar uno nombre, no un correo electrónico.  
6. El uso de CFDI, la forma de pago, el método de pago y el tipo de moneda deben llenarse con la información del cliente seleccionado. 
7. El cliente debe contar con un monto de crédito y este no debe ser rebasado, solo si el cliente lo tiene permitido. 
8. Para las partidas, el usuario deberá ingresar primero una cantidad de artículo, después buscar el artículo en el autocomplete, al seleccionar uno, el sistema validara existencias y asignara valores a la partida correspondiente.
9. Cuando la partida es válida, se harán los calculos necesarios para obtener los totales. 
10. El usuario puede navegar por la tabla de detalles usando las flechas de navegación del teclado.
11. Las partidas cuentan con iconos, los cuales tienen diferentes funcionalidades, tales como: abrir modal de tramos (si aplica), agregar descripciones a la partida, eliminar partida, ver imagen del artículo y ver la ficha tecnica.
12. El usuario puede cambiar los valores de precio y de descuento del artículo siempre y cuando pase las validaciones necesarias.
13. Cada artículo puede tener o no artículos relacionados y sugeridos, los cuales se muestran en la parte derecha de la pantalla.
14. En la segunda pestaña, se muestran los anticipos del cliente (si aplica) y aquí el usuario puede elegir los montos a aplicar de los anticipos.
15. Los campos marcados con * son obligatorios. Si no se capturan correctamente, no se podrá presionar el botón `Guardar`.

## 🧪 Casos de Prueba  

### Captura de Factura de Venta (Venta Regular)  
#### 💼 Operación  
- [ ] No se permite guardar folios duplicados.  
- [ ] No se permite guardar facturas con datos inválidos.

#### 🛡️ Validaciones  
- [ ] Se debe capturar al menos la siguiente información obligatoria:  
  - Fecha factura  
  - Cliente  
  - Forma Pago  
  - Vendedor
  - Tipo de Pago
  - Uso de CFDI
  - Almacén de facturación
  - Almacén de Salida  
  - Tipo de moneda  
  - Forma de entrega
  - Plataforma pago (cuando el cliente es de venta en línea)
  - Cantidad
  - Artículo
  - Precio Lista
- [ ] Se debe seleccionar un cliente válido, ya sea haciendo clic en el registro o presionando Tab al ingresar el número de cliente.
- [ ] Se debe seleccionar un almacén salida válido, ya sea haciendo clic en el registro o presionando Tab al ingresar el número de almacén.  

### Captura de Factura de Anticipo (Cliente Mostrador)  
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
> 👤 **Luis Guillermo Pérez Fuentes**  
> 🏷️ **Versión:** 1
---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/02 | GP |Entrega|
