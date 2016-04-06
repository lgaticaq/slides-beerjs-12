title: Beerjs #12
author:
  name: Leonardo Gatica
  twitter: lgaticaq
  url: https://about.me/lgatica
  email: lgatica@protonmail.com
output: index.html
style: style.css
controls: false

--

# es6 + babel + eslint + webpack + npm scripts

--

<img src="img/nodejs.png" width="50%">

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
echo "v5" > .nvmrc
nvm install v5
nvm use
```

--

<img src="img/npm.svg" width="50%">

```bash
npm config set init.author.name "Leonardo Gatica"
npm config set init.author.email lgatica@protonmail.com
npm config set init.author.url https://about.me/lgatica
npm config set init.license MIT
npm config set init.version 1.0.0
echo -e "node_modules\nsrc\n.*" > .npmignore
```

--

```bash
npm init -y
```

```json
{
  "name": "slides-beerjs-12",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/lgaticaq/slides-beerjs-12.git"
  },
  "keywords": [],
  "author": "Leonardo Gatica <lgatica@protonmail.com> (https://about.me/lgatica)",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/lgaticaq/slides-beerjs-12/issues"
  },
  "homepage": "https://github.com/lgaticaq/slides-beerjs-12#readme"
}
```

--

<img src="img/editorconfig.png">

```bash
npm i -g editorconfig-cli
ec init
```

--

```ini
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = true
```

--

<img src="img/eslint.svg" width="50%">

```bash
npm i -D eslint
./node_modules/.bin/eslint --init
```

--

```json
{
  "env": {
    "es6": true,
    "node": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "sourceType": "module"
  },
  "rules": {
    "indent": ["error", 4],
    "linebreak-style": ["error", "unix"],
    "quotes": ["error", "single"],
    "semi": ["error", "always"]
  }
}
```

--

<img src="img/babel.png" width="50%">

```bash
npm i -D babel-core babel-preset-es2015 babel-loader babel-cli
echo '{ "presets": ["es2015"] }' > .babelrc
```

--

<img src="img/webpack.png" width="50%">

```bash
npm i -D webpack
touch webpack.config.js
```

--

```javascript
const path = require('path');
const webpack = require('webpack');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: [path.join(__dirname, 'client', 'index.js')],
  output: {path: path.join(__dirname, 'build'), filename: 'bundle.js'},
  plugins: [
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify(process.env.NODE_ENV || 'development'),
      }
    }),
    new ExtractTextPlugin('style.css')
  ],
  module: {
    loaders: [
      {test: /\.js$/, exclude: /node_modules/, loader: 'babel'},
      {test: /\.css$/, loader: ExtractTextPlugin.extract('style', 'css')},
      {test: /\.(ttf|eot|svg|png|jpg|woff(2)?)$/, loader: 'url'}
    ]
  }
};
```

--

<img src="img/npm-scripts.png" width="50%">

```bash
npm i -D parallelshell
npm run dev
```

--

```json
"start": "node server/index.js",
"start:dev": "nodemon --debug --exec npm run babel-node -- server/index.js",
"babel-node": "babel-node --presets=es2015"
"lint:server": "eslint server",
"lint:client": "eslint client",
"lint": "eslint server client",
"clean:server": "rimraf dist",
"clean:client": "rimraf build/*",
"prebuild:server": "npm run lint:server -s && npm run clean:server -s",
"build:server": "babel src --out-dir dist --source-maps",
"prebuild:client": "npm run lint:client -s && npm run clean:client -s",
"build:client": "webpack -d",
"prebuild:client:dist": "npm run clean:client -s",
"build:client:dist": "webpack -p",
"build": "parallelshell 'npm run build:server -s' 'npm run build:client -s'"
"build:dist": "parallelshell 'npm run build:server -s' 'npm run build:client:dist -s'"
"watch": "webpack -w",
"pretest": "npm run lint",
"test": "mocha --compilers js:babel-core/register",
"dev": "parallelshell 'npm run start:dev -s' 'npm run watch -s' 'node-inspector'"
```
