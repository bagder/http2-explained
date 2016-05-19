11. curl 에서 http2


curl 프로젝트 는 시험 적으로 http2 의 지원 을 2013 년 9 월부터 실시하고 있습니다 .


curl 정신에 의해서, 우리는 할 수있는 한 모든 http2 기능 을 제공 할 예정 입니다. curl 은 종종 테스트 도구, 그리고 web 사이트 를 여러가지 가져 오는 개발자 의 수단 으로 쓰이기 때문에
http2 에서도 이 전통을 잇는 예정입니다.

curl 은 타사 라이브러리 nghttp2 을 http2 프레임 레이어 의 구현 에 사용 하고 있습니다. 
curl 은 nghttp2 1.0 이상 이 필요합니다.


curl 및 libcurl 은 Linux 배포판 에서 설치 한 경우 아직 반드시 HTTP / 2 프로토콜 이 지원 되는 것은 아니라는 점에 주의 하십시오.



11.1 HTTP 1.x 고스란히

curl 은 내부적으로 받은 http2 헤더 를 HTTP 1.x 스타일 의 헤더 로 변환 하여 사용자에게 제시 하기 때문에 기존 HTTP 처럼 보입니다

이에 따라 기존 curl 및 HTTP 의 사용 에서 쉽게 마이그레이션 할 수 있습니다. 전송 헤더 도 마찬가지입니다. 
HTTP 1.x 스타일 에서 헤더 를 curl 에 전달하고 http2 서버 와 통신 할 때 자동으로 변환 됩니다 이를 통해 사용자 는 특정 HTTP 버전 이 사용되고 있는지 여부 등 것에 신경 쓰지 않아도 입니다.



11.2 안전하지 않은 평문
curl 은 Upgrade 헤더 http2 을 표준 TCP 에서 지원 하고 있습니다 당신 이 HTTP 요청 에서 HTTP / 2 를 요청하는 경우 curl 은 서버 에 가능하다면 연결 을 http2 으로 업그레이드 하도록 요청 합니다.



11.3 TLS 와 라이브러리

curl 은 많은 TLS 라이브러리 를 지원 하며 http2 지원 도 마찬가지입니다. TLS 의 http2 지원 의 문제점 은 ALPN 지원을 하지 않고 NPN만 지원 합니다.

최근 OpenSSL 또는 NSS 와 함께 curl 을 빌드하면  ALPN 와 NPN 모두 의 지원을 얻을 수 있습니다
GnuTLS 과 PolarSSL 의 경우 ALPN 를 사용할 수 있지만, NPN은 사용할 수 없습니다.


11.4 명령 줄 에서 사용
curl 에 http2 를 사용하도록 지시 하려면 TLS 에 관계없이  --http2 옵션 을 사용합니다
curl 은 아직 기본 이 HTTP 1.1 이고 http2 를 사용하는 경우에는 추가 옵션 이 필요합니다


11.5 libcurl 옵션
11.5.1 HTTP / 2 를 사용 \
응용 프로그램 에서 https : // 또는 http : // URL을 지금 까지대로 사용하지만 http2 를 사용하려면 curl_easy_setopt 의 CURLOPT_HTTP_VERSION 옵션 을 CURL_HTTP_VERSION_2 합니다.
이렇게 함으로써 가능한 한 http2 를 사용하게 되지만 그것을 할 수없는 경우는 HTTP 1.1 이 사용됩니다


11.5.2 다중화
libcurl 은 기존 의 행동 을 유지하려고 하기 때문에 HTTP / 2 의 다중화 응용 프로그램 에서 사용 하려면 CURLMOPT_PIPELINING 옵션 을 사용합니다
이 옵션 을 사용하지 않는 경우 지금 까지와 같이 연결 당 동시 요청 수는 1이 됩니다.

또 하나 주의해야 할 것은 
multi 인터페이스 를 사용하여 여러 전송 을 동시에 libcurl 로 할 경우 다중 연결을 사용 하는 것입니다
libcurl 을 조금 기다리게 동일한 연결 에 모든 전송 을 다중화 하려면 기다리게 전송 에 CURLOPT_PIPEWAIT 옵션 을 사용합니다.


11.5.3 서버 푸시
libcurl 7.44.0 이후는 HTTP / 2 서버 푸시 를 지원 하고 있습니다
이 기능 을 사용하려면 CURLMOPT_PUSHFUNCTION 옵션 을 사용하여 푸시 콜백 을 설정합니다

푸시 가 응용 프로그램 에 의해 받아 들여진 경우 새로운 CURL easy handle 이 생성 되어 다른 전송 뿐만 아니라 컨텐츠를 수신 합니다.
