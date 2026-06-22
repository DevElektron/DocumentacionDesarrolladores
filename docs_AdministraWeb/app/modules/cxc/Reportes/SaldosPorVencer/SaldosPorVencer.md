# 📦 Módulo: Saldos por Vencer de CxC
#### 📁 **Código:** `src\app\modules\cxcobrar\reportes\saldos-por-vencer`
#### 💻 **Menú:** Menú > CxC > Reportes > Saldos por Vencer  [Ver en QA](http://192.168.2.16:1089/app/cxcobrar/reportes/saldos-por-vencer)

---

## 📝 Descripción

El módulo muestra los saldos por vencer de Cuentas por Cobrar, clasificados en tres períodos de días configurables por el usuario, con opción de incluir anticipos. Permite exportar el resultado en dos formatos PDF: **Detallado** (por documento) o **Concentrado** (por cliente).

La pantalla se organiza en dos zonas:

**Barra de acciones** — en la parte superior, con el título del módulo a la izquierda y tres botones a la derecha:
- **Parámetros** (icono embudo): abre el diálogo de parámetros de consulta. Disponible en todo momento.
- **PDF Detallado** (icono PDF rojo): genera y descarga el reporte con detalle de cada documento por cliente. Habilitado solo cuando hay datos en el grid.
- **PDF Concentrado** (icono PDF rojo): genera y descarga el reporte con totales acumulados por cliente. Habilitado solo cuando hay datos en el grid.

**Tabla de resultados** (AG Grid, client-side) — visible cuando se han cargado datos. Incluye las siguientes columnas:

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

Al pie del grid se muestra una **fila de totales** fija (resaltada en azul) con la suma de todos los importes de los registros actualmente visibles. Se recalcula automáticamente al aplicar o quitar filtros de columna.

Cuando no hay datos cargados, se muestra un estado vacío con el mensaje: *"Configure los parámetros y haga clic en Aceptar para cargar los saldos."*

---

## 🔐 Seguridad

El módulo no implementa permisos granulares por elemento de interfaz. El acceso se controla únicamente a nivel de ruta mediante `authGuard` (sesión activa requerida).

---

## 💼 Políticas Generales

<a id="politica-1-parametros-de-consulta"></a>
### POLÍTICA 1. Parámetros de consulta

Al entrar al módulo, el diálogo de parámetros se abre automáticamente. El usuario puede reabrirlo en cualquier momento con el botón de filtro.

Los parámetros disponibles son:

| Parámetro | Control | Default | Descripción |
|-----------|---------|---------|-------------|
| Cliente inicial | Número | 1 | Número de cliente inicial del rango a consultar |
| Cliente final | Número | 99999 | Número de cliente final del rango a consultar |
| Fecha inicial | Fecha | Hoy −30 días | Fecha de inicio del período de emisión de documentos |
| Fecha final | Fecha | Hoy | Fecha de fin del período de emisión de documentos |
| Fecha de corte | Fecha | Hoy | Fecha de referencia para calcular saldos vencidos y por vencer |
| Límite día 1 | Número | 0 | Límite superior del primer período de análisis (en días) |
| Límite día 2 | Número | 0 | Límite superior del segundo período de análisis (en días) |
| Límite día 3 | Número | 0 | Límite superior del tercer período de análisis (en días) |
| Anticipos | Selección | Sin Anticipos | Cómo tratar los anticipos en el reporte ([ver Política 3](#politica-3-tratamiento-de-anticipos)) |
| Zona de cobranza | Autocomplete | Todas | Filtra por zona; seleccionar "Todas" incluye todas las zonas |
| Vendedor | Autocomplete | Todos | Filtra por vendedor; seleccionar "Todos" incluye todos |
| Tipo de cliente | Selección | Todos | Filtra por tipo: Todos / Crédito / Contado |

Los parámetros se conservan en `localStorage` y se pre-cargan en la siguiente apertura del módulo.

Las tres fechas son obligatorias. El Cliente Inicial debe ser ≥ 1. El Cliente Final no puede ser menor que el Cliente Inicial. Si alguna condición no se cumple, el sistema muestra un mensaje de validación y no ejecuta la consulta.

<a id="politica-2-periodos-de-analisis"></a>
### POLÍTICA 2. Períodos de análisis de días

Los parámetros **Límite día 1**, **Límite día 2** y **Límite día 3** definen los rangos de días para clasificar los saldos por vencer a partir de la fecha de corte:

- **Día 1**: documentos con días restantes dentro del primer límite configurado
- **Día 2**: documentos con días restantes entre el primer y el segundo límite
- **Día 3**: documentos con días restantes entre el segundo y el tercer límite
- **Día+**: documentos con días restantes superiores al tercer límite

Los encabezados de columna en el PDF reflejan el valor del límite configurado (ej. "30 días", "60 días", "90 días").

<a id="politica-3-tratamiento-de-anticipos"></a>
### POLÍTICA 3. Tratamiento de anticipos

El parámetro **Anticipos** controla cómo se presentan los documentos de tipo `A` (Anticipo):

| Opción | Comportamiento en grid | PDF Detallado | PDF Concentrado |
|--------|----------------------|---------------|-----------------|
| Sin Anticipos | No incluye anticipos | No incluye anticipos | No incluye anticipos |
| Con Anticipos Detallado | Lista filas con T.Doc = `A` | Lista anticipos individualmente bajo las facturas de cada cliente | Agrupa facturas por cliente; lista anticipos individuales después del subtotal |
| Con Anticipos Concentrado | Incluye anticipos en los totales | Suma anticipos dentro del subtotal del cliente | Suma anticipos con facturas en el total del cliente |

<a id="politica-4-pdf-detallado"></a>
### POLÍTICA 4. Exportación a PDF Detallado

El PDF Detallado se genera con **los datos actualmente visibles en el grid** (respetando los filtros de columna aplicados). No re-consulta al servidor.

El PDF agrupa los registros por cliente e imprime cada documento en una fila, con un subtotal por cliente y un total general al final.

El encabezado del PDF muestra: nombre de la empresa, tipo de reporte, período (Fc.Ini – Fc.Fin), fecha de corte, y zona/vendedor cuando se filtraron valores específicos.

El nombre del archivo descargado sigue el patrón: `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf`.

<a id="politica-5-pdf-concentrado"></a>
### POLÍTICA 5. Exportación a PDF Concentrado

El PDF Concentrado se genera también con **los datos actualmente visibles en el grid**.

En lugar de listar documentos individuales, muestra un renglón por cliente con los totales acumulados de sus saldos. El tratamiento de anticipos depende del parámetro seleccionado ([ver Política 3](#politica-3-tratamiento-de-anticipos)).

El nombre del archivo descargado sigue el patrón: `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.

---

## 🧪 Casos de Prueba

### 1. Carga exitosa del reporte

#### 💼 Operación
- [ ] 1. Navegar a Menú > CxC > Reportes > Saldos por Vencer.
- [ ] 2. El diálogo de parámetros se abre automáticamente.
- [ ] 3. Configurar parámetros válidos con un rango de fechas que contenga saldos existentes y hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El loader muestra el mensaje *"Consultando saldos por vencer..."* durante la consulta.
- [ ] El grid se puebla con los registros correspondientes.
- [ ] La fila de totales al pie del grid muestra la suma acumulada de todos los importes.
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** quedan habilitados.

---

### 2. Sin resultados con los parámetros indicados

#### 💼 Operación
- [ ] 1. Abrir parámetros (botón filtro o apertura automática).
- [ ] 2. Configurar un rango de clientes o fechas sin actividad (ej. clientes 99990–99999).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"No se Encontraron Registros con los Filtros Indicados"* con título *"Sin Resultados"*.
- [ ] El grid permanece vacío.
- [ ] Los botones PDF permanecen deshabilitados.

---

### 3. Validación de rango de clientes inválido

#### 💼 Operación
- [ ] 1. Abrir el diálogo de parámetros.
- [ ] 2. Ingresar un **Cliente Final** menor que el **Cliente Inicial** (ej. Inicial = 500, Final = 100).
- [ ] 3. Intentar hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"El Cliente Final no puede ser menor que el Inicial"* y no ejecuta la consulta.

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
- [ ] El PDF muestra un renglón por cliente (sin detalle por documento) con sus totales acumulados y un total general.

---

### 6. Filtro de columna actualiza la fila de totales

#### 💼 Operación
- [ ] 1. Cargar el reporte con datos ([ver Caso 1](#1-carga-exitosa-del-reporte)).
- [ ] 2. Aplicar un filtro en cualquier columna del grid (ej. filtrar por un cliente específico en la columna **Cliente**).

#### 🛡️ Validaciones
- [ ] El grid muestra únicamente los registros que coinciden con el filtro.
- [ ] La fila de totales al pie se actualiza automáticamente con la suma de los registros visibles.
- [ ] Al quitar el filtro, los totales regresan al acumulado general.

---

## 📎 Observaciones adicionales

- Los componentes `ZonasCobranzaDisponiblesAutocompleteComponent` y `VendedorAutocompleteComponent` se usan en el diálogo de parámetros. Son componentes compartidos existentes reutilizados sin modificaciones internas.
- Los campos de rango de cliente (`CteIni` / `CteFin`) son entradas numéricas directas; no utilizan autocompletado por nombre.
- Ambos botones PDF tienen el mismo estilo visual (`btn-danger` rojo). El análisis original propuso `btn-secondary` o `btn-warning` para el Concentrado; la implementación final usa `btn-danger` en ambos. Diferencia cosmética aceptada.

---

> 🗓️ **Fecha de última modificación:** 22/06/2026
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/06/22 | ST | Primera versión |
