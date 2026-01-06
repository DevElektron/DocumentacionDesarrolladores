# 📦 Módulo: Habilitar Reimpresión de Factura

#### 📁 **Código Frontend:** `src/app/modules/ventas/consultadefacturas/components/habilitar-reimpresion-factura`

#### 📁 **Código Backend:** `BackVentas/Modulo/ConsultadeFacturas`

#### 💻 **Menú:** Ventas > Consulta de Facturas > (Botón de acción en grid) [Ver en QA](http://192.168.2.16:1089/app/ventas/consultadefacturas)

## 📝 Descripción

Este componente modal permite liberar una factura que ya fue impresa para que el sistema permita una nueva impresión física. Para evitar duplicidad descontrolada, el módulo aplica reglas estrictas de negocio basadas en el estado de la factura y el tiempo transcurrido desde su generación.

## 🔐 Seguridad

| Tipo UI | Elemento | Descripción | Rol permitido |
| --- | --- | --- | --- |
| Modal | `HabilitarReimpresionFactura` | Ventana de confirmación para habilitar el folio | Ventas / Gerente |
| Botón | `onHabilitar` | Ejecuta la actualización del campo `Bndimpresa` en la DB | Gerente |

## 💼 Políticas Generales

1. **Restricción por Estado:** No es posible habilitar la reimpresión de facturas que ya se encuentran **CANCELADAS** (`Estado == 0`).
2. **Facturas de Activos:** Las facturas con procedencia "V" (Activos) no se procesan por este medio.
3. **Validación de Estatus:** Si la factura ya está habilitada (`Bndimpresa == 0`), el sistema arrojará el mensaje: `"Esta factura YA se encuentra habilitada"`.
4. **Regla de Ventana de Tiempo (Crítico):** Solo se permite la habilitación automática en los siguientes casos:
* **Hoy:** Facturas generadas en el día actual.
* **Cierre del día anterior:** Facturas de ayer después de las **17:00:00** (siempre que hoy no sea lunes).
* **Cierre de fin de semana:** Si hoy es lunes, permite facturas del sábado después de las **12:00:00**.



## 🧪 Casos de Prueba

### 1. Habilitación Exitosa (Factura de Hoy)

#### 💼 Operación

* [ ] Seleccionar una factura generada el día de hoy que ya haya sido impresa.
* [ ] Confirmar la acción en el modal.

#### 🛡️ Validaciones

* [ ] El sistema debe mostrar el mensaje de éxito: `"Factura habilitada para reimpresión correctamente."`.
* [ ] En la base de datos, el campo `Bndimpresa` debe cambiar a `0`.

### 2. Bloqueo por Factura Cancelada

#### 💼 Operación

* [ ] Intentar habilitar una factura que tenga el estatus "Cancelada" en el grid de consulta.

#### 🛡️ Validaciones

* [ ] El sistema debe lanzar una alerta de advertencia: `"No es posible habilitar la reimpresión de una factura CANCELADA."`.

### 3. Validación de Ventana de Tiempo (Fuera de Rango)

#### 💼 Operación

* [ ] Intentar habilitar una factura de hace 3 días o una factura del día de ayer generada antes de las 17:00 hrs.

#### 🛡️ Validaciones

* [ ] El backend debe rechazar la solicitud con el error: `"La factura está fuera del rango permitido para reimpresión automática..."`.

### 4. Flujo de Lunes (Facturas de Sábado)

#### 💼 Operación

* [ ] (Simulación) Siendo día Lunes, intentar habilitar una factura del sábado anterior generada a las 14:00 hrs.

#### 🛡️ Validaciones

* [ ] El sistema debe permitir la habilitación ya que cumple con la regla de cierre de mediodía de sábado (Hora > 4320000 en centésimas).

## 📎 Observaciones adicionales

* Las horas de corte están programadas en centésimas de segundo para compatibilidad con el estándar de Clarion: 17:00 hrs = `6120000` y 12:00 hrs = `4320000`.
* El componente frontend utiliza `NgToastService` para mostrar las alertas de "Atención!" y "Confirmación!".


# [FEATURE: Habilitar Reimpresión de Facturas]

[Motivo del PR]:
- **[ADD]**: Endpoint `habilitar-reimpresion` en `FacturasController`.
- **[ADD]**: Lógica de validación de fechas/horas basada en el calendario laboral (incluye lógica de Lunes/Sábado).
- **[FIX]**: Control de excepciones para evitar reimpresiones en facturas canceladas o de activos.

[Breve descripción de la funcionalidad]:
**Implementación de la regla de negocio para liberar folios de facturación ya impresos, permitiendo su re-emisión física bajo condiciones controladas de tiempo y estatus fiscal.**

### Puntos clave
- **Lógica de Tiempo**: Se implementó el cálculo de horas en centésimas de segundo (estándar legacy) para asegurar paridad con el sistema anterior.
- **Integridad**: Validación forzosa del campo `Estado` y `Procedencia` antes de permitir la actualización de `Bndimpresa`.

- Ruta principal: **_`api/ventas/consultadefacturas`_**
- Endpoints:
1. **[POST][habilitar-reimpresion]**: Actualiza el estatus de impresión del folio.


> 🗓️ **Fecha de última modificación:** 2025-12-29
> 👤 **Samuel Valles Sanchez**
> 🏷️ **Versión:** 1



