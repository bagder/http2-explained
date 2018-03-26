# 11. curl에서의 http2


curl 프로젝트는 http2의 지원을 2013년 9월부터 시험적으로 실시하고 있습니다.


curl 정신에 의해서, 우리는 할 수 있는 한 모든 http2 기능을 제공할 예정입니다. curl은 종종 테스트 도구, web 사이트의 여러가지를 가져오는 개발자의 수단으로 쓰이기 때문에 http2에서도 이 전통을 이을 예정입니다.

curl은 타사 라이브러리 [nghttp2](https://nghttp2.org/)를 http2 프레임레이어의 구현에 사용하고 있습니다. 
curl은 nghttp2 1.0 이상이 필요합니다.


curl 및 libcurl은 Linux 배포판에서 설치한 경우 반드시 HTTP/2 프로토콜이 지원 되는 것은 아니라는 점에 주의 하십시오.



## 11.1 HTTP 1.x을 고스란히

curl은 내부적으로 받은 http2 헤더를 HTTP 1.x 스타일의 헤더로 변환하여 사용자에게 제시하기 때문에 기존 HTTP처럼 보입니다.

이에 따라 기존 curl 및 HTTP의 사용에서 쉽게 마이그레이션 할 수 있습니다. 전송 헤더도 마찬가지입니다. 
HTTP 1.x 스타일에서 헤더를 curl에 전달하고 http2 서버와 통신 할 때 자동으로 변환됩니다. 이를 통해 사용자는 특정 HTTP 버전이 사용되고 있는지 여부 등은 신경 쓰지 않아도 됩니다.



## 11.2 안전하지 않은 Plain text
curl은 Upgrade 헤더 http2을 표준 TCP에서 지원하고 있습니다. 당신이 HTTP 요청에서 HTTP/2를 요청하는 경우 curl은 서버에 연결을 http2으로 업그레이드 하도록 요청합니다.


## 11.3 TLS와 라이브러리

curl은 많은 TLS 라이브러리를 지원하며 http2 지원도 마찬가지입니다. TLS의 http2 지원의 문제점은 ALPN 지원을 하지 않고 NPN만 지원 합니다.

최근 OpenSSL 또는 NSS와 함께 curl을 빌드하면  ALPN와 NPN 모두의 지원을 얻을 수 있습니다.
GnuTLS와 PolarSSL의 경우 ALPN를 사용할 수 있지만, NPN은 사용할 수 없습니다.


## 11.4 명령줄에서 사용하기
curl에 http2를 사용하도록 지시하려면 TLS에 관계없이  --http2 옵션을 사용합니다.
curl은 아직 기본이 HTTP1.1이고 http2를 사용하는 경우에는 추가 옵션이 필요합니다.


## 11.5 libcurl 옵션
### 11.5.1 HTTP/2를 사용하기 
응용 프로그램에서 https:// 또는 http:// URL을 지금까지대로 사용하지만 http2를 사용하려면 'curl_easy_setopt'의 'CURLOPT_HTTP_VERSION' 옵션을 'CURL_HTTP_VERSION_2'로 해야 합니다.
이렇게 함으로써 가능한 한 http2를 사용하게 되지만 사용할 수 없는 경우는 HTTP1.1이 사용됩니다.


### 11.5.2 다중화
libcurl은 기존의 행동을 유지하려고 하기 때문에 HTTP/2의 다중화 응용 프로그램에서 사용하려면 [CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html) 옵션을 사용합니다.
이 옵션을 사용하지 않는 경우 지금까지와 같이 연결 당 동시 요청 수는 1이 됩니다.

또 하나 주의해야 할 것은 multi 인터페이스를 사용하여 여러 전송을 동시에 libcurl로 할 경우 다중 연결을 사용하는 것입니다.
libcurl을 동일한 연결에 모든 전송을 다중화 하려면 전송시 [CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) 옵션을 사용합니다.


### 11.5.3 서버 푸시
libcurl 7.44.0 이후는 HTTP/2 서버 푸시를 지원하고 있습니다.
이 기능을 사용하려면 [CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html) 옵션을 사용하여 푸시 콜백을 설정합니다.

푸시가 응용 프로그램에 의해 받아 들여진 경우 새로운 CURL easy handle이 생성되어 다른 전송 뿐만 아니라 컨텐츠를 수신합니다.
