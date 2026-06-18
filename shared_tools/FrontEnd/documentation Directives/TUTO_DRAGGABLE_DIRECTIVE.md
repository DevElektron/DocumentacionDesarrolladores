# 🖱️ Dialog Draggable Directive

Directiva reutilizable para **arrastre libre de diálogos modales** en Angular 18, diseñada para usarse con `MatDialog` de Angular Material.

Permite al usuario **mover el dialog a cualquier posición de la pantalla** arrastrando su barra de título, sin necesidad de escribir lógica de drag en cada componente.

---

## 🚀 Características

- Arrastre suave con el mouse
- Compatible con `MatDialogRef.updatePosition()`
- Inyección de `MatDialogRef` **opcional** (no falla si se usa fuera de un dialog)
- Bloquea la selección de texto durante el arrastre
- Cursor cambia a `grabbing` mientras se arrastra
- Sin dependencias adicionales

---

## 📦 Ubicación

```bash
shared/directives/dialog-draggable.directive.ts
```

---

## 🧠 Descripción General

La directiva escucha el evento `mousedown` sobre el elemento host (generalmente la barra de título del dialog). Al detectarlo, calcula el desplazamiento relativo entre el cursor y el panel `.cdk-overlay-pane`, y actualiza su posición en tiempo real a través de `MatDialogRef.updatePosition()`.

La lógica de mousemove y mouseup se registra directamente en `document` para garantizar que el arrastre continúe aunque el cursor salga del elemento.

---

## 🧩 Ejemplo de Uso

```html
<!-- En el template del dialog -->
<div class="dialog-header" appDialogDraggable>
  <i class="fa-solid fa-calculator icon-dialog"></i>
  <span class="header-text">Mi Dialog</span>
  <!-- El botón de cierre debe detener la propagación para no iniciar el drag -->
  <button (click)="cerrar()" (mousedown)="$event.stopPropagation()">✕</button>
</div>
```

```typescript
// En el componente del dialog, importar la directiva
imports: [
  DialogDraggableDirective,
  // ...
]
```

---

## ⚙️ Configuración del Dialog

El dialog debe abrirse con las siguientes opciones para evitar que el clic fuera lo cierre:

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

Esta directiva no expone `@Input()`. Su comportamiento está completamente determinado por el contexto del dialog en el que se usa.

---

## 📤 Outputs

Esta directiva no emite eventos. Interactúa directamente con `MatDialogRef`.

---

## 🔁 Comportamiento

1. El usuario presiona el mouse sobre el elemento con `appDialogDraggable`
2. La directiva calcula la posición relativa del cursor dentro del panel
3. Al mover el mouse, actualiza la posición del panel en tiempo real
4. Al soltar el mouse, elimina los listeners de `document`
5. El cursor del body vuelve a su estado normal

---

## ⚠️ Consideraciones

- Aplicar **siempre sobre el header** del dialog, no sobre el contenedor raíz
- El botón de cierre u otros elementos interactivos dentro del header deben incluir `(mousedown)="$event.stopPropagation()"` para cancelar el inicio del arrastre
- Si se usa fuera de un `MatDialog`, la directiva es inerte (no lanza errores gracias a `inject(MatDialogRef, { optional: true })`)
- El dialog debe tener `hasBackdrop: false` para permitir interacción libre con la pantalla mientras está abierto

---

## 🏷️ Versionado

- Versión: **1.0**
- Angular: **18+**
- Tipo: **Directiva compartida**

---

## 👤 Autor

**Luis Guillermo Pérez Fuentes**  
Fecha de última modificación: **2026-06-18**
