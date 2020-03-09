



## Mise en place des tests unitaires

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

