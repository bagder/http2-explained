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

Http2'nin TLS kullanırken zorunlu olması gereken şifrelerin bir listesini mi tutmalı, belki de bir seti kara listeye almalı ya da TLS'i katman olarak değiştirilmeli yada TLS'den hiç bir şey talep etmemesi gerekip gerekmediğine ilişkin konularda uzun ve sıkı müzakereler oldu.

## 5.3. TLS üzerinden http2 anlaşması

Next Protocol Negotiation (NPN), SPDY'yi TLS sunucularıyla pazarlık etmesi için kullanımış protokoldür. Uygun bir standart olmadığı için, IETF aracılığıyla oluşturuldu ve sonuç ALPN oldu: Application Layer Protocol Negotiation. SPDY istemcileri ve sunucuları hala NPN kullanırken, ALPN http2 tarafından kullanılmak üzere yükseltilmektedir.

The fact that NPN existed first and ALPN has taken a while to go through standardization has led to many early http2 clients and http2 servers implementing and using both these extensions when negotiating http2. Also, NPN is what's used for SPDY and many servers offer both SPDY and http2, so supporting both NPN and ALPN on those servers makes perfect sense.

ALPN differs from NPN primarily in who decides what protocol to speak. With ALPN, the client gives the server a list of protocols in its order of preference and the server picks the one it wants, while with NPN the client makes the final choice.

## 5.4. http2 for http://

As previously mentioned, for plain-text HTTP 1.1 the way to negotiate
http2 is by presenting the server with an Upgrade: header. If the server speaks
http2 it responds with a “101 Switching” status and from then on it speaks
http2 on that connection. Of course this upgrade procedure
costs a full network round-trip, but the upside is that it's generally possible to 
keep an http2 connection alive much longer and re-use it more than a typical HTTP1
connection.

While some browsers' spokespersons stated they will not implement this means
of speaking http2, the Internet Explorer team once expressed that they would -
although they have never delivered on that. curl and a few other non-browser
clients support clear-text http2.

Today, no major browser supports http2 without TLS.
