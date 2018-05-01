# 5. http2 a grandi linee

Dunque, che innovazione porta http2? Fino a che punto si spingono i limiti imposti dal gruppo HTTbis ?

I confini sono abbastanza limitati e pongono al team molte restrizioni sull'abilità di innovare.

- http2 deve mantenere i paradigmi HTTP. Rimane un protocollo in cui un client manda una richiesta ad un server via TCP.

- gli URL http:// e https:// non possono essere modificati. Non può esistere un nuovo schema. La quantità di contenuti che utilizzano tali URL è troppo grande per pensare ad un cambiamento.

- servers e clients HTTP1 esistono da decadi, dobbiamo essere in grado di "proxyficare" (traghettare, incapsulare) tutto verso servers http2.

- di conseguenza un proxy deve essere capace di mappare le features di http2 uno-a-uno su un client HTTP 1.1.

- Rimuovere o ridurre parti opzionali del protocollo. Non esattamente un requisito ma piuttosto un mantra proveniente dalla filosofia SPDY e dal team Google. Se si fa in modo che tutto sia obbligatorio fin da subito non si arriverà mai ad "implementare oggi" senza cadere in un possibile bug domani.

- Basta con le versioni minori. È stato deciso che i client ed i server o sono compatibili con http2 oppure no, in maniera assoluta. Se ci sarà bisogno di estendere il protocollo o modificare le cose, http3 prenderà forma. Non ci sono più versioni intermedie in http2.

## 5.1. http2 per gli schemi URI gia esistenti

Come gia detto, gli schemi URI esistenti non possono essere modificati, dunque http2 deve utilizzare gli esistenti. Visto che sono utilizzati per HTTP 1.x ad oggi, abbiamo necessariamente bisogno di una maniera per upgradare il protocollo ad http2, o altrimenti domandare al server di utilizzare http2 piuttosto che protocolli precedenti.

HTTP 1.1 possiede gia un modo di procedere a tale upgrade; l'omonimo header "Upgrade:" permette al server di inviare una risposta utilizzando il nuovo protocollo ogni volta che ricevesse una richiesta sul vecchio, al costo di un solo round-trip aggiuntivo.

La penalità imposta da tale round-trip non era una condizione che il team SPDY avrebbe potuto accettare, e visto che hanno implementato SPDY solo su TLS, hanno sviluppato una nuova estensione TLS che raccorcia la nettamente la negoziazione. Utilizzando questa estensione detta NPN (Next Protocol Negotiation) il server istruisce il client a proposito di quali protocolli siano di sua conoscenza e lascia al client la libera scelta fra quelli supportati.

## 5.2. http2 per https://

Molta attenzione è stata messa per far si che http2 si comporti degnamente su TLS. SPDY necessita di TLS, di conseguenza si è attuata una discreta pressione per rendere TLS obbligatorio per http2, ma infine tale mozione non ha ricevuto il consenso dunque per http2, TLS rimane opzionale. Ciononostante, due dei partecipanti hanno chiaramente espresso la volontà di implementare http2 esclusivamente su TLS: i capi di Mozilla Firefox e Google Chrome, due dei principali browser esistenti.

Le ragioni per propendere per il TLS-esclusivo includono sicuramente il rispetto della privacy dell'utente e le misurazioni iniziali, che hanno mostrato come http2 abbia un tasso di successo superiore quando in presenza di TLS. Ciò a causa del fatto che molte box intermedie (router, firewall etc) sono convinte che tutto ciò che passa per la porta 80 sia sempre e solo traffico HTTP 1.1, il che crea interferenze e drop in caso si cerchi di utilizzare un altro protocollo.

Tale argomento (TLS obbligatorio) ha causato svariati disaccordi sulle mailing
lists e durante gli incontri - è buono o cattivo ? Rimane un giudizio
controverso - attenzione a non tirare fuori questo argomento davanti ad un
membro della HTTPbis !

Un altro estenuante dibattito ha avuto luogo, riguardo alla decisione se http2 debba o meno fornire una lista dei cifrari che dovrebbero obbligatoriamente essere impiegati durante un dialogo TLS, o se almeno dovesse fornire una blacklist, o se invece non dovesse richiede assolutamente nulla a livello di TLS, lasciando tale decisione al gruppo di lavoro TLS. Risultato del dibattito, la specifica detta una versione minima di TLS 1.2 e menziona restrizioni rispetto agli algoritmi di cifratura.

## 5.3. negoziazione di http2 su TLS

NPN (Next Protocol Negotiation) è il protocollo impiegato per negoziare SPDY quando si ha a che fare con server TLS. Visto che non era un vero e proprio standard, è stato incorporato per mano della IETF fino ad arrivare a diventare ALPN: Application Layer Protocol Negotiation. L'uso di ALPN è incoraggiato (richiesto) da http2, mentre SPDY rimane basato su NPN.

Il fatto che NPN esistesse gia da prima e che poi ALPN abbia preso forma tramite standardizzazione, ha fatto sì che diversi clients e servers abbiano implementato (e pretendano di utilizzare) entrambe le estensioni in presenza di http2. Inoltre come se non bastasse, NPN è utilizzato da SPDY, molti server supportano sia http2 sia SPDY: è dunque logico che tali server supportino entrambi, NPN e ALPN.

ALPN è diverso da NPN principalmente rispetto a chi prenda la decisione sul protocollo da adottare durante un dialogo. Con ALPN, il client fornisce al server una lista di protocolli che è in grado di supportare, in ordine di preferenza; il server si occupa poi di scegliere quelle che preferisce, mentre in NPN è il client che prende tale decisione finale.

## 5.4. http2 per http://

Come detto prima, per negoziare http2 sul vecchio HTTP/1.1, dobbiamo inviare il
famoso header "Upgrade:". Se il server parla http2, risponderà con un messaggio
di stato "101 Switching" e da quel momento in poi tenterà di parlare solo http2
nel contesto di quella connessione. Di certo, questa procedura di upgrade ci 
costa un intero round-trip ma il vantaggio è che poi è possibile mantenere e 
riutilizzare una connessione http2 per molto più tempo rispetto ad una tipica
HTTP/1.1 vecchio stile.

Mentre un portavoce di una casa produttrice di browser ha annunciato di non
voler implementare questa maniera di parlare http2, il team di Internet Explorer
ha affermato di volerlo fare, benchè poi non abbiano mai mantenuto la promessa.
Solo curl e pochi altri non-browsers supportano http2 in clear-text.

Ad oggi, nessuno dei maggiori browser supporta http2 al di fuori di TLS.
