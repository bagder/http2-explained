# 6. http2-protokollet

Nog om bakgrunden, historien och politiken bakom det som tog oss hit. Låt oss
dyka ner i detaljerna i protokollet. De bitarna och koncepten som gör http2.

## 6.1. Binärt

http2 är ett binärt protokoll.

Bara låt det sjunka in en liten stund. Om du är en person som varit involverad
i Internetprotokoll förr så är chansen stor att du helt instinktivt reagerar
mot det valet och plockar fram dina argument om hur protokoll baserade på
text/ascii är överlägsna för att människor kan köra dem manuellt över telnet
och så vidare.

http2 är binärt för att göra inramningen av paket mycket enklare. Att lista ut
början och slutet av paket är en av de riktigt komplicerade sakerna i HTTP 1.1
och faktiskt i text-baserade protokoll i allmänhet. Genom att plocka bort
valbart antal white space och andra sätt att skriva samma sak så blir
implementationer mycket enklare.

Dessutom gör det det mycket lättare att separera själva prokotolldelarna från
inramningen - vilket var förvirrande blandat i HTTP1.

De faktum att protokollet har komprimering och ofta kommer kör över TLS
minskar också värdet av text eftersom man inte skulle se text över kabeln i
alla fall. Vi måste helt enkelt vänja oss vid tanken på att använda en
Wireshark-inspector eller liknande för att lista ut exakt vad som pågår på
protokollnivån i http2.

Debugging av det här protokollet kommer istället antagligen göras med verktyg
som curl eller analyse av nätverksströmmen med Wiresharks http2-stupport och
liknande.

## 6.2. Det binära formatet

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2 skickar binära paket ("frames"). Det finns olika paket-typer som kan
skickas och de har allihop samma upplägg:

Typ, längd, flaggor, ström-identifierare och paket-data ("payload").

Det finns tio olika paket-typer definierade i http2-specen och de två kanske
mest fundamentala som direkt mappar HTTP 1.1-funktioner är DATA och
HEADERS. Jag kommer beskrvia några av paket-typerna i närmare detaljer nedan.

## 6.3. Multiplexade strömmar

Ström-identifieraren som nämndes i stycket ovan om det binära formatet, gör
att varje paket som kommer över http2 är associared med en "ström". En ström
är en logisk förbindelse. En oberoende, bi-direktionell sekevens av paket som
utbyts mellan klienten och servern inom ett http2-koppel.

Ett enda http2-koppel kan överföra många samtidiga strömmar mellan
ändpunkterna genom att skicka paket från olika strömmar över kopplet. Strömmar
kan etableras och användas ensidigt eller delas av både klienten och servern
och de kan stängas av endera sidan. Ordningen som paketen skickas inom
strömmen behålls, så mottagaren tar emot de i samma ordning som de skickades.

Multiplexande strömmar betyder att paket från många strömmar blandas och
skickas över samma koppel. Två (eller fler) individuella tåg med data trycks
ihop över ett enda koppel och delas upp igen på andra sidan. Här är två tåg:

![ett tåg](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![ett till tåg](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

De två tågen multiplexade över samma koppel:

![multiplexat tåg](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Prioriteter och beroendeen

Varje ström har också en prioritet (även känd som "vikt"), vilken används för
att berätta för andra sidan vilka strömmare som skall anses viktigast ifall
resurs-brist tvingar servern att välja vilka strömmar som ska skickas först.

Genom att använda ett PRIORITY-paket kan en klient också berätta för server
vilken annan ström den beror på. Detta möjliggör för en klient att bygga ett
"träd" med prioriteter där flera "barn-strömmar" kan bero att
"föräldra-strömmar" först avslutas.

Prioritetsvikter och beroeenden kan ändras dynamiskt under körning, vilket bör
tillåta webbläsare att se till att när användare scrollar ner en sida full med
bilder så kan den berätta vilka bilder som är viktigast, eller om du byter
tabbar så kan den prioritera en ny uppsättning strömmar som då plötsligt
kommer i fokus.

## 6.5. Header-komprimering

HTTP är ett state-löst protokoll. I korthet betyder det att varje förfrågan
måste ta med sig precis så mycket detaljer som servern behöver för att serva
den frågan utan att servern ska behöva mellanlagra info eller meta-data från
tidigare förfrågningar. Eftersom http2 inte ändra några sådana paradigmer så
måste det också fungera så.

Detta gör HTTP repetitivt. När en klient frågar efter många resurser från
samma server, typ bilder på en webbsida, så kommer det göras en lång serie med
förfråganingar som ser nästan identiska ut. En serie med nästan identiska
någonting ber om komprimering.

Allt medan antalet objekt per webbsida ökat som jag nämnt tidigare, har
användandet av cookies och storleken på förfrågningarna också gjort
det. Cookies behöver också inkluderas i alla förfrågningar, oftast helt
identiska för många förfrågningar.

Storleken på HTTP 1.1-förfrågningar har faktiskt blivit så stora över tid att
de ibland överstiger det intiala TCP-fönstret, vilket gör dem väldigt
långsamma att skicka eftersom de då behöver en hel rund-tur för att få en ACK
tillbaks från servern innan hela förfrågan har skickats. Ytterligare ett
argument för komprimering.

### 6.5.1. Komprimering är ett lurigt ämne

HTTPS- och SPDY-komprimering befanns vara känsliga för
[BREACH](http://en.wikipedia.org/wiki/BREACH_%28security_exploit%29)- och
[CRIME](http://en.wikipedia.org/wiki/CRIME)-attackerna. Genom att infoga känd
text i strömmarna och se hur det förändrade utdatan, kunde en attackerare
lista ut vad som skickades.

Att komprimera dynamiskt innehåll för ett protokoll utan att bli sårbar för
dessa attacker kräver lite omtanke och försiktiga övervägningnar. Det är vad
HTTPbis-teamet försökte sig på.

In kommer då [HPACK](http://www.rfc-editor.org/rfc/rfc7541.txt), Header-
komprimering för http2, vilket – som namnet lämpligt indikerar - är ett
komprimeringsformat speciellt framtaget för http2-headers och det är strikt
talat specifierat i en egen specifikation. Det nya formatet, tillsammans med
andra motåtgärder, som en bit som ber mellanhänder att inte komprimera en viss
header och valbar utfyllnad av paket är tänkt att göra det svårare att
exploatera den här komprimeringen.

Som Roberto Peons sade (en av skaparna av HPACK):

> "HPACK utformades för att göra det svårt för en korrekt implementation att
> läcka information, att koda och dekoda väldigt snabbt/billigt, att erbjuda
> kontroll över storleken på komprimerings-kontexten, att tillåta proxys att
> om-indexera (dvs ett delat tillstån mellan frontend och backend inom en
> proxy), och för snabba jämförelser av huffman-kodade strängar."

## 6.6. Reset - ångra dig

En av nackdelarna med HTTP 1.1 är att när ett HTTP-meddelande har skickats
iväg med en Content-Length av en viss storlek, så kan man inte enkelt bara
sluta skicka det. Visst kan du ofta (men inte alltid) stänga TCP-kopplet men
det kommer till priset av att du måste förhandla upp ett nytt TCP-koppel igen.

En bättre lösning skulle vara att bara stoppa meddelandet och skicka ett
nytt. Det kan göras med http2:s RST_STREAM-paket som därmed hjälper till att
undvika slösa på bandbredd och onödiga nedrivningar av koppel.

## 6.7. Server push

Detta är en funktion också kallad "cache push". Idén här är att ifall en
klient ber om resurs X så kanske servern vet att klienten då troligen också
kommer vilja ha resurs Z och skickar den till klienten utan att den har frågat
efter den. Den hjälper klienten att stoppa in Z i sin cache så att den finns
där direkt när klienten vill ha den.

Server push är något en klient explicit måste tillåta servern att skicka och
även om en klient gör det, så kan den på eget beslut snabbt terminera en
pushad ström med RST_STREAM ifall den inte vill ha den.

## 6.8. Flödeskontroll

Varje individuell ström över http2 har sin eget annonserade flödesfönster som
den andra sidan är tillåten att sända data för. Det är väldigt likt hur SSH
fungerar i stil och koncept, om du råkar veta hur det fungerar.

För varje ström måste båda sidor berätta för den andra hur mycket utrymme det
finns att lagra inkommande data i, och den andra änden har bara tillåtelse att
skicka så mycket data tills dess att fönstret utökas. Bara DATA-paket är
flödeskontrollerade.
