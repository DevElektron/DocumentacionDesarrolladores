# 📦 Módulo: Altas Rápidas de Clientes...
#### 📁 **Código:** `Modules/Ventas/altasRapidasCtes`
#### 💻 **Menú:** Ventas > Altas rápidas de clientes
---
## 📝 Descripción
Éste módulo permite la captura de información de nuevos clientes desde mostrador. También posibilita la consulta y modificación de los datos de facturación del cliente para correcciones provistas del cliente de forma presencial.

---
## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir contacto   | Permite añadir un contacto especial     | Ventas       |
| Botón   | Modificar contacto   | Permite modificar un contacto especial     | Ventas       |
| Botón   | Eliminar contacto   | Permite eliminar un contacto especial     | Ventas       |

---
## 💼 Políticas Generales
- Todos los clientes nuevos deben tener el check tildado: Timbrar la factura del cliente
- El número de cliente, debe ser derivado de tabla de control de clientes noctuna, en donde se analiza en la noche y se asignan lugares disponibles de forma intermedia en catálogo.
- La zona de cobranza debe coincidir con el almacén del vendedor relacionado.

---
## 🧪 Casos de Prueba

### 1. Capturar cliente

#### 💼 Operación
- [ ] No se permite capturar un RFC que ya exista en tabla.

#### 🛡️ Validaciones
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Nombre cte. SAT
    - Código postal
    - RFC
    - Régimen fiscal
    - Uso CFDi
- [ ] Se debe seleccionar la clasificación de cliente Schneider. (Default: Ninguno)
---

### 2. Modificar cliente

#### 🛡️ Validaciones
- [ ] No se permite modificar el número de cliente
- [ ] Si existe el bloqueo de datos fiscales, no se deben liberar los campos:
    - Nombre cte. SAT
    - Código postal
    - RFC
    - Régimen fiscal
    - Uso CFDi
---

### 3. Botones ABC de contactos

#### 🛡️ Validaciones
- [ ] El nombre del contacto es requerido
---

## 📎 Observaciones adicionales
- Nanay.

---
> 🗓️ **Fecha de última modificación:** 2025-06-30
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 2
