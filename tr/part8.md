# 8. http2 dünyası

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

Telekom ve diğer ağ operatörleri, örneğin ATIS Open Web Alliance'da, uydu, uçak ve benzeri ortamlarda hızlı bir web deneyimi sağlamak için önbellek, sıkıştırma ve diğer teknikler sunacaklarını söylüyorlar[need unencrypted
traffic](http://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf). http2, TLS kullanımını zorunlu kılmaz, bu nedenle şartları birleştirmemeliyiz.

Pek çok İnternet kullanıcısı, kullanıcıların gizliliğinin korunması gerekçesiyle TLS'in daha yaygın kullanılmasını tercih ettiğini belirtti.

Experiments have also shown that by using TLS, there is a higher degree of success than when implementing new plain-text protocols over port 80 as there are just too many middle boxes out in the world that interfere with what they would think is HTTP 1.1 if it goes over port 80 and might look like HTTP at times.

Deneyler ayrıca, TLS'yi kullanarak, bağlantı noktası 80 üzerinden yeni düz metin protokolleri uygularkenkinden daha yüksek bir başarı derecesine sahip olduğunu gösterdi; çünkü dünyada, HTTP1.1 olduğunu düşüneceklerine müdahale eden çok fazla orta kutu var 80 numaralı bağlantı noktasını aşıyor ve bazen HTTP gibi görünebilir.

TLS'i kullanan deneyler gosteriyor ki, 80 numaralı bağlantı noktasından yeni düz metin protokolleri uygularkenkinden daha yüksek bir başarı derecesi var çünkü 80 numaralı bağlantı noktasından geçerse HTTP1 olduğunu düşünerek çok fazla müdahale olabilir.?

Son olarak, tek bir bağlantı üzerinden HTTP2'nin çok katlı akışları sayesinde, normal tarayıcı kullanım örnekleri halen daha az TLS el sıkışmasıyla sonuçlanabilir ve böylece hala HTTP 1.1 kullanıldığında HTTPS'den daha hızlı performans gösterebilir.

### 8.4.5. "ASCII olmamak bir anlaşmazlıktır"

Evet, hata ayıklama ve izleme kolaylığı sağladığından açık bir şekilde protokolleri seviyoruz. Ancak metin tabanlı protokoller daha hataya eğilimli ve çok daha dönüştürme(parsing) problemlerine sahiptir.

If you really can't take a binary protocol, then you couldn't handle TLS and compression in HTTP 1.x either and its been there and used for a very long time.

### 8.4.6. "HTTP / 1.1'den daha hızlı değil"

Bu tabi ki daha hızlı olanı ölçmenin nasıl yapıldığı üzerine tartışmalara tabidir, ancak daha önce SPDY günlerinde daha fazla tarayıcı sayfası yüklendiğini kanıtlayan birçok test yapıldı (like ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) by people at University of Washington and ["Evaluating the Performance of SPDY-enabled Web Servers"](http://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) by Hervé Servy) ve bu tür denemeler de http2 ile tekrarlandı. Bu tür testlerin ve deneylerin yayınlanmaya başlaması için sabırsızlıkla bekliyorum. [basic first test made by httpwatch.com](http://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2), HTTP / 2'nin sözlerini tuttuğunu ima edebilir.

### 8.4.7. "Katman ihlalleri var"

Cidden, bu argümanınız bu mı? Katmanlar, küresel bir dinin kutsal dokunulmaz direkleri değildir ve eğer http2 yaparken birkaç gri alana girersek, verilen kısıtlamalar içinde iyi ve etkili bir protokol yapmakla ilgilenilmiştir.

### 8.4.8. "Birkaç HTTP / 1.1 eksikliğini gidermez"

Bu doğru. HTTP / 1.1 paradigmalarını sürdürmenin spesifik amacı ile birlikte, kalması gereken eski HTTP özellikleri de vardı. Sıklıkla korkulan çerezleri, yetkilendirme başlıklarını ve daha fazlasını içeren ortak başlıklar olduğu gibi. Fakat bu paradigmaların korunabilmesi le birlikte, protokol, temel parçaların bir yukseltmeye ihtiyaç duymdan tamamen değiştirilebilmesi veya yeniden yazılabilmesini mümkün kılar.

## 8.5. Http2 yaygın olarak kullanılacak mı?

Herhalde söylemek için henüz erken, fakat yine de tahmin edebilir ve burada yapacağım da budur.

On yıllarca süren ve yeni bir protokolün yaygınlaşması için örnek olarak naysayerler "IPv6'nın ne kadar iyi yapıldığına bakın" diyecek. http2 IPv6 değildir. Bu sıradan HTTP güncelleme mekanizmalarını ve port numaralarını ve TLS'yi kullanan TCP'nin üstünde bir protokoldür. Çoğu yönlendirici veya güvenlik duvarının hiç değişmesini gerektirmez.

Google, SPDY çalışmalarıyla dünyaya onu kanıtladı; bunun gibi yeni bir protokol tarayıcılar ve hizmetler tarafından çok sayıda uygulama ile oldukça kısa sürede kullanılabiliyor. Internet'te bugün SPDY'yi sunan sunucuların miktarı % 1 aralığında iken, bu sunucuların ele aldığı veri miktarı çok daha büyük. Bugün kesinlikle en popüler web sitelerinden bazıları SPDY'yi önriyor.

http2, SPDY ile aynı temel paradigmaları temel alarak, bir IETF protokolü olduğundan daha fazla konuşlandırılacağını söyleyebilirim. SPDY dağıtımı her zaman "bir Google protokolüdür" damgasının gerisinde kaldı.

Yayınlamanın arkasında birkaç büyük tarayıcı var. Firefox, Chrome, Safari, Internet Explorer ve Opera temsilcileri http2 özellikli tarayıcılar göndereceklerini ve  çalışma uygulamalarını gösterdiklerini belirttiler.

Google, Twitter ve Facebook dahil olmak üzere yakında http2 sunacak pek çok büyük sunucu operatörü var ve http2'nin yakında Apache HTTP Sunucusu ve nginx gibi popüler sunucu uygulamalarına eklendiğini görmek istiyoruz. H2o, potansiyelini gösteren http2 desteğine sahip yeni ve inanılmaz hızlı bir HTTP sunucusudur.

HAProxy, Squid ve Varnish de dahil olmak üzere en büyük proxy sunucularından bazıları, http2'yi destekleme niyetlerini ifade ettiler.

2015 yılına kadar, http2 olan trafik miktarı artıyor. Google, yaklaşık% 18 oranında gelen HTTP / 2'yi görürken, Eylül ayının başında Firefox 40'ın kullanımı tüm HTTP trafiğinin% 13'ünde, tüm HTTPS trafiğinin% 27'sinde gerçekleşti. Google'ın, http2 kullanım düzeylerini başka türlü olabildiğince düşük kılan diğer yeni protokol deneylerini de çalıştırdığı unutulmamalıdır (bkz. QUIC 12.1).
