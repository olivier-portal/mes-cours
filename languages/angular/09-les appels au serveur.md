# Les appels au serveur

* [Les variables d'environnement](#les-variables-denvironnement)
* [Angular get](#angular-get)
* [CombineLatest](#combinelatest)
* [CRUD](#crud)
    * [Post](#post)
    * [Delete](#delete)
    * [Put & Patch](#put-&-patch)

## Les variables d'environnement

Avant de pouvoir faire des requêtes serveurs, il nous faut configurer les environnements de travail. Ceux-ci se trouvent
à la racine, dans le dossier 'environments'. On y trouve 2 fichiers typescript, un appelé environment et l'autre
environments.prod. Le premier sert à travailler en local, le second à déployer en production l'app finalisée. Pour le moment dans les
2 fichiers, o va ajouter une variable api avec pour adresse l'api locale sur laquelle on travaille (on changera plus tard
l'api de prod par la bonne api) :

```
export const environment = {
  production: false,
  api: 'http://localhost:3015/api/',
};
```

## Angular get

Attention !!! Il est strictement interdit de faire une requête au serveur à partir d'un component !

* Pour faire une requête serveur, il faut créer un service dédié (`ng g s services/nom-du-service)

* Dans le service ainsi créer il faut créer des requêtes HTPP, pour cela on utilise l'outil Angular HttpClient (en private) :

```
import { Injectable } from '@angular/core';
import {HttpClient} from "@angular/common/http";

@Injectable({
  providedIn: 'root'
})
export class TeamService {

  constructor(
=============================================================
    private httpClient: HttpClient,
=============================================================
  ) { }
}
```

* On va alors créer une variable root (qui est la racine de l'api) :

```
export class TeamService {
=============================================================
  root:string = environment.api;
=============================================================
  constructor(
    private httpClient: HttpClient,
  ) { }
}
```

> À noter : lors de l'import de l'environnement, il nous est demandé si l'on souhaite importe l'environnement ou l'environnement.prod
> On choisit toujours l'environnement (la prod sera importé plus tard naturellement)

* On peut alors créer une fonction (qui est aussi un observable) de type getObjectById$ pour faire une get de l'id de l'objet :

```
export class TeamService {

  root:string = environment.api;

  constructor(
    private httpClient: HttpClient,
  ) { }
=============================================================
  getTeamById$(teamId: string) {
    return this.httpClient.get(this.root + teamId);
  }
=============================================================
}
```

* On peut aussi créer une fonction pour faire un get sur tous les objets de la collection :

```
export class TeamService {

  root:string = environment.api;

  constructor(
    private httpClient: HttpClient,
  ) { }
  getTeamById$(teamId: string) {
    return this.httpClient.get(this.root + teamId);
  }

=============================================================
  getAllTeams() {
    return this.httpClient.get(this.root);
  }
=============================================================
}
```

* Comme on est en typescript, il faut typer les objets. On va donc récupérer en copiant/collant les modèles de nos collections
du back-end vers le front, puis on type nos objets :

```
export class TeamService {

  root:string = environment.api;

  constructor(
    private httpClient: HttpClient,
  ) { }

=============================================================
  getTeamById$(teamId: string): Observable<TeamModel> {
    return this.httpClient.get(this.root + teamId) as Observable<TeamModel>;
  }

  getAllTeams(): Observable<TeamModel[]> {
    return this.httpClient.get(this.root) as Observable<TeamModel[]>;
  }
=============================================================
}
```

* On va alors faire l'appel des fonctions dans notre app, en important le service d'abord :

```
export class AppComponent implements OnInit, OnDestroy {
  title = 'promo4Angular';

  message: string;

  destroy$: Subject<boolean> = new Subject();

  allRoutes: MenuModel[] = this.menuService.allRoutes;

  age = 42;

  constructor(
    private menuService: MenuService,
    private moodService: MoodService,
=============================================================
    private teamService: TeamService,
=============================================================
  ) {
  }

  ngOnInit(): void {

=============================================================
    this.teamService.getAllTeams()
      .pipe(
        takeUntil(this.destroy$),
        tap(x => console.log(x)),
      )
      .subscribe();
=============================================================

    this.moodService.handleMood$
      .pipe(
        takeUntil(this.destroy$),
        tap((message: string) => this.message = message)
      )
      .subscribe();
  }

  ngOnDestroy() {
    this.destroy$.next(true);
    this.destroy$.complete();
  }

}
```

* Il faut pour finir importer dans le module principal de l'app le HttpClientModule :

```
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import {MainMenuModule} from "./components/main-menu/main-menu.module";
import {MoodModule} from "./components/mood/mood.module";
=============================================================
import {HttpClient, HttpClientModule} from "@angular/common/http";
=============================================================

@NgModule({
  declarations: [
    AppComponent
  ],
    imports: [
        BrowserModule,
        AppRoutingModule,
        MainMenuModule,
        MoodModule,
=============================================================
        HttpClientModule,
=============================================================
    ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## CombineLatest

> Afin de ne pas gérer des abonnements multiples (subscribe1, subscribe2...subscribe1200), ce qui serait compliqué à gérer,
> il nous faut nous abonner qu'une seule fois avec le mot clé combineLatest.

CombineLatest prend en paramètre un array, composé d'objets qui sont tous les appels des fonctions de notre App au serveur :

```
export class AppComponent implements OnInit, OnDestroy {
  title = 'promo4Angular';

  message: string;

  destroy$: Subject<boolean> = new Subject();

  allRoutes: MenuModel[] = this.menuService.allRoutes;

  age = 42;

  constructor(
    private menuService: MenuService,
    private moodService: MoodService,
    private teamService: TeamService,
  ) {
  }

  ngOnInit(): void {

=============================================================
    combineLatest([
      this.teamService.getAllTeams()
        .pipe(
          tap(x => console.log(x)),
        ),
      this.teamService.getTeamById$('')
        .pipe(
          tap(x => console.log(x)),
        ),
      this.moodService.handleMood$
        .pipe(
          tap((message: string) => this.message = message)
        )
    ])
      .pipe(
          takeUntil(this.destroy$),
      )
      .subscribe();
=============================================================
  }

  ngOnDestroy() {
    this.destroy$.next(true);
    this.destroy$.complete();
  }

}
```

> À noter : En plus de s'abonner qu'une seule fois, on sort le takeUntil en ajoutant un pipe après le combineLatest, pour
> ne l'écrire qu'une seule fois.


## CRUD

Nous pouvons faire des crud sur la db de la même manière que nous avons déjà fait un get :

#### Post

* Il faut d'abort créer l'observable du post dans le service dédié :

```
createNewTeam$(team: TeamModel): Observable<TeamModel> {
    return this.httpClient.post(this.root + 'teams', team) as Observable<TeamModel>;
  }
```

* On crée alors une fonction dans le component de l'app :

```
  createTeam(): void {
    this.teamService.createNewTeam$({
      name: 'Olympique Lyonnais',
      shirtColor: 'Bleu',
      championship: 'Ligue 1',
      leaguePlace: 3,
    }).subscribe();
  }
```

* Pour un post, il nous faudra alors créer un bouton dans le html de l'app afin de créer notre envoi du nouvel objet :

```
<button (click)="createTeam()">Envoyer</button>
```

* On obtient bien une réponse avec la nouvelle team créée :

Dans le network :

![team créée](img/team%20crée%20console.PNG)

Dans la db :

![team créée](img/team%20crée%20firebase.PNG)

* Pour plus de visibilité, on peut rajouter un pipe avec un tap d'un console log dans notre observable :

```
  createNewTeam$(team: TeamModel): Observable<TeamModel> {
    return (this.httpClient
      .post(this.root + 'teams', team) as Observable<TeamModel>)
      .pipe(
        tap(x => console.log(x)),
      );
  }
```

#### Delete

* Il faut d'abort créer l'observable du delete dans le service dédié :

```
  deleteTeam$(teamId: string): Observable<string> {
    return (this.httpClient
      .delete(this.root + 'teams/' + teamId) as Observable<string>)
      .pipe(
        tap(x => console.log(x)),
      );
  }
```

* On crée alors une fonction dans le component de l'app :

```
  deleteTeam(): void {
    this.teamService.deleteTeam$('qAMoM5yBvX0KModdndM1').subscribe();
  }
```

* Pour un delete, il nous faudra alors créer un bouton dans le html de l'app afin de créer notre envoi du nouvel objet :

```
<button (click)="deleteTeam()">Supprimer</button>
```

* On obtient bien une réponse avec la team supprimée :

Dans le network :

![team delete](img/delete%20team.PNG)

Dans la db :

![team créée](img/delete%20team%20firebase.PNG)

Si on relance l'appel avec la même id, on obtient bien la réponse attendue :

![team créée](img/delete%20team%20with%20no%20id.PNG)

#### Put & Patch

Les put et les patch fonctionnent de la même façon.
