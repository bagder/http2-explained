# 12. http2 sonrası

Http2 için çok zor kararlar ve uzlaşmalar yapılmıştır. Http2'nin dağıtılmasıyla birlikte, ileride daha fazla protokol revizyonu yapmanın temelini oluşturan diğer protokol sürümlerine yükseltme için önceden belirlenmiş bir yol vardır. Aynı zamanda birden fazla farklı versiyonu paralel olarak işleyen bir kavram ve bir altyapı da getiriyor. Belki yeni tanıttığımızda eskilerini tamamen ortadan kaldırmamız gereklidir?

HTTP2, HTTP 1 ve http2 arasında ileri geri trafiği proxy vasıtasıyla tutma arzusundan dolayı, geleceğe getirilen bir sürü HTTP 1 "miras" içeriyor. Bu mirastan bazıları daha fazla gelişme ve icatlara engel oluyor. Belki de http3 bazılarından kurtulabilir.

Hâlâ http'de neyin eksik olduğunu düşünüyorsunuz?

## 12.1. QUIC

Google'ın [QUIC] (https://www.chromium.org/quic) (Hızlı UDP İnternet Bağlantıları) protokolü, SPDY ile aynı tarzda ve ruhta çok ilginç bir deneydir. QUIC, UDP kullanılarak gerçekleştirilen TCP + TLS + HTTP / 2 birleşimidir.

QUIC, çok daha az gecikme ile bağlantıların oluşturulmasına izin verir, sadece HTTP / 2 için olduğu gibi her biri için değil, bireysel akışları engellemek için de paket kaybını çözer ve farklı ağ arayüzleri üzerinden kolayca bağlantı yapılmasını sağlar, dolayısıyla MPTCP'nin çözeceği alanları da kapsar.

QUIC şimdiye kadar yalnızca Google tarafından Chrome'da uygulanmaktadır ve bu kod, bir [libquic] (https://github.com/devsisters/libquic) çabasıyla tam olarak çalışılsa bile başka yerlerde kolayca yeniden kullanılamaz. Protokol IETF ulaştırma çalışma grubuna [taslak] (https://tools.ietf.org/html/draft-tsvwg-quic-protocol-01) olarak getirildi.
