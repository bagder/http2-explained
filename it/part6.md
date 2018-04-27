# 6. Il protocollo http2

Si è detto abbastanza della storia e delle ragioni politiche che ci hanno accompagnati fin qui. Passiamo ad una analisi delle specifiche del protocollo: i fondamenti e i concetti di cui http2 si nutre.

## 6.1. Binario

http2 è un protocollo binario.

Lasciamo questo concetto da parte per qualche minuto. Se sei stato coinvolto in protocolli internet prima d'ora, ci sono probabilità che reagirai istintivamente a questa scelta, argomentando con fervore quanto un protocollo text/ascii sia migliore (più adatto, più versatile) visto che possiamo instaurare richieste e sessioni via telnet e strumenti simili...

http2 è binario per permettere un incapsulamento più dinamico. Trovare inizio e fine di un frame è uno dei compiti più complessi in HTTP 1.1, e per tutti i protocolli text-based in generale. L'implementazione si rende più semplice allontanandosi da "spazi opzionali" e modi diversi di scrivere la stessa cosa.

Allo stesso tempo, rende più semplice separare protocollo e incapsulamento - cosa che in HTTP 1.1 è completamente interdipendente.

Il supporto nativo per compression, assime all'omnipresente TLS, diminuisce il valore di un protocollo puramente testuale, dato che non vedresti alcun testo in una capture on-the-wire. Dobbiamo semplicemente abituarci all'idea di usare uno strumento di cattura tipo Wireshark, al fine di comprendere cosa succeda a livello di http2.

Per operare un buon debug su questo protocollo si avrà probabilmente bisogno di strumenti quali curl, analisi di traffico IP con dissector http2 tipo Wireshark e simili.

## 6.2. Il formato binario

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

https spedisce frame binari. Diversi tipi di frame possono essere spediti, e condividono la stessa fase di inizializzazione: Length, Type, Flags, Stream Identifier, e payloaddel frame.

Esistono dieci tipi diversi di frame, definiti dalla specifica http2. Probabilmente due dei più fondamentali che possono essere ricondotti facilmente a HTTP 1.1 sono DATA e HEADERS. Li descriverò in dettaglio successivamente.

## 6.3. Flussi multiplexati

L'ID dello Stream menzionato nella sezione precedente associa ogni frame inviato via http2 ad uno "stream". Durante una connessione http2, ogni stream è indipendente, dunque una sequenza bidirezionale di frame viene scambiata fra client e server.

Una sola connessione http2 può contenere molteplici stream -aperti allo stesso tempo- all'interno dei quali ogni endpoint accavalla frame provenienti da streams differenti. Gli streams possono essere aperti ed utilizzati unilateralmente oppure essere condivisi sia dal sia dal server, e possono essere chiusi da entrambi gli endpoint. L'ordine in cui i frames sono inviati nel contesto dello stream è importante, caratteristico. I destinatari elaborano i frame nell'ordine in cui essi sono ricevuti.

"Multiplexare" gli streams significa che i contenuti di diversi streams sono mescolati nella stessa connessione. Due (o più) flussi di dati sono riuniti in un singolo, per poi essere nuovamente divisi e riassemblati (individualmente) all'altro estremo della connessione. Ecco due flussi (treni di dati, NdR):

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

Ecco due "treni" multiplexati all'interno di una sola connessione:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Priorities and dependencies

Each stream also has a priority (also known as “weight”), which is used to tell the peer which streams to consider most important, in case there are resource restraints that force the server to select which streams to send first.

Using the PRIORITY frame, a client can also tell the server which other stream this stream depends on. It allows a client to build a priority “tree” where several “child streams” may depend on the completion of “parent streams”.

The priority weights and dependencies can be changed dynamically at run-time, which should enable browsers to make sure that when users scroll down a page full of images, the browser can specify which images are most important, or if you switch tabs it can prioritize a new set of streams that suddenly come into focus.

## 6.5. Header compression

HTTP is a stateless protocol. In short, this means that every request needs to bring with it as much detail as the server needs to serve that request, without the server having to store a lot of info and meta-data from previous requests. Since http2 doesn't change this paradigm, it has to work the same way.

This makes HTTP repetitive. When a client asks for many resources from the same server, like images from a web page, there will be a large series of requests that all look almost identical. A series of almost identical somethings begs for compression.

While the number of objects per web page has increased (as mentioned earlier), the use of cookies and the size of the requests have also kept growing over time. Cookies also need to be included in all requests, often the same ones in multiple requests.

The HTTP 1.1 request sizes have actually gotten so large that they sometimes end up larger than the initial TCP window, which makes them very slow to send as they need a full round-trip to get an ACK back from the server before the full request has been sent. This is another argument for compression.

### 6.5.1. Compression is a tricky subject

HTTPS and SPDY compression were found to be vulnerable to the [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) and [CRIME](https://en.wikipedia.org/wiki/CRIME) attacks. By inserting known text into the stream and figuring out how that changes the output, an attacker can figure out what's being sent in an encrypted payload.

Doing compression on dynamic content for a protocol - without becoming vulnerable to one of these attacks - requires some thought and careful consideration. This is what the HTTPbis team tried to do.

Enter [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2, which – as the name suggests - is a compression format especially crafted for http2 headers, and it is being specified in a separate internet draft. The new format, together with other counter-measures (such as a bit that asks intermediaries to not compress a specific header and optional padding of frames), should make it harder to exploit compression.

In the words of Roberto Peon (one of the creators of HPACK):

> “HPACK was designed to make it difficult for a conforming implementation to
> leak information, to make encoding and decoding very fast/cheap, to provide
> for receiver control over compression context size, to allow for proxy
> re-indexing (i.e., shared state between frontend and backend within a proxy),
> and for quick comparisons of Huffman-encoded strings”.

## 6.6. Reset - change your mind

One of the drawbacks with HTTP 1.1 is that when an HTTP message has been sent
off with a Content-Length of a certain size, you can't easily just stop
it. Sure, you can often (but not always) disconnect the TCP connection, but that
comes at the cost of having to negotiate a new TCP handshake again.

A better solution would be to just stop the message and start anew. This can be done with http2's RST_STREAM frame which will help prevent wasted bandwidth and the need to tear down connections.

## 6.7. Server push

This is the feature also known as “cache push”. The idea is that if the client asks for resource X, the server may know that the client will probably want resource Z as well, and sends it to the client without being asked. It helps the client by putting Z into its cache so that it will be there when it wants it.

Server push is something a client must explicitly allow the server to do. Even then, the client can swiftly terminate a pushed stream at any time with RST_STREAM should it not want a particular resource.

## 6.8. Controllo di Flusso

Ogni singolo stream http2 possiede e pubblicizza la propria "finestra di flusso", verso la quale l'altro estremo è autorizzato ad inviare dati. Se conoscete il funzionamento di SSH, questo è in linea con lo stesso spirito e principio.

Per ogni singolo stream, i due estremi devono avvertirsi l'un l'altro quando ci dovesse essere abbastanza spazio per accogliere dati in entrata; allo stesso tempo un è abilitato a spedire tanti dati quanti sono ammessi dalla finestra di scambio. Solo i frame DATA sono sottoposti a flow-control (controllo di flusso).

