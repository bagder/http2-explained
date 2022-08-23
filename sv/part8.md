# 8. En http2-värld

Så hur kommer saker fungera när http2 blir använt? Kommer det att användas?

## 8.1. Hur kommer http2 påverka vanliga människor?

http2 har ännu inte utbredd användning. Vi kan inte säga med säkerhet exakt
hur saker kommer utvecklas. Vi har sett hur SPDY har använts och vi kan gissa
och räkna baserat på det och andra experiment som gjorts tidigare.

http2 minskar antalet turer fram och tillbaks över nätverket och det undviker
först-i-kön-blockeringsproblemen genom att multiplexa och att kunna avsluta
oönskade strömmar snabbt.

Det tillåter ett stort antal parallella strömmar som i antal vida överstiger
antal koppel hos även de mest shardade sajterna av idag.

Med prioriteter använda ordentligt på strömmarna finns chansen att klienten
på ett mycket bättre sätt kommer kunna få den viktiga datan före den mindre
viktiga
datan. Allt sammantaget, skulle jag säga att chanserna är väldigt goda att det
kommer leda till snabbare sidladdning och mer responsiva webbsajter. Kort
sagt: en bättre webbupplevelse.

Hur mycket snabbare och hur mycket förbättringar vi kommer se tror jag inte vi
kan säga ännu. Först och främst är ju teknologin fortfarande väldigt ung och
sen har vi knappt ens börjat se klienter och servrar trimma sina
implementationer till att verkligen dra nytta av alla de möjligheter detta nya
protokoll erbjuder.

## 8.2. Hur kommer http2 påverka webbutveckling?

Genom åren har webbutvecklare och webbutvecklarmiljöer samlat på sig
verktygslådor fulla med trick och verktyg för att jobba runt problem med HTTP
1.1, precis som jag beskrev i början av det här dokumentet som en förklaring
till varför http2 togs fram.

Många av de trick som verktyg och utvecklare nu använder per default och utan
att tänka efter kommer troligen skada http2-prestanda eller åtminstone inte
riktigt nyttja den fulla potentialen som http2 erbjuder. Spriting och inlining
ska med största säkerhet inte användas med http2. Sharding kommer troligen att
vara direkt skadligt för http2 som kommer tjäna på att ha färre koppel.

Ett problem här är ju att webbsajter och webbutvecklare behöver utveckla och
drifta för en värld där det åtminstone på kort sikt kommer finnas både
HTTP1.1- och http2-klienter som användare och att nå maximal prestanda för
alla användare kan bli utmanande utan att drifta två olika front-ends.

Av enbart dessa anledningar tror jag det kommer ta en tid innan vi kommer nå
http2s fulla potential.

## 8.3. http2-implementationer

Att försöka dokumentera specifika implementationer i ett dokument som det här
är förstås helt futilt och dömt at misslyckas och kommer endast kännas gammalt
redan inom kort. Istället kommer jag förklara situationen i bredare termer och
hänvisa läsare till [listan med
implementationer](https://github.com/http2/http2-spec/wiki/Implementations) på
http2-sajten.

Det fanns ett stort antal implementationer redan tidigt och antalet har ökat
under tiden vi jobbade med http2. När jag skriver detta finns det över 40
implementationer listade, och de flesta av dem implementerar den slutliga
versionen.

Firefox har varit webbläsaren som varit först med support för de allra senaste
versionerna av specen. Twitter har hängt med och erbjuder sina tjänster över
http2. Google började under april 2014 att erbjudera http2-support på en del
testservrar som kör deras tjänster och sedan maj 2014 har de erbjudit
http2-support för sin utvecklingsversion av Chrome. Microsoft har visat en "tech
preview" med http2-support i deras nästa Internet Explorer-version. Safari och
Opera har båda sagt att de kommer stöda http2.

curl och libcurl stöder osäker http2 likväl som TLS-baserad, användandes en av
flera olika TLS-bibliotek.

[H2O](https://h2o.examp1e.net/), [Apache Traffic
Server](https://trafficserver.apache.org/) och [nghttp2](https://nghttp2.org/)
har alla släppt http2-kapabla open source-servrar.

### 8.3.1. Saknade implementationer

De två otroligt populära servrarna Apache HTTPD och Nginx har båda erbjudit
SPDY support. Den 22:a september 2015 kom så Nginx med sin första version med
officiell support för http2. Nginx har släppt
["nginx-1.9.5"](https://www.nginx.com/blog/nginx-1-9-5/) och http2-modulen för
Apache heter [mod_h2](https://icing.github.io/mod_h2/) och är på väg att
släppas i en publik version "snart".

## 8.4. Vanlig kritik av http2

Under utvecklingen av det här protokollet så har debatten böljat fram och
tillbaka och det är förstås ett visst antal människor som anser att
protokollet till slut hamnade helt fel. Jag vill adressera några av de
vanligaste klagomålen och nämna några argument mot dem.

### 8.4.1. "Protokollet är designat och gjort av Google"

Det har också varianter som implicerar att världen därmed blir ännu mer
beroende och kontrollerad av Google genom detta. Det är inte sant. Protokollet
har utvecklats inom IETF på samma sätt som protokoll har utvecklats i över 30
år. Men, vi alla erkänner och kan ju bara bekräfta Googles imponerande arbete
med SPDY som inte enbart visade att det går att driftsätta ett nytt protokoll
på det här sättet utan också tillhandahöll siffror som visade vilka vinster
som kunde göras.

Google har
[annonserat](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html)
att de kommer ta bort support för SPDY och NPN i Chrome under 2016 och de
uppmanar servrar att migrera till http2 istället.

### 8.4.2. “Protokollet är endast användbart för webbläsare"

Det här är typ sant. En av de primära drivkrafterna bakom utvecklingen av
http2 var
att fixa HTTP pipeliningen. Om ditt användningsfall inte har något behov av
pipeliningen så finns det en sannolikhet att http2 inte kommer vara till
mycket nytta
för dig. Det är inte den enda förbättringen i protokollet men en stor sådan.

Så snart tjänster börjar använda den fulla kraften och möjligheterna med
multiplexade strömmar över ett enda koppel så misstänker jag att vi kommer se
fler applikationer använda http2.

Små REST-APIer och enklare programatiska användningar av HTTP 1.1 kommer
kanske inte se att steget till http2 erbjuder väldigt stora fördelar. Men
också att det är väldigt få nackdelar med http2 för de flesta användarna.

### 8.4.3. “Protokollet är bara bra för stora sajter"

Inte alls. Multiplexande strömmar kommer förbättra upplevelsen rejält för
höglatensförbindelser som ofta mindre sajter utan bred geografisk distribution
erbjuder. Stora sajter är redan väldigt ofta snabbare och mer distribuerade
med kortare tur-och-returtider till användarna.

### 8.4.4. “Dess användning av TLS gör den långsammare"

Det kan vara sant till viss utsträckning. TLS-handskakningen lägger på lite
extra men det finns redan pågående ansträngningar att reducera antalet
tur-och-retur varv ännu mer för TLS. Den extra kostnaden för att använda TLS
över kabeln istället för klartext är inte osignifikant och tydligt noterbar så
mer CPU och kraft kommer användas för samma trafikmönster än för ett osäkert
protokoll. Hur mycket och vilken effect det får är ett ämne för tyckande och
mätningar. Se till exempel [istlsfastyet.com](https://istlsfastyet.com/) för
en källa till sådan info.

Telecom och andra nätverskoperatörer, till exempel inom ATIS Open Web
Alliance, säger att [de behöver okrypterad
trafik](https://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf)
för att erbjuder cache, komprimering och andra tekniker som är nödvändiga för
att tillhandahålla en snabb webbupplevelse över satellit, i flygplan och
liknande. http2 gör inte TLS obligatoriskt så vi ska inte blanda ihop
termerna.

Många Internet-användare har uttalat sina preferenser för att TLS ska användas
mer utbrett och vi borde hjälpa till att skydda användarnas integritet.

Experiment har visat att genom att använda TLS så får man en högre grad av
framgång än när man implementerar nya klartextprotokoll över port 80, eftersom
det finns lite för många mellan-boxar där ute i världen som ingriper i det som
de tror är HTTP 1.1 om det kommer över port 80 och ibland kan se ut som HTTP.

Till slut, tack vare https multiplexande strömmar över ett fysikt koppel, så
kommer vanliga användningsfall med webbläsare ändå göra väsentligt färre
TLS-handskakningar och därmed prestera bättre än HTTPS skulle göra för HTTP
1.1.

### 8.4.5. “Att det inte är ASCII, förstör affären"

Ja, vi gillar att se protokoll i klartext eftersom det gör debuggning och
spårning enklare. Men text-baserade protokoll är också mer felbenägna och
öppnar upp för mycket mer parsning och fler parsningsproblem.

Om du verkligen inte kan hantera ett binärt protokoll, så kan du inte hantera
TLS och komprimering i HTTP 1.1 heller och det har funnits där och används
under en väldigt lång tid.

### 8.4.6. “Det är inte snabbare än HTTP/1.1”

Det är förstås ett ämne för debatt och diskussioner om hur man mäter och vad
snabbare betyder, men redan under SPDY-dagarna gjordes många tester och som
bevisade snabbare sidladdningar (som ["How Speedy is
SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf)
av folk vid University of Washington och ["Evaluating the Performance of
SPDY-enabled Web
Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers)
av Hervé Servy) och såna experiment har repeterats med http2 också. Jag ser
fram emot att få se fler såna tester och experiment publicerade. Ett
[grundläggande första test av
httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2)
kan indikera att http2 håller sina löften.

### 8.4.7. “Den bryter mot lager-principer"

Seriöst, är det ditt argument? Lager är inte heliga oberörbara pelare i en
global religion och om vi har klivit över inpå en del gråzoner när vi gjorde
http2 så har det varit med avsikten att göra ett bra och effektivt protokoll
inom de givna ramarna.

### 8.4.8. “Det fixar inte flera av HTTP 1.1s problem"

Det är sant. Med det specifika målet att behålla HTTP/1.1-paradigmer så vare
det flera gamla HTTP funktioner som var tvugna att finnas kvar. Som t.ex
headers och därmed också de ofta ogillade cookiesarna, authorization-headrar
och mer. Men med fördelen att genom att behålla dessa paradigmer fick vi ett
protokoll som det är möjligt att driftssätta utan att kräva en helt otänkbar
mängd uppgraderingsarbete där fundamentala delar måste ersättas eller skrivas
om. http2 är i grund och botten bara ett ny inramning ("framing layer").

## 8.5. Kommer http2 att driftssättas vitt och brett?

Det är för tidigt att säga säkert, men jag kan ändå gissa och estimera och det
är vad jag kommer göra här.

Nej-sägarna kommer säga "kolla hur bra IPv6 har gjort ifrån sig" som ett
exempel på ett protokoll som det tagit decennier bara för att börja bli
driftsatt brett. http2 är inte en IPv6 dock. Det här är ett protokoll ovanpå
TCP som använder vanliga HTTP-uppgraderingsmekanismer, portnummer och TLS
etc. Det kommer inte kräva att de flesta routers eller brandväggar ändras
alls.

Google bevisade för världen med deras SPDY-arbete att ett nytt protokoll som
det här kan driftsättas och bli använt av webbläsare och tjänster med multipla
implementaitoner inom en relativt kort tidsperiod. Medan antalet servrar på
Internet idag som erbjuder SPDY är på 1%-nivån, så är mängden data dessa
servrar hanterar mycket större. Några av de allra mest populära webbsajterna
idag använder SPDY.

http2, baserat på samma principer som SPDY, skulle jag säga kommer sannolikt
driftsättas ännu mer eftersom det är ett IETF-protokoll. SPDY-användningen hölls
alltid tillbaks en del av dess "det är ett Google-protokoll"-stigma.

Det finns flera stora webbläsare bakom utrullningen. Representanter från
Firefox, Chrome, Safari, Internet Explorer och Opera har sagt att de kommer
skeppa http2-kapabla webbläsare och de har visat fungerande implementaitoner.

Det finns flera stora server-operatörer som är sannolika att erbjuda http2
snart, inklusive Google, Twitter och Facebook och vi hoppas få se att
http2-support snart läggs till i populära server-implementationer som Apache
HTTPD och Nginx. H2o är en ny vrålsnabb HTTP-server med http2-stöd som visar
potential.

Några av de största proxy-tillverkarna, inklusive HAProxy, Squid och Varnish har
uttalat sina intentioner att stödja http2.

Allt igenom 2015 har mängden http2-trafik ökat. I början av September visade
användningen av Firefox 40 http2 i 13% av all HTTP-trafik och 27% av all
HTTPS-trafik, medan Google ser http2 i ungefär 18% av sin inkommande
trafik. Det kan noteras att Google driver andra experiment med nya protokoll
(Se QUIC i 12.1) vilket ger lägre http2-nivåer än det annars kunde vara.
