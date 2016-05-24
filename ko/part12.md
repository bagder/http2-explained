
# 12. HTTP2 다음에 오는 것


많은 어려운 결정과 타협이 HTTP2에서 이루어졌습니다. HTTP2가 배포되면 다음 프로토콜 버전으로 업그레이드를 위한 기초가됩니다.
또한 다른 여러 버전을 동시에 처리 할 수 있는 개념과 인프라를 제공합니다. 아마도 새로운 것을 도입 할 때 과거를 모두 버릴 필요가 없어 질 것입니다.


HTTP2는 HTTP1 HTTP2 사이 의 통신 을 프록시 할 수 있도록 욕망을 위해 HTTP1 "유산"을 많이 계승하고 있습니다.
유산 중 일부는 발전과 발명을 방해합니다. 어쩌면 HTTP3에서는 그들을 버리는 것이 될지도 모르겠네요.

혹시 HTTP를 통해 부족하다고 생각하는  부분이 있나요?

## 12.1. QUIC

Google의 [QUIC](https://www.chromium.org/quic)( Quick UDP Internet Connections )은 흥미로운 실험입니다. 그것은 SPDY 때와 유사한 스타일과 정신으로 이루어지고 있습니다.
QUIC은 TCP + TLS + HTTP/2의 대안이며 UDP를 사용하여 구현되었습니다.

QUIC은 연결을 빠르게 할 수 있습니다.  HTTP/2는 패킷 손실에 의해 전체 스트림을 차단 했지만, QUIC에서는 대상 스트림만 차단 만하면됩니다.
다른 네트워크 인터페이스를 동시에 연결 유지도 가능합니다. 즉 MPTCP이 해결 하려고 하는 문제의 영역까지 커버하려 합니다.

QUIC은 현재 Google이 Chrome과 Google 서버에만 구현 되어 있습니다. 코드는 쉽게 재사용 할 수 있는 형태가 되어 있지 않습니다.
libquic라는 프로젝트가 그것을 실현 하려고 하고 있습니다. 프로토콜은 초안으로 [libquic](https://github.com/devsisters/libquic) 전송 워킹 그룹에 [제출](http://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) 되었습니다 .
