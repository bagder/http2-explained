# 12. Dopo http2

A lot of tough decisions and compromises have been made for http2. With http2 getting deployed there is an established way to upgrade into other protocol versions that work which lays the foundation for doing more protocol revisions ahead. It also brings a notion and an infrastructure that can handle multiple different versions in parallel. Maybe we don't need to phase out the old entirely when we introduce new?

http2 ha a che fare con un sacco di eredità di HTTP 1, per via del fatto che si è voluto poter lasciare aperta la possibilità di proxificare avanti e indietro il traffico HTTP 1 e http2. Parte di questa eredità pone un limite a futuri sviluppi e invenzioni. Magari http3 potrà disfarsi di questa eredità ?

Cosa pensate che possa ancora mancare a http?

## 12.1. QUIC

Il protocollo di Google [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) è un esperimento interessante che si è svolto in linea con lo stesso spirito e stile di SPDY. QUIC è un rimpiazzo per TCP + TLS + HTTP/2 implementato su UDP.

QUIC permette la creazione di connessioni con minor latenza, risolve la perdita di pacchetti bloccando individualmente uno stream piuttosto che tutti insieme come avviene in HTTP/2, olre a rendere possibile un muliplex semplificato su NIC diversi - coprendo quindi aree relative a ciò che MPTCP dovrebbe risolvere.

QUIC è per il momento implementato solo dai server di Google e dal loro browser Chrome; tale codice non è facilmente riutilizzabile altrove, anche se esiste una libreria che cerca di provvedere alla lacuna [libquic](https://github.com/devsisters/libquic). Il protocollo è stato presentato al gruppo di lavoro "transport" dela IETF nella draft [draft](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01).
