# 9. http2 et Firefox

Firefox a suivi les drafts de près et fourni des protocoles de test de http2 depuis des mois. Pendant le développement du protocole http2, les clients et serveurs devaient s'accorder sur le draft du protocole à utiliser, ce qui compliquait les tests. Soyez juste conscient de vous mettre d'accord sur le draft à utiliser entre client et serveur.

## 9.1. Assurez-vous de l'activer

Toute version de Firefox 35 et ultérieure, depuis le 13 janvier 2015, a le support http2 activé par défaut.

Allez dans "about:config" depuis la barre d'URL et cherchez l'option appelée "network.http.spdy.enabled.http2draft". Assurez-vous qu'elle est à "true". Firefox 36 ajouta une option activée par défaut "network.http.spdy.enabled.http2". Cette dernière contrôle la version finale de http2 tandis que la première active ou non la négociation des versions drafts de http2. Les deux sont activées depuis Firefox 36.

## 9.2. TLS uniquement

Souvenez-vous que Firefox n'implémente http2 que sur TLS. Vous ne verrez du http2 que si vous allez sur les sites https:// qui offrent http2.

## 9.3. Transparent!

![ Copie d'écran montrant que Firefox utilise http2 draft-12](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Il n'y a pas d'indication graphique que vous utilisez http2. Vous ne pouvez pas le voir facilement. Une manière de le trouver est d'activer "Web developer->Network" et de vérifier l'en-tête de réponse. Elle doit comporter "HTTP/2.0", Firefox insère son propre entête "X-Firefox-Spdy:" comme montré sur la copie d'écran précédente.

Les en-têtes que vous voyez dans l'outil ont été convertis du format binaire http2 vers un format qui ressemble aux en-têtes HTTP 1.x.

## 9.4. Voir l'utilisation http2

Il existe des plug-ins Firefox permettant de visualiser si un site utilise http2. En voici un [“SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/).
