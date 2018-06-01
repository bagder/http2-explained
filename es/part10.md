# 10. http2 en Chromium

El equipo de Chromium ha implementado y soportado http2 en sus canales dev y beta desde hace bastante tiempo. Desde Chrome 40, publicado el 27 de enero de 2015, http2 está activado por defecto para algunos usuarios. El número de estos, comenzó muy pequeño, y ha ido aumentado a lo largo del tiempo.

El soporte para SPDY será cancelado próximamente. En una entrada de blog, el proyecto anunció lo siguiente en [febrero de 2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> “Chrome ha soportado SPDY desde Chrome 6, pero al tener la mayor parte de las ventajas presentes en HTTP/2, es hora de decir adiós. Hemos planificado quitar el soporte para SPDY a comienzos de 2016”

## 10.1. primero, asegurar que está activado

Introducir “chrome://flags/#enable-spdy4" en la barra de direcciones del navegador y hacer click en “activar” (“enable”), si no está previamente activado.

## 10.2. Sólo TLS

Recordar que Chrome sólo implementa http2 sobre TLS. Únicamente se verá http2 en acción con Chrome, al visitar sitios con https:// que ofrezcan soportes http2.

## 10.3. Visualizar el uso de http2
Existen plugins de Chrome disponibles que ayudan a visualizar si un sitio está usando HTTP/2. Uno de ellos es [“SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC
Los experimentos actuales de Chrome con QUIC (ver sección 12.1), diluyen de alguna manera los número de HTTP/2.
