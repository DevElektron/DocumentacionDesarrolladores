# 📑 Módulo: Pedidos
#### 📁 **Código:** `Modules/Ventas/Pedidos`
#### 💻 **Menú:** Ventas > Pedidos

## 📝 Descripción
Este módulo permite la consulta y gestión de pedidos. Al cargar el componente, si el usuario tiene el rol de gerente, se mostrará un componente adicional para visualizar partidas canceladas y facturas pendientes ([Ver documentación de partidas canceladas]()) ([Ver documentación de facturas pendientes()]).

El módulo presenta una tabla de pedidos, una tabla de detalles y una tabla de tramos. Al seleccionar un pedido, se muestran sus detalles; al seleccionar un detalle con tramos, estos se visualizan en la tabla correspondiente.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|--------------|
| Componente | Partidas Canceladas y Facturas Pendientes | Visualiza información relevante al cargar el módulo | Gerente |
| Barra superior | Botones y filtros operacionales | Permite operar sobre los pedidos y filtrar información | Todos los roles con acceso a pedidos |

## 💼 Políticas Generales
- El módulo está preparado para mostrar información adicional según el rol del usuario.
- La barra superior contiene botones y filtros para operar sobre los pedidos (detallar en futuras versiones).
- El botón "Imprimir pedido individual" permanece deshabilitado si no hay un pedido seleccionado.

## 🧪 Casos de Prueba

### Barra superior de operaciones
#### 💼 Operación
- [ ] El botón "Imprimir pedido individual" solo se habilita cuando hay un pedido seleccionado.

#### 🛡️ Validaciones
- [ ] Al presionar el botón, el sistema solicita confirmación para imprimir el documento.
- [ ] Si el usuario confirma, el sistema consulta si alguno de los detalles del pedido tiene tramos en la base de datos.
    - Si existen detalles con tramos, se muestra un mensaje de confirmación adicional preguntando si desea imprimir el pedido con todo y tramos.
        - Si el usuario confirma, el documento generado incluye la lista de detalles del pedido, información del cliente y del pedido, y los tramos de los detalles en una lista horizontal bajo el registro del detalle correspondiente.
        - Si el usuario cancela, no se imprime nada.
    - Si no existen tramos, el documento solo muestra los detalles del pedido, datos del cliente, datos del almacén e información del pedido.
- [ ] Si no hay pedido seleccionado, el botón permanece deshabilitado y no realiza ninguna acción.

## 📎 Observaciones adicionales
- 
- 

> 🗓️ **Fecha de última modificación:** 2025-08-04
> 👤 **[Erick López]**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩|2025/08/04 | EL |Listo para revisión(solo servicio de impresipón)|
