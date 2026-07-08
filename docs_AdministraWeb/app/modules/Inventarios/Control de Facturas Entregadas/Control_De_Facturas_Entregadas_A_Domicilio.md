# 📦 Módulo: Control de Facturas Entregadas a Domicilio
#### 📁 **Código:** `src\app\modules\inventarios\control-facturas-entrega`
#### 💻 **Menú:** Menú > Inventarios > Control de Facturas Entregadas  [Ver en QA](http://192.168.2.16:1089/app/inventarios/control-facturas-entrega)

---

## 📝 Descripción

Pantalla de consulta maestro-detalle para administrar las rutas de entrega de facturas a domicilio y las facturas asignadas a cada una. Se compone de las siguientes zonas:

- **Barra de acciones (encabezado):** botones de operación sobre la ruta seleccionada, en el siguiente orden:
  - **Agregar** (➕): abre el diálogo para capturar una nueva ruta de entrega.
  - **Iniciar Ruta** (🚚): cambia la ruta seleccionada de "En Espera" a "En Ruta".
  - **Eliminar** (🗑️): borra la ruta seleccionada y sus facturas asociadas.
  - **Recepción de Facturas** (📋): abre el diálogo para registrar el resultado de entrega (Entregada/No Entregada) de cada factura de la ruta.
  - **Imprimir Folio Seleccionado** (🖨️): genera el PDF "Bitácora de Trayectos" de la ruta seleccionada.
  - **Imprimir Reporte Contrarecibo** (🧾): genera el PDF de Contrarecibo de la ruta seleccionada.
  - **Control de Entrega de Facturas a Unidades** (📝): genera el PDF de control de entrega a unidades de la ruta seleccionada.
  - **Reporte Control de Facturas** (✅): abre un diálogo de parámetros (fechas, almacenes, estado) para generar un listado de facturas multi-ruta.
  - **Refrescar** (🔄): siempre visible, recarga la tabla de rutas desde la primera página.

- **Tabla de rutas (grid superior):** listado de rutas de entrega (una fila por ruta), con leyenda de color encima:
  - 🔘 Gris = En Espera · 🔵 Azul = En Ruta · 🟢 Verde = Ruta Terminada
  - Columnas: círculo de estado, Consecutivo, Almacén, Zona, Chofer, Inicio Ruta (Fecha/Hora), Fin Ruta (Fecha/Hora). Las fechas/horas sin capturar se muestran como `--/--/----` y `--:--:--`.
  - Con paginación (25/50/100 registros por página).

- **Tabla de facturas (grid inferior):** al seleccionar una ruta en el grid superior, se cargan sus facturas asociadas, con leyenda de color encima:
  - 🔵 Azul = En Ruta · 🟢 Verde = Entregada · 🔴 Rojo = No Entregada · 🟠 Naranja = Se Reasigna Ruta
  - Columnas agrupadas: Factura (Letra, Número), Cliente (N° Cliente, Nombre), Importe, Revisión (Contrarecibo / Factura).
  - Inicia vacío hasta que se selecciona una ruta; no se refresca automáticamente al recuperar el foco de la pantalla.

- **Diálogo "Nueva Ruta de Entrega" / "Recepción de Facturas" (`add-edit-cfed`):** una misma ventana reutilizada en dos modos:
  - **Modo Agregar** (desde el botón "Agregar"): encabezado editable (Almacén, Zona, Supervisor, Chofer) y tabla de captura donde se buscan facturas por autocompletar (agregando/eliminando líneas). El consecutivo de la ruta lo asigna el sistema al guardar.
  - **Modo Recepción** (desde el botón "Recepción de Facturas"): encabezado de solo lectura (precargado con los datos de la ruta) y tabla con las facturas de la ruta, donde cada fila requiere seleccionar un estado (Entregada / No Entregada) antes de poder guardar. Un ícono junto a cada partida indica si la fila es válida (✅) o inválida (⚠️).

- **Diálogo "Reporte Control de Facturas" (`vpr-control-facturas`):** ventana de parámetros independiente de la ruta seleccionada, con:
  - Sección **Fechas** (Fecha Inicial / Fecha Final, selección únicamente vía calendario, sin tecleo ni pegado).
  - Sección **Almacenes** (Almacén Inicial / Almacén Final, opcionales).
  - Sección **Estado** (radios: Recibido, En Tránsito, Sin Recibir, Todos).
  - Botones **Limpiar** (restablece los valores por defecto), **Generar** (genera y descarga el PDF) y **Cancelar** (cierra el diálogo).

---

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
|---------|----------|-------------|---------------|
| Botón | `cfe-btn-agregar` | Botón "Agregar" (nueva ruta de entrega) | Almacenista |
| Botón | `cfe-btn-en-ruta` | Botón "Iniciar Ruta" | Almacenista |
| Botón | `cfe-btn-eliminar` | Botón "Eliminar" ruta | Almacenista |
| Botón | `cfe-btn-rec-fac` | Botón "Recepción de Facturas" | Almacenista |
| Botón | `cfe-btn-imprimir-folio` | Botón "Imprimir Folio Seleccionado" | Almacenista |
| Botón | `cfe-btn-contrarecibo` | Botón "Imprimir Reporte Contrarecibo" | Almacenista |
| Botón | `cfe-btn-entrega-unidades` | Botón "Control de Entrega de Facturas a Unidades" | Almacenista |
| Botón | `cfe-btn-con-fac` | Botón "Reporte Control de Facturas" | Almacenista |
| Pantalla | `administra.inventarios.controldefacturasentregadas` | Acceso general a la pantalla y a los grids (Rutas / Facturas) | Almacenista |

> El botón "Refrescar" no tiene permiso asociado — está siempre visible.  
> Si se desea agregar otro rol o permiso para un usuario se solicita vía Security.

---

## 💼 Políticas Generales

<a id="politica-1-estados-de-ruta"></a>
### POLÍTICA 1. Estados de una ruta de entrega

Una ruta de entrega (ELCFED) transita por 3 estados, y las acciones disponibles dependen del estado de la fila seleccionada en el grid de Rutas:

| Estado | Valor | Botón "Iniciar Ruta" | Botón "Eliminar" | Botón "Recepción de Facturas" |
|---|---|---|---|---|
| En Espera | 1 | Habilitado | Habilitado | Deshabilitado |
| En Ruta | 2 | Deshabilitado | Deshabilitado | Habilitado |
| Ruta Terminada | 3 | Deshabilitado | Deshabilitado | Deshabilitado |

Los botones de impresión (Imprimir Folio, Reporte Contrarecibo, Control de Entrega a Unidades) no dependen del estado de la ruta — solo requieren que exista una ruta seleccionada. El botón "Reporte Control de Facturas" tampoco depende de selección alguna, ya que genera un listado multi-ruta filtrado por parámetros propios.

<a id="politica-2-agregar-ruta"></a>
### POLÍTICA 2. Alta de una nueva ruta de entrega

Al capturar una nueva ruta desde el botón "Agregar":

- El encabezado (Almacén, Zona, Supervisor, Chofer) es obligatorio en su totalidad.
- Debe capturarse al menos una factura en la tabla de detalle; cada línea se completa buscando la factura por su autocompletador (el sistema solo permite seleccionar facturas existentes).
- No se permite capturar la misma factura dos veces en la lista.
- Al confirmar, el sistema asigna automáticamente el número de consecutivo de la nueva ruta.
- Tras registrar exitosamente la ruta, el sistema genera en paralelo el reporte "Bitácora de Trayectos" y — si la ruta incluye al menos una factura marcada como Contrarecibo — el reporte "Contrarecibo", y pregunta al usuario si desea visualizarlos.

<a id="politica-3-eliminar-ruta"></a>
### POLÍTICA 3. Eliminación de una ruta

Solo se puede eliminar una ruta en estado "En Espera" (1). Al eliminar se borran tanto el encabezado de la ruta como todas sus facturas asociadas, de forma conjunta e irreversible.

<a id="politica-4-recepcion-facturas"></a>
### POLÍTICA 4. Recepción de facturas (cierre de ruta)

Aplica únicamente a rutas en estado "En Ruta" (2). Cada factura de la ruta debe recibir un estado (Entregada / No Entregada) — no se permite guardar mientras alguna partida conserve el valor "Ninguno". Al confirmar el proceso, la ruta cambia a estado "Ruta Terminada" (3) y esta acción no puede deshacerse ni modificarse posteriormente.

<a id="politica-5-reportes-de-ruta"></a>
### POLÍTICA 5. Reportes asociados a una ruta seleccionada

Tres reportes se generan sobre la ruta actualmente seleccionada en el grid: Bitácora de Trayectos ("Imprimir Folio"), Contrarecibo y Control de Entrega de Facturas a Unidades. En los tres casos:

- El PDF se descarga automáticamente al generarse.
- La visualización en una pestaña nueva **no es automática**: el sistema pregunta *"¿Deseas visualizar el reporte generado?"* y solo se abre si el usuario confirma.
- El reporte de Contrarecibo requiere que la ruta tenga al menos un documento marcado como "Contrarecibo" (`Bnd_ConFacOri`); si no lo tiene, el sistema informa que no hay documentos de ese tipo para generar el reporte.

<a id="politica-6-reporte-control-facturas"></a>
### POLÍTICA 6. Reporte Control de Facturas (listado multi-ruta)

Este reporte no depende de una ruta seleccionada: se genera a partir de los parámetros capturados en su propio diálogo.

- Valores por defecto al abrir: Fecha Inicial y Fecha Final = hoy; Almacén Inicial/Final = sin capturar (sin filtro de almacén); Estado = "Todos".
- La Fecha Inicial no puede ser mayor a la Fecha Final.
- Si se capturan ambos almacenes, el Almacén Inicial no puede ser mayor al Almacén Final.
- Si se genera el reporte sin resultados que coincidan con los parámetros, el sistema lo informa en pantalla.
- El diálogo permanece abierto tras generar el reporte y **conserva los valores capturados** — no se limpia automáticamente. El botón "Limpiar" restablece los valores por defecto de forma explícita.

---

## 🧪 Casos de Prueba

### 1. Iniciar una ruta en espera
[ver Política 1](#politica-1-estados-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una fila del grid de Rutas cuyo estado sea "En Espera" (círculo gris).
- [ ] 2. Hacer clic en el botón "Iniciar Ruta".
- [ ] 3. Confirmar en el diálogo.

#### 🛡️ Validaciones
- [ ] El diálogo de confirmación muestra el texto exacto: *"¿Seguro quieres poner en ruta este registro?"*
- [ ] Al confirmar, la ruta cambia su círculo de estado a azul (En Ruta) y se registran fecha/hora de inicio.
- [ ] La misma fila permanece seleccionada tras recargar el grid.

### 2. Intentar iniciar ruta sin selección o con ruta que no está "En Espera"
[ver Política 1](#politica-1-estados-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta en estado "En Ruta" o "Ruta Terminada", o no seleccionar ninguna.

#### 🛡️ Validaciones
- [ ] El botón "Iniciar Ruta" permanece deshabilitado.

### 3. Agregar una nueva ruta de entrega
[ver Política 2](#politica-2-agregar-ruta)

#### 💼 Operación
- [ ] 1. Hacer clic en el botón "Agregar".
- [ ] 2. Capturar Almacén, Zona, Supervisor y Chofer.
- [ ] 3. Buscar y seleccionar al menos una factura en la tabla de captura.
- [ ] 4. Hacer clic en "Aceptar" (Generar/Guardar).
- [ ] 5. Confirmar el diálogo.

#### 🛡️ Validaciones
- [ ] El diálogo de confirmación muestra: *"¿Agregar nueva ruta? Se registrará la nueva ruta con las facturas capturadas."*
- [ ] Al confirmar exitosamente, se muestra el mensaje: *"Ruta #N registrada correctamente."*
- [ ] El sistema pregunta: *"¿Desea visualizar los reportes de la ruta?"* tras generar los reportes automáticos.
- [ ] La nueva ruta aparece en el grid de Rutas en estado "En Espera".

### 4. Intentar agregar ruta con encabezado incompleto
[ver Política 2](#politica-2-agregar-ruta)

#### 💼 Operación
- [ ] 1. Abrir el diálogo "Agregar".
- [ ] 2. Dejar sin capturar alguno de los campos del encabezado (Almacén, Zona, Supervisor o Chofer).
- [ ] 3. Intentar aceptar.

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje: *"Todos los campos del encabezado son requeridos."*
- [ ] Cada campo faltante marca su error correspondiente (ej. *"Seleccionar un almacén"*, *"Seleccionar una zona"*, *"Seleccionar un supervisor"*, *"Seleccionar un chofer"*).

### 5. Intentar agregar ruta sin facturas o con filas inválidas
[ver Política 2](#politica-2-agregar-ruta)

#### 💼 Operación
- [ ] 1. Abrir el diálogo "Agregar" con encabezado completo.
- [ ] 2. No seleccionar ninguna factura en la línea de captura, o dejar una línea sin factura seleccionada.
- [ ] 3. Intentar aceptar.

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje: *"Debe seleccionar una factura en cada fila antes de guardar."*

### 6. Intentar capturar una factura duplicada
[ver Política 2](#politica-2-agregar-ruta)

#### 💼 Operación
- [ ] 1. En el diálogo "Agregar", seleccionar la misma factura en dos líneas distintas.

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje: *"La factura ya existe en la lista."*
- [ ] La línea duplicada se limpia automáticamente.

### 7. Eliminar una ruta en espera
[ver Política 3](#politica-3-eliminar-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta en estado "En Espera".
- [ ] 2. Hacer clic en el botón "Eliminar".
- [ ] 3. Confirmar en el diálogo.

#### 🛡️ Validaciones
- [ ] El diálogo de confirmación muestra: *"¿Seguro quieres eliminar este registro? Esta acción no se puede deshacer."*
- [ ] Al confirmar, se muestra el mensaje: *"Ruta #N eliminada correctamente."*
- [ ] La ruta desaparece del grid y la vista regresa a la primera página.

### 8. Intentar eliminar una ruta que no está en espera
[ver Política 3](#politica-3-eliminar-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta en estado "En Ruta" o "Ruta Terminada".

#### 🛡️ Validaciones
- [ ] El botón "Eliminar" permanece deshabilitado (no es posible provocar el mensaje de error de estado desde la UI normal, ya que el botón nunca se habilita).

### 9. Recepción de facturas — completar el cierre de una ruta
[ver Política 4](#politica-4-recepcion-facturas)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta en estado "En Ruta".
- [ ] 2. Hacer clic en el botón "Recepción de Facturas".
- [ ] 3. Para cada factura de la lista, seleccionar un estado ("Entregada" o "No Entregada").
- [ ] 4. Hacer clic en "Aceptar".
- [ ] 5. Confirmar en el diálogo.

#### 🛡️ Validaciones
- [ ] El diálogo de confirmación muestra: *"¿Terminar el Proceso? Una vez hecho esto, no se podrá modificar."*
- [ ] Al confirmar, se muestra el mensaje: *"Recepción de facturas procesada correctamente."*
- [ ] La ruta cambia a estado "Ruta Terminada" en el grid.

### 10. Intentar cerrar la recepción con partidas sin estado asignado
[ver Política 4](#politica-4-recepcion-facturas)

#### 💼 Operación
- [ ] 1. Abrir "Recepción de Facturas" y dejar al menos una partida con el estado "Ninguno".

#### 🛡️ Validaciones
- [ ] El ícono junto a la partida se muestra en rojo (partida inválida) y aparece el texto *"Seleccionar un estado válido"* al interactuar con el campo.
- [ ] El botón "Aceptar" permanece deshabilitado mientras exista al menos una partida sin estado.

### 11. Cancelar un diálogo con cambios sin guardar (Agregar / Recepción)

#### 💼 Operación
- [ ] 1. Capturar información en el diálogo "Agregar" o "Recepción de Facturas".
- [ ] 2. Hacer clic en "Cancelar" o en el botón de cerrar (X).

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje de confirmación: *"¿Descartar cambios? Los cambios que has realizado no se guardarán."*
- [ ] Si se confirma, el diálogo se cierra sin guardar cambios; si se cancela, el diálogo permanece abierto.

### 12. Imprimir Folio Seleccionado (Bitácora de Trayectos)
[ver Política 5](#politica-5-reportes-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar cualquier ruta del grid.
- [ ] 2. Hacer clic en "Imprimir Folio Seleccionado".

#### 🛡️ Validaciones
- [ ] El PDF se descarga automáticamente.
- [ ] Se pregunta *"¿Deseas visualizar el reporte generado?"*; solo se abre en pestaña nueva si se confirma.

### 13. Imprimir Reporte Contrarecibo con ruta sin documentos de contrarecibo
[ver Política 5](#politica-5-reportes-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta sin facturas marcadas como Contrarecibo.
- [ ] 2. Hacer clic en "Imprimir Reporte Contrarecibo".

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje: *"La ruta seleccionada no tiene documentos con factura original (Bnd_ConFacOri) para generar el contrarecibo."*

### 14. Imprimir Reporte Contrarecibo con datos válidos
[ver Política 5](#politica-5-reportes-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar una ruta con al menos un documento marcado como Contrarecibo.
- [ ] 2. Hacer clic en "Imprimir Reporte Contrarecibo".

#### 🛡️ Validaciones
- [ ] El PDF se descarga automáticamente; se pregunta antes de visualizarlo en pestaña nueva.

### 15. Generar Control de Entrega de Facturas a Unidades
[ver Política 5](#politica-5-reportes-de-ruta)

#### 💼 Operación
- [ ] 1. Seleccionar cualquier ruta del grid.
- [ ] 2. Hacer clic en "Control de Entrega de Facturas a Unidades".

#### 🛡️ Validaciones
- [ ] El PDF se descarga automáticamente; se pregunta antes de visualizarlo en pestaña nueva.

### 16. Generar Reporte Control de Facturas con parámetros por defecto
[ver Política 6](#politica-6-reporte-control-facturas)

#### 💼 Operación
- [ ] 1. Hacer clic en "Reporte Control de Facturas".
- [ ] 2. Sin modificar los valores por defecto, hacer clic en "Generar".

#### 🛡️ Validaciones
- [ ] El PDF se descarga automáticamente (listado de todas las facturas del día, todos los estados y almacenes).
- [ ] El diálogo permanece abierto con los mismos valores capturados.

### 17. Reporte Control de Facturas con fecha final menor a la inicial

#### 💼 Operación
- [ ] 1. Abrir "Reporte Control de Facturas".
- [ ] 2. Capturar una Fecha Final anterior a la Fecha Inicial.
- [ ] 3. Hacer clic en "Generar".

#### 🛡️ Validaciones
- [ ] Se muestra la advertencia: *"La fecha final no puede ser menor a la fecha inicial"* (título "Error de Rango").
- [ ] No se envía la solicitud del reporte.

### 18. Reporte Control de Facturas con almacén final menor al inicial

#### 💼 Operación
- [ ] 1. Abrir "Reporte Control de Facturas".
- [ ] 2. Capturar Almacén Inicial y Almacén Final, con el Final menor al Inicial.
- [ ] 3. Hacer clic en "Generar".

#### 🛡️ Validaciones
- [ ] Se muestra la advertencia: *"El número de almacén final no puede ser menor al número de almacén inicial"* (título "Error de Rango").
- [ ] No se envía la solicitud del reporte.

### 19. Reporte Control de Facturas sin resultados

#### 💼 Operación
- [ ] 1. Capturar parámetros (fechas/almacenes/estado) que no correspondan a ninguna factura registrada.
- [ ] 2. Hacer clic en "Generar".

#### 🛡️ Validaciones
- [ ] Se muestra el mensaje: *"No se encontraron facturas con los parámetros seleccionados."*

### 20. Limpiar el diálogo de Reporte Control de Facturas

#### 💼 Operación
- [ ] 1. Modificar los parámetros del diálogo (fechas, almacenes, estado).
- [ ] 2. Hacer clic en el botón "Limpiar".

#### 🛡️ Validaciones
- [ ] Los campos regresan a sus valores por defecto: Fecha Inicial/Final = hoy, Almacenes vacíos, Estado = "Todos".

### 21. Validación de seguridad — botones sin permiso asignado

#### 💼 Operación
- [ ] 1. Iniciar sesión con un usuario que no tenga asignados los permisos `cfe-btn-agregar`, `cfe-btn-en-ruta`, `cfe-btn-eliminar`, `cfe-btn-rec-fac`, `cfe-btn-imprimir-folio`, `cfe-btn-contrarecibo`, `cfe-btn-entrega-unidades` o `cfe-btn-con-fac`.
- [ ] 2. Ingresar a la pantalla de Control de Facturas Entregadas.

#### 🛡️ Validaciones
- [ ] Cada botón correspondiente al permiso no asignado no se muestra en la barra de acciones.
- [ ] El botón "Refrescar" se sigue mostrando (no requiere permiso).

---

## 📎 Observaciones adicionales

- **Cambios a componentes compartidos realizados durante este desarrollo:**
  - `almacenistas-autocomplete.component.ts` — se agregó `@Input() modoSupervisor = false`; ambas llamadas al servicio lo propagan.
  - `catalogo-almacenistas.service.ts` — se agregó el parámetro `modoSupervisor = false`, enviado como query param.
  - Se renombró `chofer-autocomplete` a `operadores-autocomplete` (tabla ELOPE), ya que `personal-autocomplete` (RRHH) no era reutilizable. Selector nuevo: `app-operadores-autocomplete`.
  - `operadores.service.ts` se movió de `shared/services/` al módulo de catálogos de operadores.
  - `seleccionde-facturas-dto.ts` — se agregó el campo `cte_BndConfacori?: boolean`, usado para determinar si una factura corresponde a "Contrarecibo" o "Factura" al capturar una nueva ruta.
  - El helper compartido `abrirPdfBase64()` (usado por los 4 reportes del módulo) cambió su comportamiento de UX: la visualización del PDF en una pestaña nueva ya no ocurre automáticamente al generarse el reporte — ahora siempre se descarga primero y después se pregunta al usuario si desea visualizarlo, para evitar un problema intermitente de pestaña en blanco detectado en pruebas.

- **Pendiente de infraestructura, sin impacto visible para el usuario:** la tabla `ELDCFED` no cuenta con llave primaria definida a nivel de base de datos (solo un índice único). Queda pendiente que el equipo de base de datos agregue la llave primaria correspondiente; mientras tanto, el módulo opera con normalidad.

- El botón "Reporte Control de Facturas" filtra por la fecha de **registro de la ruta**, no por la fecha de inicio de ruta. Si se captura solo uno de los dos almacenes (Inicial o Final) sin el otro, el reporte puede no devolver resultados — se recomienda capturar ambos o dejarlos vacíos.

---

> 🗓️ **Fecha de última modificación:** 08/07/2026
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/07/08 | ST | Primera versión |
