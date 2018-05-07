# 3. Tecniche applicate al contrasto della latenza

Quando si trovano faccia a faccia con questo tipo di problema, le persone si ritrovano e cercano una scappatoia. Alcune di queste soluzioni saranno intuitive ed intelligenti, altre orrendi accrocchi.

## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

"Spriting" è il termine usato per descrivere la tecnica che combina immagini multiple in un'unica immagine piu ampia. Attraverso JavaScript o CSS si può in seguito provare a decomporre l'immagine riassemblata nei singoli componenti.

Un sito utilizzerà spesso questa astuzia per migliorare la percezione di velocità. Acquisire una singola grande immagine in HTTP 1.1 è molto più veloce che richiedere 100 minuscole immagini separate.

Naturalmente, metodo svantaggioso se si desidera mostrare solo una o due immagini frammentarie. Lo spriting (spezzettamento?) implica anche che tutte le immagini vengano cancellate dalla cache allo stesso momento, piuttosto che permettere la persistenza dei frammenti richiamati più frequentemente, impattando quindi le performances..

## 3.2 Inlining

Lo "inlining" è un'altra tecnica utilizzata per evitare di spedire immagini individuali (tramite connessioni separate); tale tecnica consiste nell'utilizzare URL di tipo "data" incorporati nel CSS, con benefici e svantaggi simili al caso di "spriting" precedente.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Concatenazione

Un sito abbastanza vasto può contenere numerosi file JavaScript differenti. Gli sviluppatori possono utilizzare dei cosiddetti "frontend" per concatenare -o combinare- molteplici scipts e far sì che il browser possa richiedere tutto iu un singolo grande file piuttosto che segmentando la richiesta su dozzine di file minuscoli. Molti dati sono inviati benchè ne possano servire meno, di conseguenza una quantita sempre maggiore di oggetti deve essere invalidata e "rinfrescata" al modificarsi di ogni singola risorsa inclusa (effetto negativo sulla cache).

Questa prassi è -ovviamente- un inconveniente per gli sviluppatori.

## 3.4 Sharding (partizionamento. differenziamento)

L'ultimo trick per incrementare le performance che citerò è spesso chiamato "sharding". In breve consiste nell'utilizzare il maggior numero di hosts per fornire un determinato servizio. Anche se a prima vista può sembrare strano vi è una ragione ben precisa dietro l'utilizzo di questa tecnica.

All'inizio le specifiche HTTP 1.1 dichiaravano che un client fosse autorizzato ad utilizzare un massimo di due [2] connessioni TCP per server/host; dunque per non violare le specifiche, gli sviluppatori hanno iniziato ad utilizzare alias e nuovi nomi alternativi -et voilà- molte più connessioni disponibili verso il proprio sito e maggior velocità di caricamento (page load, page speed).

Nel tempo la restrizione è stata rimossa; ad oggi un client utilizza facilmente dalle sei alle otto connessioni per nome-host. Sono tuttavia ancora limitate, perciò i siti continuano ad utilizzare questa vecchia tecnica per incrementare il numero di connessioni disponibili per ogni singolo client. Visto che il numero di oggetti richiesti via HTTP continua a crescere -come mostrato sopra- l'ampio ammontare di connessioni è utilizzato per garantire che HTTP continui a rispondere con prestazioni decorose, permettendo al sito di caricare velocemente. Non è infrequente per un sito utilizzare 50, 100 o più connessioni. Statistiche del httparchive.org mostrano come i top 300mila URL al mondo in media necessitino almeno di 40 (!) connessioni TCP ciascuno; tale trend sembra aumentare lentamente nel tempo.

Un altro scenario nel quale si può utilizzare lo sharding è l'hosting di immagini, appunto su macchine (hosts) separate, in modo da non dover utilizzare o memorizzare alcun cookie, tenendo ben presente che il volume dei cookie è anch'esso in aumento. Utilizzando hosts senza cookie associati permette di aumentare le performances semplicemente riducendo il volume della richiesta HTTP !

L'immagine qui sotto mostra una trace (packe capture) acquisita durante la navigazione su uno dei piu famosi siti Svedesi, e dimostra come le richieste siano distribuite su  svariati hostnames.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
