# 📦 Módulo: Copia de Cotización
#### 📁 **Código:** `Modules/Ventas/Cotizaciones/Components/copia-cotizacion-modal`
#### 💻 **Menú:** Ventas > Consulta de Cotizaciones > Botón "Copiar Cotización" [Ver en QA](http://192.168.2.16:1089/app/ventas/consultadecotizaciones)

## 📝 Descripción
Este componente modal permite generar una copia de una cotización existente, creando una nueva cotización con parámetros modificables. El usuario puede seleccionar un nuevo cliente, vendedor, almacén y fecha, además de decidir si conserva los precios originales de la cotización o si los recalcula según las condiciones actuales del sistema.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Modal   | Abrir modal   | Permite abrir el modal de copia desde la tabla de consulta     | Ventas       |
| Botón   | Copiar   | Permite confirmar la creación de la nueva cotización     | Ventas       |
| Botón   | Cancelar    | Permite cerrar el modal sin realizar cambios     | Ventas       |

## 💼 Políticas Generales
- La cotización origen se identifica por su Serie (lfolio) y Folio (nfolio), los cuales son de solo lectura.
- Por defecto, la nueva cotización toma la fecha actual del sistema, pero puede ser modificada por el usuario.
- El cliente, vendedor y almacén se precargan con los datos de la cotización origen, pero pueden ser modificados.
- El checkbox "Conservar precios de la cotización original" determina si los precios unitarios se copian (marcado) o se recalculan según las condiciones actuales (desmarcado).
- El usuario que realiza la copia se registra mediante su netId obtenido del localStorage.
- Al completar la copia exitosamente, se muestra el folio generado en formato "LFolio - NFolio".

## 🧪 Casos de Prueba

### Abrir modal de copia
#### 💼 Operación
- [ ] Seleccionar una cotización de la tabla principal en "Consulta de Cotizaciones".
- [ ] Presionar el botón "Copiar Cotización".
#### 🛡️ Validaciones
- [ ] El modal debe abrirse mostrando los datos precargados:
    - Serie y Folio (readonly)
    - Cliente de la cotización origen
    - Vendedor de la cotización origen
    - Almacén de la cotización origen
    - Fecha actual del sistema
- [ ] Todos los autocompletes deben cargar correctamente con los valores iniciales.

### Modificar parámetros de la nueva cotización
#### 💼 Operación
- [ ] Cambiar la fecha utilizando el datepicker.
- [ ] Seleccionar un cliente diferente usando el autocompletable.
- [ ] Seleccionar un vendedor diferente usando el autocompletable.
- [ ] Seleccionar un almacén diferente usando el autocompletable.
#### 🛡️ Validaciones
- [ ] La fecha debe ser una fecha válida seleccionada del datepicker.
- [ ] El cliente debe ser un objeto válido seleccionado de la lista (no texto libre).
- [ ] El vendedor debe ser un objeto válido seleccionado de la lista (no texto libre).
- [ ] El almacén debe ser un objeto válido seleccionado de la lista (no texto libre).
- [ ] El botón "Copiar" debe permanecer deshabilitado hasta que todos los campos requeridos sean válidos.

### Checkbox "Conservar precios"
#### 💼 Operación
- [ ] Marcar el checkbox "Conservar precios de la cotización original".
- [ ] Desmarcar el checkbox.
#### 🛡️ Validaciones
- [ ] Al marcar el checkbox, al copiar la cotización se deben conservar los precios exactos de la cotización origen.
- [ ] Al desmarcar el checkbox, los precios deben recalcularse según las condiciones actuales del sistema.

### Confirmar copia de cotización
#### 💼 Operación
- [ ] Completar todos los campos requeridos correctamente.
- [ ] Presionar el botón "Copiar".
- [ ] Confirmar en el diálogo de confirmación "¿Desea Comenzar la Copia de la Cotización?".
#### 🛡️ Validaciones
- [ ] Debe aparecer un diálogo de confirmación antes de ejecutar la copia.
- [ ] Al confirmar, se debe enviar el request con los siguientes datos:
    - nalm (número de almacén seleccionado)
    - lfolio y nfolio (de la cotización origen)
    - nven_fac (número de vendedor seleccionado)
    - ncte (número de cliente seleccionado)
    - fcReg (fecha convertida a formato entero)
    - conservarPrecios (1 si está marcado, 0 si no)
    - turnoId (de la cotización origen)
    - netId (usuario actual del sistema)
- [ ] Si la copia es exitosa, debe mostrarse el mensaje de éxito.
- [ ] El campo "Folio Utilizado" debe mostrar el folio generado en formato "LFolio - NFolio".
- [ ] Si hay un error, debe mostrarse un mensaje descriptivo del error.

### Cancelar copia de cotización
#### 💼 Operación
- [ ] Presionar el botón "Cancelar" en cualquier momento.
- [ ] Presionar la "X" en la esquina superior derecha del modal.
#### 🛡️ Validaciones
- [ ] El modal debe cerrarse sin realizar ninguna acción.
- [ ] No se debe crear ninguna cotización nueva.
- [ ] Los datos capturados en el formulario se deben descartar.

### Validaciones de campos requeridos
#### 🛡️ Validaciones
- [ ] El campo "Fecha" es requerido y no puede estar vacío.
- [ ] El campo "Cliente" es requerido y debe ser un objeto válido (validador requireObjectValidator).
- [ ] El campo "Vendedor" es requerido y debe ser un objeto válido (validador requireObjectValidator).
- [ ] El campo "Almacén" es requerido y debe ser un objeto válido (validador requireObjectValidator).
- [ ] Si el usuario escribe texto en los autocompletes sin seleccionar una opción, debe mostrar error: "Debe seleccionar una opción válida de la lista".
- [ ] El botón "Copiar" debe estar deshabilitado si el formulario es inválido.

## 📎 Observaciones adicionales
- El componente utiliza autocompletes personalizados: `app-cliente-autocomplete`, `app-vendedor-autocomplete` y `app-almacen-autocomplete`.
- La función `convertDateToInt` convierte la fecha del datepicker a formato entero para el request al backend.
- El validador personalizado `requireObjectValidator()` asegura que los autocompletes tengan objetos seleccionados y no solo texto escrito.
- El servicio `DialogService` se utiliza para mostrar el diálogo de confirmación y los mensajes de éxito/error.
- El servicio `ConsultaCotizacionService` maneja la petición de copia al backend mediante el método `copiarCotizacion()`.
- El campo "Folio Utilizado" es de solo lectura y se llena automáticamente después de una copia exitosa.

> 🗓️ **Fecha de última modificación:** 2025-11-05
> 👤 **of_dev10**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏩| 2025/11/05 | Erick López |Documentación inicial creada|