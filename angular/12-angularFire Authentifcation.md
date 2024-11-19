# AngularFire authentification

* [Introduction](#introduction)
* [Organiser son code](#organiser-son-code)
* [Créer la page de login](#créer-la-page-de-login)
* [Créer le service d'authentification](#créer-le-service-dauthentification)
* [Gérer les autorisations d'accès aux pages](#gérer-les-autorisations-daccès-aux-pages)

## Introduction

> Toujours utiliser la doc officielle !!!!

[AngularFire](https://github.com/angular/angularfire)

![AngularFire Doc](img/angularFire%20doc.PNG)

Pour rappel, il faut bien organiser son travail : On commence par créer un nouveau projet dans lequel on crée un dossier
frontend + un .gitignore et un README.md.

Dans le dossier frontend, on crée à son tour un nouveau projet angular avec la commande ng new

On crée alors un projet firebase, et on va chercher les informations de configuration (paramètres du projet => configuration)

![Firebse config](img/firebase%20config.PNG)

On les colle alors dans src/environments/environment.ts (et environment.prod.ts) comme suit :
```javascript
export const environment = {
  production: false,
  firebase: {
    apiKey: 'AIzaSyCK1IfTkvzkpRR0oaSr0f7rvizztmExxQg',
    authDomain: 'angularlogin-7a230.firebaseapp.com',
    databaseURL: 'https://angularlogin-7a230.firebaseio.com',
    projectId: 'angularlogin-7a230',
    storageBucket: 'angularlogin-7a230.appspot.com',
    messagingSenderId: '81051891735',
    appId: '1:81051891735:web:c4a66a01761b73cfdcc452',
    measurementId: 'G-CWY292W9BB',
  }
};

```

(production : true pour la prod)

On fait l'installation d'angularFire `ng add @angular/fire`

> Ne pas oublier de se mettre dans le dossier frontend `cd frontend`

On va ensuite importer dans l'app-module l'angularFireModule comme suit :

```javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { AngularFireModule } from '@angular/fire';
import { environment } from '../environments/environment';

@NgModule({
  imports: [
    BrowserModule,
/// AJOUT DE ANGULARFIREMODULE ----------------------------------------
    AngularFireModule.initializeApp(environment.firebase)
  ],
  declarations: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule {}
```

## Organiser son code

> Afin de bien commencer le projet, il faut impérativement bien organiser son code

* On commence donc par créer un nouveau module incluant une nouvelle route pour l'authentification et rattaché à l'app-module
(et donc à l'app-routing-module) :

    `ng g m auth --route=auth --module=app.module`
    
    Cela créer donc bien un nouveau module mais ajoute en plus un routing-module :
    
    ![ajout d'un module avec route](img/creation%20module%20avec%20route.PNG)
    
    Cela met aussi à jour la route du module principal :
    
    ![MAJ route](img/MAJ%20route%20module%20principal.PNG)
    

* On crée ensuite un module (dit corporate, càd accessible à tous), lui aussi rattaché à la route principale :

    `ng g m corp --route=corp --module=app.module`

* On crée un dernier module (toujours attaché à la route principale) qui sera private (accessible qu'aux personnes loggées) :

    `ng g m home --route=home --module=app.module`

* Dans l'app-routing-module on obtient bien une hiérarchisation des routes bien structurée, à laquelle il faudra ajouter
le chargement du AppComponent

    ```javascript
    const routes: Routes = [
      {
        path: '',
        component: AppComponent
      },
      {
        path: 'auth',
        loadChildren: () => import('./auth/auth.module').then(m => m.AuthModule),
      },
      {
        path: 'corp',
        loadChildren: () => import('./corp/corp.module').then(m => m.CorpModule)
      },
      {
        path: 'home',
        loadChildren: () => import('./home/home.module').then(m => m.HomeModule),
      }
    ];
    ```

* On va maintenant avoir besoin de pages complémentaires pour se logger, créer un compte, pour les mots de passe oubliés etc.
Il faut donc bien penser à comment **STRUCTURER** son app avant de commencer !

* Dans notre cas on va uniquement créer une page de logg et une page de création de compte. Ces 2 pages seront, elles, attachées
à la route du module d'authentification :

    `ng g m auth/login --route=login --module=auth.module`
    
    puis :
    
    `ng g m auth/create --route=create --module=auth.module`
    
    cela crée bien les 2 modules désirés et met à jour les routes d'authentification :
    
    > Attention : par défaut les routes sont toutes mises au même niveau, il faudra restructurer les routes afin que les pages de
    > login et de création de compte soient les enfants de la route principale !

* On restructure donc le code comme suit :

    ```javascript
    const routes: Routes = [
      {
        path: '',
        component: AuthComponent,
        children: [
          {
            path: 'login',
            loadChildren: () => import('./login/login.module').then(m => m.LoginModule)
          },
          {
            path: 'create',
            loadChildren: () => import('./create/create.module').then(m => m.CreateModule)
          }
        ]
      },
    ];
    ```
    
    Comme auth à des enfants, il faut lui ajouter un router-outlet dans le html :
    
    ```html
    <p>auth works!</p>
    
    <router-outlet></router-outlet>
    ```

## Créer la page de login

On commence par créer le formulaire de connexion. Dans le component on va initialiser le loginForm avec le formBuilder et 
 créer une fonction (private) qui va initialiser le formulaire :

```javascript
export class LoginComponent implements OnInit {

  loginForm: FormGroup;

  constructor(
    private fb: FormBuilder,
  ) {
  }

  ngOnInit(): void {
    this._initForm();
  }


  private _initForm(): void {
    this.loginForm = this.fb.group({
      login: ['', [Validators.required, Validators.minLength(6)]],
      password: ['', [Validators.required, Validators.minLength(6)]],
    });
  }

}
```

* On crée alors le form dans le html :

    ```angular2html
    <form ngForm [formGroup]="loginForm" (ngSubmit)="login()" autocomplete="off">
      <input type="text" formControlName="login" autocomplete="off">
      <input type="password" formControlName="password" autocomplete="off">
      <button type="submit">Envoyer</button>
      <button type="button" (click)="cancel()">Cancel</button>
    </form>
    ```

* Il nous faut alors créer les fonctions login() (pour le moment on fait juste un console.log de la value) et cancel() 
dans le component correspondant à nos 2 boutons :

    ```javascript
      login(){
        console.log(this.loginForm.value);
      }
    
      cancel() {
        this.loginForm.reset();
      }
    ```

* On veut maintenant que lorsque l'on se connecte à la page auth, on soit redirigé directement vers la page login (auth/login)
Dans l'auth-routing-module on va donc ajouter une redirection :

    ```javascript
    const routes: Routes = [
      {
        path: '',
        component: AuthComponent,
        children: [
          {
            path: '',
            redirectTo: 'login',
            pathMatch: 'full',
          },
          {
            path: 'login',
            loadChildren: () => import('./login/login.module').then(m => m.LoginModule)
          },
          {
            path: 'create',
            loadChildren: () => import('./create/create.module').then(m => m.CreateModule)
          }
        ]
      },
    ];
    ```
    
    > Attention : Ne pas oublier le pathMatch setté à full !!!

## Créer le service d'authentification

* Afin de créer nos fonctions, il nous faut désormais créer un service d'authentification :

    `ng g s services/auth`

* On va donc commencer à créer dans le service un behaviorSubject afin de pouvoir se servir de l'utilisateur quand on veut,
 puis initialiser :

    ```javascript
    export class AuthService {
    
      user$ = new BehaviorSubject(null);
    
      constructor() {
      }
    ```
    
* On va alors appeler l'AngularFireAuthModule (et au préalable le déclarer dans l'app-module) :
    
    ```javascript
    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        AngularFireModule.initializeApp(environment.firebase),
        AngularFireAuthModule,
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    ```
    
    ```javascript
    export class AuthService {
    
      user$ = new BehaviorSubject(null);
    
      constructor(
        private angularFireAuth: AngularFireAuth,
      ) {
      }
    ```

* On va donc pouvoir créer nos fonctions que l'on importera ultérieurement dans le login-component :

1. La première fonction à créer est celle qui permet à l'utilisateur de se logger avec un mail et mot de passe :

    ```javascript
    export class AuthService {
    
      user$ = new BehaviorSubject(null);
    
      constructor(
        private angularFireAuth: AngularFireAuth,
        private router: Router,
      ) {
      }
    
      login$(login, password): Observable<any> {
        return from(this.angularFireAuth.signInWithEmailAndPassword(login, password))
          .pipe(
            switchMap((x) => this.router.navigate(['/home'])),
            catchError(err => of(err)
              .pipe(
                tap((x) => alert(x)),
              )
            )
          );
      }
    ```
    
    > À noter : Il s'agit d'une promesse, en angular classique on n'a pas le droit d'utiliser les promesses, on ajoute donc
    > un from() autour de la promesse.
    >
    > On ajoute également une redirection automatique vers la page home une fois loggé avec un switchMap
    >
    > il faut également appeler le router

2. On va alors ajouter un bouton logout dans le html de la page login et créer sa fonction dans l'authService, mais au préalable,
Il faut récupérer l'user et on veut que toute l'app soit au courant de se qui se passe. Il faut donc aller dans le component 
principal et ajouter un constructeur qui va chercher AngularFireAuth, comme suit :

   ```javascript
   export class AppComponent implements OnInit{
   
     constructor(
       private angularFireAuth: AngularFireAuth,
     ) {
     }
   
     ngOnInit(): void {
       this.angularFireAuth.authState
         .pipe(
           tap((x) => console.log(x)),
         )
         .subscribe();
     }
   }

   ```
   
   > Ne pas oublier d'initialiser avec un ngOnInit()
   
   * On modifie alors notre fonction login dans le service :
   
   ```javascript    
       login(){
         const {login, password} = this.loginForm.value;
         this.authService.login$(login, password)
           .subscribe();
       }
   ```
   
   * On crée alors notre fonction logout dans le service :
   
    ```javascript    
      logout$(): Observable<any> {
          return from(this.angularFireAuth.signOut());
        }
    ```
   * Puis le bouton logout dans le html :
   
   ```angular2html    
     <section>
        <button (click)="logout()">logout</button>
     </section>
   ```
   
   * Et enfin dans le login-component :
   
   ```angular2html
     logout(): void {
       this.authService.logout$()
         .subscribe();
     }
   ```

3. On doit alors créer une fonction pour setter l'utilisateur :

    ```javascript    
      setUser(user): void {
        this.user$.next(user);
      }
    ```
   
4. On crée maintenant la fonction qui va getter des users :

    ```angular2   
      getUser$(): Observable<any> {
        return this.user$.asObservable();
      }
    ```
   
   > On utilise un observable pour arrêter la méthode next et interdire aux users d'utiliser la fonction
   
5. Dans l'app-component on peut désormais intégrer notre authService et compléter l'initialisation :

   ```javascript
    export class AppComponent implements OnInit{
    
      constructor(
        private angularFireAuth: AngularFireAuth,
        private authService: AuthService,
      ) {
      }
    
      ngOnInit(): void {
        this.angularFireAuth.authState
          .pipe(
            tap((user) => this.authService.setUser(user)),
          )
          .subscribe();
      }
    }
   ```
   
   > A noter : On peut setter des infos supplémentaires sur l'user en destructurant :
   ```javascript
         setUser(user): void {
             if (user) {
               this.user$.next({...user, isConnected: true});
             } else {
               this.user$.next({isConnected: false, user: null});
             }
           }
   ```

## Gérer les autorisations d'accès aux pages

On va devoir gérer les autorisations des users suivant s'ils sont connectés ou non et s'ils ont un droit d'accès aux
différentes pages. Pour cela, on copie/colle les lignes de codes de la doc d'angularFire (variables + redirections des paths
, comme suit :

```javascript
const redirectUnauthorizedToLogin = () => redirectUnauthorizedTo(['/auth/login']);
const redirectLoggedInToHome = () => redirectLoggedInTo(['/home']);

const routes: Routes = [
  {
    path: '',
    component: AppComponent
  },
  {
    path: 'auth',
    loadChildren: () => import('./auth/auth.module').then(m => m.AuthModule),
    ...canActivate(redirectLoggedInToHome),
  },
  {
    path: 'corp',
    loadChildren: () => import('./corp/corp.module').then(m => m.CorpModule)
  },
  {
    path: 'home',
    loadChildren: () => import('./home/home.module').then(m => m.HomeModule),
    ...canActivate(redirectUnauthorizedToLogin),
  }
];
```


