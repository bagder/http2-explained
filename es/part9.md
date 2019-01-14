# 9. http2 en Firefox

Firefox ha estado siguiendo la pista a los borradores muy de cerca, y ofreciendo implementaciones http2 de prueba durante muchos meses. Durante el desarrollo del protocolo http2, los clientes y servidores tienen que ponerse de acuerdo sobre que versión del borrados están utilizando, lo cual dificulta levemente ejecutar pruebas. Hay que asegurarse de que el cliente y el servidor implementan la misma versión del borrador del protocolo.

## 9.1. Primero, asegurar que está activado

Desde la versión 35, publicada el 13 de enero de 2015, Firefox soporta http2 por defecto.

Entrar en 'about:config' en la barra de direcciones, y buscar la opción denominada “network.http.spdy.enabled.http2draft”. Habrá que asegurarse qu está puesta a true. Firefox 36 añadió otra opción de configuración llamada “network.http.spdy.enabled.http2” que está a true por defecto. Ésta última controla la versión simple de http2, mientras que la primera activa y desactiva la negociación de la revisiones de los borradores de http2. Ambas opciones están actividad desde Firefox 36.

## 9.2. Sólo TLS

Recordad que Firefox únicamente soporta http sobre TLS. Solo verás http2 en acción con Firefox al entrar en sitios con https:// que ofrezcan soporte http2.

## 9.3. ¡Transparente!

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

No existe ningún elementos en la interfaz de usuario que nos diga que se está hablando http2. No es fácilmente detectable. Un modo de averiguarlo es activar “Web developer->Network” y comprobar las cabeceras de respuesta para ver que está enviando el servidor. La respuesta será entonces “HTTP/2.0” y algo más. Firefox inserta su propia cabecera denominada “X-Firefox-Spdy:” como se aprecia en la captura de pantalla de arriba.
Las cabeceras que se ven en la herramienta de red al utilizar http2 han sido convertidas desde el formato binario de http2, para parecerse al estilo clásico de las cabeceras de HTTP 1.x.No existe ningún elementos en la interfaz de usuario que nos diga que se está hablando http2. No es fácilmente detectable. Un modo de averiguarlo es activar “Web developer->Network” y comprobar las cabeceras de respuesta para ver que está enviando el servidor. La respuesta será entonces “HTTP/2.0” y algo más. Firefox inserta su propia cabecera denominada “X-Firefox-Spdy:” como se aprecia en la captura de pantalla de arriba.

Las cabeceras que se ven en la herramienta de red al utilizar http2 han sido convertidas desde el formato binario de http2, para parecerse al estilo clásico de las cabeceras de HTTP 1.x.

## 9.4. Visualizar el uso de http2

Existen plugins de Firefox disponibles que ayudan a visualizar si un sitio está usando HTTP/2. Uno de ellos es [“SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/http2-indicator/).
