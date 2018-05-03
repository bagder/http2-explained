# 7. Estensioni

<<<<<<< HEAD
Il protocollo http2 impone che il ricevente sia obbligato a leggere e ignorare tutti i frame sconosciuti (riportanti un tipo di frame sconosciuto). Due estremi possono negoziare l'uso di nuovi tipi di frame su base hop-by-hop ma tali frame non hanno il permesso di cambiare stato, e il loro flusso non beneficerà di flow-control.

Il fatto che http2 possa supportare una estensione è stato dibattuto a lungo durante lo sviluppo del protocollo con opinioni variabili, pro e contro. Dopo la draft-12 il pendolo ha ocillato un ultima volta in favore delle estensioni.

Le estensioni non fanno parte integrante ma sono e saranno documentate all'esterno del documento che specifica i fondamenti di protocollo. Ci sono gia due nuovi tipi di frame per i quali viene discussa l'inclusione ufficiale, i primi frame ad essere spediti come estensioni. Li descriverò vista la loro popolarità e il loro precedente stato di frame "nativi":

## 7.1. Servizi Alternativi

Con l'adozione di http2 abbiamo ragione di sospettare che le connessioni TCP siano ben più lunghe, durevoli, e che saranno mantenute ben più a lungo rispetto alla durata media delle anziane connessioni HTTP 1.x. Un client dovrebbe essere in grado di fare praticamente tutto all'interno di una sola connessione per ogni host/sito; tale connessione potrebbe potenzialmente permanere aperta a lungo.

Questo fattore influenzerà il funzionamento dei load-balancers HTTP fino ad arrivare ad una situazione in cui sarà lo stesso sito a consigliare al client di riconnettersi attraverso un altro host, per motivi di performance, maintenance, etc.
=======
Il protocollo http2 impone che un ricevente sia obbligato a leggere e ignorare tutti i frame sconosciuti (riportanti un tipo di frame sconosciuto). Due estremi possono negoziare l'uso di nuovi tipi di frame su base hop-by-hop, ma tali frame non hanno il permesso di cambiare stato e il loro flusso non beneficerà di flow-control.

Il fatto che http2 debba poter supportare alucna estensione fu dibattuto a lungo durante lo sviluppo del protocollo, con opinioni variabili, pro e contro. Dopo la draft-12 il pendolo ha ocillato un ultima volta, in favore delle estensioni.

Le estensioni non sono parte integrante del protocollo ma saranno documentate all'esterno del documento che specifica i fondamenti del protocollo. Ci sono gia due nuovi tipi di frame per i quali se ne discute l'inclusione ufficiale, i primi frame ad essere spediti come estensioni. Li descriverò vista la loro popolarità e il loro precedente stato di frame "nativi":

## 7.1. Servizi Alternativi

Con l'adozione di http2, abbiamo ragione di sospettare che le connessioni TCP siano ben più lunghe, durevoli, e che saranno mantenute ben più a lungo rispetto alla durata media delle anziane connessioni HTTP 1.x. Un client dovrebbe essere in grado di fare praticamente tutto con una sola connessione per ogni host/sito; tale connessione potrebbe potenzialmente rimanere aperta a lungo.

Questo fattore influenzerà il funzionamento dei load-balancers HTTP, fino ad arrivare ad una situazione in cui sarà lo stesso sito a consigliare al client di riconnettersi attraverso un altro host, per motivi di performance, maintenance, etc.
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27

Il server manderà un header [Alt-Svc:
header](https://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-10) (o un frame 
ALTSVC in http2) istruendo il client a proposito del servizio alternativo:
<<<<<<< HEAD
un'altra rotta verso lo stesso contenuto, rotta che però utilizza un servizio 
ed un numero di porta differenti.

Un client dovrebbe dunque provare a connettersi a tale servizio in maniera asincrona ed utilizzare tale alternativa solo in caso che la connessione abbia successo.

### 7.1.1. TLS opportunistico

L'header Alt-Svc permette ad un server di contenuti via http:// di informare il client che lo stesso contenuto è disponibile anche attraverso una connessione TLS.

Questa è in qualche modo una feature discutibile. Tale connessione utilizzerebbe TLS non-autenticato e non sarebbe ritenuta "sicura" comunque, non essendo predisposta per avvertire in alcun modo l'interfaccia utente (UI) della disponibilità di una connessione cifrata. Oltretutto non avremmo modo di distinguere ove l'utente stia utilizzando il caro e vecchio HTTP. In effetti molti esperti sono ancora fermamente contrari a tale opportunismo in ambito TLS.
=======
un'altra rotta verso lo stesso contenuto, utilizzando però un servizio ed un 
numero di porta differenti.

Un client dovrebbe dunque provare a connettersi a tale servizio in maniera asincrona, ed utilizzare tale alternativa solo in caso che la connessione abbia successo.

### 7.1.1. TLS opportunistico

L'header Alt-Svc permette ad un server di contenuti via http:// di informare il client che lo stesso contenuto è anche disponibile attraverso una connessione TLS.

Questa è in qualche modo una feature discutibile. Tale connessione utilizzerebbe TLS non-autenticato e non sarebbe ritenuta "sicura" comunque, non essendo predisposta per avvertire in alcun modo l'interfaccia utente (UI) della disponibilità di una connessione cifrata, ed oltretutto non avremmo modo di distinguere ove l'utente stia utilizzando il caro vecchio HTTP. In effetti, molti esperti sono ancora fermamente contrati a tale opportunismo in TLS.
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27

## 7.2. Bloccato

Si suppone che un frame di questo tipo sia inviato esattamente una sola volta
<<<<<<< HEAD
da parte di un peer http2 quando esso dovesse avere dati da spedire, ma il flow-
control glielo avesse impedito. In pratica, se la tua implementazione riceve un
frame di questo tipo, evidenzierebbe un problema a livelo di configurazione e/o
=======
da parte di un peer http2 quando esso avesse dei dati da spedire, ma il flow-
control glielo impedisse. In pratica, se la tua implementazione riceve un frame
di questo tipo, evidenzierebbe un problema a livelo di configurazione oppure
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27
transfer-rate non ottimale.

Una citazione dalla draft-12, prima che questo frame diventasse una estensione a parte:

<<<<<<< HEAD
> "Il frame BLOCKED è incluso in questa draft per facilitare la sperimentazione. Se i risultati non dovessero mostrare un vantaggio o miglioramento tale frame potrà essere rimosso"
=======
> "Il frame BLOCKED è incluso in questa draft per facilitare la sperimentazione. Se i risultati non dovessero mostrare un vantaggio o miglioramento, tale frame potrà essere rimosso"
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27
