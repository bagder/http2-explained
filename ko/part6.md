# 6. http2 프로토콜

Enough about the background, the history and politics behind what got us here. Let's dive into the specifics of the protocol: the bits and the concepts that make up http2. 지금까지 http2 가 탄생한 배경 설명, 그 간의 역사와 정치적인 일은 충분히 다루었다. 이제는 프로토콜의 스펙에 대해 자세히 알아보자. http2 를 구성하는 요소와 개념은 무엇일까.

## 6.1. Binary 바이너리

http2 is a binary protocol. http2 는 바이너리 프로토콜이다.

Just let that sink in for a minute. If you've been involved in internet protocols before, chances are that you will now be instinctively reacting against this choice, marshaling your arguments that spell out how protocols based on text/ascii are superior because humans can handcraft requests over telnet and so on...
잠시 머리를 식혀보자. 만일 당신이 이전에 인터넷 프로토콜에 관여해봤다면 당신은 지금쯤 본능적으로 이 결정에 반대할 가능성이 높다. telnet 을 통한 요청들은 사람들이 수작업할 수 있다는 이유 등을 들면서 text/ascii 에 기반을 둔 프로토콜이 우수하다는 주장을 펼치면서 말이다.

http2 is binary to make the framing much easier. Figuring out the start and the end of frames is one of the really complicated things in HTTP 1.1 and, actually, in text-based protocols in general. By moving away from optional white space and different ways to write the same thing, implementation becomes simpler.
http2 는 바이너리이기에 프레임 작업을 훨씬 손쉽게 한다. 프레임의 시작 지점과 종료 지점을 알아내기란 HTTP 1.1 에서는 정말로 복잡한 일 중 하나다. 그리고 사실 대개 그것은 text 기반의 프로토콜이 그렇다. 선택적인 공백 문자와, 같은 것을 작성하는 여러 방법들이 있다는 것으로부터 벗어남으로써 구현은 간단해진다.

Also, it makes it much easier to separate the actual protocol parts from the framing - which in HTTP1 is confusingly intermixed. 
또 프로토콜이 바이너리인 점은 프레임 작업으로부터 실제 프로토콜 부분을 분리하는 것을 훨씬 간단하게 만들어 준다. - HTTP1 에서는 혼란스럽게도 서로 섞여 있다.

The fact that the protocol features compression and will often run over TLS also diminishes the value of text, since you won't see text over the wire anyway. We simply have to get used to the idea of using something like a Wireshark inspector to figure out exactly what's going on at the protocol level in http2.
프로토콜이 압축 기능을 제공하고 TLS 상에서 자주 사용되는 사실은 text 의 가치를 감소시킨다. 왜냐하면 어쨋건 당신은 text 를 볼 수 없을 것이기 때문이다. 우리는 Wireshark 와 같은 도구를 이용하는 아이디어에 익숙해져서 http2 상에서는 프로토콜 층위에서 실제로 어떤 일이 이뤄지고 있는지 살펴보아야 한다.

Debugging this protocol will probably have to be done with tools like curl, or by analyzing the network stream with Wireshark's http2 dissector and similar.
이 프로토콜을 디버깅하는 것은 아마도 curl 과 같은 도구를 사용하거나 Wireshark 의 http2 dissector 와 같은 도구로 네트워크 스트림을 분석하는 방식으로 이뤄져야 할 것이다.

## 6.2. The binary format 바이너리 형식

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2 sends binary frames. There are different frame types that can be sent and they all have the same setup: Length, Type, Flags, Stream Identifier, and frame payload. 
http2 는 바이너리 프레임을 전송한다. 서로 다른 타입의 프레임이 존재하고, 그들은 모두 구성이 같다. 길이, 타입, 플래그, 스트림 식별자, 그리고 프레임 페이로드.

There are ten different frame types defined in the http2 spec and perhaps the two most fundamental ones that map to HTTP 1.1 features are DATA and HEADERS. I'll describe some of the frames in more detail further on.
http2 스펙에 서로 다른 10 개의 프레임 타입이 정의되어 있고, 아마도 HTTP 1.1 기능에 대응하는 가장 주요한 타입은 DATA 와 HEADERS 일 것이다. 더 자세한 내용은 앞으로 진행하면서 설명하도록 하겠다.

## 6.3. Multiplexed streams 다중화 스트림

The Stream Identifier mentioned in the previous section associates each frame sent over http2 with a "stream". A stream is an independent, bi-directional sequence of frames exchanged between the client and server within an http2 connection.
이전 섹션에서 언급된 스트림 식별자는 하나의 "스트림"이 http2 를 통해 전송된 개개의 프레임과 관련이 있다. 하나의 스트림은 하나의 독립적이며, 양방향으로 전송되는 프레임들의 나열이고, 그 프레임들은 하나의 http2 연결 속에서 클라이언트와 서버 사이에 교환된다.

A single http2 connection can contain multiple concurrently-open streams, with either endpoint interleaving frames from multiple streams. Streams can be established and used unilaterally or shared by either the client or server and they can be closed by either endpoint. The order in which frames are sent within a stream is significant. Recipients process frames in the order they are received. 
단일 http2 연결은 동시에 열린 다중 스트림을 가질 수 있다. ...??? 스트림들은 일방적으로 형성되거나 사용될 수 있고, 클라이언트 또는 서버에 의해 공유될 수 있다. 그리고 그들은 각각의 엔드포인트에 의해 닫힐 수 있다. 프레임들이 하나의 스트림 안에서 보내지는 순서는 상당히 중요하다. 프레임을 전달받은 곳에서는 그들이 전달받은 순서대로 프레임을 처리한다.

Multiplexing the streams means that packages from many streams are mixed over the same connection. Two (or more) individual trains of data are made into a single one and then split up again on the other side. Here are two trains: 스트림을 다중화하는 것은 여러 스트림으로부터 받은 패키지들이 같은 연결 상에서 섞인다는 것을 의미한다. 두 개(혹은 그 이상) 정보를 가진 개별 기차가 단일의 기차로 합쳐지고, 그리고 다시 반대편에서 쪼개진다. 여기 두 개의 기차가 있다. 

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

The two trains multiplexed over the same connection: 같은 연결 상에서 두 기차가 다중화되어 있다.

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Priorities and dependencies 우선 순위와 의존 관계

Each stream also has a priority (also known as "weight"), which is used to tell the peer which streams to consider most important, in case there are resource restraints that force the server to select which streams to send first. 개별 스트림 역시 ("weight"라 알려진) 우선 순위를 가지고 있다. 그리고 그것은 상대에게 어떤 스트림이 가장 중요하게 여겨져야 할지를 알려준다. 자원 제약이 있을 때 서버가 어떤 스트림을 가장 먼저 보낼지 선택하는 상황을 대비하기 위해서다.
Using the PRIORITY frame, a client can also tell the server which other stream this stream depends on. It allows a client to build a priority "tree" where several "child streams" may depend on the completion of "parent streams". PRIORITY 프레임을 이용하여 클라이언트 역시 서버에게 이 스트림이 어떤 다른 스트림에 의존하고 있는지를 알려 줄 수 있다. 클라이언트가 우선 순위 "나무"를 만들 수 있게 해주고, 몇몇의 "자식 스트림"이 "부모 스트림"의 완료에 의존성이 있을지에 대한 정보를 담고 있다.

The priority weights and dependencies can be changed dynamically at run-time, which should enable browsers to make sure that when users scroll down a page full of images, the browser can specify which images are most important, or if you switch tabs it can prioritize a new set of streams that suddenly come into focus.
우선 순위 무게 ??? 와 의존 관계는 실행 시간에 동적으로 바뀔 수 있다. 사용자가 이미지로 가득찬 페이지를 스크롤 할 때 어떤 이미지들 가장 중요한지 브라우저가 특정할 수 있게 해주거나, 브라우저 탭을 바꿔 갑자기 집중되는 스트림을 새롭게 우선 순위를 높일 수 있게 할 수 있어야 한다.

## 6.5. Header compression 헤더 압축

HTTP is a stateless protocol. In short, this means that every request needs to bring with it as much detail as the server needs to serve that request, without the server having to store a lot of info and meta-data from previous requests. Since http2 doesn't change this paradigm, it has to work the same way. HTTP 는 상태가 없는 프로토콜이다. 요약하자면, 모든 요청은 서버가 그 요청을 처리하는 데 필요한 가능한 한 많은 정보를 전달해야 한다는 것을 뜻한다. 서버가 이전 요청들로부터 얻은 정보와 메타 정보를 저장할 필요 없이. http2 가 이 패러다임을 고수하기 때문에 그대로 동작해야 한다.

This makes HTTP repetitive. When a client asks for many resources from the same server, like images from a web page, there will be a large series of requests that all look almost identical. A series of almost identical somethings begs for compression.
이는 HTTP 를 반복적이게 만든다. 클라이언트가 같은 서버로부터 웹 페이지의 이미지들과 같은 많은 자원을 요청할 때, 거의 동일하게 생긴 수많은 요청이 생길 것이다. 거의 동일한 것들이 연속된다면 압축이 절실하다.

While the number of objects per web page has increased (as mentioned earlier), the use of cookies and the size of the requests have also kept growing over time. Cookies also need to be included in all requests, often the same ones in multiple requests.
(이전에 언급한 것처럼) 웹 페이지 별 객체의 수가 증가해 올 때, 시간이 지남에 따라 쿠키의 사용과 요청의 크기 또한 증가해 왔다. 쿠키는 모든 요청에 포함되어야 되고, 종종 여러 요청 사이에서 쿠키는 동일하다.

The HTTP 1.1 request sizes have actually gotten so large that they sometimes end up larger than the initial TCP window, which makes them very slow to send as they need a full round-trip to get an ACK back from the server before the full request has been sent. This is another argument for compression.
HTTP 1.1 요청 크기는 사실 너무 비대해진 나머지 종종 초기 TCP 윈도우 ??? 보다 커지는 상황에 이르기도 했다. 때문에 전송하기에 너무 느려져 버렸다. 그들은 서버로부터 완전한 요청이 보내지기 전에 ACK 응답을 받기 위해서라도 온전히 round-trip 을 해야한다. 이것은 압축이 필요하다는 또 다른 이유다.

### 6.5.1. Compression is a tricky subject 압축은 까다로운 주제다

HTTPS and SPDY compression were found to be vulnerable to the [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) and [CRIME](https://en.wikipedia.org/wiki/CRIME) attacks. By inserting known text into the stream and figuring out how that changes the output, an attacker can figure out what's being sent in an encrypted payload.
HTTPS 와 SPDY 압축은 [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) 와 [CRIME](https://en.wikipedia.org/wiki/CRIME) 공격에 취약점이 있다고 밝혀졌다. 알려진 text 를 스트림에 삽입하고, 그에 따른 결과가 어떻게 바뀌는지 확인함으로써 공격자는 암호화된 페이로드 안에 무엇이 전송되었는지 알 수 있다.

Doing compression on dynamic content for a protocol - without becoming vulnerable to one of these attacks - requires some thought and careful consideration. This is what the HTTPbis team tried to do.
프로토콜 안에서 동적으로 생성된 컨텐츠를 압축하는 것 - 위와 같은 공격들에 취약점을 노출하지 않으면서 - 은 여러 고민과 섬세한 고려들이 필요하다.

Enter [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2, which – as the name suggests - is a compression format especially crafted for http2 headers, and it is being specified in a separate internet draft. The new format, together with other counter-measures (such as a bit that asks intermediaries to not compress a specific header and optional padding of frames), should make it harder to exploit compression.
HTTP/2 를 위한 헤더 압축 기술인 [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt) 을 살펴 보아라. 이 기술은 이름을 통해 알 수 있듯이 http2 헤더를 위해 특별히 고안된 압축 형식이다.이 새로운 압축 형식은 다른 보조 장치( ??? )와 함께 압축된 내용을 파헤치기 어렵게 만들 것이다. 

In the words of Roberto Peon (one of the creators of HPACK):
HPACK 창시자 중 한 명인 Roberto Peon 의 말을 인용한다:

> "HPACK was designed to make it difficult for a conforming implementation to
> leak information, to make encoding and decoding very fast/cheap, to provide
> for receiver control over compression context size, to allow for proxy
> re-indexing (i.e., shared state between frontend and backend within a proxy),
> and for quick comparisons of Huffman-encoded strings".

> "HPACK 은 구현 방식을 제대로 따를 경우 정보 누출을 어렵게 만들고, 인코딩과 디코딩을 빠르고 쉽게 만들고,
> 수신자가 압축 맥락 ??? 크기 에 대한 통제권을 제공하고, 프록시의 재 인덱싱이 가능하도록 허용해주고, 
> (예를 들면, 프록시 안에서 ??? 프론트엔드와 백엔드 사이의 공유 상태), 그리고 호프만 인코딩된 문자열의 빠른 비교
> 등이 가능하도록 디자인되었다"


## 6.6. Reset - change your mind 재설정 - 당신의 생각을 바꿔라

One of the drawbacks with HTTP 1.1 is that when an HTTP message has been sent
off with a Content-Length of a certain size, you can't easily just stop
it. Sure, you can often (but not always) disconnect the TCP connection, but that
comes at the cost of having to negotiate a new TCP handshake again.
HTTP 1.1 의 단점 중 하나는 특정 크기로 Content-Length 를 지정하여 HTTP 메시지 전송을 시작했으면, 그 전송을 쉽게 중지하지 못한다는 것이다. 당연히도, 당신은 종종 (항상은 아니지만) TCP 연결을 끊을 수 있다. 그러나 이로 인해 다시금 새로운 TCP 핸드쉐이크를 수립해야하는 비용을 지불해야 한다. 

A better solution would be to just stop the message and start anew. This can be done with http2's RST_STREAM frame which will help prevent wasted bandwidth and the need to tear down connections.
더 나은 해법은 해당 메시지를 중지하고 새롭게 시작하는 것이 될 수 있다. 이것은 http2 의 RST_STREAM 프레임으로 이루어질 수 있고, 그것은 대역폭을 낭비하거나 연결을 분해해야하는 어려움을 방지할 수 있다.

## 6.7. Server push 서버 푸시

This is the feature also known as "cache push". The idea is that if the client asks for resource X, the server may know that the client will probably want resource Z as well, and sends it to the client without being asked. It helps the client by putting Z into its cache so that it will be there when it wants it.
이것은 "캐시 푸시" 로도 알려진 기능이다. 클라이언트가 자원 X 를 요청하고, 서버는 클라이언트가 아마도 자원 Z 를 원할 수 있다는 사실을 안다면, 클라이언트가 요청하지 않아도 자원 Z 를 클라이언트에게 보내는 것이 아이디어의 골자다. 자원 Z 를 캐쉬에 저장함으로써 언제든 필요로할 때 사용할 수 있게 클라이언트에 도움을 준다.

Server push is something a client must explicitly allow the server to do. Even then, the client can swiftly terminate a pushed stream at any time with RST_STREAM should it not want a particular resource.
서버 푸시는 클라이언트가 반드시 서버에게 이 기능을 사용해도 된다고 명시적으로 허용해줘야 하는 것이다. 더 나아가 클라이언트는 특정 자원을 원하지 않다면 재빠르게 푸시된 스트림을 RST_STREAM 을 사용해 언제든 종료시킬 수 있다.

## 6.8. Flow Control 전송량 통제

Each individual http2 stream has its own advertised flow window that the other end is allowed to send data for. If you happen to know how SSH works, this is very similar in style and spirit.
각 개별 http2 스트림은 각자가 공표 ??? 하는 전송량 허용 범위가 있어서 상대가 그 만큼의 데이터만 보낼 수 있도록 허용된다. 당신이 SSH 가 어떻게 작동하는 지 알고 있다면, 이것은 그것과 스타일과 정신 측면에서 매우 비슷하다.

For every stream, both ends have to tell the peer that it has enough room to handle incoming data, and the other end is only allowed to send that much data until the window is extended. Only DATA frames are flow controlled.
모든 스트림 양 말단은 상대에게 들어오는 데이터를 처리할 충분한 공간을 가지고 있는지 알려줘야 한다. 그리고 상대방은 허용 범위가 늘어지기 전까지는 그 만큼의 데이터만을 보내도록 허용된다. 오직 DATA 프레임들이 전송량이 통제된다.
