# 10. http2 in Chromium

Il team di Chromium ha implementato http2 e ha anche fornito supporto sui canali dev e beta per lungo tempo. A partire da Chrome 40, rilasciato il 27 Gennaio 2015, http2 è abilitato per default per un determinato numero di utenti. Hanno iniziato supportando una utenza ristretta per poi aumentare gradualmente nel tempo.

Il supporto nativo per SPDY verrà eventualmente eliminato. Il progetto ha comunicato tale notizia in un post [February 2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> “Chrome ha supportato SPDY a partire dalla versione 6, ma dato che la maggior parte dei benefici sono presenti anche in HTTP/2, è tempo di dirsi addio. Pianifichiamo di rimuovere il supporto per SPDY ad inizio 2016”

## 10.1. Per prima cosa, accertarsi di averlo abilitato

Digitare “chrome://flags/#enable-spdy4" nella barra indirizzi e cliccare su “enable” se non fosse gia abilitato.

## 10.2. Solo TLS

Ricordate che Chrome implementa http2 solo attraverso TLS. Vedrete http2 in azione solamente quando Chrome verrà utilizzato su un sito https:// che offra supporto nativo http2.

## 10.3. Visualizzare l'impiego di HTTP/2

Sono disponibili plugin per Chrome che aiutano a visualizzare se un sito stia utilizzando HTTP/2. Uno di questi è [“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Gli attuali esperimenti di Chrome e l'impiego di QUIC (vedi sezione 12.1) fanno sì che il numero di connessioni HTTP/2 sia in diminuzione.
