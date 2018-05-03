# 2. HTTP oggi

<<<<<<< HEAD
HTTP 1.1 si è trasformato in un protocollo utilizzato (virtualmente) per qualsiasi scopo su Internet. Grandi investimenti sono stati fatti su questo protocollo e su infrastrutture che possano trarre beneficio da quest'ultimo, al punto che ad oggi è più semplice appoggiarsi ad HTTP piuttosto che creare da zero qualcosa di nuovo.

## 2.1 HTTP 1.1 è vasto, vastissimo

Al tempo in cui HTTP fu concepito e messo a disposizione dell'umanità, fu più probabilmente percepito come un protocollo semplice e diretto; tale teoria è stata poi negata nel corso del tempo. La RFC 1945 del 1996 su HTTP 1.0, consta di 60 pagine. La RFC 2616 che descrive HTTP 1.1 -rilasciata solo tre anni dopo, nel 1999- conta ben 176 pagine. Ebbene, quando la IETF ha approciato il lavoro sull'aggiornamento delle specifiche ha sentito il bisogno di suddividere il documento in sei parti, risultando così in un conteggio di pagine ancora più elevato (vedi RFC 7230 e correlate). In parole povere, HTTP 1.1 è IMMENSO ed include una miriade di dettagli, sottigliezze e -non meno importante- un elevato numero di parti opzionali.

## 2.2 Un mondo di opzioni

Alla luce della vastità di dettagli ed opzioni disponibili per estensioni future e dunque in perfetta relazione con la natura di HTTP 1.1, un vasto ecosistema software si è sviluppato; possiamo tuttavia notare come quasi nessuna implementazione implementi davvero tutto - ed è infatti quasi impossibile definire cosa questo "tutto" in effetti rappresenti. Questo ha fatto sì che certune caratteristiche (non utilizzate fin dall'inizio) non abbiano mai suscitato l'interesse di alcuni sviluppatori, e che di converso altri gruppi abbiano implementato esattamente tutte le funzioni, riscontrando però un basso utilizzo delle stesse.

Tutto ciò ha causato problemi di inter-operabilità, al momento in cui clients e servers hanno cominciato ad utilizzare sempre più estensivamente tali suddette caratteristiche ed estensioni. Il pipelining HTTP è un esempio principe di tale feature (e del suo stato di implementazione).

## 2.3 Uso inappropriato del TCP

Per HTTP 1.1 non è stato facile arrivare a trarre pieno beneficio dalla potenza e dalle performances che TCP mette teoricamente a disposizione. Client e server HTTP devono essere creativi per riuscire a diminuire il tempo di caricamento delle pagine.

Altri tentativi perpetrati in parallelo nel corso degli anni hanno confermato quanto il TCP non sia facilmente rimpiazzabile; si continua perciò a lavorare sul miglioramento del TCP e dei protocolli su di esso costruiti.

Mettiamola in termini semplici, il TCP può essere impiegato per evitare pause, intervalli o tempi morti in genere, frazioni di tempo che potrebbero altrimenti essere utilizzate per spedire e ricevere quantità di dati. Le sezioni seguenti evidenzieranno alcuni di questi prevedibili effetti.

## 2.4 Dimensione dei trasferimenti e numero di oggetti

Se osserviamo i trend per alcuni dei più popolari siti web odierni e quanto tempo prenda il download iniziale di tali "welcome pages" emerge chiaramente un pattern. Nel corso degli anni la quantità di dati di cui necessitiamo non ha fatto che aumentare verso e oltre gli 1.9MB. Cosa ancora più importante emerge da questo contesto: in media, più di 100 risorse individuali sono necessarie per visualizzare una determinata pagina.

Come mostra il grafico sotto, il trend è in corso da un pò di tempo e non mostra alcun segno di arresto o inversione sul breve periodo. Il grafico mostra la crescita della dimensione del trasferimento (in verde), il numero totale di richieste/risposte (in rosso) necessarie per servire il contenuto web dei siti più popolari al mondo, e come tali cifre si siano evolute nel corso degli ultimi quattro anni.
=======
HTTP 1.1 si è trasformato in un protocollo utilizzato (virtualmente) per qualsiasi scopo su Internet. Grandi investimenti sono stati fatti su questo protocollo e su infrastrutture che traggano beneficio da quest'ultimo, al punto che è ad oggi più semplice appoggiarsi ad HTTP piuttosto che creare da zero qualcosa di nuovo.

## 2.1 HTTP 1.1 è vastissimo

Al tempo in cui HTTP fu concepito e messo a disposizione del mondo intero, fu più probabilmente percepito come un protocollo semplice e diretto; tale teoria è stata negata nel corso del tempo. La RFC 1945 del 1996 su HTTP 1.0 consta di 60 pagine. La RFC 2616 descrive HTTP 1.1 ed è stata rilasciata solo tre anni dopo, nel 1999, contando così ben 176 pagine. Ebbene, quando la IETF ha approciato il lavoro sull'aggiornamento delle specifiche, ha sentito il bisogno di suddividere il documento in sei parti, risultato in un conteggio di pagine ben più elevato (risultante nella RFC 7230 e correlate). In parole povere, HTTP 1.1 è immenso ed include una miriade di dettagli, sottigliezze e -non mene importante- un elevato numero di parti opzionali.

## 2.2 Un mondo di opzioni

Alla luce della vastità di dettagli ed opzioni disponibili per estensioni future e quindi in relazione alla natura di HTTP 1.1, un vasto ecosistema software si è sviluppato, ma possiamo notare come quasi nessuna implementazione implementa tutto - ed è infatti quasi impossibile definire cosa questo "tutto" rappresenti. Questo ha fatto sì che certune caratteristiche non utilizzate fin dall'inizio non abbiano mai suscitato l'interesse degli sviluppatori, o che certi altri gruppi abbiano implementato esattamente tutte le funzioni, e che però abbiano riscontrato un basso utilizzo delle stesse.

In seguito ciò ha causato problemi di inter-operabilità, al momento in cui clients e servers hanno cominciato ad utilizzare sempre più estensivamente tali suddette caratteristiche ed estensioni. Il pipelining HTTP è un primo esempio di tale feature 

## 2.3 Uso inappropriato del TCP

Per HTTP 1.1 non è stato facile arrivare a trarre pieno beneficio della potenza e delle performances che TCP mette a disposizione. Client e server HTTP devono essere creativi per riuscire a diminuire il tempo di caricamento delle pagine.

Altri tentativi proseguiti in parallelo nel corso degli anni hanno confermato quanto il TCP non sia facilmente rimpiazzabile; si continua perciò a lavorare sul miglioramento del TCP e dei protocolli su esso costruiti.

Mettiamola in termini semplici, il TCP può essere impiegato per evitare pause, intervalli o tempi morti in genere che potrebbero altrimenti essere utilizzati per spedire e ricevere quantità di dati. Le sezioni seguenti evidenzieranno alcuni di questi prevedibili effetti.

## 2.4 Dimensione dei trasferimenti e numero di oggetti

Se osserviamo i trend per alcuni dei più popolari siti web odierni e quanto tempo prenda il download di tali welcome pages, emerge chiaramente un pattern. Nel corso degli anni, la quantità di dati di cui necessitiamo non ha fatto che aumentare verso e oltre gli 1.9MB. Cosa ancora più importante che emerge da questo contesto, in media, più di 100 risorse individuali sono necessarie per visualizzare una determinata pagina.

Come mostra il grafico sotto, il trend è in corso da un pò di tempo e non mostra alcun segno di arresto o inversione, nel breve periodo. Il grafico mostra la crescita della dimensione del trasferimento (in verde) e il numero totale di richieste/risposte usate in media (in rosso) per servire il contenuto web dei siti più popolari al mondo, e come essi si siano evoluti nel corso degli ultimi quattro anni.
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 La latenza uccide

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

<<<<<<< HEAD
HTTP 1.1 è molto sensibile alla latenza, in parte perchè il pipelining HTTP non ha mai sufficientemente risolto i propri problemi, tanto da -infine- meritare la disattivazione presso la maggior parte dell'utenza.

Mentre abbiamo potuto osservare un enorme incremento di larghezza di banda disponibile "per utente" durante il corso degli ultimi anni, non abbiamo potuto osservare lo stesso incremento di prestazione volto a ridurre la latenza. Su link ad elevata latenza come ad esempio la maggior parte delle tecnologie mobili, è perciò difficile godere di buone prestazioni web pur disponendo di una connessione a banda larga.

Un altro use-case che richiede bassa latenza è il video, video conferencing, gaming e simili, là dove non vi è la necessità di inviare uno stream "prestabilito o ordinato".

## 2.6. Bloccaggio "head-of-line" (inizio della fila)

Il cosiddetto "pipelining HTTP" è un metodo che permette di inviare una ulteriore richiesta mentre stiamo ancora aspettando la risposta alla domanda precendente. In un certo senso è come fare la coda alla banca o al supermarket: non sai in anticipo se la persona davanti si rivelerà essere un cliente veloce o uno di quei/quelle fastidiosi/e che impiegheranno un'eternità. Questo problema è noto come "head-of-line", inizio della coda (o della fila).

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Certo, puoi provare a scegliere la fila che pensi essere la più rapida o potresti addirittura iniziarne una nuova tu. Alla fine, non possiamo evitare di prendere decisioni. Una volta compiuta la scelta non si puè più cambiare.

Creare una nuova coda è allo stesso tempo associato ad una penalità in termini di risorse e performance; non consiste dunque in una soluzione scalabile oltre ad un piccolo numero di code. Non esiste soluzione ideale a questo problema.

Ancora oggi, la maggior parte dei browser "desktop" ha scelto di mantenere il pipelining disattivato.

Ulteriori risorse sull'argomento disponibili ad esempio sul bugzilla [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354) di Firefox.
=======
HTTP 1.1 è molto sensibile alla latenza, in parte perchè il pipelining HTTP non ha mai sufficientemente risolto i propri problemi tanto da meritare di essere disattivato presso la maggior parte dell'utenza.

Mentre abbiamo potuto osservare un enorme incremento di larghezza di banda disponibile per utente durante il corso degli ultimi anni, non abbiamo potuto osservare lo stesso incremento (o decremento piuttosto) di prestazioni volto ridurre la latenza. Su link ad elevata latenza, come la maggior parte delle tecnologie mobili, è difficile dunque godere di buone prestazioni web pur avendo una connessione a banda larga.

Un altro use-case che richiede bassa latenza è ad esempio il video, video conferencing, gaming e simili, la dove non vi è la necessità di inviare uno stream pregenerato e quindi "ordinato".

## 2.6. Bloccaggio "head-of-line" (ad inizio della coda)

Il pipelining HTTP è uno dei modi per mandare un'altra richiesta mentre stiamo ancora aspettando la risposta alla domanda precendente. In un certo senso è come fare la coda alla banca o al supermarket: non sai in anticipo se la persona davanti a te sarà un cliente veloce o uno di quei/quelle fastidiosi/e che ci mettono un'eternità a finire. Questo problema è noto come "head-of-line", inizio della coda.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Certo, puoi provare a scegliere la fila che pensi essere la più svelta, o potresti addirittura iniziarne una tua. Alla fine, non possiamo evitare di prendere decisioni. Una volta compiuta la scelta, non si puè più cambiare.

Creare una nuova fila è allo stesso tempo associato ad una penalità in termini di risorse e performance; non è dunque scalabile oltre un piccolo numero di file. Non esiste soluzione ideale a questo problema.

Even today, most desktop web browsers ship with HTTP pipelining disabled by default.

Ulteriori risorse sull'argomento si trovano ad esempio nella [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354) di Firefox.
>>>>>>> 37f9b9095c52ae58e06202c5b11babd5e7250b27
