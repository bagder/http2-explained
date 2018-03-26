# 8. Un mundo http2

Entonces, ¿Cómo serán las cosas cuando http2 sea adoptado? ¿Será adoptado?

## 8.1. Cómo afectará http2 en humanos normales

http2 aún no está ampliamente ni desplegado ni usado. No podemos decir como serán las cosas. Hemos observado como se ha usado SPDY  y se pueden hacer suposiciones y cálculos basados en éste y otros experimentos pasados y presentes.

http2 reduce el número de “round-trips” de red necesarios y  con la multiplexación y el descarte rápido de flujos no deseados, evita el dilema del bloqueo del primero de la fila.

El protocolo permite un alto número de flujos paralelos más allá de cualquier sitio que actualmente utilice la técnica de dominios fragmentados (sharding).

Utilizando la funcionalidad de prioridades correctamente en los flujos, hay altas probabilidades de que el cliente realmente reciban la información importante antes de la información menos priorizada.
Juntando todo esto, diría que existe una muy buena oportunidad de que se esté apuntando a tiempos de carga mucho más rápidos así como hacia sitios web más reactivos. En pocas palabras: un mejor experiencia web.

No creo que se pueda decir todavía cuánto más rápido o cuantas mejoras vamos a llegar a ver. Para empezar la tecnología está todavía en un fase muy temprana y aún no se ha comenzado a ver clientes y servidores ajustando implementaciones para aprovechar todo el poder que nos está ofreciendo el nuevo protocolo.

## 8.2. Cómo afectará http2 al desarrollo web

Durante los años los desarrolladores web así como sus entornos de desarrollo, han ido recopilando una gran colección de trucos y herramientas para solventar los problemas con HTTP 1.1, de los que he hablado al comienzo del este documento como justificación principal para http2.

Muchos de estos atajos que tanto herramientas como desarrolladores utilizan por defecto sin pensar, probablemente afectarán negativamente el rendimiento de http2, o al menos hará que no se aprovechen los nuevos súper poderes de http2. Tanto “spriting” como “inlining”, probablemente no deban utilizarse con http2. La fragmentación en varios dominios (“sharding”), será perjudicial en http2, ya que  probablemente haya más beneficio utilizando menos conexiones..

El principal problema es por supuesto que tanto los sitios web como los desarrolladores web, necesitarán desarrollar y desplegarse en un mundo en el que a corto plazo al menos, existan clientes tanto HTTP1.1 como http2, de manera que el reto será conseguir el máximo rendimiento sin tener que ofrecer dos frontales diferentes.

Únicamente por estas razones, sospecho que pasará algo de tiempo antes de que veamos alcanzado el máximo potencial de http2.


## 8.3. Implementaciones http2

Intentar documentar las implementaciones especificas en un documento como este, es por supuesto hacerlo en vano y está condenado al fracaso porque estará desfasado en un periodo muy corto de tiempo. En lugar de esto, explicaré la situación en un término más amplio y simplemente haré una referencia para los lectores hacia a la [lista del implementaciones](https://github.com/http2/http2-spec/wiki/Implementations) en el sitio web de http2.

Han existido una gran cantidad de implementaciones desde un primer momento, y este número ha ido incrementándose durante el trabajo con http2. Mientras se escribe esto, existen más de 30 implementaciones listadas, la mayoría implementando la versión final.

Firefox ha sido el navegador que ha estado encabezando los borradores más nuevos. Twitter ha estado a la altura ofreciendo sus servicios sobre http2. Google ha comenzado a ofrecer soporte http2 en algunos servidores de pruebas desde abril de 2014 y desde mayo de 2014, han ofrecido soporte http2 en las versiones de desarrollo de Chrome. Microsoft ha presentado que soporta http2 desde una “tech preview” de su próxima versión de Internet Explorer.

Tanto curl como libcurl soportan http2 inseguro así como basado en TLS utilizando una de las distintas bibliotecas TLS.

[H2O](https://h2o.examp1e.net/), [Apache Traffic Server](https://trafficserver.apache.org/) y [nghttp2](https://nghttp2.org/) han publicado todos ellos servidores con soporte para http2.

### 8.3.1. Implementaciones pendientes

Las dos opciones más populares en servidores web, Apache HTTPD y Nginx ofrecer soporte para SPDY, pero todavía no han publicado soporte oficial para http2 en ningún release. Ngnix ha publicado un ["alpha patch"](https://www.nginx.com/blog/early-alpha-patch-http2/) así como el módulo de Apache parar HTTP/2 denominado [mod_h2](https://icing.github.io/mod_h2/) parece que está encaminado a ser incluído en una release pública muy pronto.

## 8.4. Críticas comunes a http2

Durante el desarrollo de este protocolo ha existido cierto debate en determinados aspectos, y algunas personas creen que el protocolo se ha diseñado completamente mal. Me gustaría mencionar algunas de las quejas más comunes, así como los argumentos en su contra:

### 8.4.1. “El protocolo está diseñado o hecho por Google”

Existen variaciones que implican un mundo todavía más dependiente o controlado por Google. No es cierto. El protocolo ha sido desarrollado desde el IETF de la misma manera en la que se han venido desarrollando protocolos en los últimos 30 años. De cualquier manera, todo reconocemos el impresionante trabajo hecho por Google con SDPY que no solo demostró que era posible desplegar un nuevo protocolo sino que también aportó los números que indicaban que se podrían conseguir mejoras.

Google ha [anunciado](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) públicamente1 que van a retirar el soporte para SPDY y NPN en Chrome a partir de 2016, y que aconsejan a los servidores utilizar HTTP/2 en su lugar.

### 8.4.2. “El protocolo solo es útil para navegadores”

Esto es medio verdad. Una de las principales razones para el desarrollo de http2, es arreglar HTTP pipelining. Si tu caso de uso no tiene problema, entonces es probable que http2 no te suponga demasiada mejora. Aunque ésta no sea la mejora principal en el protocolo, es bastante significativa.

Según distintos servicios se vayan dando cuenta del gran poder y posibilidades que tienen los flujos multiplexados a través de una única conexión, sospecho que veremos más aplicaciones utilizando http2.

Pequeñas APIs REST o uso más simples de programación con HTTP 1.x puede que no encuentren que el salto a http2 ofrezca demasiadas ventajas. Aunque igualmente, tampoco deberían existir demasiados inconvenientes con http2 en la mayoría de los casos.

### 8.4.3. “El protocolo solo es útil para sitios grandes”

Para nada. La capacidad de multiplexar mejorará notablemente la experiencia de usuario para sitios pequeños con conexiones de altas latencias sin distribuciones geográficas. Los sitios más grandes suelen ser más rápidos, ofreciendo conexiones distribuidas, con tiempos “round-trip” más cortos para los usuarios.

### 8.4.4. “Su uso de TLS lo hace más lento”

Esto puede ser cierto hasta cierto punto. El “handshake” TLS añade un poco de tiempo extra, pero actualmente se está trabajando en reducir el número de viajes “round-trips” necesarios para TLS. El trabajo extra para hacer TLS a través de la conexión comparado con texto plano no es insignificante y es claramente notable al usar más CPU y energía para un mismo patrón de tráfico sobre protocolo no seguro. Cuantificar cuanto y su impacto está sujeto a opiniones y medidas. El sitio istlsfastyet.com ofrece por ejemplo una fuente de información.

Telecom y otros operadores de red, por ejemplo la ATIS Open Web Alliance, han dicho que necesitan tráfico no cifrado1 para ofertar cacheo, compresión u otras técnicas necesarias para ofertar una experiencia de usuario rápida a través de satélites, en aviones por ejemplo.

http2 no establece el uso de TLS como obligatorio, así que no se deberían combinar los términos.
Muchos usuarios de Internet han expresado preferencia en un uso más extenso de TLS para que podamos ayudar a proteger la privacidad de los usuarios.

Ciertos experimentos han demostrado que usar TLS aporta un mayor índice de éxito que implementar nuevos protocolos en texto plano a través del puerto 80 ya que existen demasiados dispositivos desplegados en el mundo que pueden interferir con aquello que crean que es HTTP1.1 si es que va por el puerto 80 y puede parecer HTTP a veces.

Por último, gracias a la multiplexación de flujos de http2 en una única conexión, en el caso de uso de navegadores normales, se realizarán muchos menos “handshakes” TLS, así que se conseguirá un rendimiento más raṕido qe utilizando HTTPS sobre HTTP 1.1.

### 8.4.5. “No ser ASCII es un factor decisivo”

Si, nos gusta ser capaces de ver los protocolos claramente, ya que lo hace mucho más  fácil para debuggear y tracear. Pero los protocolos basados en texto son más susceptibles a error, y provocan muchísimos errores en su interpretación.

Si no soportas un protocolo binario, entonces estas descartando TLS o la compresión en HTTP 1.x, que han estado utilizando muchísimo tiempo.

### 8.4.6. “No es más rápido que HTTP/1.1”

Por supuesto que es algo sujeto a debate y a discusión en cómo se mide que significa más rápido, pero ya en los días de SPDY, se realizaron multitud de pruebas que demostraban que la página cargaba más rápido (por ejemplo ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) de la gente de la Univesidad de Washington y ["Evaluating the Performance of SPDY-enabled Web Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) por Hervé Servy) y dichos experimentos se han repetido también con http2. Tengo ganas de ver publicados los resultados de esas pruebas y experimentos. 

Una primera prueba básica realizada por [httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) podría implicar que HTTP/2 cumple sus promesas.

### 8.4.7. “No respeta las capas”

En serio, ¿es eso tu argumento? Las capas no son pilares intocables de una religión global y si hemos pasado algunas zonas grises al hacer http2, ha sido en interés de hacer un protocolo bueno y efectivo a partir de las premisas iniciales.

### 8.4.8. “No arregla varios defectos de HTTP/1.1”

Eso es cierto. Con el objetivo específico de mantener los paradigmas HTTP/1.1, hemos tenido que mantener ciertas funcionalidades anticuadas. Por ejemplo ciertas cabeceras comunes como las temidas cookies, cabeceras de autenticación y más. Como contrapunto al mantenimiento de estos paradigmas, se ha conseguido un protocolo que puede desplegarse sin la obligación de sustituir o reescribir una inconcebible cantidad de trabajo en dicha actualización. http2 es básicamente una nueva capa de “framming”.

## 8.5. ¿Estará http2 desplegado masivamente?

Es demasiado pronto para afirmarlo con seguridad, pero puedo suponer y hacer una estimación, y eso es lo que haré aquí.

Los “no-nos” dirán “mira que bien hecho está IPv6” como ejemplo de un nuevo protocolo que ha necesitado décadas para empezar a estar ampliamente desplegado. Aunque http2 no es IPv6. Es un protocolo por encima de TCP que usa los mecanismos de actualización normales de HTTP, sus números de puerto, TLS, etc. No va a necesitar que cambien para nada la mayoría de routers o firewalls.

Google demostró al mundo con su trabajo con SPDY que un nuevo protocolo como este puede ser desplegado y utilizado por navegadores y servicios desde distintas implementaciones en un periodo de tiempo razonablemente corto. Aunque  la cantidad de servidores en Internet ofreciendo SPDY está en ñle rango del 1%, la cantidad de información que manejan esos servidores es mucho más grande. Algunos de los sitios web más populares actualmente en el mundo, ofrecen SPDY.

http2, basado en los mismos paradigmas básicos que SPDY, diría que tiene más probabilidades de ser desplegado desde que es un protocolo del IETF. El despliegue de SPDY siempre fue algo retenido por el estigma de “ser un protocolo de Google”.

Hay varios grandes navegadores detrás del despliegue. Representantes de Firefox, Chrome e Internet Explorer han expresado que ofrecerán navegadores con soporte http2 y ya han mostrado implementaciones funcionando.

Existen grandes operadores de servidor que van a ofrecer http2 pronto, entre los que se incluye Google, Twitter y Facebook así como esperamos ver soporte http2 pronto en servidores web populares como Apache HTTP Server y nginx. H2o es un nuevo increíblemente rápido servidor HTTP con soporte http2que tiene potencial.

Algunos de los fabricantes de proxy más grandes, incluyendo HAProxy, Squid and Varnish han manifestado intenciones de añadir soporte http2.

Casi a finales de 2015, la cantidad de tráfico en http2 ha continuado incrementándose. A comienzos de septiembre, el uso en Firefox 40 era del 13% del total del tráfico HTTP y el 27% del tráfico HTTPS, mientras que Google está recibiendo aproximadamente un 18% de trafico HTTP/2. Hay que tener en cuenta que Google está experimentando con nuevos protocolos (ver QUIC en la sección 12.1), lo que hace bajar las medidas de uso para http2.