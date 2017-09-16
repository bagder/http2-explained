# 5. http2 konsepti

Peki http2 neyin üstesinden geldi? HTTPbis grubunun ne yapmak için ne gibi sınırları vardır?

Sınırlar aslında oldukça katıydı ve ekibin yenilik yapma kabiliyetine çok fazla engeller oluşturuyordu:

- http2, HTTP paradigmalarını korumak zorunda. Hala, istemcinin TCP üzerinden sunucuya istek gönderdiği bir protokoldür.

- http:// ve https:// URL'leri değiştirilemez. Bunun için yeni bir plan olamaz. Bu tür URL'leri kullanarak, çok büyük değişiklikler beklenemez.

- HTTP1 sunucularını ve istemcilerini proxyleştirebilmeliyiz.

- Proxy'ler http2 özelliklerini HTTP 1.1 istemcileri ile bire bir eşleyecek halde şekillenmelidir.

- Protokoldeki isteğe bağlı parçaları çıkarın veya azaltın. Bu gerçekten bir şart değil fakat her şeyin gerekli olduğu düşünüldüğünde, yeni hiçbir şey uygulayamayacağınız tuzağına düşülebilir.

- Artık ara bir sürüm yok. İstemcilerin ve sunucuların http2 ile uyumlu olduğu ya da olmadığı kararlaştırıldı. Protokolü genişletmek veya değişiklik yapmak için http3 gibi bir ihtiyaç doğmamalı.

## 5.1. Varolan URI şemaları için http2

Daha önce de belirtildiği gibi, varolan URI şemaları değiştirilemez, bu nedenle http2 bunları kullanmalıdır. Bugün HTTP 1.x için kullanılan her neyse, protokol http2'ye yükseltildiğinde de URI şemalarında bir değişiklik olmasına sebep olmayacak bir yola ihtiyacımız var.

HTTP 1.1, bunu yapmak için tanımlanmış bir yönteme sahiptir; yani sunucu, eski protokol üzerinden böyle bir istekte bulunulduğunda, yeni bir protokolü kullansa dahi, ek bir gidiş dönüş maliyeti ile bir yanıt göndermesini sağlayan Upgrade: başlığını içerir.

Bu gidiş geliş maliyeti, SPDY ekibinin kabul etmeyeceği bir şey değildi, ve SPFY'nin TLS'i uyguladıgının ardından yeni bir TLS uzantısı geliştirdiler. NPN(Next Protocol Negotiation) olarak adlandırılan bu uzantıda, sunucu istemciye hangi protokolleri sağladığını bildirir ve istemci daha sonra tercih ettiği protokolü kullanabilir.

## 5.2. https:// için http2

Http2'nin odak noktası, TLS üzerinde uygun davranmasını sağlamak olmuştur. Spdy TLS gerektirir ve http2'de TLS zorunluluğu için güçlü bir itme söz konusu, fakat ortak bir karar sağlanamadı, bu yüzden http2, isteğe bağlı olarak TLS ile birlikte oluşturuldu. Ancak günümüzün lider web tarayıcılarından ikisi, Mozilla Firefox ve Google Chrome, sadece TLS üzerinden http2 uygulayacaklarını açıkça ifade etmişlerdir.

Yalnızca TLS'yi seçme nedenleri, kullanıcıların gizliliği konusundaki çekinceleri ve TLS ile yapılan yeni protokollerin daha yüksek başarı oranına sahip olduğunu gösteren ölçümlerdir. Ayrıca 80 numaralı bağlantı noktasını aşan HTTP 1.1'dir ve bu bağlantı noktasında diğer tüm protokoller kullanıldığında bazı ortak alanlarda etkileşime girmeleri veya trafiği yok etmeleri gibi yaygın varsayımlar olmasıdır.

Zorunlu TLS konusu, toplantılarda titrek sesler çıkmasına neden olan, çok tartışma yaratan bir konudur. İyi mi yoksa kötü mü gibi. Bu sorunu bir HTTPbis katılımcısının karşısına çıkardığınızda bunun farkında olun!

Similarly, there's been a fierce and long-running debate about whether http2 should dictate a list of ciphers that should be mandatory when using TLS, or if it should perhaps blacklist a set, or if it shouldn't require anything at all from the TLS “layer” but leave that to the TLS working group. The spec ended up specifying that TLS should be at least version 1.2 and there are cipher suite restrictions.

Http2 TLS kullanırken zorunlu olması gereken şifrelerin bir listesini tutmalı mı, bazılarını kara listeye almalı mı, TLS'i katman olarak değiştirilmeli mi, TLS'den bir şey talep etmesi gerekip gerekmediğine ilişkin konularda uzun ve sıkı müzakereler oldu ve bu konularda tartışmalar sürüyor. Beyanname TLS'in en az 1.2 sürümü olmasını ve şifre paketi kısıtlamaları olduğunu belirledi.

## 5.3. TLS üzerinden http2 anlaşması

Next Protocol Negotiation (NPN), SPDY'yi TLS sunucularıyla pazarlık etmesi için kullanımış protokoldür. Uygun bir standart olmadığı için, IETF aracılığıyla oluşturuldu ve sonuç ALPN oldu: Application Layer Protocol Negotiation. SPDY istemcileri ve sunucuları hala NPN kullanırken, ALPN http2 tarafından kullanılmak üzere yükseltilmektedir. 

The fact that NPN existed first and ALPN has taken a while to go through standardization has led to many early http2 clients and http2 servers implementing and using both these extensions when negotiating http2. Ayrıca SPDY için NPN kullanılır ve birçok sunucu hem SPDY hem de http2 sunar, bu nedenle bu sunucularda hem NPN hem de ALPN'yi desteklemek mantıklıdır.

ALPN, öncelikle kimin hangi protokolü kullanacağına karar vermesinde NPN'den farklıdır. ALPN ile istemci sunucuya kendi tercih sırasına göre bir protokol listesi verir ve sunucu, NPN ile istemci son seçim yaparken, istediği protokolü tercih eder.

## 5.4. http:// için http2

Daha önce de belirtildiği gibi, düz metin HTTP 1.1 için http2 sunucuya bir yükseltme(Upgrade): başlığı sunar. Sunucu http2 konuşursa, "101 Anahtarlama"(“101 Switching”) durumu ile yanıt verir ve o andan itibaren bu bağlantı http2 olarak devam eder. Tabi ki bu yükseltme prosedürü tam bir ağ gidiş-dönüş süresine yol açar, ancak, http2 bağantısını daha uzun süre canlı tutmak ve onu tipik bir HTTP1 bagantısından daha fazla kullanmak mümkündür.

Bazı tarayıcıların sözcüleri http2'yi uygulamayacaklarını söyleseler de, Internet Explorer ekibi destekleyecelerini söyledi. curl ve diğer tarayıcı dışı birkaç istemci de açık metin(clear-text) http2'yi destekler.

Bugün, TLS olmadan http2'yi destekleyen hiçbir başlıca tarayıcı yok.
