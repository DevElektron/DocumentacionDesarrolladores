# 📦 Módulo: Estadística Ventas Vendedores
#### 📁 **Código:** `/modules/vendedor/estadistica_ventas_vendedores`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/conauxiliares/vendedor/ventas-vendedor-ciudad)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción

Este módulo muestra las ventas por vendedor y ciudad de acuerdo a que si el usuario que inició sesión es un gerente o un vendedor, cambiando la distribución (layout) de las tablas y bloques de acuerdo al rol.

## 🔐 Seguridad

| Tipo UI      | Elemento                                   | Descripción                                      | Rol permitido v    |
|--------------|--------------------------------------------|--------------------------------------------------|--------------------|
| dashboard    | Mostrar módulo                             | Permite mostrar el módulo en el panel de inicio. | Gerente / Vendedor |
| ventana      | Mostrar opción                             | Permite mostrar la opción en el menú principal.  | Gerente / Vendedor |
| autocomplete | Autocomplete Zonas de Cobranza Disponibles | Permute buscar y seleccionar zonas de cobranza.  | Gerente            |

## 💼 Políticas Generales

1. Para hacer distinción de las vistas necesarias para el tablero, sólo se creó 2 roles: Gerente y Vendedor.
2. Si se necesitan más roles para la visualización de ambas vistas, se crearán o modificarán en el aplicativo Security, ej. Gerente Regional, Gerente de Zona, etcétera.
3. Sólo los gerentes (rol Gerente) puede ver y usar el autocomplete de Zonas de Cobranza Disponibles (arriba, esquina superior derecha), y visualizar la columna de `Tendencia Comisión` en la tabla de ventas por periodo.
4. Los vendedores sólo puede ver sus factores críticos y su calificación FCE, no la de otros. Mientras, los gerentes pueden ver todos los factores críticos y calificaciones de los vendedores que se visualizan en la tabla de ventas por periodo.
5. Sólo el rol Vendedor puede ver la tabla de top de ventas (tabla al fondo del tablero).
6. Para los usuario con NVEN perteneciente a un tipo de vendedor que no sea de tipo 'Vendedor' (ej. Facturista), se visualizará las ventas del periodo correspodiente al vendedor configurado en el almacén, ejemplo el NVEN 25, es un Facturista, por lo tanto, las ventas del periodo a visualizar serán del NVEN 501, que está figurado como vendedor del almacén 1.

## 🧪 Casos de Prueba

### Carga del tablero al iniciar sesión con AdministraWeb

#### 💼 Operación

- [ ] Para roles de Gerente y Vendedor. Al iniciar sesión, se visualizará en el inicio de la aplicación uno o varios tableros en la ruta /app/dashboard, entre los cuáles estará el tablero `Ventas por Vendedor y Ciudad`, para los usuarios que tengan el permiso `das-ventasVendedorCiudad` en la ruta de `Inicio`.

#### 🛡️ Validaciones

- [ ] Usuarios que tengan el permiso `das-ventasVendedorCiudad` verán el tablero indicado.
- [ ] Usuarios que no tengan el permiso `das-ventasVendedorCiudad` no verán el tablero indicado.

### Carga del tablero en la ruta del menú

#### 💼 Operación

- [ ] Para roles de Gerente y Vendedor. Si el usuario tiene permisos para la ruta `C. Aux. - Consultas Auxiliares Clientes / Vendedor - Vendedor / Ventas Vendedor Ciudad - Ventas por Vendedor y Ciudad`, se visualizará el tablero "Ventas por Vendedor y Ciudad".

#### 🛡️ Validaciones

- [ ] Usuarios que tengan el permiso en la ruta indicada verán el tablero indicado.
- [ ] Usuarios que no tengan el permiso en la ruta indicada no verán el tablero indicado.

### Diseño / Vista de Vendedor

#### 💼 Operación

- [ ] Para el rol `Vendedor`, se mostrará en el tablero los siguientes bloques del tablero (y en ese orden):

1. Tabla Ventas por Periodo sin columna final de Tendencia Comisión y sin fila de totales, con header del periodo guardado en tabla BD `ELCTRL` en la columna `Cuota`.
2. Tabla Factores Críticos y Calificación FCE.
3. Tablas Top Cobranza y Top Crédito (título = LÍNEAS SUBUTILIZADAS).
4. Tabla Top Ventas.

#### 🛡️ Validaciones

- [ ] Usuarios con el rol indicado verán todos los bloques indicados en el tablero.

### Diseño / Vista de Gerente.

#### 💼 Operación

- [ ] Para el rol `Gerente`, se mostrará en el tablero los siguientes bloques del tablero (y en ese orden):

1. Input de búsqueda de Zonas de Cobranza Disponibles.
2. Tabla Ventas por Periodo con columna final de Tendencia Comisión, con header del periodo guardado en tabla BD `ELCTRL` en la columna `Cuota`.
3. Fila final de Totales en Tabla Ventas por Periodo.
4. Tablas Top Cobranza y Top Crédito (título = INCREMENTOS DE LÍNEA).
5. Tabla Factores Críticos y Tarjeta Calificación FCE.

#### 🛡️ Validaciones

- [ ] Usuarios con el rol indicado verán todos los bloques indicados en el tablero.

### Eventos de Vista Gerente

#### 💼 Operación

- [ ] Para el rol `Gerente`, se podrán hacer las siguientes interacciones con el tablero:

1. Selección de Zona de Cobranza a visualizar en el Input de búsqueda de Zonas de Cobranza Disponibles, generando la información correspondiente a la Zona - Ciudad elegida en las tablas de Ventas por Periodo (filtro ciudad), Top Cobranza y Top Crédito (filtro zona).
2. Tabla de Ventas por Periodo tendrá seleccionada por defecto la primera fila seleccionada (color verde pastel), cargando la información de Factores Críticos de Éxito (FCE) del vendedor mostrado en la columna de `# | Vendedor`.
3. El usuario por dar clic o con las teclas flecha arriba / abajo para seleccionar la fila de Ventas por Periodo que contenga al vendedor del cual desea visualizar su FCE.
4. En todas las tablas se podrá resaltar la fila deseada ya sea con clic o con las teclas indicadas.

#### 🛡️ Validaciones

- [ ] Usuarios con el rol indicado podrá realizar todos los eventos listados en el tablero.

### Eventos de Vista Vendedor

#### 💼 Operación

- [ ] Para el rol `Vendedor`, se podrán hacer las siguientes interacciones con el tablero:

1. En todas las tablas se podrá resaltar la fila deseada (con color verde pastel) ya sea con clic o con las teclas indicadas.

#### 🛡️ Validaciones

- [ ] Usuarios con el rol indicado podrá realizar todos los eventos listados en el tablero.

## 📎 Observaciones adicionales

- En la información mostrada en el tablero migrado del ERP ElektronSQL con Clarion, hay ciertas celdas cuya información es un cálculo, y por la precisión de las tecnologías .NET algunas cantidades difieren por un centavo en su redondeo del tipo de dato decimal a 2 decimales, siendo aleatorios y muy escasos los casos al comparar la información original.
- En la versión original del tablero viene 2 filtros, uno para la zona y otro para la ciudad. En el nuevo tablero se unificaron los filtros con el autocomplete de Zonas de Cobranza Disponible, siendo la selección de una opción el filtro que da a las tablas la zona y ciudad.
- En la versión original del tablero tiene 2 filtros, Año y Mes, por común acuerdo de los tableros de la ruta `C. Aux. - Consultas Auxiliares Clientes / Vendedor - Vendedor` se eliminaron los filtros indicados, tomando estos datos de la tabla en la BD `ELCTRL`, columnas `CIE_ANOACTUAL` y `CIE_MESACTUAL`.
- En este tablero sólo está como parte de la seguridad el acceso al tablero, ya que en su programación la única variante que define si se muestra una vista u la otra es el NVEN del usuario que inició sesión, buscando a partir de ese dato si es gerente o no.

> 🗓️ **Fecha de última modificación:** 2026-02-05
> 👤 **Sergio Tostado, Daniel Salazar**
> 🏷️ **Versión:** 3

---

## Comunicaciones

|Dir |Fecha       |Firma |Comentario                                                                                                                                                                       |
|----|------------|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ⏩ | 2025/12/17 | JS   | [ADD] Modificación al autocomplete de Zonas de Cobranza para que arroje todas las zonas, a excepción de este tablero.                                                           |
| ⏩ | 2026/02/05 | ST   | [FIX] Carga de ventas x periodo de vendedor configurado en el almacén del usuario, de vendedores con tipo de vendedor no igual a 'Vendedor'.                                    |
| ⏩ | 2026/02/05 | ST   | [FIX] Implementación de Loader, Limpieza código, mejoras a estilos, carga en paralelo de información de grids del tablero, doble carga en zona de cobranza seleccionada.        |
