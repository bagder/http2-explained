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

## 1.1 L'autore

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

## 1.2 Aiuto!

Se dovessi trovare errori, omissioni, sbagli o falsità in questo documento, ti pregherei di mandarmi una versione corretta del paragrafo in questione e mi occuperò di redigere la versione corretto. Darò tutto il credito a chiunque aiuti concretamente! Spero di rendere questo documento sempre più completo col passare del tempo.

This document is available at [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 Licenza

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Questo documento è rilasciato sotto licenza Creative Commons Attribution 4.0: https://creativecommons.org/licenses/by/4.0/

## 1.4 Storia del documento

La prima versione del presente documento fu pubblicata il 25 Aprile 2014. Di seguito i maggiori cambiamenti attraverso le più recenti versioni.

### Versione 1.13

- Convertita la sezione principale del documento a sintassi Markdown 
- 13: Citare più risorse, link e descrizioni aggiornate 
- 12: Aggiornata la descrizione di QUIC con riferimento alla draft 
- 8.5: Aggiornata alla numerazione attuale 
- 3.4: La media è adesso 40 connessioni TCP 
- 6.4: Aggiornato per aderire a quanto affermato nella specifica 

### Versione 1.12

- 1.1: HTTP/2 è oramai una RFC ufficiale 
- 6.5.1: Collegamento alla RFC su HPACK 
- 9.1: Menzionare come Firefox 36+ configuri http2 "on by default" 
- 12.1: Aggiunta sezione a proposito di QUIC 

### Versione 1.11

- Molti miglioramenti dal punto di vista del linguaggio, grazie a molti contributi amichevoli 
- 8.3.1: Citare le attività specifiche di nginx e Apache httpd 

### Versione 1.10

- 1: Il protocollo ha ricevuto l'OK 
- 4.1: Accordare i tempi passati rispetto al fatto che 2014 è l'anno scorso 
- Front: Aggiunta immagine con nome “http2 explained”, link corretto 
- 1.4: Aggiunta sezione "Storia del documento" 
- Molti errori grammaticali e di spelling corretti 
- 14: Aggiunto rinraziamento ai bug reporters 
- 2.4: Migliori etichette per il grafico sulla evoluzione di HTTP 
- 6.3: Corretto l'ordine dei vagoni nel treno multiplexato 
- 6.5.1: draft-12 HPACK 

### Version 1.9

- Aggiornato alla draft-17 HTTP/2 e draft-11 HPACK  
- Aggiunta sezione "10. http2 in Chromium" (== più lungo di una pagina)  
- Un sacco di fix spelling  
- A quota 30 implementazioni ad oggi
- 8.5: Aggiunti alcune cifre relative all'utilizzo
- 8.3: Citare anche internet explorer  
- 8.3.1 Aggiunto "implementazioni mancanti"  
- 8.4.3: Menzionare come TLS incrementi anche il tasso di successo  
