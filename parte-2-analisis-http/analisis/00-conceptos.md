# Marco conceptual: HTTP y la comunicación cliente servidor

Documento de apoyo para los análisis de esta unidad. Reúne los conceptos que se aplicaron al inspeccionar el tráfico real con Chrome DevTools y Postman.

## Qué es HTTP

HTTP (HyperText Transfer Protocol) es el protocolo que usan navegadores y servidores para intercambiar información en la Web. Funciona sobre TCP y sigue un modelo de petición y respuesta: el cliente pide algo, el servidor contesta, y la conexión no guarda memoria de lo que pasó antes. Esa falta de memoria es lo que se llama ser un protocolo sin estado, y es la razón por la que existen mecanismos como cookies o tokens para reconocer a un usuario entre una petición y otra.

HTTPS es el mismo protocolo pero con una capa de cifrado TLS por debajo. Eso agrega una fase extra al inicio de la conexión, la negociación del certificado, que aparece en el panel Timing de DevTools como SSL.

## Estructura de una petición

Toda petición HTTP tiene tres partes:

**Línea de inicio.** Contiene el método, la ruta del recurso y la versión del protocolo. Por ejemplo `GET /posts/1 HTTP/1.1`.

**Headers.** Pares de clave y valor que describen la petición. Indican a qué host va dirigida, qué cliente la envía, qué formatos acepta como respuesta y qué compresión soporta.

**Cuerpo.** Opcional. Los métodos que solo consultan no lo llevan. Los que envían información sí, y ahí viaja el JSON, el formulario o el archivo.

La respuesta tiene la misma forma, solo que su línea de inicio lleva el código de estado en lugar del método.

## Métodos principales

| Método | Propósito | Lleva cuerpo | Idempotente |
|--------|-----------|--------------|-------------|
| GET | Solicitar un recurso existente | No | Sí |
| POST | Enviar datos para crear un recurso | Sí | No |
| PUT | Reemplazar por completo un recurso | Sí | Sí |
| PATCH | Modificar parcialmente un recurso | Sí | No necesariamente |
| DELETE | Eliminar un recurso | No | Sí |

Idempotente quiere decir que repetir la misma petición varias veces deja el servidor en el mismo estado que si se hubiera hecho una sola vez. GET es idempotente porque consultar diez veces no cambia nada. POST no lo es porque cada envío intenta crear un recurso nuevo, y por eso los navegadores advierten antes de reenviar un formulario.

## Códigos de estado

Los códigos se agrupan por su primer dígito y ese dígito ya dice de qué se trata el resultado.

**1xx informativos.** La petición se recibió y el proceso continúa. Rara vez se ven en el trabajo diario.

**2xx éxito.** La petición se procesó correctamente. El 200 OK confirma que el recurso se entregó. El 201 Created informa además que se creó algo nuevo. El 204 No Content indica que todo salió bien pero no hay cuerpo que devolver, típico de un DELETE.

**3xx redirección.** El recurso está en otra parte. El 301 señala un traslado permanente y el 302 uno temporal. El navegador normalmente sigue la redirección sin que el usuario lo note.

**4xx error del cliente.** La petición tiene algún problema. El 400 indica que está mal formada, el 401 que falta autenticación, el 403 que hay autenticación pero sin permisos, y el 404 que la ruta no corresponde a ningún recurso. Es importante notar que un 4xx no es una falla del servidor: el servidor funcionó bien y respondió de forma controlada.

**5xx error del servidor.** Algo falló del lado del servidor. El 500 es un error interno genérico, el 502 aparece cuando un servidor intermedio recibió una respuesta inválida, y el 503 cuando el servicio no está disponible.

## Headers más relevantes

De petición:

- **Host.** Dominio al que va dirigida. Necesario porque un mismo servidor puede alojar varios sitios en la misma dirección IP.
- **User-Agent.** Identifica el navegador, su versión y el sistema operativo.
- **Accept.** Formatos que el cliente puede procesar.
- **Accept-Encoding.** Métodos de compresión soportados, como gzip o brotli.
- **Authorization.** Credenciales o token cuando el recurso está protegido.

De respuesta:

- **Content-Type.** Formato del cuerpo devuelto. Es el header que determina si el cliente debe renderizar una página o parsear datos.
- **Content-Length.** Tamaño del cuerpo en bytes.
- **Cache-Control.** Cuánto tiempo puede el cliente reutilizar el recurso sin volver a pedirlo.
- **ETag.** Identificador de versión del recurso. Permite preguntar si cambió sin descargarlo completo.
- **Set-Cookie.** Guarda información en el cliente para reconocerlo en peticiones siguientes.

## Fases de una petición en el panel Timing

Cuando se inspecciona una petición en DevTools, el tiempo total se descompone así:

**DNS Lookup.** Traducción del nombre de dominio a una dirección IP.

**Initial connection.** Apertura de la conexión TCP mediante el intercambio de tres pasos.

**SSL.** Negociación del certificado y establecimiento del canal cifrado. Solo aparece en HTTPS.

**Time to First Byte.** Tiempo desde que se envió la petición hasta que llegó el primer byte de la respuesta. Refleja lo que tardó el servidor en procesar y empezar a contestar, más la latencia de ida y vuelta.

**Content Download.** Descarga del cuerpo completo. Depende del tamaño del recurso y del ancho de banda.

## Diferencia entre una página y una API

Una página HTML y un endpoint de API viajan por el mismo protocolo, con los mismos métodos y los mismos códigos de estado. Lo que cambia es el tipo de contenido y quién lo consume. La página devuelve `text/html` y está pensada para que el navegador construya el DOM y lo muestre en pantalla, y normalmente arrastra descargas adicionales de CSS, JavaScript e imágenes. La API devuelve `application/json` y está pensada para que otro programa lea los campos y decida qué hacer con ellos, sin recursos dependientes.

Entender esto es lo que permite después consumir una API desde JavaScript con fetch, porque se sabe qué esperar en la respuesta y cómo reaccionar según el código de estado que llegue.
