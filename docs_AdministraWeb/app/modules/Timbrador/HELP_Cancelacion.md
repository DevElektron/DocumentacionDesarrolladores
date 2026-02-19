# 📦 Módulo: Cancelacion
#### 📁 **Código:** `Modules/Timbrador/Cancelacion`
#### 💻 **Menú:** Es un Backend actualmente sin acceso (están en desarrollo los callers)
---

#### Datos Generales
<details>
<summary> Ver aquí la documentación </summary>

## 📝 Descripción
Backend de cancelación directa de documentos CFDi en el SAT, a través del API Rest de CEPDI.

## 🔐 Seguridad
|Tipo UI|Elemento|Descripción|Rol permitido|
|:---|:---|:---|:---|
|Llamada directa|Cancelación|Ejecuta la cancelación del documento especificado|Heredado|

## 💼 Políticas Generales
- El documento a cancelar, debe estar timbrado y vigente en las tablas correspondientes.

## 🧪 Casos de Prueba

#### 💼 Operación
- [ ] No se permite cancelar un documento previamente cancelado.

#### 🛡️ Validaciones
- [ ] Los datos requeridos para ejecutar cualquier cancelación en el SAT, son:
	- UUID
	- RFC del receptor (puede ser genérico)
	- Total del documento

#### Cancelar nota de abono por aplicación de factura de anticipo a factura de venta.
1. Se hace factura de ANTICIPO
2. Se hace factura de VENTA y se relaciona el anticipo anterior
	Ésta operación, genera una nota de abono relacionada a la factura de VENTA para equilibrar los movimientos
3. Se cancela la factura de VENTA
   ---> Aquí estamos, al cancelar la fac. de venta, se debe cancelar la NA relacionada (cancelación del presente documento)

#### Cancelación de factura de activo
1. Se elabora una factura de activo.
2. Se cancela la factura de activo.

#### Cancelación de pago de cliente
1. Se captura un pago de cliente.
2. Se cancela el pago de cliente.

## 📎 Observaciones adicionales
- Existen 2 ambientes proporcionados por CEPDI, uno demo y otro productivo.
	- El ambiente demo, responde siempre lo mismo, para cambiar el tipo de respuesta recibida, hay que contactar al personal de CEPDI para solicitarlo.

- Existe un manual de consumo del API Rest en el presente repositorio, en la siguiente liga: [WSE_Manual_CancelaCFDi_API_Rest.pdf](./WSE_Manual_CancelaCFDi_API_Rest.pdf)

> 🗓️ **Fecha de última modificación:** 2026-02-19
> 👤 **Ignacio Carranza**
> 🏷️ **Versión:** 1

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
