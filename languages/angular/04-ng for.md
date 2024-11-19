# ng for

* [Créer une boucle ng for](#créer-une-boucle-ng-for)

## Créer une boucle ng for

Le ng for, écrit *ngFor, est une directive qui apporte des 'super pouvoirs' au Dom.

* Pour créer une boucle for, il faut commencer par créer la directive sous forme d'array d'objet dans le component, dans
 notre exemple de menus, on va créer un seul et unique tableau allRoutes dans la class du component :

```
export class Menu1Component implements OnInit {

  name: string;
  age = 12;

  allRoutes: MenuModel[] = [
    {url: 'home', name: 'Accueil', minimumAge: 0},
    {url: 'à-propos', name: 'A propos', minimumAge: 32},
  ];
```

* Ensuite dans le html du component on initialise notre boucle for de la façon suivante :

```
    <li *ngFor="let current of allRoutes">
      <a [routerLink]="current.url">
        {{current.name}}
      </a>
    </li>
```


On défini donc une variable current qui va itérer toutes les instances de toutes les routes, en prenant la valeur current :

> À noter : On décompose la valeur current en 2, la façon dont se nomme la route dans l'url (par exemple /a-propos) et 
> dans le lien cliquable (À propos)

* Il ne faut pas oublier de créer le modèle typescript dans le dossier models (dans notre exemple, ce sera menu.model.ts), dans
lequel on va configurer le model de nos menus :

```
    export interface MenuModel {
        url: string;
        name: string;
    }
```

On peut désormais ajouter des routes facilement (attention de bien les créer aussi dans app-routing.module.ts)
