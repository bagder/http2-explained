# 4. Mettre à jour HTTP

Ce serait sympa d'améliorer ce protocole, n'est-ce pas ? Notamment:

1. Un protocole moins sensible au RTT (Round Trip Time, Aller-Retours)
2. En corrigeant le pipelining et le head of line blocking
3. En évitant le besoin de multiplier le nombre de connexions vers un même hôte
4. En conservant toutes les interfaces existantes, tout le contenu et le format d'URI
5. Tout cela à travers le groupe de travail de l'IETF HTTPbis

## 4.1. IETF et le groupe de travail HTTPbis

L'IETF (Internet Engineering Task Force) est une organisation qui développe et promeut les standards de l'Internet, et ce essentiellement au niveau protocolaire. Très connue pour la série de RFC documentant tout depuis TCP, DNS, FTP, HTTP aux bonnes pratiques en passant par des variantes de protocoles qui n'ont jamais vu le jour.

Au sein de l'IETF existent des groupes de travail se penchant sur un sujet bien établi avec des objectifs précis. Ils établissent une charte qui définit les règles et limitations liées à l'objectif. Toute personne intéressée peut se joindre aux discussions et au développement, et chaque participant bénéficie du même droit de parole et du même poids que les autres. Peu d'importance est apportée aux sociétés pour lesquelles les participants travaillent (le cas échéant).

Le groupe de travail HTTPbis (voir plus tard pour l'origine du nom) a été créé à l'été 2007 et chargé de mettre à jour la spécification HTTP 1.1. Des discussions sur une nouvelle version de HTTP ont réellement commencé fin 2012. La mise à jour HTTP 1.1 s'est terminée début 2014 avec la série des [RFC 7230](https://tools.ietf.org/html/rfc7230).

La réunion finale d'interopérabilité pour le groupe de travail HTTPbis s'est tenue à New York en juin 2014. Toutefois les discussions en suspens et les procédures IETF nécessaires pour obtenir la RFC officielle continueront jusque l'année suivante.

Des acteurs importants de HTTP ont manqué à l'appel du groupe de travail pendant les discussions et réunions. Je ne souhaite pas mentionner de nom de société ou produit, mais il est clair que ces acteurs de l'Internet font confiance à l'IETF pour finir le travail sans leur participation...

### 4.1.1. Le "bis" dans HTTPbis

Le groupe est appelé HTTPbis, le "bis" faisant référence au [latin "bis"](https://fr.wiktionary.org/wiki/bis#Latin). Bis est souvent utilisé comme suffixe ou partie d'un nom à l'IETF lors d'une mise à jour ou addendum d'une spécification. Comme ici avec HTTP 1.1, donc.

## 4.2. http2 provient de SPDY

[SPDY](https://en.wikipedia.org/wiki/SPDY) est un protocole développé par Google. Ils l'ont clairement créé dans un esprit d'ouverture et ont invité tout le monde à participer, mais il était évident qu'ils bénéficiaient d'une position de contrôle avec à la fois un navigateur populaire et un nombre significatif de serveurs très utilisés pour leurs services.

Quand le groupe HTTPbis décida de travailler sur http2, SPDY avait déjà prouvé que le concept fonctionnait. Il avait montré qu'il était possible de le déployer sur Internet et des mesures publiées montraient sa meilleure performance. Le travail sur http2 a donc démarré depuis le draft SPDY/3, rapidement transcrit dans un draft-00 de http2 grâce à quelques remplacements.
