# 📦 Módulo: Cotizaciones Relevantes Gerente
#### 📁 **Código:** `/modules/vendedor/estadistica_ventas_vendedores`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089//app/conauxiliares/gerente/cotizaciones-relevantes)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción

Este módulo muestra la información de las cotizaciones relevantes, es decir, cotizaciones que son filtradas según tipo de usuario y el total con cierto límite (límite Gerente Zona = 1,200,00 MXN, límite Gerente Plaza/Sucursal = 500,000 MXN).

## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| dashboard   | Mostrar módulo | Permite mostrar el módulo en el panel de inicio. | Gerente / Vendedor |
| ventana   | Mostrar opción | Permite mostrar la opción en el menú principal. | Gerente / Vendedor |

En este módulo, cualquier usuario que tenga los roles de Vendedor o Gerente tendrán acceso a realizar cualquier actividad con el tablero, salvo a excepción del botón `Actualizar Estatus de Cotización` y su habilitación condicional según lo indicado en la (`Política 1`)[#💼-políticas-generales] de `Políticas Generales`.

## 💼 Políticas Generales

1. Para hacer distinción de la habilitación del botón `Actualizar Estatus de Cotización`, se siguen estas reglas:

    - Vendedor: Sólo puede actualizar cotizaciones con la columna almacén que coincidan con el almacén configurado para el vendendor.
    - Gerente: Siempre está habilitado.

2. Las cotizaciones se cargan en el tablero de acuerdo a un criterio según el tipo de usuario que inició sesión, teniendo en cuenta que su dato "Estado" del apartado del _Estado de Cotización Relevante (ECR)_ sea ausente (cero):

    - Vendedor:
        - Las cotizaciones cuyo cliente tenga asignado al vendedor que inicó sesión.
    - Gerente de Plaza:
        - Cotizaciones cuyo Total sea mayor igual al límite de Gerente Sucursal, sean de todos los almacenes que sea el usuario configurado como "Gerente de Plaza/Sucursal".
    - Gerente de Zona:
        - Cotizaciones cuyo Total sea mayor igual al límite de Gerente Zona, sean de todos los almacenes que sea el usuario configurado como "Gerente de Zona".
    - Clientes (**NO PROBADO AÚN**):
        - **_El tablero que contiene los clientes no ha sido desarrollado, más ese tablero ya está listo para esta funcionalidad_**.
        - Las cotizaciones cuyo cliente sean el que se envió para la carga del tablero.

## 🧪 Casos de Prueba

### 1. No carga del tablero por iniciar sesión o con ruta completa con rol no permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que NO tenga el rol de `Vendedor` o de `Gerente`.
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá en las pestañas `Listado de Cotizaciones Relevantes`.
- [ ] 3. En el menú lateral izquierdo, tampoco tendrás acceso a `C. Aux. > Gerente > Cotizaciones Relevantes`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Listado de Cotizaciones Relevantes` en el dashboard al cargar sistema.
- [ ] Opción del menú `C. Aux. > Gerente > Cotizaciones Relevantes` no disponible.

### 2. No carga del tablero en dashboard pero si con ruta completa (opcional)

#### 💼 Operación

- [ ] 1. Esto ocurre debido a que el usuario con el que iniciaste sesión _no tiene ninguna cotización relevante_. Entra al sistema con las credenciales de un usuario que tenga el rol de `Vendedor` o de `Gerente`.
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá en las pestañas `Listado de Cotizaciones Relevantes`.
- [ ] 3. En el menú lateral izquierdo, da clic en `C. Aux. > Gerente > Cotizaciones Relevantes` para abrir la ventana de `LISTADO DE COTIZACIONES RELEVANTES`; verás que en la primera tabla muestra el mensaje _"No se encontraron cotizaciones relevantes"_.

#### 🛡️ Validaciones

- [ ] No se encuentra `Listado de Cotizaciones Relevantes` en el dashboard al cargar sistema.
- [ ] Opción del menú `C. Aux. > Gerente > Cotizaciones Relevantes` disponible.
- [ ] Primera tabla muestra el mensaje _"No se encontraron cotizaciones relevantes"_.

### 3. Visualización del tablero tanto en dashboard como ruta completa

#### 💼 Operación

- [ ] 1. Esto ocurre debido a que el usuario con el que iniciaste sesión _no tiene ninguna cotización relevante_. Entra al sistema con las credenciales de un usuario (ej. usuario con NVEN = 94, gerente de zona) que tenga el rol de `Vendedor` o de `Gerente`.
- [ ] 2. Al carga el sistema, en el dashboard (conjunto de tableros) verás entre las pestañas `Listado de Cotizaciones Relevantes`, con información cargada con el filtro de estado `Pendiente`, habiendo 6 estados disponibles:

    1. Todas.
    2. Pendiente (icono AMARILLO).
    3. Ganada (icono VERDE).
    4. Ganada Parcial (icono VERDE / AMARILLO).
    5. Perdida (icono ROJO).
    6. Cancelada (icono ROJO CON X).

- [ ] 3. En el menú lateral izquierdo, da clic en `C. Aux. > Gerente > Cotizaciones Relevantes` para abrir la ventana de `LISTADO DE COTIZACIONES RELEVANTES`; verás la misma información cargada en el dashboard.
- [ ] 4. Al hacer clic o navegar con las teclas de flecha arriba-abajo en la tabla debajo del título `LISTADO DE COTIZACIONES RELEVANTES`, verás los detalles de la cotización y el detalle ECR de la misma, compuesto por los siguientes widgets:

    1. Tabla de Cotizaciones Relevantes.
    2. Tabla de Detalles de cotización seleccionada.
    3. Detalle de Estado de Cotización Relevante (ECR) compuesto de Estatus (porcentaje), Fecha Estimada de Cierre (formato dd/mm/yyyy) y Probabilidad de Éxito de los tipos de usuario interesados en la cotización:

        - Vendedor.
        - Gerente de Plaza / Sucursal.
        - Gerente de Zona.

> NOTA: Tanto la `Probabilidad de Éxito` así como la `Fecha Estimada de Cierre` por defecto tienen `0.00%` y `--/--/----`, y son los datos que se puede registrar con en botón `Actualizar Estatus de Cotización`, incluyendo el Estatus.

#### 🛡️ Validaciones

- [ ] Se encuentra `Listado de Cotizaciones Relevantes` en el dashboard al cargar sistema.
- [ ] Opción del menú `C. Aux. > Gerente > Cotizaciones Relevantes` disponible.
- [ ] Muestra de cotizaciones relevantes con navegación / selección mostrando detalles y detalle ECR de c7una.

### 4. Uso del filtro de Estado

#### 💼 Operación

- [ ] 1. Ya en la pantalla del `LISTADO DE COTIZACIONES RELEVANTES`, notarás que está seleccionada `Pendiente` por defecto, en la parte superior-derecha.
- [ ] 2. Selecciona cada opción del filtro de estado, notando:

    1. Cuando está consultando las cotizaciones, la tabla de cotizaciones relevantes indica el mensaje _"Consultando registros, por favor espere..."_, cargando a su vez los detalles y detalle ECR de la primera cotización de cada página de la tabla.
    2. Cuando no encuentra ninguna cotización, aparece el mensaje _"No se encontraron cotizaciones relevantes"_.
    3. Cada vez que se cambia la página de la tabla de cotizaciones relevantes, se selecciona automáticamente la primera fila, cargando los detalles y detalle ECR de la misma.
    4. Los botones de acción así como el filtro de estado están deshabilitados al hacer la consulta, y cuando se completa, se habilitan nuevamente (ver excepción de (`Política 1`)[#💼-políticas-generales] de `Políticas Generales`).

#### 🛡️ Validaciones

- [ ] Todos los casos de interacción con el formulario se muestran como en la descripción de los puntos.

### 5. Uso del botón `Refrescar / Actualizar`

#### 💼 Operación

- [ ] 1. Ya en la pantalla del `LISTADO DE COTIZACIONES RELEVANTES`, en la esquina superior-derecha vamos a dar clic en el botón `Actualizar tablero`. Se refrescará la información del tablero.
- [ ] 2. Cambia el estado y si dar otra vez clic al botón `Actualizar tablero` se respetará el filtro al cargar nuevamente la información.

#### 🛡️ Validaciones

- [ ] Actualización de informar al dar clic en el botón mencionado.
- [ ] Actualización con el filtro de estado respetado.

### 6. Uso del botón `Cancelar Cotización`

#### 💼 Operación

- [ ] 1. Ya en la pantalla del `LISTADO DE COTIZACIONES RELEVANTES`, y con un usuario con un rol permitido (`Vendedor` o `Gerente`), selecciona una cotización, y da clic en el botón `Cambiar a Estatus de Cancelada`.
- [ ] 2. Observa que la cotización ya no está, puedes usar los filtros de `Folio` (Letra y Número) para intentar encontrarla, el resultado será que no puede encontrarla si entras al tablero mediante el dashboard o la ruta completa.

> NOTA: La razón por la que no se puede encontrar es que cuandop se cargan las cotizaciones hay un filtro en el sistema de ausencia de valor `Estado` de ECR. Cuando esté listo el otro tablero que abrir el tablero de Cotizaciones Relevantes por un número de cliente específico ahí ya no haya ese filtro antes de cargar las cotizaciones.

#### 🛡️ Validaciones

- [ ] Cotización cancelada al dar clic con el botón mencionado.
- [ ] Intento de encontrar la cotización marcada como cancelada mediante filtros de columnas `Folio`.

### 7. Uso del botón `Actualizar Estatus de Cotización`

#### 💼 Operación

- [ ] 1. Ya en la pantalla del `LISTADO DE COTIZACIONES RELEVANTES`, y con un usuario con un rol permitido (`Vendedor` o `Gerente`), selecciona una cotización, y da clic en el botón `Actualizar Estatus de Cotización`. Si lo ves deshabilitado, es porque entraste con un usurio con el rol `Vendedor` y la cotización resaltada no tiene el mismo almacén configurado para tu vendedor.
- [ ] 2. Se abrirá una ventana (modal) que tendrá los datos del Detalle ECR guardados en el sistema de la cotización seleccionada.
- [ ] 3. De acuerdo al tipo de usuario, se habilitará las zonas de `Probabilidad - Fecha Estimada de Cierre - Estatus`.
- [ ] 4. Al modificar o ingresar información de actualización, se hará la validación de los valores capturados al darle clic en `Guardar`. Dichas validaciones son:

    1. Si intentas actualizar una cotización con estatus de Cancelado ...
        - MENSAJE: `Problema de seguridad: Solo se puede poner en estado de cancelada desde el botón de cancelación.`.
    2. Si los diversos campos capturados con porcentaje (%) no está entre cero y cien (0 - 100) o no corresponden a un valor numérico (como `0-1.00`) ...
        - MENSAJE 1: `Error de Captura: Valor no numérico detectado en [CAMPO CON EL ERROR].`.
        - MENSAJE 2: `Error de Captura: [CAMPO CON EL ERROR] debe estar entre 0 y 100.`
    3. Si no pones una Fecha Estimada de Cierre habilitada por tu tipo de usuario ...
        - MENSAJE: `Error de Captura: ¡Por favor ingrese una Fecha Estimada de Cierre, [TIPO USUARIO]!`.
    4. Si tienes habilitado el campo `Otro` debido a la selección de los valores `Perdida` en la lista `Estado` y el valor `Otro(s)` de la lista `¿Contra quién?` y no establecer un valor ...
        - MENSAJE: `Error de Captura: ¡Por favor ingrese un valor para el campo Otro!`.

- [ ] 5. El formulario tiene cambios de acuerdo a los datos ingresados:

    1. **Selección de un valor de Lista `Estado`**. Este patrón asegura que el usuario solo pueda capturar la información relevante según el estado de la cotización, evitando inconsistencias (ej. no permitir % ganada en “Perdida”, ni motivos de pérdida en “Ganada”):

        1. Pendiente
            - Se limpian los campos relacionados con pérdida y contra quién.
            - El campo “Otro” queda vacío.
            - El porcentaje de ganada se fija en 0.00%.
            - Todos estos campos quedan deshabilitados:
                - Motivo de pérdida.
                - Contra quién perdió.
                - Otro.
                - % Ganada.

            👉 Interpretación: La cotización sigue abierta, sin resultado definido.

        2. Ganada
            - Se limpian los campos de pérdida, contra quién y “Otro”.
            - El porcentaje de ganada se fija en 100.00%.
            - Todos estos campos quedan deshabilitados:
                - Motivo de pérdida.
                - Contra quién perdió.
                - Otro.
                - % Ganada.

            👉 Interpretación: La cotización se cerró con éxito total, no aplica registrar pérdida ni competidores.

        3. Ganada Parcial
            - Se limpian los campos de pérdida, contra quién y “Otro”.
            - Los campos de pérdida y contra quién quedan deshabilitados.
            - El campo % Ganada queda habilitado para que el usuario indique el porcentaje parcial.

            👉 Interpretación: La cotización se ganó parcialmente, por lo que es necesario capturar el porcentaje de éxito.

        4. Perdida
            - El porcentaje de ganada se fija en 0.00% y queda deshabilitado.
            - Se habilitan los campos:
                - Motivo de pérdida (obligatorio seleccionar uno).
                - Contra quién perdió (competidor).
            - El campo “Otro” se habilita solo si se selecciona la opción “Otro(s)” en contra quién.

            👉 Interpretación: La cotización se perdió, por lo que es necesario documentar la causa y el competidor.

    2. **Selección de un valor de Lista `Contra Quién`**. Este patrón asegura que el campo “Otro” solo se use cuando realmente aplica, evitando datos redundantes o inconsistentes:

        1. Si el usuario selecciona cualquier opción distinta de “Otro(s)”:
            - El campo “Otro” se limpia automáticamente (se borra cualquier valor previo).
            - El campo “Otro” queda deshabilitado, evitando que el usuario escriba información adicional innecesaria.

            👉 Interpretación: Basta con registrar el competidor elegido de la lista (Impulsora, COEL/EUROELÉCTRICA, TAMEX, ALCIONE, etc.), no se requiere detalle extra.

        2. Si el usuario selecciona la opción “Otro(s)”:

            1. El campo “Otro” se habilita para que el usuario pueda especificar manualmente el nombre del competidor no listado.

            👉 Interpretación: Se da flexibilidad para capturar competidores adicionales que no están en la lista predefinida.

    3. **Valor ingresado al campo `% Ganada`**. El usuario puede capturar un valor numérico en el campo % Ganada únicamente cuando el estado es `Ganada Parcial` (es el único caso en que el campo queda habilitado). Existe 3 reglas de negocio:

        - Si el usuario ingresa exactamente 100.00% → el sistema interpreta que la cotización ya no es parcial, sino `Ganada total`.
        - En ese momento, el formulario actualiza automáticamente el campo Estado a `Ganada`.
        - En cualquier otro valor (ej. 50%, 75%, etc.), el estado permanece como `Ganada Parcial` y el usuario debe continuar con ese flujo.

- [ ] 6. Si todas las validaciones son cumplidas, entonces los datos ingresados son correcto y se aparece una mensaje de confirmación, si damos clic en `Sí` la actualización ECR de la cotización se guardará y se cerrará la ventana de captura, recargando las cotizaciones, y si seleccionas la cotización actualizada, en los bloques del fondo del tablero aparecerán los valores capturados; si damos clic a `No` seguimos en la ventana.
- [ ] 7. En dado caso de que queramos cancelar la captura, damos clic en el `Cancelar` o en la `X` que está en la esquina superior-derecha del modal, aparecerá un mensaje de Confirmación, si damos clic en `Sí` se cerrará la ventana de actualización ECR;  si damos clic a `No` seguimos en la ventana.

#### 🛡️ Validaciones

- [ ] Apertura de la ventana de Actualizar ECR.
- [ ] Bloqueo de botón `Actualizar Estatus de Cotización` cuando entraste con rol `Vendedor` y el almacén configurado para erl usuario no sea igual al de la cotización seleccionada.
- [ ] Habilitación condicional según tipo de usuario de bloques ECR (`Probabilidad - Fecha Estimada de Cierre - Estatus`).
- [ ] Eventos de lista `Estado` están como son descritos.
- [ ] Eventos de lista `Contra Quién` están como son descritos.
- [ ] Evento de campo lista `% Ganada` están como es descrito.
- [ ] Actualización ECR exitosa.
- [ ] Cancelación de la captura exitosa.

## 📎 Observaciones adicionales

1. Se mejora la experiencia del usuario al deshabilitar los botones y erl filtro de Estado cuando se está cargando las cotizaciones, evitando que el usuario hacer las opciones descritas en este documento.
2. Debido a que en las pruebas en varios gerentes cargaron +100 cotizaciones, se optó por hacer paginación para una lectura de los registros más cómoda.
3. Por esta ocasión, las validaciones del formulario para actualizar el ECR de la cotización seleccionada se hacer al momento de dar clic al botón `Guardar`; se tomó esta decisión debido a la _habilitación condicional de los controles según el tipo de usuario_ (vendedor, geretente de plaza, etcétera), el poco espacio inferior en el maquetado del modal en los campos de los controles por el tipo de valor que contendrían valores numéricos (ej. las probabilidades de éxito de cada usuario interesado en la cotización) y a que en varios controles el valor vacío es válido.

> 🗓️ **Fecha de última modificación:** 2025-10-31
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
