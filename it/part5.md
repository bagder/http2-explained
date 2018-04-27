# 5. http2 a grandi linee

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
