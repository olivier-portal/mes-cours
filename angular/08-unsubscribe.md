# Unsubscribe

* [Se désinscrire d'un abonnement](#se-désinscrire-dun-abonnement)
* [OnDestroy](#ondestroy)
* [Take until](#take-until)

## Se désinscrire d'un abonnement

Tant que nous ne nous sommes pas désinscrit d'un abonnement, celui-ci continue d'être actif. Cela signifie que si l'on change de page
l'abonnement à une behavior subject reste quand même actif et que si l'on revient sur la page initiale, un deuxième abonnement
se lance. On peut donc se retrouver avec des 100aines d'abonnements identiques qui tournent et génèrent donc des memory leaks,
des stack overflow, et dans le meilleur des cas fait ramer l'application, dans le pire, la fait planter. Il faut donc se
désabonner lorsque l'abonnement n'est plus utile. Dans le cycle de vie d'angular d'un abonnement, se désabonner s'appelle le destroy.

## OnDestroy

* On va donc auto-détruire l'abonnement dès que l'app ne sera plus utilisée. Pour cela, on va intégrer dans le cycle de vie
de l'abonnnement OnDestroy :

```
export class AppComponent implements OnInit, OnDestroy {
  title = 'promo4Angular';

  message: string;

  allRoutes: MenuModel[] = this.menuService.allRoutes;

  age = 42;

  constructor(
    private menuService: MenuService,
    private moodService: MoodService,
  ) {
  }

  ngOnInit(): void {
    this.moodService.handleMood$
      .pipe(
        tap((message: string) => this.message = message)
      )
      .subscribe();
  }

  ngOnDestroy() {
  }

}
```

* On ajoute bien un ngOnDestroy qui va permettre de détruire l'abonnement quand on en a plus besoin. Pour gérer la destruction
de tous les abonnements, on va utiliser la méthode du take until (tu fais tant que...)

## Take until

* On crée alors un nouveau subject propre au component qui va s'appeler destroy$ : 

```
====================================================================
export class AppComponent implements OnInit, OnDestroy {
====================================================================
  title = 'promo4Angular';

  message: string;

  destroy$: Subject<boolean> = new Subject();

  allRoutes: MenuModel[] = this.menuService.allRoutes;

  age = 42;

  constructor(
    private menuService: MenuService,
    private moodService: MoodService,
  ) {
  }

  ngOnInit(): void {
    this.moodService.handleMood$
      .pipe(
        tap((message: string) => this.message = message)
      )
      .subscribe();
  }

====================================================================
  ngOnDestroy() {
  }
====================================================================
}
```

> À noter : un Subject est un behavior subject que l'on initialise jamais


* On va alors détruire l'abonnement dans le ngOnDestroy en lui envoyant l'information de s'auto-détruire ainsi que
l'information de s'arrêter (débrancher le tuyau destroy) pour que lui aussi s'arrête une fois la destruction effectuée
et ne crée pas de stack overflow ou de memory leak :

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
  ) {
  }

  ngOnInit(): void {
    this.moodService.handleMood$
      .pipe(
        tap((message: string) => this.message = message)
      )
      .subscribe();
  }

  ngOnDestroy() {
====================================================================
    this.destroy$.next(true);
    this.destroy$.complete();
====================================================================
  }

}
```

* On va alors ajouter l'instruction à ngOnInit de continuer à fonctionner tant que le destroy n'a pas eu lieu :

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
  ) {
  }

  ngOnInit(): void {
    this.moodService.handleMood$
      .pipe(
====================================================================
        takeUntil(this.destroy$),
====================================================================
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
