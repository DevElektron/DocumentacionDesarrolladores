## Proyecto Migración ERP ElektronSQL
# Actualización de ElektronSQL

Para los desarrollos correspondientes a las tareas del proyecto de migración, se necesita actualizar los archivos con extensiones `*.dct *.app *.lib *.dll` y `*.exe` de la unidad de red conectada al equipo del Ing. Francisco.

### Prerequisitos

Una carpeta compartida a la máquina virtual de Windows XP que contenga Clarion 6 y el proyecto ElektronSQL:

- Carpeta de la máquina virtual `C:\Clarion6\Proyectos\ElektronSQL`.

Para la actualización, deberás encontrar 2 carpetas en tu local:

1. CARPETA CON ARCHIVOS DEL PROYECTO: Esta carpeta contiene todos los archivos del proyecto con el código base de Clarion, la podrás detectar entrando a la configuración de la máquina virtual, en tu local la carpeta deberá de contener la carpeta `Principal` del proyecto, ejemplo:

_Si tu carpeta compartida es `C:\aaaa`, hay una carpeta `Principal`, entonces tu carpeta para la actualización es `C:\aaaa`._

2. CARPETA DEL EJECUTABLE: Donde ejecutas en tu local `elsca.exe`, ejemplo:

_Si el acceso directo de tu `elsca.exe` apunta a `C:\APPS\ELSCA`, esta es tu carpeta del ejecutable._

> NOTA: Si no tienes la máquina virtual solicitar apoyo al equipo.

### Script de actualización

Copiar este script en un archivo con extensión `*.bat`, sustituyendo `rutaProyecto` y `rutaEjecutable` con los valores que encontramos en [`Prequisitos`](#prerequisitos), y enseguida ejecutarlo en un shell de Windows (`Powershell` o `CMD`):

```bat
@echo off

set origen=F:
set rutaProyecto=[CARPETA CON ARCHIVOS DEL PROYECTO]
set rutaEjecutable=[CARPETA DEL EJECUTABLE]
set archivos=*.dct *.app *.lib *.dll *.exe
set reglog=Recupera.log

net use %origen% \\192.168.2.145\Clarion6\Proyectos\ElektronSQL

robocopy %origen%\ %rutaProyecto% %archivos% /s /log:%reglog%

net use %origen% /Delete

robocopy %rutaProyecto%\Principal %rutaEjecutable%

@echo on
```

Donde:

- `192.168.2.145` es la IP de la unidad `f:`.
- `\Clarion6\Proyectos\ElektronSQL` es la ruta de los archivos actualizados.
- `net use` es el comando para montar/desmontar unidades de red en CMD.

Cuando montes la unidad `f:`, te preguntará credenciales:

- Usuario: `Invitado`.
- Pass: `Invitado`.

### Test de actualización

Para comprobar la actualización, ejecuta el archivo `elsca.exe` de tu carpeta compartida.

🗓️ Fecha de última modificación: 2025-08-11 👤 Santos Vallecillo, Sergio Tostado 🏷️ Versión: 1
