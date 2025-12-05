# 📦 Módulo: Captura Promesas Cobranza Gerente
#### 📁 **Código:** `src\app\modules\conaux\gerente\seguimiento-cobranza\captura-promesas`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089//app/conauxiliares/gerente/seguimiento-cobranza/captura-promesas)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción

Este módulo muestra el tablero correspondiente al **Framework de Cobranza**, teniendo 2 modos de acuerdo al rol del usuario que inició sesión en el AdministraWeb:

1. **SÓLO LECTURA (read-only)**: Usuario con el rol `Vendedor`, visualiza la información de sus promesas. **Se comprenderá como `modo sólo lectura` cuando no se encuentre ninguna celda o campo editable en el tablero**.
2. **MODO CAPTURA (editable)**: Usuario con rol `Gerente`, y de acuerdo a las fechas de captura impuestas por la configuración entre otras restricciones, se mostrará celdas editables y el campo de comentarios habilitados para la actualización con los datos acordados entre Gerente y Vendedor. Por lo tanto, **se comprenderá con `modo captura` cuando en el tablero haya al menos una celda o campo editable**.

> NOTA: _Framework_ es un procedimiento especificado entre persona con un objetivo en específico, ejemplo: el _Framework de Cobranza_ es el procedimiento para visualizar las promesas de Cobranza entre un vendedor y sus clientes, capturadas por un Gerente.

## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| dashboard   | Mostrar módulo | Permite mostrar el módulo en el panel de inicio. | Vendedor |
| ventana   | Mostrar opción | Permite mostrar la opción en el menú principal. | Gerente |
| botón   | Manual Editar Promesas | Ventana con instrucciones de edición de promesas para gerentes | Gerente |
| botón   | Actualizar Promesas | Guarda los cambios en las promesas del vendedor seleccionado | Gerente |
| autocomplete   | Vendedores | Buscador de vendedor según filtro tecleado como la llave de búsqueda | Gerente |

En este módulo, sólo aparece el tablero para el rol Vendedores al inicio del sistema, para el rol Gerente se debe de navegar con la ruta completa
según lo indicado en la (`Política 1`)[#💼-políticas-generales] de `Políticas Generales`.

## 💼 Políticas Generales

### POLITICA 1. El tablero aparece según el rol

    - Vendedor: En el `dashboard` al cargar el sistema una vez iniciando sesión.
    - Gerente: En la ruta `C. Aux. / Gerente / Seguimiento Cobranza / Captura Promesas Cobranza`.

### POLÍTICA 2. La carga de información varia según el rol

    - Vendedor: Desde la carga del tablero aparecerán las promesas de cobranza del vendedor.
    - Gerente: La tabla de registros de promesas (PCXC) (y por consecuente la tabla de detalle de promesa) aparecerá vacía, por lo tanto el Gerente tendrá que hacer uso del buscador de vendedores que se encuentra en la parte superior derecha ya sea por nombre o por el NVEN asignado al vendedor.

### POLITÍCA 3. El tablero tiene los siguientes bloques

    - **Tabla principal de registros de promesas (PCXC)**:
        - La tabla más grande del tablero, que tiene 4 conjuntos de columnas con el encabezado _Etapa [número] (donde níumero = 1, 2, 3, y 4)_representado las **4 Etapas** con su primera columna especificando la promesa del vendedor a vender en esa etapa. **Sólo hay una etapa activa, reconocida por el fondo de las celdas color `blanco`**, mientras que las otras 3 etapas serán representadas por un color `gris`, y cuando las promesas no estén para la captura de datos todas se bloqueadan coon ese mismo color. La columnas de Cliente y las finales con el encabezado `Mensual` siempre estarán de fondo color blanco.
    - **Buscador de Vendedores**:
        - Barra de búsqueda la cual mientras vas escribiendo caracteres te listará todos los vendedores del Gerente que inició sesión que correspondan al criterio tecleado.
    - **Botones de Acción**:
        - Son los botones en la parte superior derecha, que son los siguientes:
            1. Manual Editar Promesas (sólo para rol `Gerente`).
            2. Actualizar Promesas (sólo para rol `Gerente`).
            3. Actualizar tablero (sin distinción de rol).
    - **Tabla secundaria de detalle de promesa seleccionada**:
        Tablas con un registro que detalle la promesa con los saldos y demás cantidades que son útiles para clarificar el estado de la promesa seleccionada.
    - **Campo de texto de Comentarios de promesa seleccionada**:
        - En el fondo de tablero, en el que aparece los comentarios del registros PCXC seleccionado.

    Reiterando que no todos los elementos aparecerán para los usuarios, su rol es que decidirá qué bloques aparecerán y cuáles se habilitarán.

    > NOTA: Si al Gerente al cargar el tablero no le carga las instrucciones de captura de promesas, significa que no hay modo captura habilitado.

### POLÍTICA 4. Acerca de celdas y campos editables

Las celdas editables de promesas y compromiso mensual tiene un estilo característico indicado en el Manual de Editar Promesas que puedes abrir en el botón azul en la parte superior derecha (al lado del buscador de vendedores). Si no ves ninguna celda con ese estilo, significa que ninguna etapa de promesas ha sido habilitada para su edición. Salvo ciertas excepciones, **los días de captura son los lunes y martes del mes en curso**.

### POLÍTICA 5. Valores permitidos en modo captura

La reglas para las capturas de promesas y compromisos en la tabla de registros PCXC son la siguientes:

    1. Cualquier valor no numérico retorna cero.
    2. Valores negativos serán rechazados.
    3. Valores decimales serán redondeados al entero más cercano.
    4. Notación científica (e/E, ejemplo `1e10`) será rechazada y convertida a cero.
    5. Rango permitido: número entero entre $0 y $999,999,999.

### POLÍTICA 6. Ausencia de carga del tablero en Dashboard

Cuando se accede con un usuario que tiene rol `Vendedor` y no tiene registros de promesas de cobranzas, no se cargará el tablero en la pantalla de inicio.

### POLÍTICA 7. Cálculo automático de promesa en Etapa 4

Cuando la etapa activa es la 4, no se mostrara como celda editable la columna de `Promesa` del conjunto de columnas `Etapa 4`. De acuerdo al valor capturado del `Mensual - Compromiso`, para cada registros de promesa se hará el siguiente cálculo:

\(Promesa Etapa 4 = Cantidad Compromiso - Recuperado\)

## 🧪 Casos de Prueba

### 1. No carga del tablero por iniciar sesión o con ruta completa con rol no permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que NO tenga el rol de `Vendedor` o de `Gerente`.
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá en las pestañas `Promesas de Cobranza` ni la ruta del menú ``.
- [ ] 3. En el menú lateral izquierdo, tampoco tendrás acceso a `C. Aux. / Gerente / Seguimiento Cobranza / Captura Promesas Cobranza`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Promesas de Cobranza` en el dashboard al cargar sistema.
- [ ] No se encuentra la ruta `C. Aux. / Gerente / Seguimiento Cobranza / Captura Promesas Cobranza`.

### 2. No carga del tablero en dashboard con usuario con rol `Vendedor` por ausencia de registros

#### 💼 Operación

- [ ] 1. Esto ocurre debido a que el usuario con el que iniciaste sesión _no tiene ningún registro PCXC_. Entra al sistema con las credenciales de un usuario que tenga el rol de `Vendedor` (ejemplo: prueba con el NVEN = 281).
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá en las pestañas `Promesas de Cobranza`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Promesas de Cobranza` en el dashboard al cargar sistema.

### 3. Visualización del tablero en dashboard

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` (ejemplo: NVEN 280 o 519).
- [ ] 2. Al carga el sistema, en el dashboard (conjunto de tableros) verás entre las pestañas `Promesas de Cobranza`, da clic en la pestaña.
- [ ] 3. Verás el tablero en **modo sólo lectura** con los siguientes bloques:
    1. Letrero en donde se muestra el Periodo y el nombre del vendedor.
    2. Botón de `Actualizar tablero`.
    3. Tabla de registros de promesas (PCXC).
    4. Tabla de detalle de registro de promesa seleccionado.
    5. Campo de texto de comentario de registro de promesa seleccionado.
- [ ] 4. En ningún modo ninguno de los bloque se tiene que mostrar habilitado.
- [ ] 5. No se mostrará el `Manual de Editar Promesas` al cargar el tablero.
- [ ] 6. Puedes refrescar la información del tablero dando clic en el botón de `Actualizar tablero`.

#### 🛡️ Validaciones

- [ ] Pestaña `Promesas de Cobranza` en el dashboard al cargar sistema.
- [ ] Tablero `modo sólo lectura`.
- [ ] `Manual de Editar Promesas` no mostrado al cargar el tablero.
- [ ] Clic en el botón `Actualizar tablero` para refrescar información.

### 4. Visualización del tablero en ruta completa

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Gerente` (ejemplo: NVEN 94 o 65) en la base de datos (BD) del periodo actual. Ejemplo: Si la fecha de hoy es Noviembre 2025, asegúrate de entrar a BD con este periodo.
- [ ] 2. Navega mediante el menú del lado izquierdo a la siguiente ruta: `C. Aux. / Gerente / Seguimiento Cobranza / Captura Promesas Cobranza`.
- [ ] 3. Verás el tablero en **modo captura** con los siguientes bloques:
    1. Letrero en donde se muestra el Periodo y el mensaje `SELECCIONAR VENDEDOR`.
    2. Buscador de vendedores en la parte superior - derecha.
    3. Set de Botones de Acción completa:
        - `Actualizar Promesas`.
        - `Manual Editar Promesas`.
        - `Actualizar tablero`.
    4. Tabla de registros de promesas (PCXC).
    5. Tabla de detalle de registro de promesa seleccionado.
    6. Campo de texto de comentario (habilitado para edición) de registro de promesa seleccionado.
- [ ] 4. Las reglas para la obtención de celdas editables de promesas y compromisos en la tabla de registros de promesas son:
    1. El usuario que está accediendo al tablero está configurado como Gerente.
    2. El periodo de la BD con la que entraste al sistema debe de ser igual al periodo obtenido por la fecha de hoy. Ejemplo: Si la fecha de hoy es 29 de Noviembre de 2025, el periodo es `Noviembre 2025`, ambos periodo deben de coincidir para que la edición de promesas esté habilitada.
    3. Debe de haber una `etapa activa` (ver (`Política 3`)[#política-3-el-tablero-tiene-los-siguientes-bloques] de `Políticas Generales`).
    4. **CASO ESPECIAL**: Sólo debe de estar editable la columna `Mensual - Compromiso` si está como etapa activa la `Etapa 4`.
- [ ] 5. Si no hay una etapa activa habilitada en la tabla de registros de promesas pero entraste al sistema con una BD del periodo actual, sólo deberá mostrar habilitado el campo de texto de los comentarios (fondo del tablero).
- [ ] 6. Se mostrará el `Manual de Editar Promesas` al cargar el tablero si el `modo captura` está habilitado (o si actualizas la pestaña del navegador en donde tiene abierto el tablero).

#### 🛡️ Validaciones

- [ ] Tablero accedible en ruta completa `C. Aux. / Gerente / Seguimiento Cobranza / Captura Promesas Cobranza`.
- [ ] Tablero en `modo captura`de los registros de promesas + campo de comentario si hay etapa activa o sólo el campo de comentario (parte de fondo del tablero) si las condiciones descritas en el paso 4 se cumplen, de otra manera el `modo sólo lectura`.
- [ ] `Manual de Editar Promesas` mostrado al cargar el tablero si hay `modo captura` habilitado.

### 5. Uso del modo captura por el Gerente

#### 💼 Operación

- [ ] 1. Una vez con el acceso al tablero por parte de un usuario con rol `Gerente` y configurado como tal*, debemos de deber al menos celdas editables y/o campo de comentario habilitado para la edición.
- [ ] 2. Identifica las celdas editables en cada registro con un estilo de borde punteado verde, en un conjunto de columnas `Etapa #` (siendo # el número de la etapa activa de) son las de las columnas `Promesa` y `Compromiso`, así como el campo de comentarios al fondo del tablero.
- [ ] 3. Los actores del Framework de Cobranza son el Gerente (quien manipula el tablero) y el Vendedor (quien tiene los datos de promesas y compromisos). Teclea los datos deseados en las columnas con las siguientes indicaciones:

    1. **EDITA LA CELDA**: Presiona `ENTER` o `F2` o con `DOBLE CLIC`.
    2. **SIGUE LAS REGLAS DE LOS VALORES PERMITIDOS**:
        - En la tabla principal de los registros de promesas, los datos permitidos son los descritos en la (`Política 5`)[#política-5-valores-permitidos-en-modo-captura] de `Políticas Generales`.
        - Puedes poner cualquier observación en el campo de comentarios **del registro de promesa seleccionado**, cuya fila se muestra con un color verde claro.
        - Cuando ingresas un valor no permitido, se aparece un mensaje en color rojo en la esquina superior izquierda: _Edición de Registros de Promesas ... La cantidad debe ser un número entero entre $0 y $999,999,999_.
    3. **CONFIRMAR EL VALOR CAPTURADO**: Para las celdas editables de la tabla de registros de promesas, presiona `ENTER` o cambia la selección por otro registro, verás un mensaje en color azul que indica _Edición de Registros de Promesas ... Totales recalculados._ en la parte superior izquierda. Para confirmar la edición del comentario capturado, presiona `CTRL + ENTER` y en la misma ubicación del mensaje anterior aparece un mensaje _COMENTARIO DE LA PROMESA ... El comentario ha sido editado._.
    4. **CANCELAR LA CAPTURA DE UN VALOR**: Para las celdas editables de la tabla de registros de promesas, presiona la tecla `ESC`, y la celda mostrará el valor anterior.
    5. **CANCELAR LA CAPTURA / DESCARTAR TODOS LOS CAMBIOS**: Da clic en el botón de acción `Actualizar tablero` para descartar todos los cambios y volver a cargar todos los valores anteriores.

- [ ] 4. Una vez confirmado la información ingresada entre el Gerente y el Vendedor, el Gerente puede darle clic al botón de acción `Actualizar Promesas`, y una vez que el sistema haya guardado los cambios, te aparecerá un mensaje: `Actualización de promesas exitosa.`. Para comprobar si la actualización tuvo efecto, puedes cargar de nuevo al mismo vendedor y observar los valores iniciales del tablero.
- [ ] 5. Se volverá a "limpiar" todo el tablero como en la carga para que el Gerente seleccione otro vendedor para la actualización de sus promesas.

> \* Que en la BD esté registrado como un Gerente.

#### 🛡️ Validaciones

- [ ] `Modo captura` detectado en el tablero.
- [ ] Identificación de celda y campo editables.
- [ ] Manipulación del modo captura de acuerdo al paso 3.
- [ ] Confirmación de la actualización de promesas de un vendedor.
- [ ] Limpieza del tablero una vez actualizado las promesas para un vendedor.

## 📎 Observaciones adicionales

1. Se mejora la experiencia del usuario al deshabilitar los botones de acción y el buscador de vendedores (cuando no se ha hecho nigún cambio) cuando se está cargando la información del tablero, evitando que el usuario hacer las opciones descritas en este documento.
2. Se hicieron 2 mejoras al buscador de vendedores:
    - **Modo Gerente**, en donde el Gerente sólo puede buscar a los vendedores que son sus subordinados, en otras palabras, a todo vendedores que se encuentre registrado en los almacenes a cargo del Gerente.
    - **Limpieza del buscador**, en donde ya no se encuentra con el valor buscado anteriormente.
3. Se mejoró la seguridad con el nuevo método de propagación de datos sensibles.

> 🗓️ **Fecha de última modificación:** 2025-12-05
> 👤 **Sergio Tostado**
> 🏷️ **Versión:** 1

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
