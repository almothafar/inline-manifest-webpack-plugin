[![CircleCI](https://circleci.com/gh/almothafar/webpack-inline-manifest-plugin/tree/master.svg?style=shield)](https://circleci.com/gh/almothafar/webpack-inline-manifest-plugin/tree/master) [![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com) [![npm](https://img.shields.io/npm/dt/webpack-inline-manifest-plugin.svg)](https://www.npmjs.com/package/webpack-inline-manifest-plugin)  [![npm](https://img.shields.io/npm/v/webpack-inline-manifest-plugin.svg)](https://www.npmjs.com/package/webpack-inline-manifest-plugin) [![npm](https://img.shields.io/npm/l/webpack-inline-manifest-plugin.svg)](https://www.npmjs.com/package/webpack-inline-manifest-plugin)

Webpack Inline Manifest Plugin
===================

This is a [webpack](http://webpack.github.io/) plugin that inline your manifest.js with a script tag to save http request. Cause webpack's runtime always change between every build, it's better to split the runtime code out for long-term caching.


Installation
------------
Install the plugin with npm:
```shell
$ npm i webpack-inline-manifest-plugin -D
```

Basic Usage
-----------

This plugin need to work with [webpack v4+](https://github.com/webpack/webpack) and [HtmlWebpackPlugin v3](https://www.npmjs.com/package/html-webpack-plugin):

>*For webpack v3 and below, please use [version 4.0.0](https://github.com/almothafar/webpack-inline-manifest-plugin/releases/tag/v4.0.0) or original library [inline-manifest-webpack-plugin](https://github.com/szrenwei/inline-manifest-webpack-plugin/releases/tag/v3.0.1)*

__Step1__: split out the runtime code
```javascript
// the default name is "runtime"
optimization: {
    runtimeChunk: 'single'
}

// or specify another name
optimization: {
    name: 'webpackManifest'
}
```
__Step2__: add and config [HtmlWebpackPlugin](https://github.com/jantimon/html-webpack-plugin):
```javascript
[
    // https://github.com/jantimon/html-webpack-plugin
    new HtmlWebpackPlugin()
]
```

__Step3__: config WebpackInlineManifestPlugin, you need to add this right after `HtmlWebpackPlugin`
* __name__: default value is `webpackManifest`,  result in `htmlWebpackPlugin.files[name]`, you can specify any other name __except__ `manifest`, beacuse the name `manifest` haved been used by HtmlWebpackPlugin for H5 app cache manifest.

Call:

```javascript
const WebpackInlineManifestPlugin = require('webpack-inline-manifest-plugin');
```

Config:
```javascript
[
    new HtmlWebpackPlugin(),
    new WebpackInlineManifestPlugin({
        name: 'webpackManifest' // must be the same value of runtimeChunk's name
    })
]
```

Finally in HTML:
```html
<!-- index.ejs -->
<!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>App</title>
    </head>
    <body>
    
    <%=htmlWebpackPlugin.files.webpackManifest%>
    
    </body>
</html>
```
__Done!__ This will replace the external script with inline code.
