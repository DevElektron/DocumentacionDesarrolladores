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

`mask="separator.2" thousandSeparator="," decimalMarker="."`

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

### 3. Importar el módulo en tu app

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

## 📌 Notas

1. Para su uso en componentes standalone (Angular 17+):

    ```ts
    import { NgxMaskDirective, provideNgxMask } from 'ngx-mask';

    @Component({
    selector: 'app-demo',
    standalone: true,
    imports: [NgxMaskDirective], // ✅ Importar la directiva aquí
    providers: [provideNgxMask()], // ✅ Proveer aquí también
    template: `<input matInput mask="separator.2" thousandSeparator="," prefix="$ " />`
    })
    export class DemoComponent {}
    ```

2. `ngx-mask` en su configuración clásica para valores monetarios:

    ```html
    <mat-form-field appearance="outline" class="full-width extra-small">
        <mat-label>Importe</mat-label>
            <input matInput
                formControlName="chcP_Importe_FC"
                type="text"
                mask="separator.2"
                thousandSeparator=","
                prefix="$ "
                decimalMarker="."
                style="text-align: right;"
            />
            <mat-error
                *ngIf="
                    infoChequeProtegidoForm
                    .get('chcP_Importe_FC')
                    ?.hasError('required')
                "
                >
                Importe es obligatorio.
            </mat-error>
            <mat-error
            *ngIf="
                infoChequeProtegidoForm
                .get('chcP_Importe_FC')
                ?.hasError('min')
            "
            >
                Debe ser mayor a cero.
            </mat-error>
    </mat-form-field>
    ```

    ```ts
    chcP_Importe_FC: [0, [Validators.required, Validators.min(0.01)]],
    ```

    Sí permite los valores de "$ 1" o "$ 0", más `ngx-mask` sí permite que establezcas tus funciones de eventos (blur), para que conviertas a `toFixed(2)` y así proveas una UX más completa:

    ```html
    <mat-form-field appearance="outline" class="full-width extra-small">
        <mat-label>Importe</mat-label>
            <input matInput
                formControlName="chcP_Importe_FC"
                type="text"
                mask="separator.2"
                thousandSeparator=","
                prefix="$ "
                decimalMarker="."
                style="text-align: right;"
                (blur)="formateaImporte()" />
            <mat-error
                *ngIf="
                    infoChequeProtegidoForm
                    .get('chcP_Importe_FC')
                    ?.hasError('required')
                "
                >
                Importe es obligatorio.
            </mat-error>
            <mat-error
            *ngIf="
                infoChequeProtegidoForm
                .get('chcP_Importe_FC')
                ?.hasError('min')
            "
            >
                Debe ser mayor a cero.
            </mat-error>
    </mat-form-field>
    ```

    donde:

    ```ts
    formateaImporte(): void {
        const control = this.infoChequeProtegidoForm.get('chcP_Importe_FC');
        const value = Number(control?.value);
        if (!isNaN(value)) {
            control?.setValue(value.toFixed(2), { emitEvent: false });
        }
    }
    ```

> 🗓️ **Fecha de última modificación:** 2026-04-07
> 👤 **Eduardo Navarro Carranza, Sergio Tostado**
> 🏷️ **Versión:** 2

---

## Comunicaciones

|Dir|Fecha       |Firma|Comentario                        |
|---|------------|-----|----------------------------------|
|⏩ |2025/07/16  | EC  |Documentacion ngxmask             |
|⏩ |2026/04/04  | ST  |Agregado de notas de Angular +17. |
