# 5. Conceptos de http2

Entonces, ¿qué consigue http2? ¿Donde está el límite para lo que el grupo HTTPbis tiene encargado hacer?

Eran en realidad bastante estrictos y dejaban muy poca oportunidad para la capacidad de innovación dentro del equipo.

- Tiene que mantener los paradigmas HTTP. Se mantiene como un protocolo que envía peticiones al servidor a través de TCP.

- Las URLs http:// y https:// no pueden ser cambiadas. No puede existir un nuevo esquema para esto. La cantidad de contenido usando estas URLs es demasiado grande para pretender que cambie.

- Servidores y Clientes HTTP1 se mantendrán durante décadas, deberemos ser capaces de hacer un proxy hacia servidores http2.

- Así pues, los proxies deberán ser capaces de mapear funcionalidades http2 a clientes HTTP 1.1 una a una.

-  Eliminar o reducir partes opcionales del protocolo. Esto no era tanto un requisito, como un mantra que llegaba desde SPDY y el equipo de Google. Asegurándote de que todo es obligatorio, no hay manera de que no implementes algo ahora que se convierta en una trampa más adelante.

- No más versiones menores. Se decidió que tanto clientes como servidores serían o no compatibles con http2. Si aparece una necesidad de extender el protocolo o modificar las cosas, entonces habrá nacido http3. No hay más versiones menores en http2.

## 5.1. http2 para esquemas URI existentes

Como se mencionado con anterioridad, los esxquemas de URI existentes, no podrán ser modificados, así que http2 deberá construirse usando los esquemas existentes. Como actualmente se están utilizando en HTTP 1.x, obviamente necesitamos una manera de actualizar el protocolo a http2 o solicitar al servidor de otra manera la utilización de http2 en lugar de protocolos más obsoletos.

HTTP 1.1 tenía una manera definida para hacer esto denominada la cabecera “Upgrade:”, que permitía al servidor enviar una respuesta usando el nuevo protocolo al recibir ¡la a través del protocolo viejo. Todo con el coste de una petición (“round-trip”).

Esa penalización de un round-trip, no era algo aceptable por el equipo de SPDY, y ya que su implementación de SPDY únicamente funcionaba sobre TLS, desarrollaron una nueva extensión TLS utilizada para atajar la negociación de manera significativa. Utilizando esta extensión, denominada NPM (Next Protocol Negotiation), el servidor comunica al cliente que protocolo conoce, así el cliente puede proceder y utilizar su protocolo preferido.

## 5.2. http2 para https://

Se ha prestado mucha atención para conseguir un correcto comportamiento de http2 sobre TLS. SPDY funciona únicamente sobre TLS y se ha intentado con mucha fuerza que TLS sea igualmente obligatorio para http2, pero no se ha conseguido un consenso así que http2 se ha publicado con TLS opcional. Aún así, dos de los fabricantes más destacados han dicho que únicamente implementarán http2 sobre TLS: la iniciativa Mozilla Firefox y Google Chome. Hoy por hoy, dos de los navegadores principales.

Las razones para escoger solo-con-TLS incluyen el respeto por la privacidad del usuarios y medidas precoces que mostraban un mayor índice de éxito en nuevos protocolos sobre TLS. Esto se debe a la suposición de cualquier tráfico por el puerto 80 es HTTP 1.1, de manera que cualquier dispositivo que intercepte el tráfico puede interferir y destruirlo cuando se trata de otro protocolo distinto a HTTP 1.1.

La obligatoriedad de TLS ha sido causa de mucho movimiento y voces agitadas en las listas de correo y las reuniones – ¿es bueno o es malo? Es un tema infectado –  ¡ten esto en cuenta cuando se lo preguntes cara a cara a un miembro de HTTPbis!

De manera similar, ha habido un fiero y largo debate sobre si http2 debe imponer la lista de cifrados que deben ser obligatorios al usar TLS, o sobre si crear una lista negra de unos cuantos o si por el contrario no se debiera obligar nada a la capa TLS, y dejar la decisión al grupo de trabajo TLS. La especificación ha determinado que TLS debe ser al menos la versión 1.2, y ha impuesto algunas restricciones en el cifrado.

## 5.3. Negociación http2 sobre TLS

Next Protocol Negotiation (NPN), es el protocolo utilizado para negociar SPDY con servidores TLS. Como no se trataba de un estándar adecuado, fue llevado a la IETF y de aquello surgió ALPN: Application Layer Protocol Negotiation. ALPN es lo que se está promoviendo para ser usado con http2, mientras que los clientes y servidor SPDY seguirán utilizando NPN.

El hecho de que NPN estuviera primero y que ALPN haya tardado un poco en el proceso de estandarización, ha llevado a que muchos clientes y servidores http2 hayan implementado de forma prematura soporte para ambas extensiones para la negociación de http2. Igualmente como NPN se utiliza para SPDY y muchos servidores ofrecen tanto SPDY como http2, dar soporte tanto para NPN como ALPN tiene perfecto sentido.

La principal diferencia de ALPN respecto a NPN está en quién decide que protocolo hablar. Con ALPN el cliente le indica al servidor el listado de protocolos en el orden de preferencia, y el servidor escoge el que él quiere, mientras que con NPN es el cliente quien toma esa última decisión.

## 5.4. http2 for http://

Como se ha mencionado brevemente con anterioridad, para HTTP 1.1 en texto plano, la manera de negociar http2 es solicitar al servidor mediante una cabecera Upgrade:. Si el servidor habla http2, responderá con un  estado “101 Switching” y a partir de entonces hablar http2 en esa conexión. Por supuesto que te das cuenta que este procedimiento de actualización está costando un viaje de red “round-trip”, pero por otra parte la conexión http2 debería ser mantenida y reutilizada de manera más generalizada que las conexiones HTTP1.

Aunque algunos representantes de navegadores han declarado que no implementarán este modo de hablar http2, el equipo de Internet Explorer ha comunicado que ellos lo harán, así como curl, que  ya soporta actualmente esta modo.
