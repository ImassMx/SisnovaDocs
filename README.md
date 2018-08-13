![Imass](logo.png)

# API de Validación Imass

### Requisitos

Antes de empezar es necesario solicitar un token al área de sistemas.

### Validación Inicial
Antes de comenzar a realizar peticiones se recomienda realizar una prueba de conectividad con la API.

```shell
curl http://imasscotiza.com:3000/
```

```json
{
    "version": "v1.0.0",
    "system": "SISNOVA"
}
```

### Autenticación
Cada request hecho al API debera contener el header:

*Token de prueba 123ABC*

```
Authorization: Bearer [TOKEN_PROPORCIONADO]
```

### Crear un Token de Request

Crear un token para validar en requests:

Endpoint: `/token`

Payload vacío

Response:
```json
{
    "token": "c92008c09f5011e888c5618616a73235",
    "agent": "PostmanRuntime/7.2.0",
    "created_at": "8/13/2018, 11:29:55 PM",
    "used": false,
    "_id": "5b7214734a602a3b8c1352fb"
}
```

### Validar Token
Valida que un token sea válido, si el token llega a ser válido regresará un objeto Token, pero sólo una ves, si se vuelve a validar el resultado sera erróneo.

Endpoint: `/token/[Token.token]`

Ejemplo: `c92008c09f5011e888c5618616a73235`

Payload:
```json
{
    "product": "prueba"
}
```

El parámetro `product` es requerido de lo contrario se regresará una respuesta con status 400.

```json
{
    "error": "productRequired"
}
```

Se pueden agregar parámetros adicionales al request, y todo serán guardados como parte Token.body:

Request
```json
{
	"product": "prueba",
	"plan": "Plan de Prueba",
	"precio": 123.45,
	"contratacion": false
}
```

Response
```json
{
    "_id": "5b7216d90b684943ff350292",
    "token": "374abe209f5211e89364a5a0593ed40e",
    "agent": "PostmanRuntime/7.2.0",
    "created_at": "8/13/2018, 11:40:09 PM",
    "used": true,
    "body": {
        "product": "prueba",
        "plan": "Plan de Prueba",
        "precio": 123.45,
        "contratacion": false
    },
    "product": "prueba"
}
```

Si se vuelve a realiza el mismo request se obtendrá un status 400 indicando que el token ya ha sido validado:

```json
{
    "error": "usedToken"
}
```