# 📦 Módulo: Documentos y Anticipos...
#### 📁 **Código:** `/Modules/CxCobrar/DocumentosAnticipos`
#### 💻 **Menú:** CxC > Documentos y Anticipos [Ver en QA](http://192.168.2.16:1089/app/cxc/doc-ant-cxc)

## 📝 Descripción
Este módulo permite la captura de documentos del tipo Documentos por Cobrar y Anticipos. Asimismo, facilita la consulta y modificación de los registros existentes. Adicionalmente, cuenta con un servicio que permite activar una bandera para excluir un registro de los reportes de cobranza.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir      | Permite añadir un nuevo tipo de documento de naturaleza 1(documento por cobrar) o 4(anticipo) | Coordinadoras de crédito y cobranza del corporativo |
| Botón   | Modificar   | Permite  modificar la fecha de vencimiento y descripcion dependiendo de la naturtaleza del dcto | Coordinadoras de crédito y cobranza del corporativo |
| Botón   | Bandera    | Permite marcar un documento para evitar que sea considerado en reportes de cobranza | Coordinadoras de crédito y cobranza del corporativo |

## 💼 Políticas Generales
- Al momento de agragar un nuevo documento, solo los anticipos permiten ingresar una descripción
- El tipo de moneda dola solo será selecciobale para los clientes que pueden facturar en dolares.
- La fecha de vencimiento es calculada acorde al usuario seleccionado.
- El importe siempre es capturado en base de datos como moneda nacional.

## 🧪 Casos de Prueba

### Capturar documentos
#### 💼 Operación
- [ ] Los únicos tipos de documentos que se pueden capturar son Documentos por Cobrar y Anticipos.
#### 🛡️ Validaciones
- [ ] Debe capturarse el mínimo de información, requiriendo los campos:
    - Documento
    - Concepto
    - Referencia
    - Cliente
    - Tipo de moneda
    - importe
    - descripcion
- [ ] Se debe seleccionar un cliente para el calculo de la fecha de vencimiento
- [ ] Se debe seleccionar un documento para mostrar la fecha de vencimiento sugerida

### Modificar
#### 🛡️ Validaciones
- [ ] No se permite modificar los siguientes campos:
    - Documento
    - Concepto
    - Folio del documento
    - Fecha del documento
    - Referencia
    - Cliente
    - Tipo de moneda
    - Valor dólar actual
    - Importe
    - Saldo actual
- [ ] Si el usuario tiene la bandera para facturar dolares:
    - Puedes seleccionar el tipo de moneda dólar

### Botón No considerar documentos en reportes de cobranza
#### 🛡️ Validaciones
- [ ] Seleccionar un registro el cual quiera modificar

## 📎 Observaciones adicionales
- Observaciones adicionales, modos de prueba o ambientes específicos de uso.

> 🗓️ **Fecha de última modificación:** 2025-07-07
> 👤 **Erick López**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/02 | GP |Retorno|
|⏩| 2025/07/02 | IC |Avance|
