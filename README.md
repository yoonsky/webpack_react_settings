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
    "start": "webpack serve --mode development",
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
