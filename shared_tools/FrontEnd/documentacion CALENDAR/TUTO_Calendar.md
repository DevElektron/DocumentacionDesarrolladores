# 📅 COMPONENTE `app-date-range-select`

## 🎯 Descripción
Selector de fechas nivel empresarial completamente configurable. Permite seleccionar una fecha única o un rango de fechas mediante click, drag o inputs rápidos.

## 📦 Instalación
`import { DateRangeSelectComponent } from 'src/app/shared/ui/components/date-range-select/date-range-select.component';`

## 🛠️ Parámetros de Entrada (`@Input`)
| Parámetro | Tipo | Default | Descripción |
|-----------|------|---------|-------------|
| `dualCalendar` | `boolean` | `true` | `true` = Rango, `false` = Fecha única |
| `dragEnabled` | `boolean` | `true` |`true` = Habilita selección por arrastre, `false` = Habilita selección por click |
| `showQuickDateInputs` | `boolean` | `true` | Muestra inputs de captura rápida de fecha|
| `containerWidth` | `string` | `'100%'` | Ancho del contenedor selector apertura calendario |
| `calendarScale` | `string` | `'1'` | Escala del calendario (`'0.7'` = 70%), (`'1'` = 100%) |
| `dropdownPosition` | `string` | `'bottom'` | `'bottom'`, `'top'`, `'left'`, `'right'`, `'bottom-left'`, `'bottom-right'`, `'top-left'`, `'top-right'` |
| `restrictFutureDates` | `boolean` | `false` | Restringe fechas futuras |
| `selectsEnabled` | `boolean` | `true` | Habilita selects mes/año |
| `selectsVisible` | `boolean` | `true` | Muestra selects mes/año |
| `navigationEnabled` | `boolean` | `true` | Habilita flechas navegación |
| `navigationVisible` | `boolean` | `true` | Muestra flechas navegación |
| `rangeColor` | `string` | `'#d7453a'` | Color del rango |
| `startEndColor` | `string` | `'#8b1a1a'` | Color  rango de fechas seleccionadas inicio/fin |
| `labelStart` | `string` | `'Fecha inicial'` | Etiqueta calendario izquierdo |
| `labelEnd` | `string` | `'Fecha final'` | Etiqueta calendario derecho |
| `placeholder` | `string` | `'Seleccionar Fechas'` | Placeholder input |
| `initialStartDate` | `string` | - | Fecha inicial (`yyyy-mm-dd`) |
| `initialEndDate` | `string` | - | Fecha final (`yyyy-mm-dd`) |
| `minDate` | `Date` | - | Fecha mínima |
| `maxDate` | `Date` | - | Fecha máxima |

## 📤 Eventos de Salida (`@Output`)
| Evento | Tipo | Descripción |
|--------|------|-------------|
| `dateChange` | `{ start: Date\|null, end: Date\|null }` | Se emite al completar selección |

## 💻 Ejemplo de Uso
### HTML
```html
    <app-date-range-select
      [dualCalendar]="true"
      [dragEnabled]="true"
      [showQuickDateInputs]="true"
      [containerWidth]="'240px'"
      [calendarScale]="'0.7'"
      [dropdownPosition]="'left'"
      [restrictFutureDates]="true"
      [selectsEnabled]="true"
      [selectsVisible]="true"
      [navigationEnabled]="true"
      [navigationVisible]="true"
      [rangeColor]="'#d7453a'"
      [startEndColor]="'#8b1a1a'"
      placeholder="Rango de Fechas"
      labelStart="Fecha Inicial"
      labelEnd="Fecha Final"
      [initialStartDate]="startDate"
      [initialEndDate]="endDate"
      (dateChange)="onDateFilter($event)">
    </app-date-range-select>
  </div>
```

## 💻 Ejemplo de Uso
### HTML
```Typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { DateRangeSelectComponent } from 'src/app/shared/ui/components/date-range-select/date-range-select.component';

@Component({
  selector: 'app-consulta-facturacion-articulos',
  standalone: true,
  imports: [DateRangeSelectComponent, CommonModule],
  templateUrl: './consulta-facturacion-articulos.component.html',
  styleUrls: ['./consulta-facturacion-articulos.component.css']
})
export class ConsultaFacturacionArticulosComponent {
  
  // Fechas iniciales
  startDate = '2026-01-05';
  endDate = '2026-03-12';
  
  // Fechas seleccionadas para mostrar
  selectedStartDate: Date | null = null;
  selectedEndDate: Date | null = null;
  
  /**
   * Maneja el cambio de fechas (modo drag)
   */
  onDateFilter(event: { start: Date | null; end: Date | null }) {
    console.log('🎯 Fechas seleccionadas (Drag Mode):', event);
    
    this.selectedStartDate = event.start;
    this.selectedEndDate = event.end;
    
    if (event.start && event.end) {
      // Formatear para API
      const formatted = this.formatForApi(event.start, event.end);
      console.log('📅 Formato API:', formatted);
      
      // Aquí puedes hacer la llamada a tu servicio
      // this.facturacionService.consultar(formatted.start, formatted.end);
    }
  }
  
  
    /**
   * Formatea fechas para envío a API
   */
  private formatForApi(start: Date, end: Date): { start: string; end: string } {
    const format = (date: Date): string => {
      const year = date.getFullYear();
      const month = String(date.getMonth() + 1).padStart(2, '0');
      const day = String(date.getDate()).padStart(2, '0');
      return `${year}-${month}-${day}`;
    };
    
    return {
      start: format(start),
      end: format(end)
    };
  }
}
```

