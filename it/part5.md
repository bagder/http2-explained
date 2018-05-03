# 5. http2 a grandi linee

<<<<<<< HEAD
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
=======
Dunque, cosa porta http2? A che punto si pongono i limiti imposti dal gruppo HTTbis ?

I confini sono abbastanza limitati e pongono al team molte restrizioni sull'abilità di innovare.

- http2 deve mantenere i paradigmi HTTP. Rimane un protocollo dove il client manda una richiesta al server via TCP.

- gli URL http:// e https:// non possono essere modificati. Non può esistere un nuovo schema. La quantità di contenuti che utilizzano tali URL è troppo grande per pensare ad un cambiamento.

- servers e clients HTTP1 esistono da decadi, dobbiamo essere in grado di "proxyficare" tutto verso servers http2.

- Di conseguenza, un proxy deve essere capace di mappare le features di http2 uno-a-uno su un client HTTP 1.1.

- Rimuovere o ridurre parti opzionali del protocollo. Non era esattamente un requisito ma piuttosto un mantra proveniente dalla filosofia SPDY e dal team Google. Se si fa in modo che tutto sia obbligatorio fin da subito, non ci sarà mai modo di implementare qualcosa oggi senza cadere in una trappola un domani.

- Basta versioni minori. È stato deciso che i client e i server o sono compatibili con http2, oppure no, in maniera assoluta. Se ci sarà bisogno di estendere il protocollo o modificare le cose, http3 prenderà forma. Non ci sono più versioni intermedie in http2.

## 5.1. http2 for existing URI schemes

As mentioned already, the existing URI schemes cannot be modified, so http2 must use the existing ones. Since they are used for HTTP 1.x today, we obviously need a way to upgrade the protocol to http2, or otherwise ask the server to use http2 instead of older protocols.

HTTP 1.1 has a defined way to do this, namely the Upgrade: header, which allows the server to send back a response using the new protocol when getting such a request over the old protocol, at the cost of an additional round-trip.

That round-trip penalty was not something the SPDY team would accept, and since they only implemented SPDY over TLS, they developed a new TLS extension which shortcuts the negotiation significantly. Using this extension, called NPN for Next Protocol Negotiation, the server tells the client which protocols it knows and the client can then use the protocol it prefers.

## 5.2. http2 for https://

A lot of focus of http2 has been to make it behave properly over TLS. SPDY requires TLS and there's been a strong push for making TLS mandatory for http2, but it didn't get consensus so http2 shipped with TLS as optional. However, two prominent implementers have stated clearly that they will only implement http2 over TLS: the Mozilla Firefox lead and the Google Chrome lead, two of today's leading web browsers.

Reasons for choosing TLS-only include respect for user's privacy and early measurements showing that the new protocols have a higher success rate when done with TLS. This is because of the widespread assumption that anything that goes over port 80 is HTTP 1.1, which makes some middle-boxes interfere with or destroy traffic when any other protocols are used on that port.

The subject of mandatory TLS has caused much hand-wringing and agitated
voices in mailing lists and meetings – is it good or is it evil? It is a highly
controversial topic – be aware of this when you throw this question in the face
of an HTTPbis participant!

Similarly, there's been a fierce and long-running debate about whether http2 should dictate a list of ciphers that should be mandatory when using TLS, or if it should perhaps blacklist a set, or if it shouldn't require anything at all from the TLS “layer” but leave that to the TLS working group. The spec ended up specifying that TLS should be at least version 1.2 and there are cipher suite restrictions.

## 5.3. http2 negotiation over TLS

Next Protocol Negotiation (NPN) is the protocol used to negotiate SPDY with TLS servers. As it wasn't a proper standard, it was taken through the IETF and the result was ALPN: Application Layer Protocol Negotiation. ALPN is being promoted for use by http2, while SPDY clients and servers still use NPN.

The fact that NPN existed first and ALPN has taken a while to go through standardization has led to many early http2 clients and http2 servers implementing and using both these extensions when negotiating http2. Also, NPN is what's used for SPDY and many servers offer both SPDY and http2, so supporting both NPN and ALPN on those servers makes perfect sense.

ALPN differs from NPN primarily in who decides what protocol to speak. With ALPN, the client gives the server a list of protocols in its order of preference and the server picks the one it wants, while with NPN the client makes the final choice.

## 5.4. http2 for http://

As previously mentioned, for plain-text HTTP 1.1 the way to negotiate
http2 is by presenting the server with an Upgrade: header. If the server speaks
http2 it responds with a “101 Switching” status and from then on it speaks
http2 on that connection. Of course this upgrade procedure
costs a full network round-trip, but the upside is that it's generally possible to 
keep an http2 connection alive much longer and re-use it more than a typical HTTP1
connection.

While some browsers' spokespersons stated they will not implement this means
of speaking http2, the Internet Explorer team once expressed that they would -
although they have never delivered on that. curl and a few other non-browser
clients support clear-text http2.

Today, no major browser supports http2 without TLS.
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27
