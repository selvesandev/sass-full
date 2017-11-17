## Preprocessor
Sass,Less,Styles,Scss are some preprocessors built for css. It adds an additional steps before css. A language on top of css
such as scss or sass that will generate css code.

#### Why Use Preprocessor ?
    * Variables
    * Nesting
    * Mixins
    * Automatic Vendor Prefixing
    

### Sass VS Scss    
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
All the media queries that you use in the css are all valid in scss or sass.
Media queries in scss provide us more advantages. We can use nested media queries in scss so that you do not have to select the item each time. eg:-
```

.box {
  padding: 20px;
  min-height: 100px;
  background: red;
  max-width: 1200px;
  margin: 20px auto;

  @media all and (max-width: 800px) and (min-width: 600px) {
    background: blue;
  }

  @media all and (max-width: 601px){
    background: orange;
  }

}

```


##### Flexible media query using mixin
using mixin to handle all the typical media query because usually we are going to handle the same size of media query.

```
//Declare

@mixin tiny-screen() {
  @media only screen and (max-width: 320px) {
    @content;
  }
}

@mixin small-screen() {
  @media only screen and (max-width: 480px) {
    @content;
  }
}

@mixin medium-screen() {
  @media only screen and (max-width: 680px) {
    @content;
  }
}

@mixin large-screen() {
  @media only screen and (max-width: 960px) {
    @content;
  }
}


//Use
.box{
    @include large-screen{
        font-size:125%;
    }
}

```


### Operators


##### Arithmetic
###### With measurements
```
3px + 4px;
4px - 2px;
$size:2px * 5px;
$size/6px;

------------------
1in - 5px;

2em - 8px; //will not work because em and px are not compatible
(8px/4px); // when dividing keep in brackets or keep value in variable.
```

###### With colors

```
#333+#777;
#090807 - #030201;//060606
```


### Functions
Some predefined function in css
```
rgb();
rgba();
hsl();//hue saturation and lightning.
linear-gradient()
calc()
```
Function have a name, takes value as an arguments and returns value.

##### Sass Built in functions
* `darken($color,%)`
```
a{
    color:$link-color;
    &:hover{
        color:darken($link-color,15%); 
    }
}

```
* lighten()
```
@mixin warning {
    background-color:orange;
    color:#fff;
    &:hover{
        background-color:lighten(orange,10%)
    }
}

button {
    @include warning;
}

```
* Opacify();
* transparentize();


##### Sass User Defined functions

```
@function sum($left,$right){
    @return $left + $right;
}

//Function to convert pixel into em.
@function emValue($pixel,$context:16px){
    @return ($pixel/$content)*1em;
}

//Function to calculate the width of the single column in a grid layout.
@function col-width($columns:12,$page-width:100%,$gat:1%){
    @return ($page-width - $gap*($columns-1));
}

//Use
$col: col-width($columns:8)

```


### Inheritance.
Here is a example of inheritance  
you have a css class with property say.
```
.error{
    color:red;
}
```

you have another piece of css 
```
.critical-error{
    border:1px solid red;
    font-weight:bold;
}
```

What we want here is in our `critical-error` we also 
want the properties of `.error` class. We can do so by extending the .error class

```
.critical-error{
    @extend .error;
    border:1px solid red;
    font-weight:bold;
}

```

This can also be a psuedo and multiple extend.

```
.critical-error{
    @extend .error:hover;
    @extend .headen.large;//both header and .large class
    border:1px solid red;
    font-weight:bold;
}

```

### Advanced rule for using @extend directive in sass.

* Cannot have selector sequences.
```
.cta-button{
    @extend .warning .foo; //error
}
```
* Rules related to media query. (we cannot extend from selector
which is outside the media query from inside the media query)

```
@media screen{
    .super{
        @extend .error; // error;
    }
}
```
you can only use selector that is declared inside the media query. 

**Note** but you can use the method class declared inside of MQ outside of the MQ.


* Another Feature (%) reference
```
%className{
    color:red;
}

.error{
    @extend %className;
}
``` 
Will compile the css to.

```
.error{
    color:red;
}
```

Basically it will not create it's own class. But it will simple copy and paste the code only i.e the list of property and value


* !optional flag
```
.test{
    @extend .foo; // will result in error since the .foo class is not defined
}
```

```
.test{
    @extend .foo!optional; //will extend the .foo class if it is declared will ignore if it doesn't
}
```



### @extend VS @mixin





### Branch
A branch allows you to create a variation of your code. let say it allows you to 
create a another copy for eg: say you are writing some complicated feature in the new branch
and then you merge the new branch to the master back together.


```
git branch project
```
Branch `project` created.


```
git checkout project
```
Switch the current branch`(master)` to `project`

```
git add .
git commit -m "commited code"
```
code commited for the new branch.

```
git checkout master
```

