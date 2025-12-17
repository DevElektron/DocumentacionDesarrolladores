# AG-GRID-ANGULAR Editable

## 🎯 ¿Qué hace?

La información mostrada en el componente `ag-grid-angular` puede ser editable gracias a la configuración de la definición de columnas. A continuación un tutorial de cómo pueden hacerlo.

---

## ✅ ¿Permite ag-grid-angular community la edición de la información?

La duda central en un inicio fue si el componente permite la captura en la versión `community`, y efectivamente, sí lo permite:

- **Edición condicional por celda**: Usando `editable: (params) => boolean`, puedes habilitar columnas según el tipo de usuario y etapa activa.
- **Renderizado dinámico**: Puedes ocultar o mostrar columnas según el rol (`vendedor` vs `gerente` por ejemplo).
- **Captura de cambios**: Con `cellValueChanged` puedes detectar qué celdas fueron modificadas y construir un array de cambios para el request del backend.
- **Validaciones y estilos condicionales**: Puedes aplicar clases CSS o tooltips según el estado de cada celda.

---

## 🛠️ ¿Cómo implementarla en AGGRID?

Para el ejemplo, usamos snippets del desarrollo de [`Promesas de Cobranza`](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/docs_AdministraWeb/app/modules/ConAux/Gerente/Seguimiento%20Cobranza/Captura%20Promesas.md), el cuál se trató de capturar las promesas de cobranza por parte de un Gerente (con cantidades numéricas-enteras), en columnas de un etapa habilitada y el compromiso mensual que es acordado con un Vendedor.

### 1. Establecer reglas de una columna editable

Creamos una función que devolverá un `boolean` para las condiciones en las que una columna del `ag-grid-angular` puede ser editable o no:

```ts
columnaEditable(idColumna: string, esPinnedRow: boolean = false): boolean {
    if (esPinnedRow) {
      return false;
    }

    if (
      !idColumna ||
      !this.datosTablero.esGerente ||
      this.datosTablero.periodoBd !== this.datosTablero.periodoHoy
    ) {
      return false;
    }

    if (idColumna === 'C') {
      return this.etapaActiva > 0;
    }

    if (idColumna === '4' && this.etapaActiva === 4) {
      return false;
    }

    const etapa = this.datosTablero.etapas.find(e => e.cons === parseInt(idColumna));

    return etapa?.habilitada ?? false;
}
```

En esta función, se reciben 2 parámetros, el obligatorio es un `string` que se envia para identificar a una columna, y el opcional es un `boolean` para indicar si es una celda `pinnedRow` que son utilizadas para mostrar los totales de cada columna. En este ejemplo, las "reglas" fueron:

_Una celda no será editable si:_

1. Es una celda de totales.
2. No se envia un identificador de columna.
3. el usuario no es Gerente.
4. Los periodos de la BD y el calculado de hoy no son iguales.
5. El identificador es 'C' (celda de la columna `Mensual - Compromiso`) y no hay etapa activa.
6. El identificador es '4' y la etapa 4 es la etapa activa.
7. Una celda de promesa de etapa cuya configuración en la BD sea deshabilitada.

_De lo contrario, será editable si:_

1. Una celda de promesa de etapa cuya configuración en la BD sea habilitada.
2. El identificador es 'C' (celda de la columna `Mensual - Compromiso`) y hay una etapa activa.

Por lo tanto, es altamente recomendable que crees tu función aparte y establezcas las reglas de edición de celda, ya que esta función se va a usar en [`2. Definición de columnas del grid`](#2-definicion-de-columnas-del-grid)

<a id="2-definicion-de-columnas-del-grid"></a>

### 2. Definición de columnas del grid

Para declarar una columna y sus celdas como editables, se debe de establecer el campo `editable` con un callback que retorne un `boolean` indicando el estado de edición, a continuación un ejemplo de la def. de columnas usando la función que creamos en el paso anterior:

_Columna de la Promesa de Etapa 1_  

```ts
{
    field: 'pcxC_CobPromesa1',
    headerName: 'Promesa',
    headerClass: ['wrap-text', 'center-header'],
    flex: 0.4,
    ...this.commonTextColDef,
    ...this.AlignRight,
    cellClassRules: this.getRegistrosPcxcCellClassRules(1),
    cellEditor: 'agNumericCellEditor',
    editable: (params) => this.columnaEditable('1', !!params.node?.rowPinned), // <---
    valueFormatter: this.getMonedaMxFormatter(false),
    valueParser: this.parseEditorEntero,
    tooltipValueGetter: this.getTooltipValoresPermitidosCeldaEditable
},
```

Donde:

1. `editable`: Campo de la definición de columna para indicar al AG Grid si sus celdas será editables o no.
2. `(params) =>`: Callback con el parámetro de tipo EditableCallbackParams, y contiene toda la información del contexto de la celda que se está evaluando.
3. `this.columnaEditable`: Tu función que devuelve el valor solicitado de `editable`.
4. `!!params.node?.rowPinned`: Valor booleano que indica si la celda es o no de la fila fijada en el fondo del grid (`pinnedRow`).

Para los demás campos de configuración de la definición de celda aquí los explico.

#### 2.1 `valueParser` para validación de datos capturados

Un `parser` en AG Grid es el interceptor que captura el dato ingresado en la celda, haciéndolo accesible para la validación y retornando como resultado final el valor trabajado según nuestra lógica de validación, un ejemplo es el siguiente, un `parser` que filtra todos los valores que no son números enteros entre 0 y 999,999,999:

```ts
parseEditorEntero = (params: any): number => {
    const raw = params.newValue?.toString() ?? '';

    if (/e/i.test(raw)) {
      return 0;
    }

    const num = Number(raw);

    if (isNaN(num) || num < 0 || num > this.LIMITE_CANTIDAD_CELDA_EDITABLE_DECIMAL_11_2) {
      this.toastService.danger(
        'La cantidad debe ser un número entero entre $0 y $999,999,999',
        'Edición de Registros de Promesas',
        3000
      );
      return 0;
    }

    const rounded = Math.round(num);

    return rounded;
}
```

Donde:

- `parseEditorEntero`: Nombre de la propiedad (parser).
- `params`: Instancia de `ValueParserParams` que contiene el contexto de la celda y el valor capturado en el campo `newValue`.

y las validaciones que hace este parser son:

1. Cualquier valor no numérico retorna cero.
2. Valores negativos serán rechazados.
3. Valores decimales serán redondeados al entero más cercano.
4. Notación científica (e/E) será rechazada y convertida a cero.
5. Rango permitido: número entero entre 0 y 999,999,999.

> NOTA: Los valueParser se ejecutan sobre el valor devuelto por el cellEditor configurado en la definición de columna. En otras palabras, el parser intercepta la salida del editor y transforma el dato antes de guardarlo en el rowData (ver [`CellEditor disponibles`](#2-5-nota-celleditor-disponibles)).

#### 2.2 Opcional - `valueFormatter` para cambiar formato visual del valor de una celda

Campo para establecer una función que cambia el formato visual y no como está guardado en memoria los valores de una columna. Ejemplo de un `formatter` para transformas los datos numéricos en cantidad de moneda mexicana:

```ts
getMonedaMxFormatter = (cantidadConDecimales: boolean = true) => {
    return (params: ValueFormatterParams): string => {
      if (params.value === null || params.value === undefined || isNaN(params.value)) {
        return 'N/A';
      }

      return new Intl.NumberFormat('es-MX', {
        style: 'currency',
        currency: 'MXN',
        minimumFractionDigits: cantidadConDecimales ? 2 : 0,
        maximumFractionDigits: cantidadConDecimales ? 2 : 0
      }).format(params.value);
    }
};
```

#### 2.3 Opcional - `cellClassRules` para indicar estilo de celda

Si quieres que un estilo CSS aplique condicionalmente, AG Grid ofrece el campo `cellClassRules` de la def. de columna, así si el valor en tu celda cumple una condición, el estilo CSS se aplicará, de otra forma no. Ejemplo está las `cellClassRules` del grid de promesas de cobranza:

```ts
getRegistrosPcxcCellClassRules(etapa: number = 0) {
    return {
      'cpc-etapa-deshabilitada': (params) =>
        this.etapaActiva !== etapa &&
        this.fieldsColumnasDeEtapas.includes(params.colDef.field),
      'valor-negativo-evv': (params) => params.value < 0,
      'valor-positivo-evv': (params) => params.value >= 0,
      'celda-editable': (params) => params.colDef.editable && params.colDef.editable(params),
      'total-modo-captura': (params) => params.node?.rowPinned != null && this.datosTablero.esGerente
    };
}
```

Donde:

- `params` contiene el contexto de la celda, como:  

    1. `colDef` (para obtener los campos que estamos definición en la defición de esa columna)  
    2. `value` que es el dato mostrado en la celda.  

> NOTA: Al final de esta documentación está el link al código en la rama `qa`.  
> ATENCIÓN: AG Grid no aceptará ningún estilo personalizado a menos que se le declare un `encapsulation: ViewEncapsulation.None` al componente:  

```ts
@Component({
  selector: 'app-captura-promesas-cobranza',
  templateUrl: './captura-promesas-cobranza.component.html',
  styleUrl: './captura-promesas-cobranza.component.css',
  encapsulation: ViewEncapsulation.None,
})
export class CapturaPromesasCobranzaComponent {
    ...
```

#### 2.4 Opcional - `tooltipValueGetter` para mostrar tootip en una celda

Campo de la definición de celda que retorna un `string` para que muestre un `tooltip` cuando el puntero del mouse/touchpad se encuentre encima de la cerca. Si no queremos un `string` para un tooltip, devolvemos `null`:

```ts
getTooltipValoresPermitidosCeldaEditable = (params): string | null => {
    const esFilaPinned = !!params.node?.rowPinned;
    const esColumnaEditable = typeof params.colDef.editable === 'function'
      ? params.colDef.editable(params)
      : !!params.colDef.editable;

    if (!esFilaPinned && esColumnaEditable && this.datosTablero.esGerente) {
      return 'Sólo se permiten números enteros positivos';
    }

    return null;
}
```

> TIP: Los tooltip en las celda tardan 2 segundos por defecto en mostrarse, para modificar ese tiempo usa `tooltipShowDelay` y `tooltipHideDelay` en la instancia de `GridOptions` ligada a tu grid, con valores en milisegundos para cambiar el tiempo de aparición y desaparación respectivamente.

<a id="2-5-nota-celleditor-disponibles"></a>

#### 2.5 NOTA - CellEditor disponibles

AG Grid ofrece varios `cellEditorParams` según el editor que estés usando. Para `agNumericCellEditor`, puedes controlar decimales, min/max, y más. Para otros editores como `agSelectCellEditor` o `agTextCellEditor`, hay parámetros específicos.

##### 🧩 Parámetros comunes por tipo de editor

`agNumericCellEditor`

```ts
cellEditorParams: {
  allowDecimal: true | false,     // permite decimales
  min: 0,                         // valor mínimo permitido
  max: 100000,                    // valor máximo permitido
  precision: 2,                   // número de decimales
  format: '#,##0.00',             // formato visual
}
```

`agTextCellEditor`

```ts
cellEditorParams: {
  maxLength: 50,                  // longitud máxima
  useFormatter: true | false,     // si aplica el valueFormatter al editar
}
```

`agSelectCellEditor`

```ts
cellEditorParams: {
  values: ['Alta', 'Media', 'Baja'], // opciones del dropdown
}
```

`agLargeTextCellEditor`

```ts
cellEditorParams: {
  maxLength: 500,                 // longitud máxima
  rows: 10,                       // alto del textarea
  cols: 60                        // ancho del textarea
}
```

> NOTA: Para las columnas numéricas editables de `Promesa`, se eligió para validar el dato capturado el `valueParser` en lugar de los `cellEditorParams` ya que se necesitaban informar al usuario con retroalimentación acerca de qué había hecho "mal".

### 3. Test

1. Muestra tu grid con la definición de columnas de columnas editables.
2. Puedes entrar al **modo captura** presionando `F2`, `ENTER`, o hacer `DOBLE CLIC` en las celdas editables.
3. Para confirmar el valor capturado en una celda editable, presiona `ENTER`.

### 4. `cellValueChanged` - Evento de cambio de valor en una celda

Una vez configurado tus columnas y tu grid para ser editable, el método al que le tenemos que prestar atención es `cellValueChanged`, y en el HTML del ag-grid-angular se declara así:

```html
<ag-grid-angular tourAnchor="step-registros-pcxc-grid" style="flex: 1 1 0; width: 100%; min-height: 130px;" class="ag-theme-quartz"
    [gridOptions]="registrosPcxcGridOptions"
    ...
    [rowData]="registrosPcxcData"
    ...
    (gridReady)="onRegistrosPcxcGridReady($event)"
    (cellValueChanged)="onRegistrosPcxcCellValueChanged($event)">
</ag-grid-angular>
```

Este evento y su instancia da la siguiente información, vamos a usar un ejemplo de cambios de promesas y compromisos:

```ts
onRegistrosPcxcCellValueChanged(event: CellValueChangedEvent) {
    // Contenido del evento:
    // 1. data: objeto de los datos de la fila en la que se encuentra la celda que disparó el evento
    // 2. colDef: Definición de la columna de la celda que disparó el evento.
    // 3. newValue/oldValue: Valores antes y después de confirmar el dato ingresado.
    const { data, colDef, newValue, oldValue } = event;
    ...
}
```

---

Con esta información, ya podrás hacer un plan para la edición de los datos mostrados en tu AG Grid, y como mero ejemplo, aquí tienes el link a `qa` de las Promesas de Cobranza en que se aplicó todo lo que está en esta documentación:

[`Consulta Auxiliares / Gerente / Seguimiento Cobranza / Captura Promesas`](https://github.com/DevElektron/AdministraWeb-Front/tree/qa/src/app/modules/conaux/gerente/seguimiento-cobranza/captura-promesas/components/captura-promesas-cobranza).
