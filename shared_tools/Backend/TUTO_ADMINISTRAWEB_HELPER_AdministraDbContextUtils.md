# 📏 Uso del Helper `AdministraDbContextUtils`

## 🎯 ¿Qué hace?

El helper `AdministraDbContextUtils` contiene métodos para obtener los objetos de la BD de ElektonSQL dedicados a las siguientes tablas de configuraciones: 

1. `ELCTRL`
2. `ELCTRLN`

---

## 🛠️ ¿Cómo implementarlo?

### 1. Importar en tu clase C# el siguiente `using`: 

```csharp
using AdministraWebAPI.Shared.Helpers;
```

### 2. Usar inyección de dependencias con la interface `IAdministraDbContextUtils` y su clase implementadora `AdministraDbContextUtils`:

```csharp
public class TuClase : ITuClase
{
    private readonly AdministraDbContextUtils _dbUtils;

    public TuClase(AdministraDbContextUtils dbUtils)
    {
        _dbUtils = dbUtils;
    }
    ...
```

### 3. Usar la instancia de `AdministraDbContextUtils` con sus métodos que reciben el código de la configuración a obtener:

```csharp
var ctrl = await _dbUtils.obtenerConfiguracionesElctrlAsync("A");
var ctrlns = await _dbUtils.obtenerConfiguracionesElctrlnAsync(1);
```

Esto es el equivalente a las siguiente consultas `SQL`:

```sql
SELECT
    *
FROM
    ELCTRL
WHERE
    CODIGO = 'A'

SELECT
    *
FROM
    ELCTRLN
WHERE
    CODIGO = 1
```

## ✅ Ejemplo completo en una clase de capa Repositorio (Acceso a Datos de BD)

```csharp
using AdministraWebAPI.Core.Data;
using AdministraWebAPI.Modules.ConAux.Vendedor.EstadisticaVentasVendedores.Application.DTOs.Requests;
using AdministraWebAPI.Modules.ConAux.Vendedor.EstadisticaVentasVendedores.Application.DTOs.Responses;
using AdministraWebAPI.Modules.ConAux.Vendedor.EstadisticaVentasVendedores.Domain.Interfaces;
using AdministraWebAPI.Shared.Helpers;
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace AdministraWebAPI.Modules.ConAux.Vendedor.EstadisticaVentasVendedores.Infrastructure.Repositories;

public class EstadisticaVentasVendedoresRepository : IEstadisticaVentasVendedoresRepository
{
    private readonly AdministraDbContext _context;
    private readonly AdministraDbContextUtils _dbUtils;

    public EstadisticaVentasVendedoresRepository(AdministraDbContext context, AdministraDbContextUtils dbUtils)
    {
        _context = context;
        _dbUtils = dbUtils;
    }

    #region Top Cobranza
    public async Task<List<CobranzaDto>> obtenerTopCobranzaAsync(TopCobranzaRequest request)
    {
        var ctrl = await _dbUtils.obtenerConfiguracionesElctrlAsync("A");
        var ctrlns = await _dbUtils.obtenerConfiguracionesElctrlnAsync(1);

        // Si el mes y año no vienen en request, dar valores predeterminados.
        if (request.Anio == null)
        {
            request.Anio = Convert.ToInt32(ctrl.CieAnoactual);
        }
        if (request.Mes == null)
        {
            request.Mes = Convert.ToInt32(ctrl.CieMesactual);
        }

        // Top de registros de ventas por cliente (ELVXC + ELCTE<NcteNavigation>).
        var periodo = (request.Anio!.Value * 100) + request.Mes!.Value;
        var query = _context.Elvxcs
            .Include(v => v.NcteNavigation)
            .Where(v => v.Periodo == periodo);

        // Si es gerente, no necesitamos nven (número de vendedor).
        if (request.EsGerente && request.NCzo.HasValue)
        {
            query = query.Where(v => v.Nczo == request.NCzo.Value)
                .OrderBy(v => v.Nczo)
                .ThenBy(v => v.Periodo)
                .ThenByDescending(v => v.Xrecuperar);
        }
        // Si es vendedor, no necesitamos nczo (zona).
        else if (!request.EsGerente && request.NVen.HasValue)
        {
            query = query.Where(v => v.Nven == request.NVen.Value)
                .OrderBy(v => v.Nven)
                .ThenBy(v => v.Periodo)
                .ThenByDescending(v => v.Xrecuperar);
        }

        var max = Math.Max(request.MaxRegistros, 1);

        var topCobranza = await query
            .Take(max)
            .Select(v => new CobranzaDto
            {
                NCte = v.Ncte,
                Nombre = v.NcteNavigation.Nombre ?? "",
                Presupuesto = v.Presupuesto,
                Recuperado = v.Recuperado,
                PorcentajeRecuperado = v.Presupuesto > 0 ? v.Recuperado * 100 / v.Presupuesto : 0,
                xRecuperar = v.Xrecuperar,
                Tendencia = v.Presupuesto > 0
                    ? (((v.Recuperado / ctrlns.DiasHabTrans) * ctrlns.DiasHabTotales) / v.Presupuesto) * 100
                    : 0
            })
            .ToListAsync();

        // Necesidad de redondear a 2 decimales.
        var topCobranzaRedondeado2 = new List<CobranzaDto>();
        if (topCobranza.Any())
        {
            var rounder = new DecimalRounder(2);
            topCobranzaRedondeado2 = topCobranza.Select(v => rounder.Redondear(v)).ToList();

            // CASO ESPECIAL: PorcentajeRecuperado muestra valor entero en formato decimal.
            // Redondeo manual solo para PorcentajeRecuperado
            foreach (var c in topCobranzaRedondeado2)
            {
                c.PorcentajeRecuperado = decimal.Parse($"{Math.Round(c.PorcentajeRecuperado, 0, MidpointRounding.AwayFromZero):F2}");
            }
        }

        return topCobranzaRedondeado2;
    }
}
```

## 🔷 Resultado esperado

✅ Objeto con configuraciones de las tablas listadas.

---

## 📝 Nota

> Por el momento sólo están las tablas listadas, ver Shared\Helpers\AdministraDbContextUtils.cs para ver el código completo.

---
