# Learn-Webpack [阅读中文](https://github.com/SilenceHVK/articles/issues/20)

## About Webpack 
Webpack is module loader developed by German developer Tobias Koppers.

All files in webpack will be used as modules. When webpack processes an application, it recursively builds a dependency graph that contains every module the application needs, and then packages all those modules into one or more bundles.

![What can webpack do?](http://images.cnblogs.com/cnblogs_com/hvkcode/966655/o_what-is-webpack.png)

## Contrast with Gulp/Grunt
Webpack is not comparable to Gupl/Grunt because Gupl/Grunt is a tool that optimizes the front-end development process and webpack is a modular solution. However, the advantages of webpack make webpack in many scenarios can replace Gulp/Grunt category of tools.

Grunt and Gulp work in the following ways: In a configuration file,you specify the specific steps to perform similar tasks such as compiling, assembling, and compressing certain files taht the tool can then automate for you.

![Grunt and Gulp work charts](http://images.cnblogs.com/cnblogs_com/hvkcode/966655/o_gulp-grunt.png)

Webpack work in the following ways: Think of your project as a whole, and through a given master file (eg,index.js), Webpack will find all the dependencies for your project from this file, process them with loaders, and finally package it into on (or more) browser-aware JavaScript file.

![webpack work charts](http://images.cnblogs.com/cnblogs_com/hvkcode/966655/o_1031000-160bc667d3b6093a.png)

## webpack installation and use （[Demo1 Source](https://github.com/SilenceHVK/learn-webpack/tree/master/demo1)）

1. Install [webpack](https://www.npmjs.com/package/webpack) golbally via npm.
```bash
    $ npm install -g webpack
```

2. Create the project and initialize the package.json file.
```bash
    $ mkdir demo1 && cd demo1
    $ npm init
```

3. Install webpack in the project.
```bash
    $ npm install webpack --save-dev
```
> --save-dev is dependent on the development time, - save is also dependent on the release of things.

4. Create the following file structure in the project.
<pre>
.        
├── index.html  // Web page display
├── main.js    // Webpack entrance
└── bundle.js // Files generated by the webpack command do not need to be created
</pre>


5. Package the js files dependent on the project with the command.
```bash
    # webpack Js file name to be packaged. Generated after the package js file name.
    $ webpack main.js bundle.js
```

Following the webpackage command can also add the following parameters.
- ``` --watch ``` Real-time packaging.
- ``` --progress ``` Display package progress.
- ``` --display-modules ``` Display the packaged modules.
- ``` --display-reasons ``` Display reasons about module inclusion in the  output.

More parameters can be viewd by the command ``` webpack --help ``` .

## Four core cocnepts in webpack（[Demo2 Source](https://github.com/SilenceHVK/learn-webpack/tree/master/demo2)）
1. [Entry](#user-content-entry)
2. [Output](#user-content-output)
3. [Loaders](#user-content-loaders)
4. [Plugins](#user-content-plugins)

The default configuration file name in webpack is webpack.config.json,so we need to create the following file in the project:
<pre>
.        
├── index.html            // Web page display
├── main.js              // Webpack entrance
├── webpack.config.js   // The default configuration file in webpack
└── bundle.js          //  Files generated by the webpack command do not need to be created
</pre>

### Entry
The entry point indicates which module webpack should use as the start of building its internal dependency graph.

You can configure the entry property in webpack.config.js to specify an entry of multiple entry points as follows:
```javascript
    module.exports = {
        entry: './path/entryFile.js'
    };
```

### Output
The output property tells webpack where to emit the bundles it creates and how to name these files. You can configure this part of the process by specifying an output field in your configuration:
```javascript
    const path = require('path');

    module.exports = {
        entry: './path/entryFile.js',
        output: {
            path: path.resolve(__dirname,'dist'),
            filename: 'my-webpack.bundle.js'
        }
    };
```
The output.path property is used to specify the path to the generated file, and output.filename is used to specify the name of the generated file.

### Loaders
Loaders enable webpack to process more than just JavaScript files (webpack itself only understands JavaScript). They give you the ability to leverage webpack's bundling capabilities for all kinds of files by converting them to valid modules that webpack can process.

At a high level, loaders have two purposes in your webpack config. They work to:
1. Identify which file or files should be transformed by a certain loader (with the test property).
2. Transform those files so that they can be added to your dependency graph (and eventually your bundle). (use property)

Before we start the following code,we need to install the [style-loader](https://www.npmjs.com/package/style-loader) and [css-loader](https://www.npmjs.com/package/css-loader).
```bash
    $ npm install --save-dev style-loader css-loader
```

And Create the style.css style file in the project:
```css
    h1{ color: red; }
```

Then enter the following code in webpack.config.js
```javascript
    const path = require('path');

    module.exports = {
        entry: "./main.js",
        output: {
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js'
        },
        model: {
           rules: [
               {
                   test: /\.css$/,
                   use:[
                       { loader: 'style-loader' },
                       { loader: 'css-loader' }
                   ]
               }
           ]
        }
    }
```

### Plugins
While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks. Plugins range from bundle optimization and minification all the way to defining environment-like variables. The plugin interface is extremely powerful and can be used to tackle a wide variety of tasks.

In order to use a plugin, you need to require() it and add it to the plugins array. Most plugins are customizable through options. Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with the new operator.

Before we start the following code,we need to install the [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin) ：
```bash
    $ npm install html-webpack-plugin --save-dev
```
This is a webpack plugin that simplifies creation of HTML files to serve your webpack bundles.

Then enter the following code in webpack.config.js.
```javascript
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    const path = require('path');

    const config = {
        entry: './main.js',
        output: {
            path: path.resolve(__dirname,'dist'),
            filename: 'bundle.js'
        },
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: [
                        { loader: 'style-loader'},
                        { loader: 'css-loader' }
                    ]
                }
            ]
        },
        plugins: [
            new HtmlWebpackPlugin({ template: './index.html' })
        ]
    };

    module.exports = config;
```

### Run and configure
Finally,we can compile and pack directly through the webpack command.If you want to add parameters after its command, you can configure script properties in package.json.
```json
    {
        scripts: {
            "build": "webpack --config webpack.config.js --progress --display-modules"
        }
    }
```

Of course,if you want to change the default configuration file name, --config webpack.config.js configuration file name changed to your custom name.

TThrough the following command executio:
```bash
    $ npm run build
```


