# Cross origin

> Par défaut dans le protocole HTTP, on ne peut pas aller chercher des données d'une url à une autre (exemple entre
> http://localhost:3000 et http://localhost:3015)

On appelle ce système de sécurité le cross origin ou CORS.

On peut régler le problème si on autorise la db à envoyer des données à d'autres sites :

Pour cela on installe dans le fichier package.json les dependencies de cors et @types/cors, pour ce faire on les installe
grâce à la console :

```javascript
npm i -s cors
```
> cela sauvegarde dans les dependencies de cors

Et :

```javascript
npm i -D @types/cors
```
> cela sauvegarde dans les dependencies de cors

Ensuite on active le cors dans server.ts (ne pas oublier de l'importer) :

```javascript
import cors from 'cors';

app.use(cors());
```

Le serveur de la db authorise des appels de données d'autres url.
