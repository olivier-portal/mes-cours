# angularFire Créer un utilisateur avec son mail et mot de passe

* [Créer le formulaire](#créer-le-formulaire)
* [Créer la fonction dans le service](#créer-la-fonction-dans-le-service)
* [Gérer les permissions de la base de données](#gérer-les-permissions-de-la-base-de-données)

## Créer le formulaire

> Pour structurer l'app voir les notes [12-angularFire Authentifcation.md](https://gitlab.com/olivier_portal/mes-cours/-/blob/master/Languages/angular/12-angularFire%20Authentifcation.md)

1. Réfléchir aux inputs dont on va avoir besoin (prénom, nom, mail, mot de passe, etc.)

2. Initialiser le formulaire dans le create-component avec les champs requis pour les inputs :

    ```javascript
    export class CreateComponent implements OnInit {
    
      createForm: FormGroup;
    
      constructor(
        private fb: FormBuilder,
      ) { }
    
      ngOnInit(): void {
        this._initForm();
      }
    
      private _initForm(): void {
        this.createForm = this.fb.group({
          firstName: ['', [Validators.required]],
          lastName: ['', [Validators.required]],
          birthdate: ['', [Validators.required]],
          email: ['', [Validators.required]],
          password: ['', [Validators.required]],
        });
      }
    
    }
    ```

3. Initialiser la fonction createAccount :
    ```javascript
   createAccount() {
     console.log(this.createForm.value);
   }
    ```

4. Créer le formulaire dans le html :

    ```angular2html
    <form ngForm [formGroup]="createForm" (ngSubmit)="createAccount()">
      <input type="text" formControlName="firstName" placeholder="Prénom">
      <input type="text" formControlName="lastName" placeholder="Nom">
      <input type="date" formControlName="birthdate" placeholder="03/06/2010">
      <input type="text" formControlName="email">
      <input type="password" formControlName="password">
      <button type="submit">Créer</button>
    </form>
    ```

## Créer la fonction dans le service

1. Initialiser la fonction createAccount$ dans le service :

    ```javascript
    createAccount$(email: string, password: string): Observable<UserCredential> {
        return from(this.angularFireAuth.createUserWithEmailAndPassword(email, password))
        }
    ```
   
2. Il faut alors modifier la fonction afin qu'elle fasse 2 appels, le premier à la db de création de compte de firebase
dont on n'a pas accès (le mot de passe y est stocké et sécurisé), le deuxième, à notre db firebase pour que l'on puisse
récupérer les datas de l'user (sauf le password il est strictement interdit par la loi de récupérer cette info!!!!).
Il faut donc au préalable créer un UserModel afin de pouvoir récupérer les données utilisateurs :

    Entrer dans la console `ng g class models/user --type=model`
    
    Cela crée l'UserModel, que l'on complète avec les champs de l'input, le password doit-être facultatif car on ne va pas
     le récupérer dans notre db :

    ```javascript
    export class UserModel {
      firstName: string;
      lastName: string;
      birthdate: Date;
      email: string;
      password?: string;
    }
    ```

3. Compléter alors la fonction avec un switchMap :

    ```javascript
    createAccount$(user: UserModel): Observable<void> {
        const {password, lastName, firstName, birthdate, email} = user;
        return from(this.angularFireAuth.createUserWithEmailAndPassword(email, password))
          .pipe(
            switchMap((cred: UserCredential) => from(this.afs.collection('users').doc(cred.user.uid).set({
              lastName, firstName, birthdate, email,
            })))
          );
    
      }
    ```
   
   > Ne pas oublier de charger afs (AngularFireStore)

4. Compléter la fonction createAccount dans le component (et charger l'authService) :

    ```javascript
   export class CreateComponent implements OnInit {
   
     createForm: FormGroup;
     isLoading = false;
   
     constructor(
       private fb: FormBuilder,
       private authService: AuthService,
     ) { }
   
     ngOnInit(): void {
       this._initForm();
     }
   
     createAccount(): void {
       this.isLoading = true;
       this.authService.createAccount$(this.createForm.value)
         .pipe(
           tap((x) => this.isLoading = false),
           tap((x) => alert('Utilisateur bien créé')),
           catchError(err => of(err)
             .pipe(
               tap((x) => console.error(err)),
               tap((x) => this.isLoading = false),
               tap((x) => alert(err)),
             ))
         )
         .subscribe();
     }
   
     private _initForm(): void {
       this.createForm = this.fb.group({
         firstName: ['', [Validators.required]],
         lastName: ['', [Validators.required]],
         birthdate: ['', [Validators.required]],
         email: ['', [Validators.required]],
         password: ['', [Validators.required]],
       });
     }
   
   }
    ```
5. On complète alors le html avec les nouvelles infos (isLoading, etc.), afin de gérer l'expérience utilisateur et notamment
empêcher la création de multiples comptes :

    ```angular2html
    <p *ngIf="isLoading">En cours de chargement...</p>
    
    <ng-container *ngIf="!isLoading">
      <form ngForm [formGroup]="createForm" (ngSubmit)="createAccount()">
        <input type="text" formControlName="firstName" placeholder="Prénom">
        <input type="text" formControlName="lastName" placeholder="Nom">
        <input type="date" formControlName="birthdate" placeholder="03/06/2010">
        <input type="text" formControlName="email">
        <input type="password" formControlName="password">
        <button type="submit">Créer</button>
      </form>
    </ng-container>
    ```

## Gérer les permissions de la base de données

Lorsque l'on va essayer de créer un compte on va recevoir à ce stade un message d'erreur nous indiquant que nous n'avons pas
de permissions, cela est dû que par défaut un utilisateur n'a pas de droit en écriture sur la db. Il va donc falloir gérer
ce cas spécifique où c'est l'utilisateur qui envoie des données sur la db lors de la création du compte :

![permission denied](img/permission%20denied.PNG)

Pour autoriser l'utilisateur à écrire des données (nom, prénom, etc.) pour ce cas (et uniquement ce cas), il faut aller dans
firebase => Cloud Firestore => rules et écrire une nouvelle règle :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userUid} {
      allow read, write: if request.auth.uid == userUid;
    }
  }
}
```

Puis cliquer sur publier :

![Firestore rules](img/firestore%20rules.PNG)


