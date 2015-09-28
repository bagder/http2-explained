# 1. Bakgrund

Det här dokumentet beskriver http2 från en teknisk och protokollnivå. Det
började som en presentation Daniel gjorde April 2014 i Stockholm som
sedemera gjordes om och breddades till ett fullt utvecklat dokument med alla
detaljer och riktiga förklaringar.

RFC 7540 är det officiella namnet på den slutliga http2-specifikationen och den
blev publicerad den 15:e maj 2015: http://www.rfc-editor.org/rfc/rfc7540.txt

Alla misstag i det är dokumentet är mina egna och resultatet av mina fel. Peka
gärna ut dem så åtgärdar vi dem till en kommande uppdatering.

I det här dokumentet har jag försökt att konsekvent använda ordet "http2" för
att bekskriva det nya prokollet, medan det ju rent tekniskt och korrekt
faktiskt heter HTTP/2. Jag har gjort det valet för läslighetens skull och för att få
ett bättre flöde i texten.

## 1.1 Författaren

Mitt namn är Daniel Stenberg och jag jobbar för Mozilla. Jag har arbetat med
open source och nätverk i över tjugo år i otaliga projekt. Möjligvis är jag
mest känd för att jag är huvudutvecklaren av curl och libcurl. Jag har varit
involverad i IETF och dess HTTPbis arbetsgrupp under flera år och där har jag
hållit mig uppdaterad med uppdateringen av HTTP 1.1 samt varit inblandad i
arbetet med standardisering av http2.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](http://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](http://daniel.haxx.se/blog/)

## 1.2 Hjälp!

Om du hittar misstag, utelämnade detaljer, fel eller rent av lögner i det här
dokumentet, skicka mig gärna en rättad version av det aktuella avsnittet så
kommer jag snart publicera en uppdated version. Jag ger ordentliga omnämnanden
och tack till alla som hjälper till! Jag ämnar göra dokumentet bättre över
tid.

Det här dokumentet finns tillgängligt på [http://daniel.haxx.se/http2](http://daniel.haxx.se/http2)

## 1.3 Licens

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Det här dokumentet är licenserat under Creative Commons Attribution 4.0 license: http://creativecommons.org/licenses/by/4.0/

## 1.4 Dokumenthistoria

Den första versionen av det här dokumentet publicerades 25:e April 2014. Här
följer de större förändringarna i de senaste versionerna.

### Version 1.13

- Converted the master version of this document to Markdown syntax
- 13: Mention more resources, updated links and descriptions 
- 12: Updated the QUIC description with reference to draft 
- 8.5: Refreshed with current numbers 
- 3.4: The average is now 40 TCP connections 
- 6.4: Updated to reflect what the spec says 

### Version 1.12

- 1.1: HTTP/2 is now in an official RFC 
- 6.5.1: Link to the HPACK RFC 
- 9.1: Mention the Firefox 36+ config switch for http2 
- 12.1: Added section about QUIC 

### Version 1.11

- Lots of language improvements mostly pointed out by friendly contributors 
- 8.3.1: Mention nginx and Apache httpd specific acitivities 

### Version 1.10

- 1: The protocol has been “okayed” 
- 4.1: Refreshed the wording since 2014 is last year 
- Front: Added image and call it “http2 explained” there, fixed link 
- 1.4: Added document history section 
- Many spelling and grammar mistakes corrected 
- 14: Added thanks to bug reporters 
- 2.4: Better labels for the HTTP growth graph 
- 6.3: Corrected the wagon order in the multiplexed train 
- 6.5.1: HPACK draft-12 

### Version 1.9

- Updated to HTTP/2 draft-17 and HPACK draft-11  
- Added section "10. http2 in Chromium" (== one page longer now)  
- Lots of spell fixes  
- At 30 implementations now  
- 8.5: Added some current usage numbers  
- 8.3: Mention internet explorer too  
- 8.3.1 Added "missing implementations"  
- 8.4.3: Mention that TLS also increases success rate
