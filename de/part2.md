# 2. HTTP Heute

Das Protokoll HTTP 1.1 wird heutzutage für fast alles im Internet genutzt. Aufgrund der Tatsache das sehr viel Zeit und Geld in die HTTP 1.1 Infrastruktur investiert wurde ist es heutzutage einfacher etwas über HTTP 1.1 zum laufen zu bringen den etwas neues zu entwickeln.

## 2.1 HTTP 1.1 ist riesig

Als HTTP erstellt wurde dachte man das es ein einfaches und gradliniges Protokoll ist, die Zeit zeigte jedoch das diese Annahme falsch war. HTTP 1.0 in RFC 1945 wurde 1996 in nur 60 Seiten spezifiziert. RFC 2616 beschreibt HTTP 1.1 drei Jahre später, 1999, und wuchs auf 178 Seiten an.

2014 hat die IETF Arbeitsgruppe die Spezifikation aktualisiert. Diese Aktualisierung hatte zur Folge das aus einem Dokument sechs Dokumente geworden sind mit einer sehr hohen Seitenanzahl. Die neuen Spezifikationen finden sich in [RFC 7230](https://tools.ietf.org/html/rfc7230) und dessen Unternummern. 

Egal wie viele Seiten die Spezifikation hat, HTTP 1.1 ist riesig. Das Protokoll hat sehr viele Detailaspekte und ebenso viele Feinheiten.
Da HTTP 1.1 auch erweiterbar ist gibt es etliche optionale Elemente.

## 2.2 Eine Welt voller Varianten

Da das Protokoll HTTP 1.1 sehr viele Detailaspekte hat und erweiterbar ist hat sich ein großes Software Ökosystem gebildet. Keine Software kann „alle“ Möglichkeiten von HTTP 1.1 implementieren, da keiner sagen kann was „alles“ ist.

Aufgrund dieser Situation sind manche Funktionen selten implementiert worden und die diese Funktionen implementiert haben fanden heraus das dies Funktionen selten genutzt worden sind.

Weil die Software Entwickler verschiedene Funktionen implementiert haben oder eben nicht implementiert haben kam es immer wieder zu Interoperabilität-Problemen. HTTP Pipelining ist ein Paradebeispiel dafür.


## 2.3 Suboptimale Nutzung von TCP

HTTP 1.1 has a hard time really taking full advantage of all the power and performance that TCP offers. HTTP clients and browsers have to be very creative to find solutions that decrease page load times.

Other attempts that have been going on in parallel over the years have also confirmed that TCP is not that easy to replace and thus we keep working on improving both TCP and the protocols on top of it.

Simply put, TCP can be utilized better to avoid pauses or wasted intervals that could have been used to send or receive more data. The following sections will highlight some of these shortcomings.

## 2.4 Übertragungsgrößen und Anzahl der Objekte

When looking at the trend for some of the most popular sites on the web today and what it takes to download their front pages, a clear pattern emerges. Over the years the amount of data that needs to be retrieved has gradually risen up to and above 1.9MB . What is more important in this context is that on average over a hundred individual resources are required to display each page.

As the graph below shows, the trend has been going on for a while and there is little to no indication that it will change anytime soon. It shows the growth of the total transfer size (in green) and the total number of requests used on average (in red) to serve the most popular web sites in the world, and how they have changed over the last four years.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Wartezeiten Verursacher 

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 is very latency sensitive, partly because HTTP Pipelining is still riddled with enough problems to remain switched off to a large percentage of users.

While we've seen a great increase in available bandwidth to people over the last few years, we have not seen the same level of improvements in reducing latency. High latency links, like many of the current mobile technologies, make it really hard to get a good and fast web experience even if you have a really high bandwidth connection.

Another use case that really needs low latency is certain kinds of video, like video conferencing, gaming and similar where there's not just a pre-generated stream to send out.

## 2.6. Head of line blocking

HTTP Pipelining is a way to send another request while waiting for the response to a previous request. It is very similar to queuing at a counter at the bank or in a super market. You just don't know if the person in front of you is a quick customer or that annoying one that will take forever before he/she is done: head of line blocking.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Sure you can be careful about line picking so that you pick the one you really believe is the correct one, and at times you can even start a new line of your own but in the end you can't avoid making a decision and once it is made you cannot switch lines.

Creating a new line is also associated with a performance and resource penalty so that's not scalable beyond a smaller number of lines. There's just no perfect solution to this.

Even today, 2015, most desktop web browsers ship with HTTP pipelining disabled by default.

Additional reading on this subject can be found for example in the Firefox [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
