# 📏 FILTRAR CON ICONOS O INCRUSTRAR ESTADO DE UN CAMPO CON `agSetColumnFilter` DE AGGRID

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
          params.success([1, 0]);                                  --aqui puede tener varios valores, 0,1,2,3,4,5,6 las entidades que necesites, en este caso 0 y 1.
        },
        // Mostrar los valores como texto
        valueFormatter: (params) => {
          this.autcob = String(params.value);
          this.actualizarEstadoBoton();
          if (params.value === 1) return '🟢 Autorizado';
          if (params.value === 0) return '🔴 No Autorizado';      --aqui puedes declarar el estatus, esto en la parte del filtro, y aqui colocamos el emoji imagen
          return '';
        },
        // Este es el truco: hacer que el filtro compare texto y valor real correctamente
        keyCreator: (params) => {
          const val = Number(params.value);
          if (params.value === 1) return 1;                        --se necesita identificar el valor de params.value y asignar el return para params.sucess([1,0])
          if (params.value === 0) return 0;                        --cuando se haga la seleccion envie params.value al backend el numero y no la imagen en el filtro
          return '';                                               
        },
      },
      // Renderizado de iconos
      cellRenderer: (params: any) => {
        if (params.value === 1) {
          return `<span class="circle-green-small" title="Autorizado"></span>`;    --Renderizamos iconos en todas las filas si esta autorizado o no autorizado
        } else if (params.value === 0) {
          return `<span class="circle-red-small" title="No Autorizado"></span>`;   -- Esto con clases de font awesome 6, delcarados en el style.css del la ruta principal
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
### 2. Con ello ya podemos filtrar por icono en nuestro AGGRID.

