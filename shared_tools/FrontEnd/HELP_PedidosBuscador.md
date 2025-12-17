# 📦 Componente Compartido: Buscador de Pedidos
#### 📁 **Código:** `shared/ui/components/buscadores/pedidos-buscador`
#### 💻 **Uso:** Componente reutilizable en módulos que requieran búsqueda de pedidos

## 📝 Descripción
Este componente compartido permite buscar y seleccionar pedidos mediante dos métodos complementarios: búsqueda directa escribiendo Serie y Folio en campos de texto, o búsqueda avanzada mediante un modal con tabla completa de pedidos. El componente es completamente reutilizable y se comunica con el componente padre mediante eventos y FormGroup, permitiendo su integración en cualquier módulo del sistema que requiera selección de pedidos.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Input   | Serie   | Permite capturar la serie del pedido (solo letras)     | Todos los roles con acceso al módulo padre       |
| Input   | Folio   | Permite capturar el folio del pedido (solo números)     | Todos los roles con acceso al módulo padre       |
| Botón   | Buscar (search)    | Abre el modal de búsqueda avanzada de pedidos     | Todos los roles con acceso al módulo padre       |
| Modal   | Tabla de Pedidos   | Muestra listado completo de pedidos con filtros avanzados     | Todos los roles con acceso al módulo padre       |
| Modal   | Tabla de Detalle   | Muestra las partidas del pedido seleccionado     | Todos los roles con acceso al módulo padre       |

## 💼 Políticas Generales
- El componente recibe un `FormGroup` como `@Input` que debe contener los controles: `serie`, `folio` y `pedidoCompleto`.
- La búsqueda manual se ejecuta automáticamente en el evento `blur` de los campos cuando ambos (serie y folio) tienen valor.
- El campo `serie` solo acepta letras y las convierte automáticamente a mayúsculas.
- El campo `folio` solo acepta números enteros positivos.
- El componente emite tres eventos `@Output`: `buscar`, `pedidoValido` y `pedidoInvalido` para notificar al componente padre.
- Si la búsqueda manual encuentra un pedido válido, se guarda el objeto completo en el control `pedidoCompleto` del FormGroup.
- Si la búsqueda manual no encuentra el pedido, se limpian los campos automáticamente y se emite el evento `pedidoInvalido`.
- El modal de búsqueda avanzada utiliza AG-Grid con paginación server-side para manejar grandes volúmenes de datos.
- Al seleccionar un pedido en el modal (simple click), se carga su detalle de partidas en la tabla inferior.
- Al hacer doble-click en un pedido del modal, se cierra automáticamente y se retorna el pedido al componente principal.
- El componente utiliza `PartidasCanceladasService` para la búsqueda manual y `PedidosService` para el listado del modal.
- Se muestran notificaciones toast al usuario indicando el resultado de las búsquedas (éxito, información, error).

## 🧪 Casos de Prueba

### Búsqueda Manual - Captura de Serie y Folio
#### 💼 Operación
- [ ] Capturar una serie válida en el campo "Serie".
- [ ] Capturar un folio válido en el campo "Folio".
- [ ] Salir del campo (blur) para ejecutar la búsqueda automática.
#### 🛡️ Validaciones
- [ ] El campo "Serie" debe aceptar solo letras (a-zA-Z).
- [ ] El campo "Serie" debe convertir automáticamente a mayúsculas cualquier letra ingresada.
- [ ] El campo "Folio" debe aceptar solo números enteros (0-9).
- [ ] Si se intenta ingresar caracteres no permitidos, deben ser eliminados automáticamente.
- [ ] Al salir del campo "Folio" con ambos campos llenos, debe ejecutarse la búsqueda automáticamente.
- [ ] Si alguno de los campos está vacío al hacer blur, no se debe ejecutar la búsqueda.

### Búsqueda Manual - Pedido Encontrado
#### 💼 Operación
- [ ] Capturar serie y folio de un pedido existente.
- [ ] Esperar el resultado de la búsqueda automática.
#### 🛡️ Validaciones
- [ ] Debe mostrarse un toast de éxito indicando que el pedido fue encontrado.
- [ ] El control `pedidoCompleto` del FormGroup debe contener el objeto completo del pedido.
- [ ] Debe emitirse el evento `pedidoValido` al componente padre.
- [ ] Los campos serie y folio deben mantener sus valores.

### Búsqueda Manual - Pedido No Encontrado
#### 💼 Operación
- [ ] Capturar serie y folio de un pedido inexistente.
- [ ] Esperar el resultado de la búsqueda automática.
#### 🛡️ Validaciones
- [ ] Debe mostrarse un toast de información indicando que el pedido no fue encontrado.
- [ ] Los campos "Serie" y "Folio" deben limpiarse automáticamente.
- [ ] El control `pedidoCompleto` debe establecerse como objeto vacío `{}`.
- [ ] Debe emitirse el evento `pedidoInvalido` al componente padre.

### Búsqueda Manual - Error en el Servicio
#### 💼 Operación
- [ ] Simular un error en el servicio de búsqueda.
- [ ] Capturar serie y folio de un pedido.
#### 🛡️ Validaciones
- [ ] Debe mostrarse un toast de advertencia: "Error al buscar pedido".
- [ ] Los campos "Serie" y "Folio" deben limpiarse automáticamente.
- [ ] El control `pedidoCompleto` debe establecerse como objeto vacío `{}`.

### Apertura de Modal de Búsqueda Avanzada
#### 💼 Operación
- [ ] Presionar el botón de búsqueda (icono search).
#### 🛡️ Validaciones
- [ ] Debe abrirse el modal "TABLA DE PEDIDOS" con dimensiones 90vw x 90vh (máximo 1200px).
- [ ] El modal debe mostrar dos secciones: tabla principal de pedidos y tabla de detalle (vacía inicialmente).
- [ ] La tabla principal debe cargar los pedidos con paginación server-side.
- [ ] Debe mostrarse la página 1 con 50 registros por defecto.

### Modal - Tabla Principal de Pedidos
#### 💼 Operación
- [ ] Observar las columnas de la tabla principal.
- [ ] Aplicar filtros en las columnas con floating filters.
- [ ] Cambiar de página usando la paginación.
#### 🛡️ Validaciones
- [ ] La tabla debe mostrar las siguientes columnas principales:
    - Indicadores visuales: E (Estadística), D (Directo Prv), I (Incancelable)
    - Serie, Folio, Fecha Orden
    - Estado (con icono de vigencia)
    - Tipo Pedido
    - Orden Cliente, Cliente ID, Cliente, Almacén ID, Almacén
    - Vigencia
    - Grupo Vendedor (ID, Nombre)
    - Grupo Capturista (Núm, Nombre)
    - Grupo Cancelación (Fecha, Hora, Usuario)
    - Grupo Autorización (Check, Fecha Aut., Hora, Usuario)
    - Grupo Designación (Días, Desasignación)
    - Subtotal, Impuesto, Total
- [ ] Los filtros flotantes deben funcionar correctamente (texto, número, fecha).
- [ ] La paginación debe cargar nuevos registros al cambiar de página.
- [ ] Los iconos de estado deben mostrarse correctamente según el valor del campo.

### Modal - Iconos y Estados Especiales
#### 🛡️ Validaciones
- [ ] El icono "E" (Estadística) debe mostrarse solo cuando `bndEstadistica` sea true.
- [ ] El icono "D" (Directo Prv) debe mostrarse solo cuando `bndDirectoPrv` sea true.
- [ ] El icono "I" (Incancelable) debe mostrarse solo cuando `bndIncancelable` sea true.
- [ ] En la columna "Estado", debe mostrarse un check verde si `vigencia` es true/1.
- [ ] En la columna "Estado", debe mostrarse una X roja si `vigencia` es false/0.
- [ ] Los estados del pedido deben mostrarse como: Cotizado (0), Pedido (1), Fact. Total (2), Fact. Parcial (3).
- [ ] Los tipos de pedido deben mostrarse como: Asig.Trasp.Compra (1), No Stock (2), Surtido Completo (3), Asigna y Compra (4), Equipo Especial (5).
- [ ] La columna "Vigencia" debe mostrar "Vigente" o "Cancelado".
- [ ] En la columna de Autorización, debe mostrarse un check solo si `bndAutorizacion` es true/1.

### Modal - Selección de Pedido (Simple Click)
#### 💼 Operación
- [ ] Hacer click en una fila de la tabla principal de pedidos.
#### 🛡️ Validaciones
- [ ] La fila seleccionada debe marcarse visualmente.
- [ ] Debe ejecutarse una petición al servicio para obtener el detalle del pedido.
- [ ] La tabla de detalle (inferior) debe cargarse con las partidas del pedido seleccionado.
- [ ] El modal debe permanecer abierto.

### Modal - Tabla de Detalle de Partidas
#### 💼 Operación
- [ ] Seleccionar un pedido de la tabla principal.
- [ ] Observar la tabla de detalle.
#### 🛡️ Validaciones
- [ ] La tabla de detalle debe mostrar las siguientes columnas:
    - Partida, Artículo, Descripción, Cantidad
    - Grupo Cantidades (Asignados, Backorder, Facturados, Máximo)
    - Precio Venta, Descuento (%), Precio Neto, Impuesto (%)
    - Grupo CEDIS (NAlm CEDIS, CEDIS Descripción)
    - Descripción Adic. 1, Ya Pedidos, Orden Compra, Corte
    - Clasif. Hits, Descripción Adic. 2
    - Grupo Promesa de Entrega (Días Prom., Fuente Prom.)
- [ ] Los valores numéricos deben alinearse a la derecha.
- [ ] Los valores de texto deben alinearse a la izquierda.

### Modal - Doble Click en Pedido
#### 💼 Operación
- [ ] Hacer doble-click en una fila de la tabla principal de pedidos.
#### 🛡️ Validaciones
- [ ] El modal debe cerrarse inmediatamente.
- [ ] El pedido seleccionado debe retornarse al componente principal.
- [ ] Los campos "Serie" y "Folio" del componente principal deben llenarse con los valores del pedido.
- [ ] El control `pedidoCompleto` debe contener el objeto completo del pedido seleccionado.
- [ ] Debe emitirse el evento `pedidoValido` al componente padre.

### Modal - Cerrar Modal sin Selección
#### 💼 Operación
- [ ] Presionar el botón "Cerrar" del modal.
- [ ] Presionar la "X" en la esquina superior derecha del modal.
#### 🛡️ Validaciones
- [ ] El modal debe cerrarse sin retornar ningún dato.
- [ ] Los campos del componente principal no deben modificarse.
- [ ] El control `pedidoCompleto` debe establecerse como objeto vacío `{}`.
- [ ] Debe emitirse el evento `pedidoInvalido` al componente padre.

### Integración con FormGroup
#### 🛡️ Validaciones
- [ ] El componente debe recibir correctamente el FormGroup mediante `@Input`.
- [ ] El FormGroup debe contener los controles: `serie`, `folio` y `pedidoCompleto`.
- [ ] Cualquier cambio en los campos debe reflejarse en el FormGroup del componente padre.
- [ ] El FormGroup debe ser accesible desde el componente padre en todo momento.

### Eventos Emitidos
#### 🛡️ Validaciones
- [ ] El evento `@Output() buscar` debe emitirse con el objeto `{ serie: string, folio: string }` cuando se ejecuta una búsqueda manual.
- [ ] El evento `@Output() pedidoValido` debe emitirse cuando se encuentra o selecciona un pedido válido.
- [ ] El evento `@Output() pedidoInvalido` debe emitirse cuando no se encuentra un pedido o se cierra el modal sin selección.
- [ ] El componente padre debe poder suscribirse a estos eventos correctamente.

### Método focusSerie()
#### 💼 Operación
- [ ] Llamar al método público `focusSerie()` desde el componente padre.
#### 🛡️ Validaciones
- [ ] El foco debe establecerse automáticamente en el campo "Serie".
- [ ] El cursor debe estar listo para captura inmediata.

### Validaciones de Formato de Fecha
#### 🛡️ Validaciones
- [ ] Las fechas en formato 1899-12-30 deben mostrarse vacías (no renderizadas).
- [ ] Las fechas válidas deben mostrarse en formato YYYY-MM-DD.
- [ ] Las horas 00:00:00 deben mostrarse vacías.
- [ ] Las horas válidas deben mostrarse en formato HH:MM:SS.

## 📎 Observaciones adicionales
- **Componente Reutilizable**: Este componente está diseñado para ser utilizado en múltiples módulos del sistema mediante `<app-pedidos-buscador>`.
- **Comunicación con Padre**: Utiliza `@Input()` para recibir el FormGroup y `@Output()` para emitir eventos al componente padre.
- **ViewChild**: Expone el método público `focusSerie()` para que el componente padre pueda establecer el foco programáticamente.
- **AG-Grid Enterprise**: Las tablas del modal utilizan AG-Grid con funcionalidades avanzadas: server-side pagination, filtros flotantes, agrupación de columnas.
- **Paginación Server-Side**: La tabla principal carga 50 registros por página y solicita nuevos datos al servidor según navegación y filtros aplicados.
- **Servicios Utilizados**:
  - `PartidasCanceladasService.buscarPedido()`: Búsqueda manual por serie y folio.
  - `PedidosService.obtenerPedidos()`: Listado de pedidos para el modal con filtros.
  - `PedidosService.obtenerDetallePedido()`: Detalle de partidas del pedido seleccionado.
- **NgToastService**: Se utiliza para mostrar notificaciones con distintos tipos: success, info, warning.
- **MatDialog**: El modal se abre con dimensiones responsivas (90vw x 90vh, máximo 1200px).
- **Función Helper**: `convertIntToDateObject()` convierte fechas en formato entero a objetos Date para visualización correcta.
- **Localización**: AG-Grid utiliza `AG_GRID_LOCALE_ES` para textos en español (paginación, filtros, etc.).
- **Iconos de Estado**: El modal muestra iconos personalizados almacenados en `assets/images/` para representar estados visuales.
- **Columnas con Grupos**: Algunas columnas están agrupadas (Vendedor, Capturista, Cancelación, Autorización, Designación, Cantidades, CEDIS, Promesa de Entrega).
- **Filtros Personalizados**: Los filtros de fecha utilizan el date picker del navegador para mejor experiencia de usuario.

> 🗓️ **Fecha de última modificación:** 2025-11-19
> 👤 **Erick López (of_dev10)**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩| 2025/11/19 | Erick López |Documentación inicial del componente compartido|
