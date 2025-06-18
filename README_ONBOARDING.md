
# 🚀 Onboarding de Desarrolladores – Proyecto Administra Web

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
  - `/app/components` – Componentes visuales
  - `/app/services` – Lógica de comunicación con backend
  - `/app/models` – Interfaces y DTOs
  - `/app/shared` – Módulos y utilidades compartidas
- Comunicación API: HttpClient + Interceptores
- Rutas

### ⚙️ Backend (.NET 8)
- Arquitectura:
  - Clean Architecture / Repositorio / Servicios
- Principales carpetas:
  - `/Controllers` – Endpoints REST
  - `/Services` – Lógica de negocio
  - `/Repositories` – Acceso a datos
  - `/DTOs` – Modelos de entrada/salida
- ORM: Entity Framework Core
- Autenticación: JWT

---

## 🛠️ Requisitos Técnicos

| Herramienta | Versión mínima |
|-------------|----------------|
| Node.js     | 18.x           |
| Angular CLI | 18.x           |
| .NET SDK    | 8.0            |
| PostgreSQL  | 14.x           |
| Docker      | Opcional       |

### 🔧 IDE recomendado
- VS Code (Frontend) con extensiones:
  - Angular Language Service
  - ESLint
  - Prettier
- Visual Studio 2022 o VS Code para .NET

---

## 🔧 Instalación y Configuración

### Frontend
```bash
npm install
ng serve
```

- Revisa `/src/environments/environment.ts` para definir la URL base de las APIs.

### Backend
```bash
dotnet restore
dotnet run
```

- Configura `appsettings.Development.json` con:
  ```json
  {
    "ConnectionStrings": {
      "DefaultConnection": "Server=localhost;Database=mi_db;User Id=sa;Password=...;"
    },
    "JwtSettings": {
      "Secret": "clave_supersecreta",
      "Issuer": "app",
      "Audience": "usuarios"
    }
  }
  ```

---

## 🔀 Git & Flujo de Trabajo

- Branch principal: `master`
- Branch de integración: `qa`
- Branches de funcionalidades: `feature/nombre`
- Commits: usamos:
- Pull Requests:
  - Validación por al menos 1 miembro
  - Pruebas ejecutadas antes de merge

---

## 🧪 Pruebas


## 🚀 Despliegue

- Ambiente de staging: [URL del entorno de pruebas]
- Ambiente de producción: [URL del entorno productivo]
- CI/CD: GitHub Actions

Pasos manuales:
1. Commit en `develop`
2. Pull Request a `main`

---

## 📐 Convenciones de Código

- Angular:
  - Components en camelCase
  - Servicios terminan en `Service`
- .NET:
  - Interfaces con `I`
  - Inyección de dependencias en constructor

---

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
| FullStack   | Erick Adrián López Rojas          | @elektron.com.mx           |

---

## 📄 Documentación Extra

- Swagger: `/swagger/index.html`
- Postman Collection: [link]
- Referencias útiles:
  - [Documentación Angular](https://angular.io/)
  - [Documentación .NET](https://learn.microsoft.com/es-es/dotnet/)

---

## ✅ Checklist para Nuevos Integrantes

- [ ] Clonar el repositorio
- [ ] Instalar dependencias (frontend y backend)
- [ ] Configurar variables de entorno
- [ ] Ejecutar las apps localmente

---

¡Bienvenido! Estamos para ayudarte 🚀
