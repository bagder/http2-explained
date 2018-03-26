# 1. Hintergrund

Dieses Dokument beschreibt den technischen Aufbau und das Protokoll http2.

Was als Präsentation im April 2014 in Stockholm anfing wurde daraufhin zu einer detaillierten Dokumentation und fügte einige sachgerechte Erklärungen hinzu

RFC 7540 ist der offizielle Standard der letzten Spezifikation von http2. Am 15. Mai 2015 wurde diese veröffentlicht.
URL: https://www.rfc-editor.org/rfc/rfc7540.txt

Jegliche Fehler in diesem Dokument basieren auf meinem Verständnis der Materie. Ich bin offen für jede Korrektur welche beim update auf eine neue Version behoben wird.

Ich nutze in diesem Dokument konsequent "http2" um das neue Protokoll zu beschreiben. Der technisch korrekte Name ist HTTP/2. Dies hab ich gemacht um die Lesbarkeit des Dokumentes zu gewährleisten.


## 1.1 Autor

Der Name des original Autors ist Daniel Stenberg welcher für Mozilla arbeitet. Er arbeitet seit über 20 Jahren in verschiedenen Opern-Source Projekten mit. Das wohl bekannteste ist curl und libcurl wo er der leitende Entwickler ist. Daniel arbeitet auch seit Jahren in der Arbeitsgruppe der IETF HTTPbis welche HTTP 1.1 aktualisiert hat und war auch in der Entwicklung des http2 Standards involviert.

 Email: daniel@haxx.se

 Twitter: [@bagder](https://twitter.com/bagder)

 Web: [daniel.haxx.se](https://daniel.haxx.se/)

 Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Hilfe!

Falls du Fehler, Missverständnisse oder offensichtliche Lügen in diesem Dokument findest sende mir bitte eine verbesserte Version des betroffene Paragrafen und Ich erstelle eine novelliert Version des Dokumentes. Du wirst natürlich als Autor ordnungsgemäß als Mitautor aufgelistet. Ich hoffe damit dieses Dokument mit der Zeit immer wieder zu verbessern.

Dieses Dokument ist unter der folgenden URL verfügbar. [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 Lizenz

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Dieses Dokument ist unter der „Creative Commons Attribution 4.0“ Lizenz veröffentlicht: https://creativecommons.org/licenses/by/4.0/

## 1.4 Dokument Historie 

Die Historie lasse ich in Englisch um die Wartung dieser einfach zu ermöglichen.

The first version of this document was published on April 25th 2014. Here follows the largest changes in the most recent document versions.

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
