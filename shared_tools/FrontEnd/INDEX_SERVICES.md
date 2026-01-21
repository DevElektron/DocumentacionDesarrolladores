📜 Catálogo de Services Generados

<details open>
<summary><strong>🎯 Objetivo:</strong></summary>

Este documento funciona como índice técnico de los servicios implementados en el ERP. Para cada componente se detalla:

-   Nombre.
-   Ubicación dentro del proyecto.
-   Descripción de funcionamiento general.
-   Servicios proporcionados por este.

El objetivo principal es centralizar la documentación de estos componentes para promover su reutilización, estandarizar su implementación y evitar la creación redundante de nuevos servicios.

</details>

<details>
<summary><strong>📑 [Dialog Service]</strong></summary>

-   **🗂️ Código:** `src\app\shared\services\dialog.service.ts`
-   **📑 Descripción:** Servicio manejador de alertas.
-   **🧾 Servicios:**
    -   `confirm`: Muestra un diálogo de confirmación.
    -   `error`: Muestra un diálogo de error, con un parámetro opcional `err?` para enviar el objeto de la excepción capturada para complementar el mensaje y hacer un mejor inicio de la depuración.
    -   `success`: Muestra un diálogo de operación correcta.
    -   `warning`: Muestra un diálogo de advertencia o aviso.
    -   `observations`: Muestra un diálogo con observaciones.

</details>

<details>
<summary><strong>📑 [AppFocus Service]</strong></summary>

-   **🗂️ Código:** `src\app\shared\services\app-focus.service.ts`
-   **📑 Descripción:** Servicio que detecta cuando la aplicación vuelve a tener foco (cambio de pestaña o ventana). Combina eventos `visibilitychange` y `focus` para máxima compatibilidad entre navegadores. Esencial para mantener datos actualizados en aplicaciones ERP con múltiples pestañas.
-   **🧾 Servicios:**
    -   `onFocus(): Observable<void>`: Observable que emite cuando la aplicación recupera el foco.

**Características:**
-   Debounce de 200ms para evitar múltiples emisiones.
-   Filtra solo eventos donde el documento es visible (`!document.hidden`).
-   `shareReplay(1)` para múltiples suscriptores.
-   Logs en consola para debugging (`🟢 AppFocusService → foco detectado`).

</details>

<details>
<summary><strong>📑 [AgGridRefresh Service]</strong></summary>

-   **🗂️ Código:** `src\app\shared\services\ag-grid-refresh.service.ts`
-   **📑 Descripción:** Servicio especializado para refrescar automáticamente grids AG-Grid cuando la aplicación recupera el foco. Preserva el estado actual (filtros, ordenamiento) durante la recarga. Soporta los tres modos principales de AG-Grid.
-   **🧾 Servicios:**
    -   `register(options): void`: Registra un grid para refresco automático por foco.

**Parámetros de registro:**
-   `gridApi`: Instancia de GridApi de AG-Grid.
-   `mode`: Tipo de grid (`'client' | 'server' | 'infinite'`).
-   `refreshFn`: Función opcional para recarga personalizada (modo `client`).
-   `destroy$`: Subject para limpieza automática.

**Características:**
-   Throttle de 3 segundos para prevenir abuso.
-   Preserva filtros y ordenamiento en modo `client`.
-   Estrategias específicas por cada modo de grid.
-   Logs en consola para debugging.
-   Integración simple con el ciclo de vida del componente.

</details>

## 🎯 Patrón de Uso Recomendado

### Implementación en Componente con AG-Grid:

```typescript
export class MiGridComponent implements OnInit, OnDestroy {
  private gridApi!: GridApi;
  private destroy$ = new Subject<void>();
  
  constructor(private gridRefresh: AgGridRefreshService) {}
  
  onGridReady(params: GridReadyEvent) {
    this.gridApi = params.api;
    
    // Registrar refresco por foco
    this.gridRefresh.register({
      gridApi: this.gridApi,
      mode: 'server', // o 'client' o 'infinite'
      destroy$: this.destroy$
    });
  }
  
  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
