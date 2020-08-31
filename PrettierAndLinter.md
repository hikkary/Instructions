
# Mise en place d'un linter et d'un Prettier


## Installation


Premierement il faut installer les extensions Vscode suivante : 

Eslint | Rechercher ```dbaeumer.vscode-eslint```

Prettier | Rechercher ```esbenp.prettier-vscode```

il faut ensuite installer les paquets suivants :

avec yarn :

```yarn add babel-eslint prettier-eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-react --dev```

ou npm : 

``` npm i babel-eslint prettier-eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-react --save-dev ```

Pour voir ce que font les packages en details:

[eslint](https://github.com/eslint/eslint)

[babel-eslint](https://github.com/babel/babel-eslint)

[prettier-eslint](https://github.com/prettier/prettier-eslint)

[eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

[eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier)

[eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
</details>

## Mise en place des fichier de configuration


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


