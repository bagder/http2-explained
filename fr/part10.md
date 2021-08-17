# 10. http2 et Chromium

L'équipe Chromium a implémenté http2 depuis un certain temps via les canaux beta et dev. Depuis Chrome 40, sorti le 27 janvier 2015, http2 est activé par défaut pour un certain nombre d'utilisateurs. Ce nombre a commencé petit pour devenir important au fil du temps.

Le support de SPDY a été retiré dans Chrome 51, au profit de http2. Dans un blog, il est annoncé en [février 2016](https://blog.chromium.org/2016/02/transitioning-from-spdy-to-http2.html):

> “Plus de 25% des ressources dans Chrome sont actuellement délivrées en HTTP/2, contre moins de 5% pour SPDY. Vu ces chiffres, Chrome ne supportera plus SPDY à partir du 15 mai, date anniversaire du RFC HTTP/2.”

## 10.1. Assurez-vous de l'activer

Si vous utilisez une version de Chrome très ancienne, vous pouvez vérifier si http2 est géré.

Allez sur "chrome://flags/#enable-spdy4" dans la barre d'URL et cliquez sur "enable" si ce n'est déjà fait.  Dans les versions récentes http2 est activé d'office et ce réglage a été retiré.

## 10.2. TLS uniquement

Souvenez-vous que Chrome n'implémente http2 que sur TLS. Vous ne verrez du http2 que si vous allez sur les sites https:// qui offrent http2.

## 10.3. Voir l'utilisation http2

Il existe des plug-ins Chrome permettant de visualiser si un site utilise http2. En voici un: [“SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Les tests QUIC en cours masquent un peu les résultats HTTP/2. (voir chapitre 12.1)