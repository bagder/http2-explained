# 4. HTTP güncellenmesi

Geliştirilmiş bir protokol yapmak hoş olmaz mıydı? Elbette olurdu...

1. Gecikmelere daha duyarlı olan
2. Boru hattı ve satır başı engelleme sorununu çözen
3. Her bir sunucuya olan bağlantı sayısını artırma ihtiyacını ortadan kaldıran
4. Mevcut tüm arayüzleri, tüm içeriği, link formatlarını ve düzenlerini saklayan
5. IETF'nin HTTPbis çalışma grubu içinde oluşan

## 4.1. IETF ve HTTPbis çalışma grubu

The Internet Engineering Task Force (IETF) çoğunlukla protokol seviyesinde, internet standartlarını geliştiren ve tanıtan bir organizasyondur. TCP, DNS, FTP'den en iyi uygulamalara, HTTP'ye ve çok sayıda protokol türevine varıncaya kadar herşeyi belgeleyen RFC dökümanları için dünya çapında bilinirler.
 
IETF'de, bir hedefe doğru çalışmak için "çalışma grupları" sınırlı bir kapsam ile oluşturulmuştur.  Ortaya koyduklaı şey için bazı belirlenmiş yönergeler ve sınırlamalar içeren bir "tanımlama"(charter) oluştururlar. Tartışmalara ve gelişmelere herkesinkatılmasına müsade ederler.  Katılmak ve bir şeyler söylemek konusunda herkes, eşit haklara ve ağırlığa sahiptir ve herkes, çalıştığı yeri yok sayarcasına bir birey olarak sayılır.

Everyone and anyone is allowed to join in the discussions and development. Everyone who attends and says something has the same weight and chance to affect the outcome and everyone is counted as an individual, with little regard to which company they work for.

The HTTPbis working group (see later for an explanation of the name) was
formed during the summer of 2007 and tasked with creating an update of the
HTTP 1.1 specification. Within this group the discussions about a next-version
HTTP really started during late 2012. The HTTP 1.1 updating work was completed
early 2014 and resulted in the [RFC 7230](https://tools.ietf.org/html/rfc7230)
series.

The final inter-op meeting for the HTTPbis WG was held in New York City in the beginning of June 2014. The remaining discussions and the IETF procedures performed to actually get the official RFC out continued into the following year.

Some of the bigger players in the HTTP field have been missing from the working group discussions and meetings. I don't want to mention any particular company or product names here, but clearly some actors on the Internet today seem to be confident that IETF will do good without these companies being involved...

### 4.1.1. The "bis" part of the name

The group is named HTTPbis where the "bis" part comes from the [Latin adverb for two](http://en.wiktionary.org/wiki/bis#Latin).  Bis is commonly used as a suffix or part of the name within the IETF for an update or the second take on a spec; in this case, the update to HTTP 1.1.

## 4.2. http2 started from SPDY

[SPDY](http://en.wikipedia.org/wiki/SPDY) is a protocol that was developed and spearheaded by Google. They certainly developed it in the open and invited everyone to participate but it was obvious that they  benefited by being in control over both a popular browser implementation and a significant server population with well-used services.

When the HTTPbis group decided it was time to start working on http2, SPDY had already proven that it was a working concept. It had shown it was possible to deploy on the Internet and there were published numbers that proved how well it performed. The http2 work began with the SPDY/3 draft that was basically made into the http2 draft-00 with a little search and replace.
  
