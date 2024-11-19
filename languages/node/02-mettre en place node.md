# Mettre en place node

* [Installer node](#installer-node)
* [Exécuter plusieurs tâches simultanément](#exécuter-plusieurs-tâches-simultanément)
* [Créer et importer l'application express](#créer-et-importer-l'application-express)
* [Faire une requête au serveur](#faire-une-requête-au-serveur)
* [Ecouter le serveur](#ecouter-le-serveur)

## Installer node

* Pour installer node il faut aller dans le dossier server

Avant de lancer la commande d'installation `npm i`, il faut bien penser au préalable à changer de dossier en allant sur
le dossier server _(change directory => cd)_ `cd server`

Il faut ensuite installer les éléments configurer dans le fichier package.json avec la commande `npm install`

## Exécuter plusieurs tâches simultanément

> Afin de pouvoir exécuter plusieurs tâches simultanément, il faut installer npm concurrently

Pour installer npm concurrently il faut lancer la commande `npm install -g concurrently`

> Rappel: -g => global

* **La première fois, avant de lancer le serveur, il faut d'abord lancer watch**

Ensuite lancer le server

> Pour rappel: Pour afficher les scripts du serveur, faire un clic droit sur le fichier package.json puis sur Show npm Scripts

## Créer et importer l'application express

> Express est un outil donné par node pour lancer un serveur (express est donc le serveur node.js)

Pour créer et importer l'application express il faut coder:

```javascript
import express from 'express';

const app = express();
```

## Faire une requête au serveur

> Pour faire une requête, on utilise l'application express en appelant sa variable

Le verbe (c'est un mot clé qui détermine une action) pour faire une requête est **get**

On écrit:

```javascript
app.get('/api', (req, res) => {
    return res.send('Hello world!');
});
```
Cela signifie:

Je fais une **requête sur le serveur** (app.get) **avec pour URL '/api'**, j'ai **en paramètre ma requête** (req) et
**ma réponse** (res) **qui me retourne** (return) **un envoi du serveur de la réponse** à ma requête (res.send)

## Ecouter le serveur

> Pour écouter le serveur, on utilise l'application express en appelant sa variable

Le verbe pour écouter le serveur est **listen**

On écrit:

```javascript
app.listen(3015, function() {
    console.log('API listening on http://localhost:3015/api/ !');
});
```
Cela signifie:

**J'écoute le serveur** (app.listen), avec **en paramètre le port** (3015) et **une fonction** (function()) **qui m'écrit dans
la console du serveur** '_j'écoute sur l'URL http://localhost:3015/api/_' et permet donc à l'utilisateur d'avoir des infos
sur ce qui se passe dans le serveur

![listen serve](img/listen%20serve.PNG)

> A noter: Dans le serveur on code en TypeScript

