# 📦 Módulo: Listado de Documentos y sus Afectaciones
#### 📁 **Código:** `src\app\modules\cxcobrar\reportes\listado-doctos-afectaciones`
#### 💻 **Menú:** Menú > CxC > Reportes > Listado de Documentos y sus Afectaciones  [Ver en QA](http://192.168.2.16:1089/app/cxcobrar/reportes/listado-doctos-afectaciones)

## 📝 Descripción

Este módulo genera un **reporte PDF** que lista los documentos de Cuentas por Cobrar de un período, agrupados por cliente, mostrando debajo de cada documento los registros que lo afectaron (pagos aplicados, notas de crédito, anticipos, etc.).

Al acceder al módulo, el sistema abre automáticamente el diálogo de **Parámetros del Reporte**. La pantalla se divide en dos zonas:

1. **Barra de acciones**, en el encabezado del módulo, con dos botones:
   - _Parámetros del Reporte_ (ícono de filtro azul): Abre el diálogo para configurar los parámetros.
   - _Salir_ (ícono de X rojo): Sale del módulo con confirmación previa.

2. **Estado vacío**, que se muestra mientras no se ha generado ningún reporte. Indica al usuario que debe configurar los parámetros para generar el reporte.

---

### Diálogo de Parámetros del Reporte

Al abrir los parámetros, se presenta un diálogo con las siguientes zonas:

1. **Rango de Clientes** — Dos buscadores (_Cliente Inicial_ y _Cliente Final_), obligatorios, seleccionables desde la lista.

2. **Período** — Dos selectores de fecha (_Fecha Inicial_ y _Fecha Final_), obligatorios. La fecha únicamente puede seleccionarse con el ícono de calendario; no se permite escritura manual.

3. **Filtros Opcionales** — Dos buscadores opcionales:
   - _Tipo de Documento_: filtra el reporte a un tipo específico de documento CxC.
   - _Concepto_: filtra por concepto de CxC.

4. **Botones de acción**:
   - _Limpiar_: Restablece todos los campos del formulario a su estado inicial.
   - _Generar_: Genera y descarga el reporte PDF. Se habilita solo cuando el formulario es válido.
   - _Cancelar_: Cierra el diálogo sin generar el reporte.

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
|---------|----------|-------------|---------------|
| ventana | Listado de Documentos y sus Afectaciones | Permite acceder al módulo desde el menú CxC > Reportes | |

## 💼 Políticas Generales

<a id="politica-1-filtros-obligatorios"></a>

### POLÍTICA 1. Los campos de rango de clientes y período son obligatorios

Para generar el reporte es necesario definir un rango de clientes válido y un período de fechas. El botón _Generar_ permanece deshabilitado mientras el formulario no sea válido. Al intentar generar sin completar los campos, el sistema marca los campos pendientes con indicadores de error.

<a id="politica-2-validaciones-de-rango"></a>

### POLÍTICA 2. Validaciones de rango de clientes y fechas

Antes de generar el reporte, el sistema verifica:

| Condición | Mensaje de error | Título |
|-----------|-----------------|--------|
| No. de Cliente Inicial ≤ 0 | _El No. de Cliente Inicial debe ser mayor a 0_ | Datos Incompletos |
| No. de Cliente Final < Cliente Inicial | _El No. del Cliente Final no puede ser menor que el Inicial_ | Error de Rango |
| Fecha Final < Fecha Inicial | _La Fecha Final no puede ser menor que la Fecha Inicial_ | Error de Rango |

<a id="politica-3-seleccion-fecha-solo-calendario"></a>

### POLÍTICA 3. Las fechas solo pueden seleccionarse con el ícono de calendario

Los campos de fecha no permiten escritura manual ni pegado de texto. La selección es exclusivamente mediante el selector de calendario. Los campos muestran un tooltip indicando esta restricción al recibir el foco.

<a id="politica-4-filtros-opcionales"></a>

### POLÍTICA 4. Los filtros opcionales acotan el reporte por tipo de documento y concepto

Los campos _Tipo de Documento_ y _Concepto_ son opcionales. Si se dejan vacíos, el reporte incluye todos los documentos y conceptos del período. Si se completan, deben seleccionarse desde el buscador; escribir texto sin seleccionar un elemento deja el campo en estado inválido e impide generar el reporte.

<a id="politica-5-limpiar-formulario"></a>

### POLÍTICA 5. El botón Limpiar restablece todos los campos a su estado inicial

El botón _Limpiar_ reinicia el formulario completo, incluyendo los campos opcionales. Es la única forma de resolver el estado inválido de _Tipo de Documento_ y _Concepto_ cuando el usuario escribe texto sin seleccionar un elemento de la lista (ya que no cuentan con opción en blanco para limpiar directamente).

<a id="politica-6-descarga-automatica-pdf"></a>

### POLÍTICA 6. El reporte se descarga automáticamente como PDF

Al confirmar los parámetros, el sistema genera el reporte en segundo plano (mostrando un indicador _Generando reporte…_) y descarga automáticamente el archivo PDF. El nombre del archivo incluye la fecha y hora de generación. Una vez descargado, el formulario se limpia automáticamente.

## 🧪 Casos de Prueba

### 1. Acceso sin permiso al módulo

#### 💼 Operación

- [ ] 1. Inicia sesión con un usuario que **no** tenga el permiso del módulo.
- [ ] 2. Verifica que la opción no aparece en el menú _CxC > Reportes_.
- [ ] 3. Intenta acceder directamente por URL: `http://192.168.2.16:1089/app/cxcobrar/reportes/listado-doctos-afectaciones`.

#### 🛡️ Validaciones

- [ ] La opción no aparece en el menú lateral.
- [ ] El acceso directo por URL es rechazado.

---

### 2. Carga inicial del módulo

#### 💼 Operación

- [ ] 1. Navega a _Menú > CxC > Reportes > Listado de Documentos y sus Afectaciones_.
- [ ] 2. El módulo carga y abre automáticamente el diálogo de parámetros.

#### 🛡️ Validaciones

- [ ] El diálogo de parámetros se abre automáticamente al ingresar.
- [ ] El formulario inicia con todos los campos vacíos.
- [ ] El botón _Generar_ está deshabilitado.

---

### 3. Generar reporte — flujo feliz

#### 💼 Operación

- [ ] 1. Busca y selecciona un _Cliente Inicial_ y _Cliente Final_ válidos.
- [ ] 2. Selecciona _Fecha Inicial_ y _Fecha Final_ con el ícono de calendario.
- [ ] 3. Deja los filtros opcionales vacíos.
- [ ] 4. Da clic en _Generar_.
- [ ] 5. El sistema muestra el indicador _Generando reporte…_ mientras procesa.
- [ ] 6. El PDF se descarga automáticamente.
- [ ] 7. El formulario se limpia tras la descarga exitosa.

#### 🛡️ Validaciones

- [ ] El indicador de carga aparece durante la generación.
- [ ] El archivo PDF se descarga con nombre que incluye fecha y hora.
- [ ] El formulario se limpia automáticamente tras la descarga.

---

### 4. Intentar generar sin campos obligatorios

#### 💼 Operación

- [ ] 1. Sin completar ningún campo, da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El formulario marca los campos obligatorios con error.
- [ ] No se genera el reporte.

---

### 5. Validación: Cliente Inicial con número ≤ 0

#### 💼 Operación

- [ ] 1. Selecciona un _Cliente Inicial_ con número 0 o negativo.
- [ ] 2. Completa los demás campos obligatorios y da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El sistema muestra: _El No. de Cliente Inicial debe ser mayor a 0_ (título: _Datos Incompletos_).
- [ ] No se genera el reporte.

---

### 6. Validación: Cliente Final menor que el Inicial

#### 💼 Operación

- [ ] 1. Selecciona _Cliente Inicial_ con número mayor al del _Cliente Final_ (ej. Inicial = 100, Final = 50).
- [ ] 2. Completa las fechas y da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El sistema muestra: _El No. del Cliente Final no puede ser menor que el Inicial_ (título: _Error de Rango_).
- [ ] No se genera el reporte.

---

### 7. Validación: Fecha Final menor que la Inicial

#### 💼 Operación

- [ ] 1. Selecciona una _Fecha Final_ anterior a la _Fecha Inicial_.
- [ ] 2. Completa los campos de cliente y da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El sistema muestra: _La Fecha Final no puede ser menor que la Fecha Inicial_ (título: _Error de Rango_).
- [ ] No se genera el reporte.

---

### 8. Intentar escribir fecha manualmente

#### 💼 Operación

- [ ] 1. Haz clic directamente sobre el campo _Fecha Inicial_ e intenta escribir con el teclado.
- [ ] 2. Intenta pegar una fecha con Ctrl+V.

#### 🛡️ Validaciones

- [ ] El campo no acepta escritura manual ni pegado.
- [ ] El tooltip muestra: _Selecciona la fecha con el botón de calendario_.

---

### 9. Generar reporte con filtros opcionales

#### 💼 Operación

- [ ] 1. Completa los campos obligatorios.
- [ ] 2. En _Tipo de Documento_, busca y selecciona un tipo válido.
- [ ] 3. En _Concepto_, busca y selecciona un concepto válido.
- [ ] 4. Da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El reporte se genera y descarga correctamente.
- [ ] El PDF contiene únicamente documentos del tipo y concepto seleccionados.

---

### 10. Campos opcionales atascados — usar Limpiar

#### 💼 Operación

- [ ] 1. En _Tipo de Documento_, escribe texto libre sin seleccionar ningún resultado.
- [ ] 2. Verifica que el campo queda en estado inválido y el botón _Generar_ se deshabilita.
- [ ] 3. Da clic en _Limpiar_.

#### 🛡️ Validaciones

- [ ] El campo muestra error de validación.
- [ ] El botón _Generar_ queda deshabilitado mientras el campo está inválido.
- [ ] Al dar clic en _Limpiar_, todos los campos se resetean y el formulario vuelve a su estado inicial.

---

### 11. Cancelar el diálogo de parámetros

#### 💼 Operación

- [ ] 1. Abre el diálogo, completa algunos campos.
- [ ] 2. Da clic en _Cancelar_ o en la _X_ del encabezado del diálogo.

#### 🛡️ Validaciones

- [ ] El diálogo se cierra sin generar el reporte.
- [ ] La pantalla principal permanece en estado vacío.

---

### 12. Salir del módulo

#### 💼 Operación

- [ ] 1. Cierra el diálogo de parámetros.
- [ ] 2. Da clic en el botón rojo _Salir_ en la barra de acciones.
- [ ] 3. El sistema muestra: _¿Deseas salir del Listado de Documentos y sus Afectaciones?_
- [ ] 4. Da clic en _No_ para verificar que no cierra; luego repite y confirma con _Sí_.

#### 🛡️ Validaciones

- [ ] El diálogo de confirmación se muestra con el mensaje exacto.
- [ ] Al responder _No_, el módulo permanece activo.
- [ ] Al responder _Sí_, el sistema navega al _Dashboard_.

---

### 13. Error al generar el reporte

#### 💼 Operación

- [ ] 1. Completa los parámetros correctamente.
- [ ] 2. Simula un error (ej. servicio backend no disponible).
- [ ] 3. Da clic en _Generar_.

#### 🛡️ Validaciones

- [ ] El sistema muestra: _Error al generar el reporte de documentos y afectaciones_.
- [ ] No se descarga ningún archivo.
- [ ] El formulario conserva los valores ingresados para reintento.

---

### 14. Reabrir parámetros desde la pantalla principal

#### 💼 Operación

- [ ] 1. Cierra el diálogo con _Cancelar_.
- [ ] 2. Da clic en el botón azul _Parámetros del Reporte_ (ícono de filtro).

#### 🛡️ Validaciones

- [ ] El diálogo de parámetros se abre nuevamente.
- [ ] El formulario inicia limpio.

## 📎 Observaciones adicionales

- **Discrepancia cosmética DC1 (aceptada):** El PDF muestra fechas en formato `dd/MM/yyyy` (4 dígitos de año). El reporte Clarion original usa `dd/MM/yy`. Se mantiene el formato de 4 dígitos por legibilidad.
- **Discrepancia cosmética DC3 (aceptada):** Los importes en cero en filas de documento se muestran como vacío (consistente con otros reportes del sistema). El campo _Saldo_ siempre muestra el valor, incluyendo `0.00`.

> 🗓️ **Fecha de última modificación:** 22/06/2026
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---
## Comunicaciones

| Dir | Fecha      | Firma | Comentario      |
|-----|------------|-------|-----------------|
| ⏩  | 2026/06/22 | ST    | Primera versión |
