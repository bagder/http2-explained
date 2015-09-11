# 11. http2 et curl

Le [projet curl](http://curl.haxx.se/) a fourni un support http2 expérimental depuis septembre 2013.

Dans l'esprit curl, nous voulons supporter toutes les fonctionnalités http2 possibles. curl est souvent utilisé comme outil de test et nous voulons que ce soit le cas pour http2 également.

curl utilise une librairie distincte [nghttp2](https://nghttp2.org/) pour la couche http2.

## 11.1. Ressemblance HTTP 1.x

En interne, curl convertit les en-têtes entrant http2 en en-têtes du style HTTP 1.x, pour qu'ils apparaissent similaires à l'utilisateur. Cela permet une transition plus simple pour toute personne utilisant curl en HTTP aujourd'hui. Idem pour les en-têtes sortants. Donnez à curl des en-têtes au format HTTP 1.x et il les convertira en http2 à la volée vers les serveurs http2. Cela permet aux utilisateurs de ne pas forcément se soucier de la version de HTTP utilisée.

## 11.2. En clair

curl supporte http2 sur TCP via l'en-tête Upgrade:. Si vous initiez une requête HTTP en demandant HTTP 2, curl demandera au serveur de mettre à niveau (Upgrader) sa connexion en http2.

## 11.3. Quelles librairies TLS ?

curl supporte différentes librairies TLS et c'est toujours valide pour http2. La difficulté avec TLS et http2 est le support de ALPN et potentiellement NPN.

Compilez curl avec des versions récentes d'OpenSSL ou NSS pour avoir ALPN et NPN. Avec GnuTLS et PolarSSL vous n'aurez que ALPN et pas NPN.

## 11.4. Ligne de commande à utiliser

Pour indiquer à curl d'utiliser http2, en clair ou TLS, utilisez l'option `--http2` ("tiret tiret http2"). curl utilise HTTP/1.1 par défaut, d'où cette option nécessaire pour http2.

## 11.5. Options libcurl

Votre application utilisera http:// ou https:// comme d'habitude, mais vous pouvez régler la variable curl_easy_setopt de `CURLOPT_HTTP_VERSION` vers `CURL_HTTP_VERSION_2` pour que libcurl tente d'utiliser http2. Il le fera en best effort sinon utilisera HTTP 1.1.