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
