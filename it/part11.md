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

### 11.5.1 Enable HTTP/2

Your application would use https:// or http:// URLs like normal, but you set
curl_easy_setopt's `CURLOPT_HTTP_VERSION` option to `CURL_HTTP_VERSION_2` to
make libcurl attempt to use http2. It will then do a best effort and do http2
if it can, but otherwise continue to operate with HTTP 1.1.

### 11.5.2 Multiplexing

As libcurl tries to maintain existing behaviors to a far extent, you need to
enable HTTP/2 multiplexing for your application with the
[CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html)
option. Otherwise it will continue using one request at a time per connection.

Another little detail to keep in mind is that if you ask for several transfers
at once with libcurl, using its multi interface, an applicaton can very well
start any number of transfers at once and if you then rather have libcurl wait
a little to add them all over the same connection rather than opening new
connections for all of them at once, you use the
[CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) option
for each individual transfer you rather wait.

### 11.5.3 Server push

libcurl 7.44.0 and later supports HTTP/2 server push. You can take advantage
of this feature by setting up a push callback with the
[CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html)
option. If the push is accepted by the application, it'll create a new
transfer as an CURL easy handle and deliver content on it, just like any other
transfer.
