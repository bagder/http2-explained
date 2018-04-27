# 8. Un mondo http2

Quindi, come appariranno le cose quando http2 sarà adottato ? Sarà mai usato ?

## 8.1. Come http2 influenzerà l'utente tipo?

http2 non è ancora ne distribuito ne adodatto. Non possiamo ancora dire come le cose evolveranno. Abbiamo visto come SPDY sia stato messo in campo; possiamo solo fare qualche ipotesi basata su quella ed altre esperienze passate, ed sugli esperimenti in corso.

http2 riduce il numero di round-trip ed evita definitivamente l'eterno dilemma del "blocco a fine linea" attraverso l'utilizzo di multiplexing e fast-discarding degli stream non necessari.

Permette un vasto ammontare di streams paralleli, numero ampiamente sufficiente anche per il sito più "sharded" (parallelizzato) del momento.

Utilizzando correttamente le priorità sugli streams, il client sarà in grado di scaricare i dati importanti prima di quelli meno utili.
Visto tutto ciò, direi che beneficieremo di siti più  reattivi che si caricano in minor tempo. Mettiamola così: una miglior esperienza del web.

Quanto rapido sarà questo miglioramento, vedremo, non penso si possa ancora dire. Per prima cosa, la tecnologia è ancora in una fase di lancio e non si sono ancora visti client/server che implementino a dovere questa tecnologia per sfruttarne tutte le potenzialità.

## 8.2. Come http2 influenzera lo sviluppo web?

Nel corso degli anni, gli sviluppatori e gli IDE si sono riuniti a formare una immensa cassetta degli attrezzi piena di utensili e trucchi per circuire HTTP 1.1; come ricorderete, ne ho menzionati alcuni all'inizio di questo documento come giustificazioni per passare a http2.

Lots of those workarounds that tools and developers now use by default and without thinking, will probably hurt http2 performance or at least not really take advantage of http2's new super powers. Spriting and inlining should most likely not be done with http2. Sharding will probably be detrimental to http2 as it will probably benefit from using fewer connections.

A problem here is of course that web sites and web developers need to develop and deploy for a world that in the short term at least, will have both HTTP1.1 and http2 clients as users and to get maximum performance for all users can be challenging without having to offer two different front-ends.

For these reasons alone, I suspect there will be some time before we will see the full potential of http2 being reached.

## 8.3. http2 implementations

Trying to document specific implementations in a document such as this is of course completely futile and doomed to fail and only feel outdated within a really short period of time. Instead I'll explain the situation in broader terms and refer readers to the [list of implementations](https://github.com/http2/http2-spec/wiki/Implementations) on the http2 web site.

There were a large number of implementations early on, and the amount has increased over time during the http2 work. At the time of  writing this there are over 40 implementations listed, and most of them implement the final version.

### 8.3.1 Browsers

Firefox has been the browser that's been on top of the bleeding edge drafts,
Twitter has kept up and offered its services over http2. Google started during
April 2014 to offer http2 support on a few test servers running their services
and since May 2014 they offer http2 support in their development versions of
Chrome. Microsoft has shown a tech preview with http2 support for their next
Internet Explorer version. Safari (with iOS 9 and Mac OS X El Capitan) and
Opera have both said they will support http2.

### 8.3.2 Servers

There are already many server implementations of http2.

The popular Nginx server offers http2 support with since
[1.9.5](https://www.nginx.com/blog/nginx-1-9-5/) released on September 22,
2015 (where it replaces the SPDY module, so they cannot both run in the same
server instance).

Apache's httpd server has a http2 module [mod_http2](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) since 2.4.17 which was released on October 9, 2015.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](https://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/),
[Caddy](https://caddyserver.com/) and
[LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview)
have all released http2 capable servers.

### 8.3.3 Others

curl and libcurl support insecure http2 as well as the TLS based one using one
out of several different TLS libraries.

Wireshark supports http2. The perfect tool for analyzing http2 network
traffic.

## 8.4. Common critiques of http2

During the development of this protocol the debate has been going back and forth and of course there is a certain amount of people who believe this protocol ended up completely wrong. I wanted to mention a few of the more common complaints and mention the arguments against them:

### 8.4.1. “The protocol is designed or made by Google”

It also has variations implying that the world gets even further dependent or controlled by Google by this. This isn't true. The protocol was developed within the IETF in the same manner that protocols have been developed for over 30 years. However, we all recognize and acknowledge Google's impressive work with SPDY that not only proved that it is possible to deploy a new protocol this way but also provided numbers illustrating what gains could be made.

Google has publicly [announced](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) that they will remove support for SPDY and NPN in Chrome in 2016 and they urge servers to migrate to HTTP/2 instead.

### 8.4.2. “The protocol is only useful for browsers”

This is sort of true. One of the primary drivers behind the http2 development is the fixing of HTTP pipelining. If your use case originally didn't have any need for pipelining then chances are http2 won't do a lot of good for you. It certainly isn't the only improvement in the protocol but a big one.

As soon as services start realizing the full power and abilities the multiplexed streams over a single connection brings, I suspect we will see more application use of http2.

Small REST APIs and simpler programmatic uses of HTTP 1.x may not find the step to http2 to offer very big benefits. But also, there should be very few downsides with http2 for most users.

### 8.4.3. “The protocol is only useful for big sites”

Not at all. The multiplexing capabilities will greatly help to improve the experience for high latency connections that smaller sites without wide geographical distributions often offer. Large sites are already very often faster and more distributed with shorter round-trip times to users.

### 8.4.4. “Its use of TLS makes it slower”

This can be true to some extent. The TLS handshake does add a little extra,
but there are existing and ongoing efforts on reducing the necessary
round-trips even more for TLS. The overhead for doing TLS over the wire
instead of plain-text is not insignificant and clearly notable so more CPU and
power will be spent on the same traffic pattern as a non-secure protocol. How
much and what impact it will have is a subject of opinions and
measurements. See for example [istlsfastyet.com](https://istlsfastyet.com/)
for one source of info.

Telecom and other network operators, for example in the ATIS Open Web
Alliance, say that they [need unencrypted
traffic](https://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf)
to offer caching, compression and other techniques necessary to provide a fast
web experience over satellite, in airplanes and similar.  http2 does not make
TLS use mandatory so we shouldn't conflate the terms.

Many Internet users have expressed a preference for TLS to be used more widely and we should help to protect users' privacy.

Experiments have also shown that by using TLS, there is a higher degree of success than when implementing new plain-text protocols over port 80 as there are just too many middle boxes out in the world that interfere with what they would think is HTTP 1.1 if it goes over port 80 and might look like HTTP at times.

Finally, thanks to http2's multiplexed streams over a single connection, normal browser use cases still could end up doing substantially fewer TLS handshakes and thus perform faster than HTTPS would when still using HTTP 1.1.

### 8.4.5. “Not being ASCII is a deal-breaker”

Yes, we like being able to see protocols in the clear since it makes debugging and tracing easier. But text based protocols are also more error prone and open up for much more parsing and parsing problems.

If you really can't take a binary protocol, then you couldn't handle TLS and compression in HTTP 1.x either and its been there and used for a very long time.

### 8.4.6. “It isn't any faster than HTTP/1.1”

This is of course subject to debate and discussions on how to measure what faster means, but already in the SPDY days many tests were performed that proved browser page loads were faster (like ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) by people at University of Washington and ["Evaluating the Performance of SPDY-enabled Web Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) by Hervé Servy) and such experiments have been repeated with http2 as well. I'm looking forward to seeing more such tests and experiments getting published. A [basic first test made by httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) might imply that HTTP/2 holds its promises.

### 8.4.7. “It has layering violations”

Seriously, that's your argument? Layers are not holy untouchable pillars of a global religion and if we've crossed into a few gray areas when making http2 it has been in the interest of making a good and effective protocol within the given constraints.

### 8.4.8. “It doesn't fix several HTTP/1.1 shortcomings”

That's true. With the specific goal of maintaining HTTP/1.1 paradigms there were several old HTTP features that had to remain, such as the common headers that also include the often dreaded cookies, authorization headers and more. But the upside of maintaining these paradigms is that we got a protocol that is possible to deploy without an inconceivable amount of upgrade work that requires fundamental parts to be completely replaced or rewritten. Http2 is basically just a new framing layer.

## 8.5. Will http2 become widely deployed?

It is too early to tell for sure, but I can still guess and estimate and that's what I'll do here.

The naysayers will say “look at how good IPv6 has done” as an example of a new protocol that's taken decades to just start to get widely deployed. http2 is not an IPv6 though. This is a protocol on top of TCP using the ordinary HTTP update mechanisms and port numbers and TLS etc. It will not require most routers or firewalls to change at all.

Google proved to the world with their SPDY work that a new protocol like this can be deployed and used by browsers and services with multiple implementations in a fairly short amount of time. While the amount of servers on the Internet that offer SPDY today is in the 1% range, the amount of data those servers deal with is much larger. Some of the absolutely most popular web sites today offer SPDY.

http2, based on the same basic paradigms as SPDY, I would say is likely to be deployed even more since it is an IETF protocol. SPDY deployment was always held back a bit by the “it is a Google protocol” stigma.

There are several big browsers behind the roll-out. Representatives from Firefox, Chrome, Safari, Internet Explorer and Opera have expressed they will ship http2 capable browsers and they have shown working implementations.

There are several big server operators that are likely to offer http2 soon, including Google, Twitter and Facebook and we hope to see http2 support soon get added to popular server implementations such as the Apache HTTP Server and nginx. H2o is a new blazingly fast HTTP server with http2 support that shows potential.

Some of the biggest proxy vendors, including HAProxy, Squid and Varnish have expressed their intentions to support http2.

All throughout 2015, the amount of http2 traffic has been increasing. In early September, Firefox 40 usage was at 13% out of all HTTP traffic and 27% out of all HTTPS traffic, while Google sees roughly 18% of incoming request as HTTP/2. It should be noted that Google runs other new protocol experiments as well (see QUIC in 12.1) which makes the http2 usage levels lower than it could otherwise be.

