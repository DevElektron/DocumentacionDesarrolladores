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

***

> 🗓️ **Fecha de última modificación:** 2025-08-01
> 👤 **Eduardo Navarro**
> 🏷️ **Versión:** 1