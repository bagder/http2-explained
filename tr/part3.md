# 3. Gecikmelerin üstesinden gelmek için yapılanlar

Her zaman olduğu gibi sorunlarla karşı karşıya kaldıklarında, insanlar geçici çözüm bulmak için toplanırlar. Geçici çözümlerin bazıları zekice ve kullanışlı, bazıları ise berbattır.

## 3.1 Birleştirme
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Birleştirme, çok sayıda küçük görüntüyü tek bir büyük görüntü olması için bir araya getirdiğinizde bu durumu tanımlamak için sıklıkla kullanılan bir terimdir. Ardından daha küçük olanlarını göstermek adına büyük resmin parçalarını kesmek için javascript'i veya CSS'yi kullanırsınız.

Bir site bu hileyi hız için kullanır. HTTP 1.1'de tek bir büyük görüntü elde etmek, 100 küçük görüntüyü tek tek bir araya getirmekten daha hızlıdır.

Tabi ki bu, sitenin yalnızca bir veya iki küçük resmi ve benzerlerini göstermek isteyen site sayfaları için dezavantajlara sahiptir. Aynı zamanda, muhtemelen en çok kullanılan resimlerin kalmasına izin vermek yerine, tüm resimleri aynı anda önbellekten çıkarılabilecek hale getirir.

## 3.2 İçerilme

İçerilme, resimleri tek tek göndermekten kaçınmanın diğer bir hilesidir ve bu verileri kullanarak yapılır. Kaynak Konumlandırıcılar(URLs) css dosyalarında gömülmüştür. Bu birleştirme olayında olduğu gibi benzer faydalara ve zararlara sahiptir.


    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Bitiştirme

Büyük bir site, bir sürü farklı javascript dosyası ıle sonuçlanabilir. Kullanıcı arayüzünü kontrol eden araçlar, geliştiricilerin hepsini bir araya getirmelerine yardım ederek tarayıcının onlarca küçük dosya yerine tek bir büyük dosyaya ulaşmasını sağlar. Çok az veri gerektiğinde çok fazla veri gönderilir. Bir değişiklik yapılması gerektiğinde çok fazla veri yeniden yüklenmelidir.

Bu uygulama tabi ki çoğunlukla söz konusu geliştiricilere rahatsızlık veriyor.

## 3.4 Püskürtme

Soz edecegım nihai performans hilesi sıklıkla "püskürtme" olarak adlandırılır. Temel olarak hizmetinizin mümkün olabildiğince çok sayıda farklı barındırıcıya hizmet etmesi anlamına geliyor. İlk bakışta bu garip gözükse de bunun arkasında bır mantık vardır.

Başlangıçta HTTP 1.1 beyannamesi, bir istemcinin her bir ana bilgisayar için en fazla iki TCP bağlantısı kullanmasına izin verdiğini belirtti. Dolayısıyla, akıllı siteleri ihlal etmemek için, yeni barındırıcı adları keşfedildi ve sitenize daha fazla bağlantı kurabilir – voilà - ve sayfa yükleme sürelerini azaltabilirsiniz.

Zamanla bu sınırlama kaldırıldı ve bugün müşteriler barındırıcı başına 6-8 bağlantıyı kolayca kullanıyor ancak sitelerin bağlantı sayısını artırmak için bu tekniği kullanmaya devam etmesi için hala bir sınırı var. Nesnelerin sayısı arttıkça -daha önce de gösterildiği gibi- çok sayıda bağlantı daha sonra HTTP'nin iyi performans gösterdiğinden ve sitenizi daha hızlı hale getirdiğinden emin olmak için kullanılır. Sitelerin bu tekniği kullanarak tek bir site için 50'den fazla hatta 100'e kadar veya daha fazla bağlantıyı kullanması olağandışı değildir. Httparchive.org tarafından yayınlanan son istatistikler, siteyi görüntülemek için dünyanın en büyük 300K URL'lerinin ortalama 40(!) TCP bağlantısı gerektirdiğini ve eğilim bunun zaman içinde yavaş ilerlediğini gösteriyor.

Başka bir sebep de, bu günlerin oldukça önemli olabileceği gibi, çerezleri kullanmayan ayrı bir ana makine adına resim veya benzeri kaynaklar koymaktır. Çerezsiz resim hostlarını kullanarak bazen çok daha küçük HTTP isteklerine izin vererek performansı arttırabilirsiniz!

Aşağıdaki resim, İsveç'in en iyi web sitelerinden birine göz atarken, taleplerin çeşitli ana bilgisayar adları üzerinden nasıl dağıtıldığını ve bir paket izinin nasıl göründüğünü göstermektedir.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)