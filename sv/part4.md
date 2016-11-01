# 4. Uppdatera HTTP

Skulle det inte vara trevlig att göra ett förbättrat protokoll? Det skulle inkludera...

1. Göra protokollet mindre fördröjningskänsligt
2. Fixa pipelining- och först-i-kön-blockeringsproblemen
3. Ta bort behovet och önskan att fortsätta öka antalet koppel per värddator
4. Behåll alla existerande gränssnitt, allt innehåll, URI-format och dess scheman
5. Gör det inom IETF:s HTTPbis-arbetsgrupp

## 4.1. IETF och HTTPbis-arbetsgruppen

Internet Engineering Task Force (IETF) är en organisation som utvecklar och
främjar nya internetstandarder. Mest på protokollnivå. De är vitt kända för
serien av RFC-dokument som täcker allt från TCP, DNS, FTP till "best
practices", HTTP och mängder med protokollvarianter som aldrig kom någon vart.

Inom IETF formas arbetetsgrupper (working groups) med ett avgränsat
arbetsområde som jobbar mot ett mål. De etablerar en stadga med ett antal
riktlinjer och begränsningar för vad de ska åstadkomma. Alla som vill är
tillåtna att delta i diskussionerna och utvecklingen. Alla som deltar och
säker något har samma vikt och chans att påverka utgången, och alla är där som
enskilda individer. Det läggs väldigt liten vikt vid vilka företag individerna
jobbar för.

HTTPbis-gruppen (se nedan för en utvikning runt namnet) startades under
sommaren 2007 för att jobba med en uppdatering av HTTP
1.1-specifikationen. Diskussionen om en nästa versions HTTP startade inom
gruppen under slutet av 2012. Uppdateringen av HTTP 1.1 avslutades tidigt 2014
och resulterade i [RFC 7320](https://tools.ietf.org/html/rfc7320)-serien.

Det sista interop-mötet för HTTPbis-gruppen hölls i New York City i början av
juni 2014. De kvarvarande diskussionerna och avslutande IETF-procedurerna för
att verkligen få till en officiell RFC skulle visa sig fortsätta in på det
följande året.

Några av de allra största spelarna inom HTTP har saknats inom arbetsgruppens
diskussioner och möten. Jag vill inte nämna eller peka ut något särskilt
företags eller produktnamn här, men helt klart så verkar vissa aktörer på
Internet vara väldigt säkra på att IETF kommer göra rätt även utan att de är
inblandade.

### 4.1.1. Om "bis"-delen i namnet

Gruppen heter HTTPbis där "bis"-delen kommer från det [latinska adverbet för
två](https://sv.wiktionary.org/wiki/bis).  Bis används ofta som suffix eller
del av namnet inom IETF för en uppdatering en andra version av en spec. Precis
som i fallet för HTTP 1.1.

## 4.2. http2 började från SPDY

[SPDY](http://en.wikipedia.org/wiki/SPDY) är ett protokoll som utvecklades och
togs fram av Google. De utvecklade det visserligen öppet och bjöd in alla som
vill att delta, men det var tydligt att de utnyttjade sin situation med att
kontrollera både en populär webbläsare och ett signifikant server-bestånd med
välanvända tjänster.

När HTTPbis-arbetsgruppen bestämde att det var dags att börja jobba på http2
hade SPDY redan bevisats vara ett fungerande koncept. Det hade visat att det
var möjligt att driftsätta på Internet och det presenterade siffror som bevisade
hur bra det presterade. http2-arbetet började därmed från SPDY/3-utkastet som
helt enkelt gjordes om till http2 draft-00 med lite sök och ersätt.
