# 📦 Módulo: Datos de Venta
#### 📁 **Código:** `/modules/catalogos/generales/clientes/datos-venta`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/catalogos/generales/clientes/datosventas)

## 📝 Descripción
Éste módulo permite realizar la captura (Edición), de la información base de un cliente para el apartado de ventas.
Permite la captura de las opciones de venta, archivos de listas de precios, contactos, subclientes y proveedores

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir contacto      | Permite agregar nuevos contactos al cliente      |        |
| Botón   | Modificar datos   | Permite editar la información del cliente      |        |
| Botón   | 	Eliminar contacto    | Permite eliminar contactos del cliente      |        |

## 💼 Políticas Generales
- 1. Al modificar un cliente, el campo ClasificaCob siempre se actualiza al estatus "B".
- 2. Se realiza automáticamente el reajuste de nombre y domicilio del cliente.
- 3. Al modificar, se generan las referencias bancarias para Bancomer y Banamex, excepto cuando se modifica información de ciudad.
- 4. Si el cliente tiene seleccionada la opción de archivo CSV en lista de precios, se obliga a capturar en celectronicocom.
- 5. Cuando está marcada la opción de archivo CSV, se muestra un campo adicional (envlistprecruta) para seleccionar el archivo.
- 6. Si el usuario no tiene configurado el campo de importancia, se asigna por defecto "1A".
- 7. Si en la configuración (letra A) Grl_TipoDescto_Cte es igual a 1, se bloquea la política y se establece en 0.

## 🧪 Casos de Prueba

### 1. Modificación de Cliente

#### 💼 Operación

* [ ] Se ejecutan automáticamente los reajustes de nombre y domicilio.
* [ ] Se actualiza el campo ClasificaCob a estatus "B".
* [ ] Se generan referencias bancarias para Bancomer y Banamex.
* [ ] Se validan las configuraciones de lista de precios y archivos CSV.

#### 🛡️ Validaciones

* [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Código de cliente
    - Nombre/Razón Social
    - Domicilio fiscal completo
    - Régimen fiscal
    - Método de pago
    - Uso de CFDI
* [ ] Se debe seleccionar una lista de precios válida
* [ ] Si se selecciona lista de precios con archivo CSV, se deben completar:
    - Ruta del archivo CSV (envlistprecruta)
    - Configuración en celectronicocom
* [ ] Las referencias bancarias no se generan cuando solo se modifica información de ciudad

### 2. Configuración de Lista de Precios

#### 🛡️ Validaciones

* [ ] No se permite guardar sin seleccionar una lista de precios
* [ ] Si existe lista de precios con archivo CSV → se deben asegurar:
    - El campo envlistprecruta no esté vacío
    - El archivo especificado exista en la ruta indicada
    - Se haya configurado correctamente en celectronicocom
    - El formato del archivo CSV sea válido
    - Los precios en el CSV sean numéricos y positivos

### 2. Gestión de contactos

#### 🛡️ Validaciones

* [ ] Los teléfonos deben tener formato válido (10 dígitos)
* [ ] Cada contacto debe tener al menos:
    - Nombre Completo
    - Teléfono
    - Tipo de teléfono

## 📎 Observaciones adicionales

#### Ambientes de prueba específicos:
- Validar que las referencias bancarias se generen correctamente
- Probar la funcionalidad con listas de precios con y sin archivo CSV
- Verificar que los reajustes de nombre/domicilio funcionen adecuadamente

> 🗓️ **Fecha de última modificación:** 2026-01-23
> 👤 **Jose Daniel Salazar Briseño**
> 🏷️ **Versión:** 1
