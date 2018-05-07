# 6. Il protocollo http2

Si è detto abbastanza della storia e delle ragioni politiche che ci hanno accompagnati fin qui. Passiamo all'analisi delle specifiche del protocollo: i fondamenti e i concetti di cui http2 si nutre.

## 6.1. Binario

http2 è un protocollo binario.

Lasciamo questo concetto da parte per qualche minuto. Se sei stato coinvolto in protocolli internet prima d'ora, ci sono probabilità che reagirai istintivamente a questa scelta argomentando con fervore quanto un protocollo text/ascii sia più adatto e più versatile, visto che ci permette di instaurare richieste e sessioni via telnet e strumenti simili...

http2 è binario per permettere un incapsulamento più dinamico. Trovare inizio e fine di un frame è uno dei compiti più complessi in HTTP 1.1 e per tutti i protocolli text-based in generale. L'implementazione si rende più semplice allontanandosi da "spazi opzionali" e modi diversi di scrivere la stessa cosa.

Allo stesso tempo rende più semplice separare protocollo da incapsulamento - cosa che in HTTP 1.1 presenta interdipendenza.

Il supporto nativo per compressione assieme all'onnipresente TLS, diminuisciono il valore di un protocollo puramente testuale, dato che non si vedrebbe alcun testo in una capture on-the-wire. Dobbiamo semplicemente abituarci all'idea di usare uno strumento di cattura/analisi tipo Wireshark al fine di comprendere cosa succeda a livello di http2.

Per operare un buon debug su questo protocollo si avrà probabilmente bisogno di strumenti quali curl, analisi di traffico IP con dissector http2 tipo Wireshark e simili.

## 6.2. Il formato binario

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

https spedisce frame binari. Diversi tipi di frame possono essere inviati, pur condividendo la stessa fase di inizializzazione: Length, Type, Flags, Stream Identifier, e payload del frame.

Esistono dieci tipi diversi di frame definiti nella specifica http2. Probabilmente due dei più fondamentali e che più intuitivamente possono essere ricondotti ad HTTP 1.1 sono DATA e HEADERS. Li descriverò in dettaglio successivamente.

## 6.3. Flussi multiplexati

L'ID dello Stream menzionato nella sezione precedente associa ogni frame inviato via http2 ad un preciso "stream". Durante una connessione http2 ogni stream è indipendente, una sequenza bidirezionale di frame viene scambiata fra client e server.

Una sola connessione http2 può contenere molteplici stream -aperti allo stesso tempo- all'interno dei quali ogni endpoint accavalla frame provenienti da streams differenti. Gli streams possono essere aperti ed utilizzati unilateralmente oppure essere condivisi sia dal client sia dal server e possono essere chiusi da entrambi gli endpoint. L'ordine in cui tali frames sono inviati nel contesto dello stream è importante, caratteristico. I destinatari elaborano i frame nell'ordine in cui essi sono ricevuti.

"Multiplexare" gli streams significa che i contenuti di diversi streams sono mescolati nella stessa connessione. Due (o più) flussi di dati sono riuniti in uno singolo, per poi essere nuovamente divisi e riassemblati (individualmente) all'altro capo della connessione. Ecco due flussi (treni di dati, NdR):

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

Ecco due "treni" multiplexati all'interno di una sola connessione:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Priorità e dipendeze

Ogni stream è anche dotato di priorità, o "peso", priorità che viene impiegata per rendere noto all'altro estremo quale sia lo stream da considerare di maggior importanza, ad esempio in caso la penuria di risorse dovesse imporre al server di scegliere quale flusso inviare per primo.

Utilizzando il frame PRIORITY, un client può anche istruire il server su quali altri stream dipendano dallo stesso. Permette al client di definire un sistema di priorità (albero) ove diversi rami figlio possano dipendere dal completamento di altri rami padre (parent streams).

Le priorità e le dipendenze possono essere modificate dinamicamente a runtime, cosa che dovrebbe permettere ai browsers di poter decidere -quando un utente stesse scrollando una pagina piena di immagini- quali di queste immagini siano prioritarie, più importanti, o abbiano lo scopo di accelerare lo scorrimento fra diversi tab (con conseguente miglioramente a livello di reattività del focus).

## 6.5. Compressione degli header

HTTP è un protocollo senza memoria di stato. In brebe, ogni singola richiesta deve portarsi con sè più dettagli possibili, liberando così il server dall'incombenza del dover memorizzare molte informazioni e metadati relativi a richieste precedenti. Dato che http2 non modifica questo paradigma, deve poter continuare a lavorare nello stesso modo.

Ciò fa si che HTTP sembri (e sia) ripetitivo. Quando un client richiede una risorsa dallo stesso server, come i.e. le immagini di una pagina web, un alto numero di richieste verrò generato, e tali richieste sembreranno tutte molto simili fra di loro. Per tale serie sempre ripetitiva, la compressione sarebbe una benedizione.

Come il numero di oggetti per pagina è aumentato, così l'utilizzo dei cookies e la dimensione delle richieste non sono da meno, continuando ad aumentare nel tempo. I cookies devono essere inclusi in ogni richiesta, decine e decine di volte gli stessi elementi sono ritrasmessi.

Le richeste HTTP 1.1 sono al momento diventate così voluminose da eccedere la dimensione della finestra TCP iniziale, cosa che a suo turno rende abbastanza lenta l'operazione, vista la necessita di interi round-trip prima di ricevere ACK, e dunque ben prima che l'intera richiesta sia inviata (e di conseguenza processata). Questo è un altro punto a favore della compressione.

### 6.5.1. Parlare di compressione è un argomento spinoso

HTTPS e SPDY hanno sofferto a causa di vulnerabilità [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) e attacchi [CRIME](https://en.wikipedia.org/wiki/CRIME). Inserendo un testo noto nello stream ed osservandone i mutamenti in funzione dell'operazione richiesta, un attaccante può scoprire il contenuto del payload.

Creare un meccanismo di compressione dinamica per un protocollo - senza renderlo vulnerabile ad uno di questi attachi- richiede molta cura e considerazione. Proprio quello che HTTPbis ha cercato di fare.

Parliamo quindi di [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2, che – come il nome suggerisce - è un formato di compressione creato su misura per gli headers http2, descritto e specificato in una internet draft separata. Il nuovo formato, cosi come le contromisure adottabili (quali un bit che richiede agli intermediari di non comprimere un header specifico ne padding opzionale), dovrebbe rendere più difficile lo sfruttamento della vulnerabilità nel contesto di un attacco.

Nelle parole di Roberto Peon (uno dei creatori di HPACK):

> “HPACK è stato concepito per impedire ad una implementazione non-conforme
> di facilitare la fuga di informazioni, per rendere ecoding e decoding rapidi
> e semplici, per permettere al ricevente di controllare la compressione sulla
> base delle dimensioni del contesto, per permettere ad un proxy di poter
> re-indicizzare (i.e. condivisione dello stato della sessione fra backend e
> frontend proxies) e per facilitare la comparazione di stringhe Huffman-encoded.

## 6.6. Reset - change your mind

Uno degli svantaggi di HTTP 1.1 è che, una volta che un messaggio HTTP è stato
inviato con un determinato Content-Length associato, non è possibile fermarlo.
Certo, puoi spesso disconnetter la sessione TCP (non sempre), ma tale pratica
implica il costo di una nuova negoziazione TCP (handshake).

Una soluzione più intelligente consisterebbe nel poter fermare il messaggio ed inviarne uno nuovo, correto. Ciò potrà essere realizzato con il nuovo frame http2 RST_STREAM che aiuterà a risparmiare banda ed evitare di interrompere la connessione a livello TCP.

## 6.7. Push da parte del server

Questa funzione è nota come “cache push”. In pratica, quando un client richiedesse una risorsa X, il server potrebbe voler proporre anche una risorsa Z e mandarla al client senza avenre ricevuto domanda. Ciò aiuta il client ad inserire la risorsa Z in cache, così da averla a disposizione in caso di bisogno.

Il server push deve essere cocesso esplicitamente per volontà del client. Anche a quel punto, il client può terminare istantaneamente la ricezione di uno stream in ogni momento, tramite il messaggio RST_STREAM in caso non dovesse desiderare una determinata risorza Z.

## 6.8. Controllo di Flusso

Ogni singolo stream http2 possiede e pubblicizza la propria "finestra di flusso" verso la quale l'altro estremo è autorizzato ad inviare dati. Se conoscete il funzionamento di SSH, questo modo di fare è in linea con lo stesso spirito e principio.

Per ogni singolo stream i due estremi devono potersi avvertire l'un l'altro quando ci dovesse essere abbastanza spazio per accogliere dati in entrata; allo stesso tempo un estremo è abilitato a spedire tanti dati quanti siano quelli ammessi dalla finestra di scambio. Solo i frame DATA sono sottoposti a flow-control (controllo di flusso).

