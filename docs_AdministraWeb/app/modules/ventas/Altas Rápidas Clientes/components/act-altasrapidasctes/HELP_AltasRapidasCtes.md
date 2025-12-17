# 📦 Módulo: Altas Rápidas de Clientes...
#### 📁 **Código:** `Modules/Ventas/altasRapidasCtes`
#### 💻 **Menú:** Ventas > Altas rápidas de clientes [Ver en QA](http://192.168.2.16:1089/app/ventas/altasrapidasctes)

## 📝 Descripción
Éste módulo permite la captura de información de nuevos clientes desde mostrador. También posibilita la consulta y modificación de los datos de facturación del cliente para correcciones provistas del cliente de forma presencial.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir contacto   | Permite añadir un contacto especial     | Ventas       |
| Botón   | Modificar contacto   | Permite modificar un contacto especial     | Ventas       |
| Botón   | Eliminar contacto   | Permite eliminar un contacto especial     | Ventas       |

## 💼 Políticas Generales
- Todos los clientes nuevos deben tener el check tildado: Timbrar la factura del cliente
- El número de cliente se debe de generar al momento de realizar el registro, este se obtiene sumando 1 al ultimo número de cliente generado
- Se auto asigna el vendedor que este asignado dentro de la sesión y el vendedor de truper se verifica que el vendedor del almacen sea el mismo, si no se toma el del almacén.

## 🧪 Casos de Prueba

### Capturar cliente
#### 💼 Operación
- [ ] No se permite capturar un RFC que ya exista en tabla.
- [ ] No se permite capturar un CURP que ya exista en tabla.
#### 🛡️ Validaciones
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Nombre cte. SAT
    - Domicilio
    - Estado
    - Código postal
    - RFC
    - Clasificación
    - Método de pago
- [ ] Se debe seleccionar la clasificación de cliente Schneider. (Default: Ninguno)

### Modificar cliente
#### 🛡️ Validaciones
- [ ] No se permite modificar el número de cliente
- [ ] Si existe el bloqueo de datos fiscales (el cliente ya tiene facturas generadas y timbradas), no se deben liberar los campos:
    - Nombre cte. SAT
    - Código postal
    - RFC
    - Régimen fiscal

### Botones ABC de contactos
#### 🛡️ Validaciones
- [ ] El nombre del contacto es requerido

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-12-15
> 👤 **Daniel Salazar**
> 🏷️ **Versión:** 4

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|Capturar cliente|
|⏪| 2025/06/15 | GP |En la inserción permite guardar sin código postal capturado|
|⏩| 2025/06/20 | IC |Corregido|
|⏪| 2025/06/22 | GP |No es cierto|
|⏩| 2025/06/24 | IC |Que si!!!|
|⏪| 2025/06/25 | GP |Que no!!!|
