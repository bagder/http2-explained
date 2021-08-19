# 1. Background

Ce document décrit http2 d'un point de vue technique et protocolaire. Il a commencé par une présentation à Stockholm réalisée par Daniel en avril 2014, présentation qui a été par la suite convertie et étoffée dans un document complet avec des explications détaillées.

La RFC 7540 est le nom officiel de la spécification http2 finale, elle a été publiée le 15 mai 2015 : https://www.rfc-editor.org/rfc/rfc7540.txt

Toute erreur dans ce document est mienne et le résultat de mes approximations. Merci de me les indiquer afin que je les corrige pour les prochaines versions.

Dans ce document, j'ai essayé d'utiliser le terme "http2" pour décrire le nouveau protocole alors que techniquement le nom officiel est HTTP/2. J'ai fait ce choix pour améliorer la fluidité de lecture et obtenir une meilleure lisibilité.

## 1.1 Auteur

Mon nom est Daniel Stenberg. J'ai travaillé dans l'open source et la réseautique, depuis plus de 20 ans, sur de nombreux projets. Je suis peut-être surtout connu en tant que développeur principal de curl et libcurl. J'ai été impliqué dans le groupe de travail de l'IETF HTTPbis pendant plusieurs années où j'ai maintenu à jour les spécifications HTTP 1.1 et participé à la standardisation de http2.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Aide !

Si vous trouvez des erreurs, oublis, fautes ou mensonges éhontés dans ce document, je vous prierais de bien vouloir m'envoyer une version corrigée que je publierai dans une édition révisée. Je mentionnerai clairement les noms des contributeurs ! J'espère améliorer ce document avec le temps.

Ce document est disponible ici: [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 License

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Ce document est couvert par la licence Creative Commons Attribution 4.0 : https://creativecommons.org/licenses/by/4.0/

## 1.4 Historique du document

La première édition de ce document fut publiée le 25 avril 2014. Voici les changements majeurs des dernières éditions.

### Version 1.13 :
 
- Conversion de la version principale de ce document au format Markdown
- 13: Mention de plus de ressources, mise à jour des liens et descriptions
- 12: Mise à jour de la description de QUIC en mentionnant le draft
- 8.5: Mise à jour des chiffres
- 3.4: La moyenne est maintenant de 40 connexions TCP
- 6.4: Mise à jour en s'alignant sur la spécification

### Version 1.12 :

- 1.1: HTTP/2 est maintenant une RFC officielle
- 6.5.1: Lien vers la RFC HPACK
- 9.2: Mention du changement de config pour http2 dans Firefox 36 et +
- 12.1: Nouvelle section sur QUIC

### Version 1.11 :

- Nombreuses améliorations de style linguistique (Note du traducteur : en anglais)
- 8.3.1: Mention des développements spécifiques nginx et Apache httpd

### Version 1.10 :
 
- 1: Le protocole est "presque approuvé"
- 4.1: Rafraîchissement du style linguistique (Note traducteur : en anglais)
- Converture : ajout de l'image et légende "http2 explained", lien corrigé
- 1.4: Ajout de la section "Historique"
- Diverses corrections orthographiques
- 14: Ajout de remerciements pour les contributeurs
- 2.3: Meilleurs libellés pour le graphique de croissance HTTP
- 6.3: Correction de l'ordre des wagons dans le train multiplexé
- 6.5.1: HPACK draft-12

### Version 1.9

- Mise à jour HTTP/2 draft-17 et HPACK draft-11
- Ajout de la section "10. http2 et Chromium"
- Diverses corrections orthographiques
- Désormais 30 implémentations
- 8.5: Ajout des chiffres d'utilisation
- 8.3: Mention d'Internet Explorer
- 8.3.1: Ajout des "implémentations manquantes"
- 8.4.3: Mention que TLS améliore le taux de réussite
