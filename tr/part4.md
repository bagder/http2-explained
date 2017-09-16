# 4. HTTP güncellenmesi

Geliştirilmiş bir protokol yapmak hoş olmaz mıydı? Elbette olurdu...

1. Gecikmelere daha duyarlı olan
2. Boru hattı ve satır başı engelleme sorununu çözen
3. Her bir sunucuya olan bağlantı sayısını artırma ihtiyacını ortadan kaldıran
4. Mevcut tüm arayüzleri, tüm içeriği, link formatlarını ve düzenlerini saklayan
5. IETF'nin HTTPbis çalışma grubu içinde oluşan

## 4.1. IETF ve HTTPbis çalışma grubu

İnternet Mühendisliği Görev Gücü(IETF) çoğunlukla protokol seviyesinde, internet standartlarını geliştiren ve tanıtan bir organizasyondur. TCP, DNS, FTP'den en iyi uygulamalara, HTTP'ye ve çok sayıda protokol türevine varıncaya kadar herşeyi belgeleyen RFC dökümanları için dünya çapında bilinirler.
 
IETF'de, bir hedefe doğru çalışmak için "çalışma grupları" sınırlı bir kapsam ile oluşturulmuştur.  Ortaya koyduklaı şey için bazı belirlenmiş yönergeler ve sınırlamalar içeren bir "tanımlama"(charter) oluştururlar. Tartışmalara ve gelişmelere herkesinkatılmasına müsade ederler.  Katılmak ve bir şeyler söylemek konusunda herkes, eşit haklara ve ağırlığa sahiptir ve herkes, çalıştığı şirkti yok sayarcasına bir birey olarak sayılır.

HTTPbis çalışma grubu (adın açıklaması için daha sonra inceleyelim) 2007 yazında kuruldu ve HTTP 1.1 şartnamesinin güncellenmesi görevini üstlendi.  Bu grup içinde HTTP'nin bir sonraki sürümü ile ilgili tartışmalar 2012'nin sonlarında gerçekten başlamıştı.  HTTP 1.1 güncelleme çalışması 2014 yılının başında tamamlandı ve [RFC 7230](https://tools.ietf.org/html/rfc7230) serisi ile sonuçlandı.

HTTPbis WG için yapılan son görüşmeler, Haziran 2014'ün başında New York'ta yapıldı. Geri kalan tartışmalar ve resmi RFC'yi almak için gerçekleştirilen IETF prosedürleri ertesi yıl devam etti.

HTTP sahasındaki daha büyük oyuncuların bazıları çalışma grubu tartışmalarında ve toplantılarda geri plandaydı. Burada herhangi bir şirket veya ürün adından bahsetmek istemiyorum, ancak açıkça bugün Internet'teki bazı aktörler IETF'nin bu şirketlerle ilşkili olmadan başaracağına emin gibi görünüyorlar...

### 4.1.1. İsimin "bis" bölümü

"HTTPbis" isminin "bis" bölmüü latin alfabesinden[Latin adverb for two](http://en.wiktionary.org/wiki/bis#Latin) alıyor. bis genellikle sonek(suffix) veya IETF içinde bir güncelleme ya da ikinci bir spesifikasyon gibi kullanıldı: HTTP 1.1 için güncelleme'dir.

## 4.2. http2 SPDY'den başladı

[SPDY](http://en.wikipedia.org/wiki/SPDY) Google tarafından geliştirilen ve öncülük eden bir protokoldür.  Açık alanda geliştirseler ve herkesi katılmaya davet etseler dahi, hem popüler bir tarayıcı uygulaması hem de iyi kullanılan servislere sahip önemli bir sunucu popülasyonu olan Google'ın bu birikimlerden daha çok yararlandıkları açıktır.

When the HTTPbis group decided it was time to start working on http2, SPDY had already proven that it was a working concept. It had shown it was possible to deploy on the Internet and there were published numbers that proved how well it performed. The http2 work began with the SPDY/3 draft that was basically made into the http2 draft-00 with a little search and replace.

HTTPbis grubu http2 üzerinde çalışmaya başlamak için doğru bir zaman olduğuna karar verdiklerinde, SPDY halihazırda bir çalışma konsepti olduğunu kanıtlamıştı. İnternette konuşlandırmanın mümkün olduğunu göstermiş ve ne kadar iyi performans gösterdiğini ispatlayan yayınlar ve bulgular bulunmaktaydı. Böylece http2 çalışması, http2 taslak-00', temelde küçük bir arama ve değiştirme ile SPDY/3 taslağını kaynak olarak kullandı.
  
