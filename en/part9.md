# 9. http2 in Firefox

Firefox has been tracking the drafts very closely and has provided http2 test implementations for many months. During the development of the http2 protocol, clients and servers have to agree on what draft version of the protocol they implement which makes it slightly annoying to run tests. Just be aware so that your client and server agree on what protocol draft they implement.

## 9.1. First, make sure it is enabled

In all Firefox versions starting with version 36, released Februrary 24th 2015, http2 is enabled by default.

## 9.2. TLS-only

Remember that Firefox only implements http2 over TLS. You will only ever see http2 in action with Firefox when going to https:// sites that offer http2 support.

## 9.3. Transparent!

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

There is no UI element anywhere that tells that you're talking http2. You just can't tell that easily. One way to figure it out, is to enable “Web developer->Network” and check the response headers and see what you got back from the server. The response is then “HTTP/2.0” something and Firefox inserts its own header called “X-Firefox-Spdy:” as shown in the screenshot above.

The headers you see in the Network tool when talking http2 have been converted from http2's binary format into the old-style HTTP 1.x look-alike headers.

## 9.4. Visualize http2 use

There are Firefox plugins available that help visualize if a site is using http2. One of them is [“HTTP/2 and SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/http2-indicator/).
