# 📏 Uso de la Directiva `appResizable`

## 🎯 ¿Qué hace?

La directiva `appResizable` permite agregar una **barra de separación redimensionable** entre dos secciones verticales (por ejemplo, dos tablas), para que el usuario pueda ajustar el tamaño del área superior manualmente.

---

## 🛠️ ¿Cómo implementarla?

### 1. Agregar referencias & class del contenedor superior en el HTML

```html
<div #grid1Container class="principal-grid-container">
  <ag-grid-angular #grid1 style="height: 100%;" ...></ag-grid-angular>
```

### 2. Definir la class para el componente inferior en el HTML

```html
  <div class="detail-grid-container">
    <ag-grid-angular style="height: 100%;" ...></ag-grid-angular>}
```

### 3. Insertar la barra redimensionable

Justo después del contenedor superior, añade:

```html
<div class="drag-handle" appResizable [targetElement]="grid1Container" [minHeight]="200"></div>
```

> 🔸 `targetElement`: referencia al elemento cuya altura se desea modificar.  
> 🔸 `minHeight`: altura mínima que se puede asignar al contenedor.

### 4. Declarar la referencia en el componente

```ts
@ViewChild('grid1') grid1ElementRef!: ElementRef;
```

---

## ✅ Ejemplo completo en HTML

```html
<section class="detail-container" style="flex: 1; display: flex; flex-direction: column;">
  <!-- Sección superior -->
  <div #grid1Container class="principal-grid-container">
    <ag-grid-angular #grid1 style="height: 100%;" ...></ag-grid-angular>

  <!-- Barra de separación -->
  <div class="drag-handle" appResizable [targetElement]="grid1Container" [minHeight]="200"></div>

  <!-- Sección inferior -->
  <div class="detail-grid-container">
    <ag-grid-angular style="height: 100%;" ...></ag-grid-angular>
</section>
```
