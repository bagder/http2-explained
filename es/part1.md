# 1. Antecedentes

Este es un documento que describe http2 desde un nivel técnico y de protocolo. Comenzó como una presentación, que hice en Estocolmo en abril de 2014, para más tarde extender y convertirse en un documento completo con todo detalle y explicaciones concisas.

RFC 7540 es el nombre oficial de la especificación final de http2 que ha sido publicada el 15 de Mayo de 2015: https://www.rfc-editor.org/rfc/rfc7540.txt

Todos los errores encontrados en este documento son míos propios (y del traducción), resultado de mis propios defectos. Por favor, reportarlos y haré las actualizaciones con sus correcciones.

He intentado utilizar consecuentemente la palabra “http2” para describir el nuevo protocolo, aunque en términos puramente técnicos, el nombre correcto es HTTP/2. He escogido esta opción para favorecer la legibilidad y conseguir un lenguaje más fluido.

Esta es la traducción al español de la versión 1.13 del documento publicada el 12 de septiembre de 2015.

## 1.1 Autor

Mi nombre es Daniel Stenberg y trabajo en Mozilla. Llevo trabajando con open source y networking durante más de veinte años en numerosos proyectos. Posiblemente se me conozca por ser el desarrollador principal de curl y libcurl. He formado parte del grupo de trabajo HTTPbis durante mucho años, y allí he estado al tanto de las actualizaciones de HTTP 1.1 y me he involucrado en el trabajo de estandarización de http2.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 ¡Ayuda!

Si encuentras errores, omisiones o mentiras descaradas en este documento, por favor envíame un versión actualizada del párrafo afectado y haré versiones modificadas. ¡Se mencionará en los créditos a todo aquel que eche una mano!. Espero ir mejorando este documento a lo largo del tiempo.

El documento está disponible en [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)


## 1.3 Licencia

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Este documento está licenciado bajo Creative Commons Attribution 4.0 license: https://creativecommons.org/licenses/by/4.0/

## 1.4 Historial del documento

La primera versión de este documento fue publicada el 25 de abril de 2014. A continuación se muestran las versiones más recientes de este documento:

### Versión 1.13

- Convertida la versión maestra a sintaxis Markdown
- 13: Mención a más recursos. Actualización de links y descripciones
- 12: Actualización de la descripción de QUIC y referencia a su draft
- 8.5: Actualizado con números actuales
- 3.4: La media es ahora de 40 conexiones TCP 
- 6.4: Actualizada para reflejar lo que dice la especificación 

### Versión 1.12

- 1.1: HTTP/2 es ahora un RFC oficial
- 6.5.1: enlace al RFC de HPACK
- 9.1: Mención al parámetro de configuración de Firefox 36+ para http2
- 12.1: Añadida sección sobre QUIC

### Versión 1.11

- Montón de mejoras en el lenguaje, apuntadas mayormente por contribuciones amigas.
- 8.3.1: mención a actividades específicas de nginx y Apache httpd

### Versión 1.10

- 1: El protocolo ha sido “okayed”
- 4.1: Actualizada la palabra, ya que 2014 es el año pasado.
- portada: añadida imagen y nombrado “http2 explicado”, enlace arreglado
- 1.4: añadido el historial del documento
- Corregidos muchos errores de deletreo y gramática
- 14: añadido agradecimiento a reportes de bugs
- 2.4: (mejora) etiquetas para el gráfico de crecimiento HTTP
- 6.3: corregido el orden de los vagones en el tren multiplexado
- 6.5.1: HPACK draft-12

### Versión 1.9

- Actualización a HTTP/2 draft-17 y HPACK draft-11 
- Añadida la sección "10. http2 en Chromium" (== ahora, una página más larga) 
- Montón de correcciones de deletreo
- Ahora en 30 implementaciones
- 8.5: añadidos algunos números de uso actuales
- 8.3: mención también a internet explorer
- 8.3.1 añadido "implementaciones pendientes"
- 8.4.3: mencionar que TLS también eleva el índice de éxito
