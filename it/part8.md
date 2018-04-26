# 8. Un mondo http2

Quindi, come appariranno le cose quando http2 sarà adottato ? Sarà mai usato ?

## 8.1. Come http2 influenzerà l'utente tipo?

http2 non è ancora ne distribuito ne adodatto. Non possiamo ancora dire come le cose evolveranno. Abbiamo visto come SPDY sia stato messo in campo; possiamo solo fare qualche ipotesi basata su quella ed altre esperienze passate, ed sugli esperimenti in corso.

http2 riduce il numero di round-trip ed evita definitivamente l'eterno dilemma del "blocco a fine linea" attraverso l'utilizzo di multiplexing e fast-discarding degli stream non necessari.

Permette un vasto ammontare di streams paralleli, numero ampiamente sufficiente anche per il sito più "sharded" (parallelizzato) del momento.

Utilizzando correttamente le priorità sugli streams, il client sarà in grado di scaricare i dati importanti prima di quelli meno utili.
Visto tutto ciò, direi che beneficieremo di siti più  reattivi che si caricano in minor tempo. Mettiamola così: una miglior esperienza del web.

Quanto rapido sarà questo miglioramento, vedremo, non penso si possa ancora dire. Per prima cosa, la tecnologia è ancora in una fase di lancio e non si sono ancora visti client/server che implementino a dovere questa tecnologia per sfruttarne tutte le potenzialità.

## 8.2. Come http2 influenzerà lo sviluppo web?

Nel corso degli anni, gli sviluppatori e gli IDE si sono riuniti a formare una immensa cassetta degli attrezzi piena di utensili e trucchi per circuire HTTP 1.1; come ricorderete, ne ho menzionati alcuni all'inizio di questo documento come giustificazioni per passare a http2.

Molti workaround oggi utilizzati da tool e sviluppatori in maniera automagica, avranno probabilmente effetti negativi sulle performance di http2, o perlomeno non aiuteranno a sfuttare i superpoteri di http2. Spriting e inlining non dovrebbero più essere utilizzati in http2. Anche lo sharding a suo modo dovra essere abbandonato in favore di un minor numero di singole connessioni.

Il problema risiede nel fatto che i siti web devono essere mantenuti ed evoluti di continuo, a breve termine; garantire performance adeguate per entrambi gli utilizzatori di HTTP 1.1 e http2 sarà una grande sfida per gli sviluppatori che vorranno evitare di offrire due frontend separati.

Anche solo per questi motivi, sospetto che passerà un pò di tempo prima che tutti possiamo apprezzare il pieno potenziale di http2, dappertutto.

## 8.3. Implementazioni di http2

Provare a documentare le implementazioni specifiche in un documento come questo sarebbe un impresa futile e prona al fallimento, tale documento diventerebbe desueto in breve tempo. Cercherò piuttosto di spiegare la situazione in termini più ampii, oltre ad indirizzare i lettori verso il sito di http2, [list of implementations](https://github.com/http2/http2-spec/wiki/Implementations).

Un grande numero di implementazioni hanno preso vita fin dall'inizio, ed altre si sono aggiunte durante il lavoro sulla definizione di http2. Al momento attuale, pià di 40 implementazioni sono citate, di cui la maggior parte rispetta la versione finale delle specifiche.

### 8.3.1 Navigatori

Firefox è il browser che più di altri ha seguito da vicino i rapidi sviluppi 
delle ultimissime drafts, Twitter è rimasta al passo offrendo i suoi servizi su
http2. Google ha iniziato verso Aprile 2014 ad offrire il supporto per http2 su
un numero limitato di server di test, e a partire da Maggio 2014 ha fornito
supporto per http2 nelle versioni per sviluppatori Chrome. Microsoft da parte 
sua ha dato una dimostrazione del supporto http2 in Internet Explorer. Safari
(su iOS 9 e Mac OS X El Capitan) e Opera hanno entrambi annunciato che daranno
supporto a http2 in future versioni.

### 8.3.2 Servers

Esistono gia molte implementazioni di server http2.

The popular Nginx server offers http2 support with since
[1.9.5](https://www.nginx.com/blog/nginx-1-9-5/) released on September 22,
2015 (where it replaces the SPDY module, so they cannot both run in the same
server instance).

Apache's httpd server has a http2 module [mod_http2](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) since 2.4.17 which was released on October 9, 2015.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](https://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/),
[Caddy](https://caddyserver.com/) and
[LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview)
have all released http2 capable servers.

### 8.3.3 Altri

curl and libcurl support insecure http2 as well as the TLS based one using one
out of several different TLS libraries.

Wireshark supports http2. The perfect tool for analyzing http2 network
traffic.

## 8.4. Critiche comuni a http2

During the development of this protocol the debate has been going back and forth and of course there is a certain amount of people who believe this protocol ended up completely wrong. I wanted to mention a few of the more common complaints and mention the arguments against them:

### 8.4.1. “The protocol is designed or made by Google”

It also has variations implying that the world gets even further dependent or controlled by Google by this. This isn't true. The protocol was developed within the IETF in the same manner that protocols have been developed for over 30 years. However, we all recognize and acknowledge Google's impressive work with SPDY that not only proved that it is possible to deploy a new protocol this way but also provided numbers illustrating what gains could be made.

Google has publicly [announced](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) that they will remove support for SPDY and NPN in Chrome in 2016 and they urge servers to migrate to HTTP/2 instead.

### 8.4.2. “È un protocollo utile soltanto ai browser”

This is sort of true. One of the primary drivers behind the http2 development is the fixing of HTTP pipelining. If your use case originally didn't have any need for pipelining then chances are http2 won't do a lot of good for you. It certainly isn't the only improvement in the protocol but a big one.

As soon as services start realizing the full power and abilities the multiplexed streams over a single connection brings, I suspect we will see more application use of http2.

Small REST APIs and simpler programmatic uses of HTTP 1.x may not find the step to http2 to offer very big benefits. But also, there should be very few downsides with http2 for most users.

### 8.4.3. “Il protocollo è utile solo ai siti più grandi”

Not at all. The multiplexing capabilities will greatly help to improve the experience for high latency connections that smaller sites without wide geographical distributions often offer. Large sites are already very often faster and more distributed with shorter round-trip times to users.

### 8.4.4. “Il fatto che usi TLS lo rende ancora più lento”

This can be true to some extent. The TLS handshake does add a little extra,
but there are existing and ongoing efforts on reducing the necessary
round-trips even more for TLS. The overhead for doing TLS over the wire
instead of plain-text is not insignificant and clearly notable so more CPU and
power will be spent on the same traffic pattern as a non-secure protocol. How
much and what impact it will have is a subject of opinions and
measurements. See for example [istlsfastyet.com](https://istlsfastyet.com/)
for one source of info.

Telecom and other network operators, for example in the ATIS Open Web
Alliance, say that they [need unencrypted
traffic](https://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf)
to offer caching, compression and other techniques necessary to provide a fast
web experience over satellite, in airplanes and similar.  http2 does not make
TLS use mandatory so we shouldn't conflate the terms.

Many Internet users have expressed a preference for TLS to be used more widely and we should help to protect users' privacy.

Experiments have also shown that by using TLS, there is a higher degree of success than when implementing new plain-text protocols over port 80 as there are just too many middle boxes out in the world that interfere with what they would think is HTTP 1.1 if it goes over port 80 and might look like HTTP at times.

Finally, thanks to http2's multiplexed streams over a single connection, normal browser use cases still could end up doing substantially fewer TLS handshakes and thus perform faster than HTTPS would when still using HTTP 1.1.

### 8.4.5. “Il fatto che non sia ASCII è un grosso svantaggio”

Yes, we like being able to see protocols in the clear since it makes debugging and tracing easier. But text based protocols are also more error prone and open up for much more parsing and parsing problems.

If you really can't take a binary protocol, then you couldn't handle TLS and compression in HTTP 1.x either and its been there and used for a very long time.

### 8.4.6. “Non è più veloci di alcun HTTP/1.1”

This is of course subject to debate and discussions on how to measure what faster means, but already in the SPDY days many tests were performed that proved browser page loads were faster (like ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) by people at University of Washington and ["Evaluating the Performance of SPDY-enabled Web Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) by Hervé Servy) and such experiments have been repeated with http2 as well. I'm looking forward to seeing more such tests and experiments getting published. A [basic first test made by httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) might imply that HTTP/2 holds its promises.

### 8.4.7. “È sviluppato attorno una statificazione di violazioni”

Davvero, questa la vostra critica ? Gli strati non sono sacri e intoccabili pilastri di una religione globale. Se ci siamo trovati a passare per zone d'ombra durante lo sviluppo di http2, è sempre stato nell'interesse e allo scopo di creare un protocollo efficace e stabile, a partire dai vinvoli iniziali.

### 8.4.8. “Non rimedia a svariati limiti intrinsechi a HTTP/1.1”

Vero. Con l'obiettivo preciso di mantenere i paradigmi HTTP/1.1 molte delle antiche features dovevano rimanere inclusi, tali quali gli header più comuni i quali spesso includono fantomatici cookies, autorizzazioni e tanto altro. Il vantaggio nel mantenere questi paradigmi è che abbiamo ora un protocollo che è possibile distribuire e integrare senza bisogno di un eccessivo ammontare di lavoro di aggiornamento, nessun bisogno di riscrivere le parti fondamentali o di rimpiazzarle ex-novo. http2 è di fatto solo un nuovo strato di incapsulazione, di framing.

## 8.5. Sarà http2 massivamente adottato?

È troppo presto per dirlo con certezza, ma posso indovinare e stimare che è quello che succederà.

I detrattori diranno “guarda quanto bene ha fatto IPv6” portandolo come esempio di un nuovo protocollo per il quale ci sono voluti decenni prima che si iniziasse a vederlo implementato massivamente. Ebbene, http2 non è un nuovo IPv6. È un protocollo basato su TCP che fa leva sullo straodinario meccanismo di Update HTTP, sui numeri di porta, su TLS, etc. Non necessiterà di alcuna modifica a router o firewall.

Con il suo SPDY, Google ha dimostrato al mondo come un nuovo protocollo possa essere sviluppato e distribuito a browser e webservices attraverso implementazioni multiple, in un tempo tutto sommato breve. Mentre l'ammontare di server che offrono SPDY su Internet è nel range dell'1%, l'ammontare di dati con i quali questi server hanno a che fare è ben maggiore. Alcuni dei più popolari e visitati siti propongono SPDY gia ad oggi.

Direi che http2, basato sullo stesso principio e paradigma di SPDY, si diffonderà ancora di più per via del fatto che tale protocollo proviene dalla IETF. Il deploy di SPDY ha sempre sofferto dello stigma, essendo esso “un protocollo di Google”.

Vi sono svariati "grandi browser" dietro la distribuzione di http2. Rappresentati di Firefox, Chrome, Safari, Internet Explorer e Opera hanno espresso la volontà di includere http2 nei proprii broswer; essi hanni dimostrato implementazioni funzionanti.

I più grandi portali e operatori del web -fra cui Google, Twitter e Facebook- sono interessati ad offrire http2 quanto prima. Speriamo di vedere questo supporto arrivare anche alle piattaforme server tipo Apache e nginx. Un nuovo velocissimo server HTTP con supporto http2 che mostra un enorme potenziale è H2o.

Molte delle più famose case produttrici di proxy hanno annunciato di voler supportare http2, fra cui HAProxy, Squid e Varnish.

Durante i corso del 2015, il traffico http2 è aumentato. A inizio Settembre, il tasso di utilizzazione di Firefox 40 era al 13% su tutto il traffico HTTP e 27% su tutto il traffico HTTPS, mentre verso Google circa il 18% di richieste entranti sono in HTTP/2. Bisogna notare che allo stesso tempo Google è in procinto di sperimentare altri protocolli (vedi QUIC in 12.1) il che abbassa in qualche modo il tasso di connessioni http2 globali.

