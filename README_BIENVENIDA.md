
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
 src/
├── app/
│   ├── core/                     # Servicios y elementos globales
│   │   ├── auth/                 # Autenticación (guardias, login, tokens)
│   │   ├── interceptors/         # Interceptores HTTP (JWT, errores)
│   │   └── utils/                # Funciones y helpers reutilizables
│   │
│   ├── modules/                  # Módulos organizados por funcionalidad
│   │   └── traspasos/
│   │       ├── components/       # Componentes visuales del módulo
│   │       │   ├── traspaso-list/   # Lista paginada
│   │       │   └── traspaso-form/   # Formulario de edición
│   │       ├── pages/            # Vistas principales
│   │       │   ├── list/         # Página de listado
│   │       │   └── detail/       # Página de detalle
│   │       ├── services/         # Servicios del módulo
│   │       │   └── traspaso.service.ts
│   │       ├── models/           # Modelos de datos
│   │       │   ├── requests/     # Interfaces para requests
│   │       │   └── responses/    # Interfaces para responses
│   │       └── traspasos-routing.module.ts  # Rutas del módulo
│   │
│   ├── shared/                   # Componentes y utilidades compartidas
│   │   ├── ui/                   # Componentes de interfaz comunes
│   │   └── pipes/                # Pipes y directivas reutilizables
│   │
│   └── assets/                   # Estilos, íconos, imágenes, etc.
│
├── environments/                 # Configuraciones por entorno (dev, prod)
└── index.html                    # HTML principal

### ⚙️ Backend (.NET 8)
- Arquitectura:
  - Clean Architecture / Repositorio / Servicios
- Principales carpetas:
  Backend/
  ├── Properties/                  # Configuraciones de compilación del proyecto
  ├── Core/
  │   └── Data/                    # DbContext y configuración de base de datos
  ├── Modules/
  │   └── Modulo/
  │       ├── Application/         # Lógica de negocio
  │       │   ├── DTOs/
  │       │   │   ├── Requests/    # Objetos de entrada
  │       │   │   └── Responses/   # Objetos de salida
  │       │   ├── Interfaces/      # Contratos de los servicios
  │       │   └── Services/        # Implementaciones de la lógica de negocio
  │       ├── Controller/          # Controlador principal del módulo
  │       ├── Domain/              # Lógica de dominio
  │       │   ├── Entities/        # Entidades del dominio (ej. Folio, Eltp)
  │       │   └── Interfaces/      # Contratos de los repositorios
  │       └── Infrastructure/      # Infraestructura y acceso a datos
  │           └── Repositories/    # Repositorios implementados (EF Core, SQL)
  ├── Shared/                      # Utilidades y componentes reutilizables
  ├── appsettings.json             # Configuración global (conexiones, JWT, etc.)
  ├── Program.cs                   # Configuración de servicios, middleware, etc.

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
- Visual Studio 2022 para .NET

---

## 🔧 Instalación y Configuración

### Frontend
```bash
git clone `repositorio
npm install --force
ng serve
```

`/src/environments/environment.ts` contiene la configuración necesaria 

### Backend
```bash
git clone `repositorio
dotnet restore
dotnet run
```

- `appsettings.json` contiene la configuración necesaria

---

## 🔀 Git & Flujo de Trabajo

- Branch principal: `master`
- Branch para testing: `qa`
- Para cada nueva funcionalidad se crea una nueva rama a partir de qa: 
  -`feature/nombre` nuevo modulo
  -`release/nombre` modificación o añadir nuevas funciones a un modulo
  -`bugfix/nombre` corrección de error
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

Pasos manuales:
1. Commit y push de `tu rama`
2. Pull Request a `qa`
3. Pull Request a `master`

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
  - Modelos de entity `usan el nombre de la tabla en la bd`}
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
    matform-field
      apariencia `outline`
      class `extra-small`
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

---

## ✅ Checklist para Nuevos Integrantes

- [ ] Clonar el repositorio
- [ ] Instalar dependencias (frontend y backend)
- [ ] Configurar variables de entorno
- [ ] Ejecutar las apps localmente

---

¡Bienvenido! Estamos para ayudarte 🚀
