# webpack_react

웹팩으로 리액트 환경구성하기

### 1. 프로젝트 준비

```
yarn init -y
```

### 2. 리액트 라이브러리 설치

```
yarn add react react-dom
yarn add -D @types/react @types/react-dom
yarn add -D webpack webpack-cli webpack-dev-server html-webpack-plugin
yarn add -D babel-loader @babel/core @babel/preset-env @babel/preset-react
yarn add -D rimraf
```

### 3. package.json에 웹팩 동작을 위한 스크립트 추가

```
"scripts": {
    "start": "webpack serve --mode development --open",
    "prebuild":"rimraf dist",
    "build":"webpack --progress --mode production"
  },
```

### 4. 루트폴더에 webpack.config.js 파일 생성

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    'js/app': ['./src/App.jsx'],
  },
  output: {
    path: path.resolve(__dirname, 'dist/'),
    publicPath: '/',
  },
  module: {
    rules: [
      {
        test: /\.(js|tsx)$/,
        exclude: /node_modules/,
        use: ['babel-loader'],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      filename: 'index.html',
    }),
  ],
};
```

### 5. babel 설정을 위해 .babelrc를 생성한다.

```
{
  "presets": [
    [
      "@babel/preset-env",
      {"targets":{"browsers":["last 2 versions",">= 5% in KR"]}}
    ],
    "@babel/react"
  ]
}
```

### 6. webpack react를 typescript로 적용하기!

```
yarn add -D typescript @babel/preset-typescript ts-loader fork-ts-checker-webpack-plugin
```

```
yarn add -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
👇
npx eslint --init
👇
Installing eslint-plugin-react@latest, @typescript-eslint/eslint-plugin@latest, @typescript-eslint/parser@latest, eslint@latest
```

```
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```

```
Airbnb, other options..

yarn add -D eslint-config-airbnb eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-plugin-import
```

### 7. tsconfig.json 파일 작성

```
tsc --init
```

### 8 최종 설정 파일

_.babelrc_

```
{
  "presets": [
    [
      "@babel/preset-env",
      {"targets":{"browsers":["last 2 versions",">= 5% in KR"]}}
    ],
    "@babel/react",
    "@babel/typescript"
  ]
}
```

_.eslintrc.json_

```
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        // "plugin:react/recommended",
        "plugin:@typescript-eslint/eslint-recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@typescript-eslint/recommended-requiring-type-checking",
        "plugin:prettier/recommended",
        "prettier/@typescript-eslint"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        // "ecmaFeatures": {"jsx": true},
        "ecmaVersion": 2018,
        "sourceType": "module",
        "project":"./tsconfig.json"
    },
    "ignorePatterns": ["dist/","node_modules/"],
    "plugins": [
        "react",
        "@typescript-eslint"
    ],
    "rules": {
    }
}
```

_.prettierrc_

```
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": true,
  "semi": true,
  "singleQuote": true,
  "quoteProps": "consistent",
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "crlf"
}
```

_package.json_

```
{
  "name": "webpack_react",
  "version": "1.0.0",
  "main": "index.js",
  "author": "shimyuseob <shimyuseob@gmail.com>",
  "license": "MIT",
  "scripts": {
    "start": "webpack serve --mode development --open",
    "prebuild": "rimraf dist",
    "build": "webpack --progress --mode production"
  },
  "dependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  },
  "devDependencies": {
    "@babel/core": "^7.12.10",
    "@babel/preset-env": "^7.12.11",
    "@babel/preset-react": "^7.12.10",
    "@babel/preset-typescript": "^7.12.7",
    "@types/react": "^17.0.0",
    "@types/react-dom": "^17.0.0",
    "@typescript-eslint/eslint-plugin": "^4.12.0",
    "@typescript-eslint/parser": "^4.12.0",
    "babel-loader": "^8.2.2",
    "eslint": "^7.17.0",
    "eslint-config-prettier": "^7.1.0",
    "eslint-plugin-prettier": "^3.3.1",
    "eslint-plugin-react": "^7.22.0",
    "fork-ts-checker-webpack-plugin": "^6.0.8",
    "html-webpack-plugin": "^4.5.1",
    "prettier": "^2.2.1",
    "rimraf": "^3.0.2",
    "ts-loader": "^8.0.14",
    "typescript": "^4.1.3",
    "webpack": "^5.12.2",
    "webpack-cli": "^4.3.1",
    "webpack-dev-server": "^3.11.1"
  }
}

```

_tsconfig.json_

```
{
  "compilerOptions": {
    /* Basic Options */
    "target": "ES6",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "ESNext",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    "allowJs": true,                       /* Allow javascript files to be compiled. */
    "jsx": "react",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    "sourceMap": true,                     /* Generates corresponding '.map' file. */

    /* Strict Type-Checking Options */
    "strict": true,                           /* Enable all strict type-checking options. */

    /* Module Resolution Options */
    "moduleResolution": "node",                    /* List of root folders whose combined content represents the structure of the project at runtime. */
    "typeRoots": ["node_modules/@types","src/@types"],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true,

    /* Advanced Options */
    "skipLibCheck": true,                     /* Skip type checking of declaration files. */
    "forceConsistentCasingInFileNames": true  /* Disallow inconsistently-cased references to the same file. */
  },
  "exclude": [
    "node_modules",
    "build",
    "scripts",
    "acceptance-tests",
    "webpack",
    "jest",
    "src/setupTests.ts",
    "./node_moduels/**/*"
  ],
  "include": ["./src/**/*","@type"]
}
```

_webpack.config.js_

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ForkTsCheckerWebpackPlugin = require('fork-ts-checker-webpack-plugin');

module.exports = {
  entry: {
    'js/app': ['./src/App.tsx'],
  },
  output: {
    path: path.resolve(__dirname, 'dist/'),
    publicPath: '/',
  },
  module: {
    rules: [
      {
        test: /\.(ts|tsx)$/,
        use: [
          'babel-loader',
          {
            loader: 'ts-loader',
            options: {
              transpileOnly: true,
            },
          },
        ],
        exclude: /node_modules/,
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      filename: 'index.html',
    }),
    new ForkTsCheckerWebpackPlugin({ silent: true }),
  ],
};
```
