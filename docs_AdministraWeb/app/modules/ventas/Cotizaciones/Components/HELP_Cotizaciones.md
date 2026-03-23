# 📦 Módulo: Cotizaciones
#### 📁 **Código:** `Modules/Ventas/Cotizaciones`
#### 💻 **Menú:** Ventas > Cotizaciones [Ver en QA](http://192.168.2.16:1089/app/ventas/cotizaciones)
---
#### Pruebas
<details> <summary> Revisión 202601010838-IC </summary> 

## NO HACER CASO A ÉSTE, ES PARA EJEMPLIFICAR

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Pendiente | Cotización normal, 2 artículos, tramos y regular, sin exceder los descuentos | 202602181235-IC | |
| 1.1 | Revisado | Guardar un registro en ELCOTA, verificar que todos los datos sean correctos | | |

</details>
<details> <summary> Revisión 202602181235-IC </summary>

| Pruebas | Estatus | Descripción | Revisión en QA | Notas / comentarios |
|:---:|:---:|:---|:---:|:---|
| 1 | Pendiente | Cotización normal, 2 artículos, tramos y regular, sin exceder los descuentos | 202602181235-IC | |
| 1.1 | Revisado | Guardar un registro en ELCOTA, verificar que todos los datos sean correctos | | |

</details>

<details open> <summary> Revisión 202603231745-IC </summary>

|Pruebas|Status|Descripción|Revisión en QA|Notas / Comentarios|
|:---|:---|:---|:---|:---|
|1||Revisión de leyendas de tipo de cliente al seleccionar en nueva cotización|||
|1.1|Completa|CN (Fecha de alta < 92 días)|||
|1.2|Completa|MA (Npolitica < 10000 y ~SubPolítica), (Política termina en 1 y SubPolítica 6-10)|||
|1.3|Completa|mA (Npolitica > 10000 y SubPolítica 1-5)|||
|1.4|Completa|MB (Política termina en 1 y SubPolítica 1-5)|||
|2|Error|Inserción de cotización normal, 2 artículos, tramos y regular, sin exceder los descuentos|||
|2.1|Iniciado|ELCOT|El margen de vendedor tiene una diferencia de 2 centavos, pero posiblemente esté chueco clarion||
|2.2|Completa|ELDCOT|Todo idéntico||
|2.3|Error|ELCOTA|el Administra WEB ya genera el registro de autorización, pero los siguientes son incorrectos:
MARGENVEND2 centavos de diferencia, pero posiblemente esté chueco clarion
EDOAUTORIZASi es automática, el estado debe ser 5
FCAUTREC
HRAUTREC
IDUSUARIO_AUTREC
ARGUMENTOSV
EDOVISTOPosiblemente éstos 3 deban llenarse cuando es automática, posiblemente alguien los vió en la ventana de revisión del vendedor, validar contra código original
FCVISTOVENPosiblemente éstos 3 deban llenarse cuando es automática, posiblemente alguien los vió en la ventana de revisión del vendedor, validar contra código original
HRVISTOVENPosiblemente éstos 3 deban llenarse cuando es automática, posiblemente alguien los vió en la ventana de revisión del vendedor, validar contra código original||
|2.4|Error|ELCOTAD|Error, Administra WEB está generando registros del detalle de la autorización, pero como es automática, éstos no deberían existir (Administra viejito no los genera)||
|2.5|Error|Revisión de ventana de soporte de ventas (no debe haber nada)|Error, muestra la cotización en tablero y NO debería (por el estatus)||
|2.6|Error|Revisión de ventana de respuesta de soporte de ventas (no debe haber nada)|Error, muestra la cotización en tablero y NO debería (por el estatus)||
|2.7|Completa|Debe ofrecer impresión|Todo idéntico||
|2.8||Volver a abrir la cotización e incrementarle 1% al porcentaje de descuento sin exceder|||
|2.8.1|Error|ELCOT|Diferencia en totales y márgenes||
|2.8.2|Error|ELDCOT|Diferencia en margen del vendedor||
|2.8.3|Error|ELCOTA|OBS: Adicionalmente, no está borrando los registros de autorización automática previa y sigue generando el registro con los errores anteriores||
|2.8.4|Error|ELCOTAD|Genera registros aquí y es incorrecto||
|3|Completa|Inserción de cotización normal, 2 artículos, tramos y regular, excediendo los descuentos, GUARDANDO|||
|3.1|Completa|ELCOT|Todo idéntico||
|3.2|Completa|ELDCOT|Todo idéntico||
|3.3|Completa|ELCOTA|Todo idéntico||
|3.4|Completa|ELCOTAD|Todo idéntico||
|3.5|Completa|Revisión de ventana de soporte de ventas|||
|3.6|Completa|Revisión de ventana de respuesta de soporte de ventas|||
|3.7|Completa|NO debe imprimir|Todo idéntico||
|4|Completa|Inserción de cotización normal, 2 artículos, tramos y regular, excediendo los descuentos, SOLICITANDO, autorizando SIN adicionales SV|Todo idéntico||
|4.1|Completa|ELCOT|Todo idéntico||
|4.2|Completa|ELDCOT|Todo idéntico||
|4.3|Completa|ELCOTA|Todo idéntico||
|4.4|Completa|ELCOTAD|Todo idéntico||
|4.5|Completa|Revisión de ventana de soporte de ventas|Todo idéntico||
|4.6|Completa|Revisión de ventana de respuesta de soporte de ventas|Todo idéntico||
|4.7|Completa|NO debe imprimir|Todo idéntico||
|5|Completa|Inserción de cotización normal, 2 artículos, tramos y regular, excediendo los descuentos, SOLICITANDO, autorizando CON adicionales SV|||
|5.1|Completa|ELCOT|Todo idéntico||
|5.2|Completa|ELDCOT|Todo idéntico||
|5.3|Completa|ELCOTA|Todo idéntico||
|5.4|Completa|ELCOTAD|Todo idéntico||
|5.5|Completa|Revisión de ventana de soporte de ventas|Todo idéntico||
|5.6|Completa|Revisión de ventana de respuesta de soporte de ventas|Todo idéntico||
|5.7|Completa|NO debe imprimir|Todo idéntico||
|6||Inserción cot. EXCEDIENDO, sólo guardarla|||
|6.1|Completa|ELCOT|||
|6.2|Completa|ELDCOT|||
|6.3|Completa|ELCOTA|||
|6.4|Completa|ELCOTAD|||
|6.5|Completa|Revisión de ventana de soporte de ventas (no debe haber nada)|||
|6.6|Completa|Revisión de ventana de respuesta de soporte de ventas (no debe haber nada)|||
|6.7|Completa|NO imprimir|||
|6.8||Abrirla, modificarla +5%++ descto y volver a guardarla SOLICITANDO|||
|6.8.1|Error|ELCOT|NO cambia el color de una partida a negro al subirle 5% (74%), NO coincide nada (ver consulta)||
|6.8.2|Error|ELDCOT|NO coincide el márgen del vendedor de la partida a negro||
|6.8.3|Error|ELCOTA|NO coinciden los márgenes||
|6.8.4|Completa|ELCOTAD|Todo idéntico||
|6.8.5|Completa|NO imprimir|||
|6.9||AUT C/ADNLS 50%|||
|6.9.1|Error|ELCOT|Queda igual que 6.8.1||
|6.9.2|Error|ELDCOT|Baja el costo neto de venta 1%||
|6.9.3|Error|ELCOTA|Queda igual al 6.8.3||
|6.9.4|Completa|ELCOTAD|Baja el costo neto de venta||
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

## NO HE ENCONTRADO ERRORES AL MOMENTO, VOY A RESERVAR ÉSTE PARA EJEMPLIFICAR

| Error | Severidad | Pasos para reproducir | Comportamiento actual | Comportamiento esperado | Evidencia | Usuario contacto | Fecha corrección | Notas / comentarios |
|:---|:---:|:---|:---|:---|:---:|:---|:---:|:---|
| | | | | | `screenshots/error-001.png` | | | |

</details>

---
---
#### Otros ejemplos de control de QA
<details> <summary> Otros ejemplos de control de QA </summary>

## Checklist de Migración
- [x] Funcionalidades principales migradas
- [x] Datos persistentes correctos
- [ ] APIs responden igual que EXE
- [x] UI/UX similar
- [ ] Rendimiento aceptable
- [ ] Documentación actualizada

</details>

#### Hay que actualizar los tópicos del siguiente colapsable:
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
