# 8. Le monde http2

Que donnera le monde une fois http2 adopté ? http2 sera-t-il adopté ?

## 8.1. Comment http2 affecte les humains ?

http2 n'est pas encore largement déployé ou utilisé. Nous ne pouvons savoir ce qu'il adviendra. Nous avons vu comment SPDY a été utilisé et pouvons estimer et calculer l'adoption de http2 à partir de ces expérimentations passées et actuelles.

http2 réduit le nombre d'aller-retours nécessaires et évite le head of line blocking en multiplexant et en se débarrassant des flux non voulus.

Il permet un nombre important de flux parallèles, plus important que ce qui est requis par les sites utilisant massivement le sharding aujourd'hui.

Si les priorités de flux sont utilisées correctement, on a une chance que les clients obtiennent les données les plus importantes avant les moins importantes. Tout compris, je dirais qu'il y de très bonnes chances pour que cela mène à un meilleur temps de chargement des pages et à des sites web plus réactifs. En résumé: une meilleure expérience web.

Dans quelle mesure cela sera plus rapide, je ne pense pas qu'on puisse y répondre pour l'instant. La technologie est encore très jeune et nous n'avons pas encore vu d'implémentations client et serveur tirer parti de toutes les possibilités que le protocole offre.

## 8.2. Comment http2 affectera le développement web ?

Avec le temps, les développeurs Web et les environnement de développement ont accumulé plein d'astuces et outils pour contourner les limitations de HTTP 1.1, souvenez-vous de tout ce qui a été indiqué en début de document pour justifier http2.

Beaucoup de contournements utilisés par défaut sans y penser, pénaliseront probablement les performances de http2 ou ne permettront pas l'utilisation des super pouvoirs de http2. Spriting et inlining ne devraient probablement pas être utilisés avec http2. Le sharding pénalisera http2 car il bénéficiera de moins de connexions.

Un problème ici est bien sûr que les sites et développeurs web devront avoir, au moins dans un premier temps, des versions s'accommodant de clients HTTP1.1 et http2. Et avoir les meilleures performances pour tous ces utilisateurs sera compliqué sans devoir afficher deux front-ends différents.

Pour ces seules raisons, je pense qu'il faudra du temps avant de tirer le plus grand bénéfice de http2.

## 8.3. Implémentations http2

Essayer de documenter des implémentations particulières dans un document comme celui-ci est futile et voué à l'échec car il sera obsolète en peu de temps. À la place, je vous explique la situation en termes plus généraux et vous renvoie à la [liste exhaustive des implémentations](https://github.com/http2/http2-spec/wiki/Implementations) http2 sur le site web.

Il y avait déjà un certain nombre d'implémentations très tôt, et ce nombre a augmenté pendant le travail sur http2. À ce jour, il existe 30 implémentations connues, et la plupart se basent sur la version finale de http2.

Firefox a été le navigateur le plus en avance, Twitter a suivi et propose ses services en http2. Google commença en avril 2014 à offrir ses services en http2 sur quelques serveurs et depuis mai 2014 via les versions de développement de Chrome. Microsoft a montré une préversion d'Internet Explorer supportant http2.

curl et libcurl supportent http2 non-TLS et TLS à partir d'une des librairies TLS disponibles.

### 8.3.1. Implémentations manquantes

Il manque des marques importantes parmi la liste des implémentations existantes. Nous n'avons rien entendu de la part d'Apple quant au support http2 dans Safari. Les deux serveurs les plus populaires Apache HTTPD et Nginx supportent SPDY mais pas encore http2.

Nginx [indique](https://www.nginx.com/blog/how-nginx-plans-to-support-http2/) qu'ils "supporteront HTTP/2 avec nginx et NGINX Plus d'ici la fin 2015". Il y a une version préliminaire d'un module HTTP/2 pour Apache appelé [mod_h2](https://icing.github.io/mod_h2/), qualifié de "version alpha".

## 8.4. Critiques courantes de http2

Pendant le développement de ce protocole, les débats n'ont jamais cessé et plusieurs personnes pensent que ce protocole a mal fini. Je voudrais ici lister quelques problèmes et les discuter :

### 8.4.1. "Le protocole a été écrit par Google"

Cela implique aussi que le monde devient davantage dépendant de Google. Ce n'est pas vrai. Le protocole a été développé à l'IETF de la même manière que tous les autres protocoles depuis 30 ans. Cela dit, nous reconnaissons tous le travail impressionnant réalisé par Google sur SPDY qui a permis de montrer qu'on pouvait déployer un nouveau protocole avec, de plus, des données chiffrées sur les gains obtenus.

Google a publiquement [annoncé](http://blog.chromium.org/2015/02/hello-http2-goodbye-spdy-http-is_9.html) qu'ils retireraient le support de SPDY et NPN de Chrome en 2016 et qu'ils poussaient les serveurs à migrer vers HTTP/2 à la place.

### 8.4.2. "Ce protocole est seulement utile aux navigateurs"

C'est plutôt vrai. Une des principales raisons derrière le développement de http2 est de corriger le pipelining HTTP. Si votre cas d'usage n'avait pas besoin de pipelining alors il y a des chances que http2 n'améliore pas beaucoup votre situation. Ce n'est pas la seule amélioration du protocole mais une amélioration majeure.

Quand les différents services réaliseront la puissance des flux multiplexés sur une seule connexion, je pense que nous verrons davantage d'applications tirant parti de http2.

Les APIs REST simples utilisant HTTP 1.x ne verront pas d'avantages à passer à http2. Cela dit, il y aura peu d'inconvénients à passer à http2 pour la plupart des gens.

### 8.4.3. "Ce protocole n'est utile que pour les très gros sites"

Pas du tout. Les capacités de multiplexage vont énormément améliorer l'expérience des connexions à latence importante pour les petits sites n'ayant pas de présence mondiale.

Les gros sites ont déjà une présence mondiale et du coup des aller-retours moins longs vers la plupart des utilisateurs.

### 8.4.4. “Its use of TLS makes it slower”

Cela peut se révéler vrai. La négociation TLS ajoute un peu de latence, mais il existe des projets pour réduire encore les aller-retours en TLS. La surcharge du TLS par rapport à du texte en clair n'est pas neutre et a un impact CPU. L'impact en lui-même est sujet à discussions et mesures. Voir par exemple [istlsfastyet.com](https://istlsfastyet.com/) pour avoir une source d'information.

Les opérateurs télécom et réseaux, par exemple l'ATIS Open Web Alliance, indiquent qu'ils nécessitent du [trafic non chiffré](http://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf) pour permettre au cache et à la compression de fonctionner, notamment pour une expérience web rapide par satellite.
http2 n'oblige pas à utiliser TLS, on ne devrait donc pas mélanger les deux.

De nombreux utilisateurs ont indiqué leur préférence à utiliser TLS et nous devrions respecter ce droit à la vie privée.

Des expérimentations ont montré que l'utilisation de TLS a une meilleure probabilité de succès qu'avec un protocole en clair sur le port 80. Il y a trop de boîtes intermédiaires sur le réseau qui vont perturber le trafic sur le port 80 en imaginant qu'il n'y a que du trafic HTTP 1.1 sur ce port.

Enfin, grâce aux flux multiplexés que permet http2 sur une seule connexion, les navigateurs initieront moins de négociations TLS et iront finalement plus vite qu'avec HTTPS en HTTP 1.1.

### 8.4.5. "Pas de l'ASCII = inutilisable"

Oui, nous préférons voir des protocoles en clair car le debugging en est facilité. Mais un protocole texte est aussi davantage sujet à des problèmes de parsing.

Si vous ne pouvez pas vous faire à un protocole binaire, alors vous ne pouviez pas vous faire non plus à TLS ni à la compression, utilisés depuis longtemps en HTTP 1.x.

### 8.4.6. "Ce n'est pas plus rapide que HTTP/1.1"

Cela est bien sûr sujet à débat sur comment qualifier "plus rapide"; les tests menés lors des expérimentations SPDY montraient des temps de chargement de pages web plus rapides (voir [“How Speedy is SPDY?”](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) par University of Washington et [“Evaluating the Performance of SPDY-enabled Web Servers”](http://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) par Hervé Servy). Idem en http2 avec d'autres tests. Je souhaite voir davantage de tests publiés. Un [premier essai réalisé par httpwatch.com](http://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) a tendance à montrer que HTTP/2 répond aux promesses.

http2 est clairement plus rapide dans certains scénarios, en particulier avec les scénarios où une connexion à latence importante est utilisée avec un site comportant beaucoup de ressources à charger, ce nombre ayant tendance à augmenter.

### 8.4.7. "Il y a violation de couches"

Sérieusement ? Les couches OSI ne sont pas intouchables, nous avons franchi des zones grises avec http2 dans l'intérêt de le rendre efficace dans les limites imposées.

### 8.4.8. "Cela ne corrige pas certaines limitations HTTP/1.1"

C'est vrai. L'objectif de conserver certains paradigmes HTTP/1.1 font que certaines vieilles fonctionnalités restent. Par exemple les en-têtes conservent les cookies, l'authentification et davantage. Mais en maintenant ces principes nous avons un protocole qu'il est possible de déployer sans réécrire ou remplacer des parties protocolaires considérables. http2 est juste une nouvelle couche de framing.

## 8.5. http2 sera-t-il largement déployé ?

Il est encore trop tôt pour le dire, mais je peux le deviner et l'estimer, voici comment.

Les esprits négatifs montreront "regardez comme IPv6 a marché" comme un exemple qui a pris des décennies pour juste commencer à être largement déployé. http2 n'est pas IPv6. C'est un protocole au-dessus de TCP utilisant les mécanismes d'upgrade HTTP, un port standard, TLS, etc. Il ne requiert pas un changement de la plupart des routeurs et firewalls.

Avec SPDY, Google a prouvé au monde que l'on pouvait déployer et utiliser un nouveau protocole en peu de temps. Même si la somme des serveurs SPDY avoisine les 1% aujourd'hui, le volume de données est plus important. Certains des plus gros sites utilisent SPDY.

http2, basé sur les même principes que SPDY et ratifié par l'IETF, devrait être encore plus largement déployé. Les déploiements SPDY ont toujours été limités par le syndrome "inventé par Google"

Il y a plusieurs navigateurs importants derrière http2. Des représentants de Firefox, Chrome, Safari, Internet Explorer et Opera ont tous indiqué leur intention de livrer des navigateurs avec http2 et ont montré des implémentations fonctionnelles.

Plusieurs gros sites proposent http2, avec Google, Twitter et Facebook et nous espérons que http2 sera ajouté aux serveurs Apache HTTP et nginx. H2o est un nouveau serveur très rapide supportant http2 avec beaucoup de potentiel.

Les plus gros proxy, HAProxy, Squid et Varnish ont mentionné leur intention de supporter http2.

Je pense qu'il y aura davantage d'implémentations quand la RFC sera ratifiée.

Durant tout 2015, la quantité de trafic en http2 n'a cessé d'augmenter. Au début Septembre, sur Firefox 40, il génère 13% de tout le trafic HTTP, et 27% du trafic HTTPS, tandis que Google voit 18% de HTTP/2. Il faut noter que Google déroule d'autres expérimentation en parallèle (voir QUIC en 12.1), ce qui rend la quantité de trafic http2 plus basse que ce qu'elle aurait pu être.
