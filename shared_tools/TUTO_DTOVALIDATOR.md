# 📏 Uso del Validador `dtoValidator`

## 🎯 ¿Qué hace?

El validador `dtoValidator` permite validar que un valor de un `FormControl` sea un **objeto válido**, con las propiedades esperadas y del tipo correcto.  
Por ejemplo, verificar que el usuario haya seleccionado un **ClienteDto válido** de una lista.

---

## 🛠️ ¿Cómo implementarlo?

### 1. Importar el validador

En el componente donde defines el formulario, importa la función desde el archivo correspondiente (por ejemplo `src/app/shared/validators/dto-validator`):

```ts
import { dtoValidator } from "src/app/shared/validators/dto-validator";
```

---

### 2. Usar el validador en un `FormControl`

Define un `FormControl` en tu `FormGroup` y añade `dtoValidator` junto con otros validadores como `Validators.required`.

Por ejemplo, para validar un `ClienteDto`:

```ts
this.form = this.fb.group({
  cliente: this.fb.control(null, [
    Validators.required,
    dtoValidator({
      id: 'number',
      nombre: 'string'
    })
  ])
});
```

---

## ✅ Ejemplo completo en un componente

```ts
@Component({...})
export class EjemploComponent {
  form: FormGroup;

  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      cliente: this.fb.control(null, [
        Validators.required,
        dtoValidator({
          id: 'number',
          nombre: 'string',
        })
      ])
    });
  }

  guardar() {
    if (this.form.invalid) {
      console.log('Formulario inválido', this.form.errors);
      return;
    }

    console.log('Datos válidos:', this.form.value);
  }
}
```

---

## 🔷 ¿Cómo mostrar el error al usuario?

Puedes leer el error `dtoInvalid` en la plantilla para mostrarlo en un `mat-error` o `matTooltip`.

### Ejemplo con `mat-error`:
```html
<mat-form-field appearance="outline">
  <mat-label>Cliente</mat-label>

  <app-cliente-autocomplete
    [control]="form.get('cliente')"
    (selected)="onClienteSelected()">
  </app-cliente-autocomplete>

  <mat-error *ngIf="form.get('cliente')?.hasError('dtoInvalid')">
    {{ form.get('cliente')?.getError('dtoInvalid') }}
  </mat-error>
</mat-form-field>
```

---

## 🔷 Resultado esperado
✅ Si el valor no es un objeto válido con las propiedades y tipos definidos, se marca el `FormControl` como inválido y el error `dtoInvalid` contiene un mensaje descriptivo.

✅ Si el valor es correcto, el control será válido.

---

## 📝 Nota
- Este validador **no comprueba valores nulos o vacíos** si ya tienes `Validators.required`.
- Puedes reutilizar el validador para otros DTOs simplemente pasando las propiedades y tipos esperados.
- Los tipos válidos son:  
  🔹 `'string'`  
  🔹 `'number'`  
  🔹 `'boolean'`  
  🔹 `'object'`

---
