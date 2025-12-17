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
| Botón   | Bandera    | Permite activar/desactivar la bandera de un documento para incluir o excluir el registro de los reportes de cobranza mediante la modificación de un campo específico en base de datos | Coordinadoras de crédito y cobranza del corporativo |

## 💼 Políticas Generales
- Solo se pueden capturar documentos de naturaleza 1 (Documentos por Cobrar) y naturaleza 4 (Anticipos)
- Al momento de agregar un nuevo documento, solo los anticipos permiten ingresar una descripción
- El tipo de moneda dólar solo será seleccionable para los clientes que pueden facturar en dólares
- La fecha de vencimiento se calcula automáticamente sumando los días de crédito del cliente (configurados en el catálogo de clientes, sección Datos de Crédito) a la fecha del documento
- El importe siempre es capturado en base de datos como moneda nacional
- El campo folio del documento se compone de: lfolio + nfolio de la tabla ELFOF
- La tabla ELFOF se relaciona mediante el campo cfolio con la tabla de tipos de documentos ELDCC

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
#### 💼 Operación
- [ ] Permite activar/desactivar una bandera que controla si el documento será considerado en reportes de cobranza
#### 🛡️ Validaciones
- [ ] Se debe seleccionar un registro de la tabla principal antes de presionar el botón
- [ ] Al activar la bandera, el documento será excluido de los reportes de cobranza
- [ ] Cuando la bandera está activa, se muestra un punto de color rojo junto al tipo de documento como indicador visualles

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
