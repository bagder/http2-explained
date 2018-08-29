# 10. Chromium里的http2

Chromium团队并且很早之前就已经在dev和beta分支里面实现并支持了HTTP/2。从2015年1月27日发布的Chrome 40起，http2已经默认为一些用户启用该功能。虽然刚开始用户的数量会很少，但会慢慢增加。

Chrome 51移除了SPDY的支持来为http2铺路。在2016年2月的一篇[博客](https://blog.chromium.org/2016/02/transitioning-from-spdy-to-http2.html)里面有如下一段话：

> “在Chrome里有超过25%的资源是通过HTTP/2来传输的，而SPDY只有不到5%。考虑到如此大范围的采用，自5月15日，也就是HTTP/2 RFC的周年纪念日起，Chrome将不再支持SPDY。”

## 10.1. 首先，确保它已被启用

在地址栏里进入`chrome://flags/#enable-spdy4`，如果没有被enable的话，点击"enable"启用它。

## 10.2. TLS-only

请记住Chrome只在TLS上实现了http2。你只会在以`https://`做前缀的网站里得到http2的支持。

## 10.3. 图形化HTTP/2

有一些Chrome的插件可以图形化HTTP/2，比如[“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin)。

## 10.4. QUIC

Chrome正在试验QUIC（详情请看12.1），所以或多或少稀释了HTTP/2的份额。
