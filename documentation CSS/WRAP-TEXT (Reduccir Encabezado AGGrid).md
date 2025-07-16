# 📏 CLASE CSS `wrap-text`

## 🎯 ¿Qué hace?

La clase wrap-text permite hacer "n" cantidad de interlineados al detectar un espacio entre palabras esto con el fin de reduccir el tamaño del encabezado en AGGRID y
tambien se puede usar para cuando necesitar hacer interlineados en alguna etiqueta de html.

---

## 🛠️ ¿Cómo implementarla en AGGRID?

### 1. ya esta en styless.css del directorio principal del proyecto, solo es aplicarlo.

```html
 {
      field: 'nctab',
      headerName: 'Cuenta Bancaria',
      ...this.commonNumberColDef,
      ...this.AlignCenter,
      headerClass: [this.boldHeaderClass, 'wrap-text'],
      minWidth: 93,
      maxWidth: 93,
      floatingFilter: true,
      enableRowGroup: true,
    },
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
