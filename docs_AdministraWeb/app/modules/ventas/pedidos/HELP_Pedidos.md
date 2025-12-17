# 📑 Módulo: Pedidos
#### 📁 **Código:** `Modules/Ventas/Pedidos`
#### 💻 **Menú:** Ventas > Pedidos

## 📝 Descripción

Este módulo permite la consulta y gestión de pedidos. Al cargar el componente, si el usuario tiene el rol de gerente, se mostrará un componente adicional para visualizar partidas canceladas y facturas pendientes ([Ver documentación de partidas canceladas](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/src/app/modules/ventas/pedidos/partidas-canceladas/HELP_Partidas_Canceladas.md)).

El módulo presenta una tabla de pedidos, una tabla de detalles y una tabla de tramos. Al seleccionar un pedido, se muestran sus detalles; al seleccionar un detalle con tramos, estos se visualizan en la tabla correspondiente.

## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|--------------|
| Componente | Partidas Canceladas y Facturas Pendientes | Visualiza información relevante al cargar el módulo | Gerente |
| Barra superior | Botones y filtros operacionales | Permite operar sobre los pedidos y filtrar información | Todos los roles con acceso a pedidos |

## 💼 Políticas Generales

### POLÍTICA 1. ELEMENTOS LIGADOS A ROLES

- El módulo está preparado para mostrar información adicional según el rol del usuario.

<a id="politica-2-barra-superior"></a>

### POLÍTICA 2. BARRA SUPERIOR

1. La barra superior contiene botones y filtros para operar sobre los pedidos (detallar en futuras versiones).
2. El botón "Imprimir pedido individual" permanece deshabilitado si no hay un pedido seleccionado.
3. Sólo los usuarios `Vendedor` y `Gerente` tendrán acceso al botón de acción `Cambiar Cliente`, que como su nombre lo dice, la acción de este botón será cambiar de cliente el pedido seleccionado (ver [`Caso de prueba 3`](#3-cambio-de-cliente-de-un-pedido)).

> NOTA: El ERP Legacy (ELSCA) no tiene ningún tipo de seguridad dentro del módulo de Pedido para el cambio de cliente. Como mejora, se le establecieron 2 roles a la acción (indicados en la [`Política 2, inciso 3`](#politica-2-barra-superior)).

## 🧪 Casos de Prueba

### 1. Barra superior de operaciones

#### 💼 Operación

- [ ] El botón "Imprimir pedido individual" solo se habilita cuando hay un pedido seleccionado.

#### 🛡️ Validaciones

- [ ] 1. Al presionar el botón, el sistema solicita confirmación para imprimir el documento.
- [ ] 2. Si el usuario confirma, el sistema consulta si alguno de los detalles del pedido tiene tramos en la base de datos.

        - Si existen detalles con tramos, se muestra un mensaje de confirmación adicional preguntando si desea imprimir el pedido con todo y tramos.

            - Si el usuario confirma, el documento generado incluye la lista de detalles del pedido, información del cliente y del pedido, y los tramos de los detalles en una lista horizontal bajo el registro del detalle correspondiente.
            - Si el usuario cancela, no se imprime nada.
        
        - Si no existen tramos, el documento solo muestra los detalles del pedido, datos del cliente, datos del almacén e información del pedido.

- [ ] 3. Si no hay pedido seleccionado, el botón permanece deshabilitado y no realiza ninguna acción.

### 2. Denegación del Cambio de Cliente de un Pedido

#### 💼 Operación

- [ ] 1. Accede al módulo de Pedidos en `/app/ventas/pedidos` con un usuario que NO tenga el rol de `Vendedor` o `Gerente`.
- [ ] 2. Verás botones de acción en la esquina superior derecha, pero ninguno llamado `Cambiar Cliente`.

#### 🛡️ Validaciones

- [ ] Acceso y vista denegadas del botón `Cambio Cliente`.

### 3. Cambio de Cliente de un Pedido

#### 💼 Operación

- [ ] 1. Accede al módulo de Pedidos con un usuario que tenga el rol de `Vendedor` o `Gerente`.
- [ ] 2. Enseguida verás un botón de acción en la esquina superior derecha, llamado `Cambiar Cliente`.
- [ ] 3. Da clic en el botón `Cambiar Cliente`, y verá en la parte inferior del botón una zona flotante que contiene los siguiente elementos:
    1. Buscador de clientes.
    2. Botón `Cambiar cambio`.
    3. Botón `Cancelar`.
- [ ] 4. Mientras se esté mostrando la zona flotante de cambio de cliente, da clic de nuevo al botón `Cambiar Cliente`, y se ocultaráá la zona flotante, cancelando el proceso.
- [ ] 5. Otra forma de cancelar el proceso es dar clic en el botón `Cancelar` de la zona flotante, inténtalo.
- [ ] 6. Accede de nuevo a la zona flotante del cambio de cliente dando clic en `Cambiar Cliente` y da clic en el campo de texto del buscador de clientes, y da clic en `Confirmar cambio`, y verás un mensaje que indica: _**No se ha seleccionado un cliente para el cambio. No se ha seleccionado un pedido para cambiar el cliente**_.
- [ ] 7. Accede de nuevo a la zona flotante de cambio de cliente y ahora busca un cliente, verás el listado de las coincidencia de los cliente que tiene relación con tu búsqueda, selecciona uno dando dando clic en un cliente listado, luego y da clic en `Confirmar cambio`, y verás un mensaje que indica: _**No se ha seleccionado un pedido para cambiar el cliente**_.
- [ ] 8. Accede de nuevo a la zona flotante de cambio de cliente y ahora busca _al mismo cliente del pedido seleccionado_, verás el listado de las coincidencia de los cliente que tiene relación con tu búsqueda, selecciona uno dando dando clic en un cliente listado, luego y da clic en `Confirmar cambio`, y verás un mensaje que indica: _**El cliente seleccionado es el mismo que el del pedido.**_
- [ ] 9. Selecciona o busca un pedido (ejemplo, Folio `ZF-346961`), y accede de nuevo a la zona flotante de cambio de cliente y ahora busca un cliente (ejemplo, busca `ANA`), verás el listado de las coincidencia de los clientes que tiene relación con tu búsqueda, selecciona uno dando dando clic en un cliente listado, luego y da clic en `Confirmar cambio`, se mostrará un mensaje de confirmación y dependindo de la respuesta a él se continuará el proceso, y verás un mensaje que indica: _**Error al cambiar el cliente del pedido ZF - 346961 (intento de cambio por cliente 170): El cliente del pedido ZF-346961 (NCTE 24000, política 90900) no tiene la misma política del cliente seleccionado (NCTE 170, política 50000), favor de verificar**_. No se permite cambiar el cliente de un pedido si el nuevo cliente seleccionado de la barra de búsqueda no tiene aplicada la misma política que el cliente actual.
- [ ] 10. Selecciona o busca un pedido (ejemplo, Folio `ZF-346961`), y accede de nuevo a la zona flotante de cambio de cliente y ahora busca un cliente (ejemplo, busca `24001`), se mostrará un mensaje de confirmación y dependindo de la respuesta a él se continuará el proceso, verás un cliente que tiene relación con tu búsqueda, selecciona uno dando dando clic en un cliente listado, luego y da clic en `Confirmar cambio`, y verás un mensaje que indica: _**Cliente del pedido ZF-346961 cambiado correctamente**_. Como ambos clientes tiene aplicada la política `90900` sí se permite el cambio. Se ocultará la zona de cambio de cliente una vez finalizo el cambio.

#### 🛡️ Validaciones

- [ ] Acceso y vista del botón `Cambio Cliente`.
- [ ] Las 2 formas de cancelar el proceso de cambio de cliente sin cambios al registro de pedido.
- [ ] 4 validaciones antes de un cambio de clñiente exitoso acorde a los pasos 6, 7, 8 y 9.
- [ ] Caso de cambio de cliente exitoso.

## 📎 Observaciones adicionales

- Se ha propuesto la `zona flotante` como mecánica de actualizaciones de un sólo campo de un registro seleccionado en un grid.

> 🗓️ **Fecha de última modificación:** 2025-08-04
> 👤 **[Erick López], [Samuel Valles], [Sergio Tostado]**
> 🏷️ **Versión:** 2

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩|2025/08/04 | EL |Listo para revisión(solo servicio de impresión)|
|⏩|2025/12/15 | ST |Botón de Cambio de Cliente.|
