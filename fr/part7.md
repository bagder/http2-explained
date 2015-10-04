# 7. Extensions

Le protocole oblige le destinataire à lire et ignorer toutes les trames inconnues utilisant un type de trame inconnu. Les deux parties peuvent ainsi négocier l'utilisation d'un nouveau type de trame de manière unitaire; ces trames ne seront pas autorisées à changer d'état et ne bénéficieront pas de contrôle de flux.

Le fait d'autoriser ou non les extensions dans http2 a été débattu pendant le développement du protocole avec des opinions pour et contre. Depuis le draft-12, la balance a penché en faveur du pour: les extensions sont autorisées.

Les extensions ne font pas partie du protocole actuel mais seront documentées en dehors de la spécification principale. En l'état, il existe deux types de trames qui pouvaient faire partie du protocole et qui seront probablement les premières trames définies comme extensions. Je les décris ici car elles sont populaires et étaient considérées auparavant comme des trames "natives":

## 7.1. Services Alternatifs

Avec http2 adopté, on peut suspecter que les connexions TCP seront plus longues (en temps) et maintenues actives plus longtemps que les connexions HTTP 1.x. Un client doit donc pouvoir se débrouiller avec une seule connexion par hôte/site et cette connexion pourrait rester ouverte pendant un certain temps.

Cela affectera comment les load balancers HTTP réagissent quand un site voudra que les utilisateurs se connectent sur un autre host, pour des raisons de performance ou pour réaliser une maintenance.

Le serveur enverra alors [l'en-tête Alt-Svc:](http://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07) (ou la trame http2 ALTSVC) pour indiquer au client un service alternatif. Une autre route pour le même contenu, utilisant un autre service, host et numéro de port.

Un client est alors susceptible d'essayer de se connecter à ce service de manière asynchrone et n'utiliser que celui-ci s'il fonctionne.

### 7.1.1. TLS opportuniste

L'en-tête Alt-Svc permet à un serveur servant du contenu en http:// d'informer le client que le même contenu est aussi disponible en TLS.

Cette fonctionnalité est débattue. Cette connexion serait du TLS non authentifié et ne serait pas affichée comme "sécurisée", n'utiliserait pas de cadenas dans l'interface graphique du navigateur même si ce n'est pas du simple HTTP. Plusieurs personnes sont contre.

## 7.2. Bloqué

Ce type de trame doit être envoyée une seule fois par un client http2 quand des données doivent être envoyées alors que le contrôle de flux l'interdit. L'idée est que si votre implémentation reçoit une telle trame, cela vous indique que quelque chose cloche dans votre implémentation et que vous avez une performance suboptimale.

Une citation du draft-12, avant que cette trame ne soit retirée pour devenir une extension:

> “La trame BLOCKED est incluse dans ce draft pour en faciliter son expérimentation. Si les resultats de cette expérimentation ne donnent pas de feedback positif, elle pourra être retirée”

