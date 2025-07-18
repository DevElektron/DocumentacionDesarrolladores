# 📧 Documentación del Servicio de Correo Electrónico

## 🎯 Descripción General

El servicio de correo electrónico proporciona tres endpoints RESTful para el envío de mensajes con diferentes niveles de complejidad:

1. **Envío básico** - Para correos simples con un solo destinatario
2. **Envío avanzado** - Para correos con múltiples destinatarios (CC y BCC)
3. **Envío con adjuntos** - Para correos con archivos adjuntos

---

## 🔌 Endpoints Disponibles

### 1. Envío Básico de Correo
**Método:** `POST`  
**Endpoint:** `/api/email/send/`  
**Autenticación:** Requerida

#### Parámetros:
| Parámetro | Tipo | Ubicación | Requerido | Descripción |
|-----------|------|-----------|-----------|-------------|
| `to` | string | query | Sí | Dirección de correo del destinatario |
| `subject` | string | query | Sí | Asunto del correo |
| `body` | string | body | Sí | Contenido del mensaje (puede ser HTML) |
| `isHtml` | boolean | query | No | Indica si el cuerpo es HTML (default: true) |
| `codigo` | integer | query | No | Código de configuración (default: 1) |

#### Ejemplo de solicitud:
```http
POST /api/email/send/?to=user@example.com&subject=Test&isHtml=true&codigo=1
Content-Type: application/json

"<h1>Hello</h1><p>This is a test email.</p>"
