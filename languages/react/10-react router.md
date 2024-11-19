# React router

* [Installation de react router](#installation-de-react-router)
* [Importer des components](#importer-des-components)
* [Récupérer les paramètres dans un routeur](#récupérer-les-paramètres-dans-un-routeur)

## Installation de react router

> react router est une librairie supplémentaire de react que l'on trouve à l'adresse [react-rooter](https://reacttraining.com/react-router/web/guides/quick-start)

Pour installer react-router on copie colle `npm i npm --save react-router-dom`

## Importer des components

* Afin que l'App fonctionne correctement, il nous faut importer des components de la librairie react-router :

```javascript
import {
    BrowserRouter,
    Switch,
    Route,
    Link
} from "react-router-dom";
```

1. BrowserRouter est le main component, c'est lui qui prend le contrôle sur le browser et qui va gérer les routes. Quand
on click sur un link BrowserRouter va rediriger vers le lien (en catchant l'URL du link)

2. Switch est un switch, mais qui ne prend en compte que les routes. Il vérifie et affiche la bonne URL. Pour la page principale
si l'URL est différente de "/page-2" mais prend juste "/" il faut préciser que la route est exact : `<route exact path="/">Page1</route>`;

* On importe aussi les différentes pages crées (exemple) :

```javascript
import Page1 from "./components/Page1";
import Page2 from "./components/Page2";
import Page3 from "./components/Page3";
```

> Attention !!! ne jamais mélanger react-router et react redux !!!

## Récupérer les paramètres dans un routeur

On peut par exemple, vouloir récupérer l'id de l'utilisateur dans l'URL

* On importe les useParams de la librairie react-router-dom

* Introduire la variable id grâce à useParams dans la page souhaitée

* Dans la route du switch, ajouter le paramètre à prendre en compte

Dans la page 2, par exemple :

```javascript
import React from 'react';
import {useParams} from 'react-router-dom';

export default function Page2() {
    let {id} = useParams();
    return <h1>Page 2 : {id}</h1>
};
```

Dans App.js, au niveau du BrowserRouer et du Switch :

```javascript
<BrowserRouter>
    <div>
        <ul>
            <li>
                <link to="/">Page1</link>
            </li>
            <li>
                <link to="/page-2/coucou">Page2</link>
            </li>
            <li>
                <link to="/page-3">Page3</link>
            </li>
        </ul>

        <Switch>
            <Route exact path="/">
                <Page1/>
            </Route>
            <Route path="/page-2/:id">
                <Page2/>
            </Route>
            <Route path="/page-3">
                 <Page3/>
            </Route>
        </switch>
    </div>
</BrowserRouter>
```
