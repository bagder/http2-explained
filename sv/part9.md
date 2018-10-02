# 9. http2 i Firefox

Firefox har varit hack-i-häl med draftarna och har erbjudit
http2-testimplemenationer under många månader. Under utvecklingen av
http2-protkollet har klienter och servrar måste komma överens om vilken
draft-version av protokollet de implementerat vilket har gjort det lite lätt
irriterande att köra tester. Var bara medveten och kontrollera att din klient
och server är överens om vilken protokoll-draft de implementerar.

## 9.1. Först, se till att det är påslaget

I alla Firefox-versioner sedan version 35, släppt 13:e januari 2015, är http2-
support påslaget per default.

Skriv in 'about:config' i adress-fältet och sök efter ett alternativ som heter
“network.http.spdy.enabled.http2draft”. Se till att den är att till *true*
Firefox 36 lade till en annan config-variabel med namnet
“network.http.spdy.enabled.http2” vilken är *true* per default. Den senare
kontroller den "vanliga" http2-versionen medan den första slår på och av
förhandling av http2-draftversioner. Båda är true per default sedan Firefox 36.

## 9.2. Bara TLS

Kom ihåg att Firefox bara implementerar http2 över TLS. Du kommer bara se
http2 i aktion när du går till https://-sajter som erbjuder http2.

## 9.3. Transparant!

![transparant http2-användning](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Det finns inget gränssnittselement någonstans som berättar att du pratar
http2. Du kan helt enkelt inte enkelt se det. Ett sätt att lista ut det är att
välja “Web developer->Network” och kontrollera svars-headrar och se vad du
fick tillbaks från servern. Svaret är “HTTP/2.0”-någonting och Firefox stoppar
in sin egen header som heter “X-Firefox-Spdy:” som kan ses på skärmdumpen
ovan.

Headrarna du ser i nätverks-verktyget när du pratar http2 har konverterats
från https binära format till gammaldags HTTP-1.x-liknande headers.

## 9.4. Se http2-användning

Det finns Firefox-tillägg som hjälper till att visualisera om en sajt använder http2. En av dem är [“SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/).
