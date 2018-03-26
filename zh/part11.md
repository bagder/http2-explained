# 11. curl中的http2

[curl项目](https://curl.haxx.se/)从2013年9月就开始对http2提供实验性的支持。

为了遵从curl的要旨，我们尽可能全方位地支持http2。curl通常被用作一个网站连接测试工具，希望这项使命也能在http2上被得以延续。

curl使用一个叫做[nghttp2](https://nghttp2.org/)的库来提供http2帧层的支持。curl依赖于nghttp2 1.0以上版本。

请注意当前linux curl和libcurl并没有默认启用对HTTP/2协议的支持。

## 11.1. 跟HTTP 1.x非常相似

curl会在内部把收到的http2头部转换为HTTP1.x风格的头部再呈现给用户，这样一来，它们就和目前的HTTP非常类似。这也使得无论是用curl还是HTTP，转换都非常容易。<!-- TOREVIEW -->类似地，curl会用相同的方式对发出的HTTP头部做转换，即发给curl的HTTP 1.x风格头部会在被发送到http2服务器之前完成转换。这使得户无需关心底层到底使用的是哪个版本的HTTP协议。

## 11.2. 不安全的纯文本

curl通过升级头部支持基于标准TCP的http2. 当发起一个使用http2的HTTP请求，如果可能，curl会请求服务器把连接升级到http2.

## 11.3. TLS和相关库

curl可以使用许多不同TLS的底层库来提供TLS支持，http2也得这样。TLS兼容http2的挑战来自于对APLN以及一些NPN扩展的支持。

基于最新版本的OpenSSL或NSS编译curl可以同时获得ALPN和NPN支持。而使用GnuTLS或PolarSSL只能得到ALPN。

## 11.4. 命令行中使用

无论是用纯文本还是通过TLS，必须使用`--http2`参数来让curl使用http2。在未使用该参数的默认情况下，curl会使用HTTP/1.1。

## 11.5. libcurl参数

### 11.5.1 启用HTTP/2

应用程序和从前一样使用`https://`或者`http://`风格的URL，但你可以通过将`curl_easy_setopt`的`SURLOPT_HTTP_VERSION`参数设置为`CURL_HTTP_VERSION_2`来使libcurl尝试使用http2。它将优先尽可能地使用http2，如果不行的话，会继续使用HTTP 1.1。

### 11.5.2 多路复用

正如libcurl想尽可能量维持以前的用法，你需要通过[CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html)参数为你的程序启用HTTP/2多路复用功能。不然的话，它会保持一个连接只发送一个请求。

另一个需要注意的小细节是，当你通过libcurl同时请求多个传输的时候，请使用多接口模式。这样能使应用程序能同时启用任意数量的传输。如果你宁愿让libcurl等待也要把它们放到同一个连接来传输的话，请使用[CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html)参数。

### 11.5.3 服务器推送

libcurl 7.44.0及其后续版本开始支持HTTP/2服务器推送功能。你可以通过在[CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html)参数中设定一个推送回调来激活该功能。如果应用程序接受了该推送，它将为CURL建立一个新的传输，以便接受内容。<!-- 最后一句需要review -->

