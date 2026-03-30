# 📧 Documentación del Servicio de Envió de SMS.

## 🎯 ¿Qué hace?

El servicio de mensajeria de sms es un endpoint RESTful para el envío de mensajes a celulares.

**Envío** - Solo podrá enviarse mensaje a un solo numero, establecido con el endpoint del proveedor de servicios.
---

### Envío de SMS
**Método:** `POST`  
**Ruta Dinamica:** `/api/ventas/consultadecotizaciones`  
**Endpoint :** `send-sms`  
**Autenticación:** Requerida

#### Parámetros:
| Parámetro | Tipo | Ubicación | Requerido | Descripción |
|-----------|------|-----------|-----------|-------------|
| `Numero`  | integer | query | Sí | Numero de Celular a enviar mensaje |
| `Mensaje` | string | query | Sí | Mensaje a enviar al Numero de Celular |

#### Ejemplo de solicitud:
```http
POST /api/ventas/consultadecotizaciones/send-sms
Content-Type: application/json
```
---
```json
{
  "Numero": "4772320521",
  "Mensaje": "Hola es una prueba de sms"
}
```
---

#### Ejemplo de respuesta correcta
**sucess:** `Recibio una respuesta de conexion al endpoint`  
**data.enviarsms.enviado:** `true=mensaje enviado, false=error al enviar`  
**data.enviarsms.mensajeProveedor:** `mensaje del error o exito por parte del proveedor de servicios del sms`  
```
{
    "success": true,
    "message": "Operación exitosa",
    "killProcess": null,
    "data": {
        "enviarsms": {
            "enviado": true,
            "mensajeProveedor": "Mensaje Enviado"
        },
        "totalRecords": 1
    }
}
```
---

## Llamada en tu Controllers
```
[HttpPost("send-sms")]
public async Task<IActionResult> EnviarSms([FromBody] EnviarSmsRequest request)
{
    try
    {
        var response = await _smsService.EnviarSmsAsync(request); //inyecta del servicio en el endpoint creado
        return Ok(response);
    }
    catch (Exception ex)
    {
        return BadRequest(new { message = ex.Message });
    }
}
```

## 🚦 Códigos de Respuesta
| Parámetro | Tipo | 
|-----------|------|
| `200 OK`  | sms enviado exitosamente | 
| `400 Bad Request` | Error en la solicitud o validación |
| `401 Unauthorized` | Credenciales Invalidas |
| `500 Internal Server Error` | Error en el servidor |
---

> 🗓️ **Fecha de última modificación:** 2026-03-30
> 👤 **Ernesto Martin Hernandez Zuñiga**
> 🏷️ **Versión:** 1 



