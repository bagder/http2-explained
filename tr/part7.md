# 7. Uzantılar

Http2 protokolü, bir alıcının bilinmeyen tüm çerçeveleri (bilinmeyen bir çerçeve türüne sahip olanları) okuması ve yoksayması şartını getirir.  İki taraf yeni çerçeve türlerinin kullanımını hop-by-hop esasına göre müzakere edebilir, ancak bu çerçeveler durum değişikliğine ve akış denetimine izin vermiyorlar.

Http2'nin uzantılara izin verip vermeyeceği konusunun tartışılması,  protokolün gelişimi boyunca farkli ve zıt görüşlerle birlikte  devam etti. Taslak-12'den sonra nihai olarak uzantılara izin verildi.

Uzantılar gerçek protokolün bir parçası değildir ve çekirdek protokolün dışında belgelendirilecektir. Protokole dahil olacak ve  muhtemelen uzantılar olarak gönderilen ilk çerçeveler olacak olan, tartışılan iki çerçeve türü vardır:

## 7.1. Alternatif Hizmetler

Http2'nin kabulüyle, TCP bağlantılarının çok daha uzun olacağından ve HTTP 1.x bağlantılarından çok daha uzun süre canlı tutulacağından şüphelenmek için nedenler vardır. Bir istemci, her ana barındırıcıda/sitede tek bir bağlantıyla çok şey yapabilir ve bu bağlantı bir süre açık olabilir.

Bir site müşterisinin başka bir barındırıcıya bağlanmasını önerdiğinde bu durum HTTP yük dengeleyicilerin çalışmasının etkilenmesine yol acabilir. Bu durum, performans nedenleriyle veya bakım vb. için olabilir.

Sunucu, müşteriye alternatif bir servis[Alt-Svc: header](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-10) (yada http2 ile birlikte ALTSVC çerçevesi) hakkında bilgi vererek gönderir: aynı içeriğe başka bir rota, başka bir servis, ana makine ve port numarası kullanarak.

Bir istemci daha sonra bu hizmete eşzamansız olarak bağlanmaya çalışmalı ve yeni bağlantı başarılı olursa alternatifi kullanmalıdır.

### 7.1.1. Fırsatçı TLS

Alt-Svc başlığı, aynı içeriğin bir TLS bağlantısı üzerinden de erişilebilir olduğunu istemciye bildirmek için http:// üzerinden içerik sağlayan bir sunucuya izin verir.

Bu biraz tartışılabilir bir özelliktir. Böyle bir bağlantı kimliği doğrulanmamış TLS yapacak, herhangi bir yerde "güvenli" olarak ilan edilemeyecek, UI'da herhangi bir asma kilit(padlock) kullanamayacak, aslında kullanıcıya düz eski HTTP olmadığını göstremeyecektir ancak bu hala fırsatçı TLS'dir ve bazı insanlar bu kavrama karşı fazlasıyla karşıdır.

## 7.2. BlokeLİ

Bu tür bir çerçeve, gönderilecek veriler olduğunda ancak akış denetimi herhangi bir veri göndermeyi yasakladığında dahi http2 tarafında tam olarak bir kez gönderilmesi amaçlanmıştır. Fikir şudur ki; eğer uygulamanız bu çerçeveyi alırsa, bir şeyi berbat ettiğinizi ve/veya mükemmel aktarım hızlarından daha azını aldığınızı biliyorsunuzdur.

Çerçeve bir uzantı haline gelmeden önce taslak-12'den bir alıntı:

> “Deneyi kolaylaştırmak için BLOKELİ çerçeve bu taslak sürüme dahil edilmiştir. Deney sonuçları olumlu geri bildirim sağlamıyorsa kaldırılabilir.”

