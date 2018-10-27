# 1. Arkaplan

Bu belge http2'yi teknik açıdan ve protokol düzeyinde açıklamaktadır. Daniel'in Nisan 2014'de Stokholm'de yaptığı bir sunum ile başladı ve tüm detayları, isabetli  açıklamaları ile birlikte tam bir dokümana dönüştü.

RFC 7540 son http2 şartnamesinin resmi adıdır ve 15 Mayıs 2015'de yayınlanmıştır(https://www.rfc-editor.org/rfc/rfc7540.txt).

Bu dokümanda bulunan tüm hatalar bana aittir ve kendi ihmalimin sonucudur. Lütfen güncellenen sürümlerde düzeltilmesi için  hataları belirtin.

Protokolü açıklamak için geçerli bir teknik terim olan "HTTP/2" yerine okunabilirlik ve daha iyi bir akış yakalamak adına "http2" kelimesini tercih ettim.

## 1.1 Yazar

Benim adım Daniel Stenberg ve Mozilla'da çalışıyorum. Açık kaynak ve ağ ile 20 yıldan fazla bir süre sayısız projede çalıştım. Muhtemelen beni öncü curl ve libcurl geliştiricisi olarak biliyorsunuz. Birkaç yıl IETF HTTPbis çalışma grubunda yer aldım ve orada http 1.1 yeniliklerini takip ettim, aynı zamanda http2 standartlaştırma çalışmalarına dahil oldum.

  Elektronik posta: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Yardım!

Eğer bu dokümanda hatalar, eksiklikler ve bariz yalanlar bulursanız lütfen bu bölümlerin yenilenen sürümünü bana gönderin ve ben de bu sürümlerdeki hataları düzelteceğim. Yardımcı olan herkese tesekkür ederim. Bu belgeyi zamanla daha iyi hale getirmeyi umuyorum.


Bu doküman https://daniel.haxx.se/http2 adresinde mevcuttur.

## 1.3 Lisans

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Bu doküman Creative Commons Attribution 4.0 license altında yayınlanmaktadır: (https://creativecommons.org/licenses/by/4.0/).

## 1.4 Doküman tarihçesi

Bu dokümanın ilk sürümü, 25 Nisan 2014 tarihinde yayınlandı. En son doküman sürümlerindeki büyük değişiklikler aşağıdadır.

### Sürüm 1.13

- Ana sürüm Markdown sözdizimine dönüştürüldü
- 13: Daha fazla kaynak, güncel bağlantılar ve açıklamalar eklendi
- 12: Taslağına referans vererek QUIC açıklaması güncellendi
- 8.5: Yeni rakamlarla güncellendi
- 3.4: Ortalama artık 40 TCP bağlantısıdır 
- 6.4: Teknik özelliklerin ne dediği güncellendi 

### Sürüm 1.12

- 1.1: HTTP/2 artık resmi bir RFC'de yer almaktadır.
- 6.5.1: HPACK RFC'ye bağlantı verildi.
- 9.1:  http2 için Firefox 36+ yapılandırma ayarlarından bahsedildi
- 12.1: QUIC hakkında bölüm eklendi.

### Sürüm 1.11

- Katkıda bulunan arkadaşlar tarafından birçok dil iyileştirmesi yapıldı
- 8.3.1: Nginx and Apache httpd spesifik aktivitelerinden bahsedildi.

### Sürüm 1.10

- 1: Protokol tamam oldu.
- 4.1: 2014 yılından itibaren kullanılan üslup yenilendi
- Ön: Burada resim eklendi ve "http2'nin açıklaması" denildi, bağlantı düzenlendi 
- 1.4: Doküman tarihçesi bölümü eklendi
- Birçok yazım ve dil bilgisi hatası düzeltildi 
- 14: Hataları iletenler sayesinde teşekkürler bölümü eklendi 
- 2.4:HTTP büyüme grafiği için daha iyi etiketler
- 6.3: Çoklama treninde vagon sıralaması düzeltildi
- 6.5.1: HPACK taslak-12 

### Sürüm 1.9

- HTTP/2 taslak-17 and HPACK taslak-11 güncellendi
- "10. Chromium'da http2" (== şimdi bir sayfa daha uzun) bölümü eklendi
- Birçok bölüm düzenlemesi
- Şimdi 30 uygulamada  
- 8.5: Bazı güncel kullanım rakamları
- 8.3: internet explorer'a da atıf
- 8.3.1 "Eksik uygulamalar" bölümü eklendi
- 8.4.3: Ayrıca TLS'in başarı oranı artışından bahseder
