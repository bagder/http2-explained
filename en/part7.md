# 7. Extensions

The protocol mandates that a receiver must read and ignore all unknown frames using unknown frame types. Two parties can thus negotiate use of new frame types on a hop-by-hop basis, and those frames aren't allowed to change state and they will not be flow controlled.

The subject of whether http2 should allow extensions at all was debated at length during the time the protocol was developed with opinions swinging for and against. After draft-12 the pendulum swept back one last time and extensions were allowed again.

Extensions are then not part of the actual protocol but will be documented outside of the core protocol spec. Already at this point, there are two frame types that have been discussed for inclusion in the protocol that probably will be the first frames sent as extensions. I'll still describe them here just because of their popularity and previous state as “native” frames:

## 7.1. Alternative Services

With http2 getting adopted, there are reasons to suspect that TCP connections will be much lengthier and be kept alive much longer than HTTP 1.x connections have been. A client should be able to do a lot of what it wants with a single connection to each host/site and that single one could then potentially be open for quite some time.

This will affect how HTTP load balancers work and there may come situations when a site wants to advertise and suggest that the client connects to another host. It could be for performance reasons but also if a site is being taken down for maintance and similar.

The server will then send the [Alt-Svc: header](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07) (or ALTSVC frame with http2) telling the client about an alternative service. Another route to the same content, using another service, host and port number.

A client is then meant to attempt to connect to that service asynchronously and only use the alternative if that works fine.

### 7.1.1. Opportunistic TLS

The Alt-Svc header allows a server that provides content over http://, to inform the client that the same content is also available over a TLS connection.

This is a somewhat debated feature. Such a connection would do unauthenticated TLS and wouldn't be advertized as “secure” anywhere, wouldn't use any padlock in the UI or in fact in no way tell the user that it isn't plain old HTTP, but this is still opportunistic TLS and some people are very firmly against this concept.

## 7.2. Blocked

A frame of this type is meant to be sent exactly once by an http2 party when
it has data to send off but flow control forbids it to send any data. The idea
being that if your implementation receives this frame you know your
implementation has messed up something and/or you're getting less than perfect
transfer speeds because of this.

A quote from draft-12, before this frame was moved out to become an extension:

> “The BLOCKED frame is included in this draft version to facilitate experimentation.  If the results of the experiment do not provide positive feedback, it could be removed”

