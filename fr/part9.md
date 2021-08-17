# 9. http2 et Firefox

Firefox a suivi les drafts de près et fourni des protocoles de test de http2 depuis des mois. Pendant le développement du protocole http2, les clients et serveurs devaient s'accorder sur le draft du protocole à utiliser, ce qui compliquait les tests. Soyez juste conscient de vous mettre d'accord sur le draft à utiliser entre client et serveur.

## 9.1. Assurez-vous de l'activer

Toutes les versions de Firefox à partir de la 36, du 24 février 2015, ont le support http2 activé par défaut.

## 9.2. TLS uniquement

Souvenez-vous que Firefox n'implémente http2 que sur TLS. Vous ne verrez du http2 que si vous allez sur les sites https:// qui offrent http2.

## 9.3. Transparent !

![ Copie d'écran montrant que Firefox utilise http2 draft-12](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Il n'y a pas d'indication graphique que vous utilisez http2. Vous ne pouvez pas le voir facilement. Une manière de le trouver est d'activer "Web developer->Network" et de vérifier l'en-tête de réponse. Elle doit comporter "HTTP/2.0", Firefox insère son propre en-tête "X-Firefox-Spdy:" comme montré sur la copie d'écran précédente.

Les en-têtes que vous voyez dans l'outil ont été convertis du format binaire http2 vers un format qui ressemble aux en-têtes HTTP 1.x.

## 9.4. Voir l'utilisation http2

Il existe des plug-ins Firefox permettant de visualiser si un site utilise http2. En voici un [“SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/http2-indicator/).
