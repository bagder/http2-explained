# 2. HTTP aujourd'hui

Le protocole HTTP 1.1 est omniprésent sur Internet. De nombreux investissements ont été réalisés sur des protocoles et infrastructures tirant profit de celui-ci. À tel point que lors de l'implémentation d'un nouveau projet, il est souvent plus facile d'utiliser HTTP plutôt que de développer un nouveau protocole.

## 2.1 HTTP 1.1 est très dense

Lors de la création de HTTP, il fut probablement perçu comme un protocole plutôt simple et évident, ce qui, avec le temps, s'est révélé faux. HTTP 1.0 spécifié dans la RFC 1945 est une spécification de 60 pages datant de 1996. La RFC 2616 qui décrit HTTP 1.1 a été publiée trois ans plus tard en 1999 et comprend 176 pages. Puis, quand nous, à l'IETF, avons mis à jour cette spécification, elle a été répartie en six documents, avec davantage de pages au total (RFC 7230 et associées). Tout bien considéré, HTTP 1.1 est dense et comporte une multitude de détails, subtilités et, non dans une moindre mesure, de très nombreux points optionnels.

## 2.2 Une flopée d'options

De par sa nature, HTTP 1.1 possède plusieurs options et petits détails permettant l'ajout d'extensions ultérieures. Ce qui a mené à un écosystème logiciel où aucune implémentation n'a tout couvert (encore faut-il définir ce que "tout" représente). Cela a mené à un cercle vicieux où les fonctionnalités peu utilisées ont été peu implémentées ce qui en limitait l'utilité pour ceux qui en faisaient usage.

Plus tard, cela a causé des problèmes d'interopérabilité entre clients et serveurs qui utilisaient certaines de ces fonctionnalités. Le pipelining HTTP en est un exemple parlant.

## 2.3 Usage incorrect de TCP

HTTP 1.1 a du mal à vraiment tirer parti de la puissance et des performances offertes par TCP. Les clients et navigateurs HTTP doivent être très créatifs pour trouver des solutions qui abaissent les temps de chargement de pages web.

D'autres expérimentations menées en parallèle à travers les années ont confirmé qu'il est difficile de remplacer TCP et qu'il fallait donc améliorer à la fois TCP et les protocoles au-dessus.

En clair, TCP peut être utilisé à meilleur escient en évitant les pauses ou en utilisant certains moments pour envoyer et recevoir des données. Les chapitres suivants développeront ces points.

## 2.4 Volume de données et nombre d'objets

En regardant la tendance parmi les sites les plus importants sur le web aujourd'hui et ce que cela implique pour télécharger leurs pages d'accueil, une tendance se dégage. Au fil des années, le nombre de données à transférer a augmenté régulièrement pour atteindre et même surpasser 1.9Mo. Plus important encore, le nombre moyen de ressources distinctes (ou objets) va au-delà de la centaine pour afficher chaque page.

Le graphique ci-dessous montre que cette tendance date déjà et rien n'indique qu'elle s'inversera. On y constate l'évolution, sur les quatre dernières années, du volume de données (en vert) et du nombre moyen de requêtes (en rouge) pour le chargement des pages web les plus populaires au monde.

![évolution du volume des données](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 La latence tue

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 est très sensible à la latence, en partie à cause du pipelining HTTP qui demeure problématique à un tel point qu'il est désactivé pour la plupart des utilisateurs.

Bien qu'on ait observé une augmentation de la bande passante depuis plusieurs années, la réduction de la latence n'a pas suivi la même évolution. Les liens à forte latence, comme les technologies mobiles, rendent l'expérience Web difficile même avec une connexion haut débit.

D'autres usages qui nécessitent une latence faible sont certains types de vidéos comme : la vidéo-conférence, le jeu en ligne ainsi que les flux vidéos générés en direct.

## 2.6. Blocage en tête de file (Head-of-line blocking)

Le pipelining HTTP est une manière d'envoyer une requête additionnelle sans attendre la réponse de la requête précédente. C'est semblable à la file d'attente d'une caisse à la banque ou au supermarché. Vous ne savez pas si le client vous précédant est rapide ou s'il s'agit d'une personne qui prendra son temps. Ce phénomène décrit parfaitement le “head-of-line blocking”.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Bien sûr, vous pouvez faire attention et choisir la file que vous croyez être la meilleure, et parfois vous pouvez en commencer une nouvelle, mais vous devez prendre une décision que vous ne pourrez plus changer par la suite.

Créer une nouvelle file joue sur les performances et les ressources disponibles, ce qui ne permet pas de créer de nouvelles files à l'infini. Il n'y a pas de solution parfaite.

Même aujourd'hui, en 2015, le pipelining HTTP est désactivé par défaut sur la plupart des navigateurs destinés aux ordinateurs personnels.

Plus de détails sur ce sujet peuvent être consultés dans l'entrée [264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354) de la base de données Bugzilla de Firefox.
