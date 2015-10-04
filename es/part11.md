# 11. http2 en curl

El [proyecto curl](http://curl.haxx.se/) ha estado ofreciendo soporte experimental para http2 desde septiembre de 2013.

Siguiendo el espíritu de curl, pretendemos ofrecer todos los aspectos de http2 en la medida de nuestras posibilidades. Es un uso común de curl se usado como una herramienta de testeo, y una manera de “pingar” manualmente sitios web, y es nuestra intención mantenerlo igualmente para http2.

curl utiliza una librería externa [nghttp2](https://nghttp2.org/), para implementar la funcionalidad de la capa trama http2. curl necesita la versión nghttp2 1.0 o superior.

Actualmente en Linux, curl y libcurl no están siempre desplegados con el soporte activado para HTTP/2.

## 11.1. Parecido a HTTP 1.x

Internamente curl convertirá las cabeceras http2 al estilo de cabeceras HTTP 1.x, y serán entregadas al usuario, de manera que aparecerán de maneras similar al HTTP tradicional. Esto facilitará la transición a todos los usos actuales de curl. De manera similar, curl convertirá las cabeceras salientes con el mismo estilo. Serán pasadas en estilo HTTP 1.x y las convertirá al vuelo cuando se esté hablando con servidores http2. Esto permitirá que los usuarios no se preocupen demasiado sobre la versión HTTP en particular que esté siendo utilizada.

## 11.2. Texto plano, inseguro

curl  soporta http sobre TCP estándar utilizando la cabecera “Upgrade:”. Si realizas una petición HTTP solicitando http2, curl preguntará al servidor si es posible actualizar la conexión a  http2.

## 11.3. Qué bibliotecas TLS

curl tiene soporte para una amplia variedad de bibliotecas TLS distintas para su implementación TLS, y esto sigue siendo válido para el soporte http2. El desafío de TLS en el mundo http2, es el soporte APLN y en cierta medida el soporte NPN.

Compila curl con versiones modernas de OpenSSL o NSS, para obtener soporte APLN y NPN. Utiliza GnuTLS o PolarSSL y soportará ALPN pero no NPN.

## 11.4. Uso de línea de comando

Para indicar a curl que utilice http2, tanto e texto plano como sobre TLS, hay que utilizar la opción --http2 (Esto es “menos menos http2”). De momento curl por defecto ofrece HTTP/1.1 así que es necesario una opción extra cuando se quiere http2.

## 11.5. Opciones de libcurl

### 11.5.1 Activar HTTP/2

Tu aplicación utilizará URLs normales con https:// o http://, pero habrá que indicar el parámetro CURLOPT_HTTP_VERSION de curl_easy_setopt a  CURL_HTTP_VERSION_2 para intentar que libcurl utilice http2. Intentará de la mejor forma conectar con http2, pero volverá a HTTP 1.1 si éste falla.

### 11.5.2 Multiplexación

libcurl intenta mantener comportamientos existentes, por lo que se hace necesario que se active la multiplexación HTTP/2 en tu aplicación mediante la opción 
[CURLMOPT_PIPELINING](http://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html). De lo contrario se seguirá utilizando una petición en cada momento por conexión.

Otro pequeño detalle a tener en cuenta es que al solicitar varias transferencias al mismo tiempo con libcurl, usando su interfaz multiple, una aplicación puede empezar varias transferencias al mismo tiempo, y que si se pretende que libcurl espere e introduzca todas ellas por la misma conexión en lugar de abrir nuevas conexiones, existe la opción [CURLOPT_PIPEWAIT](http://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html).

### 11.5.3 Server push

A partir de la versión libcurl 7.44.0 se da soporte para la funcionalidad HTTP/2 server push. Puedes utilizarla indicando un callback con la opción [CURLMOPT_PUSHFUNCTION](http://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html).

Si la aplicación acepta el push, se creará una nueva transferencia con un manejador fácil CURL, y se enviará contenido, como cualquier otra transferencia.