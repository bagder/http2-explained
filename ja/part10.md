# 10. Chromiumにおけるhttp2

Chromiumチームはhttp2を実装していて、長い間dev、betaチャンネルでそのサポートを行っています。2015年1月27日にリリースされたChrome 40から、一部のユーザーにおいてhttp2がデフォルトで有効になりました。その数は最初は少なく設定されていましたが、時とともに徐々に増加しました。

SPDYサポートは削除される予定です。[2015年2月](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html)のブログでの発表によると:

> ”ChromeはSPDYをChrome 6からサポートしてきました。しかしそのほとんどの恩恵はHTTP/2からも得られることから、さようならをすることに決めました。SPDYを2016年の早い時期に削除する予定です。”

## 10.1. まず最初にhttp2が有効になっているか確かめてください

ブラウザーのアドレスバーに”chrome://flags/#enable-spdy4”と入力し、まだ有効になっていない場合は、”enable”をクリックします。

## 10.2. TLS限定

Chromeはhttp2をTLS上でのみ実装することを忘れないでください。Chromeではhttps://のhttp2をサポートするサイトでのみhttp2は動作します。

## 10.3. HTTP/2の使用を可視化する

サイトがhttp2を使用しているかどうかの可視化を手伝いするChromeプラグインがあります。それらの一つは[”HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin)です。

## 10.4. QUIC

現在行われているChromeによるQUIC（12.1を参照）の試験が、HTTP/2の使用率を幾分下げています。
