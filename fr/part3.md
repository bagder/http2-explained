# 3. Rustines pour s'accommoder de la latence

Comme toujours face aux problèmes, les gens trouvent des solutions de contournement. Certaines sont astucieuses et utiles, d'autres sont juste d'horribles rustines.

## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Spriting est un terme anglais souvent utilisé pour décrire la consolidation de petites images en une seule grosse image. Cette image est ensuite découpée en petites images individuelles, via l'utilisation de JavaScript ou de CSS.

Cette astuce est utilisée car l'obtention d'une seule grosse image est beaucoup plus rapide en HTTP 1.1 que celle de 100 petites.

Bien sûr, cela représente une surcharge pour les pages qui n'ont besoin que d'une ou deux images de la mosaïque. Cela rend aussi le cache moins pertinent car on vide du cache toutes les images de la mosaïque en une fois au lieu de garder les images les plus utilisées dans le cache.

## 3.2 Inlining

L'inlining (en ligne, en français) est une autre astuce évitant l'envoi d'images individuellement. Il est possible d'imbriquer des données à l'intérieur des URLs présentes dans le CSS. Ce genre d'approche offre des avantages et inconvénients similaires au spriting.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Concaténation

Il est courant pour des sites de taille importante d'utiliser plusieurs fichiers JavaScript séparés. Les outils de conception de sites permettent aux développeurs de fusionner ces fichiers pour qu'un navigateur ne fasse qu'une seule requête vers un gros fichier JavaScript. L'inconvénient de cette méthode est qu'elle nécessite le chargement d'une quantité importante de données là où seule une petite partie est réellement nécessaire. De la même façon, l'intégralité du fichier doit être téléchargée à nouveau si une infime partie est modifiée.

## 3.4 Sharding

La dernière astuce que je veux mentionner est connue sous le nom de "sharding". C'est la possibilité de charger le contenu d'un site depuis autant de hosts que possible. À première vue, cela paraît étrange mais c'est finalement assez astucieux.

HTTP 1.1 limitait initialement à deux le nombre de connexions TCP simultanées d'un client à un même host. Pour ne pas contredire la spécification, des sites astucieux créaient simplement de nouveaux noms de hosts, et voilà, vous pouvez avoir davantage de connexions vers votre site et réduire le temps de chargement.

Avec le temps, cette limitation a été levée et les clients utilisent aujourd'hui typiquement 6 à 8 connexions par nom de host; cela dit, la limite perdure et certains sites continuent d'utiliser cette technique pour accroître le nombre de connexions. Comme le nombre d'objets augmente continuellement, l'utilisation d'un grand nombre de connexions permet de maximiser les performances. Il n'est pas inhabituel de voir des sites utiliser plus de 50 voire 100 connexions pour un seul site utilisant cette technique. Des statistiques récentes de httparchive.org montrent que le top 300.000 des URLs requiert en moyenne 40(!) connexions TCP pour afficher le site, et que ce nombre augmente de façon continue.

La taille des cookies devenant conséquente, il est également intéressant de placer certaines ressources comme les images sur un nom d'hôte distinct, n'utilisant pas de cookies. On augmente ainsi la performance en diminuant la taille des requêtes HTTP pour ces ressources.

L'image ci dessous montre une capture de trafic lors du chargement d'un site suédois connu et comment les requêtes sont réparties sur différents noms d'hôtes.

![image sharing at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
