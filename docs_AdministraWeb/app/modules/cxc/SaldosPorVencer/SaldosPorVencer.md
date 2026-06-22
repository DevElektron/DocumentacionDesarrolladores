# 📦 Módulo: Saldos por Vencer de CxC
#### 📁 **Código:** `src\app\modules\cxcobrar\reportes\saldos-por-vencer`
#### 💻 **Menú:** Menú > CxC > Reportes > Saldos por Vencer  [Ver en QA](http://192.168.2.16:1089/app/cxcobrar/reportes/saldos-por-vencer)

---

## 📝 Descripción

El módulo presenta los saldos vencidos y por vencer de los clientes de Cuentas por Cobrar, organizados en tres períodos de días configurables. Permite generar reportes en PDF en dos modalidades: Detallado (por documento) y Concentrado (totales por cliente).

**Barra de acciones**
- **Parámetros** (ícono de filtro, azul): abre el diálogo de configuración de parámetros. Siempre disponible.
- **PDF Detallado** (ícono PDF, rojo): genera y descarga un PDF con el detalle de documentos por cliente. Habilitado únicamente cuando el grid contiene datos.
- **PDF Concentrado** (ícono PDF, rojo): genera y descarga un PDF con los totales por cliente. Habilitado únicamente cuando el grid contiene datos.
- Mientras se genera un PDF, el botón correspondiente muestra un indicador de carga (spinner); el otro botón permanece operable de forma independiente.

**Estado vacío**
- Mientras no se ejecute una consulta, la sección del grid muestra el mensaje: *"Configure los parámetros y haga clic en Aceptar para cargar los saldos."*

**Grid de resultados**
- Visible únicamente cuando la consulta retorna datos.
- Columnas: **N.Cte**, **Cliente**, **T.Doc** (F=Factura / A=Anticipo), **Folio**, **Fc.Doc**, **Fc.Venc**, **Imp.Vencido**, **Día 1**, **Día 2**, **Día 3**, **Día+**.
- Todas las columnas cuentan con filtros flotantes independientes.
- Los anchos de columna se persisten en el navegador (localStorage).
- **Fila de totales al pie** (fija): muestra la suma de los importes de los registros visibles tras aplicar los filtros del grid. Se actualiza automáticamente al cambiar los filtros.

---

## 🔐 Seguridad

El módulo no expone acciones con permisos granulares en la interfaz. El acceso está controlado a nivel de ruta mediante `authGuard`.

| Tipo UI | Elemento | Descripción | Rol permitido |
|---------|----------|-------------|---------------|
| Ruta | Saldos por Vencer | Acceso al módulo completo | |

---

## 💼 Políticas Generales

<a id="politica-1-parametros-de-consulta"></a>
### POLÍTICA 1. Parámetros de Consulta

Al ingresar al módulo, el diálogo de parámetros se abre automáticamente. La consulta no se ejecuta hasta que el usuario confirme los parámetros.

| Parámetro | Descripción | Default |
|-----------|-------------|---------|
| Cliente Inicial / Final | Rango de número de cliente a incluir | 1 / 99999 |
| Fecha Inicial | Fecha mínima del documento | Hoy − 30 días |
| Fecha Final | Fecha máxima del documento | Hoy |
| Fecha de Corte | Fecha de referencia para calcular vencimientos | Hoy |
| Día 1 / Día 2 / Día 3 | Límites de los tres períodos de análisis (en días) | 0 / 0 / 0 |
| Anticipos | Tratamiento de documentos tipo anticipo ([ver Política 3](#politica-3-tratamiento-de-anticipos)) | Sin Anticipos |
| Zona de Cobranza | Filtra por zona; 0 = todas | 0 |
| Vendedor | Filtra por vendedor; 0 = todos | 0 |
| Tipo de Cliente | 0=Todos / 1=Crédito / 2=Contado | Todos |

Los parámetros se guardan en el navegador y se recuperan automáticamente la próxima vez que se abre el diálogo.

Validaciones del sistema:

| Condición | Mensaje |
|-----------|---------|
| Cliente Inicial < 1 | *"El Cliente Inicial debe ser ≥ 1"* |
| Cliente Final < Cliente Inicial | *"El Cliente Final no puede ser menor que el Inicial"* |
| Fecha Inicial no especificada | *"La Fecha Inicial es requerida"* |
| Fecha Final no especificada | *"La Fecha Final es requerida"* |
| Fecha de Corte no especificada | *"La Fecha de Corte es requerida"* |

<a id="politica-2-tipos-de-reporte-pdf"></a>
### POLÍTICA 2. Tipos de Reporte PDF

El módulo ofrece dos formatos de PDF independientes, ambos generados con los datos actualmente visibles en el grid (respetando los filtros aplicados):

| Tipo | Descripción |
|------|-------------|
| **Detallado** | Lista cada documento por cliente: folio, fecha de emisión, fecha de vencimiento e importes por período |
| **Concentrado** | Muestra únicamente los totales acumulados por cliente, sin detalle de documentos |

El nombre del archivo generado sigue el formato: `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf` o `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.

<a id="politica-3-tratamiento-de-anticipos"></a>
### POLÍTICA 3. Tratamiento de Anticipos

Los anticipos son documentos de tipo "A". Su inclusión en el reporte se controla con el parámetro **Anticipos**:

| Opción | Comportamiento en el grid | Comportamiento en el PDF |
|--------|--------------------------|--------------------------|
| Sin Anticipos | La consulta excluye anticipos | PDF no incluye anticipos |
| Con Anticipos Detallado | Incluye filas de anticipo en el grid | PDF lista los anticipos de forma individual por cliente |
| Con Anticipos Concentrado | Incluye filas de anticipo en el grid | PDF suma los anticipos junto al total del cliente |

<a id="politica-4-filtrado-del-grid-y-generacion-de-pdf"></a>
### POLÍTICA 4. Filtrado del Grid y Generación de PDF

Los filtros flotantes del grid permiten acotar los registros visibles sin volver a consultar el servidor. La fila de totales al pie refleja siempre los importes de las filas visibles.

Al generar un PDF, el sistema envía al servidor únicamente los registros visibles en ese momento (después de aplicar los filtros del grid), no el total de registros de la consulta original. Esto garantiza que el PDF es consistente con lo que el usuario ve en pantalla.

---

## 🧪 Casos de Prueba

### 1. Consulta exitosa con resultados

#### 💼 Operación
- [ ] 1. Navegar a Menú > CxC > Reportes > Saldos por Vencer.
- [ ] 2. En el diálogo de parámetros, verificar que los valores predeterminados son correctos (rango de clientes 1-99999, fechas del período actual, anticipos = Sin Anticipos).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El diálogo se cierra y aparece un indicador de carga con el texto *"Consultando saldos por vencer..."*.
- [ ] El grid muestra las filas de saldos con sus respectivos períodos.
- [ ] La fila de totales al pie muestra la suma de los importes visibles.
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** quedan habilitados.

---

### 2. Consulta sin resultados

#### 💼 Operación
- [ ] 1. Abrir parámetros con el botón de filtros (ícono de embudo).
- [ ] 2. Establecer un rango de clientes que no tenga saldos (por ejemplo, cliente 99998 – 99999).
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"No se Encontraron Registros con los Filtros Indicados"* con título *"Sin Resultados"*.
- [ ] El grid permanece vacío y los botones de PDF quedan deshabilitados.

---

### 3. Validación: Cliente Final menor al Inicial

#### 💼 Operación
- [ ] 1. Abrir parámetros.
- [ ] 2. Establecer **Cliente Inicial** en 500 y **Cliente Final** en 100.
- [ ] 3. Hacer clic en **Aceptar**.

#### 🛡️ Validaciones
- [ ] El sistema muestra el mensaje *"El Cliente Final no puede ser menor que el Inicial"* [ver Política 1](#politica-1-parametros-de-consulta).
- [ ] El diálogo permanece abierto; no se realiza ninguna consulta al servidor.

---

### 4. Descarga de PDF Detallado

#### 💼 Operación
- [ ] 1. Ejecutar una consulta que retorne resultados.
- [ ] 2. (Opcional) Aplicar filtros al grid para limitar los registros visibles.
- [ ] 3. Hacer clic en **PDF Detallado**.

#### 🛡️ Validaciones
- [ ] El botón **PDF Detallado** muestra el spinner mientras se procesa; el botón **PDF Concentrado** permanece operable.
- [ ] Al terminar, el navegador descarga el archivo `SaldosPorVencer_Detallado_YYYY-MM-DD.pdf`.
- [ ] El PDF contiene únicamente los registros visibles en el grid en el momento de la descarga [ver Política 4](#politica-4-filtrado-del-grid-y-generacion-de-pdf).
- [ ] El PDF muestra el detalle de documentos por cliente con los períodos de análisis configurados.

---

### 5. Descarga de PDF Concentrado

#### 💼 Operación
- [ ] 1. Con el grid cargado, hacer clic en **PDF Concentrado**.

#### 🛡️ Validaciones
- [ ] El botón **PDF Concentrado** muestra el spinner de forma independiente al botón PDF Detallado.
- [ ] El navegador descarga el archivo `SaldosPorVencer_Concentrado_YYYY-MM-DD.pdf`.
- [ ] El PDF muestra únicamente los totales por cliente, sin detalle de documentos individuales [ver Política 2](#politica-2-tipos-de-reporte-pdf).

---

### 6. Intento de PDF sin datos en pantalla

#### 💼 Operación
- [ ] 1. Sin haber ejecutado una consulta (o con el grid vacío), observar el estado de los botones PDF.

#### 🛡️ Validaciones
- [ ] Los botones **PDF Detallado** y **PDF Concentrado** permanecen deshabilitados mientras el grid no contenga datos.

---

## 📎 Observaciones adicionales

- **Componentes compartidos reutilizados (sin modificación):** `VendedorAutocompleteComponent` y `ZonasCobranzaDisponiblesAutocompleteComponent` se utilizan en el diálogo de parámetros tal como están definidos.
- Los errores técnicos (caída del servicio, error de red) se presentan mediante el diálogo de error del sistema y no constituyen flujos que el usuario pueda provocar de forma normal.

---

> 🗓️ **Fecha de última modificación:** 22/06/2026
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/06/22 | IC | Primera versión |
