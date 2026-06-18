# 👁️ Dialog Focus Aware Directive

Directiva reutilizable para **detección de foco en diálogos modales** en Angular 18, diseñada para usarse con `MatDialog` de Angular Material.

Permite que un dialog permanezca abierto al hacer clic fuera de él, aplicando un **efecto visual de segundo plano** (opacidad reducida) cuando el usuario interactúa con otra parte de la pantalla.

---

## 🚀 Características

- Detección automática de clic dentro o fuera del dialog
- Clase CSS de "segundo plano" configurable via `@Input`
- Aplicación y remoción de clase via `Renderer2` (sin manipulación directa del DOM)
- Sin dependencias externas más allá de Angular core
- Compatible con múltiples dialogs abiertos simultáneamente

---

## 📦 Ubicación

```bash
shared/directives/dialog-focus-aware.directive.ts
```

El estilo del estado de segundo plano vive en el archivo de estilos globales:

```bash
src/styles.css  →  .en-segundo-plano
```

---

## 🧠 Descripción General

La directiva escucha el evento `mousedown` a nivel de `document`. En cada clic, evalúa si el objetivo del evento está contenido dentro del elemento host. Si el clic fue **dentro**, remueve la clase de segundo plano. Si fue **fuera**, la aplica.

El resultado es un indicador visual claro de qué dialog tiene el foco activo cuando hay múltiples ventanas flotantes en pantalla, sin cerrar ni interrumpir ninguna de ellas.

---

## 🧩 Ejemplo de Uso

```html
<!-- En el template del dialog, aplicar al contenedor raíz -->
<div mat-dialog-content class="mi-dialog" appDialogFocusAware>
  <!-- contenido del dialog -->
</div>
```

Clase personalizada (opcional):

```html
<div mat-dialog-content class="mi-dialog" appDialogFocusAware focusOutClass="mi-clase-inactivo">
  <!-- contenido del dialog -->
</div>
```

```typescript
// En el componente del dialog, importar la directiva
imports: [
  DialogFocusAwareDirective,
  // ...
]
```

---

## ⚙️ Configuración del Dialog

Para que el dialog no se cierre al hacer clic fuera, debe abrirse con:

```typescript
this.dialog.open(MiDialogComponent, {
  hasBackdrop: false,   // sin overlay oscuro
  disableClose: true,   // evita cierre con Escape
  width: '480px',
  height: '520px',
});
```

---

## 📥 Inputs

| Input | Tipo | Default | Descripción |
|-------|------|---------|-------------|
| `focusOutClass` | `string` | `'en-segundo-plano'` | Clase CSS que se aplica al host cuando pierde el foco |

---

## 📤 Outputs

Esta directiva no emite eventos. Interactúa directamente con el DOM via `Renderer2`.

---

## 🔁 Comportamiento

1. El usuario hace clic en cualquier punto de la pantalla
2. La directiva evalúa si el clic fue dentro o fuera del host
3. **Clic dentro** → remueve `focusOutClass` del host → dialog se ve a plena opacidad
4. **Clic fuera** → aplica `focusOutClass` al host → dialog se opaca visualmente
5. El dialog permanece abierto y funcional en ambos casos

---

## 🎨 Estilos Requeridos

El siguiente estilo debe existir en `styles.css` (ya incluido en el proyecto):

```css
.en-segundo-plano {
  opacity: 0.55;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.12) !important;
  transition: opacity 0.2s ease, box-shadow 0.2s ease;
}
```

Para usar una clase personalizada via `focusOutClass`, definir los estilos correspondientes en `styles.css` o en el CSS del componente con `encapsulation: ViewEncapsulation.None`.

---

## ⚠️ Consideraciones

- Aplicar **sobre el contenedor raíz** del dialog para que `contains()` evalúe correctamente todos los elementos hijos
- Si hay múltiples dialogs con esta directiva abiertos, cada uno reacciona de forma independiente: el que recibe el clic gana el foco, los demás se opcan
- La directiva **no controla** si el dialog se cierra o no — eso es responsabilidad de la configuración del `MatDialog` (`hasBackdrop`, `disableClose`)
- El `transition` en el CSS del host es necesario para que el cambio de opacidad sea suave; si el host tiene `overflow: hidden`, verificar que la transición no cause artefactos visuales

---

## 🧪 Casos de Uso

- Paneles de consulta flotantes abiertos mientras se trabaja en un formulario principal
- Calculadoras o utilerías que conviven con otros dialogs
- Cualquier dialog no modal que deba indicar visualmente si está activo

---

## 🏷️ Versionado

- Versión: **1.0**
- Angular: **18+**
- Tipo: **Directiva compartida**

---

## 👤 Autor

**Luis Guillermo Pérez Fuentes**  
Fecha de última modificación: **2026-06-18**
