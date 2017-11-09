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

### Project Structure
```


```


### Variables
```
// ====COLOR=====
$text-color:#999;
$secondary-color:#567;
$link:$secondary-color;

p{
    color:$text-color;
}
```


### Partials
Are scss files of their own. We can make our code more modular. 
`partials/_variables.scss`
```
$var:value;
```
`main.scss`
```
@import "partials/variables";
```


### Mixins
Mixin allows you to create reusable styles. Can reuse them through out the project or in other projects. Keep you mixin as a partial for good practice.
```
//Declare Mixin
@mixin warning {
  background: $warning-color;
  color: white;
}

//Use Mixin
.warning {
  @include warning;
  padding: 20px;
}

```
##### Bonus
In scss or sass `font-size` or `font-weight` can be written like this
```
@mixin large-text{
    font:{
        size:20px;
        weight:bold;
    }    
}

@mixin round{
    border-radius:2px;
}

@mixin box{
    @include round;
    padding:20px;
}

```

##### Mixin can be used in different level
```
@mixin fancy-links{
    a{
        text-decoration:none;
        font-style:italic;
    }
}

//You can add this mixin on the root level like this
@include fancy-links;
//or inside a selector
body{
    @include fancy-links;
}



```


##### Mixins with arguments
```
//Define
@mixin round($radius:5px){
    //default value is 5px
    border-radius:$radius;
}

@mixin border($border:1px solid black,$padding:2px){
    border:$border;
    padding:$padding;
}

//Use
.box{
    @include round(10px);
    @include border($padding:20px);//here passing value to the second argument excluding the first.
}

```

##### Passing array
```

@mixin box-shadow($shadows...){
    box-shadow:$shadow;
    -moz-box-shadow:$shadows;
    -webkit-box-shadow:$shadows;
}


#header{
    @include box-shadow(1px 1px 1px red,1px 2px 4px black,1px 3px 5px brown);
}
```


##### Passing Content Blocks

```
//* html is a small hack to apply css to ie only.
@mixin apply-to-ie-6{
    * html{
        @content;//this will fill in any content we pass to our mixin directly in here.
    }
}

@include apply-to-ie-6{
    body{
        font-size:120%;//now only in ie 6 the font size will be 20% larger.
    }
}
```

##### Mixin and font
```
//Declare
@mixin google-font($font){
    $font:unquote($font); //so that the font url is in correct format using the build in function 
    @import url(https://fonts.googleapis.com/css?family=#{$font});
    //using interpolation to use variable in cannot use the interpolation when importing files 
}


//Use
@include google-font("Barlow");

  p {
    font: {
      family: "Barlow";
    }


``` 


### Media Queries
