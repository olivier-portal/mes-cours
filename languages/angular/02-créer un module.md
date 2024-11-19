# Créer un module

* [Les modules](#les-modules)
* [Les modules de type page](#les-modules-de-type-page)
* [Les modules de type component](#les-modules-de-type-component)

## Les modules

Avec Angular il existe 2 types de modules :

1. les modules de type page
2. les modules de type component

## Les modules de type page

Les modules de type pages doivent s'appeler de la même manière que la page, par exemple pour la home page, le module s'appellera
home, pour une page about, le module prendra le nom about etc.

Dans un module de type page, il va y avoir tout le html et css, ainsi que tous les components rattachés à ce module.

Pour créer un module, on va dans le terminal de son IDE et on entre la ligne de code :

```
ng generate module nomDeLaPAge --route=nomDeLaPAge --module=app-routing
```

On peut raccourcir cette ligne de commande avce les initiales g et m pour generate et module :

```
ng g m nomDeLaPAge --route=nomDeLaPAge --module=app-routing
```

Cela signifie que l'on crée une nouvelle page avec une route de type /nomDeLaPAge et on va lier cette page à app-routing-module.ts :
 
![route new page](img/route%20page.PNG)

> À noter : Par défaut une page principale est déjà présente lorsque l'on crée un nouveau projet angular, il faut supprimer tout
> le html (sauf la balise `<router-outlet></router-outlet>`, qui va attacher les routes nécessaires)

La page ainsi créer, Angular ajoute automatiquement un décorateur dans le fichier nomDeLaPAge-module.ts grâce aux métadonnées
@NgModule. Ce décorateur va indiquer à Angular comment va fonctionner la page (import d'autres modules, les déclarations...)

#### Créer un lien

Pour créer un lien dans Angular, on procède de la même manière qu'en html, liste à puce (\<ul>) ou liste ordonnées (\<ol>)
+ éléments de liste (\<li>) + ancre (\<a>)

La seule différence est que dans la balise d'ancrage, on n'utilise pas href mais routerLink :

```angular2html
<ul>
    <li>
        <a routerLink="home">home</a>
    </li>
</ul>
```

## Les modules de type component

* Pour créer un module de type component, on ouvre son terminal et on écrit la ligne de code :

```
ng g m /components/nom-de-mon-module
```

> À noter : En Angular, on utilise pas le camelCasing mais le kebab-casing

Cela crée un dossier components avec un sous-dossier du nom du module contenant un seul ficher du même nom :

![component module](img/component%20module.PNG)

* Le fichier main-menu.modules.ts (dans notre exemple) va alors exporter le module main-menu, dans lequel on va ajouter plusieurs
components qui vont gérer plusieurs fonctions. Il faut alors créer les nouveaux components (par exemple, si on a 2 menus différents),
on entre la ligne de code dans son terminal :

```
ng g c /components/main-menu/menu1
```

> la lettre 'c' signifie component

Cela crée tous les fichiers nécessaires pour le component menu1 et met à jour le fichier main-menu.modules.ts en y insérant
la déclaration du menu1 :

![components](img/sous%20modules%20components.PNG)

* Une fois les components créer, il faut exporter les components sur lesquels on autorise d'autres devs à avoir accés et 
travailler dessus. Par exemple, dans notre cas avec 2 menus, si ces 2 components ont en commun un autre component (menu-logo
par exemple), on veut autoriser l'équipe à travailler sur les menus 1 et 2 mais interdire l'accès au component menu-logo
on écrit :

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { Menu1Component } from './menu1/menu1.component';
import { Menu2Component } from './menu2/menu2.component';
import { MenuLogoComponent } from './menu-logo/menu-logo.component';
import {RouterModule} from "@angular/router";



@NgModule({
  declarations: [
    Menu1Component,
    Menu2Component,
    MenuLogoComponent,
  ],
  exports: [
    Menu1Component,
    Menu2Component,
  ],
  imports: [
    CommonModule,
    RouterModule,
  ]
})
export class MainMenuModule { }
```

On exporte bien uniquement les components Menu1Component et Menu2Component mais pas MenuLogoComponent (qui est seulement
déclaré)

* Il faut alors intégrer le MenuLogoComponent dans les 2 autres components de menus. On intègre la balise app-menu-logo
 dans les fichiers html des menus 1 et 2 :

```
<app-menu-logo></app-menu-logo>
```

Comme les 3 components sont déclarés ensemble dans main-menu.modules.ts, la balise est reconnue par Angular. On  ne peut pas s'en
servir ailleurs.

Attention !!! Ne pas oublier d'importer le RouterModule !!!

* Il faut alors importer dans le fichier app-module.ts (le module principal), le component main-menu :

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import {MainMenuModule} from "./components/main-menu/main-menu.module";

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    MainMenuModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

L'import du component dans pp-module.ts est proposé automatiquement lorsque l'on importe le component menu1 (ou 2) dans le
component principal app-component.ts en y écrivant la balise ``<app-menu1></app-meu1>``

![import components](img/import%20components.PNG)
