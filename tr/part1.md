# 1. Arkaplan

Bu belge http2'yi teknik açıdan ve protokol düzeyinde açıklamaktadır. Girişim Daniel'in Nisan 2014'de Stokholm'de yaptığı bir sunum ile başladı ve artdından tüm detaylar ve açıklamalar ile dönüştürüldü ve düzenlendi.

RFC 7540 son http2 şartnamesinin resmi adıdır ve 15 Mayıs 2015'de yayınlanmıştır(http://www.rfc-editor.org/rfc/rfc7540.txt).

Bu dökümanda bulunan tüm hatalar bana aittir ve kendi ihmalimin sonucudur. Lütfen güncellenen sürümlerde düzeltilmesi için  hataları belirleyin.

Protokolü açıklamak için geçerli bir teknik terim olan "HTTP/2" yerine okunabilirlik ve daha iyi bir akış yakalamak adına "http2" kelimesini tercih ettim.

## 1.1 Yazar

Benim adım Daniel Stenberg ve Mozilla'da çalışıyorum. Açık kaynak ve ağ ile 20 yıldan fazla bir süredir sayısız projede yer aldım. Muhtemelen bilinen en öncü curl ve libcurl geliştiricisiyim.Birkaç yıldır IETF HTTPbis çalışma grubunda yer aldım ve orada http 1.1 yeniliklerini takip ettim, aynı zamanda http2 standartlaştırma çalışmalarına dahil oldum.

  Elektronik posta: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Yardım!

Eğer bu dökümanda hatalar, eksiklikler ve bariz yalanlar bulursanız lütfen bu bölümlerin yenilenen halini bana gönderin ve ben de versyonlarda bu hataları düzelteceğim. Yardımcı olan herkese tesekkür ederim. Bu belgeyi zamanla daha iyi hale getirmeyi umuyorum.


Döküman http://daniel.haxx.se/http2 adresinde mevcut.

## 1.3 Lisans

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Bu döküman Creative Commons Attribution 4.0 license tarafından lisanslanmıştır(http://creativecommons.org/licenses/by/4.0/).

## 1.4 Döküman tarihi

Dökümanın ilk versiyonu, 25 Nisan 2014 tarihinde yayınlandı. Burada en son doküman versiyonundaki en büyük değişiklikler takip ediliyor.

### Versiyon 1.13

- Ana versiyonun Markdown sözdizimine dönüşümü
- 13: Daha fazla kaynak, güncel bağlantılar ve açıklamalar 
- 12: QUIC açıklaması referans ile güncelleme
- 8.5: Şimdiki sayılarla yenilenme
- 3.4: Ortalama 40 TCP bağlantısı 
- 6.4: Tenik özellikleri aksettirmek için güncelleme 

### Versiyon 1.12

- 1.1: HTTP/2 bu versiyonda official RFC'de
- 6.5.1: HPACK RFC'ye bağlantı
- 9.1:  http2 için Firefox 36+ yapılandırma ayarları
- 12.1: QUIC ile ilgili eklemeler

### Versiyon 1.11

- Dostça katkıda bulunanların işaret ettiği birçok dil iyileştirmesi
- 8.3.1: Nginx and Apache httpd hakkında atıflar.

### Versiyon 1.10

- 1: Protokol tamamlandı
- 4.1: 2014 yılından intibaren yenilene kelimeler
- Ön: Resim eklendi ve "http2 açıklama" denildi,bağlantı düzenlendi 
- 1.4: Döküman tarihi bölümü eklendi
- Birçok yazım ve dil bilgisi hatası düzenltildi 
- 14: Hata raporcularına teşekkür bölümü eklendi 
- 2.4:Büyüme artışı için daha iyi duyuru
- 6.3: Çoklama özelliğinde sıralama düzeltildi
- 6.5.1: HPACK taslağı-12 

### Versiyon 1.9

- HTTP/2 taslak-17 and HPACK taslak-11 güncellemesi
- "10. Chromium'da http2" (== şimdi bir sayfa daha uzun) bölümü
- Birçok bölüm düzenlemesi
- 30 uygulama  
- 8.5: Bazı güncel kullanım numaraları
- 8.3: internet explorer'a atıf
- 8.3.1 Eksik uygulamalar bölümü
- 8.4.3: TLS'nin başarı oranı artışı
