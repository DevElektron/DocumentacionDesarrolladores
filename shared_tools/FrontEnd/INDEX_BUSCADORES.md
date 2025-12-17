# 📜 Catálogo de `Buscadores` Generados

<details open>
<summary><strong>🎯 Objetivo:</strong></summary>

- Este documento funciona como índice técnico de los componentes buscador implementados en AdministraWeb.  
- Para cada componente se detalla:
 	- Nombre.
 	- Ubicación dentro del proyecto.
 	- Estructura de datos devuelta (campos).
 	- Parámetros utilizados para realizar búsquedas.
- El objetivo principal es centralizar la documentación de estos componentes para promover su reutilización, estandarizar su implementación y evitar la creación redundante de nuevos buscadores.

</details>

---

<details>
<summary><strong>📑 [Pedidos Buscador]</strong> <code>app-pedidos-buscador</code></summary>

- 🗂️ **Código:** `src\app\shared\ui\components\buscadores\pedidos-buscador\pedidos-buscador-modal`  
- 📋 **Tablas involucradas:** `ELPDC`
- 🧾 **Campos:** `PDC.*`,
- 📏 **Filtro de Búsqueda:**
 	- **Joins:** 
 		- `N/A`  
 	- **Where:**
   		- `PDC:LFOLIO = [lfolio] & PDC:NFOLIO = [nfolio]`

</details>

***

> 🗓️ **Fecha de última modificación:** 2025-12-13
> 👤 **Eduardo Navarro**
> 🏷️ **Versión:** 1