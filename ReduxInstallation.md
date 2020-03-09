# Mise en Place Redux (Compteur)

il faut tout d'abord installer les paquet ```react-redux``` et ```redux```

```npm install --save react-redux redux```

```yarn add react-redux redux```


Avant d'initialiser le store de redux, nous allons créer un fichier actions et reducers, pour mettre en place notre structure.

Nous allons créer un dossier ```actions``` et un dossier ```reducers```

je crée ensuite dans le dossier ```actions``` le fichier ```counter.js```

```js
export const INCREMENT_COUNTER = 'INCREMENT_COUNTER'
export const DECREMENT_COUNTER = 'DECREMENT_COUNTER'

export const incrementCounter = payload => ({
  type: INCREMENT_COUNTER,
  payload
})

export const decrementCounter = payload => ({
  type: DECREMENT_COUNTER,
  payload
})

```

je crée ensuite dans le dossierr ```actions``` un fichier ```index.js```

```js
import * as counter from './counter'

export default { theme }

```

Nous importerons tout les fichiers d'actions dans ce fichier, pour y avoir accès dans les composants.

Je crée ensuite dans le dossier ```reducers``` un fichier ```counter.js```

```js
import { INCREMENT_COUNTER, DECREMENT_COUNTER } from '../actions/counter'

const initialState = {
  counter: 0
}

export default (state = initialState, action) => {
  switch (action.type) {
    case INCREMENT_COUNTER:
      return {
        ...state,
        counter: state.counter + action.payload
      }
    case DECREMENT_COUNTER:
      return {
        ...state,
        counter: state.counter - action.payload
      }
    default:
      return state
  }
}
```

j'y importe les types definis dans le fichier ```actions/counter.js``` pour permettre au reducers d'effectuer les actions selon le type d'actions emis par le front.

le state que j'initialise ici sera celui auquel j'accederai lorsque je connecterai mon composant compteur.

je crée ensuite dans le dossier ```reducers``` un fichier ```index.js```

```js
import { combineReducers } from 'redux'

import counter from './counter'

export default combineReducers({
  counter
})

```

Tout comme le fichier ```index.js``` present dans le dossier ```actions``` j'importe tout mes reducers, on notera ici qu'on les exportent via combineReducers, une fonction donnée par redux qui permet de combiner tout nos petit reducers par un unique qui sera donné en paramètre lors de la creation de notre store.

on crée ensuite le fichier store ```config/store.js```

```js
import { createStore } from 'redux'

import reducers from '../reducers'

export const store = createStore(reducers)
```

Notre configuration est terminé, il faut désormais connecté nos composant.

Nous créerons une page que nous connecterons a ```redux``` via la fonction ```connect``` de ```react-redux```  

```js

import React from 'react'

import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'
import styled from 'styled-components'

import Counter from '../components/counter'

import allTheActions from '../actions'

const Timers = props => {
  return (
    <Container>
      <Counter
        counter={props.counterState.counter}
        incrementCounter={() => props.actions.counter.incrementCounter(3)}
        decrementCounter={() => props.actions.counter.decrementCounter(4)}
        label='gryffondor'
      ></Counter>
    </Container>
  )
}
const Container = styled.div`
  background-color: ${props => props.theme.primary};
`

const mapStateToProps = state => ({
  counterState: state.counter
})

const mapDispatchToProps = () => dispatch => ({
  actions: {
    counter: bindActionCreators(allTheActions.counter, dispatch),
    theme: bindActionCreators(allTheActions.theme, dispatch)
  }
})

export default connect(mapStateToProps, mapDispatchToProps)(Timers)

```

plusieurs point a noter ici :

- La fonction connect prend deux paramètres, la premiere est une fonction qui va faire passer la state de redux dans les props de notre page ```Timers``` (mapStateToProps), la seconde va créer des fonctions qui permettrons de déclancher nos actions (mapDispatchProps), et serons egalement passer en props de notre page
- La syntaxe peux paraitre singulière, pour ```mapStateToProps``` la state sera injecté par le ```connect``` elle ne vient pas de nulle part, pareil pour ```mapDispatchToProps```, il s'agit d'une fonction qui renvoi une fonction qui prend ```dispatch``` en paramétre, encore une fois le ```dispatch``` ne vient pas de nulle part, dispatch est la fonction qui permet a redux d'emettre des actions, couplé au ```bindActionsCreators``` react-redux nous crée des fonctions prêtes a l'emploi pour emettre nos actions. dans une utilisation classique de redux nous aurions du importer l'actions dans notre composant, ainsi que le ```store``` puis pour emettre l'action lancer ```store.dispatch(monAction)```, il faut comprendre que le ```mapDispatchToProps``` nous simplifie l'emission d'actions et permet de garder une syntaxe normale, on invoquera tout simplement nos fonctions envoyer dans les props (Les fonctions seront envoyés dans l'objet ```actions``` que nous avons défini dans le ```mapDispatchToProps```).
