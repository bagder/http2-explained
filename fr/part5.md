# http2 concepts

Que réalise http2 ? Quelles sont les limites imposées par le groupe HTTPbis ?

Elles sont plutôt strictes avec des contraintes.

- il faut maintenir le principe de HTTP. C'est toujours un protocole où le client envoie une requête vers un serveur en TCP.

- les URLs http:// et https:// ne doivent pas changer. Il ne faut pas de nouvelle méthode. Le contenu utilisant ces URLs est trop important pour s'attendre à ce que tout le contenu change de méthode.

- clients et serveurs HTTP1 seront encore là pour des décennies, on doit pouvoir les faire basculer (“proxifier”) vers des serveurs http2.

- en conséquence, les proxys doivent mapper les fonctionnalités http2 à 1:1 vis-à-vis des clients HTTP 1.1.

- retirer ou réduire les composants optionnels du protocole. Ce n'est pas vraiment un pré-requis mais un principe venant de SPDY et des équipes Google. En s'assurant que tout est obligatoire, il n'y a pas de risques d'omettre d'implémenter quelque chose, puis de se retrouver piégé plus tard.

- pas de versions mineures. Il est décidé que clients et serveurs sont compatibles http2 ou pas du tout. S'il faut modifier ou étendre le protocole, alors http3 sera défini. Il n'y aura pas de versions mineures en http2.

## 5.1. http2 avec les méthodes URI existantes

Comme mentionné, les formats d'URL existants ne peuvent pas être modifiées, http2 doit donc gérer les URI actuelles. Comme elles sont déjà utilisées en HTTP 1.x, on doit pouvoir mettre à niveau (“upgrader”) le protocole vers http2 ou demander au serveur d'utiliser http2 plutôt que les anciens protocoles.

HTTP 1.1 a défini un moyen de le faire, l'en-tête Upgrade:, qui permet au serveur de renvoyer une réponse avec le nouveau protocole quand la requête était envoyée avec un protocole plus ancien. Au prix d'un aller-retour (round-trip).

Le coût de cet aller-retour n'est pas quelque chose que l'équipe SPDY allait tolérer. Comme ils n'implémentaient SPDY qu'au dessus de TLS, ils ont développé une extension TLS permettant de raccourcir la négociation de façon drastique. Cette extension, appelée NPN pour Next Protocol Negotiation, permet au serveur d'indiquer au client quels protocoles il connaît, celui-ci choisissant alors d'utiliser celui qu'il préfère.

## 5.2. http2 pour https://

Une attention particulière a été portée au bon fonctionnement du protocole http2 en conjonction avec TLS. SPDY lui-même ne fonctionnait qu'au dessus de TLS, et nombreux sont ceux qui souhaitaient que http2 ne soit utilisé qu'avec TLS. Ces derniers n'ont pas réussi à obtenir un consensus, et l'utilisation de TLS n'est qu'optionnelle. Néanmoins, les équipes de deux des plus importants navigateurs actuels, Mozilla Firefox et Google Chrome, ont clairement fait savoir qu'ils n'avaient pas l'intention d'implémenter http2 sans TLS.

Les motivations pour imposer TLS impliquent le respect de la vie privée et les mesures montrant que le nouveau protocole avait plus de chances de succès avec TLS. En effet, il est couramment répandu que tout ce qui passe sur le port TCP 80 est du HTTP 1.1 et pas mal d'intermédiaires réseau interfèrent ou cassent le trafic quand d'autres protocoles sont utilisés.

Le sujet TLS obligatoire a causé pas mal d'agitation dans les meetings et mailing-list, est-ce bien ou mal ? C'est une question typiquement empoisonnée, attention quand vous abordez ce sujet avec un membre du groupe!

De même, il y eut un long débat pour savoir si http2 devrait forcer une liste obligatoire d'algorithmes de chiffrement (ciphers en anglais) avec TLS, ou en bloquer certains ou encore laisser cette tâche au groupe de travail TLS. La spécification indique finalement la version minimale TLS à utiliser, 1.2, et une restriction sur les algorithmes à utiliser.

## 5.3. Négociation http2 en TLS

Next Protocol Negotiation (NPN) est le protocole utilisé pour négocier l'usage de SPDY avec les serveurs TLS. Ce n'était pas un protocole standardisé, après passage à l'IETF, il devint ALPN: Application Layer Protocol Negotiation. ALPN est promu pour son utilisation en http2, tandis que les clients et serveurs SPDY continuent d'utiliser NPN.

Le fait que NPN arriva en premier et que la standardisation d'ALPN prit du temps, eut la conséquence d'avoir des clients et serveurs http2 implémentant les deux extensions pour négocier http2. Puisque NPN est utilisé dans SPDY et que de nombreux serveurs offrant SPDY et http2, supporter à la fois NPN et ALPN fait sens.

ALPN diffère de NPN sur qui décide du protocole à utiliser. Avec ALPN, le client indique au serveur la liste des protocoles et ses préférences, le serveur obtient le dernier mot. Au contraire avec NPN, c'est le client qui impose son choix.

## 5.4. http2 for http://

Comme indiqué précédemment, en HTTP 1.1 et en clair, la façon de négocier du http2 est de le demander au serveur avec l'en-tête Upgrade:. Si le serveur parle http2, il répond avec le code "101 Switching" et passe en http2 pour cette connexion. Bien que cette procédure d'upgrade coûte un aller-retour réseau, elle peut être conservée et réutilisée de manière bien plus efficace qu'une connexion HTTP1, amortissant ainsi le coût initial.

Même si certains porte-paroles de navigateurs ont indiqué qu'ils n'implémenteraient pas cette méthode, l'équipe d'Internet Explorer le fera, et curl la supporte déjà.