# 📦 Módulo: 
#### 📁 **Código:** `Modules/CxC/ConsultaAuxClientes/HistoricoVentasClientes`
#### 💻 **Menú:** CXC > Consulta Aux Clientes > Histórico Ventas >  [Ver en QA](http://192.168.2.16:1089/app/cxc/auxiliarclientes/historicoventas)

## 📝 Descripción
Éste módulo permite consultar el historico de ventas de los ultimos 12 meses del cliente seleccionado. Además de permitir cambiar el estatus para permitir o negar la promoción de vales de despensa por cobranza y generar un reporte con la información generada en el modulo.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido  |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Imprimir Historico | Genera un reporte con los datos del historico de ventas del cliente |        |
| Botón   | Autorizar Vales Despensa | Muestra/modifica el estatus de autorización de vales de despensa |        |

## 💼 Políticas Generales
- Para generar un reporte es necesario primero seleccionar un cliente.

## 🧪 Casos de Prueba

### Autorizar Vales Despensa
#### 🛡️ Validaciones
- [ ] Valida el valor del campo BND_AUTVALESCOMPRAS en la tabla ELCTE para determinar el estatus de la autorización de vales, si es 1 es igual a verdadero (color verde) si es 0 es igual a falso (color rojo).
- [ ] Dependiendo del color que tenga el botón al precionarlo realiza un update a los campos BND_AUTVALESCOMPRAS, IDUSUARIO_AUTVALESCOMPRAS, HRAUTVALESCOMPRAS de la tabla ELCTE:
    - Botón en verde (BND_AUTVALESCOMPRAS = 1/true):
        - Color Botón: Rojo
        - BND_AUTVALESCOMPRAS: 0 (false)
        - IDUSUARIO_AUTVALESCOMPRAS: Usuario loggeado
        - HRAUTVALESCOMPRAS: Hora actual al precionar el botón
    - Botón en verde (BND_AUTVALESCOMPRAS = 0/false):
        - Color Botón: Verde
        - BND_AUTVALESCOMPRAS: 1 (true)
        - IDUSUARIO_AUTVALESCOMPRAS: limpia el campo
        - HRAUTVALESCOMPRAS: 0

## 📎 Observaciones adicionales
- 

> 🗓️ **Fecha de última modificación:** 2025-07-07
> 👤 **Eduardo Navarro**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
|⏪| 2025/07/07 | EN |Entrega del modulo|
|⏩|            |     |                             |
