---
title: ESLint 配置
date: '2018-07-31'
tags: 
  - Wabpack
  - ESLint
---

```js
module.exports = {
    "env": {
        "browser": true,
        "es6": true,
        "node": true
    },
    "parser": "babel-eslint",
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module",
        "no-empty-function": {
            "allow": ["methods", "constructors"]
        },
    },
    "plugins": [
        "react",
        "babel"
    ],
    "rules": {
        "indent": [
            "error",
            2
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "semi": [
            "error",
            "never"
        ],
        "no-console": "off",
        "react/prop-types": 1
    }
};
```

+ babel-eslint
+ eslint 
+ eslint-plugin-babel 
+ eslint-plugin-react

```shell
"lint": "./node_modules/.bin/eslint --ext .js,.jsx frontend/",
"lint:table": "npm run lint -- -f table",
"lint:code": "npm run lint -- -f codeframe",
"lint:fix": "npm run lint -- --fix"
```

+ <https://eslint.org/>
+ <http://eslint.cn/>
+ <http://www.alloyteam.com/2017/08/13065/>
+ <https://github.com/yannickcr/eslint-plugin-react>