## PROYECTO MIGRACIÓN ERP
# ENTREGABLES

# Índice de Contenido

- 📦 [Pull Request - Proyecto Frontend](#pull-request---proyecto-frontend)
- 🔧 [Pull Request - Proyecto API](#pull-request---proyecto-api)
- 📘 [Documentación de Módulos](#documentación-de-módulos)
- 🛠️ [Documentación de Recursos de Ayuda](#documentación-de-recursos-de-ayuda)

## Pull Request - Proyecto Frontend

Plantilla para título y mensaje de los Pull Request del proyecto Frontend en GitHub [`AdministraWeb Front`](https://github.com/DevElektron/AdministraWeb-Front):

```markdown
## [Nombre Proyecto]
# [Nombre Módulo / Bug / Fix / Feature]

[Motivo del PR]:
- **[Detalle 1]**: [Descripción detalle 1].
- **[Detalle 1]**: [Descripción detalle 1].

[Breve descripción de la funcionalidad de Módulo / Bug / Fix / Feature]:
**[Descripción de funcionalidad]**

### Puntos clave

[Se explica a detalle punto que resuelve o agrega al proyecto].

### Nuevos elementos de ayuda del proyecto

**_Autocompletes_**

- **[Ruta-del/priyecto/del/nuevo/autocomplete1]**: [Descripción breve del autocomplete1].
- **[Ruta-del/priyecto/del/nuevo/autocomplete2]**: [Descripción breve del autocomplete2].

**_Helpers_**

- **[Ruta-del/priyecto/del/nuevo/helper1]**: [Descripción breve del helper1].
- **[Ruta-del/priyecto/del/nuevo/helper2]**: [Descripción breve del helper2].

**_Otro recurso de ayuda_**

- **[Ruta-del/priyecto/del/nuevo/recurso1]**: [Descripción breve del recurso1].
- **[Ruta-del/priyecto/del/nuevo/recurso2]**: [Descripción breve del recurso2].

> NOTA: [Detalle de alguna recurso de ayuda].
```

## Pull Request - Proyecto API

Plantilla para título y mensaje de los Pull Request del proyecto Backend en GitHub [`AdministraWeb API`](https://github.com/DevElektron/AdministraWeb-API):

```markdown
## [Nombre Proyecto]
# [Nombre Módulo / Bug / Fix / Feature]

[Motivo del PR]:
- **[Detalle 1]**: [Descripción detalle 1].
- **[Detalle 1]**: [Descripción detalle 1].

[Breve descripción de la funcionalidad de Módulo / Bug / Fix / Feature]:
**[Descripción de funcionalidad]**

### Puntos clave

[Se explica a detalle punto que resuelve o agrega al proyecto].

[Favor de hacer caso omiso si tu PR no tiene referencia a endpoints]
- Ruta principal: **_[ruta/padre/de/tu/]_**
- Endpoints:
1. **[Verbo HTTP][endpoint1]**: [Descripción endpoint1].
2. **[Verbo HTTP][endpoint2]**: [Descripción endpoint2].

### Nuevos elementos de ayuda del proyecto

**_Autocompletes_**

- **[Ruta-del/priyecto/del/nuevo/autocomplete1]**: [Descripción breve del autocomplete1].
- **[Ruta-del/priyecto/del/nuevo/autocomplete2]**: [Descripción breve del autocomplete2].

**_Helpers_**

- **[Ruta-del/priyecto/del/nuevo/helper1]**: [Descripción breve del helper1].
- **[Ruta-del/priyecto/del/nuevo/helper2]**: [Descripción breve del helper2].

**_Otro recurso de ayuda_**

- **[Ruta-del/priyecto/del/nuevo/recurso1]**: [Descripción breve del recurso1].
- **[Ruta-del/priyecto/del/nuevo/recurso2]**: [Descripción breve del recurso2].

> NOTA: [Detalle de alguna recurso de ayuda].

```

## Documentación de Módulos

A continuación se muestra la plantilla para la documentación de módulo del proyecto AdministraWeb, que será guardada en [text](https://github.com/DevElektron/DocumentacionDesarrolladores/tree/main/src/app/modules) con tu ruta de carpetas de frontend:

```markdown
# 📦 Módulo: [Nombre del Módulo]
#### 📁 **Código:** `/ruta/del/código/frontend`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ruta/de/tu/modulo/1)
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/ruta/de/tu/modulo/2)

## 📝 Descripción
[Descripción breve del módulo].

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| [Tipo Control UI] | [Descripción del Control] | [Descripción de funcionalidad del control UI]. | [Roles del control] |

## 💼 Políticas Generales
[Aquí se describen cuestiones de permisos y reglas de negocios]
1. [Politica1].
2. [Politica2].

## 🧪 Casos de Prueba

### [Título Prueba 1].
#### 💼 Operación
- [ ] [Descripción Prueba 1].
#### 🛡️ Validaciones
- [ ] [Validacion1 para saber si pasó la prueba].
- [ ] [Validacion2 para saber si pasó la prueba].


## 📎 Observaciones adicionales
- [Observación1].
- [Observación2].
- [Observación3].

> 🗓️ **Fecha de última modificación:** [YYYY-MM-DD]
> 👤 **[Primer Nombre] [Primer Apellido]**
> 🏷️ **Versión:** 1

---
# Comunicaciones
|Dir|Fecha       |Firma|Comentario                    |
|---|------------|-----|------------------------------|
```

## Documentación de Recursos de Ayuda

Para Helpers, componentes reutilizables, patrones DRY, y convenciones semánticas entre backend y frontend visita la [Documentacion para Desarrolladores](https://github.com/DevElektron/DocumentacionDesarrolladores).
