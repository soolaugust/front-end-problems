* 1. [使用方法](#)
	* 1.1. [config配置示例](#config)
* 2. [场景](#-1)
	* 2.1. [使用webpack打包成一个html](#webpackhtml)
		* 2.1.1. [下载插件](#-1)
		* 2.1.2. [配置](#-1)
		* 2.1.3. [常见问题](#-1)
	* 2.2. [需要将css编入js中](#cssjs)
		* 2.2.1. [下载插件](#-1)
		* 2.2.2. [配置](#-1)
		* 2.2.3. [常见问题](#-1)
	* 2.3. [需要将原生html中的引用图片全部编进最终的html中](#htmlhtml)

# 打包

> *以下所有示例的js版本都是ES6+，如果使用的ES5，则需要相应的替换，例如将const换成var*

webpack主要用于对JavaScript进行打包。

##  1. <a name=''></a>使用方法

```sh
webpack --config webpack.***.js

webpack (会寻找配置文件webpack.config.js)
```

> webpack.***.js 为相应的配置文件，具体可参考 [配置-webpack中文文档](https://www.webpackjs.com/concepts/configuration/#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)

###  1.1. <a name='config'></a>config配置示例

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

##  2. <a name='-1'></a>场景

###  2.1. <a name='webpackhtml'></a>使用webpack打包成一个html

webpack主要是针对javascript进行打包，所以默认情况下只会对javascript进行打包，如果需要打包成html，需要使用 [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin);

####  2.1.1. <a name='-1'></a>下载插件

```bash
  npm i --save-dev html-webpack-plugin
```

####  2.1.2. <a name='-1'></a>配置

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

####  2.1.3. <a name='-1'></a>常见问题

##### 自己已经有html文件了，需要js压缩后插入到html中

1. 只是需要在html中引入javascript作为链接

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

2. 需要将压缩后的javascript的全部内容直接编入html中

这时需要引入外部插件 [html-webpack-inline-source-plugin](https://github.com/DustinJackson/html-webpack-inline-source-plugin)

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

> 如果出现 **AssertionError [ERR_ASSERTION]: The HtmlWebpackInlineSourcePlugin does not accept any options** ， 说明你使用的是旧的版本， 使用最新版本或者将 *new HtmlWebpackInlineSourcePlugin(HtmlWebpackPlugin)* 转成 *new HtmlWebpackInlineSourcePlugin()*

###  2.2. <a name='cssjs'></a>需要将css编入js中

需要使用 [css-loader](https://github.com/webpack-contrib/css-loader)  和 [style-loader](https://github.com/webpack-contrib/style-loader)

####  2.2.1. <a name='-1'></a>下载插件

```bash
npm install style-loader --save-dev

npm install --save-dev css-loader
```

####  2.2.2. <a name='-1'></a>配置

Step 1: 在js中导入css

```javascript
const cssFile = require('demo.css');
```

Step 2: 在配置文件中加入css loader

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

####  2.2.3. <a name='-1'></a>常见问题

##### css中含有 *background: url(...)*

需要使用 [url-loader](https://github.com/webpack-contrib/url-loader)

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

###  2.3. <a name='htmlhtml'></a>需要将原生html中的引用图片全部编进最终的html中

有时候最终只需要一个html，这时需要将原先需要的图片都编入html中。这时需要配合使用 [html-loader](https://github.com/webpack-contrib/html-loader) 和 [url-loader](https://github.com/webpack-contrib/url-loader)

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