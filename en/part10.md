# 10. http2 in Chromium

The Chromium team has implemented http2 and provided support for it in the dev and beta channel for a long time. Starting with Chrome 40, released on January 27th 2015, http2 is enabled by default for a certain amount of users. The amount started off really small and then increased gradually over time.

SPDY support will eventually be removed. In a blog post, the project announced in [February 2015](http://blog.chromium.org/2015/02/hello-http2-goodbye-spdy-http-is_9.html):

> “Chrome has supported SPDY since Chrome 6, but since most of the benefits are present in HTTP/2, it’s time to say goodbye. We plan to remove support for SPDY in early 2016”

## 10.1. First, make sure it is enabled

Enter “chrome://flags/#enable-spdy4" in your browser's address bar and click “enable” if it isn't already showing it as enabled.

## 10.2. TLS-only

Remember that Chrome only implements http2 over TLS. You will only ever see http2 in action with Chrome when going to https:// sites that offer http2 support.

## 10.3. Visualize HTTP/2 use

There are Chrome plugins available that helps visualize if a site is using HTTP/2. One of them is [“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Chrome's current experiments with QUIC (see section 12.1) dilute the HTTP/2 numbers somewhat.
