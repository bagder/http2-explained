# 10. http2 i Chromium

Chromium-teamet har implementerat http2 och stött det i dev- och beta-kanaler
under en lång tid. Med början i Chrome 40, släppt den 27:e januari 2015, så är
http2 påslaget per default för en viss mängd användare. Det började med ett
fåtal och har sedan gradvis ökats.

SPDY-support kommer tas bort framöver enligt en bloggpost från projektet
postat i [februari
2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> "Chrome har stött SPDY sedan Chrome 6, men eftersom de flesta av fördelarna finns i HTTP/2, är det dags att säga adjö. Vi planerar att ta bort stödet för SPDY i början av 2016.

## 10.1. Först, se till att det är påslaget

Skriv in “chrome://flags/#enable-spdy4" i din webbläsares adressfält och klick
“enable” om det inte redan säger att det är påslaget.

## 10.2. Endast TLS

Kom ihåg at Chrome bara implementerat http2 över TLS. Du kommer endast see
http2 i aktion i Chrome när du går til https://-sajter som erbjuder http2-support.

## 10.3. Se HTTP/2-anvädning

Det finns Chrome-tillägg tillgängliga som hjälper att visualisera om en sajt
använder http2. En av dem är [“SPDY
Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Chromes nuvarande experiment med QUIC (se sektion 12.1) späder ut
http2-siffrorna något.
