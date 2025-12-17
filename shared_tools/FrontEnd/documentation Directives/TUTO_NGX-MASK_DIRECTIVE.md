# 📏 Uso de la Directiva `ngx-mask`

## 🎯 ¿Qué hace?

La directiva `ngx-mask` en Angular, para usar máscaras de entrada (input masks), se puede utilizar una biblioteca externa, que es una de las más populares y fáciles de integrar.

## 🔧 ¿Qué es una máscara?

Una máscara de entrada restringe el formato que el usuario puede escribir en un campo de texto. Por ejemplo:

  Teléfono: (999) 999-9999  

  Fecha: dd/MM/yyyy  

  RFC, CURP, etc.  

---

## 🛠️ ¿Cómo implementarla?

### 1. Agregar referencias en el HTML, caso usado para dar formato a valores con 2 decimales:
###    mask="separator.2" thousandSeparator=","    decimalMarker="."

```html
<div class="col-md-4">
   <mat-form-field appearance="outline" class="w-100 extra-small">
     <mat-label>Importe Total</mat-label>
     <input matInput placeholder="Importe Total" formControlName="importe" autocomplete="off"
            `mask="separator.2"` thousandSeparator="," decimalMarker="." tabindex="5">
   </mat-form-field>
</div>
```

### 2. Instalación de Libreria en PowerShell

```bash
     npm install ngx-mask --save
```

### 3. Importar el módulo en tu app.
```typescript
    import { NgxMaskDirective, provideNgxMask } from 'ngx-mask';
    import { provideAnimations } from '@angular/platform-browser/animations';

    @NgModule({
    declarations: [...],
    imports: [..., NgxMaskDirective],
    providers: [provideNgxMask()],
    })
    export class AppModule {}
```
## ✅ Ejemplos de varios formatos

```html
📅 Ejemplo: Fecha (dd/MM/yyyy)
    <input type="text" mask="00/00/0000" placeholder="dd/mm/yyyy" />

☎️ Ejemplo: Teléfono
    <input type="text" mask="(000) 000-0000" />

💳 Ejemplo: Tarjeta de crédito
    <input type="text" mask="0000 0000 0000 0000" />

🎯 Ejemplo: RFC (4 letras + 6 dígitos + 3 alfanuméricos)
    <input type="text" mask="SSSS000000AAA" />
    Donde:
    0: número
    A: letra o número
    S: solo letra

🎛️ Otras opciones
❓Input opcional (con ?)
        <input type="text" mask="0000-0000?0" /> <!-- permite 8 o 9 dígitos -->

🔁 Máscaras dinámicas con Angular
    <input [mask]="mask" />
  
    mask = '0000 0000 0000 0000'; // puedes cambiarla en tiempo real

📌 Tips adicionales
    Puedes usarlo con formControlName si trabajas con Reactive Forms.
    Asegúrate de importar el módulo correctamente en el módulo donde estés usando el input (puede ser un SharedModule si está separado).
    También funciona en formularios con [(ngModel)].
```
