# 8. Bir http2 dünyası

Peki, http2 kabul edildiğinde işler nasıl gözükecek? Kabul edilecek mi?

## 8.1. Http2 sıradan insanları nasıl etkiler?

http2 henüz geniş ölçüde ne kullanıldı ne de uygulandı. Her şeyin nasıl gerçekleşeceğini kesin olarak bilemeyiz fakat SPDY'nin nasıl kullanıldığını gördük ve o andaki geçmiş ve güncel deneylere dayanan bazı tahminler ve hesaplamalar yapabiliriz.

http2, gerekli ağ gidiş-dönüş sayısını azaltır ve istenmeyen satır başını engelleme problemini çoğullama ve hızlı atılma tamamen önler.

Bugünün en yaygın kullanılan sitelerinde bile büyük miktarda paralel akışa izin verir.

Akışlar üzerinde doğru olarak kullanılan önceliklerle, istemcilerin daha az önemli olan veriden önce aslında daha önemli verileri alması ihtimali daha yüksektir.

Bütün bunlar bir araya getirildiğinde, şansın çok daha yüksek olduğunu ve bunun daha hızlı sayfa yüklemelerine ve daha duyarlı web sitelerine yol açacağını söyleyebilirim. Kısaca: daha iyi web deneyimi.

Ne kadar hızlı  bir sürede ne kadar iyileştirme göreceğimizi, henüz söyleyebileceğimizi sanmıyorum. Birincisi, teknoloji hala çok erken ve istemcilerin ve sunucuların, bu yeni protokolün sunduğu tüm güçlerden gerçekten yararlanmak için uygulamaların düzeltildiğini görmeye bile başlamadık.

## 8.2. Http2 web gelişmelerini nasıl etkiler?

Yıllar içinde web geliştiricileri ve web geliştirme ortamları HTTP 1.1 ile ilgili sorunları çözmek için çeşitli araçlar(toolbox) oluşturmuşlardır; hatırlatmak gerekirse, http2'nin aleyhinde haklı neden olarak bunlardan bazılarını özetledim.

Araçlar ve geliştiricilerin düşünmeden kullandıkları bu geçici çözümlerin büyük bir kısmı büyük olasılıkla http2'nin performansını düşürür veya en azından yeni süper güçlerinden yararlanmaz. İçerilme ve püskürtme http2 ile yapılmamalıdır. Muhtemelen daha az bağlantı kullanacağı için, Sharding? http2 için zararlıdır.

web sitelerinin ve web geliştiricilerinin en azından kısa vadede geliştirmeye ve konuşlandırmaya ihtiyacı olması bir problemdir ki bu, hem HTTP1.1 hem de http2 istemcilerine sahip olması ve tüm kullanıcılar için maksimum performans sağlaması açısından iki farklı ön uç sunmak zorunda kalmadan zorlu olabilir.

Yalnız bu nedenlerden dolayı, http2'nin tam potansiyeline ulaşıldığına karar vermemizin biraz zaman alacağını düşünüyoruz.

## 8.3. http2 uygulamaları

Böyle bir belgede belirli uygulamaları belgelemeye çalışmak, tamamen boş bir çabaydı ve başarısızlığa mahkûm edildi ve yalnızca kısa sürede zaman aşımına uğramıştı. Bunun yerine durumu daha geniş anlamda açıklayacağım ve okuyucuları http2'nin web sitesine  [uygulamaların listesi](https://github.com/http2/http2-spec/wiki/Implementations) yönlendireceğim.

Daha önce çok fazla sayıda uygulama vardı ve bu miktar http2 çalışması sırasında zamanla arttı. Yazı yazarken 40'dan fazla uygulama listelendi ve bunların çoğu son sürümü uyguluyor.

### 8.3.1 Tarayıcılar
Firefox yeni tasarimlarin ustunde bir internet tarayicisi olmustur, twitter uzak kalmistır ve servislerini http2 nin ustunde sunmuştur.? Google, hizmetlerini çalıştıran birkaç test sunucusunda http2 desteği sunmak için Nisan 2014'te başladı ve Mayıs 2014'ten bu yana Chrome'un geliştirme sürümlerinde http2 desteği sağladı. Microsoft, bir sonraki Internet Explorer sürümü için http2 desteğiyle bir teknik önizleme gösterdi. Hem Safari (iOS 9 ve Mac OS X El Capitan ile birlikte) hem Opera hem de http2'yi destekleyeceklerini söyledi.

### 8.3.2 Sunucular

Http2'nin zaten çok sayıda sunucu uygulaması var.

Popüler Nginx sunucusu 22 Eylül 2015'te piyasaya sürülen [1.9.5](https://www.nginx.com/blog/nginx-1-9-5/)'den beri http2 desteği sunmaktadır(SPDY modülünün yerine geçer; böylece her ikisi de aynı sunucuda çalışamazlar).

Apache'nin httpd sunucusu, 9 Ekim 2015'te çıkan 2.4.17'den bu yana http2 modülü [mod_http2](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) içeriyor.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](http://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/),
[Caddy](http://caddyserver.com/) and
[LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview)
have all released http2 capable servers.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](http://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/),
[Caddy](http://caddyserver.com/) ve
[LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview); bu yetenekli sunucuları hepsi http2'yi destekledi.

### 8.3.3 Diğerleri

curl ve libcurl, güvensiz http2'yi ve farklı TLS kütüphanelerini destekler.

Wireshark http2'yi destekliyor. Http2 ağ trafiğini analiz etmek için mükemmel bir araç.

## 8.4. Http2'nin ortak eleştirileri

Bu protokolün geliştirilmesi sırasında tartışmalar vardır ve tabi ki bu protokolün tamamen hatalı olduğuna inanılan belli bir miktarda insanlar var. Yaygın olan birkaç şikayetten bahsetmek ve onlara karşı oluşan argümanlardan bahsetmek istedim:

### 8.4.1. "Protokol, Google tarafından tasarlanmış veya üretilmiştir"

Dünyanın Google tarafından daha da bağımlı veya denetime tabi tutulduğunu ima eden önyargılar de vardır. Bu doğru değildir. Protokol, protokollerin 30 yılı aşkın bir süredir geliştirildiği şekilde IETF içinde geliştirildi. Bununla birlikte, hepimiz, Google'ın bu şekilde yeni bir protokol kurulmasının mümkün olduğunu kanıtlayan SPDY ile, yaptığı etkileyici çalışmaları tanıklık ettik ve bu kazanımlar ile neler yapılabileceğini gösteren numaralar sağladığını da kabul ediyoruz

Google has publicly [announced](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) that they will remove support for SPDY and NPN in Chrome in 2016 and they urge servers to migrate to HTTP/2 instead.

Google, 2016'da Chrome'da SPDY ve NPN'ye olan desteği kaldıracaklarını kamuoyuna ilan etti (https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) ve sunucuları geçiş yapılması için teşvik ediyorlar. Sunucularınızı HTTP / 2 olarak ayarlayın.

### 8.4.2. "Protokol yalnızca tarayıcılar için yararlıdır"

Bu doğru. Http2 gelişiminin ardındaki ana sürücülerden biri, HTTP boru hattının kurulumudur. Kullanım durumunuz başlangıçta boruhattı imkânına ihtiyaç duymuyorsa, http2 sizin için bir şey yapmaz. Protokoldeki tek gelişme kesinlikle kesinlikle bu değildir ama büyük bir gelişmedir.

Hizmetler, güç ve yetenekleri tam anlamıyla fark ettirmeye başladığında, tek bir bağlantı üzerinden birden çok akış getiriyor, http2'nin daha fazla uygulama kullanımını göreceğimizden şüphe yoktur.

Small REST APIs and simpler programmatic uses of HTTP 1.x may not find the step to http2 to offer very big benefits. But also, there should be very few downsides with http2 for most users.

Küçük REST API'leri ve HTTP 1.x'in daha basit programlı kullanımı, http2 için çok büyük yararlar sağlamak için bir adım bulamayabilir. Ancak aynı zamanda çoğu kullanıcı http2 ile ilgili çok az olumsuzluğa sahip olmalıdır.

### 8.4.3. "Protokol yalnızca büyük siteler için yararlıdır"

Not at all. The multiplexing capabilities will greatly help to improve the experience for high latency connections that smaller sites without wide geographical distributions often offer. Large sites are already very often faster and more distributed with shorter round-trip times to users.

Hepsi değil. Çoklama yetenekleri, geniş coğrafi dağılımlara sahip olmayan daha küçük sitelerin genellikle sundukları yüksek gecikme süresin ve bağlantı deneyimini iyileştirmeye yardımcı olacaktır. Büyük siteler zaten sıklıkla daha hızlıdır ve kullanıcılara daha kısa dolaşma süreleri ile daha fazla performans sağlarlar.

### 8.4.4. "TLS kullanımı yavaşlatıyor"

Bu, bir dereceye kadar doğru olabilir. TLS el sıkışması biraz ekstra para harcatabilir, ancak TLS için gereken gidişatları azaltmaya yönelik mevcut ve devam eden çabalar vardır. Düz metin yerine wire-hat? üzerinde TLS yapmak için yapılan ek yük çaba önemsiz değildir, daha fazla CPU ve güç, güvenli olmayan bir protokol ile aynı trafik modelinde harcanır. Ne kadar ve ne gibi bir etkiye sahip olacağı düşünülür. Bir bilgi kaynağı için [istlsfastyet.com](https://istlsfastyet.com/)'a bakın.

Telecom and other network operators, for example in the ATIS Open Web
Alliance, say that they [need unencrypted
traffic](http://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf)
to offer caching, compression and other techniques necessary to provide a fast
web experience over satellite, in airplanes and similar.  http2 does not make
TLS use mandatory so we shouldn't conflate the terms.

Many Internet users have expressed a preference for TLS to be used more widely and we should help to protect users' privacy.

Experiments have also shown that by using TLS, there is a higher degree of success than when implementing new plain-text protocols over port 80 as there are just too many middle boxes out in the world that interfere with what they would think is HTTP 1.1 if it goes over port 80 and might look like HTTP at times.

Finally, thanks to http2's multiplexed streams over a single connection, normal browser use cases still could end up doing substantially fewer TLS handshakes and thus perform faster than HTTPS would when still using HTTP 1.1.

### 8.4.5. “Not being ASCII is a deal-breaker”

Yes, we like being able to see protocols in the clear since it makes debugging and tracing easier. But text based protocols are also more error prone and open up for much more parsing and parsing problems.

If you really can't take a binary protocol, then you couldn't handle TLS and compression in HTTP 1.x either and its been there and used for a very long time.

### 8.4.6. “It isn't any faster than HTTP/1.1”

This is of course subject to debate and discussions on how to measure what faster means, but already in the SPDY days many tests were made that proved faster browser page loads (like ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) by people at University of Washington and ["Evaluating the Performance of SPDY-enabled Web Servers"](http://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) by Hervé Servy) and such experiments have been repeated with http2 as well. I'm looking forward to seeing more such tests and experiments getting published. A [basic first test made by httpwatch.com](http://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) might imply that HTTP/2 holds its promises.

### 8.4.7. “It has layering violations”

Seriously, that's your argument? Layers are not holy untouchable pillars of a global religion and if we've crossed into a few gray areas when making http2 it has been in the interest of making a good and effective protocol within the given constraints.

### 8.4.8. “It doesn't fix several HTTP/1.1 shortcomings”

That's true. With the specific goal of maintaining HTTP/1.1 paradigms there were several old HTTP features that had to remain. Such as the common headers that also include the often dreaded cookies, authorization headers and more. But by the upside of maintaining these paradigms is that we got a protocol that is possible to deploy without an inconceivable amount of upgrade work that requires fundamental parts to be completely replaced or rewritten. Http2 is basically just a new framing layer.

## 8.5. Will http2 become widely deployed?

It is too early to tell for sure, but I can still guess and estimate and that's what I'll do here.

The naysayers will say “look at how good IPv6 has done” as an example of a new protocol that's taken decades to just start to get widely deployed. http2 is not an IPv6 though. This is a protocol on top of TCP using the ordinary HTTP update mechanisms and port numbers and TLS etc. It will not require most routers or firewalls to change at all.

Google proved to the world with their SPDY work that a new protocol like this can be deployed and used by browsers and services with multiple implementations in a fairly short amount of time. While the amount of servers on the Internet that offer SPDY today is in the 1% range, the amount of data those servers deal with is much larger. Some of the absolutely most popular web sites today offer SPDY.

http2, based on the same basic paradigms as SPDY, I would say is likely to be deployed even more since it is an IETF protocol. SPDY deployment was always held back a bit by the “it is a Google protocol” stigma.

There are several big browsers behind the roll-out. Representatives from Firefox, Chrome, Safari, Internet Explorer and Opera have expressed they will ship http2 capable browsers and they have shown working implementations.

There are several big server operators that are likely to offer http2 soon, including Google, Twitter and Facebook and we hope to see http2 support soon get added to popular server implementations such as the Apache HTTP Server and nginx. H2o is a new blazingly fast HTTP server with http2 support that shows potential.

Some of the biggest proxy vendors, including HAProxy, Squid and Varnish have expressed their intentions to support http2.

All through-out 2015, the amount of traffic that is http2 has been increasing. In early September, Firefox 40 usage was at 13% out of all HTTP traffic and 27% out of all HTTPS traffic, while Google see roughly 18% incoming HTTP/2. It should be noted that Google runs other new protocol experiments as well (see QUIC in 12.1) which makes the http2 usage levels lower than it could otherwise be.

