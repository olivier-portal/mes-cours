# La boucle for

* [Générer un tableau](#générer-un-tableau)
    
## Générer un tableau

> La boucle for permet de générer des tableaux

* La boucle for va répéter une ou plusieurs actions, un nombre de fois déterminer par les paramètres que l'on assigne à la boucle

> A noter, parmi les 3 paramètres, on a:
>
> Le 1er qui s'appelle l'initialisateur et dans lequel on indique généralement qu'on initialise la boucle à l'index 0
>
> Le 2éme est la condition de continuation, tant que la condition n'est pas remplie, on reste dans la boucle
>
> Le 3éme est le paramètre qui indique ce que l'on répète à chaque tour de boucle, en général on indique que l'on passe
> de l'index 0 à l'index 1 et ainsi de suite, que l'on écrit i = i + 1 ou i++

Par exemple, si l'on veut répéter 20 fois coucou!, on écrit:

```javascript
for (let i = 0; i < 20; i++) {
    console.log('coucou');
}
``` 

**Attention aux conditions de sorties d'une boucle afin de ne pas créer une boucle infinie!!!**
