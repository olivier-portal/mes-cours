# Create react app

* [Installation de react](#installation-de-react)
* [Paramétrer son App](#paramétrer-son-app)
* [Créer des components](#créer-des-components)

## Installation de react

> l'outil Create react app a été crée par facebook, on le trouve sur le [github de facebook](https://github.com/facebook/create-react-app)

Afin d'installer react, on utilise npx et non npm i (cela n'installe pas react mais on crée une application avec react)

Pour créer l'application react, on ouvre sa console et on copie colle `npx create-react-app my-app` et renommer my-app par le nom souhaité

Une fois l'installation terminée on se crée un projet sur gitlab (ou github...) et on va linker les 2.

Pour cela retourner dans la console et aller dans l'app crée `cd create-react-app-olivier` (le nom est à titre d'exemple)

Puis on fait `npm start`, cela ouvre le localhost:3000 comme suit :

![create react app](img/create%20react%20app.PNG)

Il faut maintenant le lier à son projet gitlab, on va dans vcs => git => push , (ou Ctrl + Maj + k), cela ouvre une fenêtre :

![react git push](img/react%20app%20git%20push.PNG)

il suffit alors de copier/coller l'url de son projet dans master origin et de le pusher, l'app est bien liée à son projet
gitlab :

![react app créée](img/react%20app%20créée.PNG)

À noter : On utilise plus les fichiers jsx mais js depuis 2019. React évolue constamment, toujours se tenir au courant des derniers changements !!!

Pour aller sur le site offiel de react, il faut clicker sur le user guide de react et aller sur get started

De là on peut très facilement installer des frameworks ou librairies, par exemple bootstrap, on va sur l'onglet Building your app
 ,puis adding bootstrap, il suffit de suivre les instructions et copier coller le code dans sa console
 
## Paramétrer son App

* Attention !!! on ne touche jamais au fichier index.js (il est essentiel au bon fonctionnement de l'App)

On commence par faire le ménage dans l'app (on enlève tout ce qui nous sert à rien, le logo react, le css prédéfinie etc.)

On peut alors commencer à coder dans App.js en ajoutant des balises avec leurs className, exemple :

```javascript
import React from 'react';
import 'bootstrap/dist/css/bootstrap.css';


function App() {
  return (
    <div className="container-fluid">
      <header>
        <button className="btn btn-danger">bouton danger</button>
        <h1>Coucou</h1>
      </header>
    </div>
  );
}

export default App;
```

On obtient bien la mise en page classique de bootstrap :

![react bootstrap](img/react%20bootstrap.PNG)

## Créer des components

* Avant de créer des components on crée un dossier components (à l'intérieur du dossier src) dans lequel on va mettre tous les components

Attention !!! Une page entière n'est pas un component, un component est un header ou n'importe quelle feature qui reviens
régulièrement

On ajoute alors un dossier par component, par exemple Header, dans lequel on vient créer un fichier Header.js et un autre
Header.css

> Avec react, on met toujours une majuscule aux noms de fichiers (sauf pour les fichiers natifs du type index.js...)

* Dans le component Header.js, on commence par importer Header.css

* Les nouvelles pratiques React sont d'utiliser des functions (et non plus des class comme auparavant)

* Pour créer un formulaire de recherche, il faut :

1. Créer un component MyForm (dans un dossier du même nom) dans lequel on vient écrire la function propre au formulaire

2. On crée un autre component DisplayUser (dans un dossier du même nom) dans lequel on vient écrire la function qui va getter
l'user

3. On ajoute la variable d'état dans App.js

4. On retourne les components dans App.js

-----------

* Dans MyForm.js :

```angular2html
import React from 'react';
import axios from 'axios';

const api = 'https://jsonplaceholder.typicode.com/users';

export default function MyForm({handleUser}) {
    const [uid, setUid] = React.useState('');
    
    React.useEffect(() => {
        async function getUserByUid(uid) {
            if (!uid) {
                handleUser({user: null});
                return;
            }
            handleUser('...loading');
            const newUser = await axios.get(api + uid);
            handleUser(newUser);
        }
        getUserByUid(uid);
    }, [uid]);

    function handleInput(event) {
        const {value} = event.target;
        setUid(value);
    }

    return <form>
        <label htmlFor="uid">Uid</label>
        <input
            value={uid}
            onChange={handleInput}
            type="text" id="uid"/>
    </form>;
}
```

* Dans DisplayUser.js :

```angular2html
import React from 'react';

export default function DisplayUser({user}) {
    return <div>{JSON.stringify(user)}</div>;
}
```

* Dans App.js :

```angular2html
import React from 'react';
import 'bootstrap/dist/css/bootstrap.css';
import Header from "./components/Header/Header";
import MyForm from "./components/MyForm/MyForm";
import DisplayUser from "./components/DisplayUser/DisplayUser";


function App() {

    const [user, setUser] = React.useState({user: null});

    return (
        <div className="container-fluid main">
            <Header/>
        <MyForm handleUser={setUser}/>
        <DisplayUser user={user}/>
        </div>
)
    ;
}

export default App;
```
