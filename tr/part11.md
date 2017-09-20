# 11. http2 in curl

[curl projesi](http://curl.haxx.se/), Eylül 2013'ten beri deneysel http2 desteği sağlıyor.

curl ruhu içinde, mümkün olduğunca http2'nin her yönünü desteklemeyi düşünüyoruz. curl sıklıkla bir test aracı ve web sitelerinde takla(poke on) atmanın yolu olarak kullanılır ve bunu http2 için de tutmak niyetindeyiz.

curl, http2 çerçeve katmanı işlevselliği için ayrı kütüphane [nghttp2](https://nghttp2.org/) kullanır. curl, nghttp2 1.0 veya üstünü gerektirir.

Şu anda linux'da curl ve libcurl her zaman HTTP / 2 protokol desteği etkin değildir.

## 11.1. HTTP 1.x look-alike

Dahili olarak, curl gelen HTTP2 üstbilgilerini HTTP 1.x stil üstbilgilerine dönüştürür ve kullanıcıya sunar; böylece mevcut HTTP'ye çok benzer görünürler. Bu, curl ve HTTP'yi bugün kullananlar için daha kolay bir geçiş sağlar. Benzer şekilde curl, giden üstbilgileri aynı stilde dönüştürür. Onları HTTP 1.x tarzında curl'e verin ve http2 sunucularıyla konuşurken bunları anında dönüştüreceksiniz. Böylece, kullanıcıların hat(wire) üzerinde gerçekten kullanılan belirli HTTP sürümleriyle uğraşmasına veya bakım yapmasına gerek kalmamaktadır.

## 11.2. Düz metin, güvensiz

curl, HTTP2'yi Standart TCP üzerinden Upgrade: başlığı üzerinden destekler. Bir HTTP isteği yaparsanız ve HTTP 2'yi sorarsanız, curl, sunucudan mümkünse http2 bağlantısını güncellemesini isteyecektir.

## 11.3. TLS ve bazı kütüphaneler

curl, TLS arka uç için geniş bir yelpazede farklı TLS kütüphanelerini destekler ve bu hala http2 desteği için geçerlidir.  Http2'nin uğruna TLS ile olan meydan okuma ALPN desteğidir ve bir ölçüde NPN desteğidir.

Hem ALPN hem de NPN desteği almak için curl'ü OpenSSL veya NSS'nin modern sürümlerine karşı oluşturun. GnuTLS veya PolarSSL'yi kullanarak NPN değil ALPN desteği elde edersiniz.

## 11.4. Komut satırı kullanımı

Curl'e http2'yi (düz metin veya TLS) kullanmasını söylemek için `--http2` seçeneğini (yani" tire çizgisi http2 ") kullanırsınız. curl hâlâ HTTP / 1.1 varsayılanıdır, bu nedenle HTTP2'yi istediğinizde ekstra seçenek gereklidir.

## 11.5. libcurl seçeneği

### 11.5.1 Etkin HTTP/2

Uygulamanız normal gibi https: // veya http: // URL'leri kullanıyor ancak libcurl'un http2 kullanmaya teşvik etmek için curl_easy_setopt'ın `CURLOPT_HTTP_VERSION` seçeneğini `CURL_HTTP_VERSION_2` olarak ayarlamalısınız. Daha sonra elinden gelen çabayı gösterebilir ve http2 yapabilir, aksi halde HTTP 1.1 ile çalışmaya devam eder.

### 11.5.2 Çoğullama

Libcurl mevcut davranışları büyük ölçüde korumaya çalıştığından, uygulamanız için [CURLMOPT_PIPELINING](http://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html) seçeneği ile HTTP / 2 çoğullama özelliğini etkinleştirmeniz gerekir. Aksi takdirde, her bağlantı için bir defada bir istek kullanmaya devam edecektir.

Another little detail to keep in mind is that if you ask for several transfers
at once with libcurl, using its multi interface, an applicaton can very well
start any number of transfers at once and if you then rather have libcurl wait
a little to add them all over the same connection rather than opening new
connections for all of them at once, you use the
[CURLOPT_PIPEWAIT](http://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) option
for each individual transfer you rather wait.

Akılda tutulması gereken diğer bir küçük ayrıntı da, bir seferde libcurl ile çoklu aktarım isterseniz, çoklu arayüzü kullanmak, bir uygulama aynı anda herhangi bir sayıda aktarmaya başlayabilir ve daha sonra libcurl'yı eklemek için biraz beklemek zorunda kalırsanız hepsinin aynı anda birden fazla bağlantı kurmasına değil, aynı bağlantıya her seferinde beklediğiniz her bir aktarım için [CURLOPT_PIPEWAIT] [CURLOPT_PIPEWAIT](http://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) seçeneğini kullanabilirsiniz.

### 11.5.3 Sunucu itme

libcurl 7.44.0 ve sonrası, HTTP / 2 sunucu itme özelliğini destekler. [CURLMOPT_PUSHFUNCTION](http://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html) seçeneği ile bir geri arama geri alma kurarak bu özelliğin avantajından yararlanabilirsiniz. Baskı uygulama tarafından kabul edilirse, başka herhangi bir aktarımda olduğu gibi, CURL kolay işleyici olarak yeni bir aktarım oluşturacak ve içeriği teslim edecektir.
