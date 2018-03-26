# 4. Actualizando HTTP

¿No estaría bien hacer un protocolo mejorado? Algo incluyendo...

1. Hacer que el protocolo sea menos sensible a RTT.
2. Arreglar el pipelinig y el problema del primero de la cola.
3. Parar la necesidad y el deseo de seguir aumentando el numero de conexiones para cada host.
4. Mantener todas las interfaces existentes, todo el contenido todos los formatos de URI y sus esquemas.
5. Esto habría de hacerse con el grupo de trabajo del IETF, HTTPbis.

## 4.1. IETF y el HTTPbis working group

El Internet Engineering Task Force (IETF) es una organización que desarrolla y promueve estándares en Internet. La mayoría a nivel de protocolo. Son ampliamente conocidos por las series de documentos RFC que vienen a documentar todo, desde TCP, DNS, buenas prácticas FTP, HTTP y numerosas variaciones de protocolos que nunca han llegado a ningún sitio.

Los grupos de trabajo dentro IETF son formados con un ámbito limitado a conseguir un objetivo concreto. Establecen un “capítulo” (chapter) con un conjunto de guías y limitaciones de lo que deberían ser. Todo el mundo y cualquiera puede unirse al debate y al desarrollo. Cualquier que se une y dice algo, tienen el mismo peso y la misma oportunidad de afectar al resultado y todo el mundo es una persona humana e individual, sin importar demasiado en que empresa trabaja el individuo en concreto.

El grupo de trabajo HTTPbis (más tarde se explica este nombre) fue formado durante el verano de 2007 y se le encomendó la tarea de crear una actualización para la especificación HTTP 1.1. Aunque el debajo dentro del grupo comenzó realmente a finales de 2012. El trabajo de de actualización de HTTP 1.1 fue terminado a comienzos de 2014 y resultó en la serie [RFC 7320](https://tools.ietf.org/html/rfc7320).

La reunión operativa supuestamente final del grupo de trabajo HTTPbis tuvo lugar a comienzos de 2014 en Nueva York. Las debates restantes y los procedimientos del IETF necesarios para publicar el RFC se sucedieron durante el año siguiente.

Algunos de los agentes más grandes en HTTP no estuvieron en los debates o reuniones del grupo de trabajo. No pretendo citar ninguna empresa o producto en particular en este documento, pero claramente algunos actores del Internet de hoy, parecen confiar plenamente en que el IETF hará un buen trabajo sin tener en cuenta a estas empresa.

### 4.1.1. La parte “bis” del nombre

El grupo se ha denominado HTTPbis de manera que la parte “bis” procede del [adverbio en latín para “dos”](https://en.wiktionary.org/wiki/bis#Latin). Bis es muy usado por el IETF bien como un sufijo o bien como parte de un nombre para una actualización o para la segunda parte de una especificación. En este caso para HTTP 1.1.

## 4.2. http2 empieza en SPDY

[SPDY](https://en.wikipedia.org/wiki/SPDY) es un protocolo encabezado y desarrollado por Google. Ha sido indudablemente desarrollado en abierto y todo el mundo ha sido invitado a participar, pero obviamente son los principales beneficiados al controlar una implementación muy popular de navegador y una parte significativa de la población de servidores con bastantes servicios muy conocidos.

Cuanto el grupo HTTPbis decidió que era hora de comenzar a trabajar en http2, SPDY había demostrado que era un concepto que funcionaba. Había demostrado que era posible desplegar en Internet y habían publicado números demostrando su rendimiento. El trabajo para http2 comenzó posteriormente desde el draft SPDY/3 que se convirtió en el draft-00 de http2 básicamente con un poco de búsqueda y reemplazo.
