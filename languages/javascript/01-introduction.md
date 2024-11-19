# Welcome to JavaScript

* [Un peu d'histoire](#un-peu-dhistoire)
* [Les notions de base de JavaScript](#les-notions-de-base-de-javascript)
    * [Les variables](#les-variables)
    * [Les valeurs primitives](#les-valeurs-primitives)
    * [Les fonctions](#les-fonctions)
    * [Arrow](#arrow)
    * [Les objets](#les-objets)
* [Le template literal](#le-template-literal)

## Un peu d'histoire

Le JavaScript est un langage de programmation dynamique qui fut créé en 1995 par Brendan Eich, co-fondateur du projet Mozilla, de la Mozilla Foundation et de la Mozilla Corporation.
Pour info, avant de s'appeler JavaScript, ce langage s'appelait LiveScript.
Pour d'avantage d'historique à ce sujet, vous pouvez aller sur Wikipedia et bien sur Mozilla Developper Network.
Aujourd'hui, de nombreux projets utilisent le langage ECMAScript ( l'autre nom de JS):

* Node.js
* React (JS, Native...)
* Angular ( jusqu'à la 2 puis TypeScript)
* TypeScript
* Babylon.JS
* Vue.JS


## Les notions de base de JavaScript

#### Les variables

> Il s'agit de boites dans lesquelles on peut stocker des valeurs. Pour commencer, il faut déclarer une variable avec le mot-clé let ou const en le faisant suivre de son nom :

const est utilisé si la variable n'est jamais réaffectée, let si elle est réaffectée

`const box = 'Hey you';`

#### Les valeurs primitives

* Les strings : Il s'agit d'une chaîne de caractères.
* Les numbers: Il s'agit des nombres
* Les booleans : Il s'agit de booléens True et False.
* Les undefined : Il s'agit d'une valeur indéfinie.
* Les Nuls : Il s'agit de la valeur nulle


#### Les fonctions

C'est un moyen de compacter des fonctionnalités en vue de leur réutilisation. Quand vous avez besoin de la procédure, vous pouvez appeler une fonction, par son nom, au lieu de ré‑écrire la totalité du code chaque fois.

```javascript
const pen = {
    brand: 'Bic',
    color: 'Black',
    };

function pen() {
    return pen.map((pen) => pen.color)
}
console.log(pen);
```

#### Arrow

C'est une version abrégée et simplifiée de la fonction d'où l'appellation fonction fléchée.

Contrairement à une fonction, il ne peut être utilisé pour déclarer des méthodes.

```javascript
const color = pen.map(pen => pen.color);

console.log(color);
```

#### Les objets

Il s'agit d'une collection de données apparentées et/ou de fonctionnalités (qui, souvent, se composent de plusieurs variables et fonctions, appelées propriétés et méthodes quand elles sont dans des objets).

```javascript
const you = {
    name: 'Baby Shark',
    Age: 1,
    Sexe: 'Undefined',
};
```

## Le template literal

> le template literal est une façon de concaténer des strings avec des variables de façon plus simple, on a plus besoin
> d'échapper les ' et les "

Un template literal simplifie l'écriture (avec les backtick) :

* Sans template literal

```javascript
console.log('L\'hôtel avec l\'id ' + hostelId + ' n\'existe pas');
```

* Avec template literal

```javascript
console.log(`L'hôtel avec l'id ${hostelId} n'existe pas`);
```
