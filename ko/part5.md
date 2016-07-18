# 5. http2 컨셉

http2로 무엇을 이룰 수 있을까요? HTTPbis 그룹이 시작한 일이 무엇인지에 대한 경계는 어디인가?

경계는 사실상 상당히 엄격했으며, 팀 능력 향상, 동기부여의 가능성을 방해하는 구속의 역할을 했다:

- http2 패러다임을 유지한다. 이것은 아직 client가 TCP server에 requests를 보내는 프로토콜의 형태를 띄고있다.

- http:// and https:// URLs 은 변할 수 없다. 그것을 위한 새로운 scheme 같은 것은 없다. URLs같은 것들을 이용하는 모든 콘텐츠는 바꾸기엔 너무나도 많은 예외처리를 해주어야하기 때문이다.

- HTTP1 server 와 clients는 앞으로도 수십년은 존재하고 있을 것이며, 우리는 이것들을 http2 서버로 proxy 할 수 있어야한다.

- 그 후, proxy들은 http2 features 와 HTTP 1.1 clients간의 1 대 1  관계를 만들 수 있어야 한다. 

- 프로토콜에서 오는 선택 사항들을 없애거나 줄여라. 이것들은 요구사항이 아니지만 SPDY와 Google팀 으로부터 나온 mantra 이다. 모든 것을 마킹하는 것은 필수적이다. 그것은 너가 구현하지 못할 상황에 빠지거나 나중에 함정에 빠지는 상황이일어나지 않게 해준다.

- No more minor version. It was decided that clients and servers are either compatible with http2 or they are not. If a need arises to extend the protocol or modify things, then http3 will be born. There are no more minor versions in http2.

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

While some browser's spokespersons have stated they will not implement this means of speaking http2, the Internet Explorer team has expressed that they will, and curl already supports this.
