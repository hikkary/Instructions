
# Passer son application React en PWA
Passer son application en PWA avec React ne demande pas beaucoup de travail

la configuration est déja faite, il suffit juste d'activer l'options.

Pour ce faire dans aller dans le fichier ```src/index.js```

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import * as serviceWorker from './serviceWorker'

ReactDOM.render(<App />, document.getElementById('root'))

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA

// serviceWorker.unregister(); <-- modifier cette ligne
serviceWorker.register()
```
On remarque qu'il suffit de faire passer ```serviceWorker.unregister()``` à ```serviceWorker.register()``` pour que notre App deviennent une PWA.

## Tester sa PWA 

Nous allons nous assurez que sa fonctionne, nous allons installer un paquet qui va noous permettre d'instancier un serveur http :

avec yarn: 

``` yarn add --dev http-server ```

avec npm :

```npm i --save-dev http-server```

nous allons editer le ```package.json``` pour y ajouter le script qui lancera notre app sur le serveur http.
```json
  "scripts": {
    ...
    "start-http": "http-server ./build"
  },
```
pour tester notre app il faudra la build et passer lancer la commande que l'on a ajouter ci dessus:

avec npm :

```npm run build && npm run start-http```

avec yarn :

```yarn build && yarn start-http```



