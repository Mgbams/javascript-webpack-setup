# Setting up ES6 to use Babel and Webpack
> We are going to setup an environment for developement of native javascript application
using webpack and Babel

## PREREQUISITE

* Node JS
* Vs Code

Download the above applications from the sites below
[Node JS](https://nodejs.org/en/download/)
[Vs Code](https://code.visualstudio.com/Download)

Install them after downloading

Create a project on your computer and open the project in Vs Code.
Open a terminal in Vs Code and initialize the project

#### Initialization

```npm 
$ npm init
```
Next we are going to install babel transpiler then configure it for webpack module bundler. 
This will add sass preprocessor and browser-sync hot reloader for sync-loading of our web pages

### Install babel preprocessor with the below code

```
$ npm install @babel/cli @babel/core @babel/preset-env @babel/register babel-loader browser-sync browser-sync-webpack-plugin css-loader mini-css-extract-plugin file-loader node-sass sass-loader style-loader webpack webpack-cli --save-dev
```
## Babel

* Create a .babelrc file at the root of your project
* Paste the below code into .babelrc file

```
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

## Webpack

* Create a webpack.config.js file at the root of your project
* Paste the below code into webpack.config.js

```
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

module.exports = {
    entry: [
        './src/index.js',
        './src/index.scss'
    ],
    output: {
        path: __dirname + "/dist",
        filename: 'index.js'
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [
                    'babel-loader'
                ]
            },
            {
                test: /\.scss$/,
                exclude: /node_modules/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'sass-loader'
                ]
            },
            {
                test: /\.(jpg|png|woff|eot|ttf|svg)$/,
                loader: 'file-loader',
                options: {
                    name: '[path][name].[ext]',
                },
            }
        ]
    },
    watchOptions: {
        ignored: [
            /node_modules/,
            /test/
        ]
    },
    plugins: [
        new MiniCssExtractPlugin(
            {
                filename: 'index.css',
            }
        ),
        new BrowserSyncPlugin({
            host: 'localhost',
            port: 3000,
            files: [
                'index.html',
            ],
            server: {
                baseDir: [
                    './'
                ],
                middleware: [
                    function (req, res, next) {
                        if (-1 === req.url.indexOf(".") && "/" !== req.url) {
                            res.writeHead(302, { 'Location': '/' });
                            res.end();
                        }
                        next();
                    }
                ]
            }
        })
    ]
};

```
Note: You have to modify the entry and output points of your project in the webpack.config.js file.
So by standard we replace the entry and the outputs in the file above with this below:

```
entry: [
        './src/app.js',
        './src/app.scss'
    ],
    output: {
        path: __dirname + "/dist",
        filename: 'app.js'
    },
```

Also replace the filename that can be found under the Plugins attribute to become:
We just changed all files with .js, .scss to have app before them

```
 {
    filename: 'app.css',
    }
```

## Package
Open package.json file and delete "script" : "test".
Then paste this code to replace the deleted script.
The below scripts act as shortcuts to our webpack cli instructions.

* Add scripts in the package.json

```
 "scripts": {
    "start": "npm run dev",
    "dev": "webpack --mode development --watch",
    "prod": "webpack --mode production --watch",
    "build": "webpack --mode production"
  }
```
## Project File structure

You can try structuring your code as follows to follow standards

```
|_ dist
|_ src
   |_ index.js
   |_ index.scss
|_ .babelrc
|_ index.html
|_ package.json
|_ webpack.config.js
```
  
## Linking scripts  and links in index.html
You should link your index.html file to the files stored in the dist folder.

##### Note: Do not modify files stored in the dist folder
 Add this two links to your index.html file to reference your css and javascript files

```
<link rel="stylesheet" type="text/css" href="dist/index.css" />

<script type="text/javascript" src="dist/index.js"></script>
```

* You can now run your project after these settings by typing

```
npm run start
```
