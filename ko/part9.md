
9. Firefox 에서 http2

Firefox는 매우 긴밀하게 초안 에 추종 하고 몇 달 동안 http2 테스트 구현을 제공해 왔습니다 . http2 프로토콜 의 개발 을 통해 클라이언트 와 서버 는 어떤 초안 버전 을 구현 하고 있는지 에 대해 합의 해야 테스트 를 할 때 약간 어색 했다. 클라이언트 와 서버 가 그 구현하는 프로토콜 초안 이 무엇인지 합의 하고 있는지 주의 하십시오.

9.1 먼저 http2 가 활성화 되어 있는지 확인 하십시오


2015 년 1 월 13 일 에 출시 된 Firefox 35 에서 http2 지원 이 기본적으로 활성화되어 있습니다.
주소창 에 ' about : config'를 입력 하고 " network.http.spdy.enabled.http2draft " 라는 옵션 을 찾을 수 있습니다. 그것이 true로 설정되어 있는지 확인 하십시오. Firefox 36 은 " network.http.spdy.enabled.http2 " 라는 다른 옵션 을 도입 하고 기본적으로 true로 설정되어 있습니다. 후자 는 http2 " 표준 " 버전 을 제어 하는 반면 , 전자는 초안 시대 의 http2 버전 의 협상 을 활성화 / 비활성화 합니다. 모두 Firefox 36 에서 기본적으로 true로 되어 있습니다.



9.2 TLS 한정

Firefox는 http2 을 TLS 에서만 구현 하는 것을 잊지 마십시오. Firefox에서 https : // 의 http2 을 지원 하는 사이트 에서만 http2 작동 합니다.

9.3 투과 !


http2 이 사용되고 있는지 를 보여주는 UI는 없습니다. 쉽게 알 수 없도록 되어 있습니다. 확인 하나 의 방법 은 " Web developer- > Network " 를 열고 응답 헤더 를 보고 서버 가 무엇 을 리턴 하는지 볼 수 있습니다. 위 의 스크린 샷 에 보는 바와 같이 응답 은 " HTTP / 2.0 " 이며, Firefox가 " X - Firefox - Spdy : " 라는 독자적인 헤더 를 삽입 합니다.

네트워크 도구 에서 볼 수있는 헤더 는 http2 바이너리 형식 에서 기존 HTTP 1.x 스타일 의 헤더 로 변환 되어 있습니다.



9.4 http2 의 사용 을 시각화


사이트 가 http2 을 사용 하고 있는지 시각화 를 도와 하는 Firefox 플러그인 이 있습니다. 그 중 하나 는 " HTTP / 2 and SPDY Indicator " 입니다.
