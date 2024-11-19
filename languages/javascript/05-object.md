# Les object

* [Object destructuration](#object-destructuration)
* [Object attribute access](#object-attribute-access)
* [Object as string](#object-as-string)
* [Object keys and values](#object-keys-and-values)

## Object destructuration

> La destructuration d'un objet permet de récupérer les attributs d'un objet afin d'y accéder directement
> plus facilement

* Pour créer un objet déstructuré, on crée une variable d'un objet vide qu'on assigne à un autre objet:

```javascript
const user {
    firtsName: 'Seb',
    lastName: 'LeProf',
    age: 34,
    billionaire: false,
    eyes: 'blue',
}

const {} = user;
``` 

> Cela crée un objet destructuré qui n'a pour l'instant aucun attribut en relation avec l'objet initial

* Pour récupérer les attributs dont on a besoin, on écrit:

```javascript
const user {
    firtsName: 'Seb',
    lastName: 'LeProf',
    age: 34,
    billionaire: false,
    eyes: 'blue',
}

const {age, lastName} = user;

console.log(age, lastName);
``` 

* Ce qui donne:

![destruturation](img/object%20destructuration.PNG)

## Object attribute access

> Cette méthode sert à récupérer l'attribut d'un objet quand on ne sait pas quel est le nom de l'attribut à l'avance
>
>On l'utilise sur un site internet ou une application car l'utilisateur va faire des recherches par couleurs de produits, etc.

* Afin de récupérer des attributs que l'on ne connaît pas à l'avance, on crée un tableau puis on parcourt ce tableau:

```javascript
const user {
    firtsName: 'Seb',
    lastName: 'LeProf',
    age: 34,
    billionaire: false,
    eyes: 'blue',
}

const attrs = ['eyes'];

attrs.forEach(attr => console.log(user[attr]));
``` 

> Attention! l'attribut doit être renseigné dans le tableau sous forme de string uniquement

* On récuprère bien l'attribut:

![objec access](img/object%20access.PNG)

## Object as string

> Parfois au lieu d'avoir de traiter les données sous forme d'un array, on doit les traiter sous forme d'object

* A la place d'avoir des index, nous avons alors des attributs clés (dans notre exemple: hostel1, rooms):

```javascript
const hostels = {
  hostel1: {
    id: 1,
    name: 'hotel rose',
    roomNumbers: 10,
    pool: true,
    rooms: {
      {
        roomName: 'suite de luxe',
        size: 2,
        id: 1,
      },
      {
        roomName: 'suite nuptiale',
        size: 2,
        id: 2,
      },
      {
        roomName: 'suite familiale',
        size: 4,
        id: 3,
      },
    },
  },
};
``` 
## Object keys and values

> Dans la librairie javascript il y a des commandes qui permettent de transformer les object keys et values en array

* Pour transformer les keys des objets en tableau, il faut utiliser la commande:

```javascript
const keys = Object.keys(hostels);

console.log(keys);
``` 

* On récupère alors les keys des objets sous forme de tableau:

![objec keys](img/object%20keys.PNG)

* Idem pour les values on utilise la commande:

```javascript
const values = Object.values(hostels);

console.log(values);
``` 

* On récupère alors les values des objets sous forme de tableau:

![objec keys](img/object%20values.PNG)

* Pour les objets qui ont des objets imbriqués, on peut aussi les récupérer:

```javascript
const rooms = Object.values(hostels.hotel1.rooms);

console.log(rooms);
``` 

* On récupère alors les values des objets à l'intérieur d'autres objets sous forme de tableau:

![objec keys](img/object%20values%20profondeur.PNG)

* Ces commandes Object.keys et Object.values permmettent alors d'utiliser toute la puissance des opérateurs:

```javascript
const rooms = Object.values(hostels.hotel1.rooms);
console.log(rooms);

rooms.forEach((room) => console.log(room));
``` 

* On parcourt bien le tableau rooms grâce à l'opérateur forEach:

![objec keys](img/object%20values%20opérateurs.PNG)
