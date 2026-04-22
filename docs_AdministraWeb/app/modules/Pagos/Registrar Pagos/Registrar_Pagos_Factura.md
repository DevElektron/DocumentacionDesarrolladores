# 📦 Módulo: Registrar Pagos Facturas
#### 📁 **Código:** `src\app\modules\controlPagosCorte\registrar-pagos-factura`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089//app/ctrl-pagos/registrar-pagos-factura)

## 📝 Descripción

Este módulo muestra la pantalla correspondiente a **Registrar Pagos**, cuyo objetivo es armar y guardar los registros de las Formas de Pago y sus datos para una factura cargada, teniendo 2 tipos de facturas a procesar (Anticipo y Otras) y un vasto número de validaciones para cada forma de pago así como de las cantidades totales. La pantalla se divide en 4 zonas:

1. **Zona de búsqueda de factura**, en donde se encuentran:
    - _CORTE:_ Es el número de Turno del corte asignado a un almacén por el día de hoy.
    - _Buscador de Facturas:_ El campo que te ayuda a seleccionar las facturas, teniendo la posibilidad de escanero o búsqueda manual.
    - _Botones de acción:_ Por el momento sólo 2, _Cancelar captura_ (limpia la ventana de todo valor ingresado) y _Manual Registrar Pagos_ (muestra una ventana de ayuda).
2. **Zona de Información de Factura**, que muestra:
    - _Folio de la factura seleccionada_, en donde se presenta un borde coloreado según el tipo de factura cargada, rojo = Otra, azul = Anticipo.
    - _Fecha de la factura_, formato _`día de la semana`, `no. día` `mes` `año`_.
    - _Forma de Pago_, `Contado` o `Crédito`.
    - _Ubicación original_, Estado de Entrega de la factura.
    - _Cliente_, Número y nombre registrado del cliente.
    - _Vendedor_, Número y nombre registrado del vendedor.
    - _Almacén de salida_.
3. **Zona de Registro de Formas de Pagos**, en donde se encuentra la tabla principal que se encuentra bloqueada si no has seleccionado una factura en el buscador.
4. **Zona de Totales**, en donde se encuentran:
    - _IMPORTE FACTURA_.  
    - _SALDO A PAGAR_.  
    - _ANTICIPO A GENERAR_.  
    - Parcializado
        - _MONTO_.  
        - _MENSUALIDADES_.  
    - Sin Parcializar
        _COMISIÓN_.  
    - _IMPORTE A PAGAR_.  
    - _RECIBÍ_.  
    - _CAMBIO_.  

## 🔐 Seguridad

| Tipo UI | Elemento                                   | Descripción                                                                        | Rol permitido                 |
|---------|--------------------------------------------|------------------------------------------------------------------------------------|-------------------------------|
| ventana | Mostrar opción principal                   | Permite mostrar la opción en el menú principal.                                    | Vendedor / Vendedor Mostrador |
| botón   | Aceptar (realizar pagos)                   | Confirma la información capturada para pagar la factura.                           | Vendedor / Vendedor Mostrador |
| botón   | Guardar (registrar cheque protegido)       | Guarda la información capturada en la ventana de Registro de Cheques Protegidos.   | Vendedor / Vendedor Mostrador |
| botón   | Aceptar (leer código de barras de N.A.*)   | Acepta el código de barras ingresado en la ventana de solicitud de Notas de Abono. | Vendedor / Vendedor Mostrador |

> *N.A. = Nota de Abono.

## 💼 Políticas Generales

<a id="politica-1-el-modulo-aparece-solo-para-roles-autorizados"></a>

### POLÍTICA 1. El módulo aparece sólo para roles autorizados

Para llegar a esta pantalla, sólo aparece la ruta para los roles _Vendedor / Vendedor Mostrador_, en la ruta del menú lateral de Administra Web _`Pagos > Registrar Pagos Factura`_. Si se requiere para otro rol, se deberá de agregar los permisos correspondientes vía _Security_.

<a id="politica-2-orden-de-pasos-para-pagar-factura"></a>

### POLÍTICA 2. Orden de pasos para pagar factura

**El proceso de registrar los pagos de una factura se puede resumir en lo siguiente:**

1. Buscar o escanear la factura a pagar.
2. La pantalla detecta el estado de la factura en el sistema (si ya fue entregada, tipo, información adicional), si detecta que puedes entregarla, el módulo lo puede hacer, de otra manera puedes cancelar la captura de pago o continuar.
3. Observa el tipo de factura segn la zona del folio (azul = Anticipo, rojo = Otra).
4. Una vez seleccionada la factura, se habilitará la tabla principal, ahora indica la o las formas de pago que el cliente pagará la factura:
    - 4.1 Cada Forma de pago tiene sus reglas, y dependiendo de la seleccionada puede requerirte algunas columnas e información adicional (como escanear Nota de Abono o registrar un Cheque Protegido).
    - 4.2 Cada fila que esté en la tabla principal tiene un icono en la primera columna, en donde además se encuentra el número de la fila, y el color del icono indica si es válida o no toda la información de la forma de pago; si es válida (icono en verde), te permitirá agregar otra fila tecleando `flecha abajo`, pero si no es válida (icono en rojo) no te permitirá agregar otra fila hasta que hayas corregido las indicaciones en rojo.
    - 4.3 Al hacer clic en una fila, si la forma de pago incluye parcialización, lo puedes ver en la zona de totales centrales, en las tarjetas amarillo y verde.
    - 4.4 Si eliges la forma de pago `EFECTIVO`, se habilitará el campo _RECIBÍ_, en la que tendrá que captura la cantidad de efectivo recibida para pagar el o los montos de las formas de pago en efectivo.
5. Al finalizar la captura de las formas de pago, da clic en el botón `Aceptar`, se aparecerá un mensaje de confirmación, da clic en _SÍ_ para confirmar la captura de pago y espera a que aparezca uno de 2 mensajes, ambos significan que la factura fue pagada, más hay algunas formas de pago que activan procesos después del pago:
    a. _**Éxito! Operación Realizada, Pago(s) registrado(s) con éxito** a la factura ###-######_.
    b. _**Pago exitoso con detalles** ... Se han registrado los pagos, pero con algunas observaciones. Revisa los detalles para más información_.

<a id="politica-3-validaciones-formas-pago"></a>

### POLÍTICA 3. Validaciones de Formas de Pago

Las columnas de la tabla principal son:

1. **Indicador del estatus de registro (#)**, indica si es válida (icono verde) o no (icono rojo) la información capturada para la fila de forma de pago.
2. **Forma de Pago (obligatoria)**, listado de las vías que tiene el cliente para pagar el saldo de la factura.
3. **Banco**, listado de los bancos registrados en el sistema.
4. **Monto**, cantidad que se va a dar como pago para la Forma de Pago indicada.
5. **Número de Referencia**, campo de texto libre que se usa para dar mayor información de la forma de pago, ejemplo de uso es capturar los últimos 4 dígitos de una tarjeta.
6. **Nota de Abono (_sólo lectura_)**, para algunas formas de pago, es un documento que indica el saldo que la empresa _le debe a un cliente_, y ese documento tiene un saldo que también funciona como pago.
7. **Cuenta Bancaria Interna**, listado de las cuentas bancarias registradas en el sistema destino del monto especificado con la forma de pago seleccionada.

Los registros de la tabla principal tienen a la _**Forma de Pago (segunda columna)**_ para indicar cuál es la información requerida de pago, lo cual pueden ser:

<a id="politica-3-1-info-faltante"></a>

1. **Columnas**, que en caso necesario aparecerá un mensaje de error en rojo solicitando acción del usuario de la pantalla.
2. **Lectura Código de Barras de Notas de Abono**, en la que se tendrá que escanear (o en su defecto teclear) un código de barras que hace referencia a una Nota de Abono y que aparecerá la información en la columna 6.
3. **Registro de Cheque Protegido**, en la que aparecerá un formulario para registrar los datos de un cheque protegido.

Para una forma de pago puede ser obligatorio especificar un Número de Referencia, pero para otras no, **es por eso que la forma de pago es el primer dato a seleccionar** de un registro de pago para activar las validaciones correspondientes.
  
Una validación que todas las Formas de Pagos comparten es referente al _Monto_:

1. Tiene que ser mayor a cero.
2. Es obligatorio capturar un monto.
3. La cantidad deberá ser menor o igual al máximo configurado de la forma de pago (a excepción de la forma de pago DIFERENCIA EN PESOS, sólo permitiendo montos entre 0.01 a 0.99).

> NOTA: Cuando el usuario capture una cantidad mayor a la permitida de una forma de pago, se mostrará un mensaje de advertencia en la que dirá que se pondrá en su lugar el límite máximo.

<a id="politica-4-formas-pago-con-parcializacion"></a>

### POLÍTICA 4. Formas de Pago con Parcialización

Algunas Formas de Pagos tiene configurado en el sistema un porcentaje de comisión, lo cual genera una serie de cálculos para mostrar al usuario 3 datos, divididos en 2 tarjetas en la parte inferior central de la pantalla:

1. Parcializado (AMARILLO):
    - _MONTO_: Cantidad base del cálculo.
    - _MENSUALIDADES_: Indica el número de pagos y el monto a pagar por c/uno.
2. Sin Parcializar (VERDE):
    _COMISIÓN_: La cantidad que se tendrá para generar facturas de comisión.

**Estas tarjetas NO aparecerán** si la forma de pago seleccionada no tiene la condición anteriormente dicha.

<a id="politica-5-valores-permitidos-en-modo-captura"></a>

### POLÍTICA 5. Reseteo de valores de registro de Forma de Pago

Para evitar el dejar información perteneciente a otras formas de pago puestas anteriormente en una misma fila, al igual que información adicional no provista o que fue cancelada su captura ([ver punto 2 y 3](#politica-3-1-info-faltante)), el módulo hará un reseteo a los valores iniciales en los siguientes casos:

1. Cuando seleccione una Forma de Pago cuando ya había seleccionado una.
2. Cuando cancele la captura de un cheque protegido.
3. Cuando cancele la lectura de código de barras de una N.A.
4. Cuando no es posible utilizar la Forma de Pago seleccionada para Facturas de Anticipo.

El único valor que no se reseteará es el _Monto_, salvo las restricciones que indique cada forma de pago.

<a id="politica-6-adicion-filas-pago"></a>

### POLÍTICA 6. Adición de filas de pagos

Esta pantalla permite pagar una factura con más de una forma de pago (salvo que las formas de pagos prohiban su uso según datos capturado). Para agregar una forma de pago, tecleé la `flecha abajo`, y dependiendo de estatus del registro anterior capturado:  
  
**a. Primera columna con icono en verde:** Agregará la nueva fila.
**b. Primera columna con icono en rojo:** No agregará la nueva fila, se deberá capturar los datos obligatorios faltantes que las formas de pago indiquen.
  
Para las facturas de anticipo, que se podrán reconocer en la zona superior izquierda del folio con borde azul, _**sólo se podrá pagar con una sola forma de pago**_, en caso de que el usuario intente agregar otra fila de forma de pago, se le mostrará un mensaje de error: _**Las Facturas de Anticipo sólo permiten una Forma de Pago.**_, no agregando la nueva fila.

<a id="politica-7-formas-pago-con-autorizacion"></a>

### POLÍTICA 7. Formas de Pago con Autorización previa

Algunas formas de pago establecen que se deberá tener autorización de la terminal antes de procesar los pagos, dichas formas de pago, en cuando se dé clic en el botón _Aceptar_, le preguntará antes que si la autorización ha sido concedida o no:
  
_**Seleccionó al menos una Forma de Pago que requiere Confirmación. ¿El pago fue APROBADO en la Terminal?**_
  
si no es el caso, la pantalla mostrará un mensaje que hasta que no sea autorizado no se podrán realizar los pagos:
  
_**Espere a que el pago sea aprobado en la Terminal. Una vez aprobado de clic nuevamente en el botón Aceptar para realizar los pagos.**_
  
<a id="politica-8-forma-pago-na"></a>

### POLÍTICA 8. Forma de Pago de Nota de Crédito

Cuando se seleccione como forma de pago **7 - NOTA DE CREDITO** o **8 - SALDO A FAVOR** en los registros de pago de la tabla principal, **NO se podrá escanear o teclear un código de barras de una Nota de Abono que NO sea del mismo cliente que la factura a pagar**.

## 🧪 Casos de Prueba

### 1. No carga la ruta de acceso con rol no permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que NO tenga el rol de `Vendedor` o de `Vendedor Mostrador`.
- [ ] 2. Al cargar el sistema, en el menú lateral principal, no habrá la opción `Pagos > Registrar Pagos Factura`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Pagos > Registrar Pagos Factura` en el menú al cargar sistema.

### 2. Carga la ruta de acceso con rol permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que SÍ tenga el rol de `Vendedor` o de `Vendedor Mostrador`.
- [ ] 2. Al cargar el sistema, en el menú lateral principal, mostrará la opción `Pagos > Registrar Pagos Factura`.

#### 🛡️ Validaciones

- [ ] `Pagos > Registrar Pagos Factura` en el menú al cargar sistema.

### 3. No carga de una factura entregada

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca una factura de fecha pasada en el buscador que se encuentra en la parte superior central  (usualmente ya están entregadas).
- [ ] 3. Al seleccionarla, la pantalla intentará cargar los detalles de información de la factura, y te mostrará un mensaje de error, indicando que la factura ya fue entregada con la fecha y hora en que ocurrió dicha entrega.

#### 🛡️ Validaciones

- [ ] Mensaje de factura ya entregada con detalles de operación.

### 4. Carga de una factura de Anticipo

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de anticipo.
- [ ] 3. Al seleccionar la factura y si no está entregada, aparecerá un mensaje de 3 opciones: _**¿Qué operación desea realizar, Marcar Como Entregada (sin realizar Pagos), Recibir Pagos de la factura seleccionada o Cancelar Modificaciones?**_, selecciona `Pagar`.
- [ ] 4. Revisa que la barra de búsqueda de las facturas esté **bloqueada**, esto es **a propósito**, ya que **el usuario no podrá hacer una carga de otro folio si el proceso de captura no es cancelado**; si quieres cancelarlo, ver a [cómo cancelar una captura (paso 3)](#prueba-15-cancelar-captura).
- [ ] 5. Observarás en la parte superior izquierda una tarjeta con borde **azul** con la leyenda _FOLIO ANTICIPO_ y el Folio de la factura.

#### 🛡️ Validaciones

- [ ] Muestra del mensaje de opción múltiple al carga factura.
- [ ] Tarjeta con Folio de factura con borde azul y leyenda indicada.
- [ ] Carga de facturas bloqueada una vez cargaba la factura de anticipo.

### 5. Carga de una factura de no Anticipo

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de venta.
- [ ] 3. Si no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada. 

### 6. Entrega de factura Anticipo sin realizar pagos

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de anticipo.
- [ ] 3. Al seleccionar la factura y si no está entregada, aparecerá un mensaje de 3 opciones: _**¿Qué operación desea realizar, Marcar Como Entregada (sin realizar Pagos), Recibir Pagos de la factura seleccionada o Cancelar Modificaciones?**_, selecciona `Entregar`.
- [ ] 4. Se presentara un mensaje de éxito: _**Factura ###-###### entregada.**_
- [ ] 5. Buscar o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Muestra del mensaje de opción múltiple al carga factura.
- [ ] Tarjeta con Folio de factura con borde azul y leyenda indicada.
- [ ] Mensaje de éxito en entrega de la factura.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

### 7. Pago de factura no Anticipo con Efectivo

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura que no sea de anticipo.
- [ ] 3. Si no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago.
- [ ] 4. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **1 - EFECTIVO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 5. Se activarán el resto de las celdas (a excepción de Nota de Abono, es _sólo lectura_), el campo _**RECIBÍ**_ con el mensaje de error _**Debe ser mayor a cero**_ que se encuentra al lado de los botones de **Aceptar** / **Cancelar**; captura una cantidad menor al Monto del registro de pago EFECTIVO, te aparecerá el mensaje _**Debe ser mayor igual al total en efectivo.**_.
- [ ] 6. Ahora en el campo **RECIBÍ** borra todo el contenido, te aparecerá el mensaje _**Ingrese cantidad**_.
- [ ] 7. Teclea en el campo **RECIBÍ** una cantidad mayor o igual al monto del registro de pago, se calculará el campo **CAMBIO** y notarás que al salirte de la edición del campo deja el formato de moneda de 2 decimales (si es más que 999 verás el separador de millares, la coma).
- [ ] 8. El botón **Aceptar** sigue inhabilitado, debido a que el registro de EFECTIVO le falta la _**Cuenta Bancaria Interna**_, selecciona una y verás que el icono de estatus de la primera columna se torna **verde**, indicando que la fila del registro de pago ya es válida, y el botón **Aceptar** ya se muestra en color azul listo para hacer clic.
- [ ] 9. Da clic en el botón **Aceptar**, y aparecerá un mensaje de confirmación: _**¿Estás seguro de que deseas guardar los registros de pago?**_, da clic en **No** para confirmar la información de pago de factura, da otra vez clic en **Aceptar** y ahora elige **Sí**, y aparecerá el mensaje de éxito: _**Pago(s) registrado(s) con éxito a la factura ###-######.**_, da clic en aceptar y se limpiará toda la información de la pantalla relativa al pago y a la factura.
- [ ] 10. Buscar o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar EFECTIVO como forma de pago.
- [ ] Validaciones del campo RECIBI.
- [ ] Mensaje de éxito de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

### 8. Pago de factura Anticipo con Tarjeta de Débito

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de anticipo.
- [ ] 3. Al seleccionar la factura y si no está entregada, aparecerá un mensaje de 3 opciones: _**¿Qué operación desea realizar, Marcar Como Entregada (sin realizar Pagos), Recibir Pagos de la factura seleccionada o Cancelar Modificaciones?**_, selecciona `Pagar`.
- [ ] 4. Observarás en la parte superior izquierda una tarjeta con borde **azul** con la leyenda _FOLIO ANTICIPO_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **17 - TARJETA DE DEBITO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 5. Llena los datos requeridos, verás que el icono de la primera columna se torna **verde** indicando que los datos del pago son válidos.
- [ ] 6. 9. Da clic en el botón **Aceptar**, y aparecerá un mensaje de confirmación: _**¿Estás seguro de que deseas guardar los registros de pago?**_, da clic en **No** para confirmar la información de pago de factura, da otra vez clic en **Aceptar** y ahora elige **Sí**, y aparecerá el mensaje de éxito: _**Pago(s) registrado(s) con éxito a la factura ###-######.**_, da clic en aceptar y se limpiará toda la información de la pantalla relativa al pago y a la factura. Nótese que esta vez no se habilitó el campo de _RECIBÍ_ ya que la forma de pago que seleccionaste NO fue efectivo.
- [ ] 7. Da clic en _Aceptar_ y la pantalla se limpiará de toda la información de factura y sus registros de pagos lista para cargar otra factura a pagar.
- [ ] 8. Buscar o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde azul y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar TARJETA DE DEBITO como forma de pago.
- [ ] Deshabilitación del campo RECIBI.
- [ ] Mensaje de éxito de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

### 9. Pago de factura no Anticipo con TC 3 Meses 5.5%

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura que no sea de anticipo.
- [ ] 3. Si no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago.
- [ ] 4. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **18 - TC 3 MESES 5.5%** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 5. Ahora en la zona de totales (parte inferior de la pantalla) se podrán ver las tarjetas de **PARCIALIZADO** y **SIN PARCIALIZAR**, indicando el monto base, las mensualidades y la comisión que se va a generar con el monto del registro de pago. Completa los datos requeridos de la forma de pago.
- [ ] 6. Da clic en el botón **Aceptar** y aparecerá un mensaje de confirmación adicional: _**Seleccionó al menos una Forma de Pago que requiere Confirmación. ¿El pago fue APROBADO en la Terminal?**_, selecciona _NO_, y te mostrará un mensaje de advertencia: _**Espere a que el pago sea aprobado en la Terminal. Una vez aprobado de clic nuevamente en el botón Aceptar para realizar los pagos.**_.
- [ ] 7. Da clic en el botón **Aceptar** y aparecerá un mensaje de confirmación adicional: _**Seleccionó al menos una Forma de Pago que requiere Confirmación. ¿El pago fue APROBADO en la Terminal?**_, selecciona _SÍ_ y espera un momento para que los pagos sean confirmados, ya que esta forma de pago genera también una _**Factura de Comisión**_ por lo cuál el sistema requiere la fabricación y el timbrado de este nuevo documento fiscal, al finalizar pueden pasar los siguientes resultados:
    a. **Pago exitoso con detalles. Se han registrado los pagos, pero con algunas observaciones. Revisa los detalles para más información.**: El sistema ha registrado los pagos exitosamente, más el proceso de facturación ha reportado advertencias y/o errores, el diálogo permite desplegar tanto advertencias como errores, y cada registro se puede distinguir con UUID (formato ########-####-####-####-############) y/o el número de registro de pago del cual se refiere el contenido de la observación, estos datos son útiles para la localización del detalle del proceso de facturación.
    b. **Pago(s) registrado(s) con éxito a la factura ###-######.**: Tanto el pago de la factura así como la facturación de comsión ha resultado correctos.
- [ ] 8. Da clic en Aceptar en el mensaje de resultado de realización de pagos y busca o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar TC 3 MESES 5.5% como forma de pago.
- [ ] Deshabilitación del campo RECIBI.
- [ ] Mensaje de éxito o detalles de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

### 10. Pago de factura no Anticipo con Efectivo + Tarjeta de Débito

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de no anticipo.
- [ ] 3. Sí no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **1 - EFECTIVO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 4. Llena los datos requeridos, y cambia el `Monto` por una cantidad menor que el monto inicial.
- [ ] 5. Ve que el icono de la primera columna se ha cambiado a **verde**, lo cuál indica que _el registro es válido_ y la tabla está habilitada para agregar otra forma de pago, ahora teclea _**flecha abajo**_ para agregar otra fila, observarás que en esta nueva fila el monto inicial es el restante de la cantidad que apareció primero al cargar la factura menos la cantidad de la forma de pago EFECTIVO que acabaste de llenar.
- [ ] 6. Elige **17 - TARJETA DE DEBITO** como forma de pago en la segunda fila y completa los datos obligatorios hasta que sea válida la fila.
- [ ] 7. Notarás que el botón _Aceptar_ esta inhabilitado debido a que el campo RECIBÍ está habilitado y con cantidad de cero (lo cuál está prohibido), ya que hay al menos una forma de pago EFECTIVO, ingresa una cantidad mayor igual al total del monto de EFECTIVO, no es necesario que cubra ambas formas de pago.
- [ ] 8. Ahora sí el botón _Aceptar_ está activado, da clic en él y confirma tu captura de pagos.
- [ ] 9. Busca o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar las 2 formas de pago.
- [ ] Habilitación del campo RECIBI.
- [ ] Mensaje de éxito de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

> NOTA: Al agregar una nueva fila, si tú tecleas _flecha abajo_ estando en una celda de lista (forma de pago, banco, cuentas bancarias internas), a parte de agregarte la nueva fila, se desplegará otra vez la lista de la celda, lo cual es un comportamiento esperado, para evitar esto, simplemente nos movemos a otra celda que no sea de lista.

### 11. Pago de factura no Anticipo con Nota de Crédito

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de no anticipo.
- [ ] 3. Sí no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **7 - NOTA DE CREDITO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 4. Aparecerá una nueva ventana con el título _**Teclee o escanee una Nota de Abono del cliente <No. de Cliente>**_, en la cual se te pedirá escanear o teclear el código de barras de una nota abono que, como el título lo dice, sea del mismo cliente de la factura a pagar [ver Pólitica 8](#politica-8-forma-pago-na), intenta escanear una Nota de Abono de otro cliente, da clic en _Aceptar_, y te mostrará mensajes de advertencia y/o error: **No se obtuvieron los valores de la Nota de Abono para la Forma de Pago seleccionada. Favor de buscar la Nota de Abono y reingresar la forma de pago.**, al darle al botón _Aceptar / Entendido_, notarás que se ha borrado la información del registro para que vuelvas a ingresarla (a excepción del monto), ya que si surgen detalles al seleccionar una forma de pago no se podrá continuar con la operación.
- [ ] 5. Vuelve a realizar el paso 4 más con la diferencia de escanear o teclear un código de barras de una Nota de Abono que sí sea del cliente de la factura a pagar, das clic en _Aceptar_ y enseguida en tu registro de pago, en la sexta columna, aparecerá tanto el Folio como el saldo de la Nota de Abono.
- [ ] 6. Completa los datos del registro (si es que te lo pide el mismo) y has clic en _Aceptar_, confirma la captura y la ventana te mostrará el mensaje de éxito en tu pago de la factura.
- [ ] 7. Busca o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar NOTA DE CRÉDITO como forma de pago.
- [ ] Muestra de la ventana de Lectura de Nota de Abono.
- [ ] Máscara de campo de código de barra de Nota de Abono (como si fuera una contraseña).
- [ ] Mensajes mensajes de advertencia / error al leer una nota de abono de cliente diferente a la factura cargada.
- [ ] Reseteo de registro de pago al cancelar lectura de Nota de Abono.
- [ ] Folio y Saldo de Nota de Abono en registro de pago una vez aceptada la lectura de Nota de Abono.
- [ ] Deshabilitación del campo RECIBI.
- [ ] Mensaje de éxito de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

### 12. Pago de factura no Anticipo con Checkplus

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de no anticipo.
- [ ] 3. Sí no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **9 - CHEQUE POSTFECHADO CHECKPLU** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 4. El módulo te mostrará un mensaje de observaciones: _**Antes de seleccionar una cuenta para esta forma de pago, deberás de introducir Número de Referencia y Monto mayor a cero.**_, esto te dice directamente qué datos son obligatorios en esta forma de pago. Da clic en _Entendido_.
- [ ] 5. Completa la información requerida dejando al último la _Cuenta Bancaria Interna_, y cuando selecciones la cuenta bancaria interna, se mostrará una nueva ventana con un formulario para el _Registro de Cheques Protegidos_, en donde se activan todos los mensajes de error para que el usuario esté conciente de la información a llenar para continuar.
- [ ] 6. Haz clic en el botón _Cancelar_ o en la _cruz_ de la esquina superior derecha de la ventana del formulario, esto para cancelar la captura de un cheque protegido, enseguida se mostrará el mensaje: _**No se registró el cheque protegido para la Forma de Pago seleccionada. Favor de reingresar la información de la forma de pago.**_.
- [ ] 7. Vuelve a capturar la información de la captura del registro de pago con la forma de pago **9 - CHEQUE POSTFECHADO CHECKPLU** y ahora llena el formulario del cheque protegido y haz clic en _Guardar_, y te mostrará el mensaje: _**Información de Cheque Protegido registrada con éxito (Consecutivo del nuevo cheque protegido)**_.
- [ ] 8. Haz clic en _Aceptar_ en el mensaje de éxito de guardado del cheque protegido, y revisa si el registro de pago es válido, si no es así, completa el registro.
- [ ] 9. Si ya es válido el registro, ya te mostrará habilitado el botón de _Aceptar_, haz clic en él y confirma el pago.
- [ ] 10. Busca o escanea la misma factura, ahora se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar CHEQUE POSTFECHADO CHECKPLU como forma de pago.
- [ ] Muestra de la ventana de Anexo de Información de Cheques Protegidos.
- [ ] Muestra de mensajes de error al cargar Anexo de Información de Cheques Protegidos.
- [ ] Mensajes mensajes de error al cancelar captura de cheque protegido.
- [ ] Reseteo de registro de pago al cancelar captura de cheque protegido.
- [ ] Deshabilitación del campo RECIBI.
- [ ] Mensaje de éxito de realización de pagos.
- [ ] Mensaje de error de factura ya entregada con detalles de operación.

> NOTA: Los mismo pasos son aplicables a la forma de pago **CHEQUE AL DÍA CHECKPLUS**, más con la diferencia de que no aparecerá el mensaje de observaciones del paso 4, ambas formas de pagos son de _Checkplus_.

### 13. Intento de pagar como más de una Forma de Pago a factura de Anticipo

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de anticipo.
- [ ] 3. Al seleccionar la factura y si no está entregada, aparecerá un mensaje de 3 opciones: _**¿Qué operación desea realizar, Marcar Como Entregada (sin realizar Pagos), Recibir Pagos de la factura seleccionada o Cancelar Modificaciones?**_, selecciona `Pagar`.
- [ ] 4. Observarás en la parte superior izquierda una tarjeta con borde **azul** con la leyenda _FOLIO ANTICIPO_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **17 - TARJETA DE DEBITO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 5. Llena los datos requeridos, verás que el icono de la primera columna se torna **verde** indicando que los datos del pago son válidos.
- [ ] 6. Teclea _flecha abajo_ y te mostrará el mensaje de error: _**Las Facturas de Anticipo sólo permiten una Forma de Pago.**_.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de factura con borde azul y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar TARJETA DE DEBITO como forma de pago.
- [ ] Muestra de mensaje de error al intentar agregar fila de la tabla principal.

### 14. Pagar con cantidad mayor al saldo de factura

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de no anticipo.
- [ ] 3. Sí no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **1 - EFECTIVO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 4. Completa los datos del registro de pago.
- [ ] 5. Cambia el monto a una cantidad superior a la cargada inicialmente, ejemplo +$100.00 MXN.
- [ ] 6. Captura la cantidad correspondiente en el campo _RECIBÍ_.
- [ ] 7. Notarás que en la esquina inferior izquierda la cantidad del campo **ANTICIPO A GENERAR** ha cambiado a una cantidad diferente de cero, esto es una cantidad que _**en caso de hacerse un pago exitoso se puede usar con forma de pago en facturas futuras**_. Da clic en el botón _Aceptar_ y a parte del texto de confirmación de pagos, te aparecerá un texto de saldo a favor: _**El Total de Pagos registrado es mayor al Saldo de la Factura, se creará un saldo a Favor ... ¿Estás seguro de que deseas guardar los registros de pago?**_, da clic en _Sí_ y te mostrará el éxito de tu pago.
- [ ] 8. Ahora vamos a usar ese saldo a favor generado en el pago anterior. Carga una factura a pagar de no anticipo del _mismo cliente_ de la factura anterior.
- [ ] 9. Obtén el Folio de la cuenta por cobrar del cliente para ingresarla como forma de pago.
- [ ] 10. Sí no está entregada la factura a pagar, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **8 - SALDO A FAVOR** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 11. Al igual que la forma de pago _7 - NOTA DE CREDITO_ se te pedirá que escanees o teclees el código de barras del Folio del Anticipo, una vez ingresado, haz clic en _Aceptar_ y verás en la sexta columna tanto el folio como el saldo del anticipo generado en el pago anterior.
- [ ] 12. Si al hacer clic en _Aceptar_ para confirmar el pago aparece el mensaje de error _**Error de Captura: La Factura debe de ser pagada completamente.**_, es debido a que el saldo del anticipo no fue suficiente, completa con otra forma de pago con su registro válido para completar el importe a pagar.
- [ ] 13. Nuevamente da clic en _Aceptar_ para confirmar el pago y mira el mensaje de pago exitoso.
- [ ] 14. Busca o escanea las facturas pagadas, y se mostrará el mismo mensaje de error que en el Caso 3.

#### 🛡️ Validaciones

- [ ] Tarjeta con Folio de ambas facturas pagadas con borde rojo y leyenda indicada.
- [ ] Activación de la tabla principal.
- [ ] Validaciones del registro al seleccionar formas de pago.
- [ ] Muestra de la ventana de Lectura de Código de Barras de Anticipo al seleccionar 8 - SALDO A FAVOR.
- [ ] Habilitación del campo RECIBI (en caso de haber optado EFECTIVO como forma de pago).
- [ ] Mensajes de éxito de realización de pagos.
- [ ] Mensajes de error de factura ya entregada con detalles de operación.

<a id="prueba-15-cancelar-captura"></a>

### 15. Usar botón de limpiar / cancelar captura

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Teclea cualquier cosa en el buscador de facturas.
- [ ] 3. Da clic en botón color rojo **Cancelar Captura**, ubicado en la esquina superior derecha, se habrá limpiado el buscador de facturas.
- [ ] 4. Haz el proceso para pagar un factura **sin darle al botón de _Aceptar_**.
- [ ] 5. Da clic en botón color rojo **Cancelar Captura**, ubicado en la esquina superior derecha, se habrá limpiado toda la información capturada.

#### 🛡️ Validaciones

- [ ] Limpiado de datos como indican las instrucciones al usar botón.

### 16. Usar Manual de Registrar Pagos

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Haz clic en el botón **Manual Registrar Pagos** localizado en la parte superior derecha.
- [ ] 3. Te lanzará una ventana con instrucciones para el uso de esta ventana.
- [ ] 4. Haz clic en la _x_ en la esquina superior del manual para cerrarlo.

#### 🛡️ Validaciones

- [ ] Muestra de Manual de Registrar Pagos.
- [ ] Cierre del Manual.

### 17. Captura de monto superior al límite

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura que no sea de anticipo.
- [ ] 3. Si no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago.
- [ ] 4. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **1 - EFECTIVO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 5. Teclea en la celda de _Monto_ **9,999,999,999.00**, inmediatamente te salga el mensaje de advertencia: _**Monto capturado es mayor al Límite Máximo Permitido para la Forma de Pago seleccionada, se le asignará el valor del Límite Máximo ($999,999,999.00).**_.
- [ ] 6. Haz clic en _Aceptar_ del mensaje de advertencia y observa que sustituyo en automático por la cantidad dicha en el mensaje, que es el límite máximo de la forma de pago EFECTIVO.

#### 🛡️ Validaciones

- [ ] Muestra de mensaje de advertencia de límite máximo.
- [ ] Carga del límite máximo en celda _Monto_ al cerrar el mensaje de advertencia.

### 18. Eliminación de filas de pagos en captura

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` o `Vendedor Mostrador` y da clic en la opción del menú `Pagos > Registrar Pagos Factura`.
- [ ] 2. Busca o escanea una factura de no anticipo.
- [ ] 3. Sí no está entregada, observarás en la parte superior izquierda una tarjeta con borde **rojo** con la leyenda _FOLIO FACTURA_ y el Folio de la factura, y en la parte derecha datos de la factura necesarios para que el usuario haga el proceso de pago. Se habilitará la tabla principal debajo de la información de la factura cargada con el saldo inicial diferente de cero, con una fila precargada con el estatus de `no válida` (icono rojo con número 1), debido a que falta información acerca del pago, teniendo el mensaje de error en la celda de `Monto` _"Selecciona una forma de pago primero"_. Selecciona la forma de pago **1 - EFECTIVO** en el listado de la segunda columna _Forma de Pago / Descripción_.
- [ ] 4. Llena los datos requeridos, y cambia el `Monto` por una cantidad menor que el monto inicial.
- [ ] 5. Ve que el icono de la primera columna se ha cambiado a **verde**, lo cuál indica que _el registro es válido_ y la tabla está habilitada para agregar otra forma de pago, ahora teclea _**flecha abajo**_ para agregar otra fila, observarás que en esta nueva fila el monto inicial es el restante de la cantidad que apareció primero al cargar la factura menos la cantidad de la forma de pago EFECTIVO que acabaste de llenar.
- [ ] 6. Elige **17 - TARJETA DE DEBITO** como forma de pago en la segunda fila y completa los datos obligatorios hasta que sea válida la fila.
- [ ] 7. Aparecerán iconos de **bote de basura rojos** al final de las filas de pagos, haz clic en cualquier de esos iconos, y la fila del icono seleccionado desaparecerá, y los cálculos de los totales lo tomarán en cuenta.
- [ ] 8. Cancela la captura y vuelve a realizar la prueba pero esta vez borra la otra fila con el icono de eliminación.
- [ ] 9. Cancela la captura y vuelve a realizar la prueba hasta que queden las 2 formas de pago en la tabla principal (no es necesario que la segunda forma de pago está válida).
- [ ] 10. Sitúate en cualquier celda de captura del segundo registro, y teclea _**flecha arriba**_ para borrar la segunda fila, este es otro método de eliminación de registros en la tabla principal. Si tecleas _**flecha arriba**_ en el primer registro, no funciona la eliminación, ya que esta funcionalidad no está en el primer registro.
- [ ] 9. En los 3 casos, cuando sólo hay una fila, no aparece el icono de basura de la fila restante, ya que siempre debe de haber una fila en la tabla principal.

#### 🛡️ Validaciones

- [ ] Filas borradas de los iconos de basura (eliminación) seleccionados.
- [ ] Fila borrada del segundo registro con la tecla FLECHA ARRIBA.
- [ ] Fila única de tabla principal sin icono de eliminación.

## 📎 Observaciones adicionales

- Nuevo diálogo en DialogService: **QuestionDialogComponent**, muestra las opciones (hasta 3) a las que pueden dar clic y devolver el valor configurado para cada una de las opciones.
- Nuevo diálogo en Dialog Service: **ExecutionDetailsDialogComponent**, muestra mensaje, listados de advertencias y errores de procesos del almacenamiento del sistema.
- Correcciones y nuevas funcionalidades a los autocompletes usados en la tabla principal, ejemplo son:
    1. Método de limpieza en Autocomplete de Selección de Facturas.
    2. Muestra de listado de banco inicial en cuanto entra al Autocomplete de Bancos (antes no mostraba nada a menos que el usuario empezar a escribir).
    3. Readonly en Autocomplete de Selección de Facturas.
- Ejemplo del uso de la librería **ngx-mask** para inputs de captura de cantidades monetarias (Registrar Pagos y Cheques Protegidos).
- Agregado de nueva detección de mensaje en Dialog Service `error` para excepciones que tengan que ver con permisos.

> 🗓️ **Fecha de última modificación:** 2026-04-22
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---

## Comunicaciones

|Dir |Fecha       |Firma|Comentario                    |
|----|------------|-----|------------------------------|
| ⏩ | 2025/04/22 | ST  | Primera versión              |
