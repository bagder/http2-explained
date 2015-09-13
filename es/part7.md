# 7. Extensiones

El protocolo obliga que a un receptor a leer e ignorar todas las tramas desconocidas utilizando tipo de trama desconocida (unknown frame types). Dos partes pueden así negociar el uso de nuevos tipos de trama en una base hop-by-hop, y las tramas no podrán cambiar el estado y no tendrán control de flujo.

Si http2 debía permitir o no extensiones, fue un tema ampliamente debatido durante el tiempo de desarrollo del protocolo con opiniones igualmente balanceadas a favor y en contra. Después del draft-12, el péndulo basculó por última vez, y las extensiones se permitieron de nuevo.

Por lo tanto, las extensiones no son partes del protocolo real por lo que son documentadas al margen de la especificación del núcleo. Llegados a este punto, existen dos tipos de trama que se debatieron para su inclusión en el protocolo, y que probablemente serán las primeras tramas en ser extensiones. Las describiré a continuación por su popularidad y por su estado anterior como tramas “nativas”:

## 7.1. Alternative Services (Servicios Alternativos)

Durante la adopción de http2, existen razones para sospechar que las conexiones TCP serán mucho más largas y se mantendrán vivas mucho más tiempo de lo que han estado las conexiones con HTTP 1.x. Un cliente debe ser capaz de hacer un montón de las cosas que hace con una única conexión hacia cada dominio/sitio, así que una conexión estará abierta por un tiempo bastante elevado.

Esto afectará a como funcionan lo balanceadores HTTP y se generarán situaciones en las cuales un sitio querrá anunciar y sugerir que el cliente se conecte a otro host, tanto por razones de rendimiento, como porque el sitio puede necesitar mantenimiento u otra razón similar.

El servidor enviará entonces la cabecera [Alt-Svc: header](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07) (o la trama ALTSVC en http2) que indicará al cliente el servidor alternativo. Otra ruta al mismo contenido, utilizando otro servicio, dominio y número de puerto.

El cliente intentará conectarse a ese servicio asíncronamente, y utilizará ese servicio alternativo si éste funciona correctamente.

### 7.1.1. TLS oportunista

La cabecera Alt-Svc permite al servidor que proporciona el contenido por http://, informar al cliente que ese mismo contenido esta también disponible a través de una conexión TLS.

Esta es una funcionalidad debatida en cierta manera. Una conexión de ese tipo, realizaría una conexión TLS sin autenticar que no sería advertida como “segura” en ningún sitio, ya que no usaría un candado en la Interfaz de Usuario o avisaría al usuario de cualquier manera que no se trata del viejo HTTP plano, sino de que se trata de TLS oportunista, concepto que en sí mismo, que encuentra firmes detractores.

## 7.2. Blocked (Bloqueado)

Una trama de este tipo está pensada para ser enviada únicamente una vez por un agente http2 cuando aún queda información que enviar, pero el control de flujo lo prohíbe. La idea de esto es que si tu implementación recibe esta trama, entonces sabes que tu implementación ha cometido algún error y/o están recibiendo menos que la velocidad que puedes recibir por esta razón.

Se cita en el draft-12, antes de que esta trama fuera expulsada como extensión: 

> “La trama BLOCKED está incluida en esta revisión para facilitar la experimentación. Si los resultados no son positivos, se eliminará”
