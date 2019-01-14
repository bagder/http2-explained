# 6. Http2 protokolü

Arkaplan hakkında, bizi ilgilendiren tarih ve siyaset hakkında yeterince bilgi bulunuyor. Protokolün özelliklerine, http2'yi oluşturan ikili sayılar ve kavramlara bakalım.

## 6.1. İkili protokol

http2, ikili bir protokoldür.

Bir dakika izin ver yeter. Daha önce internet protokolleri ile ilgilendiyseniz, bu değişime bir şekilde tepki göstereceksiniz, çünkü insanlar telnet ve benzeri yollarla istekte bulunabileceğinden metin / ascii tabanlı protokollerin nasıl daha üstün olduğunu argümanlarınızı sıralayarak ispatlamaya çalışacaktır...

http2, çerçevelemeyi çok daha kolay hale getirmek için ikilidir. Çerçevelerin başlangıcını ve bitişini bulmak HTTP 1.1'de ve aslında genel olarak metin tabanlı diğer protokollerde de karmaşık durumlardan biridir. İsteğe bağlı beyaz boşluğun dışına ve aynı şey için farklı yollarla ilerleyerek uygulama daha basit hale gelir?

Ayrıca, gerçek protokol bölümlerini çerçeveden ayırmak daha kolaydır, ki HTTP1 karmaşıktır.

Protokolün sıkıştırma özelliğine sahip olması ve genellikle TLS ile çalışması da, metnin önemini düşürür; zira yine de hat üzerinde metin görmezsiniz. Http2'deki protokol seviyesinde neler olduğunu tam olarak anlamak için bir Wireshark gibi ağdaki paketleri inceleyebileceğiniz bir uygulama kullanma fikrine alışmamız yeterlidir.

Bu protokolün hata ayıklamasının muhtemelen curl gibi araçlarla veya ağ akışının Wireshark ve benzerleriyle analiz ederek yapılması gerekecektir.

## 6.2. İkili format

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2, ikili çerçeveler gönderir. Gönderilebilen farklı çerçeve türleri de vardır ve hepsi aynı ayarlara sahiptir: Uzunluk, Tip, Bayraklar, Akış Tanımlayıcı ve çerçeve yükü.

Http2 beyannamesinde tanımlanan on farklı çerçeve türü vardır ve HTTP 1.1 özelliklerine eşleyen iki temel çerçeve VERİ ve BAŞLIK'tır. Çerçevelerin bazılarını daha ayrıntılı olarak açıklayacağım.

## 6.3. Çoklu akışlar

Önceki bölümde bahsedilen Akım Tanımlayıcı, http2 üzerinden gönderilen her kareyi bir "akış" ile ilişkilendirir. Akış, http2 bağlantısı içinde istemci ve sunucu arasında değiştirilen bağımsız, çift yönlü bir çerçeve dizisidir.

Tek bir http2 bağlantısı, eş zamanlı birden fazla açık akış içerebilir; bu uç noktalarda çoklu akışlardan çerçeveler araya girebilir. Akışlar kurulabilir ve tek taraflı olarak kullanılabilir veya istemci veya sunucu tarafından paylaşılabilir ve iki uç nokta tarafından da kapatılabilir. Çerçevelerin bir akış içinde gönderilme sırası önemlidir. Alıcılar, çerçeveleri aldığı sıraya göre işlerler.

Akışların çoğullaması, birçok akıştan gelen paketlerin aynı bağlantı üzerinden karışabilmesi anlamına gelir. İki bireysel tren tek bir tren haline gelebilir ve daha sonra diğer tarafta tekrar ayrılabilir.

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

İki tren aynı bağlantı üzerinden çoğullandı:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Öncelikler ve Bağımlılıklar

Her bir akış, sunucuyu öncelikle hangi akışların gönderileceğini seçmeye zorlayan kaynak kısıtlamaları olması durumunda, hangi akışın en önemli olduğunu söylemek için kullanılan bir öncelik(ağırlık olarak da bilinir) bilgisine sahiptir.

ÖNCELİK çerçevesini kullanarak bir istemci, sunucuya bir akışın bağlı olduğu diğer bir akışı da söyleyebilir. Bu istemciye öncelik agacı oluşturmasına izin verir  ki bu ağacda "cocuk akışlar" bircok "ebeveyn akışa" bağlı olabilir.

Öncelik ağırlıkları ve bağımlılıkları çalışma zamanında etkin olarak değiştirilebilir, ki bu da, kullanıcılar görüntülerin bulunduğu bir sayfayı aşağıya kaydırdığında, tarayıcıların hangi görüntülerin en önemli olduğunu belirleyebilmesine veya sekmeleri değiştirirseniz, birdenbire odaklanacak yeni bir dizi akışın önceliğini oluşturabilmesine olanak tanır.

## 6.5. Başlık sıkıştırma

HTTP yurtsuz(stateless) bir protokoldür. Kısaca, bu, sunucunun önceki isteklerden çok fazla bilgi ve meta veri depolaması gerekmeden, her talebin sunucunun bu talebi sunması için gereken kadar ayrıntılı getirmesi gerektiği anlamına gelir. Http2 bu paradigmayı değiştirmediği için aynı şekilde çalışması gerekir.

Bu, HTTP'yi tekrarlı yapar. Bir müşteri bir web sayfasındaki, örneğin görüntüler gibi aynı sunucudan birçok kaynak istediğinde, hepsi için neredeyse aynı görünen bir istek dizisi talep edecektir. Sıklıkla aynı olan bu dizi sıkıştırma için yalvarır.

Web sayfası başına düşen nesne sayısı arttıkça (önceden belirtildiği gibi), çerezlerin kullanımı ve isteklerin boyutu da zamanla artmaya devam etti. Çerezlerin tüm isteklerde bulunması gerekir, çoğu zaman aynı istek çoklu istekler içindedir.

HTTP 1.1 istek boyutları o kadar büyük oluyor ki bazen ilk TCP penceresinden daha büyük olabiliyor, bu da istek gönderilmeden önce sunucudan ACK almak için tam bir gidiş dönüş zamanına ihtiyaç duyduklarından göndermenin çok yavaş olmasına sebep oluyor. Sıkıştırmanın bir başka kanıtı bu durumdur.

### 6.5.1. Sıkıştırma hileli bir konudur

HTTPS ve SPDY sıkıştırmasının [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) ve [CRIME](https://en.wikipedia.org/wiki/CRIME) saldırılarına karşı savunmasız olduğu tespit edildi. Bilinen metni akışa çıktıyı ekleyerek, nasıl değiştirdiğini öğrenebilir, böylece saldırgan şifreli bir yükte gönderileni anlamaya çalışabilir.

Bir protokol için etkin içeriğe sıkıştırma yapmak biraz düşünülmesi ve dikkatli olması gereken bir konudur(saldırılardan birine karşı savunmasız hale gelmeden yapılır). HTTPbis ekibi bunu yapmaya çalıştı.

[HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt) bakın, HPACK(adından da anlaşılacağı gibi), HTTP/2 için Başlık Sıkıştırmasıdır. Bu sıkıştırma biçimi özellikle http2 başlıkları için hazırlanmış olup, ayrı bir internet taslağında belirtilmektedir. Yeni format, diğer karşı ölçümlerle(belirli bir üstbilgiyi ve çerçevelerin dolgusunu(adding) sıkıştırmamasını sağlayan bir bit gibi), sıkıştırmanın kullanılmasını zorlaştırır.

Roberto Peon'un (HPACK'in yaratıcılarından biri) sözleriyle:

> HPACK sızan bilgiyi önlemek,
> kodlama ve kod çözme işlemini hem hızlandırmak hem ucuzlaştırmak,
> sıkıştırılan içerik boyutu üzerinde alıcı adına kontrol sağlamak,
> proxy'nin yeniden indexlenmesine izin vermek(yani, bir proxy içindeki ön uç ve arka uç arasında paylaşılan durum)
> ve Huffman kodlu dizelerin çabuk karşılaştırmaları için tasarlandı.

## 6.6. Sıfırla - fikrini değiştir

HTTP 1.1'in getirdiği dezavantajlardan biri, belirli bir boyuta sahip bir İçerik-Uzunluğu ile bir HTTP mesajı gönderildiğinde, onu kolayca durdurmanızın mümkün olamayacağıdır. Tabi, TCP bağlantısını kesebilirsiniz (her zaman değil) ancak yine de yeni bir TCP el sıkışması yapmak zorunda kalmanız maliyetlidir.

Daha iyi bir çözüm, mesajı durdurup yeni bir başlangıç yapmak olacaktır. Bunu, boşa harcanmış bant genişliğini ve bağlantıları yıkma ihtiyacını önlemeye yardımcı olacak, http2'nin RST_STREAM çerçevesiyle yapabilirsiniz.

## 6.7. Sunucu İtme

Bu, "önbellek itme" olarak da bilinen özelliktir. Müşteri kaynak X sorduğunda, sunucu istemcinin istemcinin Z kaynağı da istediğini bilir ve sorulmadan istemciye gönderir. İstediğinde orada olacak şekilde Z'yi önbelleğine koyarak müşteriye yardımcı olur.

Sunucu itme, istemcinin sunucuya açıkça izin vermesi gereken bir özelliktir. İstemci belirli bir kaynağı istemiyorsa, itilen bir akışı RST_STREAM ile hızla sonlandırabilir.

## 6.8. Akış kontrolü

Her bir http2 akışının kendi akış penceresi vardır, bu pencerede diğer ucun veri göndermesine izin verilir. SSH'ın nasıl çalıştığını biliyorsanız, çok benzer  olduğunu göreceksiniz.

Her bir akış için, iki uçun da gelen veriyi işlemek için yeterli alana sahip olup olmadığı veya diğer uçta yalnızca pencere genişletilinceye kadar belirli miktarda veri gönderilmesine izin verilebileceği gibi durumları bildirmeye hakları vardır. Sadece VERİ çerçeveleri akış kontrollüdür.