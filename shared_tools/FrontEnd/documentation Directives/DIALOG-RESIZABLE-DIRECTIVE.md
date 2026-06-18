# ↔️ Dialog Resizable Directive

Directiva reutilizable para **redimensionamiento libre de diálogos modales** en Angular 18, diseñada para usarse con `MatDialog` de Angular Material.

Permite al usuario **ajustar el ancho y alto del dialog** desde cualquiera de sus 8 bordes y esquinas, sin necesidad de agregar markup adicional en el template.

---

## 🚀 Características

- **8 handles de redimensionamiento**: N, S, E, W, NE, NW, SE, SW
- Handles creados **automáticamente en el DOM** (sin `<div>` en el template)
- Tamaños mínimos configurables via `@Input`
- Compatible con `MatDialogRef.updatePosition()` para handles Norte y Oeste
- Limpieza automática de listeners y elementos al destruir el componente
- Cursor específico por dirección de resize
- Sin dependencias adicionales

---

## 📦 Ubicación

```bash
shared/directives/dialog-resizable.directive.ts
```

Los estilos de los handles viven en el archivo de estilos globales:

```bash
src/styles.css  →  .dialog-resize-handle, .dialog-resize-{n,s,e,w,ne,nw,se,sw}
```

---

## 🧠 Descripción General

Al inicializarse (`ngAfterViewInit`), la directiva crea 8 elementos `<div>` con clases CSS predefinidas y los agrega como hijos del elemento host. Cada handle escucha `mousedown` y dispara la lógica de resize correspondiente a su dirección.

Para handles que afectan la posición del panel (N, W y sus esquinas combinadas), la directiva actualiza también la posición via `MatDialogRef.updatePosition()`, garantizando que el dialog no "salte" al redimensionar desde arriba o la izquierda.

Al destruirse el componente (`ngOnDestroy`), los handles y sus listeners son eliminados del DOM.

---

## 🧩 Ejemplo de Uso

```html
<!-- En el template del dialog, aplicar al contenedor raíz -->
<div mat-dialog-content class="mi-dialog"
     appDialogResizable
     [minWidth]="480"
     [minHeight]="520">

  <div class="dialog-header">...</div>
  <div class="dialog-body">...</div>
  <div class="dialog-actions">...</div>

</div>
```

```typescript
// En el componente del dialog, importar la directiva
imports: [
  DialogResizableDirective,
  // ...
]
```

---

## ⚙️ Configuración del Dialog

El dialog debe abrirse con `hasBackdrop: false` para permitir interacción libre:

```typescript
this.dialog.open(MiDialogComponent, {
  hasBackdrop: false,
  disableClose: true,
  width: '480px',
  height: '520px',
});
```

---

## 📥 Inputs

| Input | Tipo | Default | Descripción |
|-------|------|---------|-------------|
| `minWidth` | `number` | `480` | Ancho mínimo en píxeles al redimensionar |
| `minHeight` | `number` | `520` | Alto mínimo en píxeles al redimensionar |

---

## 📤 Outputs

Esta directiva no emite eventos. Interactúa directamente con el DOM y `MatDialogRef`.

---

## 🔁 Comportamiento por Handle

| Handle | Dirección | Afecta posición |
|--------|-----------|-----------------|
| `dialog-resize-n` | Norte | Sí (top) |
| `dialog-resize-s` | Sur | No |
| `dialog-resize-e` | Este | No |
| `dialog-resize-w` | Oeste | Sí (left) |
| `dialog-resize-ne` | Noreste | Sí (top) |
| `dialog-resize-nw` | Noroeste | Sí (top + left) |
| `dialog-resize-se` | Sureste | No |
| `dialog-resize-sw` | Suroeste | Sí (left) |

---

## 🎨 Estilos Requeridos

Los siguientes estilos deben existir en `styles.css` (ya incluidos en el proyecto):

```css
.dialog-resize-handle { position: absolute; z-index: 200; }
.dialog-resize-n  { top: 0;    left: 10px;  right: 10px;  height: 5px;  cursor: n-resize;  }
.dialog-resize-s  { bottom: 0; left: 10px;  right: 10px;  height: 5px;  cursor: s-resize;  }
.dialog-resize-e  { right: 0;  top: 10px;   bottom: 10px; width: 5px;   cursor: e-resize;  }
.dialog-resize-w  { left: 0;   top: 10px;   bottom: 10px; width: 5px;   cursor: w-resize;  }
.dialog-resize-ne { top: 0;    right: 0;    width: 12px;  height: 12px; cursor: ne-resize; }
.dialog-resize-nw { top: 0;    left: 0;     width: 12px;  height: 12px; cursor: nw-resize; }
.dialog-resize-se { bottom: 0; right: 0;    width: 12px;  height: 12px; cursor: se-resize; }
.dialog-resize-sw { bottom: 0; left: 0;     width: 12px;  height: 12px; cursor: sw-resize; }
```

---

## ⚠️ Consideraciones

- Aplicar **siempre sobre el contenedor raíz** del dialog (el que tiene `mat-dialog-content`)
- El contenedor raíz debe tener `position: relative` para que los handles se posicionen correctamente
- Si se usa fuera de un `MatDialog`, los handles se crean pero el resize no actualiza posición (no lanza errores gracias a `inject(MatDialogRef, { optional: true })`)
- El `maxWidth` del panel CDK se anula durante el resize para permitir anchos arbitrarios

---

## 🧪 Casos de Uso

- Dialogs de trabajo flotante (calculadoras, buscadores, paneles de referencia)
- Cualquier dialog donde el usuario necesite más o menos espacio según su contenido

---

## 🏷️ Versionado

- Versión: **1.0**
- Angular: **18+**
- Tipo: **Directiva compartida**

---

## 👤 Autor

**Luis Guillermo Pérez Fuentes**  
Fecha de última modificación: **2026-06-18**
