# 📦 Módulo: Cargos y Abonos a Doctos
#### 📁 **Código:** `/Modules/CxCobrar/CargosAbonosDoctos`
#### 💻 **Menú:** CxC > Cargos y Abonos a Doctos [Ver en QA](http://192.168.2.16:1089/app/cxc/car-abo-cxc)

## 📝 Descripción
Este módulo permite consultar los documentos de afectación que impactan (cargan o abonan) al importe de los documentos del módulo de Documentos y Anticipos. Incluye funcionalidades para registrar y modificar documentos de afectación mediante un modal de captura/edición. Los documentos de afectación pueden ser de naturaleza 2 (cargos) o 3 (abonos), excluyendo documentos de tipo pago.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Registrar      | Permite registrar nuevos documentos de afectación de naturaleza 2 (cargos) o 3 (abonos) | Coordinadoras de crédito y cobranza del corporativo |
| Botón   | Modificar   | Permite modificar documentos de afectación existentes (solo referencia y fecha de depósito) | Coordinadoras de crédito y cobranza del corporativo |
| Modal   | Captura/Edición    | Formulario para registro y modificación de documentos de afectación | Coordinadoras de crédito y cobranza del corporativo |

## 💼 Políticas Generales
- Los documentos de afectación pueden ser de naturaleza 2 (cargos) o 3 (abonos), excluyendo documentos de tipo pago
- El formulario de registro tiene 7 campos para cargos y 8 campos para anticipos
- Solo se pueden seleccionar documentos con saldo mayor a 0 que no sean anticipos
- El importe por aplicar no puede superar el saldo del documento, excepto para documentos de tipo cargo
- La suma total de importes por aplicar no debe superar el importe total del documento de afectación
- Si el importe por aplicar es mayor a la suma de importes, se puede generar un documento de anticipo con el sobrante
- Los cargos suman al saldo actual, los abonos restan del saldo actual
- Se generan tantos documentos de afectación como documentos afectados existan en la tabla

## 🧪 Casos de Prueba

### Registrar documentos de afectación
#### 💼 Operación
- [ ] Los únicos tipos de documentos que se pueden registrar son de naturaleza 2 (cargos) o 3 (abonos)
- [ ] No se permite registrar documentos de tipo pago dentro de la naturaleza 3
#### 🛡️ Validaciones
- [ ] Debe capturarse la información requerida según el tipo de documento:
    - Para cargos: 7 campos (Documento, Concepto, Folio, Fecha del documento, Referencia, Cliente, Importe)
    - Para anticipos: 8 campos (incluye campo adicional Fecha de depósito)
- [ ] Se debe seleccionar un cliente mediante nombre o número de cliente
- [ ] El folio del documento se autocompleta al seleccionar el tipo de documento
- [ ] Solo se pueden agregar documentos a la tabla si hay un cliente seleccionado

### Tabla de documentos con saldos pendientes
#### 🛡️ Validaciones
- [ ] El autocompletable Serie/Folio busca documentos con saldo mayor a 0 que no sean anticipos
- [ ] El importe por aplicar no puede superar el saldo del documento (excepto para cargos)
- [ ] Al presionar tabulador en importe por aplicar con documento seleccionado correctamente, se genera una nueva fila
- [ ] Es posible navegar entre autocompletables usando las flechas del teclado
- [ ] La suma total de importes por aplicar no debe superar el importe total
- [ ] No se pueden guardar registros con renglones vacíos
- [ ] Se puede eliminar filas específicas mediante el botón de basurero al final de cada fila

### Modificar documentos de afectación
#### 🛡️ Validaciones
- [ ] Se debe seleccionar un registro de la tabla principal
- [ ] El modal se abre en modo edición con un solo registro en la tabla
- [ ] Todos los campos permanecen bloqueados excepto:
    - Referencia
    - Fecha de depósito
- [ ] Al guardar se actualiza la tabla principal y se cierra el modal

### Funcionalidades adicionales
#### 🛡️ Validaciones
- [ ] Si el importe por aplicar es mayor a la suma de importes, el sistema pregunta si generar un anticipo con el sobrante
- [ ] Los cargos incrementan el saldo actual del documento
- [ ] Los abonos decrementan el saldo actual del documento
- [ ] Se generan tantos documentos de afectación como documentos afectados en la tabla

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-07-17
> 👤 **Erick López**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/02 | GP |Retorno|
|⏩| 2025/07/02 | IC |Avance|
