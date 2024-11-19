# Créer du contenu en Markdown

* [Les titres](#les-titres)
* [Présenter une ou plusieurs lignes de code](#présenter-une-ou-plusieurs-lignes-de-code)
* [Mettre en forme du texte](#mettre-en-forme-du-texte)
* [Insérer des liens](#insérer-des-liens)
    * [Les liens externes](#les-liens-externes)
    * [Les liens internes](#les-liens-internes)
* [Insérer des images](#insérer-des-images)


## Les titres

* Le symbole `#` correspondau titre principal de la page. il ne peut y avoir qu'un seul titre principal par page.

* Le symbole `##` correspond au titre niveau 2.

* Le symbole `###` correspond au titre niveau 3.

etc.

## Présenter une ou plusieurs lignes de code

* Si c'est une ligne de code: ``

  * Ex: `const toto = 4`

* Si c'est plusieurs lignes de codes: ``` au debut et a la fin du bout de code ( on ouvre et on ferme
)

  * Ex:

```Javascript
const  toto = 42
const  toto = 42
const  toto = 42
const  toto = 42
```

_PS: Le langage utilise peut etre indique juste apres le symbole ``` et ce en fonction des interpreters. Ceci est vivement recommande._

## Mettre en forme du texte

* Pour ecrire en **gras** on utilise le symbole: ** **
* Pour ecrire en _Italique_ on utilise le symbole: _ _
* Pout ecrire une citation on utilise le symbole: >

  - Ex:

    > salut
    > Comment va-t-on aujourd'hui?

_note: Il convient de sauter une ligne afin que les phrases ne soient pas collées._

* Pour saisir des listes a puce on utilise le symbole: `*`. Il doit y avoir un espace entre le `*` et le mot en question.

* Pour saisir des listes numerotees on utilise le symbole: 1 . Il doit y avoir un espace entre le 1,2 ou 3 et le mot en question.

    * Les sous listes sont presentees comme suit:  `tab * TEXT`
    
## Insérer des liens

#### Les liens externes

* pour insérer un lien externe (vers un site internet par exemple, on utilise les symboles: `[nom du site](URL du site)`

  * Ex:
[wikipedia](https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Accueil_principal)

#### Les liens internes

* pour insérer un lien interne (vers un paragraphe de la page, on utilise les symboles: `[nom du site](#title)`

  * Ex:
[Les titres](#les-titres)

**Attention ! ne mettre qu'un seul `#` peu importe le niveau du titre (Titre principale, Titre 2, etc.)**

**Attention ! Après le `#` il ne faut pas de majuscule et il faut remplacer les espaces par des `-`*

## Insérer des images

* Pour insérer une image dans un fichier Markdown, il faut utiliser les liens en utilisant `![img](chemin-vers-l-image)`
    * Exemple:
     
     ![cat-hacker](Languages/markdown/img/cat-hack.jpg)
