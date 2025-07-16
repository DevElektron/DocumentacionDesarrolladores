# 📏 FORZAR PARA QUE EN VEZ DE VALORES EN LOS FILTROS PUEDAN APARECER ICONOS con `agSetColumnFilter`

## 🎯 ¿Qué hace?

Filtrar y/o incrustar una imagen de estatus en columna de AGGRID.

---

## 🛠️ ¿Cómo implementarla en AGGRID?

### 1. En nuestro typescript en el campo que necesitamos agregar filtro o incrustrar una imagen de estatus debemos hacer lo siguiente.

```html
  public AgGridDepositos: (ColDef | ColGroupDef)[] = [
    {
      field: 'bndautcob',                                          --Campo del servicio usado en AGGRID
      headerName: 'AUT COB',
      filter: 'agSetColumnFilter',                                 --Utilizar en filter agSetColumnFilter
      filterParams: {
        // Forzar los valores que aparecerán en el filtro
        values: (params) => {
          params.success([1, 0]);
        },
        // Mostrar los valores como texto
        valueFormatter: (params) => {
          this.iniautcob++;
          this.autcob = String(params.value);
          this.actualizarEstadoBoton();
          if (params.value === 1) return '🟢 Autorizado';
          if (params.value === 0) return '🔴 No Autorizado';
          return '';
        },
        // Este es el truco: hacer que el filtro compare texto y valor real correctamente
        keyCreator: (params) => {
          const val = Number(params.value);
          if (params.value === 1) return 1;
          if (params.value === 0) return 0;
          return '';
        },
      },
      // Renderizado de iconos
      cellRenderer: (params: any) => {
        if (params.value === 1) {
          return `<span class="circle-green-small" title="Autorizado"></span>`;
        } else if (params.value === 0) {
          return `<span class="circle-red-small" title="No Autorizado"></span>`;
        } else {
          return '';
        }
      },
      pinned: 'left',
      floatingFilter: true,
      minWidth: 73,
      maxWidth: 73,
      headerClass: [this.boldHeaderClass, 'wrap-text', 'center-header'],
      ...this.AlignCenter,
    },
```
### 2. El resultado del encabezado Cuenta Bancaria, en la linea de arriba estara Cuenta y abajo Bancaria.

