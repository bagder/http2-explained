# 6. El protocolo http2

Suficiente sobre antecedentes, historia y asuntos políticos que nos han traído hasta aquí. A continuación vamos a bucear en asuntos específicos del protocolo. Los bits y los conceptos que crean http2.

## 6.1. Binario

http2 es un protocolo binario.

Déjame asentar esto un minuto. Si eres una persona que ha estado involucrada en protocolos de Internet, las posibilidades de que reacciones instintivamente contra esta opción y de que calcules argumentos que expliquen como los protocolos de texto/ascii son superiores por permitir a los humanos hacer consultas a mano con telnet.
http2 es en binario para conseguir hacer el entramado (framing) mucho más sencillo. Determinar el comienzo y el final de cada trama es una de las cosas realmente complicadas en HTTP 1.1 así como en todos los protocoles en texto en general. Eliminando espacios en blanco opcionales y distintas formas de escribir la misma cosa, las implementaciones serán mucho más simples.

Igualmente  hace mucho más fácil separar las partes de protocolo de las tramas en si, lo que en HTTP1 está confusamente entremezclado.

El hecho de las funcionalidades de compresión del protocolo y de que casi siempre correrá sobre TLS, restan importancia a que el protocolo no sea en texto plano, ya que la información tampoco viajaría en texto de cualquier manera. Simplemente tendremos que acostumbrarnos a utilizar el inspector de Wireshark o algo similar para determinar que está pasando a nivel de protocolo con http2.

El Debugging en este protocolo deberá hacerse utilizando herramientas como curl o analizando el tráfico de red con el disector de tráfico http2 de  Wireshark u otra herramienta similar.

## 6.2. El formato binario

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2 envía tramas en binario. Pueden enviarse distintos tipos de trama, y todos ellos comienzan de la misma manera:

Tipo, Tamaño, Flags, Identificador de Flujo y la carga útil de la trama.

Existen 10 tipos de tramas definidos en la especificación http2 y los dos tipos fundamentales que se mapean con las funcionalidades de HTTP 1.1 son DATA y HEADERS. Más adelante en el documento, voy a describir algunas de las tramas en algo más de detalle.

## 6.3. Flujos multiplexados

El identificador de flujo mencionado en la sección anterior en la descripción de la trama binaria, asocia cada trama enviada a través de http2 con un “flujo”. Un flujo es una asociación lógica. Una secuencia de tramas independiente bidireccional intercambiados entre el cliente y el servidor dentro de una conexión http2.

Una conexión http2 puede contener múltiples flujos abiertos concurrentes, ya sea con tramas de finalización de distintos flujos. Los flujos pueden ser establecidos y usados unilateralmente por el cliente o el servidor, y pueden ser cerrados por cualquiera de los dos puntos. El orden en el que se envía cada flujo es significativo. Los receptores procesan las tramas en el orden en el que son recibidos.

La multiplexación de los flujos significa que paquetes de distintos flujos se mezclan en la misma conexión. Dos (o más) trenes independientes se convierten en uno único y luego son separados en el otro punto. Aquí están los dos trenes:

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

Los dos trenes multiplexados sobre la misma conexión:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Prioridades y dependencias

Cada flujo tiene una prioridad (denominada también "peso"), que se usa para indicar al peer que flujos deben ser considerados más importantes en caso de contar con restricciones de recursos que obliguen al servidor a seleccionar que flujo enviar primero.

Utilizando la trama PRIORITY, un cliente puede indicar al servidor que otro flujo, es dependiente un flujo. Esto permite al cliente construir un "árbol" de prioridades, en el que varios “flujos hijos”, pueden depender de que vaios "flujos padre" sean completados.

Los pesos de prioridad y las dependencias pueden ser cambiados dinámicamente en tiempo real, lo que permitirá en una página con muchas imágenes por ejemplo, que el navegador cambie la prioridad en las solicitudes a medida que el usuario hace scroll; o al cambiar entre tabs, puede priorizar el conjunto de flujos que acaba de coger el foco del usuario.

## 6.5. Compresión de Cabeceras

HTTP es un protocolo sin estado. De manera resumida significa que cada petición debe indicar al servidor los detalles necesarios para que la petición sea atendida, sin que el servidor tenga que almacenar gran cantidad de información y meta-información de peticiones anteriores. Ya que http2 no cambia para nada este paradigma, debe cumplirlo igualmente.

Esto convierte el HTTP en repetitivo. Cuando un cliente solicita muchos recursos de un mismo servidor, como imágenes para una página web, habrán una gran serie de solicitudes que parecerán prácticamente idénticas. Una serie de algo casi idéntico, está pidiendo compresión a gritos.

Mientras el número de objetos sigue en aumento, como ya se ha comentado con anterioridad, el uso y el tamaño de las cookies ha seguido creciendo igualmente durante este tiempo. Esta cookies deben ser incluidas en todas las peticiones. Mayormente las misma cookies en cada petición.

El tamaño en la peticiones HTTP 1.1 ha ido haciéndose tan grande en los últimos tiempos que han terminado siendo más grandes que la ventana TCP inicial, lo que hace que el envío de la petición sea muy lento al necesitar recibir un ACK de vuelta del servidor antes de completar la petición por completo. Este sería otro argumento más para la compresión.

### 6.5.1. La compresión tiene truco

Las compresiones HTTPS y SPDY han resultado ser vulnerables a ataques [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) y [CRIME](https://en.wikipedia.org/wiki/CRIME). Insertando texto conocido en el flujo, y averiguando que es lo que cambio en el resultado comprimido, un atacante puede averiguar que se está enviando.

Hacer compresión en contenido dinámico para un protocolo evitando las vulnerabilidades de estos dos ataques, requiere reflexiones y consideraciones cuidadosas. Esto es lo que el grupo HTTPbis intenta hacer.

Se introduce [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2, que – como sugiere adecuadamente su nombre – es un formato de compresión especialmente diseñado para cabeceras http2 y estrictamente hablando, está siendo especificado en otro borrador separado de Internet. El nuevo formato, junto con otras contra-medidas como un bit que solicita a los intermediarios no comprimir una cabecera específica o el desplazamiento opcional de tramas deberían hacer mucho más difícil llegar a romper esta compresión.

En palabras de Roberto Peon (uno de los creadores de HPACK):

> “HPACK ha sido diseñado para dificultar que una implementación conforme filtre
> información, para hacer que la codificación y decodificación muy rápida y
> barata, para proporcionar al receptor control del tamaño del contexto de la 
> compresión, para permitir que un proxy reindice (por ejemplo, estados 
> compartidos entre un frontal y una trasera a través de un  proxy), y para
> una comparación rápida de cadenas codificadas mediante huffman”.

## 6.6. Reset - piensa diferente

Uno de los inconveniente con HTTP es que cuando un mensaje HTTP ha sido enviado con cierto tamaño especificado en la cabecera Content-Length, no puedes ser parado fácilmente. A menudo (aunque no siempre), se puede cerrar la conexión TCP, pero pagando el precio de tener que negociar un handshake TCP de nuevo.

Una solución mejor para esto es simplemente parar el mensaje,  y comenzar uno nuevo. En http2, esto puede hacerse con la trama RST_STREAM, que previene derrochar el ancho de banda y permite mantener la conexión.

## 6.7. Server push

Esta funcionalidad también se conoce como “cache push”. La idea aquí es que si el cliente solicita un recurso X determinado, el servidor determina que es altamente probable que el cliente solicite también el recurso Z, de manera que es enviado sin que el cliente lo solicite. Esto ayudará al cliente a tenerlo en su cache, de manera que ya estará allí cuando sea necesario.

El envío desde el servidor (Server push) es algo que un cliente debe permitir explícitamente, e incluso si está permitido, podrá a su propia elección rápidamente terminar el flujo con un RST_STREAM no permitiendo así ningún Server Push en particular.

## 6.8. Flow Control (Control de Flujo)

Cada flujo individual sobre http2 tiene su propia ventana de flujo anunciada, sobre la que el otro extremo puede enviar información. Si conoces como funciona SSH, es muy similar en estilo y espíritu.

Para cada flujo, cada uno de los extremos tiene que indicar a su peer que tiene más espacio para la información entrante, de manera que el otro extremo únicamente puede enviar tanta información como espacio se dispnga, hasta que la ventana sea hecha más grande. Únicamente existe control de flujo para las tramas DATA.
