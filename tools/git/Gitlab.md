# Gitlab

* [Installer Git sur Webstorm](#installer-git-sur-webstorm)
* [Configurer ses paramètres](#configurer-ses-paramètres)
* [Ignorer des fichiers avec gitignore](#ignorer-des-fichiers-avec-gitignore)
* [Supprimer des fichiers déjà pusher avec gitignore](#supprimer-des-fichiers-déjà-pusher-avec-gitignore)
* [Commit file sur Gitlab](#commit-file-sur-gitlab)
* [Récupérer un fichier de Gitlab](#récuprer-un-fichier-de-gitlab)
* [Utiliser les Branch](#utiliser-les-branch)
* [Travailler à plusieurs](#travailler-à-plusieurs)

> GIT est un protocole
>
> Gitlab est un site internet (un outil) qui exploite le protocole Git (ainsi que github/bitbucket...)

**Il faut tout d'abord créer un fichier REPO (REPOSITORY), appeler Readme.md**

## Installer Git sur Webstorm:

* Ouvrir le terminal et taper les instructions:

`git init`

## Configurer ses paramètres:

* Ouvrir la console dans Webstorm
* Aller dans son naviateur et rechercher _git set username and password_
* Aller sur le site [atlassian](https://confluence.atlassian.com/bitbucket/configure-your-dvcs-username-for-commits-950301867.html)
* Copier coller dans la console les instructions du paragraphe _Configure your Git username/email_
* Aller sur Webstorm -> File -> settings
* Activer le plugin Gitlab, dans le marketPlace chercher Gitlab Project, installer le et redémarrer Webstorm
* Rouvrir les settings et cliquer dans version Control -> Gitlab
* Add a new server
* Mettre https://gitlab.com, gitlab.com puis cliquer sur Token Page
* Créer une clé API avec le personal Token pour webstorm

## Ignorer des fichiers avec gitignore:

* Pour ne pas envoyer des fichiers inutiles sur gitlab (cela ne sert à rien et n'est pas eco-responsable!)
* Créer un fichier .gitignore
* Entrer le nom des fichiers et dossiers que l'on souhaite ignorer

    _En général: .idea, node_modules, .firebaserc, .mesdatasecretes.js_

## Supprimer des fichiers déjà pusher avec gitignore:

* Si on a pusher des fichiers ou des dossiers avant de créer le fichier .gitignore, il faut les remove:

`git rm -rf nomDuFichier`

> Cela peut parfois faire bugger Webstorm, il suffit de relancer l'IDE

## Commit file sur Gitlab:

* Le protocole Git est lancé mais le fichier n'est pas encore partagé sur GIT (fichier en rouge)
* Faire clic droit sur le fichier REPO -> GIT -> Add (Le fichier passe en vert)
*  **Faire un COMMIT:** Pour envoyer le fichier sur GIT il faut cliquer sur le stick vert
* Une fenêtre de Commit s'ouvre, Il faut ajouter un message pour signaler l'action que l'on fait (exemple: Add new file Readme)
* Cliquer sur commit & push
    * Entrer ses identifiants Gitlab si ce n'est pas déjà fait
    * Sélectionner le projet souhaiter dans Gitlab, pour cela soit créer un nouveau projet, soit sélectionner un projet existant
Cliquer sur Clone et copier coller l'URL dans la fenêtre de Webstorm
* Cliquer sur Push
* Le fichier passe en blanc, il est bien ajouter dans le projet en cours sur Gitlab

## Récupérer un fichier de Gitlab:

* Aller dans son projet sur Gitlab
* Sélectionner le fichier à récupérer
* Cliquer sur Clone & copier l'adresse dans Webstorm
    * Aller dans VCS -> Get version control
    * Coller L'URL dans la fenêtre ouverte (Changer le nom du directory si nécessaire)
* Cliquer sur cloner
* Le fichier est uploader dans Webstorm

## Utiliser les Branch:

* Cliquer sur _Git: master_
* Sélectionner New Branch et le nommer de façon adéquate (ex: OlivierBranch)
* Commit & Push la nouvelle Branche, **cela permet de modifier le fichier sans toucher au fichier master qui est le fichier de référence**
* Il faut faire un check out pour passer d'une branche à une autre

** Toujours faire les modifs dans sa branche et jamais dans la master **

Une fois le travail fini on fait un **merge request** pour demander au chef de projet d'intégrer les modifiations au master (dans gitlab)

* Si les modifications sont accepter (le chef de projet merge et delete la branche secondaire)
* On doit récupérer le fichier master en faisant un check out sur le master
* Puis on pull (flèche bleue) pour mettre à jour le nouveau fichier master

## Travailler à plusieurs:

> Attention de pas faire de merge request si on est pas sûr de son résultat!

* Une branche doit s'appeller par sa feature (une fonctionnalité)
* Lorsqu'un conflit se retrouve dans les merge, ne pas résoudre directement le conflit dans gitlab
* Envoyer un message de mis à jour à la personne concernée
* La personne concernée quand elle récupère la master et veux la merger avec sa version cela montre les conflits
* Cette même personne est en charge de régler le conflit
* Si désaccord voir le chef de projet
* On ne peux plus commiter car un merge n'est pas reconnu comme une modification
* Faire Shift + Crl + K pour faire un Push
* Le chef de projet reçoit une notification et peux vérifier si les conflits sont réglés
* Le master est de nouveau propre
