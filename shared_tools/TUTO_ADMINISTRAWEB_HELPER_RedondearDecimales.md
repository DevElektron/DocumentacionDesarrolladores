# 📏 Uso del helper `RedondearDecimales`

## 🎯 ¿Qué hace?

El helper `RedondearDecimales` permite redondear todas las propiedades de un DTO de tipo de dato `decimal` con la precisión indicada en su constructor.

---

## 🛠️ ¿Cómo implementarlo?

### 1. Importar el `using` de los helpers

```csharp
using AdministraWebAPI.Shared.Helpers;
```

### 2. Usar el helper `RedondearDecimales`

Nuestra clase de interés es `DecimalRounder` cuyo constructor recibe cuántos decimales (precisión del dato numérico) usará el objeto de redondeo

```csharp
var rounder = new DecimalRounder(2);
```

En este caso, el objeto de redondeo usar 2 decimales, como para el redondeo de cantidades monetarias o contables. Este objeto contiene el método `Redondear` cuyo parámetro es el DTO  que tiene propedades de tipo `decimal`, ejemplo:

```csharp
public class TotalesVentasPeriodoDto
{
    public decimal TotalObjGte { get; set; }
    public decimal TotalObjetivo { get; set; }
    public decimal TotalVtaHoy { get; set; }
    public decimal TotalCto1Mensual { get; set; }
    public decimal TotalVtaMensual { get; set; }
    public decimal TotalVtaReq { get; set; }
    public decimal TotalLogradox100 { get; set; }
    public decimal TotalDifMes { get; set; }
    public decimal TotalDifDiaria { get; set; }
    public decimal TotalTendPesos { get; set; }
    public decimal TotalTendx100 { get; set; }
    public decimal TotalMargen { get; set; }
}

public class CobranzaDto
{
    public required int NCte { get; set; }
    public required string Nombre { get; set; } = "";
    public required decimal Presupuesto { get; set; }
    public required decimal Recuperado { get; set; }
    public required decimal PorcentajeRecuperado { get; set; }
    public required decimal xRecuperar { get; set; }
    public required decimal Tendencia { get; set; }
}
```

Para usar el método `Redondear` simplemente llámalo:

```csharp
// Redondea todas las propiedades tipo decimales del dto a 2 decimales.
var totalesRedondeados2 = new DecimalRounder(2).Redondear(totales);

// Creando una nueva lista del DTO que tendrá los valores de las propiedades
// redondeadas a 2 decimales, ej. 167.745 → 167.74
topCobranzaRedondeado2 = topCobranza.Select(v => rounder.Redondear(v)).ToList();
```

También puedes especificar qué tipo de redondeo deseas:

```csharp
var rounder = new DecimalRounder(2, MidpointRounding.AwayFromZero);
```

## 🎯 Opciones de `MidpointRounding` en .NET

| Opción                           | Comportamiento                                                                                   | Casos de uso comunes                                                                 |
|----------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| `ToEven` (default)               | Redondea al número par más cercano cuando el valor termina en `.5`                               | Predeterminado en .NET. Ideal para cálculos financieros o estadísticos, evita sesgo acumulativo |
| `AwayFromZero`                   | Redondea `.5` hacia el número más alejado del cero (ej. `2.5 → 3`, `-2.5 → -3`)                   | Presentaciones visuales, KPIs, interfaces donde se espera que `.5` suba consistentemente |
| `ToZero` *(desde .NET 6)*        | Redondea `.5` hacia el número más cercano a cero (ej. `2.5 → 2`, `-2.5 → -2`)                     | Cálculos donde se desea minimizar sobreestimación                                     |
| `ToNegativeInfinity` *(.NET 6)*  | Redondea siempre hacia abajo, incluso si termina en `.5`                                         | Truncamiento o cálculos conservadores                                                 |
| `ToPositiveInfinity` *(.NET 6)*  | Redondea siempre hacia arriba, incluso si termina en `.5`                                        | Cálculos optimistas o márgenes garantizados                                           |

---

## ✅ Ejemplo completo en un método de capa Repositorio

```csharp
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
        // ej. 0 en lugar de 0.00 en propiedad decimal. Redondeo manual solo para PorcentajeRecuperado.
        foreach (var c in topCobranzaRedondeado2)
        {
            c.PorcentajeRecuperado = decimal.Parse($"{Math.Round(c.PorcentajeRecuperado, 0, MidpointRounding.AwayFromZero):F2}");
        }
    }

    return topCobranzaRedondeado2;
}
```

---

## 🔷 Resultado esperado

✅ Valores de propiedades `decimal` redondeados a los decimales especificados.

---

## 📝 Nota

> El helper no afectará a las propiedaes que no sean de tipo `decimal`, pasa los valores a un nuevo objeto del tipo de DTO pasado como argumento en el método `Redondear`.
> Es recomendable el crear otro objeto que recibe el uso del helper como buena práctca para el uso de un debugger.

---
