# 11. http2 et curl

Le [projet curl](https://curl.haxx.se/) a fourni un support http2 expérimental depuis septembre 2013.

Dans l'esprit curl, nous voulons supporter toutes les fonctionnalités http2 possibles. curl est souvent utilisé comme outil de test et nous voulons que ce soit le cas pour http2 également.

curl utilise une bibliothèque distincte [nghttp2](https://nghttp2.org/) pour la couche http2. curl requiert nghttp2 1.0 ou plus.

Notez qu'actuellement, sous linux, curl et libcurl ne sont pas toujours délivrés avec le support du protocole HTTP/2 activé.

## 11.1. Ressemblance HTTP 1.x

En interne, curl convertit les en-têtes entrant http2 en en-têtes du style HTTP 1.x, pour qu'ils apparaissent similaires à l'utilisateur. Cela permet une transition plus simple pour toute personne utilisant curl en HTTP aujourd'hui. Idem pour les en-têtes sortants. Donnez à curl des en-têtes au format HTTP 1.x et il les convertira en http2 à la volée vers les serveurs http2. Cela permet aux utilisateurs de ne pas forcément se soucier de la version de HTTP utilisée.

## 11.2. En clair

curl supporte http2 sur TCP via l'en-tête Upgrade:. Si vous initiez une requête HTTP en demandant HTTP 2, curl demandera au serveur de mettre à niveau (Upgrader) sa connexion en http2.

## 11.3. Quelles bibliothèques TLS ?

curl supporte différentes bibliothèques TLS et c'est toujours valide pour http2. La difficulté avec TLS et http2 est le support de ALPN et potentiellement NPN.

Compilez curl avec des versions récentes d'OpenSSL ou NSS pour avoir ALPN et NPN. Avec GnuTLS et PolarSSL vous n'aurez que ALPN et pas NPN.

## 11.4. Ligne de commande à utiliser

Pour indiquer à curl d'utiliser http2, en clair ou avec TLS, utilisez l'option `--http2` ("tiret tiret http2"). curl utilise HTTP/1.1 par défaut pour les URL en HTTP: donc cette option est nécessaire si vous voulez utiliser http2 en clair.  Pour les URL en HTTPS:, curl essaiera http2.

## 11.5. Options libcurl

### 11.5.1 Activez HTTP/2

Votre application utilisera http:// ou https:// comme d'habitude, mais vous pouvez régler la variable curl_easy_setopt de `CURLOPT_HTTP_VERSION` vers `CURL_HTTP_VERSION_2` pour que libcurl tente d'utiliser http2. Il le fera si possible sinon utilisera HTTP 1.1.

### 11.5.2 Multiplexage

Etant donné que libcurl essaye de maintenir ses anciens comportements dans la mesure du possible, vous devez activer le multiplexage HTTP/2 pour votre application via l'option [CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html). Sinon elle continuera à utiliser une requête à la fois par connexion.

Un autre détail à garder à l'esprit est que si vous demandez plusieurs transferts simultanés à libcurl, en utilisant son interface multi, une application peut très bien commencer autant de transfert que voulu, et que si vous préférez que libcurl attende un peu pour les placer tous sur la même connexion, plutôt que d'ouvrir une connexion pour chacun, utilisez l'option [CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) pour chaque transfert que vous préférez attendre.

### 11.5.3 Server push

libcurl 7.44.0 et plus supporte le server push de HTTP/2. Vous pouvez tirez avantage de cette fonctionnalité en configurant un callback de push avec l'option [CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html). Si le push est accepté par l'application, il créera un nouveau transfert en tant que CURL easy handle et l'utilisera pour délivrer le contenu, comme tout autre transfert.
