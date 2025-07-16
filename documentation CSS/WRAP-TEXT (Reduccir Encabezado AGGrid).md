# 📏 CLASE CSS `wrap-text`

## 🎯 ¿Qué hace?

La clase wrap-text permite hacer "n" cantidad de interlineados al detectar un espacio entre palabras esto con el fin de reduccir el tamaño del encabezado en AGGRID y
tambien se puede usar para cuando necesitar hacer interlineados en alguna etiqueta de html.

---

## 🛠️ ¿Cómo implementarla en AGGRID?

### 1. ya esta en styless.css del directorio principal del proyecto, solo es aplicarlo en los campos del AGGRID en el typescript.

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
### 2. El resultado del encabezado Cuenta Bancaria, en la linea de arriba estara Cuenta y abajo Bancaria.

