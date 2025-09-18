## Proyecto Migración ERP ElektronSQL
# Actualización de ElektronSQL

Para los desarrollos correspondientes a las tareas del proyecto de migración, se necesita actualizar los archivos con extensiones `*.dct *.app *.lib *.dll` y `*.exe` de la unidad de red conectada al equipo del Ing. Francisco Martínez.

## Prerequisitos

Una carpeta compartida a la máquina virtual de Windows XP que contenga Clarion 6 y el proyecto ElektronSQL:

- Estación de trabajo encedido del Ing. Francisco Martínez.
- Carpeta de la máquina virtual `C:\Clarion6\Proyectos\ElektronSQL` conectada a tu local.

Para la actualización, deberás encontrar 2 carpetas en tu local:

**1. CARPETA CON ARCHIVOS DEL PROYECTO:** Esta carpeta contiene todos los archivos del proyecto con el código base de Clarion, la podrás detectar entrando a la configuración de la máquina virtual, en tu local la carpeta deberá de contener la carpeta `Principal` del proyecto, ejemplo:

_Si tu carpeta es `C:\aaaa`, hay una carpeta `Principal`, entonces tu carpeta para la actualización es `C:\aaaa`._

**2. CARPETA DEL EJECUTABLE:** Donde ejecutas en tu local `elsca.exe`, ejemplo:

_Si el acceso directo de tu `elsca.exe` apunta a `C:\APPS\ELSCA`, esta es tu carpeta del ejecutable._

> NOTA: Si no tienes la máquina virtual solicitar apoyo al equipo.

### Script de actualización

Copiar este script en un archivo con extensión `*.bat`, proporcionando los valores de  `rutaProyecto` y `rutaEjecutable` que encontramos en [`Prequisitos`](#prerequisitos), y enseguida ejecutarlo en un shell de Windows (`Powershell` o `CMD`):

```bat
@echo off
:: Salida con Encoding UTF-8. 
chcp 65001 >nul

:: Fase 1 - Preparación
echo ================================
echo [FASE 1] Preparación de entorno
echo ================================
echo [FASE 1] Preparando ...
echo.

set origen=F:
set ipOrigen=192.168.2.145
set rutaProyecto=C:\aaaa
set rutaEjecutable=C:\APPS\ELSCA - copia
set archivos=*.dct *.app *.lib *.dll *.exe
set fecha=%date:~6,4%%date:~3,2%%date:~0,2%
set hora=%time:~0,2%%time:~3,2%%time:~6,2%
set reglog=Recupera_%fecha%_%hora%.log

echo Origen: %origen%
echo Ruta del Proyecto: "%rutaProyecto%"
echo Ruta del ejecutable (elsca.exe): "%rutaEjecutable%"
echo Archivo de Log: %reglog%
echo.

:: Fase 2 - Montado de unidad de red
echo =================================
echo [FASE 2] Montado de unidad de red
echo =================================

echo [FASE 2] Montando %origen% de %ipOrigen% ...
net use %origen% \\%ipOrigen%\Clarion6\Proyectos\ElektronSQL
echo [FASE 2] %origen% montado exitosamente.
echo.

:: Fase 3 - Copia de archivos actualizados
echo =======================================
echo [FASE 3] Copia de archivos actualizados
echo =======================================

echo [FASE 3] Copiando actualización ...
robocopy %origen%\ "%rutaProyecto%" %archivos% /s /log:%reglog%

if %errorlevel% GEQ 16 (
    echo Error fatal. Código: %errorlevel%
) else if %errorlevel% GEQ 8 (
    echo Fallos en algunos archivos. Código: %errorlevel%
) else if %errorlevel% GEQ 1 (
    echo Copia con cambios. Código: %errorlevel%
) else (
    echo Copia sin cambios. Código: %errorlevel%
)

echo.

:: Fase 4 - Desmontado de unidad de red
echo ====================================
echo [FASE 4] Desmontado de unidad de red
echo ====================================

echo [FASE 4] Desmontando ...
net use %origen% /Delete
echo [FASE 4] %origen% desmontado.
echo.

:: Fase 5 - Copia de actualización a local
echo =======================================
echo [FASE 5] Copia de actualización a local
echo =======================================

echo [FASE 5] Copiando ...
robocopy "%rutaProyecto%\Principal" "%rutaEjecutable%"
@echo off
:: Salida con Encoding UTF-8. 
chcp 65001 >nul

:: Fase 1 - Preparación
echo ================================
echo [FASE 1] Preparación de entorno
echo ================================
echo [FASE 1] Preparando ...
echo.

set origen=F:
set ipOrigen=192.168.2.145
set rutaProyecto=C:\aaaa
set rutaEjecutable=C:\APPS\ELSCA - copia
set archivos=*.dct *.app *.lib *.dll *.exe
set fecha=%date:~6,4%%date:~3,2%%date:~0,2%
set hora=%time:~0,2%%time:~3,2%%time:~6,2%
set reglog=Recupera_%fecha%_%hora%.log

echo Origen: %origen%
echo Ruta del Proyecto: "%rutaProyecto%"
echo Ruta del ejecutable (elsca.exe): "%rutaEjecutable%"
echo Archivo de Log: %reglog%
echo.

:: Fase 2 - Montado de unidad de red
echo =================================
echo [FASE 2] Montado de unidad de red
echo =================================

echo [FASE 2] Montando %origen% de %ipOrigen% ...
net use %origen% \\%ipOrigen%\Clarion6\Proyectos\ElektronSQL
echo [FASE 2] %origen% montado exitosamente.
echo.

:: Fase 3 - Copia de archivos actualizados
echo =======================================
echo [FASE 3] Copia de archivos actualizados
echo =======================================

echo [FASE 3] Copiando actualización ...
robocopy %origen%\ "%rutaProyecto%" %archivos% /s /log:%reglog%

if %errorlevel% GEQ 16 (
    echo Error fatal. Código: %errorlevel%
) else if %errorlevel% GEQ 8 (
    echo Fallos en algunos archivos. Código: %errorlevel%
) else if %errorlevel% EQU 3 (
    echo Archivos extra detectados en destino. Código: %errorlevel%
) else if %errorlevel% EQU 1 (
    echo Copia con cambios. Código: %errorlevel%
) else if %errorlevel% EQU 0 (
    echo Copia sin cambios. Código: %errorlevel%
) else (
    echo Resultado inesperado. Código: %errorlevel%
)

echo.

:: Fase 4 - Desmontado de unidad de red
echo ====================================
echo [FASE 4] Desmontado de unidad de red
echo ====================================

echo [FASE 4] Desmontando ...
net use %origen% /Delete
echo [FASE 4] %origen% desmontado.
echo.

:: Fase 5 - Copia de actualización a local
echo =======================================
echo [FASE 5] Copia de actualización a local
echo =======================================

echo [FASE 5] Copiando ...
robocopy "%rutaProyecto%\Principal" "%rutaEjecutable%"

if %errorlevel% GEQ 16 (
    echo Error fatal. Código: %errorlevel%
) else if %errorlevel% GEQ 8 (
    echo Fallos en algunos archivos. Código: %errorlevel%
) else if %errorlevel% EQU 3 (
    echo Archivos extra detectados en destino. Código: %errorlevel%
) else if %errorlevel% EQU 1 (
    echo Copia con cambios. Código: %errorlevel%
) else if %errorlevel% EQU 0 (
    echo Copia sin cambios. Código: %errorlevel%
) else (
    echo Resultado inesperado. Código: %errorlevel%
)

echo.

@echo on
```

Donde:

- `192.168.2.145` es la IP de la unidad `F:`.
- `\Clarion6\Proyectos\ElektronSQL` es la ruta de los archivos actualizados.
- `net use` es el comando para montar/desmontar unidades de red en CMD.

Cuando montes la unidad `F:`, te preguntará credenciales:

- Usuario: `Invitado`.
- Pass: [Sin password].

### Actualización del proyecto de Clarion

1. Deberás de copiar los archivos de la [FASE 3] a la carpeta compartida de tu máquina virtual en `(carpeta_compartida)\ElektronSQL\Principal`, ya que el script toma en cuenta que la carpeta de archivos del proyecto _no es la misma que la carpeta compartida_.
2. Después en tu máquina virtual copia los archivos, de tu local de la ruta conectada a la carpeta compartida, a `C:\Clarion6\Proyectos\ElektronSQL`, ya que el IDE de Clarion hace referencia a la carpeta indicada.

    Ejemplo:

    1. Identifica tu carpeta compartida en la configuración de tu máquina virtual:

        - LOCAL: `C:\APPS\ELSCA`.
        - Máquina Virtual: Unidad de red `W:`.

    2. Copia todos los archivos de la unidad `W:` a `C:\Clarion6\Proyectos\ElektronSQL\Principal`.

    Así cuando inicies un nuevo desarrollo tendrás la última versión de ElektronSQL. Si es la misma, favor de hacer caso omiso a esta indicación.

### Test de actualización

Para comprobar la actualización:

1. Ejecuta el archivo `elsca.exe` de tu carpeta local que contiene el ejecutable.
2. En tu máquina virtual, ejecuta el IDE de Clarion abriendo el proyecto _*.APP_ deseado.

🗓️ Fecha de última modificación: 2025-08-11
👤 Santos Vallecillo, Sergio Tostado
🏷️ Versión: 1
