# 5. http2 concepts

So what does http2 accomplish? Where are the boundaries for what the HTTPbis group set out to do?

They were actually quite strict and put quite a few restraints on the team's ability to innovate.

- It has to maintain HTTP paradigms. It is still a protocol where the client sends requests to the server over TCP.

- http:// and https:// URLs cannot be changed. There can be no new scheme for this. The amount of content using such URLs is too big to expect them to change.

- HTTP1 servers and clients will be around for decades, we need to be able to proxy them to http2 servers.

- Subsequently, proxies must be able to map http2 features to HTTP 1.1 clients 1:1.

- Remove or reduce optional parts from the protocol. This wasn't really a requirement but more a mantra coming over from SPDY and the Google team. By making sure everything is mandatory there's no way you can not implement anything now and fall into a trap later on.

- No more minor version. It was decided that clients and servers are either compatible with http2 or they are not. If there comes a need to extend the protocol or modify things, then http3 will be born. There are no more minor versions in http2.

## 5.1. http2 for existing URI schemes

As mentioned already the existing URI schemes cannot be modified so http2 has to be done using the existing ones. Since they are used for HTTP 1.x today, we obviously need to have a way to upgrade the protocol to http2 or otherwise ask the server to use http2 instead of older protocols.

HTTP 1.1 has a defined way how to do this, namely the Upgrade: header, which allows the server to send back a response using the new protocol when getting such a request over the old protocol. At a cost of a round-trip.

That round-trip penalty was not something the SPDY team would accept, and as they also only implemented SPDY over TLS they developed a new TLS extension which is used to shortcut the negotiation quite significantly. Using this extension, called NPN for Next Protocol Negotiation, the server tells the client which protocols it knows and the client can then proceed and use the protocol it prefers.

## 5.2. http2 for https://

A lot of focus of http2 has been to make it behave properly over TLS. SPDY is only done over TLS and there's been a strong push for making TLS mandatory for http2 but it didn't get consensus and so http2 shipped with TLS as optional. However, two prominent implementers have stated clearly that they will only implement http2 over TLS: the Mozilla Firefox lead and the Google Chrome lead. Two of the leading web browsers of today.

Reasons for choosing TLS-only include respect for user's privacy and early measurements showing that new protocols have a higher success rate when done with TLS. This because of the widespread assumption that anything that goes over port 80 is HTTP 1.1 makes some middle-boxes interfere and destroy traffic when instead other protocols are communicated there.

The subject about mandatory TLS has caused much hand-waving and agitated
voices in mailing lists and meetings – is it good or is it evil? It is an
infected subject – be aware of this when you throw this question in the face
of an HTTPbis participant!

Similarly, there's been a fierce and long-going debate on whether http2 should dictate a list of ciphers that should be mandatory when using TLS, or if it perhaps should blacklist a set or if it shouldn't require anything at all from the TLS “layer” but leave that to the TLS WG. The spec ended up specifying that TLS should be at least version 1.2 and there are  cipher suite restrictions.

## 5.3. http2 negotiation over TLS

Next Protocol Negotiation (NPN), is the protocol used to negotiate SPDY with TLS servers. As it wasn't a proper standard, it was taken through the IETF and ALPN came out of that: Application Layer Protocol Negotiation. ALPN is what is being promoted to be used for http2, while SPDY clients and servers still use NPN.

The fact that NPN existed first and ALPN has taken a while to go through standardization has lead to many early http2 clients and http2 servers implementing and using both these extensions when negotiating http2. Also, as NPN is what's used for SPDY and many servers offer both SPDY and http2 so supporting both NPN and ALPN on those servers make perfect sense.

ALPN primarily differs from NPN in who decides what protocol to speak. With ALPN the client tells the server a list of protocols in its order of preference and the server picks the one it wants, while with NPN the client makes that final choice.

## 5.4. http2 for http://

As mentioned briefly previously, for plain-text HTTP 1.1 the way to negotiate
http2 is by asking the server with an Upgrade: header. If the server speaks
http2 it responds with a “101 Switching” status and from then on it speaks
http2 on that connection. You of course realize that this upgrade procedure
costs a full network round-trip, but the upside is that an http2 connection
should be possible to keep alive and re-use to a larger extent than HTTP1
connections generally are.

While some browsers' spokespersons have stated they will not implement this means of speaking http2, the Internet Explorer team has expressed that they will, and curl already supports this.
