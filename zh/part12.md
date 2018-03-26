# 12. 后http2时代

http2做了许多艰难的折衷和妥协。随着http2逐渐部署，将会带来一个健全的协议升级方式，而这为将来更多的协议升级奠定了基础。同时，它也引入了一套概念和基础架构来并行处理多个不同版本协议。也许我们并不需要在引入新协议时就完全将旧的淘汰掉。

http2仍然背负了许多HTTP1的历史包袱，主要是为了保证数据流量能够在HTTP 1和http2之间无碍转发。这些包袱会阻碍进一步的的开发和创造，期待http3能丢掉其中一部分。

亲爱的读者，你认为http还缺少什么？

## 12.1. QUIC

Google的[QUIC](https://www.chromium.org/quic) （快速UDP互联网连接）协议是一个非常有趣的试验，它在很大程度上继承了SPDY的衣钵。QUIC是一个UDP版的TCP + TLS + HTTP/2替代实现。

QUIC可以创建更低延迟的连接，并且也像HTTP/2一样，通过仅仅阻塞部分流解决了包裹丢失这个问题，让连接在不同网络上建立变得更简单 － 这其实正是MPTCP想去解决的问题。

QUIC现在还只有Google的Chrome和它后台服务器上的实现，虽然有第三方库[libquic](https://github.com/devsisters/libquic)，但这些代码仍然很难在其他地方被复用。该协议也被IETF通信工作组引入了[草案](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01)。

<!-- 后一个section需要review -->