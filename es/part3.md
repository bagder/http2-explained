# 3. Estrategias para evitar los dolores de latencia

Como siempre que se enfrentan distintos problemas, la gente consigue reunir distintas técnicas para solventarlos.
Algunas técnicas son inteligentes y muy útiles, otras son trampas horribles.

## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Spriting es el término que describe la técnica de unir varias imágenes pequeñas, en una única imagen más grande. Más tarde a través de CSS o Javascript, se recortan ciertos pedazos de la imagen grande para ir mostrando las imágenes más pequeñas.

Un sitio web usaría este truco por velocidad. En HTTP 1.1, descargar una única imagen grande es mucho más rápido que descargarse individualmente 100 pequeñas.

Por supuesto que tiene ciertas desventajas en sitios web donde sólo se quieren mostrar uno o dos imágenes pequeñas. De la misma manera, todas las imágenes serán descartadas de la cache al mismo tiempo, en lugar de  las imágenes que sean más utilizadas.

## 3.2 Inlining

Inlining es otro truco para evitar enviar imágenes individuales, cosa que se consigue utilizado URLS “data:” desde un fichero CSS. Tiene beneficios e inconvenientes similares al caso de “spriting”.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Concatenación

Un gran sitio web puede tener un montón de ficheros javascript diferentes. Ciertas herramientas de front-end ayudan a los desarrollares a juntar todos ellos en un único paquete gigante de manera que el navegador deberá descargar un único fichero en lugar de docenas de ficheros más pequeños. Se envía mucha información cuando se necesita poca. Un único cambio, fuerza el refresco de mucha información.

La mayoría de las veces, esta técnica es un inconveniente para los desarrolladores implicados.

## 3.4 Sharding

El truco de rendimiento final que mencionaré es denominado a menudo fragmentación (“sharding”). Básicamente se basa en servir distintos elementos de tu servicios desde el mayor número de servidores posible. En un primer vistazo puede parecer extraño, pero hay algo razonable detrás de todo ello.

Inicialmente la especificación HTTP 1.1 fijaba que un cliente tenía permitido utilizar un máximo de dos conexiones TCP para cada dominio. Para conseguir no saltarse la especificación, ciertos sitios astutamente se intentaron nuevos nombre de dominio y  – voilá – ya disponían de más conexiones de manera que descendían los tiempos de carga.

Con el paso del tiempo, se ha eliminado dicha limitación y los clientes actuales utilizan fácilmente 6-8 conexiones por nombre de dominio, contando con esta limitación, algunos sitios siguen utilizando esta técnica para aumentar el número de conexiones. A medida que el número de objetos ha ido aumentando – como he mostrado anteriormente – utilizar un alto número de conexiones asegura que HTTP rinde bien y hace que tu sitio sea rápido. No es inusual que un sitio web utilice más de 50 o incluso se llegue casi a 100 conexiones en un sitio web utilizando esta técnica. Estadísticas recientes de httparchive.org muestran que las primeras 300.000 URLs del mundo, necesitan una media de 40(!) conexiones TCP para mostrar una página, y la tendencia parece ir incrementando lentamente en el tiempo.

Otra razón es poner las imágenes y otros recursos similares en otro nombre de dominio que no utilice ninguna cookie, ya que el tamaño de las cookies es bastante significativo. Utilizar imágenes sin cookies puede mejorar el rendimiento simplemente consiguiendo ¡peticiones HTTP mucho más pequeñas!

La imagen abajo muestra una traza para una petición de uno de los sitios principales de Suecia, de manera que las peticiones se distribuyen en diferentes nombres de dominio.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
