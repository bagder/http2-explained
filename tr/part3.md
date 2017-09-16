# 3. Gecikmelerin üstesinden gelmek için yapılanlar

Her zaman olduğu gibi sorunlarla karşı karşıya kaldıklarında, insanlar geçici çözüm bulmak için toplanırlar. Geçici çözümlerin bazıları zekice ve kullanışlı, bazıları ise berbattır.

## 3.1 Birleştirme
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Birleştirme, çok sayıda küçük görüntüyü tek bir büyük görüntü olması için bir araya getirdiğinizde bu durumu tanımlamak için sıklıkla kullanılan bir terimdir. Daha sonra javascript ve css gibi daha küçük parçaları belirlemek adına bu birleşim daha küçük parçalara bölünür, bu da kesme olarak ifade edilir.

Bir site bu hileyi hız için kullanır. HTTP 1.1'de Tek bir büyük görüntü elde etmek, 100 küçük görüntüyü tek tek bir araya getirmekten daha hızlıdır.

Tabi ki bu, sitenin yalnızca bir veya iki küçük resmi ve benzerlerini göstermek isteyen site sayfaları için dezavantajları vardır. 
Aynı zamanda bu durum, muhtemelen en çok kullanılan resimlerin gösterilmesine izin vermek yerine, tüm resimleri aynı anda önbellekten çıkarılabilecek hale getirir.

## 3.2 İçerilme

İçerilme, resimleri tek tek göndermekten kaçınmanın diğer bir hilesidir ve bu kullanılan veri ile gerçekleşir. Kaynak Konumlandırıcılar(URLs) css dosyalarında gömülmüştür. Bunun birleştirme'de olduğu gibi benzer faydaları ve zararları vardır.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Bitiştirme

Büyük bir site, bir sürü farklı javascript dosyası bulundurabilir. Kullanıcı arayüzünü kontrol eden araçlar, geliştiricilerin hepsini bir araya getirmelerine yardım ederek, tarayıcının onlarca küçük dosya yerine tek bir büyük dosyaya ulaşmasını sağlar. Çok az veri gerektiğinde çok fazla veri gönderilir. Bu yüzden, bir değişiklik yapılması gerektiğinde çok fazla veri yeniden yüklenmelidir.

Bu uygulama tabi ki çoğunlukla söz konusu geliştiricilere zahmet veriyor.

## 3.4 Püskürtme

Nihai performans hilesi ben sıklıkla "püskürtme" olarak bahsedeceğim. Bu basitçe şu anlama geliyor; olabildiğince çok sayıda farklı barındırıcıya hizmet edilmesi. İlk bakışta bu tuhaf gözükse de bunun arkasında bır mantık vardır.

Başlangıçta HTTP 1.1 beyannamesi, bir istemcinin her bir ana bilgisayar için en fazla iki TCP bağlantısı kullanmasına izin verildiğini belirtti. Dolayısıyla, akıllı siteleri ihlal etmemek için, yeni barındırıcı adları keşfedildi ve böylece voilà tekniği sitenize daha fazla bağlantı sağlayabilir ve sayfa yükleme sürelerini azaltabilirsiniz.

Zamanla bu sınırlama kaldırıldı ve bugün müşteriler barındırıcı başına 6-8 bağlantıyı kolayca kullanıyor ancak hala limit vardır, bu nedenle siteler bağlantı sayısını arttırmak için tekniği kullanmaya devam ediyor. Nesnelerin sayısı arttıkça, daha önce de gösterildiği gibi, çok sayıda bağlantı olması, HTTP'nin iyi performans göstermesinden ve sitenizi daha hızlı hale getirdiğinden emin olmak için kullanılır. Sitelerin bu tekniği kullanarak tek bir site için 50'den fazla, hatta 100'e kadar veya daha fazla bağlantıyı kullanması olağandır. Httparchive.org tarafından yayınlanan son istatistikler, siteyi görüntülemek için dünyanın en büyük 300K URL'lerinin ortalama 40 TCP bağlantısı gerektirdiğini ve eğilim bunun zaman içinde yavaş ilerlediğini gösteriyor.

Bir başka sebep de, resimler veya benzeri kaynakları, çerezleri kullanmayan ayrı bir barındırıcı adına koymaktır; çünkü bu günlerde çerezlerin boyutu oldukça önemli olabilir. Bazen çerezsiz resim barındırıcıları kullanarak çok daha küçük HTTP isteklerine izin verebilir ve böylece performansı arttırabilirsiniz!

Aşağıdaki resim, İsveç'in en iyi web sitelerinden birinde, taleplerin çeşitli ana barındırıcı adları üzerinden nasıl dağıtıldığını ve bir paket izinin nasıl göründüğünü göstermektedir.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
