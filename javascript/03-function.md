# Les fonctions

* [Déclarer une function](#déclarer-une-function)
* [Déclarer une arrow function](#déclarer-une-arrow-function)
* [Appeler les fonctions](#appeler-les-fonctions)
* [Le rest parameter](#le-rest-parameter)
* [La récursivité d'une fonction](#la-récursivité-dune-fonction)

## Déclarer une function

* Pour déclarer une fonction il faut écrire le mot clé **function** suivi du nom que l'on souhaite lui donner et de parenthèses:

```javascript
function myFunction();
```

* Dans les parenthèses on entre **les paramètres** de la fonction:

```javascript
function myFunction(a, b) {

};
```
* Entre les accolades on écrit les instructions de la function:

```javascript
function myFunction(a, b) {
    return a + b;
};
```
## Déclarer une arrow function

> L'arrow function est une écriture abrégée des function

* Pour déclarer un arrow function, il faut d'abord déclarer une variable et y intégrer une **fonction** dîtes **anonyme** (on ne lui a pas assigné de nom)

```javascript
const myFunction = (a, b) => { return a + b };
```

> **l'arrow function est essentiellement utilisée dans les fonctions de Call Back**
>
> Attention!!! Une arrow function étant déclarée par une variable, elle doit nécessairement être déclarée avant d'âtre
> utilisée contrairement aux fonctions 'standards' qui peuvent être déclarées avant ou après être appelées.

* Dans une Call Back function (par exemple un opérateur filter), on a pas besoin de mettre les accolades, on peut donc l'écrire
de 2 façons:

```javascript
tab.filter(() => a + b);    // la façon la plus courte et donc la plus propre

// ou

tab.filter(() => {
    return a + b;
})
```

## Appeler les fonctions

* Pour appeler les function ou arrow function, il suffit d'écrire le nom de la function en y ajout des valeurs aux paramètres:

```javascript
myFunction(2, 3);
```

## Le rest parameter

> Le rest parameter permet de déclarer une fonction avec autant d'arguments que l'on veut sans en savoir le nombre à l'avance

* Le rest parameter, comme son nom l'indique doit être le dernier argument à être utilisé (c'est celui qui reste).
Il peut aussi être l'unique argument

```javascript
function add(...args) {
    console.log(args);
}

add(2, 3, 4, 5);
```

On obtient les paramètres sous forme de tableau:

![rest parameter](img/rest%20parameter.PNG)

On peut donc gérer les arguments avec les opérateurs des arrays. Pour une addition, par exemple, on écrit :

```javascript
function add(...args) {
    console.log(args);
    args.reduce((acc, value) => acc + value, 0);
}

console.log(add(2, 3, 4, 5));
```

On obtient bien le résultat de l'addition des arguments 2, 3, 4, 5 qui donne 14 :

![rest parameter](img/rest%20parameter2.PNG)

Avec plusieurs arguments, le principe est le même :

```javascript
function add(message, ...args) {
    const result = args.reduce((acc, value) => acc + value, 0);
    console.log(message, args);
}

add('Le résultat est : ', 2, 3, 4, 5);
```

![rest parameter](img/rest%20parameter3.PNG)

## La récursivité d'une fonction

> La récursivité d'une fonction est quand on appelle une fonction à l'intérieur de cette même fonction

* Quand on appelle une fonction de manière récursive (l'appeller à l'intérieur d'elle-même), la fonction va permettre de soit se terminer, soit s'auto-appeler à nouveau.

Par exemple générer un nombre aléatoire unique (il ne doit jamais y avoir 2 fois le même nombre):

```javascript
const allReadyUsedNumbers = [];

function randomIntFromInterval(min, max) {
  return Math.floor(Math.random() * (max - min + 1 ) + min);
}

function generateUid(min, max) {
    const temp = randomIntFromInterval(min, max);
    if (allReadyUsedNumbers.length === max) {
        return;
    }
    if (allReadyUsedNumbers.find((value)  => value === temp)) {
        return generateUid(min, max);
    } else {
        allReadyUsedNumbers.push(temp);
        return temp;
    }
}

console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
console.log(generateUid(1, 10));
```

* Le résultat est bien un nombre aléatoire qui se génère et **SI** il se génère une 2ème fois, on régénère un nouveau nombre aléatoire
et **SI** on a utilisé tous les nombres disponibles, on sort de la fonction:

![récursivité](img/récursivité.PNG)

* **Atention !!! Il ne faut pas oublier de mettre des conditions de sortie afin de ne pas avoir de boucle infinie
ou d'avoir un message d'erreur stack overflow**
