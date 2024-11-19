# Créer du contenu en HTML


* [Créer sa première page HTML](#créer-sa-premire-page-html)
* [Les balises](#les-balises)
    * [Balises spéciales](#balises-spciales)
    * [Balises principales](#balises-principales)
* [Les attributs](#les-attributs)
* [Organiser son projet](#organiser-son-projet)
    * [Faire un lien vers le css](#faire-un-lien-vers-le-css)
    * [Faire un lien vers bootstrap](#faire-un-lien-vers-bootstrap)
* [Créer des liens entre plusieurs pages](#créer-des-liens-entre-plusieurs-pages)
* [Créer des classes CSS](#créer-des-classes-css)
    * [Sélectioner uniquement une partie d'une section](#sélectioner-uniquement-une-partie-dune-section)

## Créer sa première page HTML

A partir de l'IDE créer un nouveau fichier **index.html** (toujours appeler la première page sous le nom index)

Cela crée automatiquement le début d'une structure correcte de la page HTML, à savoir:

     <!DOCTYPE HTML> ...
    
> **Le HTML est un language MARKUP (HyperText Markup Language = html)**

## Les balises

Ce language est fait de balises qui s'écrivent par une balise ouvrante <> et fermante </> (nb: à l'exception de certaines balises qui ne se ferment pas)

    <div class="Toto">Hello World!</div>

Il faut toujours bien indenté son code afin de pouvoir s'y retrouver et pouvoir se relire, ou permettre à d'autres Dev de nous relire

Il suffit de faire 1 tab pour la 2éme balise dans la balise parent (chaque enfant peut-être parent d'une autre balise, rajouter un tab)

> Astuce: en cas de problème d'indentation, faire **Alt + Ctrl + l** pour réindenter correctement le fichier

#### Balises spéciales

Meta, Body, Title....

> Astuce: avec l'IDE webstorm quand on clique dans la page, l'IDE propose automatiquement le choix d'un navigateur afin de voir le résultat sur internet (on reste en local pour le moment


#### Balises principales

\<h1>\</h1> pour le gros titre de la page
h2, 3, 4 pour les titres secondaires, tertiaires etc.

> astuce: Ne pas s'embêter avec les chevrons, écrire juste h1 + tab cela ajoute les chevrons pour la balise ouvrante et fermante

les paragraphes ont la balise p

## Les attributs

A l'intérieur de chaque balise il y a des attributs (ex: dans la balise HTML on retruve l'attribut lang="en" qui siggnifie que la page sera en anglais; fr pour français)

> ctrl + espace nous donne toutes les attributs possibles

## Organiser son projet

* Créer un fichier index.html à la racine du projet **recommandé** _ou créer un dossier **main** (pour la page principale)_
* Créer un dossier **assets** (pour y intégrer les img etc.)
* Créer un dossier pour chaque autre page _(si le site n'est pas un onepage)_
    * exemple : un dossier **mes xp** ( qui renverr vers la page mes expériences) avec à l'intérieur un fichier **index.html**

#### Faire un lien vers le css:

* Dans la section \<head>\</head> ajouter le lien vers le fichier css qui se trouve dans le dossier css:

    `<link rel="stylesheet" href="../css/style.css">`

_Pour info, pour remonter dans l'arborescence il faut écrire ../_

> Astuce: faire ctrl + espace pour avoir les dossiers ou fichiers disponibles cela évite les erreurs et est un gain en productivité

#### Faire un lien vers bootstrap:

* Dans le \<head>\</head> ajouter le lien vers le framework bootstrap ([Voir la fiche bootstrap](https://gitlab.com/olivier_portal/mes-cours/-/blob/master/Languages/bootstrap/Bootstrap.md)):

    `<link rel="stylesheet" href="../assets/vendors/bootstrap/css/bootstrap.min.css">`

## Créer des liens entre plusieurs pages:

* Les liens entre les pages sont créés par la balise:

    `<a href="../xp/index.html">Mes expériences</a>`

_Note:_
       
>./ signifie dossier courant.       
>../ signifie dossier parent.

## Créer des classes CSS

* D'abord créer la Balise dans la page html (pour créer une partie indépendante de la page) exemple:

        <article></article> 

* On crée alors des class (dans la balise), ces class servent à pouvoir modifier le style de plusieurs balises en même temps peu importe le nom de la balise, exemple:

        <artile class="my-color"></article>

#### Sélectioner uniquement une partie d'une section

* On peut sélectionner seulement une partie d'une section, par exemple juste le h2 en ajoutant la class card-title:

````html
<article class="card">

   <h2 class="card-title">Mon titre de la carte</h2>
   
   <p>lorem ipsum dolor sit met</p>

</article>
````




