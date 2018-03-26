# 4. HTTP'nin güncellenmesi

Geliştirilmiş bir protokol yapmak hoş olmaz mıydı? Elbette olurdu...

1. Gecikmelere daha duyarlı olansun
2. Boru hattı ve satır başı engelleme sorununu düzeltsın
3. Her bir barındırıcıya olan bağlantı sayısını artırmaya gerek duymazsın
4. Mevcut tüm arayüzleri, tüm içeriği, URI formatını saklasın
5. IETF'nin HTTPbis çalışma grubu içinde yapılansın

## 4.1. IETF ve HTTPbis çalışma grubu

İnternet Mühendisliği Görev Gücü(IETF), çoğunlukla protokol seviyesinde, internet standartlarını geliştiren ve tanıtan bir organizasyondur. TCP, DNS, FTP'den en iyi uygulamalara, HTTP'ye ve çok sayıda protokol türevine varıncaya kadar her şeyi belgeleyen RFC dökümanları için dünya çapında yaygınca bilinirler.
 
IETF içinde, bir hedefe doğru çalışmak için "çalışma grupları" sınırlı bir kapsam ile oluşturulmuştur.  Ürettikleri şey için bazı belirlenmiş yönergeler ve sınırlamalar içeren bir "charter" (tanımlama) oluştururlar. Tartışmalara ve gelişmelere herkesin katılmasına izin verilir.  Katılan ve bir şeyler söyleyen herkes, sonuca etkı den aynı ağırlığa ve şansa sahiptir ve herkes, kendisi için çalıştığı şirkete pek saygı duymaksızın, bir birey olarak sayılır.

HTTPbis çalışma grubu (adın açıklaması için daha sonra inceleyelim) 2007 yazında kuruldu ve HTTP 1.1 şartnamesinin güncellenmesi görevini üstlendi.  Bu grup içinde HTTP'nin bir sonraki sürümü ile ilgili tartışmalar 2012'nin sonlarında gerçekten başladı.  HTTP 1.1 güncelleme çalışması 2014 yılının başında tamamlandı ve [RFC 7230](https://tools.ietf.org/html/rfc7230) serisi ile sonuçlandı.

HTTPbis WG için gerçekleşen son görüşmeler, Haziran 2014'ün başında New York'ta yapıldı. Geri kalan tartışmalar ve resmi RFC'yi almak için gerçekleştirilen IETF prosedürleri ertesi yıl devam etti.

HTTP sahasındaki daha büyük oyuncuların bazıları çalışma grubu tartışmalarında ve toplantılarda eksik kaldı. Burada herhangi bir şirket veya ürün adından bahsetmek istemiyorum, ancak bugün Internet'teki bazı aktörlerin IETF'nin bu şirketlerin karışımı olmadan iyi olacağından emin oldukları açıkça görünüyor...

### 4.1.1. İsmin "bis" bölümü

Grup, HTTPbis olarak adlandırıldı ve burada "bis" kısmı iki Latin alfabesinden [Latin adverb for two](https://en.wiktionary.org/wiki/bis#Latin) geliyor. Bis genellikle bir güncelleme için IETF içindeki adın bir sonek veya bir parçası olarak kullanılır veya ikinci bir beyanname'ye geçmek;bu durumda HTTP 1.1 güncellemesi.

## 4.2. http2 SPDY'den başladı

[SPDY](https://en.wikipedia.org/wiki/SPDY) Google tarafından geliştirilen ve öncülük eden bir protokoldür. Kesinlikle bunu açık alanda geliştirdiler ve herkesi katılmaya davet ettiler, ancak hem popüler bir tarayıcı uygulaması hem de iyi kullanılan servislere sahip önemli bir sunucu popülasyonu üzerinde kontrol sahibi olduklarından yararlandıkları açıktır.

HTTPbis grubu http2 üzerinde çalışmaya başlamanın zamanı olduğuna karar verdiklerinde, SPDY zaten bir çalışma konsepti olduğunu kanıtlamıştı. İnternette konuşlandırmanın mümkün olduğunu göstermiş ve ne kadar iyi performansı olduğunu gösteren yayınlanmış rakamlar bulunmaktadır.  Http2 çalışması, temelde http2 taslak-00'a küçük bir arama ve değiştirme ile yapılan SPDY/3 taslağı ile başladı.