# 12. After http2

A lot of tough decisions and compromises have been made for http2. With http2 getting deployed there is an established way to upgrade into other protocol versions that work which lays the foundation for doing more protocol revisions ahead. It also brings a notion and an infrastructure that can handle multiple different versions in parallel. Maybe we don't need to phase out the old entirely when we introduce new?

http2 still has a lot of HTTP 1 “legacy” brought with it into the future because of the desire to keep it possible to proxy traffic back and forth between HTTP 1 and http2. Some of that legacy hampers further development and inventions. Perhaps http3 can drop some of them?

What do you think is still lacking in http?

## 12.1. QUIC

(This chapter was written in 2015, when QUIC was still in its early days. I
have decided to leave it like this to give the reader a glimpse of how we
viewed the future back then. See the next chapter for the 2022 take.)

Google's [QUIC](https://www.chromium.org/quic) (Quick UDP Internet Connections) protocol is an interesting experiment, performed much in the same style and spirit as they did with SPDY. QUIC is a TCP + TLS + HTTP/2 replacement implemented using UDP.

QUIC allows the creation of connections with much less latency, it solves packet loss to only block individual streams instead of all of them like it does for HTTP/2 and it makes creating connections over different network interfaces easy - thus also covering areas MPTCP is meant to solve.

QUIC is so far only implemented by Google in Chrome and their server ends and that code is not easily re-used elsewhere, even if there's a [libquic](https://github.com/devsisters/libquic) effort trying exactly that. The protocol has been brought as a [draft](https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) to the IETF transport working group.

## 12.2 HTTP/3

**QUIC** is no longer an acronym. It is the name of a transport protocol that
operates over UDP and is documented in four specifications: RFC 8999 to
RFC 9002. Note that what is described in 12.1 above is the original Google
QUIC protocol, and what is now made a standard is a different QUIC protocol
even though the name has been kept.

HTTP/3 is the new HTTP version in progress. It is designed to work over the
QUIC protocol.

For HTTP/3 and QUIC details, see [HTTP/3
explained](https://http3-explained.haxx.se/).
