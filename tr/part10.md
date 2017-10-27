# 10. Chromium'da http2

Chromium ekibi http2'yi uygulamaya uyguladı ve uzun süredir dev ve beta kanalında destek verdi. 27 Ocak 2015'te yayınlanan Chrome 40 ile başlayarak, http2 varsayılan olarak belirli bir kullanıcı grubu için etkinleştirilmiştir. Bu miktar zamanla kademeli olarak arttı.

SPDY desteği sonunda ortadan kaldırılacaktır. Bir blog yazısında, şu şekilde ifade edilmiştir: [Şubat 2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> “"Chrome, Chrome 6'dan bu yana SPDY'yi destekledi, ancak avantajlarının çoğunun HTTP / 2'de mevcut olması nedeniyle, vedalaşma zamanı. 2016'nın başında SPDY'ye yönelik desteği kaldırmayı planlıyoruz "”

## 10.1. İlk olarak, etkinleştirildiğinden emin olun

Tarayıcınızın adres çubuğuna “chrome://flags/#enable-spdy4" yazın ve etkinleştirilmiş olarak göstermiyorsa "etkinleştir" seçeneğini tıklayın.

## 10.2. TLS-only(Sadece-TLS)

Firefox'un yalnızca TLS üzerinden http2 uyguladığını unutmayın. Firefox'da yalnızca https: // sitelerine giderken http2'yi göreceksiniz.

## 10.3. Http2 kullanımını görselleştir

Bir site http2 kullanıyorsa görselleştirmeye yardımcı olan Firefox eklentileri bulunur. Bunlardan biri [“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Chrome'un QUIC ile gerçekleştirdiği deneyler (see section 12.1) HTTP/2 çalışmalarını etkiler.
