
# Mise en place Router | Système de routes protégées
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
