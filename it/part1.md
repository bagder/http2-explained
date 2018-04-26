# 1. Background

Questo documento descrive http2 su un livello tecnico e di protocollo. Tale documenta ha
preso vita da una presentazione di Daniel a Stoccolma in Aprile 2014, presentazione che
fu successivamente convertita ed estesa in un documento vero e proprio, maggiormente
dettagliato e contentente spiegazioni piu proprie.

RFC 7540 è il nome ufficiale della specifica finale di http2, pubblicata il 15 Maggio 2015: https://www.rfc-editor.org/rfc/rfc7540.txt

Ogni eventuale errore in questo documento è il risutato della mia pigrizia
e stoltezza, pregasi segnalarne eventuale presenza; saranno corretti in versioni
successive.

In questo documento ho provato ad usare in maniera consistente il termine "http2"
per desrcivere il nuovo protocollo, quando invece in pura termininologia tecnica
il vero nome è HTTP/2. Ho fatto questa scelta per migliorare la leggibilità e 
per favorive una migliore scorrevolezza del linguaggio.

## 1.1 Author

Il mio nome è Daniel Stenberg e lavoro per Mozilla. Ho lavorato in ambito open
source e networking per più di vent'anni in svariati progetti. Molto probabilmente
sono meglio conosciuto per essere il principale sviluppatore di curl and libcurl.
Sono stato coinvolto nel gruppo di lavoro IETF HTTPbis per svariati anni e là ho avuto
la possibilità di tenermi aggiornato sul progetto HTTP 1.1 ed in seguito partecipare
alla standardizzazione di http2.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Help!

Se trovassi errori, omissioni, sbagli o falsità in questo documento, ti pregherei di mandarmi una versione corretta del paragrafo in questione e mi occuperò di redigere la versione corretto. Darò tutto il credito a chiunque aiuti concretamente! Spero di rendere questo documento sempre meglio col passare del tempo.

This document is available at [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 License

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Questo documento è rilasciato sotto licenza Creative Commons Attribution 4.0: https://creativecommons.org/licenses/by/4.0/

## 1.4 Document history

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
