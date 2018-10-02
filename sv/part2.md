# 2. HTTP idag

HTTP 1.1 har förvandlats till ett protokoll som används för i princip allting
på Internet. Stora investeringar har gjorts i protokoll och infrastruktur som
utnyttjar detta. Det är draget så långt att det idag oftast är lättare att få
saker att köra över HTTP hellre än att bygga något som är helt nytt och eget.

## 2.1 HTTP 1.1 är stort

När HTTP skapades och slängdes ut i världen ansågs det antagligen vara ett
ganska enkelt och rakt-fram protokoll, men tiden har bevisat att detta inte är
sant. HTTP 1.0 är RFC 1945 som är en 60 sidors specifikation släppt 1996. RFC
2616, som beskriver HTTP 1.1, släpptes bara tre år senare 1999, och växte
avsevärt till sina 176 sidor. Trots att den redan växt så, delade IETF senare
upp specifikationen i sex delar under arbetet med att uppdatera den, med ett
mycket högre totalt sidantal. Det som blev RFC 7230 med familj. Hur man än
räknar är HTTP 1.1 stort och inkluderar myriader av detaljer, nyanser och inte
minst valbara delar.

## 2.2 En värld av alternativ

HTTP 1.1:s natur med massor av små detaljer och alternativ tillgängliga för
senare utökningar har lett till ett ekosystem av programvara där nästan ingen
implementation någonsin implementerar allting - och det är inte ens möjligt
att riktigt säga vad "allting" är. Det ledde till en situation där funktioner
i protokollet som utnyttjades väldigt lite ofta inte alls implementerades - i
början - och de fall när någon faktiskt implementerade dem såg de väldigt
liten användning.

Senare skapade detta interoperabilitetsproblem när klienter och servrar väl
började använda såna funktioner. HTTP pipelining är kanske det främsta exempel
på en sådan funktion.

## 2.3 Otillräckligt nyttjande av TCP

HTTP 1.1 har svårt att verkligen till fullo nyttja all kraft och den prestanda
som TCP erbjuder. HTTP-klienter och webbläsare måste vara väldigt kreativa för
att hitta lösningar som minskar laddningstider.

Andra försök som pågått under åren har också bekräftat att TCP inte är så
enkelt att ersätta och därför fortsätter vi att både förbättra TCP och de
protokoll som vi kör ovanpå det.

Enkelt uttryckt, TCP kan användas bättre för att undvika pauser och ögonblick
i tiden som kunde användas till att skicka eller ta emot mer data. De följande
avsnitten belyser några av de existerande bristerna.

## 2.4 Överföringsstorlekar och antal objekt

När man tittar på trenden för några av de mest populära sajterna på webben
idag och vad det krävs för att ladda ner deras framsidor, så ser man ett
tydligt mönster. Genom åren har mängden data som behöver laddas ner gradvis
ökat till och förbi 2.1MB. Vad som är än viktigare i det här sammanhanget är
att i genomsnitt så behövs det över ett hundra enskilda objekt för att visa
varje sida.

Som diagramet nedan visar, så har trenden pågått ett tag och det finns inga
indikationer på att den kommer ändras inom kort. Den visar tillväxten av
överföringsstorlek (i grönt) och det totala antalet förfrågningar som använts
i genomsnitt (i rött) för att visa de mest populära webbsajterna i världen, och
hur det har förändrats de senaste fyra åren.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Fördröjning dödar

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 är väldigt känsligt för fördröjningar (latency), delvis för att HTTP
Pipelining fortfarande är fyllt med problem och är avslaget per default hos en
stor andel av användarna.

Medan vi sett en rejäl ökning i tillgänglig bandbredd hos människor över de
senaste åren, så har vi inte sett samma förbättring i att minska
fördröjningar. Länkar med stora fördröjningar, som många nuvarande mobila
teknologier, gör det riktigt svårt att få en bra och snabb upplevelse av
webben, även om du har en riktigt hög bandbredd på uppkopplingen.

Ett annat användningsfall som verkligen behöver låga fördröjningar är vissa
former av video, som till exempel videokonferenser, spel och liknande, där det
inte bara är en på förhand genererad ström att skicka ut.

## 2.6. Först-i-kön-blockering

HTTP Pipelining är ett sätt att skicka en till förfrågan till servern medan
klienten fortfarande väntar på svaret på den förra frågan. Det är
väldigt likt en kö på banken eller till kassan i en mataffär. Du vet helt
enkelt inte om personen framför dig i kön är en snabb kund eller den där
irriterande personen som kommer ta en evighet innan hon/han är klar:
först-i-kön-blockering.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Visst, du kan välja kö omsorgsfullt så att du ökar chansen att verkligen ta
rätt kö, och ibland kan du till och med starta en ny, egen kö, men hur du än
gör så kan du inte undvika att ta ett beslut och när det väl är taget kan du
inte byta kö.

Skapa nya köer är också belagt med prestanda- och resurs-förluster så det
skalar inte upp bra till mer än ett ganska lågt antal köer. Det finns helt
enkelt ingen perfekt lösning för detta.

Även idag, 2015, så har de flesta desktop-webbläsare HTTP pipelining avslaget
per default.

Vidare fördjupning på det här ämnet kan man hitta i till exempel Firefox
[bugzilla 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
