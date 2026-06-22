# 📦 Módulo: Solicitudes Ajuste Inventario (SAI)
#### 📁 **Código:** `src\app\modules\contraloria\solicitudes-ajuste-inventario`
#### 💻 **Menú:** Menú > Contraloría > Solicitudes de Ajuste al Inventario  [Ver en QA](http://192.168.2.16:1089//app/contraloria/solicitudes-ajuste-inventario)

## 📝 Descripción

Este módulo muestra la pantalla de **Consulta de Solicitudes de Ajuste al Inventario**, cuyo objetivo es registrar, revisar y aplicar las diferencias detectadas entre el conteo físico de mercancía y las existencias registradas en el sistema. La pantalla se divide en 4 zonas:

1. **Barra de acciones**, en donde se encuentran los botones:
    - _Nueva Solicitud_: Crea una nueva Solicitud de Ajuste de Inventario.
    - _Modificar Solicitud_: Edita la solicitud seleccionada; sólo disponible cuando hay una solicitud seleccionada.
    - _Importar CSV_: Carga partidas de ajuste desde un archivo CSV.
    - _Cancelar Renglón_: Cancela la partida de detalle marcada con `Ctrl+Clic`; sólo se activa cuando hay exactamente un renglón marcado en la tabla de detalle inferior.
    - _Cancelar Solicitud_: Cancela la solicitud completa seleccionada.
    - _Marcar como Aclarada_: Registra la solicitud como aclarada sin aplicar movimientos al inventario.
    - _Aplicar Movimientos_: Ejecuta el ajuste de inventario de la solicitud seleccionada. Esta acción **no se puede deshacer**.
    - _Generar Reporte CSV_: Exporta un listado de solicitudes a archivo CSV según los parámetros indicados.
    - _Refrescar_: Recarga la lista de solicitudes desde el sistema.

2. **Tabla de Solicitudes (cuadrícula superior)**, que lista todas las solicitudes de ajuste con las columnas:
    - _Consec._: Número de consecutivo de la solicitud.
    - _Captura_: Fecha en que se capturó la solicitud (`DD/MM/AAAA`).
    - _Estado_: Estado actual de la solicitud (_Captura_, _Cancelada_, _Aclarada_, _Aplicada_, entre otros).
    - _Motivo_: Razón del ajuste.
    - _Almacén_: Número y descripción del almacén.
    - _Almacenista_: Código y nombre del almacenista responsable.
    - _Vendedor_: Número y nombre del vendedor asociado.
    Al dar clic en cualquier fila, se carga automáticamente su detalle de partidas en la tabla inferior. La navegación con `↑ ↓` también cambia la solicitud seleccionada.

3. **Tabla de Detalle de Solicitud (cuadrícula inferior)**, que muestra los renglones de ajuste de la solicitud seleccionada con las columnas:
    - _●_ (indicador): punto **verde** si el renglón ya fue aplicado al inventario; punto **rojo** si fue cancelado; sin indicador si está activo y pendiente.
    - _Partida_: Número de partida dentro de la solicitud.
    - _Artículo_ y _Descripción_: código y nombre del artículo ajustado.
    - _Existencia_, _Apartados_, _Disponible_: cantidades registradas en el sistema al momento del conteo.
    - _Conteo_: cantidad física contada.
    - _Costo Repos._ y _Costo Capas_: costos del artículo usados para calcular el impacto monetario.
    - _Movimiento_: tipo de ajuste (Alta o Baja) y su descripción.
    - _Concepto_: concepto de movimiento de inventario seleccionado.
    - _Observaciones_: notas del renglón.
    Para marcar renglones y calcular subtotales de la selección, usa `Ctrl+Clic` o `Ctrl+↑↓`.

4. **Zona de Totales**, con cuatro tarjetas que muestran los montos de impacto en **Alta**, **Baja** y **Neto** en pesos mexicanos:
    - _Sub Capas Selección_: impacto de costo de capas sólo de los renglones marcados con `Ctrl`.
    - _Sub Repos Selección_: impacto de costo de reposición sólo de los renglones marcados con `Ctrl`.
    - _Subtotales Capas_: impacto total en costo de capas de toda la solicitud seleccionada.
    - _Subtotales Reposición_: impacto total en costo de reposición de toda la solicitud.

---

### Ventana de Captura / Modificación

Al dar clic en _Nueva Solicitud_ o _Modificar Solicitud_ se abre una ventana con dos secciones:

**Datos generales de la solicitud:**
- _Consecutivo_ (visible y solo lectura en modo modificar; asignado por el sistema en modo nuevo).
- _Fecha_ (solo lectura, asignada automáticamente).
- _Almacén_ (buscador obligatorio).
- _Almacenista_ (buscador opcional).
- _Vendedor_ (buscador opcional).
- _Motivo_ (texto libre, máximo 250 caracteres).
- _Observaciones_ (área de texto, máximo 500 caracteres).

**Tabla de partidas de ajuste:**

| Columna | Descripción |
|---------|-------------|
| # | Número de partida (solo lectura) |
| Artículo | Buscador; al escribir el código exacto y dar `Tab`, se selecciona automáticamente |
| Existencia | Solo lectura; se llena al confirmar el artículo |
| Conteo | Cantidad física contada; captura del usuario |
| Ajuste | Solo lectura: diferencia entre Conteo y Existencia |
| Asigna | Indicador de asignación de tramos (solo lectura) |
| Concepto | Buscador de concepto de movimiento de inventario |
| Costo Capas | Solo lectura; se llena al confirmar el artículo |
| Costo Reps | Solo lectura; se llena al confirmar el artículo |
| Imp Capas | Solo lectura; calculado |
| Imp Reps | Solo lectura; calculado |
| Estado | Solo lectura |
| Rotativo (Folio / No. Folio) | Referencia al documento de rotativo de origen |
| Observaciones | Notas por partida |
| _(Eliminar)_ | Aparece sólo cuando hay más de una partida |

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
|---------|----------|-------------|---------------|
| ventana | Mostrar opción principal | Permite mostrar la opción en el menú de Contraloría | |
| botón | Nueva Solicitud (`sai-btn-insertar`) | Crear una nueva solicitud de ajuste de inventario | |
| botón | Modificar Solicitud (`sai-btn-modificar`) | Editar una solicitud en estado _Captura_ | |
| botón | Importar CSV (`sai-btn-importar-por-csv-sais`) | Cargar partidas de ajuste desde archivo CSV | |
| botón | Cancelar Renglón (`sai-btn-cancelar-renglon-solicitud`) | Cancelar una partida individual de la solicitud | |
| botón | Cancelar Solicitud (`sai-btn-cancelar`) | Cancelar la solicitud completa seleccionada | |
| botón | Marcar como Aclarada (`sai-btn-marcar-como-aclarada`) | Marcar la solicitud como aclarada | |
| botón | Aplicar Movimientos (`sai-btn-aplicar-movs`) | Ejecutar el ajuste de inventario (acción irreversible) | |
| botón | Generar Reporte CSV (`sai-btn-generar-reporte-csv`) | Exportar solicitudes a archivo CSV | |

## 💼 Políticas Generales

<a id="politica-1-acceso-por-permisos"></a>

### POLÍTICA 1. El módulo y sus acciones están restringidos por permisos

Cada botón de acción de la pantalla se controla individualmente: si el usuario no tiene el permiso correspondiente, el botón no aparece en la interfaz. Para otorgar o revocar accesos se utiliza _Security_.

<a id="politica-2-estados-de-la-solicitud"></a>

### POLÍTICA 2. Estados de la solicitud y operaciones permitidas

El estado de una solicitud determina qué acciones pueden realizarse sobre ella:

| Estado | Descripción | Modificar | Cancelar | Marcar Aclarada | Aplicar Movs |
|--------|-------------|-----------|----------|-----------------|--------------|
| **Captura** | Solicitud en proceso de captura | ✅ | ✅ | ✅ | ✅ |
| **Cancelada** | Solicitud cancelada por el usuario | ❌ | ❌ | ❌ | ❌ |
| **Aclarada** | Marcada como aclarada sin aplicar | ❌ | ❌ | ❌ | ❌ |
| **Aplicada** | Movimientos ya aplicados al inventario | ❌ | ❌ | ❌ | ❌ |

Los renglones de detalle tienen su propio indicador: punto **verde** si ya fue aplicado, punto **rojo** si fue cancelado, y sin indicador si está activo y pendiente de aplicar.

<a id="politica-3-flujo-de-trabajo"></a>

### POLÍTICA 3. Flujo de trabajo para una solicitud de ajuste

El proceso completo de una Solicitud de Ajuste de Inventario es:

1. **Crear** la solicitud con el botón _Nueva Solicitud_.
2. **Llenar el encabezado** con Almacén (obligatorio), Almacenista, Vendedor, Motivo y Observaciones.
3. **Capturar las partidas**: por cada artículo a ajustar, ingresar su código, la cantidad contada físicamente y el concepto de movimiento. El sistema completa automáticamente la existencia, costos y tipo de movimiento.
4. **Guardar** la solicitud. Queda en estado _Captura_.
5. **Revisar** la solicitud desde la tabla principal; al seleccionarla se ven sus renglones abajo.
6. **Aplicar los movimientos** con el botón correspondiente una vez que la solicitud esté autorizada. Esta acción es **irreversible**.

<a id="politica-4-navegacion-con-teclado-en-la-tabla-de-partidas"></a>

### POLÍTICA 4. Navegación con teclado en la tabla de captura de partidas

La tabla de partidas de la ventana de captura admite navegación completa por teclado:

- `↑ ↓ ← →`: mueve el cursor entre celdas y entre filas.
- `↑` en la primera columna de una fila (distinta a la primera): **elimina esa fila**.
- `↓` en la última columna de la última fila válida y completa: **agrega una nueva fila**.
- `Tab` en la última columna visible de la última fila válida y completa: también **agrega una nueva fila** y lleva el cursor al campo _Artículo_ de la nueva fila.
- `Tab` desde el campo _Observaciones_ del encabezado: mueve el cursor al campo _Artículo_ del primer renglón.

<a id="politica-5-confirmar-articulo-por-tab"></a>

### POLÍTICA 5. Selección automática de artículo con código exacto

Al escribir en el campo _Artículo_ un código que coincide exactamente con un artículo registrado y dar `Tab`, el sistema selecciona el artículo automáticamente y avanza el cursor al campo _Conteo_, sin necesidad de elegirlo desde la lista desplegable. Si el campo está vacío al dar `Tab`, el cursor avanza al campo siguiente sin ningún proceso adicional.

<a id="politica-6-llenado-automatico-al-confirmar-articulo"></a>

### POLÍTICA 6. Datos que se llenan automáticamente al confirmar el artículo

Al confirmar un artículo en la tabla de partidas, el sistema consulta el almacén seleccionado y completa automáticamente los siguientes campos de solo lectura:

- _Existencia_, _Apartados_, _Disponible_ (cantidades actuales en el almacén).
- _Costo Capas_ y _Costo Repos_ (precios de valuación del artículo).

Si el artículo maneja **tramos**, el sistema solicita que el usuario identifique el tramo específico a ajustar antes de continuar ([ver Política 7](#politica-7-articulos-con-tramos)).

<a id="politica-7-articulos-con-tramos"></a>

### POLÍTICA 7. Artículos con tramos

Cuando el artículo capturado maneja tramos, el sistema abre un diálogo de selección de tramo para que el usuario elija cuál tramo ajustar. Al seleccionar el tramo, los valores de _Existencia_, _Apartados_ y _Disponible_ se actualizan con las cantidades de ese tramo específico. En algunos casos, si el tramo se identifica automáticamente, el diálogo no se abre.

<a id="politica-8-tipo-de-movimiento-y-concepto"></a>

### POLÍTICA 8. Determinación del tipo de movimiento y autocompletado del concepto

Al salir del campo _Conteo_, el sistema calcula automáticamente:

1. El **tipo de movimiento**: si el conteo es mayor que la existencia → _Alta_; si es menor → _Baja_.
2. La **cantidad de ajuste** (diferencia entre conteo y existencia).
3. Abre el buscador de _Concepto_ para que el usuario elija el concepto de movimiento correspondiente.

El concepto de las demás filas ya capturadas **no se borra** al asignar el concepto de una nueva fila.

<a id="politica-9-restricciones-cancelar-renglon"></a>

### POLÍTICA 9. Restricciones para cancelar un renglón

Para cancelar un renglón de detalle con el botón _Cancelar Renglón_, se deben cumplir todas estas condiciones:

1. Exactamente **un renglón** marcado con `Ctrl+Clic` en la tabla de detalle.
2. La solicitud **no** está cancelada.
3. La solicitud **no** está marcada como _Aclarada_.
4. La solicitud **no** tiene fecha de aplicación, ni el renglón tampoco.
5. El renglón **no** está ya cancelado.

Al cumplirse todas las condiciones, se abre una ventana de confirmación donde el usuario ingresa las observaciones de cancelación y confirma la operación.

<a id="politica-10-restricciones-cancelar-solicitud"></a>

### POLÍTICA 10. Restricciones para cancelar la solicitud completa

Una solicitud sólo puede cancelarse si **no está ya cancelada** y **no ha sido aplicada** (no tiene fecha de aplicación). Al confirmar, se muestra el mensaje: _**¿Cancelar la solicitud {consecutivo}? Esta acción no se puede deshacer.**_

<a id="politica-11-restricciones-aplicar-movimientos"></a>

### POLÍTICA 11. Restricciones para aplicar movimientos

La operación _Aplicar Movimientos_ sólo puede ejecutarse si la solicitud:

- **No** está cancelada.
- **No** está marcada como _Aclarada_.
- **No** ha sido aplicada anteriormente.

Al confirmar, se muestra el mensaje: _**¿Aplicar los movimientos de la solicitud {consecutivo}? Esta acción es irreversible.**_

<a id="politica-12-importar-csv"></a>

### POLÍTICA 12. Importación de solicitud desde CSV

Permite crear una solicitud completa cargando sus partidas desde un archivo CSV. Al finalizar la importación, el sistema informa cuántas partidas fueron procesadas correctamente y cuántas fueron rechazadas, indicando el consecutivo de la nueva solicitud generada si aplica. El archivo deberá de tener la extensión CSV (*.csv) con el siguiente formato para /una de la filas que representan una partida (detalle) de una solicitud:

```csv
Consecutivo,Captura,Estado cabecera,No. Alm,Almacén,No Almacenista,Almacenista,No. Ven,Vendedor,Capturó,Motivo,FcAplicación,Aplicó,FcCancelación,Canceló,Observaciones,Partida,Clave,Artículo,Id Tramo,Existencia,Conteo,Ajuste,Movimiento,Costo Capas,Impo Capas,Costo Reps,Impo Reps,Rotativo,Obs. Partida,Estado Ren
```

Ejemplo:

```csv
1,015,0,SOLO ACLARACION ROT. 02 MAYO 2026,"MATERIAL SE ENTREGA MAL, CLIENTE CAMBIA MATERIAL CORRECTO",SD-LC1D25F7,"7,000,000",4,11,001,002036,FALTAN DATOS EN LA FACTURA LE
1,015,0,"SOLO ACLARACION ROT, 02 MAYO 2026",MATERIAL SE ENTREGA MAL CLIENTE CAMBIA MATERIAL CORRECTO,SD-LC1DT25F7,"-2,000.00",5,11,001,002036,"FALTAN DATOS EN LA FACTURA LE"
```

El contenido de ejemplo si lo copias a un archivo `EjemploImportacionSai.csv` con el botón de importar te dará una nueva solicitud con 2 detalles.

<a id="politica-13-exportar-csv"></a>

### POLÍTICA 13. Exportación de solicitudes a CSV

El botón _Generar Reporte CSV_ abre una ventana de parámetros donde el usuario indica el rango de fechas y, opcionalmente, el almacén a filtrar. Al confirmar, el sistema descarga el archivo CSV con las solicitudes del período indicado.

## 🧪 Casos de Prueba

### 1. Acceso sin permisos al módulo

#### 💼 Operación

- [ ] 1. Entra al sistema con un usuario que **no** tenga permisos para el módulo de SAI.
- [ ] 2. Verifica que en el menú lateral la opción _Contraloría > Solicitudes de Ajuste al Inventario_ no aparece o no es accesible.

#### 🛡️ Validaciones

- [ ] La ruta del módulo no aparece en el menú para el usuario sin permiso.

---

### 2. Carga inicial del módulo con permiso

#### 💼 Operación

- [ ] 1. Entra al sistema con un usuario con permisos en SAI y da clic en _Contraloría > Solicitudes de Ajuste al Inventario_.
- [ ] 2. La pantalla carga mostrando un indicador de progreso y luego presenta la tabla superior con las solicitudes existentes.
- [ ] 3. La primera fila se selecciona automáticamente y sus renglones aparecen en la tabla inferior.
- [ ] 4. La zona de totales muestra los valores calculados para esa solicitud.

#### 🛡️ Validaciones

- [ ] Tabla superior cargada con la lista de solicitudes.
- [ ] Primera fila seleccionada automáticamente y su detalle visible en la tabla inferior.
- [ ] Zona de totales (_Subtotales Capas_ y _Subtotales Reposición_) con valores calculados.
- [ ] Los botones de acción aparecen según los permisos del usuario.

---

### 3. Navegar entre solicitudes con teclado

#### 💼 Operación

- [ ] 1. Con la tabla principal activa, presiona `↓` para pasar a la siguiente solicitud.
- [ ] 2. La tabla de detalle inferior se actualiza con los renglones de la nueva solicitud seleccionada.
- [ ] 3. Presiona `↑` para volver a la solicitud anterior.

#### 🛡️ Validaciones

- [ ] La tabla de detalle se actualiza con cada cambio de solicitud.
- [ ] La zona de totales se recalcula en cada cambio.

---

### 4. Crear una nueva solicitud de ajuste

#### 💼 Operación

- [ ] 1. Da clic en el botón _Nueva Solicitud_ (icono `+` verde).
- [ ] 2. Se abre la ventana _Nueva Solicitud de Ajuste_ con algunos datos ya cargados (ej. Almacén).
- [ ] 3. Selecciona un _Almacén_ en el buscador correspondiente (campo obligatorio).
- [ ] 4. Completa _Almacenista_, _Vendedor_ (opcionales) y escribe un _Motivo_.
- [ ] 5. En la tabla de partidas, escribe el código de un artículo válido en la primera fila y presiona `Tab` para confirmarlo; los campos _Existencia_, _Costo Capas_ y _Costo Repos_ se llenan automáticamente.
- [ ] 6. Escribe la cantidad contada en _Conteo_ y presiona `Tab`; el campo _Ajuste_ se calcula y el buscador de _Concepto_ se abre.
- [ ] 7. Selecciona el concepto de movimiento correspondiente.
- [ ] 8. Da clic en _Guardar_. El sistema confirma el registro y cierra la ventana.
- [ ] 9. La tabla principal se recarga y la nueva solicitud aparece con estado _Captura_.

#### 🛡️ Validaciones

- [ ] La tabla de partidas permanece bloqueada mientras el encabezado no esté completo.
- [ ] Los campos de solo lectura del artículo se llenan al confirmarlo.
- [ ] El campo _Ajuste_ se calcula correctamente (Conteo − Existencia).
- [ ] La nueva solicitud aparece en la tabla principal tras guardar.

---

### 5. Buscar artículo con código exacto usando Tab

#### 💼 Operación

- [ ] 1. Abre la ventana de nueva solicitud, completa el encabezado y sitúate en el campo _Artículo_ de la primera partida.
- [ ] 2. Escribe el código exacto de un artículo existente (ejemplo: `A001`) y presiona `Tab`.
- [ ] 3. El artículo se selecciona automáticamente sin necesidad de elegirlo de la lista; el cursor avanza directamente al campo _Conteo_.
- [ ] 4. Los campos _Existencia_, _Costo Capas_ y _Costo Repos_ se rellenan con los datos del artículo.

#### 🛡️ Validaciones

- [ ] Artículo seleccionado automáticamente sin abrir la lista desplegable.
- [ ] Cursor posicionado en _Conteo_.
- [ ] Campos de solo lectura del artículo rellenos.

---

### 6. Dejar vacío el campo Artículo y presionar Tab

#### 💼 Operación

- [ ] 1. En la ventana de captura, sitúate en el campo _Artículo_ sin escribir nada y presiona `Tab`.
- [ ] 2. El cursor avanza al siguiente campo sin mostrar ningún indicador de carga ni mensaje de error.

#### 🛡️ Validaciones

- [ ] Ningún indicador de carga ni mensaje de error al dar `Tab` con el campo vacío.
- [ ] El cursor avanza normalmente.

---

### 7. Agregar nueva partida con Tab al final de la fila

#### 💼 Operación

- [ ] 1. En la ventana de captura, con al menos una partida completa y válida, sitúa el cursor en el último campo de esa fila.
    - Si sólo hay una partida, el último campo visible es _Observaciones_.
    - Si hay más de una partida, el último campo visible es el botón color rojo de eliminar (🗑️).
- [ ] 2. Presiona `Tab`.
- [ ] 3. Se agrega una nueva fila y el cursor se posiciona en el campo _Artículo_ de esa nueva fila.

#### 🛡️ Validaciones

- [ ] Nueva partida creada al dar `Tab` en el último campo de la última fila válida.
- [ ] Cursor en el campo _Artículo_ de la nueva fila.
- [ ] La fila activa destacada visualmente.

---

### 8. Capturar concepto en segunda partida sin borrar el de la primera

#### 💼 Operación

- [ ] 1. Captura la primera partida completa incluyendo el concepto.
- [ ] 2. Agrega una segunda partida con FLECHA ABAJO o TAB al final de la partida existente e ingresa el artículo y el conteo.
- [ ] 3. Al posicionarte en el campo _Concepto_ de la segunda partida y seleccionar un concepto, verifica que el concepto de la primera partida **no se haya borrado**.

#### 🛡️ Validaciones

- [ ] El concepto de la primera partida permanece intacto.
- [ ] Sólo el panel del buscador de conceptos de otras filas se cierra, sin limpiar el valor seleccionado.

---

### 9. Capturar partida con artículo de tramos

#### 💼 Operación

- [ ] 1. En la ventana de captura, ingresa el código de un artículo que maneje tramos y presiona `Tab`.
- [ ] 2. El sistema muestra una ventana para seleccionar el tramo específico a ajustar.
- [ ] 3. Selecciona un tramo de la lista y confirma.
- [ ] 4. Los campos _Existencia_, _Apartados_ y _Disponible_ se actualizan con las cantidades de ese tramo.
- [ ] 5. Agregar otra partida e ingresa otro código de un artículo que maneje tramos y presiona `Tab`.
- [ ] 6. Esta vez cuando te aparezca la ventana de tramo no selecciones nada, haz clic en el botón _Cancelar_.
- [ ] 7. El sistema te preguntará _**No se selecciono un ID de tramo ¿Crear un tramo nuevo? Sí - crea un ID de tramo, No - Elimina el renglón**_, selecciona una opción y el sistema te permitirá seguir la captura.

#### 🛡️ Validaciones

- [ ] Ventana de selección de tramos abre al confirmar un artículo con tramos.
- [ ] Los campos de existencias se actualizan con los datos del tramo seleccionado.
- [ ] Mensaje de tramo nuevo cuando no se haya seleccionado uno en la ventana de tramos.

---

### 10. Modificar una solicitud en estado Captura

#### 💼 Operación

- [ ] 1. Selecciona en la tabla principal una solicitud con estado _Captura_.
- [ ] 2. Da clic en _Modificar Solicitud_ (icono de lápiz).
- [ ] 3. La ventana _Modificar Solicitud de Ajuste_ se abre con los datos actuales; el consecutivo y la fecha aparecen como solo lectura.
- [ ] 4. Realiza un cambio (por ejemplo, modifica el motivo o agrega una partida) y da clic en _Guardar_.
- [ ] 5. La tabla principal se recarga con los datos actualizados.

#### 🛡️ Validaciones

- [ ] La ventana abre con los datos actuales de la solicitud.
- [ ] Consecutivo y Fecha no son editables.
- [ ] Los cambios quedan guardados correctamente.

---

### 11. Intentar modificar una solicitud fuera de estado Captura

#### 💼 Operación

- [ ] 1. Selecciona una solicitud con estado distinto de _Captura_ (por ejemplo, _Cancelada_ o _Aplicada_).
- [ ] 2. Da clic en _Modificar Solicitud_.
- [ ] 3. El sistema muestra el mensaje: _**La solicitud seleccionada no se puede modificar, ya no está en Captura.**_

#### 🛡️ Validaciones

- [ ] Mensaje de error mostrado.
- [ ] La ventana de edición no se abre.

---

### 12. Marcar renglones del detalle y revisar subtotales de selección

#### 💼 Operación

- [ ] 1. Selecciona una solicitud con varios renglones en la tabla superior.
- [ ] 2. En la tabla de detalle inferior, usa `Ctrl+Clic` en dos o más renglones para marcarlos.
- [ ] 3. Observa que las tarjetas _Sub Capas Selección_ y _Sub Repos Selección_ en la zona de totales se actualizan mostrando sólo el impacto de los renglones marcados.
- [ ] 4. Usa `Ctrl+↑` o `Ctrl+↓` para agregar más renglones a la selección y verifica que los subtotales cambian.
- [ ] 5. Desmarca un renglón con `Ctrl+Clic` nuevamente y confirma que los subtotales se recalculan.

#### 🛡️ Validaciones

- [ ] Los subtotales de selección cambian al marcar y desmarcar renglones.
- [ ] Los subtotales generales (_Subtotales Capas_ y _Subtotales Reposición_) no cambian por la selección.
- [ ] Aparece un contador con el número de renglones marcados debajo de la tabla de detalle.

---

### 13. Cancelar un renglón individual de detalle

#### 💼 Operación

- [ ] 1. Selecciona una solicitud activa (sin fecha de aplicación, no cancelada, no aclarada).
- [ ] 2. En la tabla de detalle, usa `Ctrl+Clic` en **un solo** renglón activo (sin punto rojo o verde).
- [ ] 3. Da clic en el botón _Cancelar Renglón_.
- [ ] 4. Se abre la ventana de confirmación de cancelación; escribe las observaciones y confirma.
- [ ] 5. El renglón aparece con el indicador **rojo** en la tabla de detalle.

#### 🛡️ Validaciones

- [ ] El botón _Cancelar Renglón_ sólo se activa con un renglón marcado.
- [ ] La ventana de confirmación se abre correctamente.
- [ ] El renglón queda con indicador rojo tras la cancelación.

---

### 14. Intentar cancelar renglón en solicitud aplicada

#### 💼 Operación

- [ ] 1. Selecciona una solicitud con fecha de aplicación o cancelada.
- [ ] 2. Marca un renglón con `Ctrl+Clic` y da clic en _Cancelar Renglón_.
- [ ] 3. El sistema muestra el mensaje de error correspondiente.

#### 🛡️ Validaciones

- [ ] Mensaje de error: _**La solicitud seleccionada ya se aplicó, la partida no puede ser cancelada.**_ (o el mensaje según el estado bloqueante).

---

### 15. Intentar cancelar más de un renglón al mismo tiempo

#### 💼 Operación

- [ ] 1. En la tabla de detalle, usa `Ctrl+Clic` para marcar **dos o más** renglones.
- [ ] 2. Da clic en _Cancelar Renglón_.
- [ ] 3. El sistema muestra el aviso: _**Sólo se puede cancelar un detalle seleccionado.**_

#### 🛡️ Validaciones

- [ ] Mensaje de advertencia mostrado cuando hay más de un renglón marcado.
- [ ] No se realiza ninguna cancelación.

---

### 16. Cancelar la solicitud completa

#### 💼 Operación

- [ ] 1. Selecciona en la tabla principal una solicitud en estado _Captura_.
- [ ] 2. Da clic en _Cancelar Solicitud_.
- [ ] 3. El sistema muestra la confirmación: _**¿Cancelar la solicitud {consecutivo}? Esta acción no se puede deshacer.**_ Da clic en _No_ para verificar que no cancela, luego repite el paso 2 y confirma con _Sí_.
- [ ] 4. La solicitud aparece con estado _Cancelada_ en la tabla principal.

#### 🛡️ Validaciones

- [ ] Mensaje de confirmación con advertencia de irreversibilidad.
- [ ] Al responder _No_, la solicitud no se cancela.
- [ ] Al responder _Sí_, la solicitud queda en estado _Cancelada_.

---

### 17. Marcar solicitud como Aclarada

#### 💼 Operación

- [ ] 1. Selecciona una solicitud activa en la tabla principal.
- [ ] 2. Da clic en _Marcar como Aclarada_.
- [ ] 3. El sistema muestra: _**¿Marcar la solicitud {consecutivo} como aclarada?**_; confirma.
- [ ] 4. El sistema muestra: _**Solicitud marcada como aclarada correctamente.**_
- [ ] 5. La solicitud aparece con estado _Aclarada_ en la tabla.

#### 🛡️ Validaciones

- [ ] Mensaje de confirmación mostrado antes de proceder.
- [ ] Mensaje de éxito tras confirmar.
- [ ] Estado de la solicitud actualizado a _Aclarada_.

---

### 18. Aplicar movimientos de inventario

#### 💼 Operación

- [ ] 1. Selecciona una solicitud en estado _Captura_ (sin fecha de aplicación).
- [ ] 2. Da clic en _Aplicar Movimientos_.
- [ ] 3. El sistema muestra: _**¿Aplicar los movimientos de la solicitud {consecutivo}? Esta acción es irreversible.**_ Da clic en _No_ para verificar que no aplica; repite el paso 2 y confirma con _Sí_.
- [ ] 4. El sistema procesa y muestra: _**Movimientos aplicados correctamente.**_
- [ ] 5. Los renglones de la solicitud muestran el indicador **verde** y la solicitud queda como _Aplicada_.
- [ ] 6. Se enviará un correo de notificación de la aplicación de movimientos a los interesados en la solicitud.

#### 🛡️ Validaciones

- [ ] Mensaje de confirmación con advertencia de irreversibilidad.
- [ ] Al responder _No_, ningún movimiento se aplica.
- [ ] Al responder _Sí_, mensaje de éxito y renglones con indicador verde.
- [ ] Los botones _Aplicar Movimientos_, _Cancelar Solicitud_ y _Modificar_ quedan bloqueados para esa solicitud.
- [ ] Correos recibidos por los interesados de la solicitud.

---

### 19. Intentar aplicar movimientos en solicitud no elegible

#### 💼 Operación

- [ ] 1. Selecciona una solicitud en estado _Cancelada_ y da clic en _Aplicar Movimientos_.
   - [ ] El sistema muestra: _**La solicitud seleccionada se encuentra cancelada, no se pueden aplicar los movimientos.**_
- [ ] 2. Selecciona una solicitud en estado _Aclarada_ y da clic en _Aplicar Movimientos_.
   - [ ] El sistema muestra: _**La solicitud ya se marcó como "Aclarada", no se pueden aplicar los movimientos.**_
- [ ] 3. Selecciona una solicitud ya aplicada y da clic en _Aplicar Movimientos_.
   - [ ] El sistema muestra: _**La solicitud seleccionada ya se aplicó, no se pueden aplicar los movimientos nuevamente.**_

#### 🛡️ Validaciones

- [ ] Mensaje de error correcto en cada caso según el estado de la solicitud.
- [ ] En ningún caso se ejecutan movimientos.

---

### 20. Importar solicitud desde CSV

#### 💼 Operación

- [ ] 1. Da clic en _Importar CSV_.
- [ ] 2. Se abre la ventana de importación. Selecciona un archivo CSV con el formato correcto (ver [Política 12](#politica-12-importar-csv)).
- [ ] 3. Confirma la importación y espera el resultado.
- [ ] 4. El sistema informa el número de partidas procesadas y rechazadas, y el consecutivo de la nueva solicitud si el proceso fue exitoso.
- [ ] 5. La tabla principal se recarga mostrando la nueva solicitud en la última página en el último lugar.

#### 🛡️ Validaciones

- [ ] Ventana de importación se abre correctamente.
- [ ] Resultado con conteo de partidas procesadas y rechazadas.
- [ ] Nueva solicitud visible en la tabla principal tras una importación exitosa.

---

### 21. Exportar solicitudes a CSV

#### 💼 Operación

- [ ] 1. Da clic en _Generar Reporte CSV_.
- [ ] 2. Se abre la ventana de parámetros; ingresa un rango de fechas válido y, si lo deseas, selecciona un almacén para filtrar.
- [ ] 3. Da clic en _Generar_; el sistema descarga el archivo CSV.
- [ ] 4. Para salir sin descargar, da clic en _Cancelar_ o en la _x_ de la esquina superior derecha.

#### 🛡️ Validaciones

- [ ] Ventana de parámetros se abre correctamente.
- [ ] Descarga del CSV al confirmar.
- [ ] Cierre de la ventana con _Cancelar_ o la _x_.

---

### 22. Refrescar la pantalla

#### 💼 Operación

- [ ] 1. Da clic en el botón _Refrescar_ (icono de recarga verde).
- [ ] 2. La tabla principal muestra un indicador de carga mientras consulta nuevamente las solicitudes.
- [ ] 3. Al terminar, la primera fila se selecciona automáticamente y su detalle se carga en la tabla inferior.

#### 🛡️ Validaciones

- [ ] Indicador de carga visible durante el refresco.
- [ ] Tabla principal actualizada con los datos más recientes.
- [ ] Primera fila seleccionada automáticamente al finalizar.

## 📎 Observaciones adicionales

- **Nuevos autocompletes de búsqueda reutilizables** disponibles para toda la aplicación: _Catálogo de Almacenistas_, _Rotativos_ y _Conceptos de Movimientos al Inventario_. Los tres cuentan con búsqueda por texto, selección con clic o tecla `Enter`, y selección automática del primer resultado con `Tab`.
- **Mejoras a la navegación por teclado en la ventana de Nueva / Modificar solicitud**: se reforzó la navegación entre campos con `Tab` para avanzar de campo en campo sin usar el ratón, incluyendo el paso del encabezado a la tabla de partidas y la adición automática de nuevas filas al llegar al final de la última partida válida. Además se implementó el soporte para `F10` con los campos de Rotativos de la tabla de captura, como atajo para guardar desde la tabla sin abandonar el flujo de captura.
- **Mecánica de selección múltiple en la tabla de detalle**: los renglones de la tabla _Detalles de Solicitud_ se pueden seleccionar individualmente o en grupo mediante `Ctrl+Clic` o `Ctrl+↑↓`. La selección actualiza en tiempo real las tarjetas _Sub Capas Selección_ y _Sub Repos Selección_ de la zona de totales, permitiendo verificar el impacto monetario de un subconjunto de partidas sin perder de vista los totales generales de la solicitud completa.

> 🗓️ **Fecha de última modificación:** 2026-06-09
> 👤 **Sergio Tostado, Santos Vallecillo**
> 🏷️ **Versión:** 1

---

## Comunicaciones

| Dir | Fecha      | Firma | Comentario                                                                                |
|-----|------------|-------|-------------------------------------------------------------------------------------------|
| ⏩  | 2026/06/09 | ST    | Primera versión                                                                           |
| ⏩  | 2026/06/09 | SV   | Participación en análisis de fabricación del módulo y mejora de autocomplete de rotativos |
