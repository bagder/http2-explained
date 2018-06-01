# 12. Después de http2

Se han tomado muchas decisiones duras y compromisos en http2. Con el despliegue de http2 por delante, se ha determinado una forma para actualizar a otras versiones, que sienta las bases para tener nuevas revisiones del protocolo más adelante. Se ha introducido el concepto y la infraestructura necesarias para manejar múltiples versiones en paralelo. ¿Quizás no es necesario descontinuar completamente lo viejo para introducir algo nuevo?

http2 lleva al futuro un montón de “legado” HTTP 1 con la intención de mantener posible tráfico de proxy de ida y vuelta entre HTTP 1 y http2. Parte de ese legado dificulta aún más el desarrollo y la inventiva. ¿Quizás http3 se deshará de parte de este legado?

¿Qué crees que falta todavía en http?

## 12.1. QUIC

El protocolo de Google, [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) es un experimento muy interesante, desarrollado con el mismo estilo y espíritu con el que en su día se hizo SPDY. QUIC es un sustituto para TCP + TLS + SPDY implementado usando UDP.

QUIC permite la creación de conexiones con mucha menos latencia, soluciona la pérdida de paquetes al sólo bloquear flujos individuales en lugar de todos a la vez como hacer HTTP/2, y posibilita la creación de conexiones por distintas interfaces de red fácilmente – así también cubre otras áreas que MPTCP pretende resolver.

QUIC de momento está únicamente implementado por Google en Chrome y en sus servidores, y es código no fácilmente reutilizable en otras partes, aunque [libquic](https://github.com/devsisters/libquic) es un esfuerzo intentado conseguir eso exactamente. La especificación es todavía algo vaga y cambia rápidamente. Ya existe un [borrador](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) en el grupo de trabajo de transporte del IETF.
