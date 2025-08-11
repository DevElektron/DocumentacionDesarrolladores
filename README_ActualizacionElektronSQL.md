## Proyecto Migración ERP ElektronSQL
# Actualización de ElektronSQL

Para los desarrollos correspondientes a las tareas del proyecto de migración, se necesita actualizar los archivos con extensiones `*.dct *.app *.lib *.dll` y `*.exe` de la unidad de red conectada al equipo del Ing. Francisco.

## Prerequisitos

Una carpeta compartida a la máquina virtual de Windows XP que contenga Clarion 6 y el proyecto ElektronSQL:

- Carpeta de la máquina virtual `C:\Clarion6\Proyectos\ElektronSQL`.

Esta carpeta contiene todos los archivos del proyecto con el código base de Clarion, la podrás detectar entrando a la configuración de la máquina virtual, en tu local la carpeta deberá de contener la carpeta `Principal` del proyecto, ejemplo:

_Si tu carpeta compartida es `C:\APPS\ELSCA`, hay una carpeta `ElektronSQL`, donde se encontrará a su vez la carpeta `Principal`, entonces tu carpeta para la actualización es `C:\APPS\ELSCA\ElektronSQL`._

> NOTA: Si no tienes la máquina virtual solicitar apoyo al equipo.

## Script de actualización

Copiar este script en un archivo con extensión `*.bat`, sustituyendo `[Tu Carpeta Compartida (Prerequisitos)]` con la carpeta que encontramos en [`Prequisitos`](#prerequisitos), y enseguida ejecutarlo en un shell de Windows (`Powershell` o `CMD`):

```bat
@echo off

set origen= f:\
set destino=[Tu Carpeta Compartida (Prerequisitos)]
set archivos=*.dct *.app *.lib *.dll *.exe
set reglog=Recupera.log

net use F: \\192.168.2.145\Clarion6\Proyectos\ElektronSQL

robocopy %origen% %destino% %archivos% /s /log:%reglog%

net use F: /Delete

robocopy %destino%\Principal W:\ *.dll *.exe

@echo on
```

Donde:

- `192.168.2.145` es la IP de la unidad `f:`.
- `\Clarion6\Proyectos\ElektronSQL` es la ruta de los archivos actualizados.
- `net use` es el comando para montar/desmontar unidades de red en CMD.

Cuando montes la unidad `f:`, te preguntará credenciales:

- Usuario: `Invitado`.
- Pass: `Invitado`.

> NOTA: Si tiene otras carpetas en donde tienes tanto los archivos del proyecto (primer `robocopy`) como la ruta de la carpeta que contiene el ejecutable final (`robocopy` final), favor de modificar el script con las carpetas correspondientes en los comandos `robocopy`, ejemplo:

```bat
...
REM Primer robocopy.
robocopy %origen% C:\aaaa %archivos% /s /log:%reglog%
...
REM robocopy final.
robocopy C:\APPS\ELSCA W:\ *.dll *.exe
@echo on
```

## Test de actualización

Para comprobar la actualización, ejecuta el archivo `elsca.exe` de tu carpeta compartida.

🗓️ Fecha de última modificación: 2025-08-11 👤 Santos Vallecillo, Sergio Tostado 🏷️ Versión: 1
