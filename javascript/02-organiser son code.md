# Organiser son code en js

> **Il est primordiale de bien organiser son code !!!**

* [Créer des fichiers séparés](#créer-des-fichiers-séparés)
* [Créer des fonctions avec Firebase](#créer-des-fonctions-avec-firebase)

## Créer des fichiers séparés

* Créer un fichier pour les données appeler user.data.js

* Créer un fichier où utiliser les données (dans notre cas playground.js)

* Exporter les données du fichier user.data.js avec le mot clé export, comme suis:

```javascript
const me = {
    firstName: 'Olivier',
    lastName: 'Portal',
    age: 39,
};

const user1 = {
    firstName: 'Joe',
    lastName: 'Lachance',
    age: 49,
};

const user2 = {
    firstName: 'Bobbye',
    lastName: 'La Teigne',
    age: 23,
};

export const tab = [me, user1, user2];
```

* Importer les données dans le fichier playground.js:

```javascript
import {tab} from './user.data';

console.log(tab);
```

## Créer des fonctions avec Firebase

* On commence par créer 1 dossier principal dans son app, appelé backend

* Avec le terminal, on se met dans le bon répertoire `cd backend/`

* On crée un nouveau dossier dans backend, avec un nom qui va correspondre à la fonction par exemple : onUserCreated

* Avec le terminal, on se met dans le bon répertoire `cd onUserCreated/`

* Puis on initialise les functions firebase avec la ligne de commande `firebase init functions`

* Choisir le projet existant (préalablement créer dans Firebase)

* Utiliser typescript (et non javascript)

* installer les tslint et les dependencies (npm i)

* On obtient l'arborescence complète pour travailler correctement :

![firebase functions](img/firebase%20function.PNG)

* On décommente la fonction helloWorld que l'on renomme onUserCreated

* On ouvre le package.json et on indique que l'on veut qu'au start et au deploy que seulement cette fonction soit exécutée :

`"deploy":onUserCreated: "firebase deploy --only functions:onUserCreated",`

![firebase function exe](img/firebase%20function%20exe.PNG)

* On peut alors commencer à travailler sur sa fonction

> Attention ! Un dossier pour chaque fonction !!!

* On peut alors deploy notre fonction sur firebase avec `firebase init`

* On choisit comme emplacement du public directory `functions/lib`

* On choisit aussi les 3 options : functions, firestore et hosting

![emulator setting](img/emulator%20setting.PNG)

* On peut alors vérifier que sa fonction marche bien avec l'émulateur (ne pas oublié de rappeler le lieu de connexion au serveur et la mémoire
allouée, toujours choisir la plus basse en fonction de la taille de la function) : 

```javascript
import * as functions from 'firebase-functions';

export const onUserCreated = functions
    .region('europe-west3')
    .runWith({memory: '128MB'})
    .https
    .onRequest((request, response) => {
        response.send('Hello World');
    });
```
