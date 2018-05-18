# 11. http2 in curl

Il [curl project](https://curl.haxx.se/) ha fornito supporto sperimentale ad 
http2 a partire da Settembre 2013.

Nello spirito di curl, abbiamo intenzione di supportare praticamente ogni singolo aspetto di http2. curl è spesso usato come tool di test per interagire in maniera flessibile con i siti web, dunque vogliamo mantenere lo stesso per http2.

curl utilizza la libreria separata [nghttp2](https://nghttp2.org/) per poter 
offrire funzionalità a livello di frame. curl necessita di nghttp2 1.0 o superiore.

Notate che al momento su linux, curl e libcurl non sono sempre distribuiti con il
supporto HTTP/2 abilitato.

## 11.1. Sembra HTTP 1.x

Al suo interno, curl converte gli headers entranti http2 in headers stile HTTP 1.x presentandoli all'utente e facendoli apparire molto simili ai pre-esistenti HTTP. Ciò permette una transizione facilitata per tutti gli strumenti che usano curl e HTTP oggi. In modo simile curl convertirà gli header uscenti con lo stesso stile. Passateli a curl in stile HTTP 1.x e lui si occuperà di convertirli al volo durante il dialogo con server http2. Questo permette agli utenti di non doversi occupare troppo di quale particolare versione di headers HTTP si stia usando.

## 11.2. Plain text, non sicuro

curl supporta http2 su TCP standard attraverso l'header "Upgrade:". Se si 
esegue una richesta HTTP e si richiede HTTP 2, curl chiederà al server di 
aggiornare la connessione a http2 ove possibile.

## 11.3. TLS, quali librerie

curl supporta un vasto numero di librerie TLS per il proprio back-end TLS, ed è ancora il caso con http2. La difficoltà per http2 utilizzando TLS è offirre buon supporto ALPN e talvolta NPN.

Lancia una build di curl con moderne version di OpenSSL o NSS per assicurare il support di ALPN e NPN. Se utilizzi GnuTLS o PolarSSL, avrai ALPN ma non NPN.

## 11.4. Utilizzo in linea di comando

Per istruire curl ad utilizzare http2 -via plain-text o su TLS- utilizzare la 
opzione `--http2` (meno meno http2). curl è ancora impostato per utilizzare 
HTTP/1.1 per default, quindi l'opzione è necessaria se desideriamo http2.

## 11.5. Opzioni di libcurl

### 11.5.1 Abilitare HTTP/2

La tua applicazione continuerà ad utilizzare URL di tipo https:// o http:// 
ma dovrai anche settare la voce curl_easy_setopt `CURLOPT_HTTP_VERSION` a
`CURL_HTTP_VERSION_2` per far sì che libcurl provi ad utilizzare http2. Su 
base best-effort proverà ad utilizzare http2 altrimenti continuerà su HTTP 1.1.

### 11.5.2 Multiplexing

Dato che libcurl prova a mantenere gli stessi comportamenti di sempre, dovrai
abilitare il multiplexing HTTP/2 nella tua applicazione tramite l'opzione 
[CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html).
In caso contrario, continuerai ad utilizzare una richiesta per volta per ogni 
connessione disponibile.

Altro piccolo dettaglio da tenere a mente quando si richiedono trasferimenti
multipli via libcurl tramite la sua interfacia "multi", una applicazione 
potrebbe decidere di iniziare un numero infinito di trasferimenti simultanei;
se desideriamo veicolarli tutti tramite la stessa connessione piuttosto che
utilizzarne una moltitudine, possiamo istruire libcurl affinchè aspetti un 
determinato lasso di tempo utilizzando l'opzione
[CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html).

### 11.5.3 Server push

libcurl 7.44.0 e successivi supportano la server push HTTP/2. Potrete trarne 
vantaggio impostando un callback tramite l'opzione
[CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html)
Se l'applicazione accettase il push, utilizzerebbe un handler di tipo CURL easy
per trasmettere il contenuto del trasferimento, così come avverrebbe in ogni
altro caso.
