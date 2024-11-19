# Les classes

* [L'héritage des classes](#l'héritage-des-classes)
    * [Constructor](#constructor)
    * [Les méthodes](#les-méthodes)
    * [L'héritage](#l'héritage)

## L'héritage des classes

> L'héritage est la façon de classifier des concepts complexes, par exemple les mammifères sont une classe, dans lequel
> on trouve, les félins, les hominidés.... Qui eux-mêmes sont subdivisés (lion, chat, tigre...)

Les classes sont considérés comme des modèles, nous travaillons donc dans le fichier 'class.model.ts', on commence par 
exporter le modèle :

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    pool?: boolean;
    isOccupied?: boolean;
}
```


Une classe posséde un constructeur :

#### Constructor

**Un constructeur est la fonction qui est destinée à créer une instance d'une classe**.

Une instance étant la différence entre les différents objets d'une même classe (par exemple dans la classe être humains,
chaque être humain est différent de part sa taille, son poids, sa couleur des yeux, de peau, de cheveux....)

Pour travailler sur une instance, on utilise le mot clé 'this'.

On crée donc l'instance de la classe grâce au constructor (pour générer le constructer faire clic droit sur la class + generate + constructor) :

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    pool?: boolean;
    isOccupied?: boolean;

   constructor(
            id: number,
            name: string,
            roomName?: string,
            pool?: boolean,
            isOccupied?: boolean,
    ) {
            this.id = id;
            this.name = name;
            this.roomName = roomName;
            this.pool = pool;
            this.isOccupied = isOccupied;
    }
}
```

> À noter : Si des données sont non obligatoires on peut mettre des points d'interrogataion. Mais ils doivent être à la fin !

Dans son fichier sever.ts, on appelle l'instance de la classe grâce à une variable du type 'myClass' :

```typescript
app.get('/e', async (req, res) => {
    try {
        const myHostel = new HostelClass(12, 'Hôtel des fleurs');
        return res.send(myHostel);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

Pour gagner du temps et être plus productif, on peut écrire le constructeur plus simplement (cela permet aussi de ne pas
avoir plus de 4 arguments dans une fonction, qui est une règle de JS, certains systèmes refuseront de compiler si on met plus de 4 args) :

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    pool?: boolean;
    isOccupied?: boolean;

   constructor(hotel: HotelModel) {
        Object.assign(this, hotel);
    }
}
```

Dans le server.ts, il faudra juste réécrire l'HotelClass sous forme d'objet :

```typescript
app.get('/e', async (req, res) => {
    try {
        const myHostel = new HostelClass({id: 12, name: 'Hôtel des fleurs'});
        return res.send(myHostel);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

#### Les méthodes

> Les méthodes sont des fonctions qui permettent de calculer automatiquement des données, par exemple, si on ajoute ou
>supprime une chambre d'un hôtel, on veut que le paramètre roomNumbers se mette à jour automatiquement :

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    roomNumbers?: number;
    pool?: boolean;
    isOccupied?: boolean;
    rooms: RoomModel[];

   constructor(hotel: HotelModel) {
        Object.assign(this, hotel);
    }
    
    calculateRoomNumbers() {
        this.roomNumbers = this.rooms.length;
    }
}
```

Dans server.ts, on écrit :

```typescript
app.get('/e', async (req, res) => {
    try {
        const myHostel = new HostelClass({id: 12, name: 'Hôtel des fleurs', rooms: []});
        myHostel.calculateRoomNumbers();
        return res.send(myHostel);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

On obtient en retour du serveur une réponse ou les roomNumbers sont calculées automatiquement sans avoir eu besoin de les
appeler :

![Les méthodes des classes](../javascript/img/Les%20méthodes%20des%20classes.PNG)

On peut enchaîner les méthodes :

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    roomNumbers?: number;
    pool?: boolean;
    isOccupied?: boolean;
    rooms: RoomModel[];

   constructor(hotel: HotelModel) {
        Object.assign(this, hotel);
    }
    
    calculateRoomNumbers(): HotelClass {
        this.roomNumbers = this.rooms.length;
        return this;
    }
    
    destroyPool(): HotelClass {
        this.pool = false;
        return this;
    }

    createPool(): HotelClass {
        this.pool = false;
        return this;
    }
}
```

> À noter : Il faut retourner 'this afin de pouvoir enchaîner les instances dans le crud'

```typescript
app.get('/e', async (req, res) => {
    try {
        const myHostel = new HostelClass({id: 12, name: 'Hôtel des fleurs', rooms: []});
        myHostel
            .calculateRoomNumbers()
            .createPool();
        return res.send(myHostel);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

![Les méthodes des classes2](../javascript/img/Les%20méthodes%20des%20classes2.PNG)

#### L'héritage

> L'héritage permet de récupérer toutes les données d'une classe

Par exemple, si on veut créer une classe d'hôtels de luxes, o va récupérer tous les attributs des hôtels, et on va y
ajouter de nouveaux attributs propres aux hôtels de luxes :

> À noter : Quand on crée un constructeur chez un enfant, on utilise le mot clé 'super'

```typescript
export class HotelClass {
    id?: number;
    name?: string;
    roomName?: string;
    roomNumbers?: number;
    pool?: boolean;
    isOccupied?: boolean;
    rooms: RoomModel[];

   constructor(hotel: HotelModel) {
        Object.assign(this, hotel);
    }
    
    calculateRoomNumbers(): HotelClass {
        this.roomNumbers = this.rooms.length;
        return this;
    }
    
    destroyPool(): HotelClass {
        this.pool = false;
        return this;
    }

    createPool(): HotelClass {
        this.pool = false;
        return this;
    }
}

export interface LuxHotelModel extends HotelModel {
    numbersOfStars: number;
    numbersOfRoofTop: number;
}

export class LuxHotelClass extends HotelClass {
    numbersOfStars: number;
    numbersOfRoofTop: number;
    
    constructor(hotel: LuxHotelModel) {
        super(hotel);
        this.numbersOfStars = hotel.numbersOfStars || 1;
        this.numbersOfRoofTop = hotel.numbersOfRoofTop || 1;
    }
}
```

> À noter : On peut forcer un argument à avoir un minimum prérequis, par exemple si l'on veut que un hôtel de luxe aie au
> minimum une étoile, on écrit `this.numbersOfStars = hotel.numbersOfStars || 1`;

En appellant la class LuxHotelClass dans server.ts, on obtient bien les paramères de la classe parente plus ceux de la classe
enfant :

![héritage](../javascript/img/héritage.PNG)

On renseigne donc dans le serveur :

```typescript
app.get('/e', async (req, res) => {
    try {
        const myHostel = new LuxHostelClass({
            id: 12,
            name: 'Hôtel des fleurs',
            numbersOfRoofTops: 2,
            numbersOfStars: 4,
});
        return res.send(myHostel);
    } catch (e){
        return res.status(500).send({error: 'erreur serveur' + e.message});
    }
});
```

On peut aussi implémenter un parent, cela sert à forcer la classe d'avoir le même modèle :

```typescript
export interface RoomModel {
    roomName: string;
    size: number;
    id: number;
}

export class RoomClass implements RoomModel {
    roomName: string;
    size: number;
    id: number;

    constructor(id: number, roomName: string, size: number) {
        this.id = id;
        this.roomName = roomName;
        this.size = size;
    }
}
```
