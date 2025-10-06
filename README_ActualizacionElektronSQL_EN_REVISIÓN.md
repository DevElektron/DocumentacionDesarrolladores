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

:: ====================
:: Variables del script
:: ====================
set origen=F:
set ipOrigen=192.168.2.145
set rutaProyecto=C:\aaaa
set rutaEjecutable=C:\APPS\ELSCA - copia
set archivos=*.dct *.app *.lib *.dll *.exe
set fecha=%date:~6,4%%date:~3,2%%date:~0,2%
set hora=%time:~0,2%%time:~3,2%%time:~6,2%
set reglog=Recupera_%fecha%_%hora%.log

:: ================================
:: Fase 0 - Inicio
:: ================================
call :Log "======================================"
call :Log "  Actualización Local de ElektronSQL  "
call :Log "======================================"  
call :Log __BLANK__

:: ================================
:: Fase 1 - Preparación
:: ================================
call :Log "==============================="
call :Log "[FASE 1] Preparación de entorno"
call :Log "==============================="
call :Log "[FASE 1] Preparando ..."         
call :Log __BLANK__

call :Log "Origen: %origen%"
call :Log "Ruta del Proyecto: %rutaProyecto%"
call :Log "Ruta del ejecutable (elsca.exe): %rutaEjecutable%"
call :Log "Archivo de Log: %reglog%"
call :Log __BLANK__

:: =================================
:: Fase 2 - Montado de unidad de red
:: =================================
call :Log "================================="
call :Log "[FASE 2] Montado de unidad de red"
call :Log "================================="

call :Log "[FASE 2] Desmontando %origen% de %ipOrigen% (en caso de que haya quedado montada la unidad) ..."
net use %origen% /delete >nul 2>&1
call :Log "[FASE 2] Montando %origen% de %ipOrigen% ..."
net use %origen% \\%ipOrigen%\Clarion6\Proyectos\ElektronSQL /user:CORPELEKTRON\Invitado "" /persistent:no >> %reglog% 2>&1

if errorlevel 1 (
    call :Log "[ERROR] No se pudo montar la unidad %origen%. Abortando script."
    exit /b 1
)

call :Log "[FASE 2] %origen% montado exitosamente."
call :Log __BLANK__

:: =======================================
:: Fase 3 - Copia de archivos actualizados
:: =======================================
call :Log "======================================="
call :Log "[FASE 3] Copia de archivos actualizados"
call :Log "======================================="

call :Log "[FASE 3] Copiando actualización ..."
robocopy %origen%\ "%rutaProyecto%" %archivos% /s /log+:%reglog%
set rc=%errorlevel%

if %rc% GEQ 16 (
    call :Log "Error fatal. Código: %rc%"
    exit /b %rc%
) else if %rc% GEQ 8 (
    call :Log "Fallos en algunos archivos. Código: %rc%"
    exit /b %rc%
) else if %rc% EQU 3 (
    call :Log "Archivos extra detectados en destino + copia con cambios. Código: %rc%"
) else if %rc% EQU 2 (
    call :Log "Archivos extra detectados en destino. Código: %rc%"
) else if %rc% EQU 1 (
    call :Log "Copia con cambios. Código: %rc%"
) else if %rc% EQU 0 (
    call :Log "Copia sin cambios. Código: %rc%"
) else (
    call :Log "Resultado inesperado. Código: %rc%"
    exit /b %rc%
)

call :Log "[FASE 3] Actualización descargada de %ipOrigen%."
call :Log __BLANK__

:: ====================================
:: Fase 4 - Desmontado de unidad de red
:: ====================================
call :Log "===================================="
call :Log "[FASE 4] Desmontado de unidad de red"
call :Log "===================================="

call :Log "[FASE 4] Desmontando ..."
net use %origen% /Delete >> %reglog% 2>&1
if errorlevel 1 (
    call :Log "[WARNING] No se pudo desmontar la unidad %origen%. Continuando..."
) else (
    call :Log "[FASE 4] %origen% desmontado."
)
call :Log __BLANK__

:: =======================================
:: Fase 5 - Copia de actualización a local
:: =======================================
call :Log "======================================="
call :Log "[FASE 5] Copia de actualización a local"
call :Log "======================================="

rem Si ambas rutas son iguales, no es necesario hacer copia de los archivos 
rem descargados a la carpeta compartida a tu máquina virtual.
if "%rutaEjecutable%"=="%rutaProyecto%" (
    call :Log "[FASE 5] Omitida: rutaEjecutable es igual a rutaProyecto."
    call :Log __BLANK__
    goto :FinProceso
)

call :Log "[FASE 5] Copiando ..."
robocopy "%rutaProyecto%\Principal" "%rutaEjecutable%" /log+:%reglog%
set rc=%errorlevel%

if %rc% GEQ 16 (
    call :Log "Error fatal. Código: %rc%"
    exit /b %rc%
) else if %rc% GEQ 8 (
    call :Log "Fallos en algunos archivos. Código: %rc%"
    exit /b %rc%
) else if %rc% EQU 3 (
    call :Log "Archivos extra detectados en destino + copia con cambios. Código: %rc%"
) else if %rc% EQU 2 (
    call :Log "Archivos extra detectados en destino. Código: %rc%"
) else if %rc% EQU 1 (
    call :Log "Copia con cambios. Código: %rc%"
) else if %rc% EQU 0 (
    call :Log "Copia sin cambios. Código: %rc%"
) else (
    call :Log "Resultado inesperado. Código: %rc%"
    exit /b %rc%
)

call :Log "[FASE 5] Copia de actualización a local: OK."
call :Log __BLANK__

goto :FinProceso

:: Fin del proceso
:FinProceso
call :Log "Actualización exitosa, recuerda ahora copiar todos los archivos de '%rutaEjecutable%' que corresponde a tu carpeta compartida a tu MV a 'C:\Clarion6\Proyectos\ElektronSQL\Principal' dentro de tu MV."
exit /b

:: =======================================
:: Helper para escribir en pantalla y log
:: Uso: call :Log "mensaje"
:: =======================================
:Log
if "%~1"=="" (
    rem Caso: sin argumento → salto de línea
    echo.
    echo. >> %reglog%
    goto :eof
)

if /i "%~1"=="__BLANK__" (
    rem Caso: marcador explícito → salto de línea
    echo.
    echo. >> %reglog%
    goto :eof
)

rem Caso general: imprimir texto
echo %~1
echo %~1 >> %reglog%
goto :eof

@echo on
```

Donde:

- `192.168.2.145` es la IP de la unidad `F:`.
- `\Clarion6\Proyectos\ElektronSQL` es la ruta de los archivos actualizados.
- `net use` es el comando para montar/desmontar unidades de red en CMD.
- `CORPELEKTRON\Invitado ""` son la credenciales para montar la unidad de red.

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
