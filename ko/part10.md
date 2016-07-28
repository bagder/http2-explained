
#10. Chromium에서의 http2

Chromium팀은 http2을 구현하고 오랫동안 dev, beta 채널에서 그 지원을 실시하고 있습니다. 2015년 1월 27일에 출시된 Chrome 40에서 일부 사용자에 한해서 http2이 기본적으로 활성화 되었습니다. 그 숫자는 처음에는 낮게 설정 되어 있었습니다만, 시간이 지나면서 점차 증가 했습니다.

SPDY 지원은 삭제 될 예정 입니다. 2015년 2월에 블로그에서의 발표에 따르면 :
> "Chrome은 SPDY를 Chrome6에서 지원하고 왔습니다. 그러나 대부분의 혜택은 HTTP/2도 얻을 수 있어 안녕을 하기로 결정 했습니다. [SPDY을 2016년 초에 제거 할 예정](http://blog.chromium.org/2015/02/hello-http2-goodbye-spdy-http-is_9.html)입니다."


## 10.1. http2 활성화 확인
브라우저의 주소 표시줄에 "chrome://flags/#enable-spdy4"를 입력하고 아직 활성화 되어 있지않은 경우에는 "enable"을 클릭합니다.


## 10.2 TLS로 한정
Chrome은 http2를 TLS에서만 구현한 것을 잊지 마십시오. Chrome에서는 https://의 http2을 지원하는 사이트에서만 http2 작동합니다.


## 10.3 HTTP/2 시각화

사이트가 http2를 사용 하고 있는지 시각화를 도와 주는 Chrome플러그인이 있습니다. 그 중 하나는 [“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin) 입니다.



## 10.4 . QUIC

크롬은 HTTP/2를 어느 부분만 지원하고 있으며 QUIC를 실험하고 있습니다. (자세한 내용은 12.1를 참조하십시오.)
