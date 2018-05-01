# 8. Un mondo http2

Quindi, come appariranno le cose quando http2 sarà adottato ? Sarà mai usato ?

## 8.1. Come http2 influenzerà l'utente tipo?

http2 non è ancora ne distribuito ne adodatto. Non possiamo ancora dire come le cose evolveranno. Abbiamo visto come SPDY sia stato messo in campo; possiamo solo fare qualche ipotesi basata su quella ed altre esperienze passate, ed sugli esperimenti in corso.

http2 riduce il numero di round-trip ed evita definitivamente l'eterno dilemma del "head-of-line blocking" attraverso l'utilizzo di multiplexing e fast-discarding degli stream non necessari.

Permette un vasto ammontare di streams paralleli, numero ampiamente sufficiente anche per il sito più "sharded" (parallelizzato) del momento.

Utilizzando correttamente le priorità sugli streams il client sarà in grado di scaricare i dati importanti prima dei dati quelli meno utili.
Visto tutto ciò, sono sicuro che beneficieremo di siti più reattivi, che si caricano in tempi più rapidi. Mettiamola così: una miglior esperienza del web.

Quanto celere sarà questo miglioramento, staremo a vedere, non penso si possa ancora dire. Per prima cosa, la tecnologia è ancora in fase di lancio e non si sono ancora visti client/server che implementino a dovere questa tecnologia per sfruttarne tutte le potenzialità.

## 8.2. Come http2 influenzerà lo sviluppo web?

Nel corso degli anni, gli sviluppatori e gli IDE si sono riuniti a formare una immensa cassetta degli attrezzi piena di utensili e trucchi per circuire HTTP 1.1; come ricorderete, ne ho menzionati alcuni all'inizio di questo documento come giustificazioni per passare a http2.

Molti workaround oggi utilizzati da tool e sviluppatori in maniera automagica, avranno probabilmente effetti negativi sulle performance di http2, o perlomeno non aiuteranno a sfuttare i superpoteri di http2. Spriting e inlining non dovrebbero più essere utilizzati in http2. Anche lo sharding a suo modo dovra essere abbandonato in favore di un minor numero di singole connessioni.

Il problema risiede nel fatto che i siti web devono essere mantenuti ed evoluti di continuo, a breve termine; garantire performance adeguate per entrambi gli utilizzatori di HTTP 1.1 e http2 sarà una grande sfida per gli sviluppatori che vorranno evitare di offrire due frontend separati.

Anche solo per questi motivi, sospetto che passerà un pò di tempo prima che tutti possiamo apprezzare il pieno potenziale di http2, dappertutto.

## 8.3. Implementazioni di http2

Provare a documentare le implementazioni specifiche in un documento come questo sarebbe un impresa futile e prona al fallimento, tale documento diventerebbe desueto in breve tempo. Cercherò piuttosto di spiegare la situazione in termini più ampii, oltre ad indirizzare i lettori verso il sito di http2, [list of implementations](https://github.com/http2/http2-spec/wiki/Implementations).

Un grande numero di implementazioni hanno preso vita fin dall'inizio ed altre si sono aggiunte durante il lavoro di definizione di http2. Al momento attuale, sono citate più di 40 implementazioni, la maggior parte delle quali rispetta la versione finale delle specifiche.

### 8.3.1 Navigatori

Firefox è il browser che più di altri ha seguito da vicino i rapidi sviluppi 
delle ultimissime drafts, Twitter è rimasta al passo offrendo i suoi servizi
su http2. Google ha iniziato verso Aprile 2014 ad offrire supporto per http2
su un numero limitato di server di test e a partire da Maggio 2014 ha fornito
supporto per http2 nelle versioni per sviluppatori Chrome. Microsoft da parte 
sua ha dato una dimostrazione del supporto http2 in Internet Explorer. Safari
(su iOS 9 e Mac OS X El Capitan) e Opera hanno entrambi annunciato supporto
per http2 in future versioni.

### 8.3.2 Servers

Esistono gia molte implementazioni di server http2.

Il popolare Nginx offre supporto per http2 gia dalla [1.9.5](https://www.nginx.com/blog/nginx-1-9-5/)
rilasciata il 22 Settembre 2015 (dove rimpiazza il modulo SPDY, impedendo
l'utilizzo contemporaneo di entrambi all'interno della stessa istanza.

Apache's httpd server has a http2 module [mod_http2](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) since 2.4.17 which was released on October 9, 2015.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](https://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/),
[Caddy](https://caddyserver.com/) e
[LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview)
hanno tutti dimostrato di essere capaci di gestire richieste http2.

### 8.3.3 Altri

curl e libcurl supportano http2 non-sicuro (testuale) e la versione TLS
servendosi di una delle svrariate librerie TLS disponibili.

Anche Wireshark supporta http2; strumento professionale per l'analisi
del traffico di rete http2.

## 8.4. Critiche comuni a http2

Durante lo sviluppo di questo protocollo, il dibattito ha preso pieghe diverse, avanti e indietro rispetto a determinate posizioni intellettuali, è ovvio che a detta di alcune persone questo protocollo abbia fatto davvero una brutta fine. Vorrei citare alcune delle critiche più frequenti e rispondere tono su tono:

### 8.4.1. “Il protocollo è progettato o prodotto da Google”

Vi sono anche varianti in cui si afferma che il mondo diventa sempre più incline al controllo globale di Google. Ciò non è assolutamente vero. Il protocollo è stato sviluppato in seno alla IETF nello stesso modo in cui altri protocolli hanno visto luce durante gli ultimi 30 anni. Ciononostante, riconosciamo e prendiamo atto che l'incredibile lavoro di Google su SPDY non solo ha dimostrato come sia possible diffondere un nuovo protocollo in maniera "non-standard" ma ha anche fornito i numeri per valutarne futuri guadagni in prestazioni.

Google ha pubblicamente [annunciato](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) che rimuoverà il supporto per SPDY e NPN su Chrome nel 2016, incoraggiando la migrazione a http2.

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

Si, amiamo poter vedere i protocolli cleartext in azione, rende più facile la diagnostica. Però tali protocolli sono più proni all'errore, oltre ad aprire le porte ad errori di parsing.

Se davvero non sopporti i protocolli binarii, allora non hai mai interagito con TLS e compressione in HTTP 1.x, pratica che peraltro è in atto da lunghissimo tempo.

### 8.4.6. “Non è più veloce di nessun HTTP/1.1”

Questo è ovviamente oggetto di dibattiti e discussioni animate riguardo la definizione di "velocità", benchè svariati test eseguiti al tempo di SPDY abbiano gia mostrato che il tempo di "page load" sia deavvero minore (per esempio ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) dell'Università di Washington e ["Evaluating the Performance of SPDY-enabled Web Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) di Hervé Servy) e chiaramente tali esperimenti sono stati ripetuti su http2.

Aspetto con ansia che altri risultati ed esperimenti simili vengano pubblicati. Un [primo semplice test eseguito da httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) sembra indicare che HTTP/2 stia mantenendo le proprie promesse.

### 8.4.7. “È sviluppato attorno ad una statificazione di violazioni”

Davvero, questa la vostra critica ? Gli strati non sono sacri e intoccabili pilastri di una religione globale. Se ci siamo trovati a passare per zone d'ombra durante lo sviluppo di http2, è sempre stato nell'interesse e allo scopo di creare un protocollo efficace e stabile, a partire dai vinvoli iniziali.

### 8.4.8. “Non rimedia a svariati limiti intrinsechi a HTTP/1.1”

Vero. Con l'obiettivo preciso di mantenere i paradigmi HTTP/1.1 molte delle antiche features dovevano rimanere incluse, tali quali gli header più comuni, che a loro volta spesso includono fantomatici cookies, autorizzazioni e tanto altro. Il vantaggio nel mantenere questi paradigmi è che ora abbiamo un protocollo che distribuibile e integrabile senza bisogno di un eccessivo ammontare di aggiornamenti, evitando di riscrivere le parti fondamentali o di rimpiazzarle ex-novo. http2 è di fatto solo un nuovo strato di incapsulazione, di framing.

## 8.5. Sarà http2 massivamente adottato?

È troppo presto per dirlo con certezza, ma posso indovinare e stimare che è quello che succederà.

I detrattori diranno “guarda quanto bene ha fatto IPv6” portandolo come esempio di un nuovo protocollo per il quale ci sono voluti decenni prima che si iniziasse a vederlo implementato massivamente. Ebbene, http2 non è un nuovo IPv6. È un protocollo basato su TCP che fa leva sullo straodinario meccanismo di Update HTTP, sui numeri di porta, su TLS, etc. Non necessiterà di alcuna modifica a router o firewall.

Con il suo SPDY, Google ha dimostrato al mondo come un nuovo protocollo possa essere sviluppato e distribuito a browser e webservices attraverso implementazioni multiple, in un tempo tutto sommato breve. Mentre l'ammontare di server che offrono SPDY su Internet è nel range dell'1%, l'ammontare di dati con i quali questi server hanno a che fare è ben maggiore. Alcuni dei più popolari e visitati siti propongono SPDY gia ad oggi.

Direi che http2, basato sullo stesso principio e paradigma di SPDY, si diffonderà ancora di più per via del fatto che tale protocollo proviene dalla IETF. Il deploy di SPDY ha sempre sofferto dello stigma, essendo esso “un protocollo di Google”.

Vi sono svariati "grandi browser" dietro la distribuzione di http2. Rappresentati di Firefox, Chrome, Safari, Internet Explorer e Opera hanno espresso la volontà di includere http2 nei proprii broswer; essi hanni dimostrato implementazioni funzionanti.

I più grandi portali e operatori del web -fra cui Google, Twitter e Facebook- sono interessati ad offrire http2 quanto prima. Speriamo di vedere questo supporto arrivare anche alle piattaforme server tipo Apache e nginx. Un nuovo velocissimo server HTTP con supporto http2 che mostra un enorme potenziale è H2o.

Molte delle più famose case produttrici di proxy hanno annunciato di voler supportare http2, fra cui HAProxy, Squid e Varnish.

Durante il corso del 2015, il traffico http2 è aumentato. A inizio Settembre il tasso di utilizzazione di Firefox 40 era al 13% su tutto il traffico HTTP e 27% su tutto il traffico HTTPS, mentre verso Google circa il 18% di richieste entranti sono in HTTP/2. Bisogna notare che allo stesso tempo Google è in procinto di sperimentare altri protocolli (vedi QUIC in 12.1) il che abbassa in qualche modo il tasso di connessioni http2 globali.

