# Administrer une base de données

> Pour créer une base de données voir les notes [firebase : créer une db](https://gitlab.com/olivier_portal/mes-cours/-/blob/master/tools/firebase/firebase.md#cr%C3%A9er-une-bdd-firebase)

* [Initialiser le lien avec la db](#initialiser-le-lien-avec-la-db)
* [Générer les credentials](#générer-les-credentials)
* [Activate cert](#activate-cert)
* [Get one doc](#activate-cert)
* [L'asynchronisation](#lasynchronisation)

## Initialiser le lien avec la db

* Pour initialiser le lien avec le cloud, comme nous travaillons en typescript et non en node.js, il faut importer firebase-admin :

![import firebase cloud](img/import%20firebaseAdmin.PNG)

En écrivant dans le fichier server.ts :

```typescript
import admin from 'firebase-admin';
```
* Il faut ensuite initialiser l'application admin (choisir la ligne de commande pour google) :

![admin init](img/admin%20init.PNG)

En copiant/collant dans le fichier server.ts la ligne de code :

```typescript
admin.initializeApp({
  credential: admin.credential.applicationDefault()
});
```
* Il faut alors créer la base de donnée en copiant/collant dans le fichier server.ts la ligne de code :

```typescript
const db = admin.firestore();
```

* Puis créer une variable qui stocke la référence de la collection de la base de données :

```typescript
const ref = db.collection('hostels');
```

## Générer les credentials

> Les credentials permettent grâce à une clé de services, à firebase que l'application est autorisée à accéder à la db

* Il faut se connecter à son projet dans firebase

* Aller dans les paramètres du projet et sélectionner l'onglet 'compte de service'

![paramètres firebase](img/param%20firebase.PNG)

* Cliquer sur Générer une nouvelle clé privée

> Attention !!! Comme le message l'indique, il ne faut jamais mettre la clé privée dans un repo public !!!
>
>![private key caution](img/private%20key%20caution.PNG)

* Créer un nouveau dossier 'cred' dans server et y copier le fichier json de la clé

* Créer un nouveau fichier typescript cert.ts

* On copie/colle le contenu du fichier json dans une variable que l'on exporte et remettre le code au format objet de ts :

```typescript
export const cert = {
                        type: "service_account",
                        project_id: "project-name-xxxxx",
                        private_key_id: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                        private_key: "-----BEGIN PRIVATE KEY-----\nxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                        client_email: "firebase-adminsxxxxxxxxx@project-name-xxxxx.iam.gserviceaccount.com",
                        client_id: "xxxxxxxxxxxxxxxxxxxx",
                        auth_uri: "https://xxxxxxxxxxxxxxxxxxxxxxxxx",
                        token_uri: "https://xxxxxxxxxxxxxxxxxxxx",
                        auth_provider_x509_cert_url: "https://www.xxxxxxxxxxxxxxxx",
                        client_x509_cert_url: "https://www.xxxxxxxxxxxx/firebase-adminsxxxxxxxxx@project-name-xxxxx.xxxxxxxx.com"
                    }
```

## Activate cert

> Il faut maintenant activer la variable cert

* Retourner dans les paramètres du projet à l'onglet 'compte de service' et copier/coller l'initialisateur de l'app admin
dans server.ts :

![paramètres firebase](img/param%20firebase.PNG)

* Remplacer les doubles quote par des singles quote (dans databaseURL) :

```typescript
admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  databaseURL: 'https://node-lab-d228c.firebaseio.com'
});
```

* Ouvrir serviceAccount (en rouge car non configuré pour l'instant) ===> Ctrl + clic sur cert, cela nous renvoie à l'objet
serviceAccount :

![service account](img/service%20acount.PNG)

* Aller à l'objet `admin.ServiceAccount` (Ctrl + clic) :

![Objet service account](img/objet%20serviceAccount.PNG)

* Importer ServiceAccount (Alt + Enter), choisir Add import statement et bien choisir 'from firebase-admin'

* Dans le fichier server.ts, reproduire le même format pour notre objet cert, en supprimant tout ce qui n'est pas nécessaire
et en remplaçant les noms des keys par les mêmes noms que dans l'objet ServiceAccount, ainsi qu'en typant l'objet cert
au type ServiceAccount et importer * Importer ServiceAccount (Alt + Enter), choisir Add import statement et bien choisir 'from firebase-admin' :

```typescript
import {ServiceAccount} from "firebase-admin";

export const cert: ServiceAccount = {
                        projectId: "project-name-xxxxx",
                        privateKey: "-----BEGIN PRIVATE KEY-----\nxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                        clientEmail: "firebase-adminsxxxxxxxxx@project-name-xxxxx.iam.gserviceaccount.com",
                    }
```

* Dans le fichier server.ts, remplacer l'objet serviceAccount par l'objet cert dans les credentials :

* Importer l'objet (Alt + Enter), bien choisir l'objet que l'on a créé (./cred/cert) :

```typescript
import {cert} from "./cred/cert";

admin.initializeApp({
    credential: admin.credential.cert(cert),
    databaseURL: "https://node-lab-d228c.firebaseio.com"
});
```

## Get one doc

> On peut désormais faire un crud au serveur

* Dans notre variable ref, on veut getter un (et un seul) hotel, il faut pour ça utiliser .doc() :

```typescript
const ref = db.collection('hostels').doc();
```

* Il faut alors récupérer l'UID (Uniq Id !== User Id sous Unix et Linux) dans notre db firebase :

![UID](img/UID.PNG)

* Puis la copier/coller dans doc() sous forme de string : 

```typescript
const ref = db.collection('hostels').doc('TWJoX9rV7tcrl8FrjG24');
```

On peut désormais faire un scrud sur un hôtel, mais afin de ne pas obtenir en réponse un objet complet avec des données
qui nous intéressent pas, il faut demander au serveur de nous renvoyer que les données qui nous intérressent dans l'objet
hostel, pour ça on utilise la fonction .data :

```typescript
app.get('/api', async (req, res) => {
    try {
        const hostel = await ref.get();
        return res.send(hostel.data());
    } catch (e){
        console.log(e.message);
        return res.status(500).send({error: 'erreur serveur', + e.message});
    }
});
```

* On obtient bien la réponse à notre requête du serveur :

![res.send](img/resSend.PNG)

## L'asynchronisation

> Avant de commencer à administrer la db et y faire du CRUD, il faut comprendre un principe important, l'asynchronisation

Par défaut le serveur node synchronise toues les tâches d'une requête, ce qui signifie qu'il lance, la tâche a, b et c
,par exemple, simultanément (ou quasi simultanément).

Maintenant que nous hébergeons la db online (sur firebase), le serveur doit d'abord aller chercher les infos dans la db
avant de pouvoir exécuter des requêtes sur cette base de données. Il faut donc désynchroniser les tâches dans le temps :

1. le serveur va chercher les informations sur la db
2. le serveur attend la réponse de la db
3. le serveur exécute les requêtes sur la db ainsi récupérer

**Cela s'appelle l'asynchronisation (ou l'asynchronicité)**

Afin d'asynchroniser les tâches on ajoute `async` devant la fonction de call back `(req, res)`

> Attention : dans la fonction il peut y avoir des tâches synchrones (par exemple un console.log('message')) et
> asynchrones (par exemple ref.get())

Il faut donc utiliser le mot clé `await` devant les tâches que l'on veut asynchroniser :

```typescript
app.get('/api/hostels', async (req, res) => {
    try {
        const hostels = await ref.get();
        return res.send(hostels)
    } catch (e) {
        return res.status(500).send({error: 'erreur serveur : ' + e.message});
    }
});
```

> À noter : get() est une promesse de QuerySnapshot de 't' (requête d'une image de la db à un instant t)
