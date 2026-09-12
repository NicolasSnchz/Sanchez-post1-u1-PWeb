# Análisis 2: Petición GET — API REST pública

## Información general

- URL: https://jsonplaceholder.typicode.com/posts/1
- Método: GET
- Código de estado: 200 OK
- Tipo de recurso en DevTools: fetch / xhr / document según cómo se abra
- Herramienta usada: panel Network de Chrome DevTools

## Headers de Request

| Header | Valor |
|--------|-------|
| Host | jsonplaceholder.typicode.com |
| User-Agent | `[PEGAR]` |
| Accept | `[PEGAR]` |

## Headers de Response

| Header | Valor | Significado |
|--------|-------|-------------|
| Content-Type | `[PEGAR: application/json; charset=utf-8]` | El cuerpo de la respuesta es JSON, no HTML. El navegador no lo renderiza como página sino como datos. |
| Cache-Control | `[PEGAR]` | Política de caché que aplica la API a este recurso. |
| Content-Length | `[PEGAR]` | Tamaño del cuerpo JSON en bytes. |
| Access-Control-Allow-Credentials | `[PEGAR, si aparece]` | Header de CORS. Indica cómo puede ser consumida la API desde otros dominios. |

## Cuerpo de la respuesta

```json
[PEGAR el JSON que devolvió la pestaña Response]
```

## Tiempos de carga

| Fase | Tiempo (ms) |
|------|------------|
| DNS Lookup | `[PEGAR]` |
| TTFB | `[PEGAR]` |
| Content Download | `[PEGAR]` |

## Petición fallida: recurso inexistente

- URL: https://jsonplaceholder.typicode.com/posts/999
- Método: GET
- Código de estado: `[PEGAR: debe ser 404 Not Found]`
- Cuerpo de la respuesta: `[PEGAR]`

El servidor responde con 404 porque la ruta existe pero el recurso identificado con ese id no está en la base de datos. Es importante notar que la petición sí llegó correctamente al servidor y este contestó de forma controlada. Un 404 no es un fallo de red ni un error del cliente al construir la petición, es una respuesta válida que informa que el recurso pedido no existe.

## Comparación: página HTML vs API REST

| Aspecto | GET a example.com | GET a la API REST |
|---------|-------------------|-------------------|
| Content-Type | text/html | application/json |
| Propósito de la respuesta | Documento para renderizar en pantalla | Datos para que los consuma otro programa |
| Recursos adicionales | Descarga CSS, JS e imágenes asociadas | Una sola respuesta, sin recursos dependientes |
| Uso típico | Navegación de un usuario | Consumo desde JavaScript u otra aplicación |
| Interpretación del cuerpo | El navegador construye el DOM | El código hace JSON.parse y usa los campos |

## Conclusión

Comparar las dos peticiones deja claro que HTTP es el mismo protocolo en ambos casos y que lo único que cambia realmente es el tipo de contenido que viaja en la respuesta. El header Content-Type es el que le indica al cliente cómo debe interpretar lo que recibió. También quedó claro que los códigos de estado son la forma en que el servidor comunica el resultado de la operación: el 200 confirma que el recurso se entregó y el 404 confirma que la ruta se procesó pero no hay nada que devolver. Entender esta diferencia es lo que permite después manejar errores correctamente al consumir una API desde JavaScript.

## Evidencia

![Petición GET 200 OK](../capturas/09-api-200.png)

![Petición GET 404 Not Found](../capturas/10-api-404.png)
