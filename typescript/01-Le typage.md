# Le typage

* [Typer les éléments en typescript](#typer-les-éléments-en-typescript)
* [La valeur retour d'une fonction](#la-valeur-retour-dune-fonction)
* [Faire un cast](#faire-un-cast)

## Typer les éléments en typescript

> typescript est un language qui permet de préciser de quel type sont les objets (string, number...)

* On commence toujours par créer un dossier models (au pluriel)

* Ensuite on crée des fichiers interfaces pour chaque objet, par exemple si l'on a des objets du type company et users :

```javascript
    const company = {
    name: '',
    address: '',
    size: '',
    users: [],
}

let users = [
    {
        id: 1,
        lastName: 'toto',
        age: 34,
    },
    {
        id: 2,
        lastName: 'toto1',
        age: 35,
    },
    {
        id: 3,
        lastName: 'toto2',
        age: 36,
    }
]
```

On va créer un fichier interface pour les objets users et l'objet company :

* **company.model.ts** et **user.model.ts**

> À noter, on type pour une company et un user, on ne met donc jamais de s aux noms de fichiers d'interfaces

* Dans chaque fichier, on commence toujours par exporter l'interface pour qu'elle soit accessible dans les autres fichiers
en typant chaque élément de l'objet, comme suit :

```typescript
export interface UserModel {
    id: number;
    lastName: string;
    age: number;
}
```

et :

```typescript
import {UserModel} from './user.model';

export interface CompanyModel {
    name: string;
    address: string;
    size: number;
    users: UserModel[];
}
```

> À noter, on met toujours une majuscule à une interface et on commence toujours par typer l'objet le plus en profondeur
>dans notre cas l'user.

* Il faut pour finir typer les fichiers typescript dans lesquels il y a les objets, en ajoutant ':' et le type :

```typescript
import {CompanyModel} from './company.model';
import {UserModel} from './user.model';

const company: CompanyModel = {
    name: '',
    address: '',
    size: '',
    users: [],
}

let users: UserModel[] = [
    {
        id: 1,
        lastName: 'toto',
        age: 34,
    },
    {
        id: 2,
        lastName: 'toto1',
        age: 35,
    },
    {
        id: 3,
        lastName: 'toto2',
        age: 36,
    }
]
```

## La valeur retour d'une fonction

> On peut typer la valeur retour d'une fonction

```typescript
function getUserById(id: number): UserModel | undefined{
  return users.find((user) => user.id === id);
}

console.log(getUserById(3));
```

On obtient bien l'user dont l'id est 3 :

![getUser](img/valeur%20retour.PNG)

> À noter, la valeur peut être non définie si l'user id n'existe pas il faut donc bien ajouter le type undefined avec
>un ou (une seule barre verticale e typescript)

## Faire un cast

> Faire un cast sert à forcer typesScript à comprendre que le type que l'on souhaite utiliser n'est pas celui défini
> dans la doc mais d'un autre type

Pour faire un cast on écrit :

```typescript
const hostelToDelete: HostelModel | undefined = snapHostelToDelete.data() as HostelModel | undefined;
```
