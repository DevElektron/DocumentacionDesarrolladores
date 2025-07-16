# 📦 Módulo: Depositos por Confirmar de Clientes
#### 📁 **Código:** `Modules/ConAux/Clientes/DepositosPendientes`
#### 💻 **Menú:** C. Aux. > Clientes > Depositos Pendientes [Ver en QA](http://192.168.2.16:1089/app/conauxiliares/clientes/depositospendientes)

## 📝 Descripción
Éste módulo permite la captura del deposito pendiente por confirmar de Clientes, hace la busqueda en el estado de cuenta del banco que se descarga todos los dias al 
Administra y si es localizado aun como "Pendiente" lo registra marcandolo como "Aplicado" si este coincide con la refencia.
Si la Referencia capturada ya esta previamente como "Aplicado" le informa al usuario.
Si la Referencia capturada no es localizada tambien le informa al usuario.

La referencia de la busqueda es la siguiente:
(Fecha Deposito + Cuenta Bancaria + Importe) y en el estatus del estado de cuenta banco este aun como "Pendiente" por conciliar.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Ventana | Captura Información | Ingresa los Datos requeridos | Clientes       |

## 💼 Políticas Generales
- Solo debera conciliar el deposito pendiente por confirmar si es localizada la referencia en el estado de cuenta bancario y se encuentre en "Pendiente" por Aplicar
  debera marcalo posteriormente como "Aplicado".
  
## ◉ TABLAS QUE OCUPA EL MODULO.
● ELCTE (Para obtener el nombre)␣␣  
● PCCTB (Lista de Cuentas bancarias)
● ELUSD (Precio tasa de cambio dolar de la fecha deposito bancario)
● ELDPI (Estado de Cuenta Bancario que se baja al administra todos los dias)
● ELCXC (Tabla donde se guardan los anticipos el campo CDOC por default su identificación es "AN" y la serie del campo LFOLIO es "APA")
         ∙ Para determinar que se guarde en esta tabla ELCXC en la tabla ELDPI campo numerico BND_SBC debe estar en 1.
● ELDPC (Se guarda en esta tabla cuando el proceso es exitoso con el proximo folio que le prosigue)
         ∙ Si seleccionan un banco que maneja en dolares, se guarda en la tabla ELCXC el precio del dolar con fecha deposito y
           el importe del anticipo se multiplica el valor del dolar por el importe del deposito.
         ∙ En la tabla ELDPI en el campo ESTADO se marca como "Aplicado" para que no sea considerado como Pendiente por Confirmar, 
           la fecha del campo FCIDENTIFICA se graba la fecha que se identifico el deposito.
         ∙ Para relacionar la tabla ELDPI con la tabla ELDPC una vez que el deposito fue identificado con exito, en la tabla ELDPC en 
           el campo CONSECUTIVO_DPI se guarda el numero de folio de la tabla ELDPI que se llama el campo CONSECUTIVO.

## 🧪 Casos de Prueba

### Capturar Deposito Pendiente por Confirmar
#### 💼 Operación
- [ ] Seleccionar el cliente o nombre del cliente para definir el cliente que hizo el deposito (Requerido).
- [ ] Selecccionar la fecha del deposito (Requerido).
- [ ] Seleccionar la cuenta bancaria o nombre del banco donde se realizo el deposito (Requerido).
- [ ] Ingresar el importe del deposito con 2 decimales (Requerido).
- [ ] Ingresar una descripción u observación del deposito. (Opcional)
- [ ] Hacer Click en el boton guardar para validar los datos capturados, si pasa la validación concilia el deposito capturado.
- [ ] Hacer Click en el boton Cancelar para cerrar la Ventana.

#### 🛡️ Validaciones de Captura del Deposito por Confirmar
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Cliente.
    - Fecha deposito.
    - Cuenta Bancaria.
    - Importe Total del Deposito.

### Boton Guardar
#### 🛡️ Validaciones
- Si el Cliente es Mostrador se le enviara mensaje al usuario que no podra capturar deposito para estos numeros de cliente.
- Si la fecha deposito es mayor a la fecha actual indicara que seleccione la fecha correcta.
- En el estado de cuenta bancario hay una variable que indica que si no esta "Salvo buen Cobro" se guarda como anticipo

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-07-16
> 👤 **Ernesto Martin Hernandez Zuñiga**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|Capturar Deposito|
|⏪| 0000/00/00 |   | |
