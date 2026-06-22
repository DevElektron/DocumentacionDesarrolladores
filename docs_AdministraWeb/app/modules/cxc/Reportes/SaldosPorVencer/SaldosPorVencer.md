# 📦 Módulo: Saldos por Vencer
#### 📁 **Código:** `src\app\modules\cxcobrar\reportes\saldos-por-vencer`
#### 💻 **Menú:** Menú > CxC > Reportes > Saldos por Vencer  [Ver en QA](http://192.168.2.16:1089/app/cxcobrar/reportes/saldos-por-vencer)

---

## 📝 Descripción

La pantalla se organiza en dos zonas:

**Barra de acciones** — encabezado superior con el título del módulo a la izquierda y tres botones a la derecha:
- **Parámetros** (icono filtro, azul): abre el diálogo de configuración de la consulta. Disponible en todo momento.
- **PDF Detallado** (icono PDF rojo): genera y descarga el reporte con detalle de cada documento por cliente. Habilitado solo cuando hay datos en el grid.
- **PDF Concentrado** (icono PDF rojo): genera y descarga el reporte con totales acumulados por cliente. Habilitado solo cuando hay datos en el grid.

**Tabla de resultados** (AG Grid) — visible cuando se han cargado datos. Incluye las siguientes columnas:

| Columna | Descripción |
|---------|-------------|
| N.Cte | Número de cliente |
| Cliente | Nombre del cliente |
| T.Doc | Tipo de documento: `F` = Factura, `A` = Anticipo |
| Folio | Folio del documento |
| Fc.Doc | Fecha del documento (dd/MM/yyyy) |
| Fc.Venc | Fecha de vencimiento (dd/MM/yyyy) |
| Imp.Vencido | Importe ya vencido a la fecha de corte |
| Día 1 | Importe dentro del primer período de días configurado |
| Día 2 | Importe dentro del segundo período de días configurado |
| Día 3 | Importe dentro del tercer período de días configurado |
| Día+ | Importe más allá del tercer período |

Al pie del grid se muestra una **fila de totales** fija (resaltada en azul) con la suma de los importes de los registros actualmente visibles. Se recalcula automáticamente al aplicar o quitar filtros de columna.

Cuando no hay datos cargados, se muestra un estado vacío con el mensaje: *"Configure los parámetros y haga clic en Aceptar para cargar los saldos."*

---

## 🔐 Seguridad

El módulo no implementa permisos granulares por elemento de interfaz. El acceso se controla únicamente a nivel de ruta mediante `authGuard` (sesión activa requerida).

---

## 💼 Políticas Generales

<a id="politica-1-parametros-de-consulta"></a>
### POLÍTICA 1. Parámetros de consulta

Al entrar al módulo, el diálogo de parámetros se abre automáticamente. El usuario puede reabrirlo en cualquier momento con el botón de filtro.

| Parámetro | Default | Descripción |
|-----------|---------|-------------|
| Cliente inicial | 1 | Número de cliente inicial del rango a consultar |
| Cliente final | 99999 | Número de cliente final del rango a consultar |
| Fecha inicial | Hoy −30 días | Fecha de inicio del período de emisión de documentos |
| Fecha final | Hoy | Fecha de fin del período de emisión de documentos |
| Fecha de corte | Hoy | Fecha de referencia para calcular saldos vencidos y por vencer |
| Límite día 1 | 0 | Límite superior del primer período de análisis (días) |
| Límite día 2 | 0 | Límite superior del segundo período de análisis (días) |
| Límite día 3 | 0 | Límite superior del tercer período de análisis (días) |
| Anticipos | Sin Anticipos | Cómo tratar los anticipos en el reporte ([ver Política 3](#politica-3-tratamiento-de-anticipos)) |
| Zona de cobranza | Todas | Filtra por zona; "Todas" incluye todas las zonas |
| Vendedor | Todos | Filtra por vendedor; "Todos" incluye todos |
| Tipo de cliente | Todos | Filtra por tipo: Todos / Crédito / Contado |

Los parámetros se conservan automáticamente en el navegador al confirmar y se pre-cargan en la siguiente apertura del módulo.

**Validaciones:** las tres fechas son obligatorias; el Cliente Inicial debe ser ≥ 1; el Cliente Final no puede ser menor que el Inicial. Si alguna condición no se cumple, el sistema muestra un mensaje y no ejecuta la consulta.

<a id="politica-2-periodos-de-analisis"></a>
### POLÍTICA 2. Períodos de análisis de días

Los parámetros **Límite día 1**, **Límite día 2** y **Límite día 3** definen los rangos de días para clasificar los saldos por vencer a partir de la fecha de corte:

- **Día 1**: documentos con días restantes dentro del primer límite.
- **Día 2**: documentos con días restantes entre el primer y el segundo límite.
- **Día 3**: documentos con días restantes entre el segundo y el tercer límite.
- **Día+**: documentos con días restantes superiores al tercer límite.

Los encabezados de columna en el PDF reflejan el valor del límite configurado (ej. "30 días", "60 días", "90 días").

<a id="politica-3-tratamiento-de-anticipos"></a>
### POLÍTICA 3. Tratamiento de anticipos

El parámetro **Anticipos** controla cómo se presentan los documentos de tipo `A` (Anticipo):

| Opción | PDF Detallado | PDF Concentrado |
|--------|---------------|-----------------|
| Sin Anticipos | No incluye anticipos | No incluye anticipos |
| Con Anticipos Detallado | Lista anticipos individualmente bajo las facturas de cada cliente | Agrupa facturas por cliente; lista anticipos individuales después del subtotal |
| Con Anticipos Concentrado | Suma anticipos dentro del subtotal del cliente | Suma anticipos con facturas en el total del cliente |

<a id="politica-4-pdf-detallado"></a>
### POLÍTICA 4. Exportación a PDF Detallado

El PDF Detallado se genera con los **datos actualmente visibles en el grid** (respetando los filtros de columna activos). No re-consulta al servidor.

Agrupa los registros por cliente e imprime cada documento en una fila, con subtotal por cliente y total general al final. El encabezado del PDF incluye: nombre de empresa, tipo de reporte, período (Fc.Ini – Fc.Fin), fecha de corte, y zona/vendedor cuando se filtraron valores específicos.

Nombre del archivo descargado: `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf`.

<a id="politica-5-pdf-concentrado"></a>
### POLÍTICA 5. Exportación a PDF Concentrado

El PDF Concentrado se genera también con los **datos actualmente visibles en el grid**.

Muestra un renglón por cliente con los totales acumulados de sus saldos, sin detalle de documentos individuales. El tratamiento de anticipos depende del parámetro seleccionado ([ver Política 3](#politica-3-tratamiento-de-anticipos)).

Nombre del archivo descargado: `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.

---

## 🧪 Casos de Prueba

### 1. Carga exitosa del reporte

#### 💼 Operación
- [ ] 1. Navegar a Menú > CxC > Reportes > Saldos por Vencer.
- [ ] 2. El diálogo de parámetros se abre automáticamente.
- [ ] 3. Configurar parámetros válidos con un rango de fechas que contenga saldos y hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El loader muestra el mensaje *"Consultando saldos por vencer..."* durante la consulta.
- [ ] El grid se llena con los registros correspondientes.
- [ ] La fila **TOTAL** al pie del grid muestra la suma acumulada de todos los importes.
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** quedan habilitados.

---

### 2. Sin resultados con los parámetros indicados

#### 💼 Operación
- [ ] 1. Abrir el diálogo de parámetros.
- [ ] 2. Configurar un rango de clientes o fechas sin actividad (ej. clientes 99990–99999).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"No se Encontraron Registros con los Filtros Indicados"* (título: *"Sin Resultados"*).
- [ ] El grid permanece vacío.
- [ ] Los botones PDF permanecen deshabilitados.

---

### 3. Validación de rango de clientes inválido

#### 💼 Operación
- [ ] 1. Abrir el diálogo de parámetros.
- [ ] 2. Ingresar un **Cliente Final** menor que el **Cliente Inicial** (ej. Inicial = 500, Final = 100).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"El Cliente Final no puede ser menor que el Inicial"* y no ejecuta la consulta ([ver Política 1](#politica-1-parametros-de-consulta)).

---

### 4. Descarga de PDF Detallado

#### 💼 Operación
- [ ] 1. Cargar el reporte con datos ([ver Caso 1](#1-carga-exitosa-del-reporte)).
- [ ] 2. Hacer clic en el botón **PDF Detallado**.

#### 🛡️ Validaciones
- [ ] El botón muestra un spinner mientras se genera el PDF.
- [ ] Se descarga automáticamente el archivo `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf`.
- [ ] El PDF agrupa los documentos por cliente con subtotal por cliente y total general al final.
- [ ] Si se aplicaron filtros de columna en el grid, el PDF incluye únicamente los registros visibles.

---

### 5. Descarga de PDF Concentrado

#### 💼 Operación
- [ ] 1. Cargar el reporte con datos ([ver Caso 1](#1-carga-exitosa-del-reporte)).
- [ ] 2. Hacer clic en el botón **PDF Concentrado**.

#### 🛡️ Validaciones
- [ ] El botón muestra un spinner mientras se genera el PDF.
- [ ] Se descarga automáticamente el archivo `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.
- [ ] El PDF muestra un renglón por cliente con sus totales acumulados (sin detalle por documento) y un total general.

---

### 6. Filtro de columna actualiza la fila de totales

#### 💼 Operación
- [ ] 1. Cargar el reporte con datos ([ver Caso 1](#1-carga-exitosa-del-reporte)).
- [ ] 2. Aplicar un filtro en cualquier columna del grid (ej. filtrar por un cliente específico en la columna **Cliente**).

#### 🛡️ Validaciones
- [ ] El grid muestra únicamente los registros que coinciden con el filtro.
- [ ] La fila **TOTAL** al pie se recalcula automáticamente con la suma de los registros visibles.
- [ ] Al quitar el filtro, los totales regresan al acumulado general.

---

### 7. Botones PDF deshabilitados sin datos

#### 💼 Operación
- [ ] 1. Ingresar al módulo sin haber consultado aún (o tras una consulta sin resultados).

#### 🛡️ Validaciones
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** aparecen deshabilitados y no responden al clic.

---

## 📎 Observaciones adicionales

- Los componentes `ZonasCobranzaDisponiblesAutocompleteComponent` y `VendedorAutocompleteComponent` se usan en el diálogo de parámetros sin modificaciones. Cambios a componentes compartidos: **Ninguno**.
- Los campos de rango de cliente (`CteIni` / `CteFin`) son entradas numéricas directas; no utilizan autocompletado por nombre.
- Ambos botones PDF usan el estilo `btn-danger` (rojo). El análisis original propuso `btn-secondary` o `btn-warning` para el PDF Concentrado; la implementación final usa `btn-danger` en ambos. Diferencia cosmética aceptada.

---

> 🗓️ **Fecha de última modificación:** 22/06/2026
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/06/22 | IC | Primera versión |
