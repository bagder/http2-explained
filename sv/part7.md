# 7. Utökningar

Protokollet kräver att en mottagare måste ta emot och ignorera alla paket som
innehåller okända paket-typer. Två parter kan därmed förhandla om en
användning av nya paket-typer på hop-by-hop basis (dvs överenskommelsen
gäller endast mellan dessa två ändpunkter och inte längre än så), och de paket
kan då inte ändra state och de kan inte flödeskontrolleras.

Frågan huruvida http2 skulle tillåta utökningar eller inte debatterades länge
medan protokollet utvecklades med åsikter som svängde åt båda hållen. Efter
draft-12 svängde pendeln tillbaks en sista gång och utökningar tilläts igen.

Utökningar är därmed inte del av det egentliga protokollen utan kommer
dokumenteras utanför huvudspecen. Redan nu finns det två paket-typer som har
diskuterats för att inkluderas i protokollet och som troligen tillhör de
första typerna att skickas som utökningar. Jag beskriver dem här bara på grund
deras popularitet och deras tidigare roll som "inhemska" typer.

## 7.1. Alternativa Tjänster

När http2 antas, finns det anledning att misstänk att TCP-koppel kommer bli
mycket långvariga och hållas levande mycket längre än vad HTTP 1.x-kopper
någonsin hållits. En klient bör kunna göra mycket av vad den vill över ett
enda koppel per varje host/sajt och det enda kopplet kan då potentiellt hållas
uppe riktigt länge.

Detta påverkar hur HTTP-loadbalancerare arbetar och det kan komma situationer
där en sajt vill annonsera och föreslå att klienten kopplar upp sig mot en
annan host. Det kan vara för prestandans skull men även om en sajt håller på
att tas ner för underhåll och liknande.

Servern kommer då skicka
[Alt-Svc:-headern](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07)
(eller ALTSVC-paketet över http2) och berätta för klienten om en alternativ
tjänst. En annan rutt till samma innehåll erbjudet av en annan tjänst, host
och portnummer.

En klient är då tänkt att försöka koppla upp sig mot den tjänsten asynkront
och bara använda alternativet ifall det fungerar bra.

### 7.1.1. Opportunistisk TLS

Alt-Svc-headern tillåter en server som tillhandahåller innehåll över http://
att informera klienten att samma innehåll även finns tillgängligt över ett
TLS-koppel.

Detta är en någon omdiskuterad funktion. Sådana koppel använder o-autentiserad
TLS och kommer inte visas som "säkra" någonstans, kommer inte använda något
hänglås i gränssnittet eller faktiskt inte på något vis berätta för användaren
att det inte är gammal hederlig HTTP. Men det är fortfarande opportunistisk
TLS och en del människor är emot det konceptet väldigt starkt.

## 7.2. Blockad

Ett paket av den här typen är tänkt att skickas exakt en gång av en http2-part
när denne har data att skicka men flödeskontroll förbjuder den att skicka
data. Tanken är att ifall din implementation tar emot ett sådant paket så vet
du att din implementation har strulat till någonting och/eller du får mindre
än optimal överföringshastighet på grund av det.

Ett citat från draft-12, innan det här paketet togs ut och blev en utökning:

> “Paket-typen BLOCKED är med i den här draft-versionen för att erbjuda
> experimentering.  Om resultaten av experimenten inte resulterar i positiv
> feedback kommer den tas bort.
