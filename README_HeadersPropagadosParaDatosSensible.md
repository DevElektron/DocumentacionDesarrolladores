# Headers Propagados para Datos Sensibles

## Objetivo

Con motivo de mejorar la seguridad del `AdministraWeb` de Elektron, cuyo proyecto y como lo dice su nombre es tecnología web, se implementó una estrategia de `Headers Propagados para Datos Sensibles`, mejorando la seguridad para el pase de parámetros iniciales de los módulos desde el Frontend hacia el Backend con la `Nueva Arquitectura de Microservicios (NAMS)`.

A continuación los detalles.

## Datos sensibles propagados a toda solicitud

Cuando se inicia sesión en el `AdministraWeb` con credenciales correctas, se obtiene un token con encriptación con datos sensible que se usan para los valores iniciales que los módulos necesitan para obtener la información a mostrar en cada uno de ellos.

El listado de datos y sus descripciones son:

| Dato      | Nombre Header | Tipo Dato (.NET) | Descripción                                                              |
|-----------|---------------|:----------------:|--------------------------------------------------------------------------|
| nven      | `x-nven`      | int              | Número de vendedor del usuario                                           |
| nalm      | `x-nalm`      | int              | Número de almacén asignado al usuario                                    |
| idUsuario | `x-idUsuario` | string           | Identificador del registro del usuario                                   |
| idRol     | `x-idRol`     | int              | Identificador del registro del rol asignado al usuario                   |
| netId     | `x-netId`     | string           | Identifica al usuario con fines de auditoría y trazabilidad de operación |
| nombre    | `x-nombre`    | string           | Nombre del usuario                                                       |
| aPaterno  | `x-aPaterno`  | string           | Apellido Paterno del usuario                                             |
| aMaterno  | `x-aMaterno`  | string           | Apellido Materno del usuario                                             |
| iniciales | `x-iniciales` | string           | Iniciales registradas del usuario                                        |
| estatus   | `x-estatus`   | int              | ?                                                                        |

## Uso de Headers propagados

1. En el endpoint del Backend que estes trabajando, usar el siguiente parámetro:

```csharp
[HttpX("endpoint")]
public async Task<IActionResult> miEndpointAsync([FromHeader(Name = "x-headerPropagado")] tipoDato nombreParam, MiRequest request)
```

Donde:

- **headerPropagado**: Es uno de los listados en la tabla de la [`sección anterior`](#datos-sensibles-propagados-a-toda-solicitud).
- **tipoDato**: Tipo de Datos requerido por el header en el código, ej. int para x-nven (ver [`Tipo Dato .NET`](#datos-sensibles-propagados-a-toda-solicitud)).
- **nombreParam**: Nombre de header para usar en el código.

Deberás de reemplazarlo con el valor del campo que viene en su objeto de request, ejemplo:

```csharp
[HttpPost("miEndpoint")]
public async Task<IActionResult> miEndpointAsync([FromHeader(Name = "x-nven")] tipoDato nven, MiRequest request) {
    request.NVen = nven;
    ...
}
```

Con esta forma de uso los headers propagados:

- Ya no vamos a tener que recorrer todos los `Request.Headers` de los endpoints de los Backends con un ciclo.
- No tienes que hacer la conversión al tipo de dato `.NET`, porque los `Request.Headers` tiene `string?` por defecto.
- `.NET` se encargará de mandar error cuando un dato en el header venga con un valor de tipo erróneo.

**Esta forma de uso no compete a los endpoint del `Gateway`, sino a los `Backs` conectados al mismo.**

## Detección de Necesidad

Durante el desarrollo de módulos de tableros (dashboard) para el proyecto, la primera fase de seguridad se basó con el `localStorage` del navegador, un almacenamiento plano que proveía los datos sensibles con una llave que se llamaba `auth-user` que contenía todos los pares `llave - valor` descritos en [`la lista de headers propagados`](#datos-sensibles-propagados-a-toda-solicitud).

### Prueba

1. Cambiamos el valor de `idRol` de un `Vendedor` al identificador de `Gerente` en el `localStorage` accediendo a las `Herramientas de Desarrollador` del navegador.
2. Recargamos la pestaña del navegador, antes prestando atención al menú lateral izquierdo para observar las opciones habilitadas.
3. **Resultado**: Las opciones habilitadas del menú lateral son correspondientes al rol `Gerente`.

### Conclusiones de Prueba

**_Esto supone un fallo de seguridad grave (security breach)_**, ya que cualquier usuario que tenga el conocimiento de acceder a las `Herramientas de Desarrollador` del navegador y se dé cuenta que los datos sensible de la sesión del usuario se encuentran en texto plano, podrá realizar pruebas para ver qué usuario (nven), almacén (nalm) o rol (idRol) le puede dar los permisos necesarios para realizar operaciones que no le fueron concedidas por el equipo de `Mesa de Ayuda`.

## Aprendizaje de Seguridad

1. En soluciones web **NUNCA BASAR LA SEGURIDAD** en los almacenamientos de los navegadores, ya que los usuarios mediante las `Herramientas del Desarrollador` pueden acceder y manipular dichos datos sensibles.
2. **La encriptación / cifrado de datos sensibles es un DEBER y NO una opción**.
3. Se debe de crear rutas de tráfico no coinvencionales para la transmisión de datos sensibles cuya intervención del usuario final sea **NULA** o en su defecto **vista por el usuario más NO _human-readable_ (entendible)**.
