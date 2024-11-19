# Le css


* [Organiser son css](#organiser-son-css)
* [Les cascades](#les-cascades)
* [Les flexbox](#les-flexbox)

> Le css est le Cascade Sheet Style soit en français les feuilles de styles en cascade
>
> Cela sert à mettre en forme le contenu (ce qui se trouve dans le html) d'un site, d'une newsletter...

## Organiser son css:

* Spécialiser ses feuilles de styles css, il faut en créer plusieurs, chacune d'entre elle sera spécialisée pour un style particulier

    exemple: Faire une feuille css pour le menu, une autre pour le header, le footer etc.

* Créer un directory css, y créer la première page css, par défaut style.css (ne pas oublier le link dans le html)
* Créer un nouveau fichier pour le main par exemple -> main.css
* Dans le style.css, ajouter tout en haut du fichier

    `@import url("main.css")`
    
* Répéter pour chaque feuille de style

## Les cascades:

* **important !** Les feuilles de styles étant en cascade, la dernière feuille de style prend le pas sur celle en amont
    * exemple: S'il y a 2 class identiques utilisées dans 2 css, le css en aval écrasera les données du css en amont
    
        Dans style.css, on a:
        
        ```css
         @import url(header.css);
         @import url(main.css);
        ```
      
      Dans les header.css et main.css on utilise la même class = "mon-contenu"
      
      Si on écrit dans header.css:
      ```css
      .mon-contenu{
          color: red;
        }
      ```      
      Et dans main.css:
      ```css
      .mon-contenu{
          color: blue;
        }
      ```

        **Le texte sera bleu !**
        
## Les flexbox:

* Créer une feuille de style flex-container.css
* Dans la div parente mettre sur la class un display: flex;
* On peut ajouter des arrangements ex: justify-content: space-between;
* Cela applique le flex à la Div enfant

    ```css
  .card{
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;    
    }
  ```

    _Astuce:_
    
    _Un bon site pour connaître toutes les possibilités flex est [css-tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)_
    
    _Un bon site pour connaître comment modifier les angles d'une box est [bennettfeely.com](https://bennettfeely.com/clippy/)_

