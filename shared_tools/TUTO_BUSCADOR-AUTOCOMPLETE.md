# 📏 Uso del `buscador autocomplete`

## 🎯 ¿Qué hace?

Componente reutilizable de entrada tipo autocomplete que despliega un listado personalizado basado en el contexto del módulo (clientes, almacenes, tipos de documento, etc.). Filtra dinámicamente las coincidencias en función del texto ingresado por el usuario y permite seleccionar un elemento del conjunto de resultados

## 🔎 Ubicacioón

Podemos encontrar los autocompletes ya creados en la siguiente ruta del proyecto AdministraWeb-Front: `src\app\shared\ui\autocompleters`

---

## 🛠️ ¿Cómo implementarla?

### 1. Importamos SharedComponentsModule en el module de nuestro componente:

```typescript
import { SharedComponentsModule } from 'src/app/shared/ui/shared-components.module';
```

### 2. Implementamos la etiqueta en el HTML de nuestro componente.

Aqui le asignamos los valores a los parametros del buscador como el control, el tamaño y la accion a realizar al seleccionar una opción. Con estos parametros se construira el autocomplete.

```html
<div class="col-md-4">
    <app-ctasbancarias-autocomplete 
        [control]="form.get('ctabancaria')" 
        [panelWidth]="800"
        [placeholder]="'Buscar Cuenta bancaria...'" 
        (selected)="onCuentaBancariaSelected($event)">
    </app-ctasbancarias-autocomplete>
</div>
```

### 3. En el Typescript de nuestro componente creamos e inicializamos el control que usará el autocomplete.

```typescript
@Component({
  selector: 'app-adddepositospendientes',
  templateUrl: './adddepositospendientes.component.html',
  styleUrl: './adddepositospendientes.component.css'
})
export class AdddepositospendientesComponent implements OnInit, OnDestroy {
  ctabancaria = new FormControl(null,[Validators.required]);   👈 // Aquí declaramos el control

  // [...] Resto del código
}
```

***

# 📏 Uso del Parametro `centered` en Autocompletes

## 🎯 ¿Qué hace?

El parametro `centered` nos permite **determinar la posición** en la que se desplegara un autocomplete, ya sea en el centro del modulo o debajo de su respectivo input.

---

## 🛠️ ¿Cómo implementarla?

### 1. Definir el paramtro en el componente de TypeScript de nuestro autocomplete

```typescript
@Input() centered: boolean = true;
```

### 2. Importamos la clase Injector desde el constructor del componente de TypeScriptde del autocomplete

```typescript
constructor(
    private injector: Injector
  ) { }
``` 

### 3. Añadimos las siguientes lineas al metodo ngOnInit() del componente de TypeScript del autocomplete.

Estas lineas validan si se aplicara la clase encargada de centrar el autocomplete, esto de acuerdo al valor asignado al parametro centered (true o false).

```typescript
ngOnInit(): void {
    //[...] Resto de nuestro codigo
    const provider = this.injector.get(MAT_AUTOCOMPLETE_DEFAULT_OPTIONS);
    provider.overlayPanelClass = this.centered ? 'custom-autocomplete' : '';
  }
```

### 4. Agregamos el parametro a la etiqueta del autocomplete en el HTML del modulo donde lo estamos implementando.

Seteamos el valor en **true** si deseamos que el autocomplete se muestre en el centro del modulo y **false** si deseamos que se despliegue justo debajo de su input.

```html
<app-cliente-autocomplete
    style="width: 450px; min-width: 0;"  
    (selected)="onClienteSelected($event)"
    [control]="cliente"
    [centered] = "false"> <!-- 👈 Aquí agregamos el nuevo parámetro -->
</app-cliente-autocomplete>
```

---

## ✅ Ejemplo completo de TypeScript y HTML

```typescript
export class ClienteAutocompleteComponent implements OnInit, ControlValueAccessor {
  @Input() control: FormControl = new FormControl("");
  @Input() placeholder: string = "Buscar cliente...";
  @Input() panelWidth: number = 500;
  @Input() set clienteInicial(ncte: number | null) {
    this._clienteInicial = ncte;
    if (this.initialized && ncte !== null && ncte !== undefined) {
      this.cargarclienteInicial();
    }
  }
  @Input() centered: boolean = true; 👈 // Aquí declaramos el parametro 

  // [...] Más definiciones para nuestro componente

  constructor(
    private clientesService: ClientesServiceService,
    private injector: Injector 👈 // Importación de clase Injector
  ) { }

  ngOnInit(): void {
    this.initializeAutocomplete();
    this.updateStyles();
    if (this.clienteInicial !== null && this.clienteInicial !== undefined) {
      this.cargarclienteInicial();
    }
    const provider = this.injector.get(MAT_AUTOCOMPLETE_DEFAULT_OPTIONS);
    provider.overlayPanelClass = this.centered ? 'custom-autocomplete' : ''; 👈 // Validación para centrar autocomplete
  }

  // [...] Resto del código
}
```

```HTML
<section class="detail-container">
    <div style="height: 100%; width: 100%; display: flex; justify-content: space-between;">
        <!--Información general cliente-->
        <div style="margin-left: 20px; margin-right: 20px;">
            <div style="display: flex; align-items: center; margin-top: 8px; min-width: 0;">
                <app-cliente-autocomplete
                    style="width: 450px; min-width: 0;"  
                    (selected)="onClienteSelected($event)"
                    [control]="cliente"
                    [centered] = "false"> <!-- 👈 Aquí agregamos el nuevo parámetro -->
                </app-cliente-autocomplete>
            </div>                    
```

***

# 📐 Uso del Parámetro `inputHeight` en Autocompletes

## 🎯 ¿Qué hace?

El parámetro `inputHeight` nos permite **ajustar dinámicamente la altura** del campo de entrada (`input`) en los componentes autocompletables, adaptándose mejor al diseño del formulario o sección en donde se utilice.

---

## 🛠️ ¿Cómo implementarlo?

### 1. Definir el parámetro en el componente de TypeScript del autocomplete

```typescript
@Input() inputHeight: number = 32;
```

> Este valor por defecto es `32`, pero puede modificarse según la necesidad del diseño.

---

### 2. Modificar el HTML del autocomplete

Ubica el `mat-form-field` donde está el `<input>` y reemplázalo por el siguiente fragmento para aplicar la altura dinámica:

```html
<mat-form-field 
  appearance="outline" 
  class="w-100 dynamic-height-field"
  [style.--dynamic-height.px]="inputHeight">
  <mat-label>{{ placeholder }}</mat-label>
  <input 
    matInput 
    [matAutocomplete]="auto" 
    [formControl]="control" 
    [placeholder]="placeholder"
    class="dynamic-height-input"
    (keydown.tab)="onKeyDown($event)" 
    (input)="onInputChange($event)" 
    (blur)="onBlur()">
</mat-form-field>
```

> 👈 Aquí se aplican las clases y estilos necesarios para que funcione la altura dinámica.

---

### 3. Agregar los estilos al archivo CSS del componente

```css
/* Clase para el mat-form-field con altura dinámica */
.dynamic-height-field {
  width: 100% !important;
  height: var(--dynamic-height) !important;
  min-height: var(--dynamic-height) !important;
  max-height: var(--dynamic-height) !important;

  --mat-form-field-container-height: var(--dynamic-height) !important;
  --mat-form-field-container-vertical-padding: 2px !important;

  font-size: 12px !important;
  margin-bottom: 0 !important;
}

/* Input dentro del mat-form-field dinámico */
.dynamic-height-field input {
  height: calc(var(--dynamic-height) - 4px) !important;
  min-height: calc(var(--dynamic-height) - 4px) !important;
  max-height: calc(var(--dynamic-height) - 4px) !important;
  line-height: calc(var(--dynamic-height) - 8px) !important;
  padding: 2px 5px !important;
  font-size: 12px !important;
  box-sizing: border-box !important;
}

/* Wrappers internos de Material */
.dynamic-height-field .mat-mdc-text-field-wrapper {
  height: var(--dynamic-height) !important;
  min-height: var(--dynamic-height) !important;
  max-height: var(--dynamic-height) !important;
}

.dynamic-height-field .mat-mdc-form-field-input-control {
  height: calc(var(--dynamic-height) - 4px) !important;
  min-height: calc(var(--dynamic-height) - 4px) !important;
  max-height: calc(var(--dynamic-height) - 4px) !important;
}

.dynamic-height-field .mat-mdc-form-field-infix {
  height: calc(var(--dynamic-height) - 8px) !important;
  min-height: calc(var(--dynamic-height) - 8px) !important;
  padding: 2px 0 !important;
}

/* Label del mat-form-field */
.dynamic-height-field .mat-mdc-floating-label {
  font-size: 11px !important;
  line-height: normal !important;
}
```

---

## ✅ Ejemplo completo de TypeScript y HTML

```typescript
export class DocumentoAutocompleteComponent implements OnInit, ControlValueAccessor {
  @Input() control: FormControl = new FormControl("");
  @Input() placeholder: string = "Buscar documento...";
  @Input() panelWidth: number = 500;
  @Input() centered: boolean = true;
  @Input() inputHeight: number = 32; 👈 // Declaramos el nuevo parámetro

  constructor(
    private documentosService: DocumentosService
  ) { }

  ngOnInit(): void {
    this.initializeAutocomplete();
    this.updateEstilos();
  }

  // [...] Resto del código
}
```

```html
<section class="detalle-doc">
  <div style="height: 100%; width: 100%; display: flex;">
    <app-documento-autocomplete
      style="width: 400px; min-width: 0;"  
      (selected)="onDocumentoSelected($event)"
      [control]="documento"
      [inputHeight]="40"> <!-- 👈 Aplicamos altura personalizada -->
    </app-documento-autocomplete>
  </div>
</section>
```

---

## 🧪 Casos de uso

```html
<!-- Altura por defecto (32px) -->
<app-documento-autocomplete [control]="miControl"></app-documento-autocomplete>

<!-- Altura personalizada (40px) -->
<app-documento-autocomplete 
  [control]="miControl"
  [inputHeight]="40">
</app-documento-autocomplete>

<!-- Altura reducida (25px) -->
<app-documento-autocomplete 
  [control]="miControl"
  [inputHeight]="25">
</app-documento-autocomplete>
```

---