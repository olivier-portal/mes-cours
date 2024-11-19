# Use effect

* [Gérer le monde extérieur](#gérer-le-monde-extérieur)
* [Faire un get](#faire-un-get)
* [Tableau de dépendance](#tableau-de-dépendance)

## Gérer le monde extérieur

> Afin de gérer des données externes (await, async), il existe des db en ligne qui permettent de travailler sans avoir une db
> encore réalisée. la plus utilisée est [jsonplaceholder](https://jsonplaceholder.typicode.com/). Nous utiliserons les users.

Pour faire des get en React, il faut utiliser une librairie (il y en a plusieurs), nous utiliserons axios. Il faut donc faire un lien
vers axios plus créer une variable api qui renvoie à la db de jsonplaceholder

```angular2html
<script crossorigin src="https://unpkg.com/react@16.13.1/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16.13.1/umd/react-dom.development.js"></script>

// Ajout de la librairie axios==========================

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

// ======================================================================

<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<script type="text/babel">

// Ajout d'une variable api qui renvoie vers la db=======================

  const api = 'https://jsonplaceholder.typicode.com/users/';

// =======================================================================

</script>
```

Pour récupérer des données sur les users, il faut créer une fonction du type DisplayUser que l'on appellera dans App :

```angular2html
<script crossorigin src="https://unpkg.com/react@16.13.1/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16.13.1/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">

  const api = 'https://jsonplaceholder.typicode.com/users/';

  function DisplayUser(props) {
    const {uid} = props;
    console.log(api + uid);
  }

  function App() {
    return <>
        <DisplayUser uid = {2}/>
    </>
  }

</script>
```

On récupère bien l'api de l'user à l'uid 2 :

![api et uid](img/api%20et%20uid.PNG)

Afin de gagner quelques lignes de codes, on peut déstructurer l'uid directement dans les paramètres de la fonction :

```angular2html
<script type="text/babel">

  const api = 'https://jsonplaceholder.typicode.com/users/';

  function DisplayUser({uid}) {
    console.log(api + uid);
  }

  function App() {
    return <>
        <DisplayUser uid = {2}/>
    </>
  }

</script>
```

## Faire un get

> Pour faire un get en react on va utiliser la librairie axios, le get sera de type : axios.get

Le get se fait en plusieurs étapes :

1. A l'intérieur du component DisplayUser, on crée nos états que l'on nommera user et setUser (à la place de state et setState)

2. On crée ensuite une fonction asynchrone avec en paramètre l'uid de l'user à getter grâce à React.useEffect qui prend
 en paramètre une fonction de callback et un tableau

3. On prévoit des sécurités dans la fonction au cas où l'uid n'existe pas ou l'user est déjà appelé

4. On fait un get de l'api + uid avec un await que l'on stocke dans une variable temporaire

5. On appelle la fonction ainsi crée

6. On retourne une balise (par exemple une div) user sous forme de string (JSON.stringify)

7. On retourne la fonction DisplayUser avec l'uid souhaitée dans le component App

Cela donne en code :

```angular2html
<script type="text/babel">

  const api = 'https://jsonplaceholder.typicode.com/users/';

  React.useEffect(() => {
        function DisplayUser({uid}) {
        
            const [user, setUser] = React.useState(null);
            async function getUserbyUid(uid) {
              if (!uid) {
                return;
              }
              if (user) {
                return;
              }
            const temp = await axios.get(api + uid);
            setUser(temp);
            }
        
            getUserbyUid(uid);
    }, [user]);

    return <div>{JSON.stringify(user)}</div>
  }

  function App() {
    return <>
        <DisplayUser uid = {2}/>
    </>
  }

</script>
```

On récupère bien notre user avec l'uid 2 :

![get user 2](img/get%20user%202.PNG)

Dans l'onglet network, on récupère bien les données demandées au serveur :

![get user network](img/get%20user%20network.PNG)

## Tableau de dépendance

> Quand on utilise React.useEffect, il y a à la fin un tableau de dépendance (,[]). 

Si l'on met une variable dans le tableau de dépendance, cela indique à useEffect que si la variable change il faut retrigger
(relancer) le useEffect à chaque fois que l'on fait un setState sur cette variable.

Dans notre exemple ci-dessus, dans useEffect on a un setUser, qui fait un get sur l'user. Si l'on ne prévoit pas le cas où
l'user déjà appeler ne doit pas être appelé à nouveau et que l'on passe la variable user dans le tableau de dépendance,
le component va appeler à l'infini l'user. On entre donc dans une boucle infinie (Attention !)

* À noter : useEffect, useState sont des hooks. c'est de la programmation fonctionnelle.

Une bonne pratique est d'éviter d'utiliser des if mais ajouter des fonctions, par exemple un button reload, dans lequel 
on passe une variable reload et setReload ainsi qu'une fonction du type handleClick :


```angular2html
<script type="text/babel">

  const api = 'http://localhost:3015/api/hostels/';

  function State({state}) {
    return <pre>{state.message}</pre>;
  }

  function List({setState, reload}) {
    const [hotels, setHotels] = React.useState([]);
    React.useEffect(() => {
      async function getHotels() {
        console.log('USE EFFECT');
        try {
          const hotelsToGet = await axios.get(api);
          setHotels(hotelsToGet.data);
          setState({message: 'ok'});
        } catch (e) {
          setState({message: 'error'});
        }
      }
      getHotels();
    }, [reload]);

    return hotels.map((hotel, index) => <li key={index}>{hotel.name} {hotel.roomNumbers}</li>);
  }

  function App() {
    const [state, setState] = React.useState({message: 'initializing'});
    const [reload, setReload] = React.useState(false);
    function handleClick() {
      setReload(!reload)
    }
    return <>
      <ul>
        <List setState={setState} state={state} reload={reload}/>
      </ul>
      <State state={state}/>
      <button onClick={handleClick}>reload</button>
    </>;
  }
  ReactDOM.render(<App/>, document.getElementById('root'));
</script>
```

À noter : On utilise l'état de reload en passant un boolean que l'on set à l'inverse de reload, cela permet de passer de
false à true, puis de true à false.... Le reload ne s'exécute donc qu'une seule fois.
