# 📦 Módulo: Saldos por Vencer
#### 📁 **Código:** `src\app\modules\cxcobrar\reportes\saldos-por-vencer`
#### 💻 **Menú:** Menú > CxC > Reportes > Saldos por Vencer  [Ver en QA](http://192.168.2.16:1089/app/cxcobrar/reportes/saldos-por-vencer)

---

## 📝 Descripción

**Barra de acciones**
- Botón **Cambiar Parámetros** (azul, ícono filtro): abre el dialog de parámetros en cualquier momento para ajustar los criterios de consulta.
- Botón **PDF Detallado** (rojo): genera el PDF con desglose por documento de cada cliente. Permanece deshabilitado hasta que el grid tenga datos.
- Botón **PDF Concentrado** (rojo): genera el PDF con totales agrupados por cliente. Permanece deshabilitado hasta que el grid tenga datos.

**Estado vacío**
Cuando aún no se han cargado datos, el área del grid muestra el mensaje: *"Configure los parámetros y haga clic en **Aceptar** para cargar los saldos."*

**Tabla de resultados** (AG Grid)
Presenta los saldos por documento con las columnas: N.Cte, Cliente, T.Doc, Folio, Fc.Doc, Fc.Venc, Imp.Vencido, Día 1, Día 2, Día 3 y Día+. Todas las columnas cuentan con filtros flotantes. La paginación muestra 100 registros por página.

**Fila de totales**
Fila fija al pie del grid, siempre visible, que suma las columnas de importes (Imp.Vencido, Día 1, Día 2, Día 3 y Día+) considerando únicamente las filas visibles tras aplicar filtros del grid. Se recalcula automáticamente cada vez que se modifica un filtro.

**Dialog de parámetros — Filtros: Saldos por Vencer**
- **Rango de Clientes**: campos *Cliente Inicial* y *Cliente Final* (autocompletadores por nombre o número).
- **Parámetros**: campo *Fecha de Corte* (requerida) y selector *Anticipos* (Sin Anticipos / Con Anticipos Detallado / Con Anticipos Concentrado).
- **Días de Análisis**: tres campos numéricos — *1er Período*, *2do Período* y *3er Período* — que definen los rangos de días para las columnas Día 1, Día 2, Día 3 y Día+ (valores por defecto: 30, 60 y 90 días).
- **Filtros de Catálogos**: *Zona de Cobranza* y *Vendedor* (autocompletadores; dejar vacío para incluir todos).
- **Tipo de Cliente**: selector Todos / Crédito / Contado.
- Botones **Aceptar** (aplica los parámetros y ejecuta la consulta) y **Cancelar** (cierra sin consultar).

---

## 🔐 Seguridad

El módulo completo está protegido a nivel de ruta por `authGuard`. No se definieron permisos de elemento de UI adicionales en esta versión.

| Tipo UI | Elemento | Descripción | Rol permitido |
|---------|----------|-------------|---------------|
| Ruta | Saldos por Vencer | Acceso al módulo completo | |

---

## 💼 Políticas Generales

<a id="politica-1-apertura-automatica"></a>
### POLÍTICA 1. Apertura automática del dialog de parámetros

Al ingresar al módulo, el dialog de parámetros se abre automáticamente. Los últimos parámetros utilizados se recuperan de `localStorage` y precargan el formulario, de modo que el usuario solo necesita confirmar o ajustar los valores antes de ejecutar la consulta.

<a id="politica-2-periodos-de-dias"></a>
### POLÍTICA 2. Configuración de períodos de días de análisis

Los tres campos *1er Período*, *2do Período* y *3er Período* (en días) determinan los rangos que la función SQL utiliza para clasificar los saldos en las columnas Día 1, Día 2, Día 3 y Día+. Por defecto se cargan con 30, 60 y 90 días. Todos son obligatorios y deben ser mayores a 0. Los valores se persisten en `localStorage` entre sesiones.

<a id="politica-3-tipos-de-documento"></a>
### POLÍTICA 3. Tipos de documento en el grid

La columna **T.Doc** puede mostrar los valores:

| Valor | Significado |
|-------|-------------|
| F | Factura |
| A | Anticipo |

<a id="politica-4-manejo-de-anticipos"></a>
### POLÍTICA 4. Manejo de anticipos

El selector *Anticipos* controla si los anticipos del cliente aparecen en la consulta y en los reportes PDF:

| Opción | Comportamiento |
|--------|---------------|
| Sin Anticipos | Solo se incluyen facturas; los anticipos se excluyen del resultado. |
| Con Anticipos Detallado | Las facturas se listan individualmente; los anticipos se presentan en sección separada por cliente. |
| Con Anticipos Concentrado | Las facturas y los anticipos se suman en el total por cliente; no se distingue el tipo de documento. |

<a id="politica-5-rango-de-clientes"></a>
### POLÍTICA 5. Rango de clientes

Los campos *Cliente Inicial* y *Cliente Final* permiten acotar la consulta a un subconjunto de clientes. Si ambos se dejan vacíos, el sistema consulta todos los clientes (equivale al rango 0–0, que internamente se interpreta como "sin restricción"). Si solo se indica el cliente final, este no puede ser menor que el inicial.

<a id="politica-6-fila-de-totales-del-grid"></a>
### POLÍTICA 6. Fila de totales del grid

La fila fija **TOTAL** al pie del grid suma los importes de las filas actualmente visibles (respetando cualquier filtro aplicado). Permite comparar subtotales al segmentar la vista sin necesidad de generar un PDF.

<a id="politica-7-generacion-de-pdf"></a>
### POLÍTICA 7. Generación de PDF

Los dos botones PDF envían al servidor las filas visibles en el grid en ese momento (no toda la consulta original), junto con los filtros activos del grid. Esto permite generar un PDF que refleje exactamente la vista del usuario.

- **PDF Detallado**: desglosa cada documento por cliente con subtotales y total general.
- **PDF Concentrado**: muestra solo el total agrupado por cliente con total general.

Los botones muestran un indicador de carga individual (spinner) mientras se procesa la solicitud y se deshabilitan hasta recibir respuesta.

---

## 🧪 Casos de Prueba

### 1. Consulta exitosa con parámetros por defecto

#### 💼 Operación
- [ ] 1. Ingresar al módulo **Saldos por Vencer** desde el menú CxC > Reportes.
- [ ] 2. El sistema abre automáticamente el dialog de parámetros con los valores por defecto (o los últimos guardados).
- [ ] 3. Verificar que la Fecha de Corte esté establecida en la fecha actual y los períodos en 30, 60 y 90 días.
- [ ] 4. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El dialog se cierra y el grid carga los registros de saldos.
- [ ] La fila **TOTAL** al pie del grid muestra la suma de las columnas de importes.
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** se habilitan.

---

### 2. Consulta sin resultados

#### 💼 Operación
- [ ] 1. Abrir parámetros.
- [ ] 2. Ingresar un rango de clientes que no tenga documentos vigentes (ej. número de cliente inexistente en ambos campos).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"No se Encontraron Registros con los Filtros Indicados"* (advertencia, título *"Sin Resultados"*).
- [ ] El grid permanece vacío.
- [ ] Los botones PDF permanecen deshabilitados.

---

### 3. Validación: Fecha de Corte requerida

#### 💼 Operación
- [ ] 1. Abrir parámetros.
- [ ] 2. Borrar el valor del campo **Fecha de Corte**.
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"La Fecha de Corte es Requerida"* (advertencia, título *"Datos Incompletos"*).
- [ ] El dialog permanece abierto; no se ejecuta ninguna consulta.

---

### 4. Validación: Períodos de análisis deben ser mayores a 0

#### 💼 Operación
- [ ] 1. Abrir parámetros.
- [ ] 2. Ingresar **0** en el campo **1er Período** y hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra *"El 1er Período de Análisis Debe ser Mayor a 0"* (advertencia, título *"Datos Incompletos"*).
- [ ] Repetir con **2do Período** y **3er Período** para verificar los mensajes *"El 2do Período de Análisis Debe ser Mayor a 0"* y *"El 3er Período de Análisis Debe ser Mayor a 0"* respectivamente.

---

### 5. Validación: Cliente Final menor que el Inicial

#### 💼 Operación
- [ ] 1. Abrir parámetros.
- [ ] 2. Seleccionar un **Cliente Inicial** con un número mayor al del **Cliente Final**.
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra *"El Cliente Final no Puede ser Menor que el Inicial"* (advertencia, título *"Error de Rango"*).
- [ ] El dialog permanece abierto.

---

### 6. Filtrado en el grid y actualización de totales

#### 💼 Operación
- [ ] 1. Con datos cargados en el grid, aplicar un filtro de texto en la columna **Cliente** (ej. escribir parte del nombre de un cliente).
- [ ] 2. Observar la fila **TOTAL** al pie del grid.

#### 🛡️ Validaciones
- [ ] Solo se muestran las filas que coinciden con el filtro.
- [ ] La fila **TOTAL** recalcula y muestra únicamente la suma de las filas visibles.

---

### 7. Descarga de PDF Detallado

#### 💼 Operación
- [ ] 1. Con datos cargados en el grid, hacer clic en **PDF Detallado**.
- [ ] 2. Esperar a que el botón muestre el spinner de carga.
- [ ] 3. Una vez completado, verificar que se descargue el archivo.

#### 🛡️ Validaciones
- [ ] Se descarga automáticamente un archivo PDF con nombre `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf`.
- [ ] Si el servidor no puede generar el archivo, el sistema muestra el mensaje de error retornado o *"No se pudo generar el PDF"* (advertencia, título *"Error al Generar PDF"*).

---

### 8. Descarga de PDF Concentrado

#### 💼 Operación
- [ ] 1. Con datos cargados en el grid, hacer clic en **PDF Concentrado**.
- [ ] 2. Esperar a que el botón muestre el spinner de carga.
- [ ] 3. Verificar que se descargue el archivo.

#### 🛡️ Validaciones
- [ ] Se descarga automáticamente un archivo PDF con nombre `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.
- [ ] Si el servidor no puede generar el archivo, el sistema muestra el mensaje de error retornado o *"No se pudo generar el PDF"* (advertencia, título *"Error al Generar PDF"*).

---

## 📎 Observaciones adicionales

- El análisis original indicaba usar `MatInput` numérico para los campos *Cliente Inicial* y *Cliente Final*; la implementación final usa `ClienteAutocompleteComponent` en ambos campos para una mejor experiencia de usuario.
- Los campos *Fecha Inicial* y *Fecha Final* de la consulta SQL son fijos y no se exponen al usuario: la fecha inicial se establece en 1982-02-22 y la fecha final en cien años a partir de hoy, de modo que la consulta retorna el historial completo. El único parámetro de fecha controlable por el usuario es la **Fecha de Corte**.
- Los botones PDF son ambos de color rojo (`btn-danger`); se distinguen exclusivamente por el tooltip y el texto del spinner.

---

> 🗓️ **Fecha de última modificación:** 22/06/2026
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/06/22 | IC | Primera versión |
