
#  EJEMPLO
#  Ruta: Inventarios -> Traspaso de mercancia
# 📦 Módulo: Traspasos de Almacén

## 📝 Descripción
Añadir descripción breve o detallada del módulo

## 🔐 Seguridad
|Tipo | Elemento       | Descripción                       | Rol permitido              |
|-----|----------------|-----------------------------------|----------------------------|
| btn | Añadir traspaso| Abre la ventana de captura        | Almacenista                |   

# 🧪 Casos de prueba
- Capturar traspaso
    - ## 🛡️ Validaciones
        [ ] No se puede cambier folio, fecha y almacén de salida
        [ ] Se toma el almacen del usuario, de tener almacen 0, el usuario debe elegir el almacen de salida.
        [ ] No se puede capturar un traspaso con un folio ya existente
        [ ] No se puede elegir el mismo almacén de salida para el de destino
        [ ] Articulo debe tener existencia 
- Capturar traspaso con artículo de tramos
    - ## 🛡️ Validaciones
        [ ] No se puede cambier folio, fecha y almacén de salida
        [ ] No se puede capturar un traspaso con un folio ya existente
        [ ] No se puede elegir el mismo almacén de salida para el de destino
        [ ] Articulo debe tener existencia  

## 🛡️ Validaciones
- RFC válido
- Productos deben tener precio > 0
- Fecha no puede ser futura
- Cliente debe existir en BD



# 🧪 Casos de prueba
- Factura con múltiples productos
- RFC inválido → debe rechazar
- Factura sin productos → debe rechazar

---

> Fecha: 2025-06-30  
> Autor: Luis Pérez
