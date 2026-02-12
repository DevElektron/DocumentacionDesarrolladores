# 📏 Uso de la Directiva `appDecimalValidator`

## 🎯 ¿Qué hace?

La directiva `appDecimalValidator` en Angular funciona para validar campos numericos en la captura con la finalidad de solo capturar numeros y decimales, tambien esta formateado para no recibir mas de un punto como separacion de decimales, podra capturar el punto despues de un numero entero establecidos en los parametros y despues del punto capturar una cantidad de decimales establecidos en los parametros, ademas podremos definir si esta captura acepta campos negativos y positivos o solo positivos.

## 🛠️ ¿Cómo implementarla?

### 1. Agregar referencias en el HTML, caso usado para dar formato a valores con 2 decimales:
### appDecimalValidator [maxEnteros]="12" [maxDecimales]="2" [Negative]="false"
### [maxEnteros]="12" define al maximo de enteros que podra capturar
### [maxDecimales]="2" define el maximo de decimales que podra capturar
### [Negative]="true" solo podra capturar numeros negativos y positivos, 
### [Negative]="false" podra capturar solo numeros positivos omitiendo el signo negativo.

```html
<div class="col-md-4">
   <mat-form-field appearance="outline" class="w-100 extra-small">
     <mat-label>Importe Total</mat-label>
     <input matInput placeholder="Importe Total" formControlName="importe" autocomplete="off"
            appDecimalValidator [maxEnteros]="12" [maxDecimales]="2" [Negative]="false" tabindex="5">
   </mat-form-field>
</div>
```

### 2. Como Utilizarlo?

```bash
     ya se encuentra como directiva en la carpeta shared/directives 
```

### 3. Importar el módulo en tu app.
```typescript
    import { DecimalValidatorDirective } from './directives/decimal-validator.directive';

    @NgModule({
    declarations: [
        UppercaseDirective,
        ResizableDirective,
        NumberFormatDirective,
        TableKeyboardNavDirectiveDirective,
        DecimalValidatorDirective
    ],
    imports: [
        CommonModule,
        AgGridAngular,
        ...modules
    ],
    providers: [],
    exports: [...modules, UppercaseDirective, AgGridAngular, ResizableDirective, NumberFormatDirective, TableKeyboardNavDirectiveDirective, DecimalValidatorDirective]
    })
    export class SharedAppModule { }
```