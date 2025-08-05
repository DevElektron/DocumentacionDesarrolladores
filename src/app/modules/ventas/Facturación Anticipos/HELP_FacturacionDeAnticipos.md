# 📦 Módulo: 
#### 📁 **Código:** `Modules/Ventas/facturacionAnticipos`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ventas/facturacionanticipos)

## 📝 Descripción
Éste módulo permite la consulta de las facturas de anticipos así como sus detalles y observaciones.
También, permite la captura de anticipos. 

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                         | Rol permitido |
|---------|-------------------|-------------------------------------|----------------|
| Botón   | Añadir anticipo   | Abre ventana de captura de anticipo |                |

## 💼 Políticas Generales
- 1 Para poder capturar, el usuario debe tener un almacén asignado
- 2 El almacén se asignará automáticamente dependiendo el almacén asignado al usuario y no se podrá modificar
- 3 La fecha será la actual y no se podrá modificar
- 4 La caja se asignará automáticamente dependiendo el almacén asignado al usuario y no se podrá modificar
- 5 Uso de Cfdi, forma de pago, método de pago y tipo de moneda se deben llenar con información del cliente seleccionado
- 6 Los campos con * son obligatorios y de no tener el valor necesario, no se podrá presionar Guardar

## 🧪 Casos de Prueba

### Añadir
#### 💼 Operación
- [ ] No se permite 
#### 🛡️ Validaciones
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - 
    - 
    - 
- [ ] Se debe seleccionar

### Modificar
#### 🛡️ Validaciones
- [ ] No se permite
- [ ] Si existe x -> se deben asegurar:
    - 
    - 
    - 
    - 
    - 

### Botón A
#### 🛡️ Validaciones
- [ ] xxx

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-07-02
> 👤 **Tu nombre**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/02 | GP |Retorno|
|⏩| 2025/07/02 | IC |Avance|



