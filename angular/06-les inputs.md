# Les inputs

* [Input](#input)
* [Les services](#les-services)

## Input

Les inputs servent à faire remonter les informations d'un component au component principal afin que tout le monde puisse y
avoir accès.

* On commence donc par couper/coller l'information que l'on veut sortir du component pour l'intégrer au component principal,
par exemple dans notre cas, on veut aue aalRoutes soit accessible à tous les components, on le coupe du component Menu1 et le colle
dans app.component.ts :

```
import { Component } from '@angular/core';
import {MenuModel} from "./models/menu.model";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'promo4Angular';

  allRoutes: MenuModel[] = [
    {url: 'home', name: 'Accueil', minimumAge: 0},
    {url: 'à-propos', name: 'A propos', minimumAge: 32},
  ];
}
```

* On ajoute alors dans le component Menu1 un input (qui est un décorateur) :

```
 export class Menu1Component implements OnInit {
 
   @Input() routes: MenuModel[];
 
   name: string;
   age = 12;
 
   constructor() { }
 
   ngOnInit(): void {
   }
 
   user() {
     return {age: 32, name: 'Joe'}
   }
 
 }   
```

* Enfin dans le html du component principal, on vient insérer dans la balise app-menu1 les routes :

```
<h1>Mon app</h1>
<app-menu1 [routes]="allRoutes"></app-menu1>
<router-outlet></router-outlet> 
```

* Idem pour l'âge :

```
// Dans le fichier app.component.ts=========================

import { Component } from '@angular/core';
import {MenuModel} from "./models/menu.model";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'promo4Angular';

  allRoutes: MenuModel[] = [
    {url: 'home', name: 'Accueil', minimumAge: 0},
    {url: 'à-propos', name: 'A propos', minimumAge: 32},
  ];

  age = 54;
}

// Dans le fichier menu1.component.ts=========================

 export class Menu1Component implements OnInit {
 
   @Input() routes: MenuModel[];
   @Input() age: number;
 
   name: string;
 
   constructor() { }
 
   ngOnInit(): void {
   }
 
   user() {
     return {age: 32, name: 'Joe'}
   }
 
 }  

// Dans le fichier app.component.html=========================

<h1>Mon app</h1>
<app-menu1 [routes]="allRoutes" [age]="age" ></app-menu1>
<router-outlet></router-outlet>
```

On récupère bien les informations dans le component souhaité. Cela s'appelle de la **délégation de compétences**

> À noter : Cette méthode pose ceci dit un problème car on se retrouve avec des data à l'intérieur d'un component. Ce n'est
> pas une bonne pratique !!! On va donc avoir recours aux services et injections de dépendances.

## Les services

Les services sont les fichiers dans lesquels on vient stocker toutes les données.

* Pour créer un service, on entre la ligne de code dans son terminal `ng g s services/mon-service`

Le service est un (décorateur) injectable. On va donc pouvoir stocker des informations dans services et les injecter là où
on en a besoin.

* Une fois le fichier menu.service.ts créer, on va à nouveau basculer les infos de allRoutes du fichier app.component.ts
vers le nouveau fichier menu.service.ts :

* On va alors dans le component principal créer un constructor, dans lequel on va injecter un service, et on va renommer allRoutes :

```
export class AppComponent {
  title = 'promo4Angular';

  allRoutes: MenuModel[] = this.menuService.allRoutes;

  age = 54;

  constructor(
    private menuService: MenuService
  ) {
  }

}
```

* Dans le fichier menu1.component.ts, on ajoute également le menu service et on initialise la route :

```
export class Menu1Component implements OnInit {

  @Input() age: number;

// Typage des routes=========================

  routes: MenuModel[];

  name: string;

// Injection du menuService=========================

  constructor(
    private menuService: MenuService,
  ) { }

// Initialisation des routes=========================

  ngOnInit(): void {
    this.routes = this.menuService.allRoutes;
  }

  user() {
    return {age: 32, name: 'Joe'}
  }

}
```

> Cela s'appelle l'injection de dépendance
