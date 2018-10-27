# 5. http2 kavramları

Peki http2 ne yapar? HTTPbis gruplarının ne yapmak için ne gibi sınırları vardır?

Sınırlar aslında oldukça katıydı ve ekibin yenilik yapma kabiliyetine çok fazla engel koyuyordu:

- http2, HTTP paradigmalarını korumak zorundadır. Hala, istemcinin TCP üzerinden sunucuya istek gönderdiği bir protokoldür.

- http:// ve https:// URL'leri değiştirilemez. Bunun için yeni bir plan olamaz. Bu tür URL'leri kullanarak içerik miktarı, değişiklik beklemek için çok büyük.

- HTTP1 sunucuları ve istemcileri onlarca yıldır dolaşıyorlar, onları http2 sunucularına proxyleştirebilmeliyiz.

- Daha sonra, proxy'ler http2 özelliklerini HTTP 1.1 istemcilerine birebir eşleyemez olmalıdır.

- Protokoldeki isteğe bağlı parçaları çıkarın veya azaltın. Bu gerçekten bir şart değil, SPDY ve Google ekibinden gelen daha mantra idi. Her şeyin gerekli olduğu düşünüldüğünde, şimdi hiçbir şeyin uygulanamayacağı tuzağına düşürebilir.

- Artık küçük sürüm yok. İstemcilerin ve sunucuların http2 ile uyumlu olduğu ya da olmadığı kararlaştırıldı. Protokolü genişletmek veya değişiklik yapmak için bir ihtiyaç ortaya çıkarsa, http3 doğar. Http2'de artık küçük sürümler yok.

## 5.1. Varolan URI şemaları için http2

Daha önce de belirtildiği gibi, mevcut URI şemaları değiştirilemez, bu yüzden http2 mevcutları kullanmalıdır. Bugün HTTP 1.x için kullanıldıklarından, protokolü http2'ye yükseltmenin veya başka bir şekilde sunucudan eski protokoller yerine http2'yi kullanmasını isteyebilecek bir yola ihtiyacımız var.

HTTP 1.1, bunu yapmak için tanımlanmış bir yönteme sahiptir; yani Sunucu, eski protokol üzerinden böyle bir istekte bulunurken, yeni bir protokolü kullanarak ek bir gidiş dönüş ücreti ile bir yanıt göndermesini sağlayan Upgrade: başlığını içerir.

Bu gidiş dönüş cezası, SPDY ekibinin kabul etmeyeceği bir şey değildi ve yalnızca TLS üzerinden SPDY'yi uyguladıkları için müzakereyi önemli ölçüde -kısmen- sağlayan yeni bir TLS uzantısı geliştirdiler. Next Protocol Negotiation için NPN olarak adlandırılan bu uzantıyı kullanarak, sunucu istemciye hangi protokolleri sağladığını bildirir ve istemci daha sonra tercih ettiği protokolü kullanabilir.

## 5.2. https:// için http2

Http2'nin odak noktası, TLS üzerinde düzgün davranmasını sağlamak olmuştur. SPDY TLS gerektirir ve http2'de TLS'i zorunlu hale getirmek için güçlü bir itme olmuştur, ancak http2 isteğe bağlı olarak TLS ile birlikte gönderildiği için fikir birliğine varılamamıştır. Bununla birlikte, iki önde gelen uygulayıcı, http2'yi TLS üzerinden uygulayacaklarını açıkça belirtti: günümüzün önde gelen web tarayıcılarından ikisi olan Mozilla Firefox ve Google Chrome liderleri.

Yalnızca TLS'yi seçme nedenleri, kullanıcıların gizliliği konusundaki çekinceleri ve TLS ile yapılan yeni protokollerin daha yüksek başarı oranına sahip olduğunu gösteren  erken ölçümlere saygıyı içerir. Bunun nedeni 80 numaralı bağlantı noktasını aşan HTTP 1.1 olmasıdır ve bu bağlantı noktasında diğer tüm protokoller kullanıldığında bazı -orta kutularla- etkileşime giren veya trafiği yok eden yaygın varsayım olmasıdır.

Zorunlu TLS konusu, e-posta listelerinde ve toplantılarda titrek sesler çıkardı -iyi mi yoksa kötü mü- gibi. Bu sorunu bir HTTPbis katılımcısının karşısına çıkardığınızda bunun farkında olun!

Benzer şekilde, http2'nin TLS kullanırken zorunlu olması gereken şifrelerin bir listesini mi yoksa belki de bir seti kara listeye alması mı gerektiği konusunda veya TLS'den hiçbir şey talep etmemesi gerekip gerekmediğine ilişkin uzun süreli tartışmalar sürüyor, ya da belki de kara listeye alınmalı ya da TLS "katmanı" ndan hiçbir şey talep etmemeli, TLS çalışma grubuna bırakılmalıdır. Beyanname, TLS'nin en azından sürüm 1.2 olması gerektiğini ve şifre paketi kısıtlamaları olduğunu belirtti.

## 5.3. TLS üzerinden http2 anlaşması

Next Protocol Negotiation (NPN), SPDY'yi TLS sunucularıyla pazarlık etmesi için kullanılmış protokoldür. Uygun bir standart olmadığı için, IETF aracılığıyla oluşturuldu ve sonuç ALPN oldu: Application Layer Protocol Negotiation. SPDY istemcileri ve sunucuları hala NPN kullanırken, ALPN http2 tarafından kullanılmak üzere yükseltilmektedir. 

NPN'nin önce varolduğu ve ALPN'nin standardizasyona geçmesi biraz zaman aldığından, http2 müzakere ederken çok erken http2 istemcileri ve http2 sunucuları uygulamakta ve bu uzantıları kullanmaya başlamıştır. Ayrıca SPDY için NPN kullanılır ve birçok sunucu hem SPDY hem de http2 sunar, bu nedenle bu sunucularda hem NPN hem de ALPN'yi desteklemek mantıklıdır.

## 5.4. http:// için http2

Daha önce de belirtildiği gibi, düz metin HTTP 1.1 için http2 sunucuya bir yükseltme(Upgrade): başlığı sunar. Sunucu http2 konuşursa, "101 Anahtarlama"(“101 Switching”) durumu ile yanıt verir ve o andan itibaren bu bağlantı http2 olarak devam eder. Tabi ki bu yükseltme prosedürü tam bir ağ gidiş-dönüş süresine yol açar, ancak, http2 bağantısını daha uzun süre canlı tutmak ve onu tipik bir HTTP1 bagantısından daha fazla kullanmak mümkündür.

Bazı tarayıcıların sözcüleri http2'yi uygulamayacaklarını söyleseler de, Internet Explorer ekibi destekleyecelerini söyledi. curl ve diğer tarayıcı dışı birkaç istemci de açık metin(clear-text) http2'yi destekler.

Bugün, TLS olmadan http2'yi destekleyen hiçbir başlıca tarayıcı yok.