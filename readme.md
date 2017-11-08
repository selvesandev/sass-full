## Preprocessor
Sass,Less,Styles,Scss are some preprocessors built for css. It adds an additional steps before css. A language on top of css
such as scss or sass that will generate css code.

* Why Use Preprocessor ?
    * Variables
    * Nesting
    * Mixins
    * Automatic Vendor Prefixing
    

### Sass Vs Scss    
##### Sass  
`Syntactically awesome style-sheet` an original language that came up with this extra capabilities
on top of css to write better and reusable codes.   
A bit sorter because you don't use any curly braces and semicolons.  
```
,container
    float:left
    width:100%
    p
      color:#333
```


##### Scss
`Sassy css` also build on top of css. This is more closure to css in syntax basis.

```
.container{
    float:left;
    width:100%;
    p{
        color:#333;
    }
}
```


### Installation
```
npm install --save-dev css-loader 
```
* the css loader read style sheet.

```
npm install --save-dev style-loader
```
* the style-loader will take any css from you webpack build and inject it into the page.


```
npm install --save-dev sass-loader node-sass
```
* Here we will need a loader sass-loader which will take case of the webpack process but we also want the node-sass compiler that actually does the work.


```
npm install --save-dev extract-text-webpack-plugin
```

* Extract css to a file.
  

webpack.config.js
```
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var path = require('path');
var webpack = require('webpack');


module.exports = {
    entry: ['./source/js/app.js', './source/scss/main.scss'],
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            { // regular css files
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            { // sass / scss loader for webpack
                test: /\.(sass|scss)$/,
                use: ExtractTextPlugin.extract({
                    use: ['css-loader', 'sass-loader'],
                    fallback: 'style-loader'
                })
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css')
    ]
};
```
