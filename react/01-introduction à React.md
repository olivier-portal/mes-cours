# Introduction à React

* [Qu'est-ce que React ?](#quest-ce-que-react-)
* [Créer un élément](#créer-un-élément)

## Qu'est-ce que React ?

> React est une librairie qui permet de gérer un virtual Dom

Pour utiliser React il faut importer les liens des librairies react-development.js et react-dom-development.js (à la fin du fichier html) :

```html
<script crossorigin src="https://unpkg.com/react@16.13.1/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16.13.1/umd/react-dom.development.js"></script>
```

On peut alors commencer à coder en React entre deux balises `<script></script>`

## Créer un élément

Pour créer un élément dans React, on va chercher une balise HTML par son id, avant de créer l'élément. On peut stocker
des variables (on écrit en js) que l'on réutilisera dans l'élément, exemple :

```html
// div que l'on souhaite modifier dans le HTML

<div id="root"></div>

// React

<script>
const value = 'React';
const version = React.version;

// Je récupère la div par son id dans le document présent

const root = document.getElementById('root');

// Je crée un élément dans cette div

const exampleDiv = React.createElement('div', {
    className: 'main',
    children: `Bienvenue dans l'initiation à ${value} (version ${version})`,
});

// J'affiche le résultat

ReactDOM.render(exampleDiv, root);
</script>
```

On obtient alors :

![React élément](img/react%20element.PNG)
