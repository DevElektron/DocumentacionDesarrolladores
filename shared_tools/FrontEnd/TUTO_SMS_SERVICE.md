# 📧 Documentación del Consumo del Servicio de Envió de SMS de Angular.

## 🎯 ¿Qué hace?

Este Servucui consume el endpoint para el envío de mensajes sms a un numero celular.

**Envío** - Solo podrá enviarse mensaje a un solo numero, establecido con el endpoint del proveedor de servicios.
---

### Consumo del Servicio en Angular en TypeScript
```
const request: EnviarSmsRequest = { Numero: '4775891245', Mensaje: 'Hola es una prueba de envio SMS' };
const resp = await lastValueFrom(this.EnviarSmsService.EnviarSms(request));
if (!resp.success || !resp.data.enviarsms.enviado) {
   await lastValueFrom(this.handleWarning(`Error al enviar Mensaje SMS || ${resp.data.enviarsms.mensajeProveedor}`));
} else {
  await lastValueFrom(this.handleWarning(`Mensaje SMS enviado con éxito`));    
}
```

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



