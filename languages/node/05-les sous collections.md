# Les sous collections

* [Introduction](#introduction)
* [Poster un nouvel objet dans la sous-collection et le lier à la collection](#poster-un-nouvel-objet-dans-la-sous-collection-et-le-lier-à-la-collection)
* [Promise all](#promise-all)
* [Get sub collection](#get-sub-collection)
* [Query params](#query-params)
* [Query dataBase](#query-database)

## Introduction

Dans une base de données, il y a des collections, mais aussi des sous-collections.

Par exemple, une base de données peut
avoir une collection 'équipes de foot', avec des objets 'équipe de foot' qui ont plusieurs paramètres ('Couleur du maillot',
'nom de la ville', 'nom du club') mais aussi un objet 'joueurs'.

Afin de pouvoir bien gérer la base de données, il va falloir séparer l'objet 'joueurs' qui contient des paramètres
propres aux joueurs (âge, poste, rémunération...). Cela, d'une part évite d'avoir des collections avec objets trop complexes
à manipuler, mais d'autres part, un joueur peut passer d'une équipe à une autre (transfert).

Il faut donc créer une sous-collection et la lier à la collection principale grâce à des paramètres uid et ref (/teams/uid)


## Poster un nouvel objet dans la sous-collection et le lier à la collection

> À noter : Dans le fichier 'sousCollection.service.ts' il faut toujours commencer par lier l'initialisateur de l'app
> avec la variable `const db = admin.firestore();`
>
> Pour plus de clarté, on va utiliser une collection team et une sous-collection players

* Commencer par importer la sous collection players et la collection team dans le fichier players.service.ts :

```typescript
const teamsCollection: CollectionReference = db.collection('teams');
const playersCollection: CollectionReference = db.collection('players');
```

* Créer une fonction addNewPlayer afin d'ajouter un nouveau joueur (créer un nouvel Objet player) :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
  
}
```

Cela signifie :

J'exporte une fonction asynchrone (puisqu'elle va faire des requêtes au serveur), avec pour paramètres un identifiant
de joueur et un nouvel Objet player.

* Dans cette fonction, il faut commencer par s'assurer que pour toute requête, les 2 paramètres sont bien respectés :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
}
```

* Nous devons maintenant récupérer les références de la collection players :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }

    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();
}
```

Cela signifie :

Je récupère une image momentanée d'un player de la collection players en la gettant sur le serveur

* Il faut alors s'assurer que le player que l'on demande au serveur existe bien :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();

    if (!playersSnap.exists) {
            throw new Error('player does not exist');
        }
}
```

* Il faut alors créer une référence pour un nouveau player :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();
    if (!playersSnap.exists) {
            throw new Error('player does not exist');
        }

    const createdPlayerRef: DocumentReference = await playersCollection.add(newPlayer);
}
```

* Nous créons alors une référence pour ce nouveau player dans un objet team de la collection teams :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();
    if (!playersSnap.exists) {
            throw new Error('player does not exist');
        }
    const createdPlayerRef: DocumentReference = await playersCollection.add(newPlayer);

    const createdPlayerInTeamRef: DocumentReference = hostelRef
            .collection('rooms')
            .doc(createdRoomRef.id);
}
```

* Il faut alors envoyer l'Objet vers la base de données avec pour keys la référence de l'objet et son uid :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();
    if (!playersSnap.exists) {
            throw new Error('player does not exist');
        }
    const createdPlayerRef: DocumentReference = await playersCollection.add(newPlayer);
    const createdPlayerInTeamRef: DocumentReference = playerRef
            .collection('rooms')
            .doc(createdPlayerRef.id);

    await createdPlayerInTeamRef.set({ref: createdPlayerRef, uid: createdPlayerRef.id});
}
```

* La fonction crée donc bien un nouveau player dans la collection players et crée un lien dans l'objet team grâce à un objet
createdPlayerInTeamRef (qui a pour paramètres la référence de l'objet et son uid), il n'y a plus qu'à renvoyer 'ok' si la
fonction s'est bien exécutée :

```typescript
export async function addNewPlayer(playerId: string, newPlayer: PlayerModel): Promise<string> {
    if (!playerId || !newPlayer) {
            throw new Error('playerId or newPlayer are missing');
        }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playersSnap:DocumentSnapshot = await playerRef.get();
    if (!playersSnap.exists) {
            throw new Error('player does not exist');
        }
    const createdPlayerRef: DocumentReference = await playersCollection.add(newPlayer);
    const createdPlayerInTeamRef: DocumentReference = playerRef
            .collection('rooms')
            .doc(createdPlayerRef.id);
    await createdPlayerInTeamRef.set({ref: createdPlayerRef, uid: createdPlayerRef.id});
    
    return 'ok';
}
```

* Nous pouvons maintenant créer la requête (post) dans le serveur (fichier server.ts) :

```typescript
app.post('/api/teams/:teamId/players/', async (req, res) => {
    try {
        const teamId: string = req.params.teamId;
        const newPlayer: PlayerModel = req.body;

        const result: string = await addNewPlayer(teamId, newPlayer)

        return res.send(result);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

Cela signifie :

Je récupère l'id de la team dans laquelle je vais ajouter un joueur (que je configure dans le body de postman), je demande
le résultat au serveur grâce à ma fonction addNewPlayer et je retourne le résultat ou un message d'erreur

## Promise all

> L'opérateur Promise.all sert à récupérer toute une collection sous forme de tableau

* Pour utiliser l'opérateur Promise.all, il faut au préalable créer une fonction du type getCollectionObjectById :

Dans notre exemple :

```typescript
export async function getPlayerById(playerId: string) {
    if (!playerId) {
        throw new Error('playerId is missing');
    }
    const playerRef: DocumentReference = playersCollection.doc(playerId);
    const playerSnap:DocumentSnapshot = await playerRef.get();
    if (!playerSnap.exists) {
        throw new Error(`player ${playerId} does not exist`);
    }
    return playerSnap.data() as PlayerModel;
}
```

* On peut alors créer la fonction getAllPlayersByUids :

```typescript
export async function getAllPlayersByUids(playersUids: string[]) {
    const promisesToResolve: Promise<PlayerModel>[] = playersUids
        .map((uid) => getPlayerById(uid));
    return await Promise.all(promisesToResolve);
}
```

Cela signifie :

Je crée des promesses à résoudre que j'envoie au serveur, qu'il doit me retourner sous forme de tableau contenant toutes
les uids de tous les players

* Dans le fichier server.ts, on fait un get avec les ids des player :

```typescript
app.get('/api/test', async (req, res) => {
    try {
        const players = await getAllPlayersByUids([
            'SFmbIFp6OabNbrGHm03t',
            '2mf9TbcvUL2Ir5vAOpbw',
            'hAMhgB8RbmktGOeSU9Yq'
        ])
        return res.send(players);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

## Get sub collection

> Afin de récupérer une sous collection sous forme de tableau, il faut créer une fonction qui va getter la collection
> parente par id puis une fonction qui reprend la fonction de la sous collection (Promise.all)

* On crée donc d'abord la fonction :

```typescript
const refTeams = db.collection('teams');

export async function getTeamById(teamId: string): Promise<TeamModel> {
    if (!teamId) {
        throw new Error(`teamId required`);
    }
    const teamToFindRef = refTeams.doc(teamId);
    const teamToFindSnap: DocumentSnapshot = await teamToFindRef.get();
    if (!teamToFindSnap.exists) {
        throw new Error(`team ${teamId} does not exist`);
    } else {
        return teamToFindSnap.data() as TeamModel;
    }
}
```

* On peut alors créer la fonction :

```typescript
export async function getTeamByIdWithAllPlayer(teamId: string): Promise<TeamModel> {
    const teamToFind: TeamModel = await getTeamById(teamId);
    const playersCollectionInTeam: CollectionReference = refTeams
        .doc(teamId)
        .collection('players');

    const playersCollectionInTeamSnap: QuerySnapshot = await playersCollectionInTeam.get();
    const playersRef: string[] = [];
    playersCollectionInTeamSnap.forEach((playerRef) => playersRef.push(playerRef.data().uid));
    TeamToFind.roomsData = await getAllPlayersByUids(playersRef);

    return TeamToFind;
}
```

* Dans le fichier server.ts, on peut alors faire un get de la collection avec la sous collection dedans :

```typescript
app.get('/api/teams/:id', async (req, res) => {
    try {
        const teamId: string = req.params.id;
        const teamToFind: TeamModel = await getTeamByIdWithAllPlayer(teamId);

        return res.send(teamToFind);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

## Query params

> On peut vouloir récupérer une partie de la base de donnée (par exemple pour une bibliothèque, on peut vouloir récupérer
> les 20 premières pages d'un livre)

* On peut ajouter des paramètres supplémentaires à une requête (défini dans le protocole HTTP), grâce à un point
d'interrogation dans l'URL :

Dans notre exemple de livre (version simplifiée) :

On crée un get avec une limite

```typescript
app.get('/api/testquery', async (req, res) => {
    try {
        const limit = req.query.limit;
        return res.send('coucou ' + limit);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

* On entre alors l'URL : **localHost:3015/api/testquery?limit=20**

On récupère la valeur définie dans la variable limit

> Attention !!! la valeur du paramètre limit est un string

* Pour mettre plus d'un argument, il faut utiliser le symbole & :

On entre alors l'URL : **localHost:3015/api/testquery?limit=20&toto=true**

## Query dataBase

> Pour faire des requêtes directement liées à la base de donnée, il faut utiliser l'opérateur where

* On crée une fonction dans le fichier service.ts de sa collection :

```typescript
export async function getHostelByRoomNumbers(roomNumbers: number): Promise<HostelModel[]> {

    if (!roomNumbers) {
        throw new Error(`roomNumbers required`);
    }

    const data = await refHostels
        .where('roomNumbers', '==', roomNumbers)
        .get();
    const result: HostelModel[] = [];

    data.forEach(doc => result.push(doc.data() as HostelModel));

    return result;
}
```
 Cela signifie :
 
 Je fais une requête dans la collection hostels par roomNumbers. Si celui-ci n'est pas ou mal renseigné on renvoie le
 message d'erreur `roomNumbers required`, sinon on va chercher dans la db à la collection hostels tous les hostel qui
 ont un élément 'roomNumbers' qui est == à la valeur  de l'argument roomNumbers
 
 > On peut aussi utiliser les opérateurs '<, <=, >, >= et =='

* Dans server.ts, on fait une get :

```typescript
app.get('/api/testquery', async (req, res) => {
    try {
        const roomNumbers = parseInt(req.query.roomNumbers);
        const result = getHostelByRoomNumbers(roomNumbers);
        return res.send({result});
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

On récupère bien tous les hôtels qui ont un nombre de chambres égales à l'argument roomNumbers
(5 dans notre cas) :

![query data](img/query%20data.PNG)
