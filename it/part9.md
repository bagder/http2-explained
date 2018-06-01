# 9. http2 in Firefox

Firefox ha seguito l'evolversi delle draft da vicino, ha reso disponibili test sulle implementazioni http2 per lunghi mesi. Durante lo sviluppo del protocollo http2, client e server hanno dovuto mettersi d'accordo su quale versione della draft usare, cosa perarltro abbastanza fastidiosa in fase di test. Giusto per essere sicuri che client e server siano allineati su quale versione della draft stiano implementando.

## 9.1. Primo passo, essere sicuro di averlo abilitato

In tutte le versioni di Firefox a partire dalla 35, rilasciata il 13 Gennaio 2015, il supporto nativo http2 è abilitato.

Digita 'about:config' nella barra dell'indirizzo e cerca l'opzione “network.http.spdy.enabled.http2draft”. Assicurati che sia impostata a *true*. In Firefox 36 esiste un'altra voce di configurazione chiamata “network.http.spdy.enabled.http2” che è impostata su *true* per default. Quest'ultima controlla la versione “plain” di http2 mentre la prima abilita o disabilita la negoziazione della versione http2-draft. Entrambe sono abilitate a partire da Fierfox 36.

## 9.2. Solo TLS

Ricordatevi che Firefox implementa https solo su TLS. Vedrete http2 in azione se e solo se starete utilizzando un sito https:// che offre supporto per http2.

## 9.3. Trasparente!

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Non esiste alcun elemento della UI (User Interface) che indichi quando si stia parlando http2. Non è facile poterlo affermare. Un modo relativamente facile di verificarlo è abilitare la parte “Web developer->Network” e controllare negli header di risposta quanto ottenuto dal server. In caso risposta sia dunque “HTTP/2.0” qualcosa, Firefox inserirà il proprio header “X-Firefox-Spdy:” come possbile osservare nello screenshotqui sopra.

Gli headers che vedete nel tool Network durante un dialogo http2 sono stati convertiti a partire dal formato http2 binario verso il vecchio stile di headers HTTP 1.x

## 9.4. Visualizzare l'utilizzo di http2

Esistono plugins Firefox che aiutano a capire se un sito utilizzi http2. Uno di questi è [“HTTP/2 and SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/).
