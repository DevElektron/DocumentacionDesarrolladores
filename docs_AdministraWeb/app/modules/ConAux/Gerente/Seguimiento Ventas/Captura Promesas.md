# 📦 Módulo: Captura Promesas Ventas Gerente
#### 📁 **Código:** `src\app\modules\conaux\gerente\seguimiento-ventas\captura-promesas`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/conauxiliares/gerente/seguimiento-ventas/captura-promesas)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción

Este módulo muestra el tablero correspondiente al **Framework de Ventas**, teniendo 2 modos de acuerdo al rol del usuario que inició sesión en el AdministraWeb:

1. **SÓLO LECTURA (read-only)**: Usuario con el rol `Vendedor`, visualiza la información de sus promesas. **Se comprenderá como `modo sólo lectura` cuando no se encuentre ninguna celda o campo editable en el tablero**.
2. **MODO CAPTURA (editable)**: Usuario con rol `Gerente`, y de acuerdo a las fechas de captura impuestas por la configuración entre otras restricciones, se mostrará celdas editables y el campo de comentarios habilitados para la actualización con los datos acordados entre Gerente y Vendedor. Por lo tanto, **se comprenderá con `modo captura` cuando en el tablero haya al menos una celda o campo editable**.

> NOTA: _Framework_ es un procedimiento especificado entre personas con un objetivo en específico, ejemplo: el _Framework de Ventas_ es el procedimiento para visualizar las promesas de Ventas entre un vendedor y sus clientes, capturadas por un Gerente.

## 🔐 Seguridad

| Tipo UI      | Elemento               | Descripción                                                          | Rol permitido |
|--------------|------------------------|----------------------------------------------------------------------|---------------|
| dashboard    | Mostrar módulo         | Permite mostrar el módulo en el panel de inicio.                     | Vendedor      |
| ventana      | Mostrar opción         | Permite mostrar la opción en el menú principal.                      | Gerente       |
| botón        | Manual Editar Promesas | Ventana con instrucciones de edición de promesas para gerentes       | Gerente       |
| botón        | Actualizar Promesas    | Guarda los cambios en las promesas del vendedor seleccionado         | Gerente       |
| autocomplete | Vendedores             | Buscador de vendedor según filtro tecleado como la llave de búsqueda | Gerente       |

En este módulo, sólo aparece el tablero para el rol Vendedores al inicio del sistema, para el rol Gerente se debe de navegar con la ruta completa
según lo indicado en la [`Política 1`](#politica-1-el-tablero-aparece-por-rol-asignado) de `Políticas Generales`.

## 💼 Políticas Generales

<a id="politica-1-el-tablero-aparece-por-rol-asignado"></a>

### POLÍTICA 1. El tablero aparece según el rol

- **Vendedor**: En el `dashboard` al cargar el sistema una vez iniciando sesión.
- **Gerente**: En la ruta `C. Aux. / Gerente / Captura Promesas Ventas`.

### POLÍTICA 2. La carga de información varia según el rol

- **Vendedor**: Desde la carga del tablero aparecerán las promesas de Ventas del vendedor.
- **Gerente**: La tabla de registros de promesas (PVXC) y por consecuente las tablas de detalle de promesas (datos del cliente, complemento y acuerdos), aparecerá vacía, por lo tanto el Gerente tendrá que hacer uso del buscador de vendedores que se encuentra en la parte superior derecha ya sea por nombre o por el NVEN asignado al vendedor.

<a id="politica-3-el-tablero-tiene-los-siguientes-bloques"></a>

### POLÍTICA 3. El tablero tiene los siguientes bloques

- **Lista de pestañas (tablist) principal**
    1. **Tabla principal de registros de promesas (PVXC)**: Contiene 4 conjuntos de columnas con el encabezado _Etapa # (donde # = 1, 2, 3, y 4)_ representado las **4 Etapas** con su primera columna especificando la promesa del vendedor a vender en esa etapa. **Sólo hay una etapa activa, reconocida por el fondo de las celdas color `blanco`**, mientras que las otras 3 etapas serán representadas por un color `gris`, y cuando las promesas no estén para la captura de datos, todas se bloquean con ese mismo color. La columnas de Cliente y las finales con el encabezado `Mensual` siempre estarán de fondo color blanco.
    2. **Tabla Scorecard**: Tabla que indica las 8 métricas / estadísticas de Ventas del vendedor:
        - Margen.
        - % Venta.
        - Cuota.
        - Venta.
        - Compromiso.
        - Venta Compromiso.
        - % Compromiso.
        - Diferencia.
    3. **Tabla de Factores Críticos**: Muestra los Indicadores Clave de Rendimiento (KPIs).
- **Buscador de Vendedores**: Barra de búsqueda la cual mientras vas escribiendo caracteres te listará todos los vendedores del Gerente que inició sesión que correspondan al criterio tecleado.
- **Botones de Acción**: Son los botones en la parte superior derecha, que son los siguientes:
    1. Manual Editar Promesas (sólo para rol `Gerente`).
    2. Actualizar Promesas (sólo para rol `Gerente`).
    3. Actualizar tablero (sin distinción de rol).
- **Lista de pestañas (tablist) secundaria**. En el fondo del tablero:
    1. **Tabla de Datos Cliente de promesa seleccionada**. Primera tabla de detalle de promesa que contiene las siguiente columnas:
        - $ Asignados (Actuales / Completos).
        - $ Backorders (Actuales / Surten Mes).
        - $ Cotizaciones (Art. 30 días / Ing. 60 días).
        - Crecimiento.
        - $ Ventas (12 Meses Prom. / Hace 1 Mes / Hace 2 Meses / Hace 3 meses).
        - Márgenes (Crece / Hace 1 Mes / Hace 2 Meses / Hace 3 meses).
    2. **Tabla de Datos Complemento de promesa seleccionada**. Segunda tabla de detalle de promesa que contiene las siguiente columnas:
        - Crédito (Uso 6M / Uso Act. / Límite / Plazo / Días Pago Prom. / Saldo / Saldo x V.)
        - Saldo Vencido (30 días / 60 días / 90 días / 90+ días).
        - Compromiso vs Cuota.
        - Tendencia $.
        - Clasificación.
    3. **Tabla de Acuerdos**. En lugar de los comentarios que aparecen en las [_promesas de Cobranza_](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/docs_AdministraWeb/app/modules/ConAux/Gerente/Seguimiento%20Cobranza/Captura%20Promesas.md).
  
Reiterando que no todos los elementos aparecerán para los usuarios, su rol es que decidirá qué bloques aparecerán y cuáles se habilitarán.

> NOTA: Si al Gerente al seleccionar y cargar el primer vendedor no te muestra el manual de captura de promesas, significa que no hay modo captura habilitado.

### POLÍTICA 4. Acerca de celdas y campos editables

Las celdas editables de promesas y compromiso mensual tiene un estilo característico indicado en el `Manual de Editar Promesas` que puedes abrir en el botón azul en la parte superior derecha (al lado del buscador de vendedores). Si no ves ninguna celda con ese estilo, significa que ninguna etapa de promesas ha sido habilitada para su edición. Salvo ciertas excepciones, **los días de captura son los lunes y martes del mes en curso**.

<a id="politica-5-valores-permitidos-en-modo-captura"></a>

### POLÍTICA 5. Valores permitidos en modo captura

La reglas para las capturas de promesas y compromisos en la tabla de registros PVXC son la siguientes:

    1. Cualquier valor no numérico retorna cero.
    2. Valores negativos serán rechazados.
    3. Valores decimales serán redondeados al entero más cercano.
    4. Notación científica (e/E, ejemplo `1e10`) será rechazada y convertida a cero.
    5. Rango permitido: número entero entre $0 y $999,999,999.

### POLÍTICA 6. Ausencia de carga del tablero en Dashboard

Cuando se accede con un usuario que tiene rol `Vendedor` y no tiene registros de promesas de Ventas, no se cargará el tablero en la pantalla de inicio.

### POLÍTICA 7. Cálculo automático de promesa en Etapa 4

Cuando la etapa activa es la 4, no se mostrara como celda editable la columna de `Promesa` del conjunto de columnas `Etapa 4`. De acuerdo al valor capturado del `Mensual - Compromiso`, para cada registros de promesa se hará el siguiente cálculo:

**Promesa Etapa 4** = _Compromiso (mensual)_ - _Venta (mensual)_  

<a id="politica-8-celdas-editables"></a>

### POLÍTICA 8. Habilitación del modo captura

Para la tablas de promesas, se tendrá las siguientes reglas para determinar si una celda es editable o no:

1. Si es una celda que muestra totales, _no es editable_.
2. Si no es una celda de promesa de etapas 1-2-3 o la columna de Mensual - Compromiso, _no es editable_.
3. Si el modo captura no se encuentra configurado en el día en que se abre el tablero, _no es editable ninguna celda (modo sólo lectura)_.
4. El usuario que está accediendo al tablero está configurado como Gerente.
5. El periodo de la BD con la que entraste al sistema debe de ser igual al periodo obtenido por la fecha de hoy. Ejemplo: Si la fecha de hoy es 29 de Noviembre de 2025, el periodo es `Noviembre 2025`, ambos periodo deben de coincidir para que la edición de promesas esté habilitada.
6. Debe de haber una `etapa activa` (ver [Política 3](#politica-3-el-tablero-tiene-los-siguientes-bloques) de `Políticas Generales`).
7. **CASO ESPECIAL**: Sólo debe de estar editable la columna `Mensual - Compromiso` si está como etapa activa la `Etapa 4`.

## 🧪 Casos de Prueba

### 1. No carga del tablero por iniciar sesión o con ruta completa con rol no permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que NO tenga el rol de `Vendedor` o de `Gerente`.
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá entre las pestañas `Promesas de Ventas`.
- [ ] 3. En el menú lateral izquierdo, tampoco tendrás acceso a `C. Aux. / Gerente / Captura Promesas Ventas`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Promesas de Ventas` en el dashboard al cargar sistema.
- [ ] No se encuentra la ruta `C. Aux. / Gerente / Captura Promesas Ventas`.

### 2. No carga del tablero en dashboard con usuario con rol `Vendedor` por ausencia de registros

#### 💼 Operación

- [ ] 1. Esto ocurre debido a que el usuario con el que iniciaste sesión _no tiene ningún registro PVXC_. Entra al sistema con las credenciales de un usuario que tenga el rol de `Vendedor` (ejemplo: prueba con el NVEN = 26 en periodo en periodo de Enero 2026).
- [ ] 2. Al cargar el sistema, en el dashboard (conjunto de tableros) no habrá en las pestañas `Promesas de Ventas`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Promesas de Ventas` en el dashboard al cargar sistema.

### 3. Visualización del tablero en dashboard

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Vendedor` (ejemplo: NVEN 280, 272, 662 o 519).
- [ ] 2. Al carga el sistema, en el dashboard (conjunto de tableros) verás entre las pestañas `Promesas de Ventas`, da clic en la pestaña.
- [ ] 3. Verás el tablero en **modo sólo lectura** con los siguientes bloques:
    1. Letrero en donde se muestra el Periodo y el nombre del vendedor.
    2. Botón de `Actualizar tablero`.
    3. Tablist principal.
    4. Tablist secundario.
- [ ] 4. En ningún modo ninguno de los bloques se tiene que mostrar habilitado para ingresar datos.
- [ ] 5. No se mostrará el `Manual de Editar Promesas` al cargar el tablero.
- [ ] 6. Puedes refrescar la información del tablero dando clic en el botón de `Actualizar tablero`.

#### 🛡️ Validaciones

- [ ] Pestaña `Promesas de Ventas` en el dashboard al cargar sistema.
- [ ] Tablero `modo sólo lectura`.
- [ ] `Manual de Editar Promesas` no mostrado al cargar el tablero.
- [ ] Clic en el botón `Actualizar tablero` para refrescar información.

### 4. Visualización del tablero en ruta completa

#### 💼 Operación

- [ ] 1. Accede al sistema con un usuario que tenga el rol de `Gerente` (ejemplo: NVEN 94) en la base de datos (BD) del periodo actual. Ejemplo: Si la fecha de hoy es Enero 2026, asegúrate de entrar a una BD con este periodo.
- [ ] 2. Navega mediante el menú del lado izquierdo a la siguiente ruta: `C. Aux. / Gerente / Captura Promesas Ventas`.
- [ ] 3. Verás el tablero en **modo captura** con los siguientes bloques:
    1. Letrero (slogan) que dice `Para mostrar promesas > SELECCIONAR VENDEDOR`.
    2. Buscador de vendedores en la parte superior - derecha.
    3. Set de Botones de Acción completo:
        - `Actualizar Promesas`.
        - `Manual Editar Promesas`.
        - `Actualizar tablero`.
    4. Tablist principal, con la tabla de registros de promesas (PVXC) con el modo captura habilitado según [`política 8 de habilitación de modo captura`](#politica-8-celdas-editables).
    5. Tablist secundario de promesa seleccionada.
- [ ] 4. Se mostrará el `Manual de Editar Promesas` al cargar el tablero con el primer vendedor que el Gerente seleccione si el `modo captura` está habilitado (o si actualizas la pestaña del navegador en donde tiene abierto el tablero).

#### 🛡️ Validaciones

- [ ] Tablero accedible en ruta completa `C. Aux. / Gerente / Captura Promesas Ventas`.
- [ ] Tablero en `modo captura` de los registros de promesas según `política 8 de habilitación de modo captura`](#politica-8-celdas-editables), de otra manera el `modo sólo lectura`.
- [ ] `Manual de Editar Promesas` mostrado al cargar el tablero con el primer vendedor si hay `modo captura` habilitado.

> NOTA: Si accedes con un usuario con rol Gerente siempre tendrá acceso a la ayuda de `Manual de Editar Promesas`, en la zona de botones de acción en la esquina superior derecha.

### 5. Uso del modo captura por el Gerente

#### 💼 Operación

- [ ] 1. Una vez con el acceso al tablero por parte de un usuario con rol `Gerente` y configurado como tal*, debemos de deber al menos celdas editables y/o campo de comentario habilitado para la edición.
- [ ] 2. Identifica las celdas editables en cada registro con un estilo de borde punteado verde, en un conjunto de columnas `Etapa #` (siendo # el número de la etapa activa) son las de las columnas `Promesa` y `Mensual - Compromiso` de la tabla de promesas.
- [ ] 3. Los actores del Framework de Ventas son el Gerente (quien manipula el tablero) y el Vendedor (quien tiene los datos de promesas y compromisos). Teclea los datos deseados en las columnas con las siguientes indicaciones:

    1. **EDITA LA CELDA**: Presiona `ENTER` o `F2` o con `DOBLE CLIC`.
    2. **SIGUE LAS REGLAS DE LOS VALORES PERMITIDOS**:
        - En la tabla principal de los registros de promesas, los datos permitidos son los descritos en la [`Política 5`](#politica-5-valores-permitidos-en-modo-captura) de `Políticas Generales`.
        - Cuando ingresas un valor no permitido, el tablero muestra un mensaje en color rojo en la esquina superior izquierda: _**Edición de Registros de Promesas ... La cantidad debe ser un número entero entre $0 y $999,999,999**_.
    3. **CONFIRMAR EL VALOR CAPTURADO**: Para las celdas editables de la tabla de registros de promesas, presiona `ENTER` o cambia la selección por otro registro, verás un mensaje en color azul que indica _**Edición de Registros de Promesas ... Totales recalculados.**_ en la parte superior izquierda. Para confirmar la edición del comentario capturado, presiona `CTRL + ENTER` y en la misma ubicación del mensaje anterior aparece un mensaje _**COMENTARIO DE LA PROMESA ... El comentario ha sido editado.**_.
    4. **CANCELAR LA CAPTURA DE UN VALOR**: Para las celdas editables de la tabla de registros de promesas, presiona la tecla `ESC`, y la celda mostrará el valor anterior.
    5. **CANCELAR LA CAPTURA / DESCARTAR TODOS LOS CAMBIOS**: Da clic en el botón de acción `Actualizar tablero` para descartar todos los cambios y volver a cargar todos los valores anteriores, se te mostrará un mensaje de confirmación.

- [ ] 4. Una vez confirmada la información ingresada entre el Gerente y el Vendedor, el Gerente puede dar clic al botón de acción `Actualizar Promesas`, y una vez que el sistema haya guardado los cambios, te aparecerá un mensaje: `Actualización de promesas exitosa.`. Para comprobar si la actualización tuvo efecto, puedes cargar de nuevo al mismo vendedor y observar los valores iniciales del tablero.
- [ ] 5. Se volverá a "limpiar" todo el tablero como en la carga para que el Gerente seleccione otro vendedor para la actualización de sus promesas.

> \* Paso 1: Que en la BD esté registrado como un Gerente.

#### 🛡️ Validaciones

- [ ] `Modo captura` detectado en el tablero.
- [ ] Identificación de celda y campo editables.
- [ ] Manipulación del modo captura de acuerdo al paso 3.
- [ ] Confirmación de la actualización de promesas de un vendedor.
- [ ] Limpieza del tablero una vez actualizado las promesas para un vendedor.
- [ ] Ejemplo de descartar cambios devolviendo a los valores pasados.

## 📎 Observaciones adicionales

1. Se mejora la experiencia del usuario al cargar el loader, deshabilitar los botones de acción y el buscador de vendedores (cuando no se ha hecho nigún cambio) cuando se está cargando la información del tablero, evitando que el usuario hacer las opciones descritas en este documento.
2. Se hicieron mejora al buscador  de vendedores mostrando el mensaje interno de las excepciones atrapadas.
3. Se mejoró la seguridad con el nuevo método de propagación de datos sensibles.

> 🗓️ **Fecha de última modificación:** 2025-12-05
> 👤 **Erick López, Sergio Tostado**
> 🏷️ **Versión:** 1

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
