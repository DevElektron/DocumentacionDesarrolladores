# 📜 Catálogo de `Services` Generados

<details open>
<summary><strong>🎯 Objetivo:</strong></summary>

- Este documento funciona como índice técnico de los servicios implementados en AdministraWeb.  
- Para cada componente se detalla:

    1. Nombre.
    2. Ubicación dentro del proyecto.
    3. Descripción de funcionamiento general.
    4. Servicios proporcionados por este.

- El objetivo principal es centralizar la documentación de estos componentes para promover su reutilización, estandarizar su implementación y evitar la creación redundante de nuevos servicios.

</details>

---

<details>
<summary><strong>📑 [Dialog Service]</strong>

- 🗂️ **Código:** `src\app\shared\services\dialog.service.ts`
- 📑 **Descripción**: Servicio manejador de alertas.
- 🧾 **Servicios:**  

    1. `confirm:` Muestra un diálogo de confirmación.
    2. `error:` Muestra un diálogo de error, con un parámetro opcional `err?` para enviar el objeto de la excepción capturada para complementar el mensaje y hacer un mejor inicio de la depuración.
    3. `success:` Muestra un diálogo de operación correcta.
    4. `warning:` Muestra un diálogo de advertencia o aviso.
    5. `observations:` Muestra un diálogo con observaciones.

</details>

---

> 🗓️ **Fecha de última modificación:** 2026-01-05
> 👤 **Eduardo Navarro, Sergio Tostado**
> 🏷️ **Versión:** 2
