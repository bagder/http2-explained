# 1. 배경

이 문서는 기술과 프로토콜 측면서에서 http2를 서술하였습니다.
2014년 4월 스톡홀롬에서 Daniel이 제출하며 시작되고 이후 세밀하고 정확한 설명으로 가득찬 문서로 전환되고 확장되었습니다.

2015년 5월 발행된 http2의 최종명세의 정식 명칭은 RFC 7540 입니다.
http://www.rfc-editor.org/rfc/rfc7540.txt

이 문서에 있는 잘못된 내용들은 모두 나의 결점에서 나온것입니다. 따라서 잘못된 부분은 언제든지 지적해주시기 바라며, 지속적으로 갱신되고 수정될 것입니다.

이 문서에서 저는 HTTP/2.1 라는 새로운 프로토콜의 기술적인 용어를 지속적으로 http2 라는 단어를 사용해 서술할 것입니다. 그 이유는 생소한 단어를 재미있게 읽고 더 나은 흐름의 언어로 설명하기 위함입니다.


## 1.1 저자

저의 이름은 Daniel Stenberg 이고 저는 Mozilla에서 일하고 있습니다. 저는 근 20년간 수 많은 오픈소스 프로젝트에 기여하고, 소통하고 있습니다. 아마도 저는 curl과 libcurl를 이끄는 개발자로 알려져 있을겁니다.
저는 IETF HTTPbis 라는 그룹에서 수 년간 HTTP 1.1이 잘 구동되고 표준을 최신화 하는 일을 했습니다

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](http://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](http://daniel.haxx.se/blog/)

## 1.2 도와주세요!

만약 이 문서를 보고 있는 당신이 잘못 된 정보나 누락, 주제넘은 내용을 찾는다면 저에게 해당 문맥을 수정, 최신화 하여 보내주신다면 저는 해당 버젼으로 반영하겠습니다. 저는 이 프로젝트를 돕는 모두에게 적당한 명예적인 보상을 드리겠습니다. 저는 오랜 시간동안 이 문서가 더 나아지길 바랍니다.

이 문서는 이 사이트에서 보실 수 있습니다. [http://daniel.haxx.se/http2](http://daniel.haxx.se/http2)

## 1.3 라이센스

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

이 문서는 Creative commons Attribution 4.0에 해당하고 해당 라이센스는 http://creativecommons.org/licenses/by/4.0/ 여기서 확인하실 수 있습니다.

## 1.4 이 문서의 역사

이 문서의 첫 번째 버젼은 2014년 4월 25일에 발행되습니다. 아래에 최신의 큰 변화를 기재해두었습니다

### Version 1.13

- 이 문서를 Markdown 버젼으로 변환하였습니다
- 13: 리소스에 대한 더 많은 언급, 링크와 서술 갱신
- 12: QUIC의 reference 초안 최신화
- 8.5: 현재 번호 새로 고침
- 3.4: 평균 40 TCP 커넥션
- 6.4: 스펙을 최신으로 반영

### Version 1.12

- 1.1 HTTP/2 은 RFC 표준
- 6.5.1: HPACK RFC 와 연결
- 9.1: Firefox 36이후 버젼을 http2로 바꾸기위한 설정 언급
- 12.1: QUIC 섹션 추가 

### Version 1.11

- 신뢰할만한 contributor 의 지적사항으로 많은 문맥 개선
- 8.3.1: nginx와 Apache httpd 구체적인 활동에 대한 언급

### Version 1.10

- 1: 승인 된 프로토콜
- 4.1: 2014년 이후 표현을 새롭게 수정
- Front: 이미지를 추가하고 그곳에 "http2 설명"을 불러오도록 링크 수정
- 1.4: 문서의 역사 항목 추가
- 많은 스펠링과 문법의 오류를 수정함
- 14: 버그를 보고한 사람들에 대한 감사함 표시
- 2.4: 더 나은 HTTP 성장 그래프 라벨링
- 6.3: 다중화 스트림의 순서 수정
- 6.5.1: HPACK 초안-12 

### Version 1.9

- HTTP/2 초안 17과 HPACK의 초안11로 최신화
- "10. http2 크롬" 항목 추가 (== 현재 한 페이지 이상)
- 많은 철자 수정
- At 30 implementations now
- 8.5: 통계치 추가
- 8.3: 인터넷 익스플로러
- 8.3.1 구현이 누락된 부분 추가 
- 8.4.3: TLS의 성공률 향상
