# Créer un User avec une Function

* [Les bonnes pratiques](#les-bonnes-pratiques)
* [Préparer la function doCreateUser](#préparer-la-function-docreateuser)

## Les bonnes pratiques

On commence toujours par créer un dossier functions dans lequel on va mettre ses différentes fonctions. Il faut toujours
une seule et unique fonction par appel à la db !!! 
* On nomme les fonctions qui sont déclenchés depuis le front en commençant par "do" (ex: _doCreateUser_)
* On nomme les fonctions qui vont réagir avec la base de donnée en commençant par "on" (ex: _onManageUserState_)

## Préparer la function doCreateUser

