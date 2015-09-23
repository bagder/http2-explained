# 12. After http2

A lot of tough decisions and compromises have been made for http2. With http2 getting deployed there is an established way to upgrade into other protocol versions that work which lays the foundation for doing more protocol revisions ahead. It also brings a notion and an infrastructure that can handle multiple different versions in parallel. Maybe we don't need to phase out the old entirely when we introduce new?

http2 still has a lot of HTTP 1 “legacy” brought with it into the future because of the desire to keep it possible to proxy traffic back and forth between HTTP 1 and http2. Some of that legacy hampers further development and inventions. Perhaps http3 can drop some of them?

What do you think is still lacking in http?

## 12.1. QUIC

Google's [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) protocol is an interesting experiment, performed much in the same style and spirit as they did with SPDY. QUIC is a TCP + TLS + HTTP/2 replacement implemented using UDP.

QUIC allows the creation of connections with much less latency, it solves packet loss to only block individual streams instead of all of them like it does for HTTP/2 and it makes connections possible to be done over different network interfaces easily - thus also covering areas MPTCP is meant to solve.

A another optimization for latency, it is have a support of 0-RTT to avoid 2-3 round trip actually on TCP+TLS (used on HTTP2). Zero RTT will be a part of [TLS 1.3](https://tlswg.github.io/tls13-spec/#rfc.section.6.2.2) specifications

QUIC is so far only implemented by Google in Chrome and their server ends and that code is not easily re-used elsewhere, even if there's a [libquic](https://github.com/devsisters/libquic) or [QUIC Go](https://github.com/romain-jacotin/quic) effort trying exactly that. The protocol has been brought as a [draft](http://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) to the IETF transport working group.
