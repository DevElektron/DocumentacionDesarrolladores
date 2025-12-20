# 📏 Uso del Helper `Converters`

## 🎯 ¿Qué hace?

El helper `Converters` contiene métodos para el tratamiento de fechas entre el formato Clarion (formato de número entero) y .NET (clases DateTime y TimeOnly), conformado por 4 funciones:

1. `public static DateTime? IntToDate(int date)`: Convierte fecha de Clarion a .NET `DateTime`.
2. `public static TimeOnly IntToTime(int time)`: Convierte hora o `timestamp` de Clarion a .NET `TimeOnly`.
3. `public static int DateToInt(DateTime fecha)`: Convierte un DateTime de C# a entero Clarion de fecha (días desde 1801-01-01 menos 4 días).
4. `public static int TimeToInt(TimeOnly hora)`: Convierte un TimeOnly de C# a entero Clarion-time (centésimas de segundo desde medianoche).

---

## 🛠️ ¿Cómo implementarlo?

### 1. Importar en tu clase C# el siguiente `using`

```csharp
using [TuBackend].Helpers.Modulo.Application.Services;
```

Donde `[TuBackend]` es el root de los `namespace` del proyecto (BackVentas, BackConsultaAux, etcétera), para que quede claro que se reemplaza según el backend.

### 2. Llamar a las funciones con una llamada estática a `Converters`

```csharp
var fechaClarion = Converters.DateToInt(DateTime.Now());
var fechaDotNet = Converters.IntToDate(82108);
var timeStampClarion = Converters.TimeToInt(TimeOnly.FromDateTime(DateTime.Now()));
var timeStampDotNet = Converters.IntToTime(3875535);
```

> NOTA: Recuerda que el tipo de dato devuelto por la función `DateToInt` es `opcional (?)`, por lo que deberás de comprobar si el resultado es nulo o no: `if (fechaClarion.HasValue)` o `if (fechaClarion == null)`.

## 🔷 Resultado esperado

✅ Valor resultado de la conversión de parámetros de entrada de las funciones a tipo Clarion / .NET según corresponda, ej:

```csharp
Converters.IntToDate(82108); // Devuelve 2025-10-17
```

---

## 📝 Notas

1. Cada una de las 4 funciones fue _traducida_ a código C# de su versión Transact SQL, las cuales puedes encontrar en la instancia de SqlServer 2019 que usa el proyecto `AdministraWeb`, ejemplo:

    - `TimeToInt` es equivalente a `dbo.fn_TIMEtoInt` en SQL Server.
    - `DateToInt` es equivalente a `dbo.fn_DATEtoInt` en SQL Server.

2. En el Backend de `AdministraWeb` hay funciones en el archivo `Helpers\AppDbContext.Extension.cs` que tienen el mismo propósito:

    1. `public DateTime? Fn_IntToDATE(int? fcDocto)`
    2. `public int? Fn_DATEtoInt(DateTime? date)`
    3. `public TimeOnly? Fn_IntToTIME(int? fcDocto)`
    4. `public int? Fn_TIMEtoInt(TimeOnly? timeOnly)`

    Su diferencia es que su uso es con `LINQ` de C# + Entity Framework Core, ejemplo:

    ```csharp
    // `context` es una instancia del `AppDbContext`.
    var dias = await context.Eldia
        .Where(e =>
            e.FcDia >= context.Fn_DATEtoInt(inicio) &&
            e.FcDia <= context.Fn_DATEtoInt(fin)
        )
        .SumAsync(e => (decimal?)e.ValorVta) ?? 0m;
    ```

    Y **NO** se pueden usar fuera del contexto de una consulta `LINQ`, ejemplo:

    ```csharp
    // Este código te dará error debido a que es una asignación
    // de valor en C#, y no una  operación con LINQ.
    var fechaClarion = context.Fn_DATEtoInt(DateTime.Now());
    ```

---
