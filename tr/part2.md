# 2. Günümüzde HTTP 

HTPP 1.1, internette neredeyse herşeyde kullanılan bir protocol haline geldi. Büyük yatırımlar protocolde yapılır  ve  bundan altyapı avantaj elde eder öyle ki artık genellikle günümüzde işleri(web işlerini ) çalışır hale getirmek ,yeni birşey üzerinden çalıştırmaktansa,HTTP üzerinde daha kolaydır.

## 2.1 HTTP 1.1 Devasadır

HTTP oluşturulduğunda ve dünyaya yayıldığında,muhtemelen oldukça basit ve karmaşıksız bir protokol olarak algılanmış olsa da zaman bunun doğru olmadığını ispatlamıştır. RFC 1945 HTTP 1.0'i, 1996 yılında, 60 sayfa tanımlamayla yayınlanmıştır. RFC2616 HTTP 1.1'i ise yalnızca 3 sene sonra 1999 da yayınlanmıştır ve önemli ölçüde artiş göstererek 176 sayfaya çıkmıştır. Bununla birlikte, IETF bu spektrumdaki güncelleme üzerinde çalışırken bölünmüş ve altı belgeye dönüştürülmüştür(RFC7230 ve ailesi). HTTP 1.1 büyüktür ve sayısız ayrıntı, incelik ve çok sayıda isteğe bağlı bölüm içermektedir.

## 2.2 Seçenekler dünyası

HTTP 1.1 in  bir çok detaya sahip yapısı ve sonraki eklentiler için de kullanılabilen seçenekleri, neredeyse hiç bir uygulamanın hiç bir zaman hiç bir şeyin uygulayamıcağı bir yazılım ekosistemi oluşturmuştur ve "hiçbirşey" kavramının tam olarak ne olduğunu söylemek mümkün değil. Bu, özelliklerin çok az sayıda olmasına ve çok az yararlanıldığı bir duruma neden oldu.

Daha sonraları, sunucu ve istemciler bu tür özellikleri daha çok kullanmaya başladığında, bu "birlikte çalışabilirlik"(interoperability) sorununa neden oldu. HTTP boruhattı, bu tarz özelliklere birincil örnektir. 

## 2.3 TCP’nin yetersiz kullanımı

HTTP 1.1, TCP'nin sunduğu tüm gücü ve performansı tam olarak kullanmanın zor bir dönemini yaşamaktadır.HTTP istemcileri ve tarayıcılar,sayfa yükleme sürelerini azaltan çözümler bulmak için çok yaratıcı olmalıdırlar.

 Yıllar boyunca paralel olarak devam eden diğer girişimler TCP’nin bu kadar kolay değiştirilmediğini doğruladı ve bu yüzden hem TCP hem de protokolleri geliştirmeye çalışmaya devam ediyoruz.

Basitce söylemek gerekirse, TCP'nin daha fazla veri göndermek veya almak ve boş sürelerden kaçınmak için daha iyi değerlendirilebilir. Sıradaki başlıklarda bu eksikliklerin bazıları vurgulanacaktır. 

## 2.4 Aktarım boyutları ve nesne sayısı

Bugün Webteki en popüler sitelerin bazılarına ve bunların ön sayfalarını indirmek için neye ihtiyaç olduğuna bakıldığında net bir model ortaya çıkıyor. Yıllar geçtikce alınan veri miktarı kademeli olarak 1.9MB'ın üzerine çıkmıştır.Burda daha önemli olan şey, her sayfayı görüntülemek için ortalama olarak yüzü aşkın bireysel kaynağa ihtiyaç duyulmasıdır.

Aşağıdaki grafiğin gösterdiği gibi, trend bir süredir devam etmektedir ve yakın zamanda bunun değişeceğine dair hiç bir gösterge yoktur.
Grafik toplam aktarım boyutunun büyümesini(yeşil olan) ve dünyadaki en populer web sitelerine gelen ortalama istek sayısını(kırmızı olan) göstermektedir.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Gecikme öldürür

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1'in gecikmeye karşı çok hassas olması, kısmen HTTP boruhattının buyuk bir kullanıcı oranına sahip olması ile ilgilidir.

Geçtiğimiz birkaç yılda kullanıcılara mevcut bant genişliğinde büyük bir artış göstersede, gecikmeyi azaltma yolunda aynı seviyede bir gelişmeye rastlamadık. Geçmişteki mobil teknolojiler gibi yüksek gecikme süresi olan bağlantılar yüksek bant genişliği bağlantısı sağladı fakat  bu dahi hızlı bir web deneyimi elde etmeyi mümkün kılmadı.

Another use case that really needs low latency is certain kinds of video, like video conferencing, gaming and similar where there's not just a pre-generated stream to send out.

## 2.6. Head of line blocking

HTTP Pipelining is a way to send another request while waiting for the response to a previous request. It is very similar to queuing at a counter at the bank or in a super market. You just don't know if the person in front of you is a quick customer or that annoying one that will take forever before he/she is done: head of line blocking.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Sure you can be careful about line picking so that you pick the one you really believe is the correct one, and at times you can even start a new line of your own but in the end you can't avoid making a decision and once it is made you cannot switch lines.

Creating a new line is also associated with a performance and resource penalty so that's not scalable beyond a smaller number of lines. There's just no perfect solution to this.

Even today, 2015, most desktop web browsers ship with HTTP pipelining disabled by default.

Additional reading on this subject can be found for example in the Firefox [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
