# Análisis 1: Petición GET — example.com

## Información general

- URL: https://example.com
- Método: GET
- Código de estado: 200 OK
- Tipo de recurso en DevTools: document
- Herramienta usada: panel Network de Chrome DevTools

## Headers de Request

| Header | Valor |
|--------|-------|
| Host | example.com |
| User-Agent | `[PEGAR: valor exacto de tu navegador]` |
| Accept | `[PEGAR: valor exacto]` |
| Accept-Language | `[PEGAR: valor exacto]` |
| Accept-Encoding | `[PEGAR: valor exacto]` |

El header Host indica a qué dominio va dirigida la petición, algo necesario cuando un mismo servidor aloja varios sitios. User-Agent identifica el navegador y el sistema operativo desde el que se hizo la petición. Accept le dice al servidor qué tipos de contenido puede procesar el cliente, y Accept-Encoding qué métodos de compresión soporta.

## Headers de Response

| Header | Valor | Significado |
|--------|-------|-------------|
| Content-Type | `[PEGAR]` | Indica que el cuerpo de la respuesta es un documento HTML y con qué codificación de caracteres debe interpretarse. |
| Content-Length | `[PEGAR]` | Tamaño en bytes del cuerpo de la respuesta, para que el navegador sepa cuánto debe recibir. |
| Date | `[PEGAR]` | Fecha y hora en que el servidor generó la respuesta. |
| Cache-Control | `[PEGAR]` | Define por cuánto tiempo el navegador puede guardar el recurso en caché antes de volver a pedirlo. |
| ETag | `[PEGAR, si aparece]` | Identificador de la versión del recurso. Permite al navegador preguntar si el archivo cambió sin descargarlo de nuevo. |

## Tiempos de carga

| Fase | Tiempo (ms) |
|------|------------|
| DNS Lookup | `[PEGAR de la pestaña Timing]` |
| Initial connection | `[PEGAR]` |
| SSL | `[PEGAR]` |
| Time to First Byte (TTFB) | `[PEGAR]` |
| Content Download | `[PEGAR]` |

## Conclusión

La petición a example.com muestra el ciclo completo de una carga de página estática. El navegador primero resuelve el nombre de dominio a una dirección IP, luego abre la conexión TCP y negocia el certificado TLS antes de poder enviar la petición. El TTFB refleja el tiempo que el servidor tardó en empezar a responder y suele ser la fase más larga cuando el servidor está lejos geográficamente. La descarga del contenido fue muy rápida porque el documento pesa apenas unos cientos de bytes. Los headers de respuesta confirman que se trata de HTML y que el recurso puede quedar en caché, lo que evitaría descargarlo otra vez en visitas siguientes.

## Evidencia

![Panel Network con la petición seleccionada](../capturas/08-network-example.png)
