# 7. Extensions

The http2 protocol mandates that a receiver must read and ignore all unknown frames (those with an unknown frame type). Two parties can negotiate the use of new frame types on a hop-by-hop basis, but those frames aren't allowed to change state and they will not be flow controlled.

The subject of whether http2 should allow extensions at all was debated at length during the protocol's development with opinions swinging for and against. After draft-12 the pendulum swung back one last time and extensions were ultimately allowed.

Extensions are not part of the actual protocol but will be documented outside of the core protocol spec. There are already two frame types that have been discussed for inclusion in the protocol that will probably be the first frames sent as extensions. I'll describe them here because of their popularity and previous state as “native” frames:

## 7.1. Alternative Services

With the adoption of http2, there are reasons to suspect that TCP connections will be much lengthier and be kept alive much longer than HTTP 1.x connections have been. A client should be able to do a lot of what it wants with a single connection to each host/site, and that connection could potentially be open for quite some time.

This will affect how HTTP load balancers work and there may arise situations when a site wants to suggest that the client connect to another host. It could be for performance reasons, or if a site is being taken down for maintenance, etc.

The server will send the [Alt-Svc: header](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07) (or ALTSVC frame with http2) telling the client about an alternative service: another route to the same content, using another service, host, and port number.

A client should then attempt to connect to that service asynchronously and only use the alternative if the new connection succeeds.

### 7.1.1. Opportunistic TLS

The Alt-Svc header allows a server that provides content over http:// to inform the client that the same content is also available over a TLS connection.

This is a somewhat debatable feature. Such a connection would do unauthenticated TLS and wouldn't be advertized as “secure” anywhere, wouldn't use any padlock in the UI, and in fact there is no way to tell the user that it isn't plain old HTTP, but this is still opportunistic TLS and some people are very firmly against this concept.

## 7.2. Blocked

A frame of this type is meant to be sent exactly once by an http2 party when
it has data to send off but flow control forbids it to send any data. The idea
is that if your implementation receives this frame you know you
have messed up something and/or you're getting less than perfect
transfer speeds.

A quote from draft-12, before this frame was moved out to become an extension:

> “The BLOCKED frame is included in this draft version to facilitate experimentation.  If the results of the experiment do not provide positive feedback, it could be removed”

