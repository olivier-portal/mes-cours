# Les serveurs

* [Un peu d'histoire](#un-peu-d'histoire)
* [Architecture client serveur](#architecture-client-serveur)
* [Les ports](#les-ports)

## Un peu d'histoire

Internet (contraction de Inter Network) est un réseau informatique à l'échelle du monde, reposant sur le protocole de
communication IP (Internet Protocol). Ce réseau rend accessible au public des services comme le courrier électronique et le web.
Internet et le World Wide Web, signifiant littéralement la Toile Mondiale sont parfois confondus mais, en fait, le Web
n’est qu’une des applications d'Internet, au même titre que le courrier électronique, la Messagerie instantanée ou les
systèmes de partage de fichiers. Par ailleurs, on distingue Internet (qui s’étend à l’échelle mondiale) de l'extranet,
réseau d'interconnexion avec les partenaires d'une entreprise et de l'intranet, qui est un réseau interne à une entreprise.
L'expression le net sert à désigner l'ensemble Internet, extranet et intranet.

Parmi les étapes importantes de la création d’Internet, on retiendra les dates suivantes :
- 1962 : début de la recherche par ARPA, un projet du ministère de la Défense américaine
- 1969 : connexion des premiers ordinateurs entre quatre universités américaines
- 1971 : 23 ordinateurs sont reliés sur ARPAnet.
- 1973 :l'Angleterre et la Norvège rejoignent le réseau Internet avec un Ordinateur par pays
- 1979 : création des NewsGroups (forums de discussion) par des étudiants américains
- 1982 : définition du protocole Tcp/ip (Transmission Control Protocol/ Internet Protocol) et du mot "Internet"
- 1983 : apparition du premier serveur de noms de sites
- 1990 : disparition d'ARPAnet
- 1991 : annonce publique du World Wide Web

Internet en tant que réseau informatique est composé d'une partie physique (les tuyaux qui relient les pays, les villes etc. entre eux) et d'une partie
immatérielle. Cette partie non-physique est constituée de protocoles, accords trouvés entre 2 parties pour envoyer et recevoir les données
(en language binaire, des 0 et des 1). Il existe de nombreux protocoles (git, http...).

**Le protocole HTTP a été inventé par Tim Berners-Lee, qui permet d'aller chercher des ressources sur internet**

À chaque fois que l'on va chercher des données, on fait une requête à un serveur que l'on appelle get.

## Architecture client serveur

> Le serveur détient la base de données

Les clients font des requêtes au serveur pour obtenir des données, le serveur renvoie les données requises:

![architecture client serveur](img/architecture%20client%20serveur.PNG)

## Les ports

> Un port est une ligne de connection que le serveur établi avec l'extérieur

* Un serveur possède 65 535 ports (dont certains sont réservés au système)

**En tant que développeur on travaille avec les ports compris entre 3000 et 6000**

Pour établir une connection, on choisit un port et on y met un node (un serveur nodejs), par exemple le port 3015

On utilise toujours un port unique pour chaque application, sinon pour la 2ème application on aura un message d'erreur "port non disponible"

* Pour faire marcher une application on utilise l'utilitaire webpack

* Pour changer de port, il suffit d'ouvrir le fichier webpack.config.js et modifier le numéro du port:

![modifier un port](img/modifier%20port.PNG)
