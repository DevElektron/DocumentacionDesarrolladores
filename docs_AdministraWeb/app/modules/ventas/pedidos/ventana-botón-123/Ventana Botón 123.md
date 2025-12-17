# 📦 Módulo: Ventana Botón 123
#### 📁 **Código:** `src/app/modules/ventas/pedidos/components/pedido-detalle-tiempos-traspasos`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ventas/pedidos)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ventas/pedidos)

## 📝 Descripción

Cuando en la ruta de `/app/ventas/pedidos` se carga los detalles de las partidas de un pedido con backorders (que no se tiene el stock del artículo disponible en almacenes), se habilita el botón 123 que está en la parte superior-derecha para abrir un ventana que muestra los detalles de los tiempos de los traspasos de la partida (registros DALMTAT). La ventana muestra los siguientes bloques:

1. **Registros DALMTAT:** Tabla que muestra los registros de los detalles de pedido por `FOLIO - PARTIDA` que muestra las fechas (tiempos) de los traspasos.
2. **Total de Cantidad de registros DALMTAT:** Suma de la columna `Cantidad` de la tabla anterior.
3. **Registros de Backorders de artículo:** Tabla que muestra los datos de backorders asociados al `almacén de destino - artículo`, obtenidos a partir de la información del registro DALMTAT que no tiene dato en la columna `Número`, con un icono de semáforo del estado del backorder respecto a la cantidad objetivo (total de registros DALMTAT) y el acumulado:

    - 🟢 **Verde:** Acumulado suficiente o excedente: acumulado >= 0. La cantidad acumulada cubre completamente la necesidad; no hay riesgo de incumplimiento.
    - 🟡 **Amarillo:** Cobertura parcial: acumulado < 0 pero aún mayor que cantidad negativa del backorder. Hay déficit, pero la partida actual contribuye a reducirlo; se está cerca de cubrir la necesidad.
    - 🔴 **Rojo:** Déficit crítico: acumulado <= cantidad negativa del backorder. La cantidad acumulada más la partida actual no alcanza a cubrir la necesidad; riesgo alto de incumplimiento.

4. **Total de Cantidad de registros Backorders:** Suma de la columna `Cantidad` de la tabla anterior.
5. **Información de Manifiestos:** Muestra los manifiestos asociado al `Folio de Traspasos` del registro DALMTAT seleccionado.
6. **Fecha Mayor y Reprogramado:** Muestra la fecha en la que tiene la tentativa de entrega / recepción por parte del pedido, con una leyenda que indica si fue reprogramada manualmente o no.
7. **Datos del artículo:** Clave y Descripción del artículo asociado a `FOLIO - PARTIDA` de la ventana.

La ventana del botón 123 tiene 3 botones en la parte superior derecha:

1. **Actualizar:** Refresca la información de la ventana.
2. **Nuevo detalle:** Abre un formulario para establecer un nuevo registro DALMTAT con la Fecha de Recepción especificada.
3. **Modificar detalle:** Abre un formulario para modifica el registro DALMTAT seleccionado con la Fecha de Recepción especificada.

## 🔐 Seguridad

Por motivos de que es una funcionalidad secundaria de la ruta `/app/ventas/pedidos`, cualquier rol/usuario que tenga acceso a la ruta mencionada podrá ver y usar esta característica.

## 💼 Políticas Generales

1. En cuanto a la activación del botón 123, se mostrará un mensaje al hacer clic en él si el pedido _no está autorizado por cuestiones de crédito_, es decir:
    - Si **NO está autorizado** el pedido.
    - Si el pedido es de tipo **No-Stock**.
2. La carga de la ventana que se abre con el botón 123 será bloqueada si no hay registros relacionados con los detalles de los tiempos de los traspasos de la partida, buscándolos en la información guardada por el `FOLIO - PARTIDA` del detalle del pedido. 

Véase [Seguridad](#🔐-seguridad).

## 🧪 Casos de Prueba

### 1. Activación y Desactivación del botón 123

#### 💼 Operación

- [ ] 1. Haz clic en `Ventas/Pedidos`.
- [ ] 2. Busca un pedido con backorders con el control `SELECT` que aparece en la parte superior central de la ventana, seleccionando `Backorders`, y/o usando los filtros de la tabla de pedidos, por ejemplo: _Folio D09 - 431185_.
- [ ] 3. En los detalle cargados en la tabla de `Detalle de Partidas del Pedido` (debajo a la izquierda de la tabla de pedidos), selecciona una Partida que tenga backorders (columna Backorders > 0), en ese momento el botón 123 se debe de mostrar _habilitado_ para hacer clic.
- [ ] 4. Si seleccionas una partida sin backorders, el botón 123 estará _desactivado_.

#### 🛡️ Validaciones

- [ ] Boton 123 habilitado si se selecciona partida con backorders.
- [ ] Boton 123 deshabilitado si se selecciona partida sin backorders.

### 2. Carga bloqueda de la ventana botón 123

#### 💼 Operación

- [ ] 1. Cuando el botón 123 esté habilitado con partidas que cumplan con una de las 2 siguientes condiciones, **NO** se deberá de cargar la ventana del botón 123:

    - Si **NO está autorizado** el pedido y es de tipo **No-Stock**.
    - Si no se encuentran los registros de información de tiempo de entrega de traspasos.

Para esta prueba, comprueba buscando los siguientes pedidos y las partidas especificadas:

1. Partida sin registros de información de tiempo de entrega de traspasos:

    - FOLIO PEDIDO = A - 424581
    - PARTIDA = 1

2. Partida sin autorización:

    - FOLIO PEDIDO = D11 - 414064
    - PARTIDA = 1

#### 🛡️ Validaciones

- [ ] Partida sin registros al dar clic en botón 123 se muestra mensaje: 'No se logró localizar ningún registro: No se logró encontrar algun traspaso u orden de compra para surtir este backorder (Folio = A - 414064, Partida = 1).'.
- [ ] Partida sin autorización al dar clic en botón 123 se muestra mensaje: 'Imposible Mostrar Datos: Pedido NO Autorizado por Crédito'.

### 3. Carga de la ventana botón 123

#### 💼 Operación

- [ ] 1. Cuando el botón 123 esté habilitado con partidas que tengan backorders y sin ninguna de los 2 detalles mencionados en la prueba 2, se cargará la ventana del botón 123 con los elementos descritos en [Descripción](#📝-descripción). Algunos datos notables:

    1. No todas las partidas que cargan la ventana tendrán información en la tablas de backorders y manifiestos.
    2. La leyenda _SIN FECHA_ en el apartado de Fecha Mayor / Reprogramado es común para las partidas que no han tenido tratamiento en algún tiempo.

#### 🛡️ Validaciones

- [ ] Carga sin errores de la ventana del botón 123.

### 4. Muestra de información de manifiesto del registros DALMTAT seleccionado

#### 💼 Operación

- [ ] En una partida de pedido que tenga registros DALMTAT, selecciona uno y te mostrará en la tabla de abajo los detalles de la información de los manifiestos.

> NOTA: Son muy escasos los ejemplos de ventana botón 123 con manifiestos.

#### 🛡️ Validaciones

- [ ] Al seleccionar un registros de la tabla DALMTAT se visualiza la información de manifiestos.

### 5. Uso del botón `Actualizar`

#### 💼 Operación

- [ ] Hacer clic en el botón `Actualizar` (dejar el cursor un segundo encima del botón para ver el nombre del botón), para que carguen nuevamente todos los bloques de la ventana.

> NOTA: Como no vuelve a traer muchos datos no se notará algún cambio, recomendado seleccionar filas de las tablas para que observes el reseteo de la información.

#### 🛡️ Validaciones

- [ ] Actualización de la información cargada en  la ventana.

### 5. Uso del botón `Nuevo detalle`

#### 💼 Operación

- [ ] 1. Hacer clic en el botón `Nuevo detalle` (dejar el cursor un segundo encima del botón para ver el nombre del botón), se te mostrará un formulario con los siguientes campos:

    1. No editables:
        - Folio del Pedido.
        - Partida cargada.
        - Consecutivo (número de captura manual).
        - Clave del Artículo.
    2. Editable:
        - Fecha de Entrega.

- [ ] 2. Selecciona una fecha de entrega en el botón del calendario (derecha del campo) menor al día de hoy, se te mostrará una validación: 'Debe ser mayor o igual a hoy.'.
- [ ] 3. Selecciona una fecha de entrega mayor igual a la de hoy y da clic en `Guardar`, se te pedirá confirmar la acción, confírmala y se cerrará en automático el formulario, y tu nuevo registro estará en la tabla de registros DALMTAT.

#### 🛡️ Validaciones

- [ ] Validación 'Debe ser mayor o igual a hoy.' mostrada.
- [ ] Validación 'Debe seleccionar una Fecha de Entrega.' mostrada.
- [ ] Nuevo registro con fecha de entrega especificada en el formulario mostrada en tabla registros DALMTAT.

### 6. Uso del botón `Modificar detalle`

#### 💼 Operación

- [ ] 1. Hacer clic en el botón `Modificar detalle` (dejar el cursor un segundo encima del botón para ver el nombre del botón), se te mostrará un formulario con los siguientes campos:

    1. No editables:
        - Folio del Pedido.
        - Partida cargada.
        - Consecutivo (número de captura manual).
        - Clave del Artículo.
    2. Editable:
        - Fecha de Entrega (_precargada con la fecha de entrega del registro seleccionado_).

- [ ] 2. Selecciona una fecha de entrega en el botón del calendario (derecha del campo) menor al día de hoy, se te mostrará una validación: 'Debe ser mayor o igual a hoy.'.
- [ ] 3. Selecciona una fecha de entrega mayor igual a la de hoy y da clic en `Guardar`, se te pedirá confirmar la acción, confírmala y se cerrará en automático el formulario, y tu nuevo registro estará en la tabla de registros DALMTAT.

#### 🛡️ Validaciones

- [ ] Validación 'Debe ser mayor o igual a hoy.' mostrada.
- [ ] Validación 'Debe seleccionar una Fecha de Entrega.' mostrada.
- [ ] Nuevo registro con fecha de entrega especificada en el formulario mostrada en tabla registros DALMTAT.

### 7. Fecha Mayor y Reprogramación Manual

#### 💼 Operación

- [ ] Cualquier interacción confirmada con los botón `Nuevo detalle` y `Modificar detalle` para agregar/actualizar registros DALMTAT se hace el cálculo de la leyenda ubicada en la parte superior `FECHA DE ENTREGA APROXIMADA:`, compuesta por 2 partes:

    1. Fecha Mayor: Fecha aproximada de entrega de la partida del pedido.
    2. Reyprogramación: Ver si se ha hecho una reprogramación de esa fecha por captura manual.

> NOTA: Debes de prestar atención a los totales de cantidades en las tablas de registros DALMTAT y Backorders, ya que si el total de registros DALMTAT _ES MENOR AL TOTAL_ de Backorders, no se mostrará la fecha mayor y sólo se mostrará la leyenda _'SIN FECHA'_. Cuando es tiene una partida con un número de orden registrado, es altamente probable que se muestren cambios en este bloque.

#### 🛡️ Validaciones

- [ ] Con la especificación contenida en `NOTA`, cualquier cambio de la `FECHA DE ENTREGA APROXIMADA:`.

## 📎 Observaciones adicionales

1. Se agregó al Microservicio del Gateway el método `PATCH` para no tener que depender de las características del método `PUT` del HttpClientService.
2. Hay campos/columnas de tipo Fecha/Hora que que calculan en tiempo de ejecución, como la última columna de la tabla de Manifiestos `Transcurrido`, cuyo cálculo toma la información de la configuración del Sistema Operativo en el que se ejecuta el proyecto. El timezone correcto es `America/Mexico_City`, ya que si está otro (por ejemplo `UTC` que es muy común en SO Linux), verificalo con el equipo si observas un comportamiento de diferencia dentre los resultados del ERP ElektronSQL basado en Clarion y el proyecto.

> 🗓️ **Fecha de última modificación:** 2025-09-09
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
