# 3. Tricks för att komma över fördröjningssmärtor

Som alltid när problem dyker upp så har folk samlats och uppfunnit tricks för att komma runt dem.
Några tricks är smarta och användbara, några av dem bara hemska fulhack.

## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Spriting är termen för att beskriva vad man gör när man stoppar massor av små
bilder ihop till en enda stor bild. Sen använder man javascript och CSS för
att "skära ut" bitar ur den stora bilden för att visa de små individuella
bilderna.

En sajt använder det här tricker för hastighet. Att hämta en enda stor bild är
mycket snabbare i HTTP 1.1 än att hämta 100 små bilder.

Självklart har det sina nackdelar för de sidor sidor på sajten som bara vill
visa en eller två små bilder och liknande. Det gör också att alla bilder
rensas från cachen på samma gång istället för låta de mest använda ligga kvar
där.

## 3.2 Inlining

Inlining är ett annat trick där man underviker skicka enskilda bilder, och det
gör man genom att använda data: URLer inbäddade i CSS-filen. Det har liknande
nackdelar som i spriting-fallet ovan.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

## 3.3 Concatenation

En stor sajt kan lätt hamna i med väldigt många olika javascriptfiler.
Frontendverktyg kan hjälpa utvecklar att slå ihop varenda en av dem till en
enda stor klump så att webbläsaren hämtar en enda stor istället för dussintals
mindre filer. För mycket data är skickat när enbart lite behvös. För mycket
data laddas om när en ändring behövs.

Den här utövningen är förstås mest ett besvär för de involverade utvecklarna.

## 3.4 Sharding

Det sista prestandatricket jag ska nämna kallas ofta "sharding". Det är i
princip att hosta olika delar av din sajt från så många olika hostar som
möjligt. Det kan förfalla konstigt vid en första anblick med det finns ett
logiskt resonemang bakom.

Från börjar sade HTTP 1.1-specifikationen att en klient bara var tillåten att
använda maximalt två TCP-koppel till varje host. Så, för att inte bryta mot
specen så uppfann smarta sajter nya host namn och - voilá - så kunde du få
många fler koppel till din sajt och minska sidladdningstider.

Över tid har den gränsen tagits bort och dagens klienter använder lätt 6-8
koppler per hostnamn, men de behöver fortfarande ha en gräns så sajter
fortsätter att använda den här tekniken för att öka antalet koppel. Med ett
ständigt ökande antal object (som jag visade tidigare) så måste det större
antalet koppel användas för att få HTTP att prestera bra och göra din sajt
snabb. Det är inte ovanligt att enskilda sajter använder långt över 50 eller
upp och förbi 100 koppel tack vare den här tekniken. Färsk statistik från
httparchive.org visar att av de top-300K URLerna i världen så behövs i
genomsnitt 40(!) TCP-koppel för att visa sajten, och trendkurvan säger att det
fortsätter öka.

En annan anledning att också lägga bilder och liknande resurser på ett separat
hostnamn som inte använder cookies, är att storleken på cookies idag kan bli
betydande. Genom att använda cookie-lösa bild-värdar så kan du ibland öka
prestandan enbart genom att HTTP-förfrågningarna blir så mycket mindre!

Bilden nedan visar paket-spårning och hur det ser ut när en webbläsaren
besöker en av Sveriges toppsajter, och hur förfrågningar är distribuerade över
flera olika hostnamn.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
