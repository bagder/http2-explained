# 1. Hintergrund

Diese Dokumentation beschreibt http2 auf technischer und Protokoll-Ebene. Was als Präsentation im April 2014 in Stockholm anfing, wurde in Folge zu einer umfassenden und detaillierten Dokumentation ausgebaut. Des Weiteren wurden einige sachgerechte Erklärungen hinzugefügt.

RFC 7540 ist der offizielle Name der finalen Spezifikation von http2, welche am 15. Mai 2015 veröffentlicht wurde: https://www.rfc-editor.org/rfc/rfc7540.txt

Jegliche Fehler in dieser Dokumentation sind meine eigenen und Resultat meines Verständnisses der Materie. Ich bin offen für jede Korrektur, welche in überarbeiteten Versionen übernommen werden.

Ich nutze in diesem Dokument konsequent "http2" um das neue Protokoll zu beschreiben. Der technisch korrekte Name ist HTTP/2. Dies hab ich gemacht um die Lesbarkeit des Dokumentes zu gewährleisten.


## 1.1 Autor

Mein Name ist Daniel Stenberg und ich arbeite bei Mozilla. Schon seit über 20 Jahren arbeite ich bei verschiedenen Opern-Source Projekten mit. Die wohl bekanntesten Projekte sind curl und libcurl, bei denen ich leitender Entwickler bin. Ich arbeite auch seit Jahren in der Arbeitsgruppe der IETF HTTPbis mit, in welcher ich bei der Aktualisierung von HTTP 1.1 sowie Entwicklung von http2 involviert war.

 E-Mail: daniel@haxx.se

 Twitter: [@bagder](https://twitter.com/bagder)

 Web: [daniel.haxx.se](https://daniel.haxx.se/)

 Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Hilfe!

Falls du Fehler, Missverständnisse oder offensichtliche Lügen in diesem Dokument findest, sende mir bitte eine verbesserte Version des betroffene Paragrafen und Ich erstelle eine überarbeitete Version des Dokumentes. Du wirst natürlich ordnungsgemäß als Mitautor aufgelistet. Ich hoffe diese Dokumentation laufend verbessern zu können.

Dieses Dokument ist unter der folgenden URL verfügbar: [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 Lizenz

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Dieses Dokument ist unter der „Creative Commons Attribution 4.0“ Lizenz veröffentlicht: https://creativecommons.org/licenses/by/4.0/

## 1.4 Historische Entwicklung der Dokumentation

Die erste Version dieses Dokumentes wurde am 25. April 2014 veröffentlicht. Die folgende Auflistung beschreibt die historische Entwicklung der Dokumentation. Um die Wartung dieser zu erleichtern, wird sie direkt aus dem Englischen übernommen.

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

- 1: The protocol has been 
- 4.1: Refreshed the wording since 2014 is last year 
- Front: Added image and call it there, fixed link 
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
