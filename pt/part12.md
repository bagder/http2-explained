# 12. Após o http2

Muitas decisões difíceis e compromissos foram tomados no http2. Com a implantação do http2, existe uma forma estabelecida para atualizar para outras versões de protocolo que garante a base desse funcionamento em futuras revisões do protocolo. Ele também traz a noção e infraestrutura que podem lidar com múltiplas versões diferentes em paralelo. Talvez nós não precisamos eliminar totalmente algo antigo quando lançarmos um novo?

http2 ainda possui muito “legado” que o HTTP 1 trouxe por causa do desejo de manter possível o _proxy_ de ida e volta entre versões HTTP 1 e http2. Alguns desses legados dificultam o avanço no desenvolvimento e evoluções. Talvez o http3 pode eliminar alguns deles?

O que você acha que ainda está faltando no http?

## 12.1. QUIC

O protocolo [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) feito pelo Google é um experimento interessante, realizado no estilo e espírito que eles fizeram com o SPDY. QUIC é um substituto implementado em UDP para TCP + TLS + HTTP/2.

QUIC permite a criação de conexões com muito menos latência, resolve a perda de pacotes bloqueando apenas os fluxos individuais, ao invés de todos eles como faz o HTTP/2, e permite que novas conexões sejam feitas em diferentes interfaces de rede facilmente - também cobrindo áreas que o MPTCP pretende resolver.

QUIC é, por enquanto, implementado somente pelo Google no Chrome e em seus servidores e esse código não é facilmente reutilizado em outro lugar, mesmo que haja um esforço nesse sentido, como a [libquic](https://github.com/devsisters/libquic). O protocolo está em [draft](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) para o grupo de trabalho de transporte IETF.
