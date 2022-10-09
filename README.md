# Webpack 5 boilerplate (Update 25.01.2021) 

## Easy way
1. Clone repository with command ```git clone https://github.com/bencubin/webpacking.git```
2. Run command ```npm i```
3. Your environment is ready

---

## Long way (detailed steps)

[1. Installing and configuring Webpack](#1-installing-and-configuring-webpack)

[2. Configuring Package.json file](#2-configuring-packagejson-file)

[3. Cleaning dist plugin](#3-cleaning-dist-plugin)

[4. Copy files to the build folder](#4-copy-files-to-the-build-folder)

[5. Server for development](#5-server-for-development)

[6. Minify/minimize your JavaScript](#6-minifyminimize-your-javascript)

[7. Control if and how source maps are generated](#7-control-if-and-how-source-maps-are-generated)

[8. Work with HTML](#8-work-with-html)

[9. Work with styles](#9-work-with-styles)

[10. Normalize CSS](#10-normalize-css)

[11. Optimize CSS](#11-optimize-css)

[12. Compile Less to CSS](#12-compile-less-to-css)

[13. Load a Sass/SCSS file and compile it to CSS](#13-load-a-sassscss-file-and-compile-it-to-css)

[14. Work with files](#14-work-with-files)

[15. Work with jQuery](#15-work-with-jquery)

[16. Use Babel](#16-use-babel)

[17. TypeScript](#17-typescript)

[18. ESLint](#18-eslint)

---

### 1. Installing and configuring Webpack 
[webpack concepts](https://webpack.js.org/concepts/)
```javascript
npm i -D webpack webpack-cli cross-env

// create file webpack.config.js
const path = require('path');

const isDev = process.env.NODE_ENV === 'development';
const isProd = !isDev;

const optimization = () => {
  const config = {
    splitChunks: {
      chunks: 'all',
    },
  };

  if (isProd) {
    config.minimizer = [
      new OptimizeCssAssetWebpackPlugin(), 
      new TerserWebpackPlugin()
    ];
  }

  return config;
};

const filename = ext => isDev ? `[name].${ext}` : `[name].[hash].${ext}`

// in webpack.config.js
module.exports = {  
  entry: {
    main: './src/index.js', // can be multiple files
  },
  output: {
    filename: filename('js'),
    path: path.resolve(__dirname, 'dist'),
  },
optimization: optimization(),
};
```
### 2. Configuring Package.json file
```javascript
// if a non-public package is being developed, the line
"main": "index.js",

// change to
"private": true,

// add scripts
"scripts": {
    "dev": "cross-env NODE_ENV=development webpack --mode development",
    "build": "cross-env NODE_ENV=production webpack --mode production",
    "watch": "cross-env NODE_ENV=development webpack --mode development --watch"
  },
  ```
### 3. Cleaning dist plugin
[clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin)
```javascript
npm i -D clean-webpack-plugin

// connection in webpack.config.js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

// config in webpack.config.js
module.exports = {  
  plugins: [    
    new CleanWebpackPlugin(),
  ],
};
```
### 4. Copy files to the build folder
[copy-webpack-plugin](https://webpack.js.org/plugins/copy-webpack-plugin/) 
```javascript
npm i -D copy-webpack-plugin

// connection in webpack.config.js
const CopyWebpackPlugin = require('copy-webpack-plugin');

// config in webpack.config.js
new CopyWebpackPlugin({
      patterns: [
        {
          from: path.resolve(__dirname, 'src/favicon.ico'), // example
          to: path.resolve(__dirname, 'dist'),
        },
      ],
    }),
```
### 5. Server for development
[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)
```javascript
npm i -D webpack-dev-server

// config in webpack.config.js
devServer: {
    port: 4200,
  },

"scripts": {    
    "start": "cross-env NODE_ENV=development webpack serve --mode development --open"
  },
```
### 6. Minify/minimize your JavaScript
[terser-webpack-plugin](https://webpack.js.org/plugins/terser-webpack-plugin/)
```javascript
npm i -D terser-webpack-plugin

// connection in webpack.config.js
const TerserWebpackPlugin = require('terser-webpack-plugin');
```
### 7. Control if and how source maps are generated 
[devtool](https://webpack.js.org/configuration/devtool/)
```javascript
module.exports = { 
  devtool: isDev ? 'source-map' : 'eval',
}
```
### 8. Work with HTML
[html-webpack-plugin](https://webpack.js.org/plugins/html-webpack-plugin/)
```javascript
npm i -D html-webpack-plugin

// connection in webpack.config.js
const HTMLWebpackPlugin = require('html-webpack-plugin');

// config in webpack.config.js
module.exports = {  
  plugins: [
    new HTMLWebpackPlugin({
      template: './src/index.html',
			minify: {
	      collapseWhitespace: isProd,
      }
    }),
  ],
};
```
### 9. Work with styles
[style-loader](https://webpack.js.org/loaders/style-loader/)
[css-loader](https://webpack.js.org/loaders/css-loader/)
[mini-css-extract-plugin](https://webpack.js.org/plugins/mini-css-extract-plugin/)
```javascript
npm i -D style-loader css-loader mini-css-extract-plugin

// connection in webpack.config.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

// config in webpack.config.js
module.exports = {  
	plugins: [    
	   new MiniCssExtractPlugin({
      filename: filename('css'),
    }),
	  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },     
    ],
  },
};
```
### 10. Normalize CSS
[normalize.css](https://necolas.github.io/normalize.css/)
```javascript
npm i normalize.css

// add the first line in the styles file
@import '~normalize.css';
```
### 11. Optimize CSS
[optimize-css-assets-webpack-plugin](https://www.npmjs.com/package/optimize-css-assets-webpack-plugin)
```javascript
npm i -D optimize-css-assets-webpack-plugin

// connection in webpack.config.js
const OptimizeCssAssetWebpackPlugin = require('optimize-css-assets-webpack-plugin');
```
### 12. Compile Less to CSS
[less-loader](https://webpack.js.org/loaders/less-loader/)
```javascript
npm i -D less-loader less

// config in  webpack.config.js
  module: {
    rules: [
      {
        test: /\.less$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'less-loader'],
      },     
    ],
  },
```
### 13. Load a Sass/SCSS file and compile it to CSS 
[sass-loader](https://www.npmjs.com/package/sass-loader)
```javascript
npm i -D sass-loader sass

// config in webpack.config.js
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'],
      },     
    ],
  },
```
### 14. Work with files
[Asset Modules](https://webpack.js.org/guides/asset-modules/)
```javascript
// config in webpack.config.js
module: {
    rules: [      
      {
        test: /\.(png|jpg|svg|gif|ttf|woff|woff2|eot|xml)$/,
        type: 'asset/resource',
      },
    ],
  },
```
### 15. Work with jQuery 
[jquery](https://www.npmjs.com/package/jquery)
```javascript
npm i -S jquery

// add import
import * as $ from 'jquery'
```
### 16. Use Babel
[babel](https://babeljs.io/setup#installation)
```javascript
npm i -D babel-loader @babel/core @babel/preset-env @babel/polyfill

// add in Package.json
"browserslist": "> 0.25%, not dead",

// config in webpack.config.js:
module.exports = {  
  entry: {
    main: ['@babel/polyfill', './src/index.js'],
  },  
  module: {
    rules: [     
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
          },
        },
      },
    ],
  },
};
```
### 17. TypeScript
[@babel/preset-typescript](https://babeljs.io/docs/en/babel-preset-typescript)
```javascript
npm i -D @babel/preset-typescript typescript @typescript-eslint/parser

// config in webpack.config.js
module.exports = {   
  module: {
    rules: [     
      {
        test: /\.ts$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env', '@babel/preset-typescript'],
          },
        },
      },
    ],
  },
};
```
### 18. ESLint
[@eslint/config](https://eslint.org/docs/latest/user-guide/getting-started)
```javascript
npm i -D eslint 
npx eslint --init
``` 

---
by [Bencubin](https://github.com/bencubin) in 2021