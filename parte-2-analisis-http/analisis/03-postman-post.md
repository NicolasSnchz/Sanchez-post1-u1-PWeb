# Análisis 3: Petición POST — Postman

## Configuración de la petición

- Herramienta: Postman
- Método: POST
- URL: https://jsonplaceholder.typicode.com/posts
- Header enviado: `Content-Type: application/json`
- Body: raw, formato JSON

```json
{
  "title": "Laboratorio Programacion Web",
  "body": "Analisis de peticiones HTTP con Postman.",
  "userId": 1
}
```

## Respuesta recibida

- Código de estado: `[PEGAR: debe ser 201 Created]`
- Tiempo de respuesta: `[PEGAR el valor que muestra Postman]`
- Tamaño de la respuesta: `[PEGAR]`

```json
[PEGAR el cuerpo de la respuesta, incluye el campo id asignado por el servidor]
```

El código 201 significa que el servidor creó un recurso nuevo como resultado de la petición. Se diferencia del 200 en que no solo confirma que todo salió bien, sino que además informa que algo se creó. La respuesta devuelve el mismo objeto que se envió más un campo id, que es el identificador que el servidor le asignó al recurso.

## Headers de respuesta relevantes

| Header | Valor | Significado |
|--------|-------|-------------|
| Content-Type | `[PEGAR]` | Formato del cuerpo devuelto por la API. |
| Location | `[PEGAR, si aparece]` | URL donde quedó disponible el recurso recién creado. |
| X-Powered-By | `[PEGAR, si aparece]` | Tecnología del servidor que atendió la petición. |

## Tests automatizados

Se agregaron dos tests en la pestaña Tests de Postman:

```javascript
pm.test("Status 201 Created", () => {
  pm.response.to.have.status(201);
});

pm.test("Respuesta incluye id asignado", () => {
  const json = pm.response.json();
  pm.expect(json).to.have.property("id");
  pm.expect(json.title).to.equal("Laboratorio Programacion Web");
});
```

Resultado: `[PEGAR: ambos tests en verde en Test Results]`

El primer test verifica el código de estado y el segundo revisa el contenido del cuerpo, comprobando que exista el campo id y que el título devuelto sea el mismo que se envió. Tener estos tests permite reejecutar la petición en cualquier momento y saber de inmediato si la API sigue comportándose igual, sin tener que revisar la respuesta a mano.

## Comparación: GET vs POST

| Aspecto | GET | POST |
|---------|-----|------|
| Propósito | Solicitar un recurso existente | Enviar datos para crear un recurso |
| Cuerpo en la petición | No lleva cuerpo | Lleva cuerpo con los datos a enviar |
| Parámetros | Van en la URL, visibles | Van en el cuerpo, no aparecen en la URL |
| Código de éxito esperado | 200 OK | 201 Created |
| Idempotencia | Sí, repetirlo no cambia nada en el servidor | No, cada envío intenta crear un recurso nuevo |
| Caché | El navegador puede cachearlo | No se cachea |

## Conclusión

Trabajar la misma API desde Postman y desde el navegador muestra que las herramientas cambian pero el protocolo no. Postman resulta más cómodo para probar métodos distintos de GET, porque permite armar el cuerpo, definir headers manualmente y guardar la petición para repetirla. La diferencia central entre GET y POST no es técnica solamente, es de intención: GET pide información y POST envía información para que el servidor haga algo con ella. Los tests automatizados agregan una capa útil, ya que convierten una prueba manual en una verificación repetible.

## Evidencia

![Respuesta 201 Created en Postman](../capturas/11-postman-201.png)

![Tests en verde en Test Results](../capturas/12-postman-tests.png)
