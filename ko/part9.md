
# 9. Firefox에서의 http2

Firefox는 매우 밀접하게 초안에 따라 http2 테스트의 구현을 몇 달 동안 제공해 왔습니다. http2 프로토콜의 개발 과정에서 클라이언트와 서버는 테스트를 실행 할 수 있는 어떤 초안 버전에 동의해야합니다. 클라이언트와 서버가 어떤 프로토콜 초안에 주의해야합니다.

## 9.1 http2 활성화 확인

2015년 1월 13일에 출시된 Firefox35에서 http2지원이 기본적으로 활성화되어 있습니다.
주소창 에 'about:config'를 입력하고 "network.http.spdy.enabled.http2draft"라는 옵션을 찾을 수 있습니다. 해당 옵션이 true로 설정되어 있는지 확인 하십시오. Firefox36은 "network.http.spdy.enabled.http2" 라는 다른 옵션을 사용하였으며 기본적으로 true로 설정되어 있습니다. 후자는 http2 "표준" 버전을 제어 하는 반면, 전자는 http2 초기버전을 옵션을 활성화/비활성화 합니다. 두 옵션 모두 Firefox36에서 기본적으로 true로 되어 있습니다.


## 9.2 TLS-only

Firefox는 http2을 TLS에서만 구현하는것을 잊지 마십시오. Firefox에서 https://의 http2을 지원하는 사이트에서만 http2 작동 합니다.


## 9.3 Transparent

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

http2이 사용되고 있는지를 보여주는 UI는 없습니다. 쉽게 알 수 없도록 되어 있습니다. 확인할수 있는 하나의 방법은 "Web developer->Network"를 열고 응답 헤더에서 서버가 무엇을 리턴하는지 볼 수 있습니다. 위의 스크린 샷에 보는 바와 같이 응답은 "HTTP/2.0" 이며, Firefox가 "X-Firefox-Spdy:" 라는 독자적인 헤더를 삽입 합니다.

네트워크 도구에서 볼 수있는 헤더는 http2 바이너리 형식에서 기존 HTTP 1.x 스타일의 헤더로 변환되어 있습니다.



## 9.4 http2 시각화


사이트가 http2을 사용하고 있는지 시각화를 도와 하는 Firefox 플러그인이 있습니다. 그중 하나는 "[HTTP/2 and SPDY Indicator](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/)" 입니다.
