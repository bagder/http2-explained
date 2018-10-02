# 12. Efter http2

En hel del tuffa beslut och kompromisser gjordes för http2. När http2
driftsätts finns det ett etablerat sätt att uppgradera till andra
protokoll-versioner och det arbetet är ett fundament för att göra fler
protokollrevisioner framöver. Det tar också in ett medvetande och en
infrastruktur som kan hantera flera olika versioner parallellt. Kanske behöver
vi inte fasa ut det gamla helt när vi introducerar nytt?

http2 har fortfarnade en massa gammal HTTP 1-"legacy" med sig i bagaget som
det tar med sig in i framtiden pga sin önskan att göra det möjligt att köra
proxies mellan HTTP1 och http2. En del av det legacyt begränsar framtida
utveckling och innovationer. Kanske kan http3 klippa bort några av dem?

Vad tycker du fortfarande saknas i http?

## 12.1. QUIC

Googles [QUIC](https://www.chromium.org/quic) (Quick UDP Internet
Connections)-protokoll är ett intressant experiment, utfört mycket i samma
stil och anda som de gjorde med SPDY. QUIC är en TCP + TLS + HTTP/2-ersättare,
implementerat över UDP.

I QUIC skapas ny koppel med mycket mindre fördröjning, det löser paket-loss
problemet så att det endast blockar individuella strömmar istället för alla
som det gör för http2 och det gör det möjligt för koppel att lätt hållas över
olika nätverksinterface - därmed täcker det också områden som MPTCP var menat
att lösa.

QUIC är än så länge bara implementerat av Google i Chrome och deras servrar,
och den koden är inte lätt att återanvända på andra ställen, även om det finns
ett [libquic](https://github.com/devsisters/libquic) som försöker precis det.
Protokollet har skrivits ner som en [Internet
draft](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) inom IETFs
transportarbetsgrupp.
