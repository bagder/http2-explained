# 1. Arkaplan

Bu belge http2'yi teknik açıdan ve protokol düzeyinde açıklamaktadır. Girişim Daniel'in Nisan 2014'de Stokholm'de yaptığı bir sunum ile başladı ve artdından tüm detaylar ve açıklamalar ile dönüştürüldü ve düzenlendi.

RFC 7540 son http2 beyannamesinin resmi adıdır ve 15 Mayıs 2015'de yayınlanmıştır(http://www.rfc-editor.org/rfc/rfc7540.txt).

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

The first version of this document was published on April 25th 2014. Here follows the largest changes in the most recent document versions.

### Versiyon 1.13

- Converted the master version of this document to Markdown syntax
- 13: Mention more resources, updated links and descriptions 
- 12: Updated the QUIC description with reference to draft 
- 8.5: Refreshed with current numbers 
- 3.4: The average is now 40 TCP connections 
- 6.4: Updated to reflect what the spec says 

### Versiyon 1.12

- 1.1: HTTP/2 is now in an official RFC 
- 6.5.1: Link to the HPACK RFC 
- 9.1: Mention the Firefox 36+ config switch for http2 
- 12.1: Added section about QUIC 

### Versiyon 1.11

- Lots of language improvements mostly pointed out by friendly contributors 
- 8.3.1: Mention nginx and Apache httpd specific acitivities 

### Versiyon 1.10

- 1: The protocol has been “okayed” 
- 4.1: Refreshed the wording since 2014 is last year 
- Front: Added image and call it “http2 explained” there, fixed link 
- 1.4: Added document history section 
- Many spelling and grammar mistakes corrected 
- 14: Added thanks to bug reporters 
- 2.4: Better labels for the HTTP growth graph 
- 6.3: Corrected the wagon order in the multiplexed train 
- 6.5.1: HPACK draft-12 

### Versiyon 1.9

- Updated to HTTP/2 draft-17 and HPACK draft-11  
- Added section "10. http2 in Chromium" (== one page longer now)  
- Lots of spell fixes  
- At 30 implementations now  
- 8.5: Added some current usage numbers  
- 8.3: Mention internet explorer too  
- 8.3.1 Added "missing implementations"  
- 8.4.3: Mention that TLS also increases success rate
