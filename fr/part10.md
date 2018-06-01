# 10. http2 et Chromium

L'équipe Chromium a implémenté http2 depuis un certain temps via les canaux beta et dev. Depuis Chrome 40, sorti le 27 janvier 2015, http2 est activé par défaut pour un certain nombre d'utilisateurs. Ce nombre a commencé petit pour devenir important au fil du temps.

Le support SPDY sera retiré. Dans un blog, il est annoncé en [février 2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> “Chrome supporte SPDY depuis Chrome 6, mais comme les bénéfices sont présents
dans HTTP/2, il est temps de lui dire au revoir. Le support SPDY sera retiré début 2016.”

## 10.1. Assurez-vous de l'activer

Allez sur "chrome://flags/#enable-spdy4" dans la barre d'URL et cliquez sur "enable" si ce n'est déjà fait.

## 10.2. TLS uniquement

Souvenez-vous que Chrome n'implémente http2 que sur TLS. Vous ne verrez du http2 que si vous allez sur les sites https:// qui offrent http2.

## 10.3. Voir l'utilisation http2

Il existe des plug-ins Chrome permettant de visualiser si un site utilise http2. En voici un: [“SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Les tests QUIC en cours masquent un peu les résultats HTTP/2. (voir chapitre 12.1)