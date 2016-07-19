# 5. http2 컨셉

http2로 무엇을 이룰 수 있을까요? HTTPbis 그룹이 시작한 일이 무엇인지에 대한 경계는 어디인가?

경계는 사실상 상당히 엄격했으며, 팀 능력 향상, 동기부여의 가능성을 방해하는 구속의 역할을 했다:

- http2 패러다임을 유지한다. 이것은 아직 client가 TCP server에 requests를 보내는 프로토콜의 형태를 띄고있다.

- http:// and https:// URLs 은 변할 수 없다. 그것을 위한 새로운 scheme 같은 것은 없다. URLs같은 것들을 이용하는 모든 콘텐츠는 바꾸기엔 너무나도 많은 예외처리를 해주어야하기 때문이다.

- HTTP1 server 와 clients는 앞으로도 수십년은 존재하고 있을 것이며, 우리는 이것들을 http2 서버로 proxy 할 수 있어야한다.

- 그 후, proxy들은 http2 features 와 HTTP 1.1 clients간의 1 대 1  관계를 만들 수 있어야 한다. 

- 프로토콜에서 오는 선택 사항들을 없애거나 줄여라. 이것들은 요구사항이 아니지만 SPDY와 Google팀 으로부터 나온 mantra 이다. 모든 것을 마킹하는 것은 필수적이다. 그것은 너가 구현하지 못할 상황에 빠지거나 나중에 함정에 빠지는 상황이일어나지 않게 해준다.

- 더 이상 이전버전은 없다. 이것은 clients와 server가 http2에 호환되거나 그러지 그렇지 않거나를 결정 짓는데에서 도출된 결과입니다. 만약 프로토콜의 확장이나 수정에 대한 필요성이 대두된다면, http3가 탄생하게 될 것입니다. 적어도 http2에서 이전 버전은 존재하지 않습니다.

## 5.1 현존하는 URI schemes에 대한 http2

앞서 언급했지만, 현존하는 URI schemes 는 수정될 수 없다, 그래서 http2를 사용해야한다. HTTP 1.x 버전을 사용하게 된 이후로 오늘날 까지, 우리는 확실히 http2 에서 프로토콜에 대한 업그레이드가 있어야 한다고 생각했다. 아니면 낡아빠진 프로토콜 대신 서버가 http2를 사용하도록 해야했습니다.

HTTP 1.1 은 이 작업을 수행하기 위해서 정의된 방법이 있는데, 업그레이드: 헤더라고불리는 낡은 프로토콜로 보내진 리퀘스트를 받으면 자동으로 새로운 프로토콜 응답으로 반환하는 방법입니다.

왕복 패널티는 SPDY팀이 받아들일 만한 문제가 아니었으며, 그들은 SPDY를 TLS에서만 구현하고 있었기 때문에, 그들은 협상을 단순화 하는 TLS의 확장판을 개발해내었습니다. 이 NPN(Next Protocol Negotiation이라고 부른다.) 확장판을 이용하면, server는 자신이 알고있는 프로토콜을 이용해서 client를 호출하며 client는 이 프로토콜을 받을 수 있습니다.

## 5.2. https://를 위한 http2

수많은 http2의 초점은 TLS에서 적절하게 동작하는 것이었습니다. SPDY는 TLS가 필요하며, http2에서 사용하도록 TLS 를 크게 밀어주는 경우도 있었습니다, 하지만 이는 합의점을 찾지 못했고 결국 TLS는 http2에서 필요없게 되었습니다. 그러나 현대의 흐름을 주도하는 두 웹브라우저인 Google Chrome 과 Mozilla Firefox 개발진들은 TLS에서 http2를 구현했다고 말했습니다.


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
