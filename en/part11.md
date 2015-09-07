# 11. http2 in curl

The curl project has been providing experimental http2 support since September 2013.

In the spirit of curl, we intend to support just about every aspect of http2 that we possibly can. curl is often used as a test tool and tinkerer's way to poke on web sites and we intend to keep that up for http2 as well.

curl uses the separate library [nghttp2](https://nghttp2.org/) for the http2 frame layer functionality.

## 11.1. HTTP 1.x look-alike

Internally, curl will convert incoming http2 headers to HTTP 1.x style headers and provide them to the user, so that they will appear very similar to existing HTTP. This allows for an easier transition for whatever is using curl and HTTP today. Similarly curl will convert outgoing headers in the same style. Give them to curl in HTTP 1.x style and it will convert them on the fly when talking to http2 servers. This also allows users to not have to bother or care very much with which particular HTTP version that is actually used on the wire.

## 11.2. Plain text, insecure

curl supports http2 over standard TCP via the Upgrade: header. If you do a HTTP request and ask for HTTP 2, curl will ask the server to update the connection to http2 if possible.

## 11.3. TLS and what libraries

curl supports a wide range of different TLS libraries for its TLS back-end, and that is still valid for http2 support. The challenge with TLS for http2's sake is the APLN support and to some extent NPN support.

Build curl against modern versions of OpenSSL or NSS to get both ALPN and NPN support. Using GnuTLS or PolarSSL you will get ALPN support but not NPN.

## 11.4. Command line use

To tell curl to use http2, either plain text or over TLS, you use the --http2 option (that is “dash dash http2”). curl still defaults to HTTP/1.1 so the extra option is necessary when you want http2.

## 11.5. libcurl options

Your application would use https:// or http:// URLs like normal, but you set curl_easy_setopt's CURLOPT_HTTP_VERSION option to CURL_HTTP_VERSION_2 to make libcurl attempt to use http2. It will then do a best effort and do http2 if it can, but otherwise continue to operate with HTTP 1.1.
