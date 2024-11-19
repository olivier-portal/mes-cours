# ng if and ng container

* [ngIf](#ngif)
* [ngContainer](#ngcontainer)

## ngIf

Le ngIf, écrit *ngIf, est une directive qui apporte des 'super pouvoirs' au Dom, qui permettent de mettre des conditions.

* Le ngIf permet de mettre des conditions suivant si l'on veut que certaines infos s'affichent ou non, prenons l'esxemple
d'un condition d'âge :

```
export class Menu1Component implements OnInit {

  name: string;
  age = 42; // ou 12

  allRoutes: MenuModel[] = [
    {url: 'home', name: 'Accueil', minimumAge: 0},
    {url: 'à-propos', name: 'A propos', minimumAge: 32},
  ];
```

* Dans le html du component on initialise notre condition de la façon suivante :

```
    <h2 *ngIf="age > 32">INFOS POUR LES VIEUX</h2>    

    <li *ngFor="let current of allRoutes">
      <a [routerLink]="current.url">
        {{current.name}}
      </a>
    </li>
```

Si dans la class l'âge est défini inférieur ou égale à 32 ans, le titre h2 ne s'affiche pas :

![ngIf not ok](img/ngIf%20not%20ok.PNG)

Si dans la class l'âge est défini supérieur à 32 ans, le titre h2 s'affiche :

![ngIf not ok](img/ngIf%20ok.PNG)

## ngContainer

Le ngContainer sert à cumuler différentes conditions et ou boucle

* Si l'on veut cumuler une condition d'âge à notre boucle for sur le menu, on va utiliser une balise qui ne sera pas prise
en compte par le html comme étant une div ou autre

* Pour cela il faut ajouter à notre objet dans la class du component une propriété ageMinimum (bien l'ajouter aussi au
MenuModel) :

```
export class Menu1Component implements OnInit {

  name: string;

  allRoutes: MenuModel[] = [
    {url: 'home', name: 'Accueil', minimumAge: 0},
    {url: 'à-propos', name: 'A propos', minimumAge: 32},
  ];
```

Dans le MenuModel :

```
    export interface MenuModel {
        url: string;
        name: string;
        minimumAge: number;
    }
```

* Puis dans le html, pour ne pas avoir une succession de ngXxx (_on ne peut avoir qu'une seule directive structurelle par balise_),
on va créer la balise ngContainer autour de notre balise li :

```
  <ng-container *ngFor="let current of allRoutes">
    <li *ngIf="age> current.minimumAge">
      <a [routerLink]="current.url">
        {{current.name}}
      </a>
    </li>
  </ng-container>
```

Cela permet ou non, en fonction de l'âge d'afficher le lien d'url ou non :

![ng container](img/ng%20container.PNG)
