
---
# 🚫 Módulo: Partidas Canceladas
#### 📁 **Código:** `Modules/Ventas/Pedidos/PartidasCanceladas`
#### 💻 **Menú:** Ventas > Pedidos > Partidas Canceladas

## 📝 Descripción
Este módulo muestra un resumen de las partidas canceladas o próximas a cancelarse en los pedidos. Al cargar el módulo de pedido, si el usuario tiene el rol de "gente", se desplegará un modal con la información de partidas canceladas, siempre y cuando existan partidas en ese estado. El objetivo es informar al usuario sobre el estatus de las partidas y los motivos de cancelación o desasignación.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                                 | Rol permitido |
|---------|-------------------|---------------------------------------------|--------------|
| Modal   | Partidas Canceladas | Visualiza partidas canceladas o próximas a cancelar | Gente        |
| Botón   | Continuar         | Oculta el componente, busca facturas pendientes y continúa con el flujo     | Gente        |

## 💼 Políticas Generales
- El modal solo se muestra si existen partidas canceladas o próximas a cancelarse.
- El acceso está restringido a usuarios con rol de "gerente" y número de vendedor con partidas canceladas eso deberá asignarse en el sistema Security.
- El usuario debe poder identificar fácilmente el estatus de cada partida mediante el color del icono.

## 🧪 Casos de Prueba

### Visualización de partidas canceladas
#### 💼 Operación
- [ ] Al cargar el módulo de pedido, si existen partidas canceladas o próximas a cancelarse y el usuario tiene el rol adecuado, se muestra el modal.

#### 🛡️ Validaciones
- [ ] La tabla debe mostrar las siguientes columnas:
    - Estado (icono de color)
    - Canc\Desasg. (Tiempo de cancelación)
    - Tipo (Tipo de pedido)
    - Serie
    - Folio
    - Det (Número de partida)
    - Cancelación (Fecha de cancelación)
    - Motivo Canc.\Deasg. (Motivo de cancelación o desasignación)
    - Cantidad (Cantidad de la partida)
    - Cant. Asg. (Cantidad asignada)
    - Clave del Artículo
    - Descripción (Descripción del pedido)
- [ ] El icono de estado debe ser:
    - Verde: Partidas que se cancelan mañana
    - Amarillo: Partidas que se cancelan hoy
    - Rojo: Partidas ya canceladas
- [ ] El pie del modal debe mostrar una leyenda visual explicando los colores y un botón para continuar.

## 📎 Observaciones adicionales
- El modal es solo informativo y no permite modificar datos.
- El botón "Continuar" Oculta el componente, busca facturas pendientes y permite seguir con el flujo normal del pedido.

> 🗓️ **Fecha de última modificación:** 2025-08-04
> 👤 **[Erick López]**
> 🏷️ **Versión:** 1

---

# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩|2025/08/04 | EL |Listo para revisión|
