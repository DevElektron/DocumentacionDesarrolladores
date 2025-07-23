# 📦 Módulo: Estadistica Lider Mostrador
#### 📁 **Código:** `/Modules/vendedor/estadisticalidermostrador`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/conauxiliares/vendedor/estadisticalidermostrador)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/dashboard)

## 📝 Descripción
Este módulo muestra las estadísticas de cada vendedor tipo mostrador, incluyendo sus ventas mensuales, diarias y las ventas esperadas. Además, presenta los clientes asignados a cada vendedor, resumiendo para ellos estadísticas similares. También permite imprimir los rubros en los que el vendedor está siendo evaluado, así como estadísticas a nivel facturista.
Adicionalmente, este módulo permite visualizar a todos los vendedores que pertenecen a un gerente, con base en los almacenes que tiene asignados.
Esta ventana también se presenta como una vista dentro del inicio o dashboard, y contará con permisos independientes para su acceso.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| search   | Buscar almacén | Permite buscar los almacenes existentes | Vendedor Mostrador |
| dashboard   | Mostrar módulo | Permite mostrar el módulo en el panel de inicio | Vendedor Mostrador |
| ventana   | Mostrar opción | Permite mostrar la opción en el menú principal | Vendedor Mostrador |

## 💼 Políticas Generales
- 1 Todos los vendedores tipo mostrador deben tener permisos para esta funcionalidad, excepto para la búsqueda de almacén.
- 2 Los gerentes de los vendedores también deben contar con permisos, incluyendo la búsqueda de almacén.
- 3 Los permisos para acceder a la ventana y para mostrar la información inicialmente estarán separados.

## 🧪 Casos de Prueba

### Realizar busquedas alamcenes
#### 💼 Operación
- [ ] No se permite realizar búsquedas de almacenes inexistentes
#### 🛡️ Validaciones
- [ ] Al buscar almacenes que no le pertenecen al gerente, no debe mostrarse información de dichos almacenes
    - La información que esté cargada debe limpiarse si se cambia de almacén

### Realizar busquedas de vendores
#### 💼 Operación
- [ ] La información debe precargarse según el tipo de usuario: vendedor o gerente
#### 🛡️ Validaciones
- [ ] Se debe seleccionar un vendedor para mostrar la información correspondiente al mismo
- [ ] Si ya hay información mostrada y se cambia de vendedor, esta debe actualizarse correctamente
- [ ] Si no se selecciona ningún vendedor, no debe mostrarse información previa
- [ ] revision de las sumatorias de los datos cargados

## 📎 Observaciones adicionales
- sin observaciones

> 🗓️ **Fecha de última modificación:** 2025-07-23
> 👤 **Esaul Castrejon**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
