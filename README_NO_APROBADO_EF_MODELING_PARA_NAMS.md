## Proyecto Migración ERP Elektron
# Entity Framework Modeling

### Objetivo

Para regenerar los DbContext de la nueva arquitectura de microservicios del proyecto de migración, se ha creado un helper para obtener el comando `dotnet ef` que genera tanto el archivo DbContext como los modelos POCO de tablas determinadas por el archivo de código  de la capa de repositorio parametrizado.

### Prerequisitos

1. Tenemos el proyecto en que generaremos el modeling.

_**Este comando sólo funciona en las rutas que contiene un archivo `*.csproj`, es decir, en un proyecto .NET como tal (si lo ejecutas en una carpeta sin proyecto, te aparecerá el mensaje `No project was found. Change the current working directory or use the --project option.`), teniendo en sus paquetes los 2 siguientes: `dotnet add package Microsoft.EntityFrameworkCore.SqlServer` y `dotnet add package Microsoft.EntityFrameworkCore.Tools`.**_

2. Tener instalado el paquete global de dotnet-ef: `dotnet tool install --global dotnet-ef`.

### Uso del comando dotnet ef para segmentación del mapeado de BD

Este es el comando para modelar las tablas que necesita un proyecto de la migración:

```bash
dotnet ef dbcontext scaffold "TuCadenaDeConexion" Microsoft.EntityFrameworkCore.SqlServer `
  --context NombreDeTuArchivoDbContext `
  --context-dir RutaDeTuCarpetaParaArchivoDbContext `
  --output-dir RutaDeModelosPOCO `
  --no-onconfiguring `
  --force `
  [... Listado de tablas con bandera `-t `] 
```

> NOTA: `POCO` son la siglas de `(Plain Old CLR Objects, por sus siglas en inglés)` son clases simples de .NET que representan entidades en tu proyecto, en otra palabras, las clases que representan tus tablas de la BD. 

Ejemplo:

```bash
PS C:\Users\test\Ruta\A\Tu\Proyecto > dotnet ef dbcontext scaffold "TuCadenaDeConexion" Microsoft.EntityFrameworkCore.SqlServer `
>>   --context AppDbContext_TEST `
>>   --context-dir Helpers `
>>   --output-dir Modulo\Modelos_TEST `
>>   --no-onconfiguring `
>>   -t ELDIA `
>>   -t ELVXC `
>>   -t ELVXV `
>>   -t ELVEN `
>>   -t ELGTE `
>>   -t ELCZO
Build started...
Build succeeded.
Skipping foreign key 'FK_ELGTE_ELALM' on table 'dbo.ELGTE' since principal table 'dbo.ELALM' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELVEN_ELALM' on table 'dbo.ELVEN' since principal table 'dbo.ELALM' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELVEN_ELCAJ' on table 'dbo.ELVEN' since principal table 'dbo.ELCAJ' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELVEN_ELEDO' on table 'dbo.ELVEN' since principal table 'dbo.ELEDO' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELVXC_ELALM' on table 'dbo.ELVXC' since principal table 'dbo.ELALM' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELVXC_ELCTE' on table 'dbo.ELVXC' since principal table 'dbo.ELCTE' was not found in the model. This usually happens when the principal table was not included in the selection set.
```

Como verás, salen algunos mensajes de `Skipping ...`, indicando que algunas llaves secundarias en las tablas especificadas en el comando no fueron puestos en tus modelos POCO. Para agregarlos ejecuta nuevamente el comando con las tablas faltantes SI ES QUE NECESITAS algunas de esas relaciones, vamos a agregar todas las tablas de los mensajes:

```bash
PS C:\Users\test\Ruta\A\Tu\Proyecto > dotnet ef dbcontext scaffold "TuCadenaDeConexion" Microsoft.EntityFrameworkCore.SqlServer `
>>   --context AppDbContext_TEST `
>>   --context-dir Helpers `
>>   --output-dir Modulo\Modelos_TEST `
>>   --no-onconfiguring `
>>   --force
>>   -t ELDIA `
>>   -t ELVXC `
>>   -t ELVXV `
>>   -t ELVEN `
>>   -t ELGTE `
>>   -t ELCZO `
>>   -t ELALM `
>>   -t ELCAJ `
>>   -t ELEDO `
>>   -t ELCTE
Build started...
Build succeeded.
Skipping foreign key 'FK_ELCTE_ELBCO' on table 'dbo.ELCTE' since principal table 'dbo.ELBCO' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELCTE_ELCOOR' on table 'dbo.ELCTE' since principal table 'dbo.ELCOOR' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELCTE_ELCTS' on table 'dbo.ELCTE' since principal table 'dbo.ELCTS' was not found in the model. This usually happens when the principal table was not included in the selection set.
Skipping foreign key 'FK_ELCTE_ELS_USO' on table 'dbo.ELCTE' since principal table 'dbo.SATUSO' was not found in the model. This usually happens when the principal table was not included in the selection set.
```

Ahora, cambiaron los mensajes `Skipping ...`, los cuales indican otras tablas a mapear si es que queremos esas llaves foráneas, muy importante el USO DE LA BANDERA `--force`, ya que sin la bandera los archivos que fueron generados en la anterior ejecución no van a tener mapeada las llaves foraneas de las tablas que agregamos.

**Estos mensajes aparecen porque NO ESTAMOS MAPEANDO TODA LA BD CON ENTITY FRAMEWORK, sino sólo las tablas y relaciones que únicamente necesitamos.**

**Se recomienda que:**

1. **BANDERA `--force`:** Si quieres sobreescribir los archivos generados si son encontrados en las carpetas especificadas, agrega esta bandera.
2. **SIEMPRE REFERENCIA A ARCHIVOS QUE NO SON LOS OFICIALES DEL PROYECTO:** Si observas en el ejemplo, `AppDbContext_TEST` y `Modulo\Modelos_TEST` son elemento a los cuales se les agregó el sufijo `_TEST`, suponiendo que tu archivo DbContext es `AppDbContext.cs` y tu carpeta de Modelos `Modulo\Modelos`. Esto se hace para asegurar queel contenido sea el correcto para tu proyecto.

### Prueba segura

Copiar el siguiente script en un archivo `*.bat`. éste genera un proyecto de consola con lo necesario para que el comando funcione y hagas tus pruebas de este comando:

```bat
dotnet new console -n TestScaffoldRunner
cd TestScaffoldRunner
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```
