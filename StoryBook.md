# Mise en place StoryBook

Lancer la commande suivante 

```npx -p @storybook/cli sb init --type react```

il faut ensuite configurer storyBook pour lui dire d'aller chercher les story dans le dossier components.

on ouvre donc le fichier ```./storybook/config.js```

et on modifie 

```js
stories: ['../stories/**/*.stories.js'],
```

par

```js
  stories: ['../src/components/**/*.stories.js'],
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


