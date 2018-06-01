# http2-koncept

Så vad skulle http2 åstadkomma? Vad var gränserna för vad HTTPbis-gruppen
satte sig för att göra?

De var faktiskt ganska strikta och satte ett antal begränsningar för gruppens
möjligheter att förnya protokollet.

- Det måste behålla HTTP:s paradigmer. Det måste fortfarande ett protokoll där
  klienten skickar förfrågningar till en server över TCP.

- http://- och https://-URLer kan inte ändras. Det kan inte göras ett nytt
  schema. Mängden innehåll som redan använder sådana URLer är helt enkelt för
  stor för att tro att de kan ändras.

- HTTP1-servrar och klienter kommer finnas kvar i decennier, vi måste kunna
  erbjuda proxys för http2-servrar.

- Därför måste proxys kunna mappa http2-funktioner till HTTP 1.1-klienter, ett
  till ett.

- Ta bort eller minska antalet valbara delar i protokollet. Det var inte
  riktigt ett krav men mer som ett mantra som följde med från SPDY och
  Google-teamet. Genom att se till att allting var obligatoriskt så finns det
  inte något sätt som man kan undvika att implementera allt nu och sedan
  upptäcka problemen i framtiden.

- Ingen mera fraktionsdel i versionsnumret. Det beslöts att klienter och
  servrar är antingen kompatibla med http2 eller så är de det inte. Om det
  kommer ett behov att utöka protokollet eller ändra saker så kommer http3 att
  skapas. Det finns inte något "minor version" i http2.

## 5.1. http2 för existerande URI-scheman

Som tidigare nämnts så kan inte existerande URI-scheman ändras, så http2 var
tvunget att göras med de redan existerande. Eftersom de används för HTTP 1.x
idag så behövde vi förstås ett sätt att uppgradera protokollet till http2
eller på något vis be servern använda http2 istället för äldre protokoll.

HTTP 1.1 har ett definierat sätt att göra detta på, nämligen genom
Upgrade:-headern, som tillåter att server skickar tillbaks ett svar som
använder det nya protokollet när den den fått en sådan förfrågan över det
gamla. Till priset av en tur-och-retur runda.

Det priset av en tur-och-retur runda var något som SPDY-teamet inte kunde
acceptera och eftersom de också implementerade SPDY över TLS utvecklade de ett
nytt TLS-tillägg som används som en genväg för att förkorta förhandlingen
ganska rejält. Genom att nyttja det tillägget, kallat NPN för Next Protocol
Negotiation, kan servern berätta för klienten vilka protokoll den kan och
klienten kan fortsätta med det protokoll den föredrar.

## 5.2. http2 för https://

En stor del av fokus för http2 har lags på att få det att bete sig ordentligt
över TLS. SPDY används bara över TLS och det har varit en kraftigt tryck för
att göra TLS obligatoriskt för http2, men det fick aldrig konsensus varvid
http2 skeppas med TLS valbart. Hursomhelst, två prominenta implementatörer har
tydligt sagt att de bara kommer implementera http2 över TLS: Mozilla
Firefox-ledaren och ledaren för Googles team. Två av de ledande webbläsarna
idag.

Anledningar att välja endast TLS inkluderar respekt för användarnas integritet
samt att tidiga mätningar visar att nya protokoll har en mycket högre chans
till att fungera när de görs över TLS. Det är på grund av den utspridda
uppfattningen att allt som går över port 80 är HTTP 1.1 och det får en del
mellan-boxar att blanda sig i och förstöra trafik när det faktiskt är något
annat protokoll som pratas där.

Obligatorisk TLS är ett ämne som orsakat en mycket handviftande och upprörda
röster på mailinglistor och möten - är det bra eller är det ondska? Det är ett
infekterat ämne - var medveten om detta när du kastar den här frågan i
ansiktet på en HTTPbis-deltagare!

Likaså var det en hård och lång debatt om huruvida http2 skulle diktera en
lista med chiffer som skulle vara obligatoriska när man använder TLS, eller om
det kanske skulle vara en svartlista eller om det inte skulle kräva nånting
alls från TLS-"lagret" utan lämna det till TLS-arbetsgruppen. Det som
slutligen hamnade i specen är att TLS måste vara minst version 1.1 och det
finns krav på vilka chiffer som måste användas.

## 5.3. http2-förhandling över TLS

Next Protocol Negotiation (NPN), är tillägget som användes i SPDY för att
förhandla med TLS-servrar.  Eftersom det inte var en riktig standard så togs
det till IETF och igenom och det som kom ut blev ALPN: Application Layer
Protocol Negotiation. ALPN är det som nu lyfts upp för att användas i http2,
medan SPDY-klienter och -servrar fortsätter använda NPN.

Det faktum att NPN fanns först och att ALPN tog ett stund att gå igenom
standardiseringsprocessen har lett till att flera tidiga http2-klienter och
servrar är gjorda att använda båda dessa tillägg när de förhandlar
http2. Vidare, eftersom NPN används för SPDY och många servrar ju stöder både
SPDY och http2 så är det vettigt att stödja både NPN och ALPN på sådana servrar.

ALPN skiljer sig i huvudsak från NPN genom vet det är som bestämmer vilket
protokoll som pratas. Med ALPN är det klienten som ger servern en lista med
protokoll i den ordningen den föredrar att använda dem, och servern väljer det
protokoll den vill ha, medan för NPN så är det klienten som gör det slutliga
valet.

## 5.4. http2 för http://

Som tidigare nämnts i förbifarten, för klartext-HTTP 1.1 så är mekanismen att
förhandla http2 att fråga servern med en Upgrade:-header. Om servern då pratar
http2 svarar den med en "101 Byter" status och från då och framöver pratar den
istället http2 på det kopplet. Du inser förstås att den
uppgraderingsproceduren kostar en hel nätverks fram-och-tillbaka-tur men en
fördel är att ett http2-koppel bör vara möjligt att hålla levande och
återanvända i mycket högre grad än HTTP1-koppel generellt är.

Medan vissa webbläsares talespersoner har sagt att de inte kommer implementera
det här sättet att prata http2, så sade Internet Explorer-teamet en gång i
tiden att de skulle göra det - även om de sedan aldrig leverat det. curl och
en del andra icke-webbläsarklienter stöder http2 i klartext.

Idag stöder ingen av de stora webbläsarna http2 utan TLS.
