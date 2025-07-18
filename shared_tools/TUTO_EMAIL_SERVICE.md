# 📧 Documentación del Servicio de Correo Electrónico

## 🎯 ¿Qué hace?

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
| `isHtml` | boolean | query | Sí | Indica si el cuerpo es HTML (default: true) |
| `codigo` | integer | query | Sí | Código de configuración (default: 1) |

#### Ejemplo de solicitud:
```http
POST /api/email/send/?to=user@example.com&subject=Test&isHtml=true&codigo=1
Content-Type: application/json

"<h1>Hola</h1><p>Esto es un ejemplo.</p>"
```
---

### 2. Envío Avanzado de Correo
**Método:** `POST`  
**Endpoint:** `/api/email/send/advanced/`  
**Autenticación:** Requerida
#### Estructura del cuerpo (JSON):
```json
{
  "to": ["to@example.com", "to2@example.com"],
  "cc": ["cc1@example.com"],
  "bcc": ["bcc1@example.com"],
  "subject": "Correo Avanzado de Prueba",
  "body": "<h1>Importante</h1><p>Esto es un ejemplo.</p>",
  "isBodyHtml": true,
  "codigo": 2
}
```
**Campos requeridos:**
1. **to (array)** - Al menos un destinatario
2. **subject (string)** - Asunto del correo
3. **body (string)** - Contenido del mensaje
4. **codigo (int)** - Código de configuración a tomar

---

 ### 3. Envío con Archivos Adjuntos
**Método:** `POST`  
**Endpoint:** `/api/email/send/with-attachments`  
**Autenticación:** Requerida
#### Estructura del cuerpo (JSON):
```json
{
  "to": ["to@example.com"],
  "subject": "Correo con adjuntos",
  "body": "<p>Ejemplo de adjuntos.</p>",
  "isBodyHtml": true,
  "codigo": 3,
  "attachments": [
    {
      "content": "base64encodedstring...",
      "fileName": "documento.pdf",
      "contentType": "application/pdf"
    }
  ]
}
```
**Especificación de adjuntos:**
1. **content (string)** - Contenido del archivo en Base64
2. **fileName (string)** - 	Nombre del archivo con extensión
3. **contentType (string)** - Tipo MIME del archivo

---

## 🚦 Códigos de Respuesta
| Parámetro | Tipo | 
|-----------|------|
| `200 OK` | Correo enviado exitosamente | 
| `400 Bad Request` | Error en la solicitud o validación |
| `401 Unauthorized` | Falta autenticación o token inválido |
| `500 Internal Server Error` | Error en el servidor |

---

## 📌 Consideraciones Importantes
1. **Formatos soportados:** - Los adjuntos pueden ser cualquier tipo de archivo válido
2. **codigo (int)** - Código de configuración a tomar

---

> 🗓️ **Fecha de última modificación:** 2025-07-18
> 👤 **Guillermo Pérez**
> 🏷️ **Versión:** 1


