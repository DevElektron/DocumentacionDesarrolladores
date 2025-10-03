# FLUJO DE TRABAJO GIT + GITHUB

## ÍNDICE

- [COMIENZO DE UN NUEVO TRABAJO](#comienzo-de-un-nuevo-desarrollo).
- [EJEMPLO DE FLUJO](#ejemplo-de-flujo).
- [RESOLUCIÓN DE CONFLICTOS](#resolución-de-conflictos)
- [POLÍTICAS](#políticas)

## COMIENZO DE UN NUEVO DESARROLLO

Para estandarizar nuestra forma de trabajo en el proyecto de migración, sigue estos pasos al iniciar un nuevo desarrollo:

0. Entrar en tu terminal de preferencia (PS, cmd, etcétera).
1. Navega con el comando `cd` hasta el directorio de tu repositorio (ya sea del `Back` o `Front`).
2. Ejecuta un `git branch` para comprobar la rama en la que se encuentra tu repositorio local.
3. Si `git branch` muestra que tu rama no es `qa`, cámbiate con el comando `git checkout qa`, de lo contrario no es necesario que ejecutes ese comando.
4. Traer cambios de rama `qa` con `git pull origin qa` para que tu `qa` local esté actualizada con el trabajo de los demás desarrolladores.
5. Crear tu rama (de acuerdo a las convenciones del README [**Git & Flujo de Trabajo**](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/src/app/modules/ventas/pedidos/ventana-bot%C3%B3n-pr%C3%B3rroga/Ventana%20Bot%C3%B3n%20Pr%C3%B3rroga.md#-git--flujo-de-trabajo)) a partir de tu rama actualizada `qa` con el comando: `git checkout -b tipo_rama/nombre_de_tu_rama`.
6. Trabajar en tu desarrollo con comandos de `git` para guardar tu progreso:

    0. `git branch`: Muestra la rama de trabajo de tu repositorio local.
    1. `git status`: Muestra si tu rama ha cambiado respecto al código y/o archivos del repositorio del último commit.
    2. `git log`: Muestra cada conjunto de cambios (`commit`) que se ha realizado en tu repositorio descargado identificados por un hash, ejemplo:

        ```bash
        commit 528b8baee6925d9361950938140fb7917208770d (HEAD -> fix/migracion_validaciones_tempranas, origin/qa, qa)
        Merge: 8358136 23ec7e1
        Author: Eduardo Navarro Carranza <janavarro@elektron.com.mx>
        Date:   Thu Oct 2 16:55:53 2025 -0600

            Merge pull request #33 from DevElektron/fix/ExistenciaArticulos-Turnos

            Fix/ExistenciaArticulosTurnos
        ```

    3. `git add`: Le indica a `git` que tome en cuenta uno o más archivos para el siguiente commit.
    4. `git diff`: Muestra las líneas cambiadas de un archivo o archivos entre el último commit y el archivo actual (_NOTA: Sólo funciona si no haz hecho `git add`_)
    5. `git commit`: Le indica a `git` que lo que has hecho con `git add` sea puesto en un nuevo commit.

    > NOTA: Se recomienda una nomeclantura en tus mensajes de commits con un prefijo3entre corchetes:

    - [ADD]: Se han agregados cambios a nuevos archivos o líneas de código.
    - [DEL]: Se eliminaron archivos o líneas de código.
    - [UPD] o [MOD]: Actualización de archivos o código ya existentes.
    - [IMP]: Se hacen mejoras.
    - [FIX]: ¿Qué se reparo con tus cambios?
    - [MERGE]: Merge con alguna rama.

7. Una vez que hayas acabado con tu desarrollo (incluyendo tus pruebas), traer cambios de rama `qa` con `git pull origin qa` para que tu nuevo desarrollo local esté actualizada con los cambios que algunos del equipo podría haber hecho mientras tú estabas trabajando en tu desarrollo, de ser necesario resolver conflictos (ver [**RESOLUCIÓN DE CONFLICTOS**](#resolución-de-conflictos)).
8. Hacer otra tandas de las misma pruebas de tu desarrollo, esto para comprobar que sigue funcionando con los cambios guardados en la rama `qa` de los demás desarrolladores. De ser necesario de que observes una diferencia con tu trabajo y su funcionamiento _solicitar asistencia_.
9. Sube tus commits con tu trabajo al servidor/hosting (`GitHub`) de donde bajaste el repositorio ejecutando `git push origin tipo_rama/nombre_de_tu_rama`.
10. Verificar que tu rama `tipo_rama/nombre_de_tu_rama` y tu trabajo estén en el repositorio designado en el servidor/hosting (`GitHub`).
11. Crear un PR (pull request) en el servidor/hosting (`GitHub`) con rama `base` = `qa` y rama `compare` = `tipo_rama/nombre_de_tu_rama`.
12. Indicar a los desarrolladores del repositorio tu PR y esperar a revisión por parte del equipo de tu PR.
13. Si la revisión salió correcta, se hará `MERGE` a `qa` (fusión de cambios de tu trabajo a la rama base) con los cambios de `tipo_rama/nombre_de_tu_rama`, de lo contrario resolver puntos indicado por los desarrolladores encargados de hacer el MERGE.

## EJEMPLO DE FLUJO

**_Desarrollo:_** _Migración de validaciones tempranas de capa Controller a capa Servicio._

0. Entrar en tu terminal de preferencia (ej. PowerShell) y navega con el comando `cd` hasta el directorio de tu repositorio:

```PowerShell
PS C:\ > cd Documents\Proyectos\Backend
PS C:\Documents\Proyectos\Backend >
```

1. Ejecuta un `git status` para comprobar la rama en la que se encuentra tu repositorio (el * indica rama actual).

```PowerShell
PS C:\Documents\Proyectos\Backend > git branch
  feature/boton123_backorders_ElDalmTAT_NAMS
  feature/botonProrroga_pedidos_ElActPdcA
  feature/tableroVentasVendedor_ElConVxv
  feature/tableroVentasVendedor_ElConVxv_NAMS
  fix/connIdRequired_estadistica_ventas_vendedores
* fix/validaciones_tempranas_requests_BackVentasConAux
  main
  qa
PS C:\Documents\Proyectos\Backend >
```

2. Si `git branch` muestra que tu rama no es `qa`, cámbiate con el comando `git checkout qa`, de lo contrario no es necesario que ejecutes ese comando. Ejecutamos antes `git status` para comprobar que `si no hay cambios` previos que guardar con el mensaje `nothing to commit, working tree clean`, de lo contrario `git checkout` no funcionará por ello:

```PowerShell
PS C:\Documents\Proyectos\Backend > git status
On branch fix/validaciones_tempranas_requests_BackVentasConAux
nothing to commit, working tree clean
PS C:\Documents\Proyectos\Backend > git checkout qa
Updating files: 100% (258/258), done.
Switched to branch 'qa'
Your branch is behind 'origin/qa' by 32 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
PS C:\Documents\Proyectos\Backend > 
```

3. Traer cambios de rama `qa` con `git pull origin qa` para que tu `qa` local esté actualizada con el trabajo de los demás desarrolladores. `origin` es el alias del repo en el servidor/hosting = `https://github.com/DevElektron/msadministraweb`.

```PowerShell
PS C:\Documents\Proyectos\Backend > git pull origin qa
remote: Enumerating objects: 324, done.
remote: Counting objects: 100% (254/254), done.
remote: Compressing objects: 100% (65/65), done.
remote: Total 172 (delta 94), reused 170 (delta 94), pack-reused 0 (from 0)
Receiving objects: 100% (172/172), 48.49 KiB | 258.00 KiB/s, done.
Resolving deltas: 100% (94/94), completed with 45 local objects.
From https://github.com/DevElektron/msadministraweb
 * branch            qa         -> FETCH_HEAD
   c047024..528b8ba  qa         -> origin/qa
Updating a84a092..528b8ba
Updating files: 100% (279/279), done.
Fast-forward
 .gitignore                                         |    2 +
 BackArchivo/.config/dotnet-tools.json              |   13 +
 BackArchivo/BackArchivo.csproj                     |    4 +-
 BackArchivo/Helpers/AppDbContext.Extension.cs      |   20 +
 ...
 276 files changed, 5940 insertions(+), 8689 deletions(-)
 create mode 100644 BackArchivo/.config/dotnet-tools.json
 create mode 100644 BackArchivo/Helpers/AppDbContext.Extension.cs
 create mode 100644 BackArchivo/Helpers/ConnectionMiddleware.cs
 ...
PS C:\Documents\Proyectos\Backend >
```

4. Crear tu rama (de acuerdo a las convenciones del README [**Git & Flujo de Trabajo**] a partir de tu rama actualizada `qa` con el comando: `git checkout -b tipo_rama/nombre_de_tu_rama`, `-b` es para _crear rama a partir de rama actual_.

```PowerShell
PS C:\Documents\Proyectos\Backend > git branch
  feature/boton123_backorders_ElDalmTAT_NAMS
  feature/botonProrroga_pedidos_ElActPdcA
  feature/tableroVentasVendedor_ElConVxv
  feature/tableroVentasVendedor_ElConVxv_NAMS
  fix/connIdRequired_estadistica_ventas_vendedores
  fix/validaciones_tempranas_requests_BackVentasConAux
  main
* qa
PS C:\Documents\Proyectos\Backend > git checkout -b fix/migracion_validaciones_tempranas
Switched to a new branch 'fix/migracion_validaciones_tempranas'
PS C:\Documents\Proyectos\Backend > git branch                                   
  feature/boton123_backorders_ElDalmTAT_NAMS
  feature/botonProrroga_pedidos_ElActPdcA
  feature/tableroVentasVendedor_ElConVxv
  feature/tableroVentasVendedor_ElConVxv_NAMS
  fix/connIdRequired_estadistica_ventas_vendedores
* fix/migracion_validaciones_tempranas
  fix/validaciones_tempranas_requests_BackVentasConAux
  main
  qa
PS C:\Documents\Proyectos\Backend >
```

5. Trabajar en tu desarrollo con comandos de `git` para guardar tu progreso:

```PowerShell
PS C:\Documents\Proyectos\Backend > git status
On branch fix/migracion_validaciones_tempranas
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   BackVentas/Helpers/AppDbContext.Extension.cs
        modified:   BackVentas/Program.cs
        modified:   Gateway/Program.cs

no changes added to commit (use "git add" and/or "git commit -a")
# Si ejecutas sólo `git diff` te mostrará todas las diferencias de todos los archivos que tiene cambios.
PS C:\Documents\Proyectos\Backend > git diff
diff --git a/BackVentas/Helpers/AppDbContext.Extension.cs b/BackVentas/Helpers/AppDbContext.Extension.cs
index 3e6ad5a..97119f8 100644
--- a/BackVentas/Helpers/AppDbContext.Extension.cs
+++ b/BackVentas/Helpers/AppDbContext.Extension.cs
@@ -33,7 +33,7 @@ namespace BackVentas.Helpers
         // sólo se registra como un modelo POCO de sólo lectura.
         public DbSet<BackordersDetPedidoPartidaDto> BackordersDetPedidoPartida { get; set; }

-        protected override void OnModelCreatingPartial(ModelBuilder modelBuilder)
+        partial void OnModelCreatingPartial(ModelBuilder modelBuilder)
         {
# `git add "archivo1 archivo2"` sólo te tomará en cuenta esos "archivo1 archivo2" ...
PS C:\Documents\Proyectos\Backend> git add BackVentas/Helpers/AppDbContext.Extension.cs
# Crea un commit como todos los `git add` que has hecho, puedes poner lo que sea de mensaje. Cuando no haz hecho ningún `git add` desde tu último commit, te aparecerá `no changes added to commit (use "git add" and/or "git commit -a")`:
PS C:\Documents\Proyectos\Backend> git commit -m "[FIX] BackVentas: AppDbContext Extensions en su OnModelCreatePartial ya no es override."
[fix/migracion_validaciones_tempranas 977554e] [FIX] BackVentas: AppDbContext Extensions en su OnModelCreatePartial ya no es override.
 1 file changed, 1 insertion(+), 1 deletion(-)
PS C:\Documents\Proyectos\Backend> git status
On branch fix/migracion_validaciones_tempranas
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   BackVentas/Program.cs
        modified:   Gateway/Program.cs

no changes added to commit (use "git add" and/or "git commit -a")
# `git add .` te tomará en cuenta para tu nuevo commit todos los cambios de todos los archivos que aparezca en tu `git status`
PS C:\Documents\Proyectos\Backend> git add .
PS C:\Documents\Proyectos\Backend> git commit -m "[FIX] Ventana Botón Prórroga: Servicios y Repositorios registrados de nuevo por detalle en MERGE con QA."
[fix/migracion_validaciones_tempranas 5047a7a] [FIX] Ventana Botón Prórroga: Servicios y Repositorios registrados de nuevo por detalle en MERGE con QA.
 2 files changed, 3 insertions(+), 1 deletion(-)
PS C:\Documents\Proyectos\Backend >
```

6. Una vez que hayas acabado con tu desarrollo (incluyendo tus pruebas), ejecuta de nuevo `git pull origin qa` para que tu nuevo desarrollo local esté actualizada con los cambios que algunos del equipo podría haber hecho mientras tú estabas trabajando en tu desarrollo.

```PowerShell
PS C:\Documents\Proyectos\Backend > git pull origin qa
From https://github.com/DevElektron/msadministraweb
 * branch            qa         -> FETCH_HEAD
Already up to date.
```

7. Sube tus commits con tu trabajo al servidor/hosting (`GitHub`) de donde bajaste el repositorio ejecutando `git push origin tipo_rama/nombre_de_tu_rama`.

```PowerShell
PS C:\Documents\Proyectos\Backend > git push origin fix/migracion_validaciones_tempranas
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Delta compression using up to 12 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (11/11), 1.16 KiB | 198.00 KiB/s, done.
Total 11 (delta 9), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (9/9), completed with 7 local objects.
To https://github.com/DevElektron/msadministraweb.git
   426d5ac..5047a7a  fix/validaciones_tempranas_requests_BackVentasConAux -> fix/validaciones_tempranas_requests_BackVentasConAux
PS C:\Documents\Proyectos\Backend >
```

## RESOLUCIÓN DE CONFLICTOS

Cuando terminamos de ejecutar los `git pull origin qa`, git se puede confundir de qué líneas va a fusionar entre la rama de `qa` y tu rama, poniendo una marca de `HEAD` acompañado de un identificador de un commit, ya que en los mismos número de líneas `git` encontró "cosas distintas" entre la rama `qa` y tu rama local, así que debes de indicar a `git` qué cambios conservar y cuáles no.

Para resolver un conflicto en un archivo:

1. Ve a un archivo que aparece en la salida del `git pull origin qa`.
2. Buscar la palabra `HEAD`, y observarás líneas divididas por `>>>>>>>>>`, `========` y `<<<<<<<<<`, un ejemplo_:

    ```csharp
    <<<<<<< HEAD
    builder.Services.AddScoped<IPedidoProrrogaRepository, PedidoProrrogaRepository>();
    =======
    builder.Services.AddScoped<IRecepcionTurnosRepository, RecepcionTurnosRepository>();
    >>>>>>> qa
    ```

    Dónde:

    - `<<<<<<< HEAD` es el inicio de líneas que están en tu rama local.
    - `=======` es el separador de cambios entre tu rama y la rama `qa`.
    - `>>>>>>> qa` es el final de las líneas que trajo el `pull` de `qa`.

    Es la parte que tiene actualmente en tu rama local.

3. Borrar las líneas que contienen `<<<<<<< HEAD`, `=======` y `>>>>>>> qa`, y decide qué lneas deben de quedar en tu version final del archivo. En la gran mayoría de casos, **ambos conjuntos de líneas se deben de conservar**, ya que `qa` es la rama de referencia del equipo.
4. Haz `git add .` > `git commit -m "Tu mensaje de MERGE con rama origin/qa"` y `git push origin tipo_rama/nombre_de_tu_rama` para finalizar con el conflicto del archivo.
5. Repite los pasos del 1 al 4 hasta que no haya conflictos `git push origin tipo_rama/nombre_de_tu_rama`.

> NOTA: RECUERDA, CUANDO HAYA CONFLICTOS EN TU `git pull origin qa` DEBES DE CONSULTAR AL EQUIPO PARA VER EL DESARROLLADOR QUE TIENE QUE VER CON LAS LÍNEAS CONFLICTIVAS DE LOS ARCHIVOS.

## POLÍTICAS

Para reducir lo más posible los conflictos, deberás de hacer:

1. **INICIO DEL DÍA: `git pull origin qa`**.
2. **FINAL DEL DÍA**:
    - `git add .` para indicar a `git` que traqueé todos los cambios.
    - `git commit -m "[DailyUpdate] ..."` con el mensaje del fin del día, un recordatorio de tu trabajo y en qué parte deberás de seguir.
    - `git push origin tipo_rama/nombre_de_tu_rama`** para subir tus cambio a `GitHub` y tener tu código respaldado.
