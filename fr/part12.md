# 12. Après http2

Plusieurs décisions et compromis sont intervenus pour concevoir http2. Avec http2 en cours de déploiement, on a un moyen éprouvé de mise à niveau de version de protocole, qui permettra le développement d'autres révisions de protocoles. Cela montre aussi qu'une même infrastructure peut supporter plusieurs versions de protocole en parallèle. Peut-être pouvons-nous garder l'ancien protocole une fois le nouveau mis en place ?

http2 a pas mal de principes hérités de HTTP 1 pour qu'il soit possible de proxifier du trafic entre HTTP 1 et http2. Certains principes freinent le développement et l'innovation. Peut-être http3 pourra-t-il passer outre ces principes ?

Que pensez-vous qu'il manque à http ?

## 12.1. QUIC

Le protocole [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) de Google est une expérimentation très intéressante, menée dans le même esprit que SPDY. QUIC est un substitut à TCP + TLS + HTTP/2 implémenté avec UDP.

QUIC permet la création de connexions avec moins de latence, il résout la perte de paquet en ne bloquant qu'un flux particulier au lieu de tous les flux en HTTP/2 et il permet d'établir des connexions à travers différentes interfaces réseau, et du coup, couvre des problématiques que MPTCP résout.

QUIC est pour l'instant uniquement disponible à travers Chrome et les serveurs Google et le code n'est pas facilement réutilisable, même s'il existe une [libquic](https://github.com/devsisters/libquic) pour ce faire justement. Le protocole a été soumis en tant que [draft](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) à l'IETF transport working group.