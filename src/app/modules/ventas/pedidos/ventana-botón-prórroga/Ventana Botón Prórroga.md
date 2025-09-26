# 📦 Módulo: Ventana Botón Prórroga
#### 📁 **Código:** `src/app/modules/ventas/pedidos/components/prorroga`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ventas/pedidos)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ventas/pedidos)

## 📝 Descripción

Cuando en la ruta de `/app/ventas/pedidos` se carga los pedidos al usuario el cual tiene el rol asignado de `Gerente`, se visualizará el botón `Aumentar días para Desasignación del Pedido` (Prórroga del pedido). Cual el usuario selecciona dando clic a un pedido, se realiza una series de validaciones para verificar si el pedido y la configuración del usuario es correcta para usar la funcionalidad de prórroga:

    1. El usuario/gerente que no sea Regional ni de Zona no podrá asignar más de 3 veces los días días adicionales del pedido seleccionado. Si es gerente de los 2 tipos mencionados, no tendrá límites de asignación, el sistema le preguntará al usuario si desea continuar con el proceso de prórroga, en caso de su confirmación (ACEPTAR), entonces se procederá a las validaciones restantes, de lo contrario, el sistema cancelará el proceso.
    2. Si el pedido no tiene apartados o no tiene fecha de última asignación, entonces no es válido para establecer prórroga y no se mostrará la ventana de prórroga.
    3. Si el pedido no tiene vigencia, entonces no es válido para establecer días adicionales.
    4. No se puede modificar los días de asignación a los pedidos con estado de Facturado Total.

Sí el pedido seleccionado por el usuario no pertenece a ninguno de los casos anteriores, entonces es válido para carga la ventana y proseguir con la asignación de días adicionales. A continuación se verás el siguiente formulario de la ventana de prórroga:

1. **Campos no editables:**
    - Backorders.
    - Fecha de Última Asignación.
    - Número de días para Deasignación del pedido.
    - Fecha Actual (tomando en cuenta los días de asignaciones previas).
    - Conteo de asignaciones previas.
    - Consecutivo de la asignación.
2. **Campos editables:**
    - Días Adicionales.
    - Comentario (razón / motivo limitado a 101 caracteres).

La ventana del botón de prórroga tiene 2 botones en la parte inferior-derecha:

1. **Guardar:** Realiza la validación final de la cantidad de días adicionales (30 con máximo), y si cumple con esa validación, se creará el nuevo registro de asignación.
2. **Cancelar:** Cierra la ventana y cancela el proceso de prórroga.

## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| botón   | Mostrar módulo    | Carga la venta de prórroga en caso de que el usuario y el pedido cumplan las condiciones comentadas en [`Descripción`](#📝-descripción). | Gerente |

## 💼 Políticas Generales

1. En cuanto a la activación del botón de prórroga, se mostrará mensajes al usuario de porqué el pedido no es calificado para asignar más días de prórroga.
2. Hay un límite de 30 días que se le puedes asignar a un pedido, en caso de necesitar más el sistema sugerirá que se generen extensiones (creación de más registros de asignación en esta ventana).
3. El Gerente que entre y cargue la pantalla de pedidos deberá de tener configurado en su usuario `Gerente Zona` o `Gerente Regional` para crear registros de asignaciones de prórroga sin límites, de lo contrario el genrente sólo tendrá 3 oportunidades de crearlos.

Véase [Seguridad](#🔐-seguridad).

## 🧪 Casos de Prueba

### 1. Visualización del botón de prórroga

#### 💼 Operación

- [ ] 1. Entra al sistema con un usuario que tenga asignado cualquier rol menos `Gerente`.
- [ ] 2. Haz clic en `Ventas/Pedidos`. Si tiene permiso de cargar la ventana de pedidos, verifica en la parte superior derecha, que es la zona de acciones de pedidos, que no veas el botón nombrado 'Aumentar días para Desasignación del Pedido' (dejando el cursor un momento encima de los botones para ver el nombre de la acción).
- [ ] 3. Salir del sistema dando clic en el botón `Salir` de la parte inferior-izquerda.
- [ ] 4. Entra al sistema con un usuario que tenga asignado el rol `Gerente`.
- [ ] 5. Haz clic en `Ventas/Pedidos`. Si tiene permiso de cargar la ventana de pedidos, verifica en la parte superior derecha, que es la zona de acciones de pedidos, que veas el botón nombrado 'Aumentar días para Desasignación del Pedido'.

#### 🛡️ Validaciones

- [ ] Boton de Prórroga visible si se entra con usuario con rol `Gerente`.
- [ ] Boton de Prórroga no visible si se entra con usuario con cualquier rol que no sea `Gerente`.

### 2. Carga bloqueda de la ventana del botón de prórroga

#### 💼 Operación

- [ ] 1. Entrar con un usuario con el rol de `Gerente` sin tener configurados `Gerente Zona` y `Gerente Regional`
- [ ] 2. Navega a `Ventas / Pedidos` y filtra los siguientes pedidos para que el sistema valide y te dé el mensaje correspondiente de bloqueo de carga de la ventana de prórroga:

    1. Pedido que si es usuario no es `Gerente Zona` o `Gerente Regional` no se cargará la ventana debido al número de asignaciones previas es mayor a 3:
        - FOLIO: A - 445602.
        - MENSAJE: "Has sobrepasado la cantidad de veces que se puede aumentar los días de desasignación por parte de un Gerente.'Es necesario que a partir de ahora autorice este Pedido algún Gerente Regional.".
    2. Pedidos sin Apartados o Fecha de Última Asignación:
        - FOLIOS: TLF - 512322, ZF - 345207.
        - MENSAJE: Aparece un mensaje de confirmación, si aceptas el pedido será evaluado a la siguientes validaciones, de lo contrario se cancelará el proceso.
    3. Pedidos sin vigencia:
        - FOLIOS: D07 - 379978, D07 - 379999.
        - MENSAJE: "No se puede modificar en Pedidos cancelados.".
    4. Pedidos con estado de Facturado Total:
        - FOLIO: P16 - 56780.
        - MENSAJE: "No se puede modificar en Pedidos Facturados Totales.".

#### 🛡️ Validaciones

- [ ] Sistema lanza los mensajes especificados con los pedidos mencionados.

### 3. Carga de la ventana botón de prórroga

#### 💼 Operación

- [ ] 1. Busca los pedidos siguientes que pasan las validaciones, se cargará la ventana con los siguientes datos del pedido seleccionado (FOLIOS: A - 445602, A - 445585):

    1. **Campos no editables:**
        - Backorders.
        - Fecha de Última Asignación.
        - Número de días para Deasignación del pedido.
        - Fecha Actual (tomando en cuenta los días de asignaciones previas).
        - Conteo de asignaciones previas.
        - Consecutivo de la asignación (el nuevo que se le asignsará en caso de confirmar el registro).
    2. **Campos editables:**
        - Días Adicionales.
        - Comentario (razón / motivo limitado a 101 caracteres).

#### 🛡️ Validaciones

- [ ] Carga sin errores ni mensajes de validación de la ventana del prórroga.

### 4. Validaciones del formulario de prórroga

#### 💼 Operación

- [ ] 1. Busca los pedidos siguientes que pasan las validaciones, se cargará la ventana con los siguientes datos del pedido seleccionado (FOLIOS: A - 445602, A - 445585).
- [ ] 2. Realiza la siguientes acciones de interacción con el formulario:

    1. Da clic en el campo de `Días adicionales`, deja vacío el campo y después da clic en cualquier área de la ventana afuera del campo, aparecerá el mensaje _"Indique días adicionales."_.
    2. Cuando ingreses cero (0) en el campo de `Días adicionales`, te mostrará debajo del campo el mensaje _"Debe ser mayor a 0."_.
    3. Da clic en el campo de `Comentario`, dejando vacío el campo y después da clic en cualquier área de la ventana afuera del campo, aparecerá el mensaje debajo del campo _"Escriba la razón de asignación."_.
    4. Ingresa un valor negativo en el campo de `Días adicionales`, te aparecerá el mensaje _"Debe ser mayor a 0."_.
    5. Ingresa un valor cualquiera que no sea un número entero en el campo de `Días adicionales`, te aparecerá el mensaje _"Solo se permiten números enteros"_.
    6. Ingresa un mensaje en el campo `Comentario`, verás que el contandor de caracteres se actualiza, y se detendrá de ingresar caracteres cuando el máximo sea alcanzado (101), incluso si es un mensaje es copiado en el portapapeles (_"Sólo se permiten 101 caracteres."_).

- [ ] 3. En todo momento las interacciones listadas anteriormente _no debe de habilitar el botón Guardar_.

#### 🛡️ Validaciones

- [ ] Todos los casos de interacción con el formulario se muestran como en la descripción de los puntos.

### Crear registro de Asignación

#### 💼 Operación

- [ ] 1. Entra al sistema con un usuario que tenga el rol `Gerente` y tenga configurado como `Gerente Regional` o `Gerente Zona`.
- [ ] 2. Navega a `Ventas / Pedidos` y busca / filtra por algún pedido para abrir la ventana de prórroga (como el FOLIO `A - 445602`) dando clic a botón `Aumentar días para Desasignación del Pedido`.
- [ ] 3. Llena los datos de `Días adicionales` con un valor mayor a 30 y un `Comentario`.
- [ ] 4. Se deberá de habilitar el botón `Guardar`, da clic en él y te dará el mensaje de máximo de días adicionales: _"El máximo de tiempo adicional por ocasión es de 30 Días. En caso de ser necesario, aplique varias extensiones."_.
- [ ] 5. Corrije el dato de `Días adicionales` (por ejemplo, 10), y cuando des clic en el botón `Guardar` se mostrará un mensaje de confirmación, si seleccionas `No` se cancelar el proceso y todavía te mostrará la ventana, de lo contrario si seleccionas `Sí` creará la asignación de prórroga y te enviará un mensaje de éxito y se cerrará la ventana de prórroga.
- [ ] 6. Vuelve a abrir la ventana de prórroga y va a aparecer en los camops no editables los `Días Actuales` incrementando por la cantidad de `Días Adicionales` que ingresaste en la captura anterior, la `Fecha Actual` aumentados por los días mencionados y `Aumentado (veces)` incrementado en +1 respecto a la primera carga de la ventana de prórroga.

#### 🛡️ Validaciones

- [ ] Mensaje de validación de máximo de días adicionales.
- [ ] Creación de registro de asignación de prórroga exitosamente.
- [ ] Validación de registro de prórroga volviendo a abrir la ventana de prórroga.

## 📎 Observaciones adicionales

1. Se cambio el evento de `RowClicked` por `SelectionChanged` del grid de pedidos para asignar la referencia del pedido seleccionado.
2. Los eventos `FirstDataRendered` y `FilterChanged` del grid de pedidos ahora se encarga de borrar toda selección que se haya hecho de filas completas, y si intentas abrir la ventana de prórroga sin hacer clic a una fila para seleccionar un pedido se te mostrará un mensaje de advertencia: _"Favor de seleccionar un pedido ..."_.

> 🗓️ **Fecha de última modificación:** 2025-09-26
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
