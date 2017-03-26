# 10. Chromium里的http2

Chromium团队并且很早之前就已经在dev和beta分支里面实现并支持了HTTP/2。从2015年1月27日发布的Chrome 40起，http2已经默认为一些用户启用该功能。虽然刚开始用户的数量会很少，但会慢慢增加。

SPDY的支持将会被移除。在2015年2月的一篇[博客](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html)里面有如下一段话：

> “Chrome自从6之后就开始支持SPDY，但既然现在它的优势已经被HTTP/2所取代, 现在是时候说再见了。我们计划在2016年早些时候移除对SPDY的支持”

## 10.1. 首先，确保它已被启用

在地址栏里进入`chrome://flags/#enable-spdy4`，如果没有被enable的话，点击"enable"启用它。

## 10.2. TLS-only

请记住Chrome只在TLS上实现了http2。你只会在以`https://`做前缀的网站里得到http2的支持。

## 10.3. 图形化HTTP/2

有一些Chrome的插件可以图形化HTTP/2，比如[“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin)。

## 10.4. QUIC

Chrome正在试验QUIC（详情请看12.1），所以或多或少稀释了HTTP/2的份额。
