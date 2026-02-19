# 📦 Módulo: Cancelacion
#### 📁 **Código:** `Modules/Timbrador/Cancelacion`
#### 💻 **Menú:** Es un Backend actualmente sin acceso (están en desarrollo los callers)
---

#### Datos Generales
<details>
<summary> Actualizar el resto de la documentación / indicar en dónde está </summary>

## 📝 Descripción
Especificar.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir contacto   | Permite añadir un contacto especial     | Ventas       |
| Botón   | Modificar contacto   | Permite modificar un contacto especial     | Ventas       |
| Botón   | Eliminar contacto   | Permite eliminar un contacto especial     | Ventas       |

## 💼 Políticas Generales
- Todos los clientes nuevos deben tener el check tildado: Timbrar la factura del cliente
- El número de cliente, debe ser derivado de tabla de control de clientes noctuna, en donde se analiza en la noche y se asignan lugares disponibles de forma intermedia en catálogo.
- La zona de cobranza debe coincidir con el almacén del vendedor relacionado.

## 🧪 Casos de Prueba

### Capturar cliente
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

### Modificar cliente
#### 🛡️ Validaciones
- [ ] No se permite modificar el número de cliente
- [ ] Si existe el bloqueo de datos fiscales, no se deben liberar los campos:
    - Nombre cte. SAT
    - Código postal
    - RFC
    - Régimen fiscal
    - Uso CFDi

### Botones ABC de contactos
#### 🛡️ Validaciones
- [ ] El nombre del contacto es requerido

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-06-01
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 2

</details>


#### Pruebas
<details> <summary> Revisión 202602191256-IC </summary> 

## NO HACER CASO A ÉSTE, ES PARA EJEMPLIFICAR

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Pendiente | Cotización normal, 2 artículos, tramos y regular, sin exceder los descuentos | 202602181235-IC | |
| 1.1 | Revisado | Guardar un registro en ELCOTA, verificar que todos los datos sean correctos | | |

</details>
<details open> <summary> Revisión 202602181235-IC </summary>
  
## NO HACER CASO A ÉSTE, ES PARA EJEMPLIFICAR
  
| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Pendiente | Cotización normal, 2 artículos, tramos y regular, sin exceder los descuentos | 202602181235-IC | |
| 1.1 | Revisado | Guardar un registro en ELCOTA, verificar que todos los datos sean correctos | | |

</details>

<details> <summary> Acotaciones </summary>

| Estatus | Descripción |
|:---|:---:|
| Pendiente | No se ha iniciado la revisión |
| Iniciado | Revisión Iniciada |
| Error | Errores en la revisión (Revisar la sección de errores) |
| Completa | Revisión completada satisfactoriamente |

</details>


### Errores
<details> <summary> Revisión 202602181235-MH </summary>

## NO HACER CASO A ÉSTE, ES PARA EJEMPLIFICAR

| Error | Severidad | Pasos para reproducir | Comportamiento actual | Comportamiento esperado | Evidencia | Usuario contacto | Fecha corrección | Notas / comentarios |
|:---|:---:|:---|:---|:---|:---:|:---|:---:|:---|
| | | | | | `screenshots/error-001.png` | | | |

</details>

---
---
#### Otros ejemplos de control de QA
<details> <summary> Otros ejemplos de control de QA </summary>

## NO HACER CASO A ÉSTE, ES PARA EJEMPLIFICAR

## Checklist de Migración
- [x] Funcionalidades principales migradas
- [x] Datos persistentes correctos
- [ ] APIs responden igual que EXE
- [x] UI/UX similar
- [ ] Rendimiento aceptable
- [ ] Documentación actualizada

</details>
