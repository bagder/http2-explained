# 2. Günümüzde HTTP 

HTTP 1.1, İnternet'teki neredeyse her şey için kullanılan bir protokoldür. Bunun avantajlarından yararlanan protokollerde ve altyapıda büyük yatırımlar yapılmıştır, çünkü bugün kendiliğinden yeni şeyler yapmaktan çok, HTTP'nin üstünde çalışmak daha kolay olmaktadır.

## 2.1 HTTP 1.1 devasadır

HTTP oluşturulduğunda ve dünyaya yayıldığında, muhtemelen basit ve anlaşılır bir protokol olarak algılanıyordu, fakat zaman bunun yanlış olduğunu kanıtladı. RFC 1945 HTTP 1.0'i, 1996 yılında, 60 sayfa tanımlamayla yayınlanmıştır. RFC2616 HTTP 1.1'i ise yalnızca 3 sene sonra 1999 da yayınlanmıştır ve önemli ölçüde artış göstererek 176 sayfaya çıkmıştır. Bununla birlikte, IETF bu spektrumdaki güncelleme üzerinde çalışırken bölünmüş ve altı belgeye dönüştürülmüştür(RFC7230 ve ailesi). HTTP 1.1 büyüktür ve sayısız ayrıntı, incelik ve çok sayıda isteğe bağlı bölüm içermektedir.

## 2.2 Seçenekler dünyası

HTTP 1.1 in  bir çok detaya sahip yapısı ve sonraki eklentiler için de kullanılabilen seçenekleri, neredeyse hiçbir uygulamanın hiçbir zaman hiçbir yerde uygulayamayacağı bir yazılım ekosistemi haline getirmiştir ve "hiçbir şey" kavramının tam olarak ne olduğunu söylemek mümkün değildir. Bu, özelliklerin çok az sayıda olmasına ve çok az yararlanıldığı bir duruma neden oldu.

Daha sonraları, sunucu ve istemciler bu tür özellikleri daha çok kullanmaya başladığında, bu "birlikte çalışabilirlik" sorununa neden oldu. HTTP boruhattı, bu tarz özelliklere birincil örnektir. 

## 2.3 TCP’nin yetersiz kullanımı

HTTP 1.1, TCP'nin sunduğu tüm gücü ve performansı tam olarak kullanmanın zor bir dönemini yaşamaktadır. HTTP istemcileri ve tarayıcılar, sayfa yükleme sürelerini azaltan çözümler bulmak için çok yaratıcı olmalıdır.

Yıllar boyunca paralel olarak devam eden diğer girişimler TCP’nin bu kadar kolay değiştirilmediğini doğruladı ve bu yüzden hem TCP'yi hem de protokolleri geliştirmeye devam ediyoruz.

Basitce söylemek gerekirse, TCP daha fazla veri göndermek veya almak ve boş sürelerden kaçınmak için daha iyi değerlendirilebilir. Sıradaki başlıklarda bu eksikliklerin bazıları vurgulanacaktır. 

## 2.4 Aktarım boyutları ve nesne sayısı

Bugün Web'deki en popüler sitelerin bazılarına ve bunların ön sayfalarını indirmek için neye ihtiyaç olduğuna bakıldığında net bir model ortaya çıkıyor. Yıllar geçtikce alınan veri miktarı kademeli olarak 1.9MB'ın üzerine çıkmıştır.Burda daha önemli olan şey, her sayfayı görüntülemek için ortalama olarak yüzü aşkın bireysel kaynağa ihtiyaç duyulmasıdır.

Aşağıdaki grafiğin gösterdiği gibi, trend, bir süredir devam etmektedir ve yakın zamanda bunun değişeceğine dair hiç bir gösterge yoktur.
Grafik toplam aktarım boyutunun büyümesini(yeşil olan) ve dünyadaki en populer web sitelerine gelen ortalama istek sayısını(kırmızı olan) göstermektedir.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Gecikme öldürür

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1'in gecikmeye karşı çok hassas olması, kısmen HTTP boruhattının buyuk bir kullanıcı oranına sahip olması ile ilgilidir.

Geçtiğimiz birkaç yılda kullanıcılara mevcut bant genişliğinde büyük bir artış gösterse de, gecikmeyi azaltma yolunda aynı seviyede bir gelişmeye rastlamadık. Geçmişteki mobil teknolojiler gibi yüksek gecikme süresi olan bağlantılar, yüksek bant genişliği bağlantısı sağladı fakat bu dahi hızlı bir web deneyimi elde etmeyi mümkün kılmadı.

Gerçekten düşük gecikmeye ihtiyaç duyan başka bir kullanım örneği video konferansı, oyunlar gibi video türleridir, ki bunlarda önceden oluşturulmuş akışlar yoktur.

## 2.6. Satır başı engelleme

HTTP Boru Hattı, bir önceki isteğe yanıt beklerken başka bir istek göndermenin bir yoludur. Bu durum bankadaki veya süper marketteki bir işlem alanında sıra oluşmasına çok benzer. Önünüzdeki kişi işini hızlıca halledecek dakik bir müşteri midir, yoksa işe başlamadan önce sonsuza dek süreceğine inanan sinir bozucu bir müşteri midir bilmiyorsunuz: satır başı engelleme işte budur.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Satır seçimi konusunda dikkatli olabilirsiniz, bu yüzden doğru olduğunu gerçekten düşündüğünüz bir satırı seçebilirsiniz veya bazen kendinize ait yeni bir satırdan başlayabilirsiniz fakat bunun sonunda seçtiğiniz bu satırı değiştiremezsiniz.

Yeni bir satır oluşturmak da bir performans ve kaynak cezasıyla ilişkilidir, bu yüzden daha küçük satır sayılarının ötesinde ölçeklenebilir değildir. Dolayısıyla bu da mükemmel bir çözüm değildir.

Bugün bile(2015), çoğu masaüstü web tarayıcısında varsayılan olarak HTTP boru hattı devre dışı bırakılmıştır.

Ek olarak bu konu ile ilgili örnekler Firefox'un adresinde bulunur[bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
