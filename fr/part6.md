# 6. Le protocole http2

Assez sur la gestation et la politique qui nous ont mené ici. Plongeons dans les spécificités du protocole: les détails et concepts qui font http2.

## 6.1. Binaire

http2 est un protocole binaire.

Posons-nous un moment. Si vous avez déjà travaillé sur des choix de protocoles IP par le passé, il est probable que votre première réaction soit négative, et que vous argumentiez que les protocoles textuels sont bien supérieurs car les êtres humains peuvent forger des requêtes à la main via telnet, etc.

http2 est binaire pour rendre le découpage (framing en anglais) plus simple. Trouver le début et la fin d'une trame en HTTP 1.1 et dans les protocoles textuels en général est toujours très compliqué. En évitant les blancs optionnels et les diverses façons d'écrire la même chose, les implémentations sont plus simples.

De plus, cela permet de séparer plus nettement les parties du protocole et le découpage. Ce manque de séparation nette a toujours été perturbant avec HTTP1.

Le fait que le protocole utilise la compression et TLS diminue la valeur ajoutée de garder du texte pur puisqu'on ne verra pas de texte en clair passer de tout façon. Nous devons simplement nous habituer à utiliser un analyseur de trafic type Wireshark pour comprendre exactement ce qu'il passe au niveau protocolaire http2.

Débugger ce protocole se fera du coup probablement via des outils type curl ou le dissecteur http2 de Wireshark.

## 6.2. Le format binaire

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2 envoit des trames binaires. Il y a différentes types de trames envoyées et elles ont toutes le même format:

Type, Longueur, Flags, Identifiant de Flux (Stream ID) et contenu (payload).

Il y a dix trames différentes définies dans la spec http2 et deux sont fondamentales car répliquent le fonctionnement HTTP 1.1 avec DATA (données ou contenu) et HEADERS (en-tête). Je décris ces trames plus en détail par la suite.

## 6.3. Multiplexage de flux

L'identifiant de flux (Stream Identifier) mentionné dans le chapitre précédent permet d'associer à chaque trame http2 un "flux" (stream). Un flux est une association logique: une séquence indépendante et bidirectionnelle de trames échangées entre un client et un serveur via une connexion http2.

Une seule connexion http2 peut contenir plusieurs flux simultanés, avec chaque extrémité de la connexion pouvant entrelacer les flux. Les flux sont établis et utilisés unilatéralement ou de concert par le client ou le serveur. Ils peuvent être terminés par l'un ou l'autre côté de la connexion. L'ordre d'envoi des trames dans un flux est important. Le traitement des trames reçues se fait dans l'ordre de réception.

Multiplexer des flux implique que des groupes de trames sont mélangés (ou “mixés”) sur une même connexion. Deux (ou plusieurs) trains de données sont fusionnés en un seul puis scindés à nouveau à la réception. Voici deux trains:

![un train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![un autre train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

Ils sont mélangés l'un dans l'autre sur la même connexion via multiplexage:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

En http2 nous allons voir des dizaines voire des centaines de flux simultanés. Le coût de création d'un nouveau flux est très faible.

## 6.4. Priorités et dépendances

Chaque flux a une priorité (ou “poids”), utilisée pour dire à l'autre extrémité de connexion quels flux sont importants, en cas de congestion (manque de ressources) forçant le serveur à faire un choix sur les trames à envoyer en premier.

En utilisant la trame PRIORITY, un client peut indiquer au serveur quel autre flux dépend du flux actuel. Cela permet au client de construire un “arbre” de priorités dans lequel des “flux enfants” dépendront de la réussite des “flux parents”.

Les priorités peuvent changer dynamiquement, ce qui permettra aux navigateurs d'indiquer quelles images sont importantes quand on fait défiler une page remplie d'images, ou encore de prioriser les flux affichés à l'écran quand on passe d'un onglet à un autre.

## 6.5. Compression de l'en-tête.

HTTP est un protocole sans état (state-less). Cela veut dire que chaque requête doit apporter tous les détails requis au serveur pour traiter la requête, sans que le serveur ait besoin de conserver les informations ou meta-données des requêtes précédentes. Comme http2 ne change pas ce principe, il doit en faire de même.

Cela rend HTTP répétitif. Quand un client demande plusieurs ressources d'un même site, comme les images d'un site web, il y a aura une série de requêtes quasi identiques. Une série de points identiques appelle un besoin de compression.

Le nombre d'objets par page web croît comme vu précédemment, l'utilisation de cookies et la taille des requêtes croît également. Les cookies sont inclus dans toutes les requêtes, souvent identiques sur plusieurs requêtes.

Les tailles de requêtes HTTP 1.1 sont devenues tellement importantes qu'elles dépassent parfois la taille de la fenêtre TCP initiale, ce qui rend le tout très lent puisqu'un aller-retour est nécessaire pour qu'un ACK soit envoyé par le serveur. Encore une bonne raison de compresser.

### 6.5.1. La compression est un sujet délicat

Les compressions HTTPS et SPDY ont été vulnérables aux attaques [BREACH](http://en.wikipedia.org/wiki/BREACH_%28security_exploit%29)
 et [CRIME](http://en.wikipedia.org/wiki/CRIME). En insérant un texte connu dans le flux et en analysant les changements résultant, l'attaquant peut déduire ce qui a été envoyé.

Compresser du contenu dynamique sans devenir vulnérable à une de ces attaques requiert de la réflexion. L'équipe HTTPbis s'y attelle.

Voici [HPACK](http://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2 (compression d'en-tête pour HTTP/2), qui, comme son nom l'indique, est un format de compression spécialement créé pour les entêtes http2 et spécifié dans un draft IETF distinct. Le nouveau format, avec d'autres contre-mesures comme un bit qui interdit aux intermédiaires de compresser un en-tête spécifique ou du remplissage de trames (padding), devrait rendre plus compliqué l'exploitation de cette compression.

Voici les les mots de Roberto Peon (l'un des créateurs de HPACK):

> “HPACK a été conçu pour rendre difficile la fuite d'information avec une implémentation s'y conformant, rendre le codage et décodage très rapide et peu coûteux, fournir au destinataire un contrôle sur la taille du contexte de compression, permettre une réindexation par un proxy, et permettre une comparaison rapide avec des chaînes codées avec un algorithme Huffman.”.

## 6.6. Reset - changez de perspective

Un des inconvénients de HTTP 1.1: quand le message HTTP est envoyé avec un en-tête Content-Length d'une taille particulière, on ne peut pas l'interrompre facilement. Bien sûr vous pouvez toujours terminer la session TCP mais cela a un coût: renégocier le handshake TCP.

Une meilleure solution serait simplement d'interrompre le message et en démarrer un nouveau. Cela peut se faire en http2 avec la trame RST_STREAM qui permet d'éviter de gâcher de la bande passante et de terminer des connexion TCP.

## 6.7. Server push

Cette fonctionnalité est aussi connue comme "cache push". Cas typique: un client demande une ressource X au serveur, le serveur sait qu'en général ce client aura besoin de la ressource Z par la suite et la lui envoie sans que le client l'ait demandée. Cela aide le client à mettre Z dans son cache pour qu'elle soit disponible quand il en aura besoin.

Le Push Serveur (Server Push) doit être explicitement autorisée par le client et, quand bien même le client l'autorise, ce dernier se réserve le droit de terminer un flux "poussé" avec un RST_STREAM.

## 6.8. Contrôle de flux

Chaque flux individuel en http2 a sa propre fenêtre de flux annoncée que l'autre extrémité du flux peut utiliser pour envoyer des données. Si vous savez comment SSH fonctionne, le principe est très similaire.

Pour chaque flux, les deux extrémités doivent indiquer qu'ils ont davantage de place pour accepter des données supplémentaires, l'autre extrémité est autorisée à envoyer ce volume de données uniquement jusqu'à l'extension de la fenêtre. Seules les trames DATA (données) bénéficient du contrôle de flux.
