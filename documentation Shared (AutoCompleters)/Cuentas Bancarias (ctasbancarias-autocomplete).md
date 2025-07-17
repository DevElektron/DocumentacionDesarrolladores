# 📏 Uso de listas de Cuentas Bancarias `ctasbancarias-autocomplete`

## 🎯 ¿Qué hace?

Lista las Cuentas Bancarias de los bancos donde nos muestra ID Cuenta Bancaria, Nombre del Banco, Descripción del Banco, Cuenta del Banco, Cuenta Contable y Moneda que maneja el banco.

---

## 🛠️ ¿Cómo implementarla?

### 1. como mandarla a llamar en tu modulo?

Importar en tu componente asi:

```html
    import { SharedComponentsModule } from 'src/app/shared/ui/shared-components.module';
```

### 2. Se Implementa en HTML, con la etiqueta app-ctasbancarias-autocomplete donde el nombre del control es ctabancaria, el panel de ancho a 800px, dentro del input despliega Buscar Cuenta bancaria.. y el evento al seleccionar onCuentaBancariaSelected($event).

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

### 3. En Typescript 

```html
@Component({
  selector: 'app-adddepositospendientes',
  templateUrl: './adddepositospendientes.component.html',
  styleUrl: './adddepositospendientes.component.css'
})
export class AdddepositospendientesComponent implements OnInit, OnDestroy {
    ctabancaria = new FormControl("");

      ngOnInit(): void {
      ctabancaria: [null,[Validators.required]]
    });

  guardar(): void {
      const requestData: RegistrardepositoRequest = {
        ctabancaria: this.form.get('ctabancaria').value?.nctab,
        --aqui tomamos el valor numerico para enviarlo 
      };
  }

 --reiniciar el autocomplete cuentabancaria y respetar el contenido de la lista
 limpiar() {
    this.form.get('ctabancaria')?.reset();
 }
}
```

