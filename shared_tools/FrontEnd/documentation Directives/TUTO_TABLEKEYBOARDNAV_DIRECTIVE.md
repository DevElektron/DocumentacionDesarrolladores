# ⌨️ Table Keyboard Navigation Directive

Directiva reutilizable para **navegación por teclado en tablas dinámicas** en Angular 18, pensada para escenarios de **captura intensiva con FormArray**, como partidas, artículos o detalles.

Permite trabajar **100% con teclado**, incluyendo inputs, autocompletes, selects e íconos.

---

## 🚀 Características

- Navegación con flechas **↑ ↓ ← →**
- Soporte para inputs, autocompletes, selects e íconos
- **Scroll suave automático** al cambiar de foco
- Agregado automático de filas
- Eliminación automática de la última fila
- Emisión de evento al **cambio de fila**
- Respeta validaciones de encabezado y detalle
- Compatible con **Angular 18 + Reactive Forms**

---

## 📦 Instalación

Copiar la directiva en el proyecto:

```bash
shared/directives/table-keyboard-nav.directive.ts
```

Declarar la directiva en un módulo compartido o módulo específico.

---

## 🧠 Descripción General

Esta directiva resuelve la navegación eficiente por teclado dentro de tablas dinámicas que utilizan `FormArray`.

Evita el uso excesivo del mouse, controla el foco automáticamente y permite flujos de captura rápidos y controlados mediante validaciones.

---

## 🧩 Ejemplo de Uso

```html
<input
  appTableKeyboardNavDirective
  [row]="i"
  [col]="0"
  [headerValid]="headerForm.valid"
  [isRowValid]="detalleFormArray.at(i).valid"
  (requestAddRow)="agregarPartida()"
  (requestRemoveLastRow)="eliminarUltimaPartida()"
  (rowChanged)="actualizaInfo($event)"
  data-row="{{ i }}"
  data-col="0"
/>
```

---

## 📥 Inputs

| Input | Tipo | Descripción |
|------|------|-------------|
| row | number | Índice de la fila actual |
| col | number | Índice de la columna actual |
| headerValid | boolean | Indica si el encabezado del formulario es válido |
| isRowValid | boolean | Indica si la fila actual cumple validaciones |

---

## 📤 Outputs

### requestAddRow

Se emite cuando:
- Se presiona **ArrowDown**
- Se está en la **última fila**
- La fila actual es válida

Responsabilidad del componente padre:
- Agregar un nuevo elemento al `FormArray`

---

### requestRemoveLastRow

Se emite cuando:
- Se presiona **ArrowUp**
- Se está en la **última fila**
- La fila actual es inválida

Responsabilidad del componente padre:
- Eliminar el último elemento del `FormArray`

---

### rowChanged

Se emite cuando:
- Cambia la fila activa durante la navegación vertical

Uso típico:
```ts
actualizaInfo(rowIndex: number): void {
  // lógica adicional
}
```

---

## 🔁 Comportamiento de Navegación

- **→** Avanza a la siguiente columna visible (wrap automático)
- **←** Retrocede a la columna anterior visible (wrap automático)
- **↓** Avanza a la siguiente fila
- **↑** Retrocede a la fila anterior
- **Enter** queda deshabilitado
- Si `headerValid` es `false`, no se permite navegación

---

## 📜 Reglas de Negocio

- El encabezado debe ser válido para habilitar navegación
- Solo se navegan columnas visibles
- En la última fila:
  - Se agrega una nueva fila si es válida
  - Se elimina si es inválida
- El foco siempre se mantiene visible mediante scroll suave

---

## 🧪 Casos de Uso

- Captura de facturas
- Traspasos
- Pedidos
- Inventarios
- Tablas dinámicas con validación progresiva

---

## ⚠️ Consideraciones

- El componente padre controla:
  - Alta y baja de filas
  - Validaciones de encabezado y detalle
- Es obligatorio usar `data-row` y `data-col`
- Diseñada para tablas o layouts tipo grid

---

## 🏷️ Versionado

- Versión: **1.0**
- Angular: **18+**
- Tipo: **Directiva compartida**

---

## 👤 Autor

**Luis Guillermo Pérez Fuentes**  
Fecha de última modificación: **2025-07-02**
