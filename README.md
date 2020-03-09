
# Mise en place d'un linter et d'un Prettier

<details>

## Installation
<details>


Premierement il faut installer les extensions Vscode suivante : 

Eslint | Rechercher ```dbaeumer.vscode-eslint```

Prettier | Rechercher ```esbenp.prettier-vscode```

il faut ensuite installer les paquets suivants :

avec yarn :

```yarn add eslint babel-eslint prettier-eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-react --dev```

ou npm : 

``` npm i eslint babel-eslint prettier-eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-react --save-dev ```

Pour voir ce que font les packages en details:

[eslint](https://github.com/eslint/eslint)

[babel-eslint](https://github.com/babel/babel-eslint)

[prettier-eslint](https://github.com/prettier/prettier-eslint)

[eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

[eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)

[eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
</details>

## Mise en place des fichier de configuration
<details>


Pour mettre en place les règle de linter et de prettier, il faudra mettre en place quelque fichier de configuration.

Dans un premier temps la mise en place d'eslint.

Elle se fait via le fichier ```.eslintrc.json``` que l'on place a la racine du projet.

.eslintrc.json : 

```json
{
  "parser": "babel-eslint",
  "plugins": ["prettier"],
  "extends": ["eslint:recommended", "plugin:react/recommended", "prettier"],
  "env": { "browser": true, "node": true, "jest": true },
  "rules": {
    "prettier/prettier": "error"
  }
}
```

la ligne ```"prettier/prettier": "error"``` indique que les règles de linter seront celles de prettier qui se trouveront dans le fichier ```.prettierrc```, toujours a la racine du projet.

.prettierrc :

```json
{
  "arrowParens": "avoid",
  "bracketSpacing": true,
  "endOfLine": "auto",
  "htmlWhitespaceSensitivity": "css",
  "insertPragma": false,
  "jsxBracketSameLine": false,
  "jsxSingleQuote": true,
  "printWidth": 80,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "useTabs": false,
  "parser": "babel"
}
```

Nous avons donc un projtet linté, cependant on aimerai que les corrrection se fassent a la sauvegarde, pour cela, nous allons creer un fichier de configuration pour vscode.

Creer a la racine du projet un dossier ```.vscode```

Dans ce dossier créer un fichier nommé ```settings.json```

settings.json:
```json
{
    // does not render single space between words
    "editor.renderWhitespace": "boundary",
    // vertical rule after 80 charachter
    "editor.rulers": [80],
    // turns off validations that VSCode
    "javascript.validate.enable": false,
    // tab = 2 spaces
    "editor.tabSize": 2,
    // allow prettier to fix eslint 
    // "eslint.autoFixOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
  }
```

le fichier settings.json définit les régles vsCode pour le projet, très pratique lorsque l'on travail a plusieurs.

```json   
"editor.codeActionsOnSave": {
      "source.fixAll.eslint": true
    }
```
cette ligne permet l'application des régles du prettier/linter à chaque sauvegarde.

</details>
</details>


# Passer son application React en PWA
<details>
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

</details>



# Mise en place Notifications Push

 <details> 
Dans un premier temps il faut vérifier que notre navigateur soit compatible, pour cela nous allons créer une petite fonction dans notre dossier ```utils``` nommer le fichier ```isPushNotificationSupported.js```

```utils/isPushNotificationSupported.js```
```js
const isPushNotificationSupported = () => {
  return "serviceWorker" in navigator && "PushManager" in window;
}

export default isPushNotificationSupported

```

il faudra également demander la permission d'envoyer une notification a l'utilisateur, on va crée la fonction suivante dans le document ```notificationsPermissions.js```

```js
const askUserPermission = async () => {
  return await Notification.requestPermission();
}

export default askUserPermission

```

 </details> 

# Mise en place StoryBook
<details>
  <summary>Voir plus</summary>
Lancer la commande suivante 

```npx -p @storybook/cli sb init --type react```

il faut ensuite configurer storyBook pour lui dire d'aller chercher les story dans le dossier component.

on ouvre donc le fichier ```./storybook/config.js```

et on modifie 

```js
configure(require.context('../stories', true, /\.stories\.js$/), module);
```

par

```js
configure(require.context('../src/components', true, /\.stories\.js$/), module);
```

les composants dans le dossier ```components``` sont maintenant visible dans notre storybook.

## Travailler avec Styled Components

Nos composant utilisent un ThemeProvider, il faut émuler ce comportement au sein de son storyBook.

pour cela il faut créer un composant perso appelé ```ThemeDecorator```

dans ./storybook créer ```themeDecorator.js```
```js
import React from "react"
import { ThemeProvider } from "styled-components"

import theme from '../src/config/theme'

const ThemeDecorator = storyFn => (
  <ThemeProvider theme={theme}>{storyFn()}</ThemeProvider>
)

export default ThemeDecorator
```

on utilisera ce ThemeDecorator dans ```./storybook/config.js```

```js
import { configure, addDecorator } from "@storybook/react"
import themeDecorator from "./themeDecorator"

```

on appliquera ensuite notre ```decorator```.

```js
addDecorator(themeDecorator);
```
Votre StoryBook est desormais pret a être utiliser avec styled component

pour creer ses stories voir [la doc de storybook](https://storybook.js.org/docs/basics/writing-stories/)

</details>


# Mise en place Router
<details>
  <!-- <summary>Voir plus</summary> -->
  Lancer la commande

  ```yarn add react-router-dom```

  <!-- Pour notre exemple il faut au moins deux screen

  créer un screen nommé ```ranking.js``` -->

creer un fichier ```routes.js``` dans le dossier ```config```

pour notre exemple creer un fichier ```ranking.js``` dans ```screens```

(il nous faut au moins 2 screens pour le router)

On créera ensuite le fichier ```routes.js``` dans ```config```

``` js
import React from 'react';

import {
  Route,
  BrowserRouter as Router,
  Redirect,
  Switch
} from 'react-router-dom';
import PrivateRoute from '../utils/privateRoute';

import App from '../screens/app';
import Ranking from '../screens/ranking';

export default class Routes extends React.Component {
  render() {
    return (
      <Router>
        <Switch>
          <Route exact path="/" component={App}></Route>
          <PrivateRoute
            path="/ranking"
            component={Ranking}
          ></PrivateRoute>
          <Redirect to="/" />
        </Switch>
      </Router>
    );
  }
}
```

Pour faire fonctionner le routeur on aura besoin de deux fichier : ```isUserConnected.js``` et ```privateRoute.js``` dans le dossier ```utils```

```isUserConnected```

```js
const isUserConnected = () => {
  const isConnected = localStorage.getItem('token');
  return !!isConnected;
};

export default isUserConnected;
```


```privateRoute.js``` 

```js
import React from 'react';
import PropTypes from 'prop-types';
import isUserConnected from './isUserConnected';
import { Redirect, Route } from 'react-router-dom';

const PrivateRoute = ({ component: Component, ...rest }) => (
  <Route
    {...rest}
    render={props =>
      isUserConnected() === true ? (
        <Component {...props} />
      ) : (
        <Redirect
          to={{
            pathname: '/',
            state: { from: props.location }
          }}
        />
      )
    }
  />
);

PrivateRoute.propTypes = {
  component: PropTypes.any,
  location: PropTypes.any
};

// To give an accurate explanation, let's break down the { component: Component, ...rest } expression into two separate operations:

// Operation 1: Find the component property defined on props (Note: lowercase component) and assign it to a new location in state we call Component (Note: capital Component).
// Operation 2: Then, take all remaining properties defined on the props object and collect them inside an argument called rest.

export default PrivateRoute;
```

le Routeur est deseormais fonctionnel on l'importera dans ```index.js``` a la racine du projet

```index.js```

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import * as serviceWorker from './serviceWorker';
import {ThemeProvider} from 'styled-components';

import Routes from './config/routes';

import theme from './config/theme';

class Index extends React.Component{
  render(){
    return(
    <ThemeProvider theme={theme}>
       <Routes /> 
    </ThemeProvider>
    );
  }
}

ReactDOM.render(<Index></Index>, document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```
 // faire la mise en place de la connexion en live

</details>



## Mise en place des tests unitaires

<details>

```yarn add enzyme enzyme-adapter-react-16 enzyme-to-json```


pour faire fonctionner enzyme il faut creer ce fichier de parametre a la racine de ```./src```

setupTests.js
```js
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

Enzyme.configure({ adapter: new Adapter() });
```

ajouter dans ```package.json```
```json
  "jest": {
    "collectCoverageFrom": [
      "app/**/*js"
    ],
    "snapshotSerializers": [
      "enzyme-to-json/serializer"
    ]
  }
  ```

Enzyme nous permet de faire du shallow rendering (render sans les composants enfant)

pour faciliter la lecture des snapshot, on installe enzyme-to-json puis on le parametre dans le ```package.json```

on peux desormais tester les composant classique, pour faciliter les test des styled-components il faut installer le package suivant 

```yarn add jest-styled-components --dev```

ce composant règle certain soucis qui fausse le resultat des snapshot.

</details>
