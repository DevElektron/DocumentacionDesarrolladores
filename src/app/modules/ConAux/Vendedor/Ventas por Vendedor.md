# 📦 Módulo: Ventas por Ciudad
#### 📁 **Código:** `/Modules/vendedor/ventasxciudad`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/conauxiliares/vendedor/ventasxciudad)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción
Este módulo muestra las ventas a nivel cuidad  asi como su objectivos y estadistica de cumplimiento en ventas esta ventana solo se puede acceder si gerente regional

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| dashboard   | Mostrar módulo | Permite mostrar el módulo en el panel de inicio | Ventas por Cuidad |
| ventana   | Mostrar menú | Permite mostrar la opción en el menú principal | Ventas por Cuidad |

## 💼 Políticas Generales
- 1 la bsuqueda incial se hara por el mes que se esta transcurriendo
- 2 la busqueda será por peridod activos

## 🧪 Casos de Prueba

### Realizar busquedas alamcenes
#### 💼 Operación
- [ ] No se permite busquedas menores a 10 años de antiguiedad
#### 🛡️ Validaciones
- [ ]  los valores tiene del calculo tiene que considir con el adminitra de clarión 
    - la vesqueda no tinene permisos dado que es una pantalla exclusiva de gerente regional

## 📎 Observaciones adicionales
- sin observaciones

> 🗓️ **Fecha de última modificación:** 2025-07-28
> 👤 **Esaul Castrejon**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
