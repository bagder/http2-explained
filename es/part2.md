# 2. HTTP hoy

HTTP 1.1 se ha convertido en un protocolo usado por prácticamente todo el mundo en Internet. Existen inversiones enormes realizadas en protocolos e infraestructura para aprovecharlo. Esto se ha interpretado cómo que a menudo es más fácil hacer funcionar algo sobre HTTP, que construir algo propiamente nuevo.

## 2.1 HTTP 1.1 es enorme

Cuando se creó HTTP y fue liberado al mundo, fue concebido como un protocolo más bien simple y sencillo, pero el tiempo ha demostrado lo contrario. HTTP 1.0 en el RFC 1945 es una especificación de 60 páginas publicada en 1996. El RFC 2616 que describe HTTP 1.1, fue publicado sólo tres años más tarde, en 1999 y creció considerablemente hasta las 176 páginas. Todavía, cuando desde el EIETF trabajamos en la actualización de la especificación que fue separada en seis documentos, con un número mucho mayor de páginas en total (resultando en el RFC 7230 y familia). De cualquier modo, HTTP 1.1 es grande e incluye una gran variedad de detalles y sutilezas, sin olvidar una gran cantidad de importantes piezas opcionales.

## 2.2. Un mundo de opciones

La naturaleza de HTTP 1.1 con multitud de pequeños detalles y opciones disponibles para extensiones posteriores, ha generado un ecosistema de software que ha hecho que casi ninguna implementación esté enteramente completada – y realmente es imposible decir que significa implemente por completo la especificación. Esto ha provocado que funcionalidades poco usadas en un principio, no hayan sido comúnmente implementadas ni por supuesto usadas.

Más tarde, esto ha causado un problema de interoperatibilidad, cuando clientes y servidores han comenzado a utilizar esas funcionalidades. HTTP Pipelining es el ejemplo principal de este tipo de funcionalidad.

## 2.3. Uso inadecuado de TCP

HTTP 1.1 nunca ha conseguido aprovechar la ventajas de todo el potencial y rendimiento que ofrece TCP. Los clientes HTTP y los navegadores tienen que ser muy creativos para encontrar soluciones que reduzcan los tiempos de carga de las páginas.

Han existido otros intentos en paralelo en los últimos años, que han confirmado que no es sencillo reemplazar TCP, y que por lo tanto hay que seguir mejorando tanto TCP, como los protocolos sobre éste.
Simplemente, TCP puede usarse mejor evitando pausas o momentos de tiempo que pueden usarse para enviar o recibir más información. Las siguiente secciones vienen a describir algunos de estos defectos.

## 2.4. Tamaños de transferencia y número de recursos

Al observar la tendencia en los sitios web más populares en la carga de su página principal, emerge un patrón muy claro. En los últimos años la cantidad de información que debe ser consumida ha ido elevándose gradualmente hasta más allá de 1.9MB. Lo que es más importante en este contexto, es que de media, se necesitan más de cien recursos individuales para mostrar cada página.

Como se muestra en el siguiente gráfico, la tendencia ha estado en marcha durante un tiempo, y no hay indicación clara de que vaya a cambiar próximamente. Muestra el tamaño total de transferencia (en verde) y el número total de peticiones de media (en rojo) para servir los sitios web más populares del mundo, así como su evolución en los últimos cuatro años.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5. Latencia asesina

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 es muy sensible a la latencia, en parte debido a que “HTTP Pipelining” todavía cuenta con demasiados problemas como para seguir apagado para un gran porcentaje de usuarios.

En los últimos años hemos ido viendo como aumentaba el ancho de banda disponible para las personas. No se ha alcanzado el mismo nivel de mejora reduciendo la latencia. Enlaces de alta latencia, como es el caso de las tecnologías móviles actuales, hacen muy complicado conseguir una buena y sobre todo rápida experiencia de usuario en web, incluso contando con un gran ancho de banda.

Otro caso de uso típico que necesita de enlaces con latencia baja, son algunos tipos de vídeo, como vídeo conferencias, juegos u otros casos similares donde no es enviado únicamente un flujo pre-generado de vídeo.

## 2.6. Bloqueo del primero de la fila

“HTTP Pipelining” es la manera de enviar otra solicitud mientras se está esperando a la respuesta de la solicitud anterior. En muy similar a esperar en el mostrador de un banco o supermercado. Tú nuca sabes si la persona delante de ti es un cliente rápido, o uno molesto que estará mil horas antes de irse: bloqueo del primero de la fila (“Head of line blocking”).

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Por supuesto que puedes tener cuidado a la hora de escoger una cola y escoger la que creas vaya a ir más rápido, incluso a veces podrás iniciar tu propia cola. Pero al final siempre habrá que tomar una decisión,  y una vez esté tomada, no podrás cambiar de fila. 

Crear una nueva fila supone una penalización en el rendimiento y el uso de recursos, de manera que no es escalable más allá de un número pequeño de filas. No existe una solución perfecta a este problema.
Incluso hoy, en 2015, los navegadores son publicados con la opción “HTTP pipelining” deshabilitada por defecto.
Se puede encontrar más información sobre está materia leyendo por ejemplo la [entrada 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354) de Firefox bugzilla.