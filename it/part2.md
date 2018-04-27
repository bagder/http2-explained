# 2. HTTP oggi

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

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Latency kills

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 is very latency sensitive, partly because HTTP pipelining is still riddled with enough problems to remain switched off to a large percentage of users.

While we've seen a great increase in available bandwidth to people over the last few years, we have not seen the same level of improvements in reducing latency. High-latency links, like many of the current mobile technologies, make it hard to get a good and fast web experience even if you have a really high bandwidth connection.

Another use case requiring low latency is certain kinds of video, like video conferencing, gaming and similar where there's not just a pre-generated stream to send out.

## 2.6. Head-of-line blocking

HTTP pipelining is a way to send another request while waiting for the response to a previous request. It is very similar to queuing at a counter at the bank or in a supermarket: you just don't know if the person in front of you is a quick customer or that annoying one that will take forever before he/she is done. This is known as head-of-line blocking.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Sure, you can attempt to pick the line you believe is the correct one, and at times you can even start a new line of your own. But in the end, you can't avoid making a decision. And once it is made, you cannot switch lines.

Creating a new line is also associated with a performance and resource penalty, so that's not scalable beyond a smaller number of lines. There's just no perfect solution to this.

Even today, most desktop web browsers ship with HTTP pipelining disabled by default.

Additional reading on this subject can be found in the Firefox [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
