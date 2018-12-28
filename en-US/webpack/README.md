# webpack

> *All codes are based on ES6+, if you are using ES5, please try to replace them, e.g. replace const with var*

**webpack** is a static module bundler for modern Javascript applications.

* 1. [How to use](#Howtouse)
	* 1.1. [ Example of webpack](#Exampleofwebpack)
* 2. [ Scenario](#Scenario)
	* 2.1. [serve webpack bundles into html file](#servewebpackbundlesintohtmlfile)
		* 2.1.1. [Pre-requires](#Pre-requires)
		* 2.1.2. [Config file](#Configfile)
		* 2.1.3. [Q & A](#QA)
	* 2.2. [ serve css file into bundle.](#servecssfileintobundle.)
		* 2.2.1. [ Pre-requires](#Pre-requires-1)
		* 2.2.2. [Config files](#Configfiles)
		* 2.2.3. [ Q & A](#QA-1)
	* 2.3. [ I want to serve all images linked in html into html.](#Iwanttoserveallimageslinkedinhtmlintohtml.)

##  1. <a name='Howtouse'></a>How to use

```sh
webpack --config webpack.***.js

webpack //will find default config file:webpack.config.js
```

> webpack.***.js is the config file of webpack，reference: [Configuration - Webpack](https://webpack.js.org/concepts/configuration/)

###  1.1. <a name='Exampleofwebpack'></a> Example of webpack

```javascript
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js'
  }
};
```

##  2. <a name='Scenario'></a> Scenario

###  2.1. <a name='servewebpackbundlesintohtmlfile'></a>serve webpack bundles into html file

As webpack serve Javascript default, so if we want to serve webpack bundles into html file, we may need external plugin: [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin);

####  2.1.1. <a name='Pre-requires'></a>Pre-requires

```bash
  npm i --save-dev html-webpack-plugin
```

####  2.1.2. <a name='Configfile'></a>Config file

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin()
  ]
}
```

####  2.1.3. <a name='QA'></a>Q & A

##### I already have a html file, I want to serve bundles into this html file.

1. Only need link output js files in html file.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'index.html',
    })
  ]
}
```

2. import generated js codes into html file.

At this time, we need another plugin: [html-webpack-inline-source-plugin](https://github.com/DustinJackson/html-webpack-inline-source-plugin)

``` javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const HtmlWebpackInlineSourcePlugin = require('html-webpack-inline-source-plugin');


module.exports = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
        inlineSource: '.(js)$',
        template: 'index.html',
    }),
    new HtmlWebpackInlineSourcePlugin(HtmlWebpackPlugin),
  ]
}
```

> If meet this error: **AssertionError [ERR_ASSERTION]: The HtmlWebpackInlineSourcePlugin does not accept any options** ， That means you are using a older version, try to use newer version or change *new HtmlWebpackInlineSourcePlugin(HtmlWebpackPlugin)* to *new HtmlWebpackInlineSourcePlugin()*

###  2.2. <a name='servecssfileintobundle.'></a> serve css file into bundle.

We need these plugin: [css-loader](https://github.com/webpack-contrib/css-loader)  & [style-loader](https://github.com/webpack-contrib/style-loader)

####  2.2.1. <a name='Pre-requires-1'></a> Pre-requires

```bash
npm install style-loader --save-dev

npm install --save-dev css-loader
```

####  2.2.2. <a name='Configfiles'></a>Config files

Step 1: import css file in entry js file.

```javascript
const cssFile = require('demo.css');
```

Step 2: add css loader in webpack config file.

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

####  2.2.3. <a name='QA-1'></a> Q & A

##### *background: url(...)* in css file.

We need this loader: [url-loader](https://github.com/webpack-contrib/url-loader)

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/i,
        use: ['url-loader']
      }
    ]
  }
}
```

###  2.3. <a name='Iwanttoserveallimageslinkedinhtmlintohtml.'></a> I want to serve all images linked in html into html.

As some times, we only need a html finally, so we may need to serve all images into generated html, then we need use these loader: [html-loader](https://github.com/webpack-contrib/html-loader) & [url-loader](https://github.com/webpack-contrib/url-loader)

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/i,
        use: ['url-loader']
      }, {
         test: /\.(html)$/,
          use: ['html-loader']
    ]
  }
}
```