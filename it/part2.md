# 2. HTTP oggi

HTTP 1.1 si è trasformato in un protocollo utilizzato (virtualmente) per qualsiasi scopo su Internet. Grandi investimenti sono stati fatti su questo protocollo e su infrastrutture che traggano beneficio da quest'ultimo, al punto che è ad oggi più semplice appoggiarsi ad HTTP piuttosto che creare da zero qualcosa di nuovo.

## 2.1 HTTP 1.1 è vastissimo

Al tempo in cui HTTP fu concepito e messo a disposizione del mondo intero, fu più probabilmente percepito come un protocollo semplice e diretto; tale teoria è stata negata nel corso del tempo. La RFC 1945 del 1996 su HTTP 1.0 consta di 60 pagine. La RFC 2616 descrive HTTP 1.1 ed è stata rilasciata solo tre anni dopo, nel 1999, contando così ben 176 pagine. Ebbene, quando la IETF ha approciato il lavoro sull'aggiornamento delle specifiche, ha sentito il bisogno di suddividere il documento in sei parti, risultato in un conteggio di pagine ben più elevato (risultante nella RFC 7230 e correlate). In parole povere, HTTP 1.1 è immenso ed include una miriade di dettagli, sottigliezze e -non mene importante- un elevato numero di parti opzionali.

## 2.2 Un mondo di opzioni

Alla luce della vastità di dettagli ed opzioni disponibili per estensioni future e quindi in relazione alla natura di HTTP 1.1, un vasto ecosistema software si è sviluppato, ma possiamo notare come quasi nessuna implementazione implementa tutto - ed è infatti quasi impossibile definire cosa questo "tutto" rappresenti. Questo ha fatto sì che certune caratteristiche non utilizzate fin dall'inizio non abbiano mai suscitato l'interesse degli sviluppatori, o che certi altri gruppi abbiano implementato esattamente tutte le funzioni, e che però abbiano riscontrato un basso utilizzo delle stesse.

In seguito ciò ha causato problemi di inter-operabilità, al momento in cui clients e servers hanno cominciato ad utilizzare sempre più estensivamente tali suddette caratteristiche ed estensioni. Il pipelining HTTP è un primo esempio di tale feature 

## 2.3 Inadequate use of TCP

HTTP 1.1 has a hard time really taking full advantage of all the power and performance that TCP offers. HTTP clients and browsers have to be very creative to find solutions that decrease page load times.

Other attempts that have been going on in parallel over the years have also confirmed that TCP is not that easy to replace, and thus we keep working on improving both TCP and the protocols on top of it.

Simply put, TCP can be utilized better to avoid pauses or wasted intervals that could have been used to send or receive more data. The following sections will highlight some of these shortcomings.

## 2.4 Transfer sizes and number of objects

When looking at the trend for some of the most popular sites on the web today and what it takes to download their front pages, a clear pattern emerges. Over the years, the amount of data that needs to be retrieved has gradually risen up to and above 1.9MB. What is more important in this context is that, on average, over 100 individual resources are required to display each page.

As the graph below shows, the trend has been going on for a while, and there is little to no indication that it will change anytime soon. It shows the growth of the total transfer size (in green) and the total number of requests used on average (in red) to serve the most popular web sites in the world, and how they have changed over the last four years.

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
