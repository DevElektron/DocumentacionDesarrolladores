# 📦 Módulo: Cotizaciones Proyectos Ingeniería...
#### 📁 **Código:** `Modules/Produccion/proy-cot-ingenieria`
#### 💻 **Menú:** Producción > Proyectos y Cotizaciones de Ingeniería [Ver en QA](http://192.168.2.16:1089/app/produccion/proycotingenieria)

## 📝 Descripción
Este módulo permite la gestión de proyectos de ingeniería y sus cotizaciones asociadas, incluyendo:
- Registro y administración de proyectos.
- Creación y modificación de cotizaciones.
- Gestión de prórrogas para proyectos y cotizaciones.
El sistema incluye validaciones por rol, control de estatus, cálculo de tiempos de atención y alertas visuales mediante colores para identificar retrasos en el proceso.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir Proyecto   | Permite añadir un proyecto     | Todos       |
| Botón   | Modificar Proyecto   | Permite modificar la información del proyecto     | Todos       |
| Botón   | Registrar Prorroga   | Permite registrar una prorroga a un proyecto     | Todos       |

Además, el comportamiento depende del **TipoArt del usuario**
| Tipo Art | Rol                    |
|----------|------------------------|
| 1        | Vendedor               |
| 2        | Cotizador              |

## 💼 Políticas Generales
- El módulo consulta la tabla CTRL con estatus = 'A' al iniciar. Si no existen registros se muestra el mensaje: **No existen los parámetros del sistema**
- Se obtiene el NVEN del usuario en sesión. Con este se consulta la información del vendedor.
- Se obtiene el campo TipoArt, el cual controla múltiples validaciones dentro del módulo.
- Se consulta la tabla ELINI para obtener *DiasMax*, este valor determina el límite máximo de días permitidos para solicitar prórrogas.

## 🧪 Casos de Prueba

### Capturar Proyecto
#### 💼 Operación
-  Se debe registrar un nuevo proyecto en la tabla ELPRY.
- El usuario debe seleccionar:
    - Cliente
    - Vendedor
    - Cotizador
- El tipo de cotización se asigna por defecto como PRESUPUESTO (puede modificarse).
- El área de cotización se asigna por defecto como INGENIERÍA (puede modificarse).
- Los siguientes campos son obligatorios:
    - Cliente
    - Vendedor
    - Cotizador
- Se selecciona Enviar notificación a, el usuario seleccionado debe tener TipoArt = 2. Si no cumple se muestra **No es Cotizador. Favor de seleccionar un Cotizador**

#### 🛡️ Validaciones
- El sistema debe validar que existan parámetros en la tabla CTRL con estatus = 'A'. Si no existen registros se debe mostrar el mensaje **No existen los parámetros del sistema**
- El sistema obtiene el NVEN del usuario en sesión.
- Se consulta el TipoArt del usuario, el cual controla el comportamiento del módulo.

### Modificar Proyecto
#### 💼 Operación
- Permite modificar la información del proyecto registrado.
- El comportamiento depende del rol del **usuario (TipoArt)**.

#### 🛡️ Validaciones
##### Restricciones adicionales
- No se puede cambiar el **vendedor asignado**.
- No se puede modificar el **tipo de cotización**.
- Si status = 1 o status = 3, el botón Guardar debe quedar bloqueado.
- Si status = 3 y comCot está vacío se debe mostrar el mensaje **Falta la Observación por la cual es Rechazado este Proyecto**
- Si el usuario que modifica es vendedor (TipoArt = 1) y el estatus es 2, el sistema debe cambiar el estatus automáticamente a **0 (Pendiente)**

##### Restricciones por tipo de usuario
Si tipoArt = 1 (Vendedor)
- Puede modificar
    - estatus.
    - comentario del cotizador.
- No puede modificar
    - descripción
    - comentario del vendedor
- Puede adjuntar archivos.

Si tipoArt = 2 (Cotizador)
- Puede Modificar.
    - estatus
    - comentario del cotizador
    - tipo de cotizacion
- No puede modificar.
    - descripcion
    - comentario del vendedor
    - archivos adjuntos

##### Notificaciones
Al modificar el proyecto se debe enviar un correo electrónico al usuario correspondiente:
- Si el usuario que modificó es un cotizador, se le envia el correo al vendedor
- Si el usuario que modifico es un vendedor, se le envia el correo al cotizador

### Capturar Prorroga
#### 💼 Operación
- Se permite registrar una prorroga para proyectos
- Las prórrogas se almacenan en la tabla **ELPRG**
#### 🛡️ Validaciones
- El número de prórroga se genera automáticamente como **MAX(ELPRG.Numero) + 1**
- Todos los campos del registro son obligatorios.
- Los siguientes valores se inicializan automáticamente

| Campo        | Valor                     |
|--------------|---------------------------|
| NoCotizacion | NoCotizacion del proyecto |
| NoPartida    | 0                         |
| Version      | 0                         |
| FcCaptura    | Fecha actual              |
| IdSolicita   | NetId del usuario         |
| NVenSolicita | NVEN del usuario          |

##### Restricciones por usuario
- Si TipoArt = 1 (Vendedor)
    - Solo puede solicitar prórroga en proyectos donde **NVEN = PRY.Nven**
- Si no
    - Se muestra el mensaje **No Puede Agregar Prorrogas a un Proyecto que no es Tuyo**

- Si TipoArt != 1
    - Se valida que **NVEN = PRY.Ncot**
- Si no
    - Se muestra el mensaje **No Puede Agregar Prorrogas**

##### Validación de límite de prorroga
- El valor **DiasMax** define el límite máximo de días permitidos para prórrogas.
    - Si los días solicitados (DiasSol) son mayores a DiasMax se debe mostrar el mensaje **El límite de la prorroga es de X días**

##### Calculo Automático de Días
Al realizar la modificación de DiasSol o  FcAVencer se hace el recalculo de valores
- Cambio de DiasSol
    - FcVencer = PRY.FcActualiza + DiasSol
- Cambio de Fecha de Vencimiento
    - DiasSol = FcVencer - PRY.FcActualiza

##### Notificaciones
Al registrar la prórroga se debe enviar un correo electrónico al usuario correspondiente:
- Si el usuario que modificó es un cotizador, se le envia el correo al vendedor
- Si el usuario que modifico es un vendedor, se le envia el correo al cotizador

## 📎 Observaciones adicionales
- El módulo utiliza indicadores visuales por color en las tablas para mostrar retrasos en los procesos.

| Color    | Significado             |
|----------|-------------------------|
| Amarillo | 1 Día de retraso        |
| Naranja  | 1 Día de retraso        |
| Rojo     | 3 o más días de retraso |

- Los colores se calculan a partir de diferentes escenarios relacionados con:
    - fecha de entrega
    - valoración
    - estatus de cotización
    - fecha de actualización

> 🗓️ **Fecha de última modificación:** 2026-03-06
> 👤 **Daniel Salazar**
> 🏷️ **Versión:** 1