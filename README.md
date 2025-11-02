# **Trabajo de investigación: Protocolo HTTP y su relación con las APIs web**

* Nombre: Javier Valenzuela
* Cohorte: 22
* Fecha de entrega: 02/11/25

## **Introducción**

El protocolo HTTP (HyperText Transfer Protocol) constituye uno de los pilares fundamentales del funcionamiento de la web moderna. A través de él, los navegadores, servidores y APIs intercambian información estructurada mediante solicitudes y respuestas estandarizadas. Comprender su funcionamiento es esencial para todo desarrollador o estudiante de programación, ya que permite interpretar cómo fluyen los datos en una aplicación web y cómo se garantiza una comunicación eficiente entre cliente y servidor.

Este trabajo de investigación tiene como objetivo analizar los fundamentos del protocolo HTTP, su evolución hacia HTTPS mediante el uso de SSL/TLS, los puertos más utilizados en la comunicación, los principales códigos de estado y los métodos que definen las operaciones dentro de las APIs RESTful. A través de ejemplos prácticos y reflexiones personales, se busca consolidar una comprensión clara y aplicada de este protocolo esencial para el desarrollo web actual.

# **Diferencias entre el modelo HTTP y HTTPS**

### ¿Qué es el modelo HTTP?

HTTP (Hipertext Transfer Protocol), es un protocolo de internet que opera en la capa de aplicación del modelo TCP/IP. Su función es realizar solicitudes cliente-servidor que puedan ser interpretadas por un navegador. 

Por ejemplo: Cuando hacemos click en un video de youtube, nuestro navegador envía una solicitud HTTP hacia los servidores de youtube. Estos responden enviando un documento HTML con el contenido solicitado. 

### ¿Qué es el modelo HTTPS? 

El modelo HTTPS es un conjunto de dos protocolos: HTTP + SSL/TLS. 

### Diferencias entre ambos

En el protocolo HTTP, los datos viajan como texto plano, por lo que alguien podría verlos si logra interceptar la red. Mientras que en el modelo HTTPS, los datos viajan encriptados gracias al protocolo SSL/TSL. 

Hoy en día, SSL está obsoleto porque se encontraron vulnerabilidaddes. TLS es el protocolo moderno utilizado por HTTP para realizar solicitudes seguras. 

Para entender mejor la diferencia entre ambos, pensemos en el siguiente escenario: 

<blockquote>

<p>Estás realizando una transacción mediante la aplicación de tu banco.
En esta aplicación existen dos partes importantes: el frontend, que es la interfaz que el usuario puede ver e interactuar, y el backend, que contiene la lógica que procesa los datos en el servidor.

Ambas partes se comunican a través de una API, que es un componente del backend encargado de recibir solicitudes HTTP y responder con datos estructurados, generalmente en formato JSON.

Un documento JSON podría verse así:</p>

<pre><code>{
  "nombre": "Daniel",
  "cuenta": "12.234.122",
  "saldo": 200000
}
</code></pre>

<p>Si la conexión se estableciera solo mediante HTTP, este documento viajaría en texto plano y podría ser interceptado por un tercero.
En cambio, con HTTPS, el mismo documento viaja cifrado mediante TLS, de modo que, aunque alguien intercepte la comunicación, su contenido no será legible.
</p>

</blockquote>

## **Puertos de comunicación**

### ¿Qué es un puerto en redes?

Un puerto es un número que identifica un canal lógico de comunicación dentro de un dispositivo conectado a la red.
Así como una casa tiene una sola dirección (la IP), pero muchas puertas (los puertos), un mismo computador o servidor puede manejar múltiples servicios al mismo tiempo, cada uno con su puerto específico.

Por ejemplo:

* El puerto 80 se usa para páginas web.
* El puerto 25 para correos electrónicos.
* El puerto 3306 para bases de datos MySQL.

De esta forma, los puertos permiten que un mismo equipo atienda distintos tipos de tráfico simultáneamente, sin confundirlos.

### Importancia de los puertos para HTTP

El protocolo HTTP necesita un puerto para enviar y recibir solicitudes entre cliente y servidor.
Sin un puerto definido, el navegador no sabría a qué servicio debe conectarse dentro del servidor.

Por defecto:

* HTTP usa el puerto 80
* HTTPS usa el puerto 443 (el mismo servicio, pero cifrado con TLS)

Cuando escribes en tu navegador https://example.com, en realidad se está conectando a:

```
IP: 142.250.72.14
Puerto: 443
```
### Propósito de los puertos 80 y 8080

|Puerto|	Protocolo|	Descripción|
|-------|--------------|-------------|
|80 |	HTTP |	Puerto predeterminado para tráfico web sin cifrar. Cuando un navegador hace una solicitud HTTP, si no se especifica otro puerto, usa el 80.|
|8080	| HTTP alternativo	| Se usa comúnmente como puerto alternativo o de desarrollo. Muchas aplicaciones web locales o entornos de prueba (como servidores Apache, Tomcat o Node.js) escuchan en 8080 para evitar conflictos con el 80.|

Ejemplo:

* Sitio en producción: http://www.ejemplo.com (usa el puerto 80).

* Servidor local en desarrollo: http://localhost:8080 (usa el 8080).

### Otros puertos comunes y su función

|Puerto|	Protocolo / Servicio|	Función principal|
|-------|------------------------|---------------------|
|21|	FTP|	Transferencia de archivos entre cliente y servidor.
|22	|SSH	|Acceso remoto seguro a otro equipo mediante línea de comandos.
|25	|SMTP	|Envío de correos electrónicos entre servidores.
|53|	DNS|	Traduce nombres de dominio a direcciones IP.
|80|	HTTP|	Comunicación web sin cifrado.
|443|	HTTPS|	Comunicación web cifrada mediante TLS.
|3306|	MySQL|	Conexión entre aplicaciones y bases de datos MySQL.
|8080	|HTTP alternativo|	Puerto de desarrollo o pruebas web.

## **Códigos de estado de respuesta HTTP**

### ¿Qué son los status code? 

Los status codes son números que el servidor envía al cliente (navegador, app, API, etc.) para indicar el resultado de una solicitud HTTP.

En otras palabras, cada vez que haces una petición al servidor (por ejemplo, GET /pagina), el servidor responde con:

* El código de estado, y
* (opcionalmente) datos o un mensaje.

Sirven para que el cliente sepa si la solicitud se completó correctamente, falló o necesita alguna acción adicional.

| **Categoría**                | **Rango** | **Descripción general**                                 | **Ejemplo de código**     |
| ---------------------------- | --------- | ------------------------------------------------------- | ------------------------- |
| **1xx – Informativos**       | 100–199   | El servidor recibió la solicitud y continúa el proceso. | 100 Continue              |
| **2xx – Éxito**              | 200–299   | La solicitud fue procesada correctamente.               | 200 OK                    |
| **3xx – Redirección**        | 300–399   | La solicitud fue redirigida a otro recurso.             | 301 Moved Permanently     |
| **4xx – Error del Cliente**  | 400–499   | Error causado por la solicitud del cliente.             | 404 Not Found             |
| **5xx – Error del Servidor** | 500–599   | El servidor tuvo un problema al procesar la solicitud.  | 500 Internal Server Error |

### Importancia de los códigos 200, 404 y 500

**200 OK – Éxito total**

Este código indica que la solicitud fue recibida, entendida y procesada correctamente.
En una API, significa que la comunicación entre el cliente y el servidor funciona como se espera, y que el backend devolvió una respuesta válida (por ejemplo, un JSON con los datos solicitados).

**Por qué es importante reconocerlo:**

* Permite confirmar que no hay errores de red ni de lógica.

* En pruebas de APIs o sitios web, un 200 OK es señal de que el endpoint o la ruta está funcionando.

* En desarrollo, sirve para verificar que el frontend recibe los datos correctamente.

Ejemplo:

Una solicitud GET /api/usuarios devuelve:
```
HTTP/1.1 200 OK
Content-Type: application/json

[{ "id": 1, "nombre": "Daniel" }]
```

**404 Not Found – Recurso no encontrado**

Este código aparece cuando el cliente solicita una ruta o archivo que no existe en el servidor.
Puede deberse a una URL mal escrita, a un enlace roto, o a que el recurso fue eliminado o movido.

**Por qué es importante reconocerlo:**

* Indica un error en la ruta, nombre del archivo o endpoint.

* Ayuda a saber que la conexión con el servidor existe, pero el problema está en la ubicación del recurso.

* En APIs, ayuda a diferenciar si el problema es del servidor (500) o del cliente (404).

Ejemplo:

El usuario intenta acceder a /api/usuarios/999 (un ID que no existe):
```
HTTP/1.1 404 Not Found
Content-Type: application/json

{"error": "Usuario no encontrado"}
```

**500 Internal Server Error – Fallo en el servidor**

Este código significa que el servidor tuvo un error interno al intentar procesar la solicitud.
El problema no está en la conexión ni en la ruta, sino dentro del backend: un fallo en el código, en la base de datos o en la configuración del servidor.

**Por qué es importante reconocerlo:**

* Indica que el cliente no tiene la culpa, pero el servidor sí necesita ser revisado.

* Permite al desarrollador saber que debe depurar la lógica del backend, revisar logs o probar el código paso a paso.

* En una API, puede deberse a consultas SQL mal escritas, errores de conexión o excepciones no controladas.

Ejemplo:
```
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{"error": "Error en la conexión con la base de datos"}
```
**Cómo usar estos códigos para diagnosticar problemas:**

| Código                        | Qué indica                  | Qué revisar                                                 | Acción recomendada                                     |
| ----------------------------- | --------------------------- | ----------------------------------------------------------- | ------------------------------------------------------ |
| **200 OK**                    | Todo funciona correctamente | Verifica que los datos enviados sean correctos              | Confirmar éxito o continuar pruebas                    |
| **404 Not Found**             | La ruta o recurso no existe | Verifica la URL, los nombres de archivo o los endpoints     | Corregir rutas o nombres mal escritos                  |
| **500 Internal Server Error** | Error dentro del servidor   | Revisa logs, conexión a la BD, excepciones o código backend | Depurar el código o revisar configuración del servidor |

## **Métodos HTTP**
### ¿Qué es una API REST? 

Una API RESTful es un backend que responde a solicitudes HTTP usando métodos estándar (GET, POST, PUT, DELETE) y devuelve datos en formato JSON.

### ¿Qué significa REST?

REST no es un lenguaje ni una tecnología, sino un conjunto de reglas o principios para construir APIs que sean simples, ordenadas y predecibles.
Estos son sus fundamentos principales:

| Principio REST                      | Qué significa                                                                          |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| **Cliente-servidor**                | El frontend (cliente) y el backend (servidor) están separados y se comunican por HTTP. |
| **Sin estado (Stateless)**          | Cada solicitud es independiente: el servidor no recuerda lo que hiciste antes.         |
| **Uso de métodos HTTP**             | GET, POST, PUT, DELETE representan acciones concretas sobre los datos.                 |
| **Recursos identificados por URLs** | Cada recurso (usuario, producto, etc.) tiene su propia dirección o “endpoint”.         |
| **Respuestas en formato estándar**  | Los datos se devuelven usualmente en formato JSON.                                     |

### GET, POST, PUT, DELETE

Los métodos HTTP son comandos estándar que el cliente (por ejemplo, una app o navegador) envía al servidor para indicar qué quiere hacer con un recurso.

En una API RESTful, cada método representa una acción específica sobre un recurso (como un usuario, producto o transacción).

| **Método** | **Acción que realiza**          | **Uso típico**                                        | **Ejemplo en una API**                                    | **Código de respuesta esperado** |
| ---------- | ------------------------------- | ----------------------------------------------------- | --------------------------------------------------------- | -------------------------------- |
| **GET**    | **Leer / Consultar datos**      | Solicitar información sin modificarla.                | `GET /api/productos` → obtiene la lista de productos.     | `200 OK`                         |
| **POST**   | **Crear datos nuevos**          | Enviar datos al servidor para crear un nuevo recurso. | `POST /api/productos` con un JSON de producto.            | `201 Created`                    |
| **PUT**    | **Actualizar datos existentes** | Modificar completamente un recurso existente.         | `PUT /api/productos/5` → actualiza el producto con ID 5.  | `200 OK`                         |
| **PATCH**  | **Actualizar parcialmente**     | Modificar solo una parte del recurso.                 | `PATCH /api/productos/5` → cambia solo el precio.         | `200 OK`                         |
| **DELETE** | **Eliminar datos**              | Borrar un recurso existente.                          | `DELETE /api/productos/5` → elimina el producto con ID 5. | `204 No Content`                 |

En pocas palabras, los cuatro métodos básicos representan el CRUD de HTTP.  

### **Ejemplos reales con JSONPlaceholder**

A continuación se presentan ejemplos reales de los métodos HTTP más utilizados en una API RESTful.  
Todos pertenecen a la API pública [JSONPlaceholder](https://jsonplaceholder.typicode.com), que permite practicar solicitudes reales sin necesidad de crear un backend propio.

**1. GET – Obtener datos**

GET https://jsonplaceholder.typicode.com/users/1


**Respuesta (200 OK):**

```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz"
}
```

**POST – Crear un nuevo recurso**

```
POST https://jsonplaceholder.typicode.com/posts
Content-Type: application/json
```

Cuerpo de la solicitud:

```
{
  "title": "Nuevo artículo",
  "body": "Este es el contenido del post.",
  "userId": 1
}
```

Respuesta (201 Created):

```
{
  "id": 101,
  "title": "Nuevo artículo",
  "body": "Este es el contenido del post.",
  "userId": 1
}
```

**PUT – Actualizar un recurso completo**

```
PUT https://jsonplaceholder.typicode.com/posts/1
Content-Type: application/json
```

Cuerpo de la solicitud:

```
{
  "id": 1,
  "title": "Título actualizado",
  "body": "Nuevo contenido del post.",
  "userId": 1
}
```

Respuesta (200 OK):

```
{
  "id": 1,
  "title": "Título actualizado",
  "body": "Nuevo contenido del post.",
  "userId": 1
}
```

**DELETE – Eliminar un recurso**

DELETE https://jsonplaceholder.typicode.com/posts/1

Respuesta (204 No Content):

El servidor confirma que el recurso fue eliminado (aunque en realidad no borra nada, solo devuelve el código correcto).

## **Cabeceras (headers)**

Los headers (o cabeceras HTTP) son líneas de información adicional que acompañan cada solicitud o respuesta HTTP.
Funcionan como una “ficha técnica” del mensaje: describen cómo deben interpretarse los datos, quién los envía, en qué formato están, o si requieren autenticación. En pocas palabras: los headers son metadatos (información sobre la información).

### ¿Qué tipo de información contienen?

Los headers pueden variar según el tipo de solicitud, pero los más comunes son:

| **Cabecera**      | **Uso principal**                                                  | **Ejemplo**                            |
| ----------------- | ------------------------------------------------------------------ | -------------------------------------- |
| **Content-Type**  | Indica el **formato** del contenido enviado o recibido.            | `Content-Type: application/json`       |
| **Accept**        | Informa al servidor qué tipo de contenido espera el cliente.       | `Accept: application/json`             |
| **Authorization** | Envía credenciales o tokens para autenticar al usuario.            | `Authorization: Bearer 8asd7as9d7a...` |
| **User-Agent**    | Identifica el tipo de cliente o dispositivo que hace la solicitud. | `User-Agent: Mozilla/5.0 (Windows 10)` |
| **Host**          | Especifica el dominio al que se envía la solicitud.                | `Host: api.banco.cl`                   |
| **Cache-Control** | Indica si se puede almacenar en caché la respuesta.                | `Cache-Control: no-cache`              |
| **Cookie**        | Envía información de sesión o preferencias del usuario.            | `Cookie: session_id=abc123`            |

### ¿Por qué son importantes al consumir APIs?

Los headers son esenciales en el consumo de APIs porque:

1. Definen el formato de los datos

    * Las APIs necesitan saber si los datos llegan como JSON, XML, texto, etc.
    * Ejemplo: Content-Type: application/json.

2. Controlan la autenticación y seguridad

    * Muchos endpoints solo responden si el header Authorization tiene un token válido.
    * Permiten personalizar la respuesta
    * Por ejemplo, Accept-Language: es-CL indica que quieres los textos en español.

3. Afectan el rendimiento y la caché

    * Con headers como Cache-Control o ETag, se mejora la velocidad y eficiencia de las solicitudes.

### Ejemplo de una solicitud HTTP completa con cabeceras

Supón que una app consulta el saldo de un usuario en la API de un banco:

```
GET /api/saldo HTTP/1.1
Host: api.banco.cl
User-Agent: Mozilla/5.0 (Windows 10)
Authorization: Bearer eyJhbGciOiJIUzI1...
Accept: application/json
Content-Type: application/json
Cache-Control: no-cache
```

Respuesta del servidor:

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "usuario": "Daniel",
  "saldo": 200000
}
```

## Reflexión

Este trabajo me permitió comprender con claridad qué es realmente un backend.
Mientras escribía el ejemplo de una solicitud HTTP para una aplicación bancaria, me di cuenta de que confundía algunos conceptos, lo que me llevó a investigar y entender la diferencia entre frontend, backend y API. Ahora sé que la API es una parte del backend encargada de la comunicación con el cliente.

También comprendí la importancia del protocolo HTTPS como base de la seguridad en la web.
Aprendí a interpretar los códigos de estado (status codes) y entendí cómo pueden ayudarme a diagnosticar errores o verificar el funcionamiento correcto de mis propias aplicaciones.

Además, los métodos HTTP me ayudaron a profundizar en cómo se establece la comunicación con una API REST, mientras que el estudio de los headers me permitió comprender mejor el intercambio de información entre cliente, API y servidor.

En conjunto, este trabajo fortaleció mi entendimiento sobre el flujo completo de comunicación en una aplicación web moderna.
