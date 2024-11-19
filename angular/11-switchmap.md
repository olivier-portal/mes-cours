# Switchmap

* [Introduction](#introduction)

## Introduction

Le switchMap s'utilise à l'intérieur d'un tuyau (un pipe). Il sert à passer du tuyau principal à un autre tuyau. Par exemple
si l'on a une base de données avec des hotels et des rooms, on peut piper l'hotel puis switchmaper sur les rooms pour les
récupérer en même temps que l'hôtel.

## En pratique

Pour utiliser un switchMap, on écrit le code suivant dans le service :

```
getHotelById$(hotelId: string): Obeservable<any> {
    .get(this.root + 'hostels/' + hotelId) as Observable<HotelModel>)
    .pipe(
        tap((hotel: HotelModel) => this.hotel = hotel),
        switchMap((hotel: HotelModel) => combineLatest(
            hotel.rooms.map(roomId => this.getRoomById$(roomId)))
        ),
        tap((rooms: RoomModel[]) => this.hotel.roomData = rooms)
    )
}
```

Dans le typescript du component :

```
export class AppComponent implements OnInit, OnDestroy {
  hotel: HotelModel;
}

getHotelById() {
    this.hotelservice
       .getHotelById$('xxxxxxxxxx')
       .pipe(
            tap((hotel: HotelModel) => this.hotel = hotel), 
    )
    .subscribe();
}
```

Dans le html du component :

```html
<div>{{hotel | json}}</div>
```

> Le json n'est pas obligatoire, on peut utiliser un ngFor

On obtient bien le résultat de notre hotel avec ses rooms :

![hotel et rooms](img/hotel%20et%20rooms.PNG)
