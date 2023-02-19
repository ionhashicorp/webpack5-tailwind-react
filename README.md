# webpack5-tailwind-react

```
npm init -y
```

- babel dependencies
```
npm install --save-dev @babel/core babel-loader @babel/cli @babel/preset-env @babel/preset-react
```

- webpack dependencies
```
npm install --save-dev webpack webpack-cli webpack-dev-server
```

- install webpack plugin
```
npm install --save-dev html-webpack-plugin
```

- react dependencies
```
npm install react react-dom
```

## public and src dirs
- create dirs
```
mkdir public src
```

- create html file in public dir
```
touch public/index.html
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

- in src dir create App.js
```
touch src/App.js
```
```js
// src/App.js
import React from "react";

const App = () =>{
    return (
        <h1>
            Hello world! I am using React
        </h1>
    )
}

export default App
```

- create `index.js` in root directory (or, also fine in src)
```
touch index.js
```
```js
// index.js
import React from 'react'
import  { createRoot }  from 'react-dom/client';
import App from './src/App.js'

const container = document.getElementById('root');
const root = createRoot(container);
root.render(<App/>);
```

## Babel config
- create babel config file in root dir
```
touch .babelrc
```
```js
{
    "presets": ["@babel/preset-env","@babel/preset-react"]
}
```

## Webpack5 config
- create `webpack.config.js`
```
touch webpack.config.js
```
```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: './index.js',
  mode: 'development',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js',
  },
  target: 'web',
  devServer: {
    port: '5000',
    static: {
      directory: path.join(__dirname, 'public')
},
    open: true,
    hot: true,
    liveReload: true,
  },
  resolve: {
    extensions: ['.js', '.jsx', '.json'],
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/, 
        exclude: /node_modules/, 
        use: 'babel-loader', 
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, 'public', 'index.html')
    })
  ]
};
```

- add scripts in `package.json`
```js
"scripts": {
    "start": "webpack-dev-server .",
    "build": "webpack ."
  }
```

## Start the development server
- run npm start
```
npm run start
```

- if this works go to the next step



## Tailwind
- install tailwindcss + dependencies
```
npm install tailwindcss autoprefixer --save-dev
```

- create `tailwind.config.cjs`
```
touch tailwind.config.cjs
```
```js
module.exports = {
    content: ['./src/**/*.{js,jsx}', './public/index.html'],
    theme: {
        extend: {
            colors: {
                primary: '#1B73E8',
            },
        },
    },
    plugins: [],
};
```

## PostCSS
- install postcss + dependencies
```
npm install postcss --save-dev
```

- config postcss
```
touch postcss.config.cjs
```
```js
const tailwindcss = require('tailwindcss');
const autoprefixer = require('autoprefixer');

module.exports = {
    plugins: [tailwindcss('./tailwind.config.cjs'), autoprefixer],
};
```

## Inject tailwindCSS into CSS file
- create css file in `src`
```
touch src/app.css
```
```js
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- import the `src/app.css` file into `src/App.js` file, like this:
```js
import React from 'react';
import './app.css'; //added line

function App() {
    return <h1>Hello world! I am using React</h1>;
}

export default App;
```

## Webpack process CSS
- install webpack dependencies
```
npm install style-loader css-loader postcss-loader
```

- add new rule in `webpack.config.js`
```js
{
  test: /\.css$/i,
  use: ['style-loader', 'css-loader', 'postcss-loader'],
},
```

## Style react compoents with Tailwind
- use this in `src/App.js`
```js
// src/App.js
import React from 'react';
import './app.css'; //load tailwindcss

function App() {
    return <h1 className="text-primary text-4xl font-bold">Hello world! I am using React</h1>;
}

export default App;
```

## Start the development server again
- run npm start
```
npm run start
```

## Delete Project
- remove dirs
```
rm -rf node_modules public src
```

- remove files
```
rm -rf .babelrc index.js package.json package-lock.json \
        webpack.config.js postcss.config.cjs tailwind.config.cjs
```
