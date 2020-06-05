# Mapping Coordonnées GPS ThingSpeak

Lorsque l'on reçoit des données GPS sur ThingSpeak, il peut être intéressant de les afficher sur une carte pour avoir une idée précise de la localisation.
Sur le channel, on peut effectivement créer des maps avec MathLab qui permettent la visualisation mais celles-ci ne sont pas très lisibles et surtout pas interactives puisque l'on ne peut pas zoomer ou se déplacer dessus.

Pour mettre en place une maps interactive, j'ai utilisé le code de Google Maps disponible sur Google Developer et j'ai adpaté ce code pour récupérer les données de ThingSpeak grâce à l'API de lecture.


L'intégralité du code est disponible sur GitHub et je vais uniquement expliquer les parties importantes de la configuration dans ce rapport.
A noter que j'ai retiré ma clé d'API Google du code pour éviter quelle soit utilisée par une personne tierce.

## Récupération du code GoogleMaps

Avant pouvoir commencer à utiliser la carte Google Maps, il faut se connecter au [site](https://console.developers.google.com/apis/) pour avoir une clé d'API Google. À noter que ce service est payant en temps normal mais en tant que nouvel utilisateur, on dispose d'un crédit de 400$ gratuitement ce qui permet de faire déjà un bon nombre de manipulations.

Pour obtenir la clé d'API on se rend dans un premier temps dans la catégorie "Identifiants":

![](https://i.imgur.com/qeBxYEG.png)

Ensuite, dans le menu supérieur, on choisit "Créer des identifiants" puis "Clé API":


![](https://i.imgur.com/NYisAPH.png)

On obtient la clé d'API qui est automatiquement sauvegardée sur l'interface de votre compte. Vous pouvez aussi mettre des restrictions sur celle-ci mais je ne m'attarderai pas sur ce point.

![](https://i.imgur.com/Ctl1p50.png)

Enfin, puisque l'on a les éléments nécessaires pour faire fonctionner la carte Google, on peut récupérer le code sur ce [lien](https://developers.google.com/maps/documentation/javascript/adding-a-google-map).

Ce code fournit par Google contient déjà des coordonnées GPS fixes qui pointent en Australie. Je vais détailler certaines parties pour expliquer le fonctionnement.


![](https://i.imgur.com/8p2WyFb.png)

Cette première partie du code permet de définir les réglages de la carte, notamment sa taille ainsi que d'afficher un texte "My Google Maps Demo" dans la page HTML.

![](https://i.imgur.com/pyWpNFd.png)

Cette partie de code en javascript permet de régler les détails de la carte.
La variable "uluru" contient les coordonnées GPS. Elle sera changée de nom prochainement.
Ensuite les autres éléments permettent de préciser que l'on souhaite un pointeur sur les coordonnées GPS de "uluru", avec un zoom de 4 au départ et que l'on souhaite aussi que la carte soit centrée sur cette destination.

![](https://i.imgur.com/wl7TUqQ.png)

Enfin cette dernière ligne permet d'appeler la carte et c'est là que notre clé d'API Google entre en jeu. En effet, avoir une carte Google Maps est un service et donc il faut pouvoir identifier la personne, ou le site qui demande la carte. La clé d'API permet cette identication et permet de facturer le gestionnaire du site en fonction du nombre d'appel de l'API.

Dans ce lien, il faut remplacer le "YOUR_API_KEY" par celle que vous avez obtenue auparavant.
Une fois que tout est correct, le lien récupère la carte et l'affiche avec les réglages demandés avant.

On obtient une carte avec un pointeur vers la destination souhaitée, et elle est interactive.

![](https://i.imgur.com/kz2zO80.png)


## Récupération et affichage des données ThingSpeak

Dans l'exemple précédent, les coordonnées GPS sont fixes et uniquement modifiables directement dans le code. Ce que l'on souhaite faire maintenant, c'est récupérer les données sur ThingSpeak et les afficher.

Pour récupérer les données de ThingSpeak, je vais utiliser le framework Ajax qui permet de faire des requêtes sur des sites pour récupérer des données.

J'ai donc adapté le code à cette situation

![](https://i.imgur.com/70s1R9l.png)

J'ai dans un premier temps rajouter Ajax au site avec la commande "<script src"https://ajax.googleapis.com...."</script>".

Il faut ensuite rajouter la commande suivante qui permet de stocker les résultats de retour de la requête dans des variables. En effet, sans cette commande, je me suis aperçu que les variables dans lesquelles étaient stockées les données restaient NULL.

![](https://i.imgur.com/thy99tX.png)

Je suis ensuite passé à la récupération des données de ThingSpeak:

![](https://i.imgur.com/Vw4puZ9.png)

J'ai d'abord défini 2 variables x et y qui représentent la latitude et la longitude.

Dans un premier temps, j'ai récupéré la latitude grâce à cette commande:

![](https://i.imgur.com/7HiPxQ5.png)

Dans cette requête, il faut préciser 3 éléments:
* Le numéro de channel, ici 1042873
* Le champ à lire sur le channel, ici c'est le numéro 3 pour la latitude
* La clé d'API de lecture du channel

On peut retrouver l'ensemble des informations du channel sur celui-ci avec la clé d'API.
Enfin, on stocke la valeur récupérée dans la variable x qui correspond à la latitude.

Pour finir la récupération, on repète la même opération en récupérant le champ 4 du channel pour la longitude et on le stocke dans la variable y.

Il ne reste plus qu'à initialiser la map.
Dans la variable coord, on stocke la latitude et la longitude ( x et y). Enfin j'ai choisi un zoom de base égale à 10 pour avoir une bonne visualisation.

La fin du code ne diffère pas avec celui vu dans la première partie.

Une fois que tout est reglé correctement, on obtient le résultat suivant:

![](https://i.imgur.com/HBI5m5i.png)


Pour finir, on ne peut pas afficher cette carte directement sur le channel de ThingSpeak. La solution la plus simple est de faire héberger le micro-site Internet avec la carte sur un serveur en ligne et avoir un lien pour y accéder.
Une fois cet hébergement réalisé, il suffit simplement de se rendre dans les réglages du channel ThingSpeak et d'indiquer le lien vers le site hébergeant la carte:

![](https://i.imgur.com/uHBNDQp.png)

Ensuite, on peut accèder à la carte depuis l'interface principale du channel en cliquant sur le bouton "More Information".

![](https://i.imgur.com/SwROYe3.png)


#### Fin de la configuration
