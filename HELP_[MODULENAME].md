# 📦 Módulo: Traspasos de Almacén
**Ruta:** Inventarios → Traspaso de mercancía

---

## 📝 Descripción
Este módulo permite registrar y consultar los movimientos de mercancía entre almacenes. Incluye validaciones específicas según el tipo de artículo, existencia y permisos del usuario.

---

## 🔐 Políticas Generales
- Los traspasos deben tener un folio único.
- El almacén de salida se obtiene del usuario actual; si no tiene uno asignado, debe seleccionarlo manualmente.
- El sistema debe impedir seleccionar el mismo almacén como origen y destino.
- El artículo debe tener existencia en el almacén de salida para ser transferido.

---

## 🔐 Seguridad

| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Añadir traspaso   | Abre la ventana de captura     | Almacenista    |
| Acción  | Guardar traspaso  | Guarda la operación en BD      | Almacenista    |
| Acción  | Eliminar traspaso | Solo si no ha sido recibido    | Supervisor     |

---

## 🧪 Casos de Prueba

### 1. Capturar traspaso normal

#### 🛡️ Validaciones
- [ ] No se puede cambiar folio, fecha ni almacén de salida.
- [ ] Se debe tomar el almacén del usuario; si tiene almacén `0`, debe elegirlo manualmente.
- [ ] No se permite folio duplicado.
- [ ] Almacén de destino debe ser distinto al de salida.
- [ ] El artículo debe tener existencia en almacén origen.

---

### 2. Capturar traspaso con artículo de tramos

#### 🛡️ Validaciones
- [ ] Las mismas reglas anteriores aplican.
- [ ] Validar tramos disponibles para el artículo.
- [ ] El traspaso debe incluir información del tramo seleccionado (si aplica).

---

## 📎 Observaciones adicionales
- El módulo debe integrarse con el sistema de Kardex para registrar movimientos de entrada y salida.
- La tabla `TraspasosDetalle` debe guardar la información de cada artículo y tramo si aplica.

---

> 🗓️ **Fecha:** 2025-06-30  
> 👤 **Autor:** Luis Pérez  
> 🏷️ **Versión:** 1.0  
> 📁 **Ubicación del código:** `Modules/Inventarios/Traspasos`

