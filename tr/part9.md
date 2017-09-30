# 9. Firefox'da http2

Firefox, taslakları yakından takip ediyor ve aylardır http2 test uygulamalarını sağlamıştır. Http2 protokolünün geliştirilmesi sırasında, istemciler ve sunucular, protokolün hangi taslak sürümü için testlerin uyguladıkları ile ilgili biraz can sıkıcı oldugu konusunda anlaşmıslardır.

## 9.1. First, make sure it is enabled

13 Ocak 2015 tarihinden itibaren piyasaya sürülen 35 sürümünden bu yana tüm Firefox sürümlerinde http2 desteği varsayılan olarak etkinleştirilmiştir.

Adres çubuğuna 'about: config' yazın ve "network.http.spdy.enabled.http2draft" adlı seçeneği arayın. * True * olarak ayarlandığından emin olun. Firefox 36, varsayılan olarak "true *" olarak ayarlanan "network.http.spdy.enabled.http2" adlı başka bir yapılandırma anahtarı ekledi. The latter one controls the “plain” http2 version while the first one enables and disables negotiation of http2-draft versions.  Her ikisi de Firefox 36'dan beri varsayılan olarak geçerlidir.

## 9.2. TLS-only(Sadece-TLS)

Firefox'un yalnızca TLS üzerinden http2 uyguladığını unutmayın. Firefox'da yalnızca https: // sitelerine giderken http2'yi göreceksiniz.

## 9.3. Transparent!

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Http2 kullanıldığını söyleyen herhangi bir UI öğesi yoktur. Bunu kolayca göremezsiniz. Bunu anlamanın bir yolu, "Web developer-> Network" özelliğini etkinleştirmek ve yanıt başlıklarını kontrol etmek ve sunucudan neyin geri geldiğini görmektir. Yanıt daha sonra "HTTP / 2.0" olur ve firefox, yukarıdaki ekran görüntüsünde gösterildiği gibi "X-Firefox-Spdy:" adlı kendi üstbilgisini ekler.

Http2 konuşurken Ağ aracında gördüğünüz başlıklar, http2'nin ikili biçiminden eski stil HTTP 1.x'e benzeyen başlıklara dönüştürülmüştür.

## 9.4. Http2 kullanımını görselleştir

Bir site http2 kullanıyorsa görselleştirmeye yardımcı olan Firefox eklentileri bulunur. Bunlardan biri [“HTTP/2 and SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/).
