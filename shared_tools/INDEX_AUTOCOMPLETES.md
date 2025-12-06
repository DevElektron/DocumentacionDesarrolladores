# 📜 Catálogo de `AutoCompletes` Generados

<details open>
<summary><strong>🎯 Objetivo:</strong></summary>

- Este documento funciona como índice técnico de los componentes autocomplete implementados en AdministraWeb.  
- Para cada componente se detalla:
 	- Nombre.
 	- Ubicación dentro del proyecto.
 	- Estructura de datos devuelta (campos).
 	- Parámetros utilizados para realizar búsquedas.
- El objetivo principal es centralizar la documentación de estos componentes para promover su reutilización, estandarizar su implementación y evitar la creación redundante de nuevos autocompletes.

</details>

---

<details>
<summary><strong>📑 Vendedores<strong> <code>app-vendedor-autocomplete</code></summary>

- 🗂️ **Código:** `src\app\shared\ui\autocompleters\vendedor-autocomplete`  
- 📋 **Tablas involucradas:** `ELVEN`, `ELGTE`  
- 🧾 **Contenido:**
        `VEN:NVEN`, 
        `VEN:NOMBRE`, 
        `VEN:DOMICILIO`, 
        `VEN:COLONIA`, 
        `VEN:CIUDAD`, 
        `VEN:NEDO`, 
        `VEN:CPOSTAL`, 
        `VEN:TELEFONOS`, 
        `VEN:TIPO_VEND`, 
        `VEN:CELECTRONICO`, 
        `VEN:PRC_COMISION_FIJO`, 
        `VEN:NALM`, 
        `VEN:PRC_COM1`, 
        `VEN:PRC_COM2`, 
        `VEN:PRC_COMN`, 
        `VEN:PRC_COMCN`, 
        `VEN:NDIASANT`, 
        `VEN:FAC_DESCTO_GRUPO`, 
        `VEN:TIPOART`, 
        `VEN:PRES_ENERO`, 
        `VEN:PRES_FEBRERO`, 
        `VEN:PRES_MARZO`, 
        `VEN:PRES_ABRIL`, 
        `VEN:PRES_MAYO`, 
        `VEN:PRES_JUNIO`, 
        `VEN:PRES_JULIO`, 
        `VEN:PRES_AGOSTO`, 
        `VEN:PRES_SEPTIEMBRE`, 
        `VEN:PRES_OCTUBRE`, 
        `VEN:PRES_NOVIEMBRE`, 
        `VEN:PRES_DICIEMBRE`, 
        `VEN:PRC_PROCOM`, 
        `VEN:DISPONIBLE`, 
        `VEN:NCAJ`, 
        `VEN:UBICACION`, 
        `VEN:ULT_ATT`, 
        `VEN:TURNO_ID`, 
        `VEN:Bnd_Puede_Cancelar`, 
        `VEN:Clasif_Vend`, 
        `VEN:FcIngreso`, 
        `VEN:FcBaja`, 
        `VEN:Tproyecto`
- 📏 **Filtro de Búsqueda:**
    - `VEN:FcBaja = 0`
    - **Modo Gerente**: Si se le parametriza `true`, devuelve todos los vendedores cuyo Gerente es el NVEN (@NVEN_GTE) del usuario que inició sesión, mediante los almacenes a cargo de dicho usuario.
        - `AND ven.NALM in (
                SELECT
                    NALM
                FROM
                    ELGTE gte
                WHERE
                    gte.NVEN = @NVEN_GTE
            )
            AND NOT EXISTS (
                SELECT
                    1
                FROM
                    ELGTE gteV
                WHERE
                    gteV.NVEN = ven.NVEN
            )`
<i>NOTA: Este autocomplete tiene método de limpiar valores (resetar).</i>
</details>

---

<details>
<summary><strong>📑 Selección de facturas<strong> <code>seleccionde-factura-autocomplete</code></summary>

- 🗂️ **Código:** `src\app\shared\ui\autocompleters\seleccionde-factura-autocomplete`  
- 📋 **Tablas involucradas:** `ELFAC`, `ELCTE` & `ELALM`  
- 🧾 **Contenido:** `FAC:LFolio`, `FAC:NFolio`, `FAC:FcFactura`, `FAC:Estado`, `FAC:NCte`, `CTE:Nombre`, `FAC:ImpTotal`, `FAC:NAlm`, `ALM:Descripcion`  
- 📏 **Filtro de Búsqueda:**
 	- **Joins:** 
 		- `FAC:NCte = CTE:NCte && FAC:NAlm = ALM:NAlm`

</details>

---

<details>
<summary><strong>📑 Doctos. por Cobrar Pendientes Saldar</strong> <code>dxc-pendienetes-saldar-autocomplete</code></summary>

- 🗂️ **Código:** `src\app\shared\ui\autocompleters\dxc-pendienetes-saldar-autocomplete`  
- 📋 **Tablas involucradas:** `ELFAC`, `ELDCC` & `ELCCC`  
- 🧾 **Contenido:** `CXC:CDoc`, `DCC:Descripcion`, `CXC:CConcepto`, `CCC:Descripcion`, `CXC:LFolio`, `CXC:NFolio`, `CXC:FcDoc`, `CXC:Referencia`, `CXC:Importe`, `CXC:Saldo`  
- 📏 **Filtro de Búsqueda:**
 	- **Joins:** 
 		- `CXC:CDoc = DCC:CDoc && CXC:CConcepto = CCC:CCOncepto`  
 	- **Where:**
   		- **Módulo Cargos y Abonos:**
     		- `CXC:Saldo > 0 && CXC:CDoc NOT IN ([Doctos. Excluidos]) && CXC:NCte = [NumCliente]`
    	- **Módulo Solicitudes de Descuento Notas de Abono:**
       - `CXC:Saldo > 0 && DCC:Naturaleza = 1 && CXC:NCte = [NumCliente]`

</details>

---

<details>
<summary><strong>📑 Zonas de Cobranza Disponibles para Gerentes</strong> <code>zonas-cobranza-disponibles-autocomplete</code></summary>

- 🗂️ **Código:** `src\app\shared\ui\autocompleters\zonas-cobranza-disponibles-autocomplete`  
- 📋 **Tablas involucradas:** `ELGTE` & `ELCZO`
- 🧾 **Contenido:** `GTE:NCZO`, `CZO:DESCRIPCION`, `GTE:CIUDAD`
- 📏 **Filtro de Búsqueda:**
 	- **Joins:** 
 		- `GTE:NCZO = CZO:NCZO`  
 	- **Where:**
   		- **Número de Vendedor asociado al Gerente**
     		- `GTE:NVEN = [NVen]`

</details>

***

> 🗓️ **Fecha de última modificación:** 2025-08-04
> 👤 **Eduardo Navarro, Sergio Tostado**
> 🏷️ **Versión:** 2
