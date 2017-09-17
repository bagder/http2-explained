# 6. Http2 protokolü

Arkaplan hakkında, bizi ilgilendiren tarih ve siyaset hakkında yeterince bilgi bulunuyor. Protokolün özelliklerine, http2'yi oluşturan bitler ve kavramlara bakalım.

## 6.1. İkili protokol

http2, ikili bir protokoldür.

Bir dakika izin ver yeter. Daha önce internet protokolleri ile ilgilendiyseniz, bu değişime bir şekilde tepki göstereceksiniz, çünkü insanlar telnet ve benzeri yollarla istekte bulunabileceğinden metin / ascii tabanlı protokollerin nasıl daha üstün olduğunu argümanlarınızı sıralayarak ispatlamaya çalışacaktır...

http2, çerçevelemeyi çok daha kolay hale getirmek için ikilidir. Çerçevelerin başlangıcını ve bitişini bulmak HTTP 1.1'de ve aslında genel olarak metin tabanlı diğer protokollerde de karmaşık durumlardan biridir. İsteğe bağlı beyaz boşluğun dışına ve aynı şey için farklı yollarla ilerleyerek uygulama daha basit hale gelir.?

Ayrıca, gerçek protokol bölümlerini çerçeveden ayırmak daha kolaydır, ki HTTP1 karmaşıktır.

The fact that the protocol features compression and will often run over TLS also diminishes the value of text, since you won't see text over the wire anyway. We simply have to get used to the idea of using something like a Wireshark inspector to figure out exactly what's going on at the protocol level in http2.

Protokolün sıkıştırma özelliğine sahip olması ve genellikle TLS ile çalışması da, metnin önemini düşürür; zira yine de hat üzerinde metin görmezsiniz. Http2'deki protokol seviyesinde neler olduğunu tam olarak anlamak için bir Wireshark gibi ağdaki paketler inceleyebileceğiniz bir ygulama kullanma fikrine alışmamız yeterlidir.

Debugging this protocol will probably have to be done with tools like curl, or by analyzing the network stream with Wireshark's http2 dissector and similar.

Bu protokolün hata ayıklamasının muhtemelen curl gibi araçlarla veya ağ akışının Wireshark ve benzerleriyle analiz ederek yapılması gerekecektir.

## 6.2. İkili format

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2, ikili çerçeveler gönderir. Gönderilebilen farklı çerçeve türleri de vardır ve hepsi aynı ayarlara sahiptir: Uzunluk, Tip, Bayraklar, Akış Tanımlayıcı ve çerçeve yükü.

There are ten different frame types defined in the http2 spec and perhaps the two most fundamental ones that map to HTTP 1.1 features are DATA and HEADERS. I'll describe some of the frames in more detail further on.

Http2 beyannamesinde tanımlanan on farklı çerçeve türü vardır ve HTTP 1.1 özelliklerine eşleyen iki temel çerçeve VERİ ve BAŞLIK'tır. Çerçevelerin bazılarını daha ayrıntılı olarak açıklayacağım.

## 6.3. Çoklu akışlar

Önceki bölümde bahsedilen Akım Tanımlayıcı, http2 üzerinden gönderilen her kareyi bir "akış" ile ilişkilendirir. Akış, http2 bağlantısı içinde istemci ve sunucu arasında değiştirilen bağımsız, çift yönlü bir çerçeve dizisidir.

Tek bir http2 bağlantısı,eşzamanlı birden fazla açık akış içerebilir; bu uç noktalarda çoklu akışlardan çerçeveler araya girebilir. Akışlar kurulabilir ve tek taraflı olarak kullanılabilir veya istemci veya sunucu tarafından paylaşılabilir ve iki uç nokta tarafından da kapatılabilir. Çerçevelerin bir akış içinde gönderilme sırası önemlidir. Alıcılar, çerçeveleri aldığı sıraya göre işlerler.

Multiplexing the streams means that packages from many streams are mixed over the same connection. Two (or more) individual trains of data are made into a single one and then split up again on the other side. Here are two trains:

Akışların çoğullaması, birçok akıştan gelen paketlerin aynı bağlantı üzerinden karışabilmesi anlamına gelir. İki bireysel tren tek bir tren haline gelebilir ve daha sonra diğer tarafta tekrar ayrılabilir.

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

İki tren aynı bağlantı üzerinden çoğullandı:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Öncelikler ve Bağımlılıklar

Her bir akış, sunucuyu öncelikle hangi akışların gönderileceğini seçmeye zorlayan kaynak kısıtlamaları olması durumunda, hangi akışın en önemli olduğunu söylemek için kullanılan bir öncelik(ağırlık olarak da bilinir) bilgisine sahiptir.

ÖNCELİK çerçevesini kullanarak bir istemci, sunucuya bir akışın bağlı olduğu diğer bir akışı da söyleyebilir. Bu istemciye öncelik agacı oluşturmasına izin verir  ki bu ağacda "cocuk akışlar" bircok "ebeveyn akışa" bağlı olabilir.

Öncelik ağırlıkları ve bağımlılıkları çalışma zamanında dinamik olarak değiştirilebilir, ki bu da, kullanıcılar görüntülerin bulunduğu bir sayfayı aşağıya kaydırdığında, tarayıcıların hangi görüntülerin en önemli olduğunu belirleyebilmsine veya sekmeleri değiştirirseniz, birdenbire odaklanacak yeni bir dizi akışın önceliğini oluşturabilmesine olanak tanır.

## 6.5. Başlık sıkıştırma

HTTP yurtsuz(stateless) bir protokoldür. Kısaca, bu, sunucunun önceki isteklerden çok fazla bilgi ve meta veri depolaması gerekmeden, her talebin sunucunun bu talebi sunması için gereken kadar ayrıntılı getirmesi gerektiği anlamına gelir. Http2 bu paradigmayı değiştirmediği için aynı şekilde çalışması gerekir.

Bu, HTTP'yi tekrarlı yapar. Bir müşteri bir web sayfasındaki, örneğin görüntüler gibi aynı sunucudan birçok kaynak istediğinde, hepsi için neredeyse aynı görünen bir istek dizisi talep edecektir. Sıklıkla aynı olan bu dizi sıkıştırma için yalvarır.

Web sayfası başına düşen nesne sayısı arttıkça (önceden belirtildiği gibi), çerezlerin kullanımı ve isteklerin boyutu da zamanla artmaya devam etti. Çerezlerin tüm isteklerde bulunması gerekir, çoğu zaman aynı istek çoklu istekler içindedir.

HTTP 1.1 istek boyutları o kadar büyük oluyor ki bazen ilk TCP penceresinden daha büyük olabiliyor, bu da istek gönderilmeden önce sunucudan ACK almak için tam bir gidiş dönüş zamanına ihtiyaç duyduklarından göndermenin çok yavaş olmasına sebep oluyor. Sıkıştırmanın bir başka kanıtı bu durumdur.

### 6.5.1. Sıkıştırma hileli bir konudur

HTTPS ve SPDY sıkıştırmasının [BREACH](http://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) ve [CRIME](http://en.wikipedia.org/wiki/CRIME) saldırılarına karşı savunmasız olduğu tespit edildi. Bilinen metni akışa çıktıyı ekleyerek, nasıl değiştirdiğini öğrenebilir, böylece saldırgan şifreli bir yükte gönderileni anlamaya çalışabilir.

Doing compression on dynamic content for a protocol - without becoming vulnerable to one of these attacks - requires some thought and careful consideration. This is what the HTTPbis team tried to do.

Bir protokol için dinamik içeriğe sıkıştırma yapmak biraz düşünülmesi ve dikkatli olması gereken bir konudur(saldırılardan birine karşı savunmasız hale gelmeden yapılır). HTTPbis ekibi bunu yapmaya çalıştı.

[HPACK](http://www.rfc-editor.org/rfc/rfc7541.txt) bakın, HPACK(adından da anlaşılacağı gibi), HTTP/2 için Başlık Sıkıştırmasıdır. Bu sıkıştırma biçimi özellikle http2 başlıkları için hazırlanmış olup, ayrı bir internet taslağında belirtilmektedir. 



Header Compression for HTTP/2, which – as the name suggests - is a compression format especially crafted for http2 headers, and it is being specified in a separate internet draft. The new format, together with other counter-measures (such as a bit that asks intermediaries to not compress a specific header and optional padding of frames), should make it harder to exploit compression.

In the words of Roberto Peon (one of the creators of HPACK):

> “HPACK was designed to make it difficult for a conforming implementation to
> leak information, to make encoding and decoding very fast/cheap, to provide
> for receiver control over compression context size, to allow for proxy
> re-indexing (i.e., shared state between frontend and backend within a proxy),
> and for quick comparisons of Huffman-encoded strings”.

## 6.6. Sıfırla - fikrini değiştir

One of the drawbacks with HTTP 1.1 is that when an HTTP message has been sent
off with a Content-Length of a certain size, you can't easily just stop
it. Sure, you can often (but not always) disconnect the TCP connection, but that
comes at the cost of having to negotiate a new TCP handshake again.

A better solution would be to just stop the message and start a new. This can be done with http2's RST_STREAM frame which will help prevent wasted bandwidth and the need to tear down connections.

## 6.7. Server push

This is the feature also known as “cache push”. The idea is that if the client asks for resource X, the server may know that the client will probably want resource Z as well, and sends it to the client without being asked. It helps the client by putting Z into its cache so that it will be there when it wants it.

Server push is something a client must explicitly allow the server to do. Even then, the client can swiftly terminate a pushed stream at any time with RST_STREAM should it not want a particular resource.

## 6.8. Flow Control

Each individual http2 stream has its own advertised flow window that the other end is allowed to send data for. If you happen to know how SSH works, this is very similar in style and spirit.

For every stream, both ends have to tell the peer that it has enough room to handle incoming data, and the other end is only allowed to send that much data until the window is extended. Only DATA frames are flow controlled.

