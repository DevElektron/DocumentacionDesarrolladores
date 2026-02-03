
# 🚀 Bienvenida de Desarrolladores – Proyecto Administra Web

## 📘 Descripción General

Bienvenido(a) al equipo 👋. Este documento te guiará para que puedas configurar tu entorno de desarrollo y entender cómo trabajamos con Angular 18 en el frontend y .NET Core 8 en el backend.

---

## 🧱 Estructura del Proyecto

### 🅰️ Frontend (Angular 18)
- Framework: Angular CLI v18.2.12
- Librerías clave:
  - Angular Material
  - AG Grid
- Organización:

 ```bash

📁 Estructura del Proyecto Angular
  src/
  ├── app/ # Código principal de la aplicación
  │ ├── core/ # 🌐 Elementos globales reutilizables
  │ │ ├── auth/ # 🔐 Autenticación (guards, login, tokens)
  │ │ ├── interceptors/ # 🔄 Interceptores HTTP (JWT, errores)
  │ │ └── utils/ # 🧰 Funciones y helpers comunes
  │ ├── modules/ # 📦 Módulos por funcionalidad
  │ │ ├── [Modulo]/ # Reemplazar con nombre real, ej. usuarios/
  │ │ │ ├── components/ # 🧩 Componentes visuales del módulo
  │ │ │ ├── pages/ # 📄 Vistas principales del módulo
  │ │ │ ├── services/ # ⚙️ Servicios del módulo
  │ │ │ └── models/ # 🧾 Modelos de datos
  │ │ │  ├── requests/ # 📥 Interfaces de entrada (request)
  │ │ │  └── responses/ # 📤 Interfaces de salida (response)
  │ │ ├── routing.module.ts # 🧭 Rutas del módulo
  │ │ └── archivo.md # 📘 Documentación del módulo (uso, excepciones)
  │ ├── shared/ # ♻️ Componentes, pipes y utilidades compartidas
  │ ├── ui/ # 🖼️ Elementos visuales comunes (botones, inputs)
  │ └── pipes/ # 🔣 Pipes y directivas reutilizables
  ├── assets/ # 🎨 Recursos estáticos: estilos, íconos, imágenes
  ├── environments/ # 🌍 Configuración por entorno (dev, prod)
  └── index.html # 🧱 HTML principal de la aplicación

```

### ⚙️ Backend (.NET 8)
- Arquitectura:
  - Mircoservicios
    - Controladores / Servicios / Repositorio
- Organización:

```bash
  📁 MSADMINISTRAWEB (MICROSERVICIOS)
  ├── BackArchivo/                # 📦 Microservicio Archivo
  ├── BackCatalogos/              # 📦 Microservicio Catálogos
  ├── BackConsultasAux/           # 📦 Microservicio Consultas Auxiliares
  ├── BackCxC/                    # 📦 Microservicio Cuentas por Cobrar
  ├── BackInventarios/            # 📦 Microservicio Inventarios
  ├── BackPromotores/             # 📦 Microservicio Promotores
  │   ├── bin/                    # ⚙️ Archivos de compilación
  │   ├── obj/                    # ⚙️ Archivos temporales
  │   ├── Helpers/                # 🔧 Utilidades propias del microservicio
  │   ├── Modulo/                 # 🧩 Núcleo funcional (bounded contexts)
  │   │   ├── ArticulosPromotores/# Submódulo funcional
  │   │   │   ├── Application/    # 🧠 Casos de uso
  │   │   │   ├── Controllers/    # 🎮 Endpoints HTTP
  │   │   │   ├── Domain/         # 🧬 Entidades y contratos
  │   │   │   └── Infrastructure/ # 🏗️ Repositorios
  │   │   │
  │   │   └── Promotores/         # Submódulo funcional
  │   │       ├── Application/    # 🧠 Lógica de aplicación
  │   │       │   ├── DTOs/
  │   │       │   │   ├── Requests/
  │   │       │   │   └── Responses/
  │   │       │   ├── Interfaces/ # Contratos de servicios
  │   │       │   └── Services/   # Implementación de casos de uso y reglas de negocio
  │   │       │
  │   │       ├── Controllers/    # 🎮 Controladores REST
  │   │       │   └── PromotoresController.cs
  │   │       │
  │   │       ├── Domain/         # 🧬 Dominio del negocio
  │   │       │   └── Interfaces/ # Contratos de repositorios
  │   │       │
  │   │       └── Infrastructure/ # 🏗️ Implementaciones técnicas
  │   │           ├── Repositories/
  │   │
  │   ├── Properties/             # ⚙️ Configuración del proyecto
  │   ├── .env                    # Variables de entorno
  │   ├── appsettings.json        # Configuración del microservicio
  │   ├── BackPromotores.csproj   # Proyecto .NET
  │   ├── BackPromotores.sln      # Solución del microservicio
  │   ├── Dockerfile              # 🐳 Imagen Docker
  │   ├── Dockerlocal.env         # Variables Docker locales
  │   ├── kubernetesqa.env        # Variables para Kubernetes
  │   ├── Program.cs              # 🚀 Bootstrap del microservicio
  │   └── Readme.md               # Documentación del MS
  │
  ├── BackUsuarios/               # 📦 Microservicio Usuarios
  ├── BackVentas/                 # 📦 Microservicio Ventas
  │
  ├── ConfigAPI/                  # ⚙️ Microservicio de configuración
  ├── EncryptionAPI/              # 🔒 Microservicio de cifrado
  ├── Gateway/                    # 🚪 API Gateway
  ├── ProxySecurity/              # 🛡️ Seguridad y autenticación
  │
  ├── docker-compose.yml          # 🐳 Orquestación local de microservicios
  ├── ElApis.sln                  # 🧩 Solución global
  └── README.md                   # 📘 Documentación general

```
---

## 🛠️ Requisitos Técnicos

| Herramienta      | Versión mínima / actual |
|------------------|-------------------------|
| Node.js          | 20.18.2                 |
| npm              | 10.8.2                  |
| Angular CLI      | 18.2.12                 |
| Angular          | 18.2.13                 |
| .NET SDK         | 8.0                     |


### 🔧 IDE recomendado
- VS Code (Frontend) con extensiones
  -  EsLint by Microsoft
  -  GitLens by GitKraken
  -  Git file history new by HenryTsz
      - Uso (En el archivo que se desea revisar):
        - Ctrl + Shift + P > Git File History
  -  Prittier by Prittier
  -  Ident-rainbow by Oderwat
      - Configuración:
        - Ctrl + Shift + P > Preferences: Open User Settings (JSON)
        - Añadir estas líneas de código, guardar cambios y recargar VSCode:
  
        ```json
        "indentRainbow.indicatorStyle": "light",
        "indentRainbow.colors": [
            "rgba(255,255,64,0.3)",
            "rgba(127,255,127,0.3)",
            "rgba(255,127,255,0.3)",
            "rgba(79,236,236,0.3)"
        ],
        ```
  -  Material Icon Theme by Philipp Kief
  -  Angular Language Service by Angular
  -  Error Lens by Alexander
  -  IntelliCode by Microsoft
  -  IntelliCode API Usage Examples by Microsoft
  -  SQL Server (mssql) by Microsoft
  -  VSCode-Pets (Opcional)
  -  VSCode-Pokemon (Opcional)
- Visual Studio 2022 para .NET

---

## 🔧 Instalación y Configuración

### Frontend
```bash
git clone repositorio
npm install --force
ng serve
```

`/src/environments/environment.ts` contiene la configuración necesaria 

### Backend
```bash
git clone repositorio
dotnet restore
dotnet run
```

- `appsettings.json` contiene la configuración necesaria

---

## 🔀 Git & Flujo de Trabajo

- Branch principal: `master`
- Branch para testing: `qa`
- Para cada nueva funcionalidad se crea una nueva rama a partir de qa: 
  - `feature/nombre` nuevo modulo
  - `release/nombre` modificación o añadir nuevas funciones a un modulo
  - `bugfix/nombre` corrección de error
- Commits: `Que sean cortos pero que se entienda lo que se realizo`:
- Pull Requests:
  - Validación por al menos 1 miembro
  - Pruebas ejecutadas antes de merge

- Repositorios privados: solo con invitación
  - FrontEnd: `https://github.com/DevElektron/AdministraWeb-Front.git`
  - BackEnd: `https://github.com/DevElektron/AdministraWeb-API.git`

---

## 🧪 Pruebas
- Se realizan primero en tu equipo
- Se suben a la rama qa
- El gerente realiza pruebas
- Se suben a la rama master


## 🚀 Despliegue

- Ambiente de pruebas: http://192.168.2.16:1089/
- Ambiente de producción: http://192.168.2.16:89/

## Pasos manuales de versionamiento

1. Posicionarte en tu rama `qa` del repositorio descargado (ya sea del `Back` o `Front`).
2. Traer cambios de rama `qa` con `git pull origin qa` para que tu `qa` local esté actualizada antes de iniciar tu desarrollo.
3. Crear tu rama (de acuerdo a las convenciones de este documento) a partir de tu rama actualizada `qa`, por ejemplo con el comando: `git checkout -b tu_rama`.
4. Trabajar en tu desarrollo con comandos `git add, commit y push` en `tu rama`. para los mensajes de tus commits, seguiremos el siguiente estándar:
5. Seguiremos el siguiente estándar para los mensajes de tus commits:
   - **[*] _Alias de proyecto_: _Mensaje_.**
   - Donde * puede ser:
      - [**MERGE**] = Bajar cambios de rama principal, correcciones por conflictos.
      - [**ADD**] = Agregar código.
      - [**IMP**] = Mejora de código / proceso.
      - [**UPD/REF**] = Modificación de código existente.
      - [**DEL**] = Borrado de código.
      - [**FIX**] = Reparación / Bug resuelto.
      - [**TEST**] = Código con fines de prueba.
      - [**DailyUpdate**] = Commit del fin del día.
7. Traer cambios de rama `qa` con `git pull origin qa` para que `tu_rama` local esté actualizada con cambios que algunos del equipo podría haber hecho mientras tú estabas trabajando en tu desarrollo, de ser necesario resolver conflictos.
8. Push de tu rama al repo de Github con `git push origin tu_rama`.
9. Verificar que `tu_rama` y tus cambios estén en el repositorio designado.
10. Crear un PR (pull request) con rama `base` = `qa` y rama `compare` = `tu rama`.
11. Esperar a revisión por parte del equipo.
12. Si la revisión salió correcta, se hará `MERGE` a `qa` con los cambios de `tu_rama`.

---

## 📐 Convenciones de Código

- Angular:
  - Se usa camelCase
  - Servicios terminan en `Service`
  - Pipes terminan en `Pipe`
  - Modules terminan en `Module`
  - Autocompleters terminan en `nombre-autocompleter`
  - Selects terminan en `nombre-select`
  - Componentes de actualización comienzan con `act-nombre`
  - Modelos Dto terminan en `Dto`
  - Modelos de request terminan en `Request`
  - Modelos de response terminan en `Response`
  - Modelos de entity `usan el nombre de la tabla en la bd`
  - Inyección de dependencias en constructor
- .NET:
  - Se usa camelCase
  - Interfaces comienzan con `I`
  - Servicios terminan en `Service`
  - Controllers terminan en `Controller`
  - Repositorios terminan en `Repository`
  - Modelos de entity `usan el nombre de la tabla en la bd`
  - Modelos de request terminan en `Request`
  - Modelos de response terminan en `Response`
  - Modelos de DTO terminan en `Dto`
  - Inyección de dependencias en constructor

---

## 🖌️ Convenciones de Diseño
  - Fuente `Quicksand, sans-serif`
  - Ag-grid `ag-theme-quartz`
  - Estilo tablas: `La configuración ya esta en styles.css y es global`
  - Debes basarte en el componente `TraspasosAlmacenComponent` para obtener los estilos necesarios para las ventanas de consulta.
  - Formularios realizados con componentes de angular material
    - matform-field
      - apariencia `outline`
      - class `extra-small`
  - Botones de angular material

## 👥 Equipo

| Rol         | Nombre                            | Contacto                   |
|-------------|-----------------------------------|----------------------------|
| Gerente     | Ernesto Aranda Estrella           | earanda@elektron.com.mx    |   
| FullStack   | Luis Guillermo Pérez Fuentes      | lperez@elektron.com.mx     |
| FullStack   | Francisco Javier Martínez Vázquez | frmartinez@elektron.com.mx |
| FullStack   | Ignacio Carranza Cornejo          | icarranza@elektron.com.mx  |
| FullStack   | Santos Israel Vallecillo Contreras| svallecillo@elektron.com.mx|
| FullStack   | Ernesto Martín Hernández Zuñiga   | emhernandez@elektron.com.mx|
| FullStack   | Rodrigo Rángel Martínez           | rrangel@elektron.com.mx    |
| FullStack   | José A. Eduardo Navarro Carranza  | janavarro@elektron.com.mx  |
| FullStack   | Samuel Valles Sánchez             | ssanchez@elektron.com.mx   |
| FullStack   | Erick Adrián López Rojas          | erojas@elektron.com.mx     |

---

## 📄 Documentación Extra

- Postman Collection: [`Próximamente`]
- Referencias útiles:
  - [Documentación Angular](https://angular.io/)
  - [Documentación Angular Material](https://v18.material.angular.dev/)
  - [Documentación AG-Grid](https://www.ag-grid.com/angular-data-grid/getting-started/)
  - [Documentación .NET](https://learn.microsoft.com/es-es/dotnet/)
  - [Documentación QuestPDF](https://www.questpdf.com/)

---

## ✅ Checklist para Nuevos Integrantes

- [ ] Clonar el repositorio
- [ ] Instalar dependencias (frontend y backend)
- [ ] Ejecutar las apps localmente

---

¡Bienvenido! Estamos para ayudarte 🚀
