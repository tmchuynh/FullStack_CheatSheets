# Web Fundamentals

# HTML
#### Links
```html
<a href="https://htmlg.com/" target="_blank" rel="nofollow">
  Click here
</a>
```

#### Forms
```html
<form action="/action.php" method="post">
  Name: <input name="name" type="text" /> <br /> 
  Age: <input max="99" min="1" name="age" step="1" type="number" value="18" /> <br />
  <select name="gender">
    <option selected="selected" value="male">Male</option>
    <option value="female">Female</option>
  </select><br /> 
  <input checked="checked" name="newsletter" type="radio" value="daily" /> Daily <input name="newsletter" type="radio" value="weekly" /> Weekly<br />
  <textarea cols="20" name="comments" rows="5">Comment</textarea><br />
  <label><input name="terms" type="checkbox" value="tandc" />Accept terms</label> <br />
<input type="submit" value="Submit" />
</form>
```

# JavaScript
## Objects
```js
Object.length                                        
// length is a property of a function object, and indicates how many arguments the function expects, i.e. the number of formal parameters. This number does not include the rest parameter. Has a value of 1.

Object.keys(obj)                                     
// Returns an array containing the names of all of the given object's own enumerable string properties.

// Methods
obj.hasOwnProperty(prop)                             
// Returns a boolean indicating whether an object contains the specified property as a direct property of that object and not inherited through the prototype chain.

prototypeObj.isPrototypeOf(object)                   
// Returns a boolean indicating whether the object this method is called upon is in the prototype chain of the specified object.

obj.propertyIsEnumerable(prop)                       
// Returns a boolean indicating if the internal ECMAScript [[Enumerable]] attribute is set.

obj.toLocaleString()                                 
// Calls toString().

obj.toString()                                       
// Returns a string representation of the object.

object.valueOf()                                     
// Returns the primitive value of the specified object.
```

## Arrays
```js
Array.length                                         
// Reflects the number of elements in an array.

arr.length                                           
// Reflects the number of elements in an array.

arr.copyWithin(target, start, end)                   
// Copies a sequence of array elements within the array.

arr.fill(value, start, end)                          
// Fills all the elements of an array from a start index to an end index with a static value.

arr.pop()                                            
// Removes the last element from an array and returns that element.

arr.flat()                                           
// merges nested array into one single array

arr.push([element1[, ...[, elementN]]])              
// Adds one or more elements to the end of an array and returns the new length of the array.

arr.reverse()                                        
// Reverses the order of the elements of an array in place — the first becomes the last, and the last becomes the first.

arr.shift()                                          
// Removes the first element from an array and returns that element.

arr.sort()                                           
// Sorts the elements of an array in place and returns the array.

array.splice(start, deleteCount, item1, item2, ...)  
// Adds and/or removes elements from an array.

arr.unshift([element1[, ...[, elementN]]])           
// Adds one or more elements to the front of an array and returns the new length of the array.

arr.concat(value1[, value2[, ...[, valueN]]])        
// Returns a new array comprised of this array joined with other array(s) and/or value(s).

arr.includes(searchElement, fromIndex)               
// Determines whether an array contains a certain element, returning true or false as appropriate.

arr.indexOf(searchElement[, fromIndex])              
// Returns the first (least) index of an element within the array equal to the specified value, or -1 if none is found.

arr.join(separator)                                  
// Joins all elements of an array into a string.

arr.lastIndexOf(searchElement, fromIndex)            
// Returns the last (greatest) index of an element within the array equal to the specified value, or -1 if none is found.

arr.slice(begin, end)                                
// Extracts a section of an array and returns a new array.

arr.toString()                                       
// Returns a string representing the array and its elements. Overrides the Object.prototype.toString() method.

arr.toLocaleString(locales, options)                 
// Returns a localized string representing the array and its elements. Overrides the Object.prototype.toLocaleString() method.

arr.entries()                                        
// Returns a new Array Iterator object that contains the key/value pairs for each index in the array.

arr.every(callback[, thisArg])                       
// Returns true if every element in this array satisfies the provided testing function.

arr.filter(callback[, thisArg])                      
// Creates a new array with all of the elements of this array for which the provided filtering function returns true.

arr.find(callback[, thisArg])                        
// Returns the found value in the array, if an element in the array satisfies the provided testing function or undefined if not found.

arr.findIndex(callback[, thisArg])                   
// Returns the found index in the array, if an element in the array satisfies the provided testing function or -1 if not found.

arr.forEach(callback[, thisArg])                     
// Calls a function for each element in the array.

arr.keys()                                           
// Returns a new Array Iterator that contains the keys for each index in the array.

arr.map(callback[, initialValue])                    
// Creates a new array with the results of calling a provided function on every element in this array.

arr.reduce(callback[, initialValue])                 
// Apply a function against an accumulator and each value of the array (from left-to-right) as to reduce it to a single value.

arr.reduceRight(callback[, initialValue])            
// Apply a function against an accumulator and each value of the array (from right-to-left) as to reduce it to a single value.

arr.some(callback[, initialValue])                   
// Returns true if at least one element in this array satisfies the provided testing function.

arr.values()                                         
// Returns a new Array Iterator object that contains the values for each index in the array.
```

## CSS
```css
/** Body Selector which applies properties to whole body <body></body> */
body {
  /* Font-Family */
  font-family: "Segoe UI", Tahoma, sans-serif; /* You can declare multiple fonts. */
  /*if first font doesn't exists other ones will be declared serial wise */

  /* Font-Style */
  font-style: italic;

  /* Font-Variant */
  font-variant: small-caps;

  /* Font-Weight */
  font-weight: bold;

  /* Font-Size */
  font-size: larger;

  /* Font */
  font: style variant weight size family;
}

/***************************

------------ 02: Text -----------

Text properties allow one to manipulate alignment, spacing, decoration, indentation, etc., in the
document.

*******************************/

/* Applies to all tags with class 'container' ex: <div class="container"></div> */
.container {
  /* Text-Align */
  text-align: justify;

  /* Letter-Spacing */
  letter-spacing: 0.15em;

  /* Text-Decoration */
  text-decoration: underline;

  /* Word-Spacing */
  word-spacing: 0.25em;

  /* Text-Transform */
  text-transform: uppercase;

  /* Text-Indent */
  text-indent: 0.5cm;

  /* Line-Height */
  line-height: normal;
}

/***************************

------------ 03: Background -----------

As the name suggests, these properties are related to background, i.e., you can change the color,
image, position, size, etc., of the document.

*******************************/

/* Applies to all tags with id 'wrapper' ex: <div id="wrapper"></div> */
#wrapper {
  /* Background-Image */
  background-image: url("Path");

  /* Background-Position */
  background-position: right top;

  /* Background-Size */
  background-size: cover;

  /* Background-Repeat */
  background-repeat: no-repeat;

  /* Background-Attachment */
  background-attachment: scroll;

  /* Background-Color */
  background-color: fuchsia;

  /* Background */
  background: color image repeat attachment position;
}

/***************************

------------ 04: Border -----------

Border properties are used to change the style, radius, color, etc., of buttons or other items of
the document.

*******************************/

/* You can also select multiple items */
div,
.container {
  /* Border-Width */
  border-width: 5px;

  /* Border-Style */
  border-style: solid;

  /* Border-Color */
  border-color: aqua;

  /* Border-Radius */
  border-radius: 15px;

  /* Border */
  border: width style color;
}

/***************************

------------ 05: Box Model -----------

In laymen's terms, the CSS box model is a container that wraps around every HTML element. It
consists of margins, borders, padding, and the actual content.
It is used to create the design and layout of web pages.

*******************************/

.wrapper {
  /* Float */
  float: none;
  /* Clear */
  clear: both;
  /* Display */
  display: block;
  /* Height */
  height: fit-content;
  /* Width */
  width: auto;
  /* Margin */
  margin: top right bottom left;
  /* Padding */
  padding: top right bottom left;
  /* Overflow */
  overflow: hidden;
  /* Visibility */
  visibility: visible;
  /* z-index */
  z-index: 1;
  /* position */
  position: static | relative | fixed | absolute | sticky;

}

/***************************

------------ 06: Colors -----------

With the help of the color property, one can give color to text, shape, or any other object.

*******************************/

p,
span,
.text {
  /* Color - 1 */
  color: cornsilk;
  /* Color - 2 */
  color: #fff8dc;
  /* Color - 3 */
  color: rgba(255, 248, 220, 1);
  /* Color - 4 */
  color: hsl(48, 100%, 93%);
  /* Opacity */
  opacity: 1;
}

/***************************

------------ 07: Template Layout -----------

Specifies the visual look of the content inside a template

*******************************/

/* '*' selects all elements on a page */
* {
  /* Box-Align */
  box-align: start;

  /* Box-Direction */
  box-direction: normal;

  /* Box-Flex */
  box-flex: normal;

  /* Box-Flex-Group */
  box-flex-group: 2;

  /* Box-Orient */
  box-orient: inline;

  /* Box-Pack */
  box-pack: justify;

  /* Box-Sizing */
  box-sizing: margin-box;

  /* max-width */
  max-width: 800px;

  /* min-width */
  min-width: 500px;

  /* max-height */
  max-height: 100px;

  /* min-height */
  min-height: 80px;
}

/***************************

------------ 08: Table -----------

Table properties are used to give style to the tables in the document. You can change many
things like border spacing, table layout, caption, etc.

*******************************/

table {
  /* Border-Collapse */
  border-collapse: separate;

  /* Empty-Cells */
  empty-cells: show;

  /* Border-Spacing */
  border-spacing: 2px;

  /* Table-Layout */
  table-layout: auto;

  /* Caption-Side */
  caption-side: bottom;
}

/***************************

------------ 09: Columns -----------

These properties are used explicitly with columns of the tables, and they are used to give the
table an incredible look.

*******************************/

/* Applies to <table class="nice-table"></table> */
/* Not <table></table> */
table.nice-table {
  /* Column-Count */
  column-count: 10;

  /* Column-Gap */
  column-gap: 5px;

  /* Column-rule-width */
  column-rule-width: medium;

  /* Column-rule-style */
  column-rule-style: dotted;

  /* Column-rule-color */
  column-rule-color: black;

  /* Column-width */
  column-width: 10px;

  /* Column-span */
  column-span: all;
}

/***************************

------ 10: List & Markers -------

List and marker properties are used to customize lists in the document.

*******************************/

li,
ul,
ol {
  /* List-style-type */
  list-style-type: square;

  /* List-style-position */
  list-style-position: 20px;

  /* List-style-image */
  list-style-image: url("image.gif");

  /* Marker-offset */
  marker-offset: auto;
}

/***************************

------------ 11: Animations -----------

CSS animations allow one to animate transitions or other media files on the web page.

*******************************/

svg,
.loader {
  /* Animation-name */
  animation-name: myanimation;

  /* Animation-duration */
  animation-duration: 10s;

  /* Animation-timing-function */
  animation-timing-function: ease;

  /* Animation-delay */
  animation-delay: 5ms;

  /* Animation-iteration-count */
  animation-iteration-count: 3;

  /* Animation-direction */
  animation-direction: normal;

  /* Animation-play-state */
  animation-play-state: running;

  /* Animation-fill-mode */
  animation-fill-mode: both;
}

/***************************

------------ 12: Transitions -----------

Transitions let you define the transition between two states of an element.

*******************************/

a,
button {
  /* Transition-property */
  transition-property: none;

  /* Transition-duration */
  transition-duration: 2s;

  /* Transition-timing-function */
  transition-timing-function: ease-in-out;

  /* Transition-delay */
  transition-delay: 20ms;
}

/***************************

------------ 13: CSS Flexbox (Important) -----------

Flexbox is a layout of CSS that lets you format HTML easily. Flexbox makes it simple to align
items vertically and horizontally using rows and columns. Items will "flex" to different sizes to fill
the space. And overall, it makes the responsive design more manageable.

*******************************/

/* ---------------------- Parent Properties (flex container) ------------ */

section,
div#wrapper {
  /* display */
  display: flex;

  /* flex-direction */
  flex-direction: row | row-reverse | column | column-reverse;

  /* flex-wrap */
  flex-wrap: nowrap | wrap | wrap-reverse;

  /* flex-flow */
  flex-flow: column wrap;

  /* justify-content */
  justify-content: flex-start | flex-end | center | space-between | space-around;

  /* align-items */
  align-items: stretch | flex-start | flex-end | center | baseline;

  /* align-content */
  align-content: flex-start | flex-end | center | space-between | space-around;
}


/* ---------------------- Child Properties (flex items) ------------ */

p,
span,
h1,
h2,
h3,
h4,
h5,
h6,
a {
    /* order */
    order: 5; /* default is 0 */

    /* flex-grow */
    flex-grow: 4; /* default 0 */

    /* flex-shrink */
    flex-shrink: 3; /* default 1 */

    /* flex-basis */
    flex-basis: | auto; /* default auto */

    /* flex shorthand */
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]

    /* align-self */
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}

/***************************

------------ 14: CSS Grid (Useful in Complex Web Designs) -----------

Grid layout is a 2-Dimensional grid system to CSS that creates complex responsive web design
layouts more easily and consistently across browsers.

*******************************/


/* ---------------------- Parent Properties (Grid container) ------------ */

section,
div#wrapper {
    /* display */
    display: grid | inline-grid;

    /* grid-template-columns */
    grid-template-columns: 12px 12px 12px;

    /* grid-template-rows */
    grid-template-rows: 8px auto 12px;

    /* grid-template */
    grid-template: none | <grid-template-rows> / <grid-template-columns>;

    /* column-gap */
    column-gap: <line-size>;

    /* row-gap */
    row-gap: <line-size>;

    /* grid-column-gap */
    grid-column-gap: <line-size>;

    /* grid-row-gap */
    grid-row-gap: <line-size>;

    /* gap shorthand */
    gap: <grid-row-gap> <grid-column-gap>;

    /* grid-gap shorthand */
    grid-gap: <grid-row-gap> <grid-column-gap>;

    /* justify-items */
    justify-items: start | end | center | stretch;

    /* align-items */
    align-items: start | end | center | stretch;

    /* place-items */
    place-items: center;

    /* justify-content */
    justify-content: start | end | center | stretch | space-around | space-between;

    /* align-content */
    align-content: start | end | center | stretch | space-around | space-between;

    /* place-content */
    place-content: <align-content> / <justify-content>;

    /* grid-auto-columns */
    grid-auto-columns: <track-size> ...;

    /* grid-auto-rows */
    grid-auto-rows: <track-size> ...;

    /* grid-auto-flow */
    grid-auto-flow: row | column | row dense | column dense;

}


/* ---------------------- Child Properties (Grid items) ------------ */

p,
span,
h1,
h2,
h3,
h4,
h5,
h6,
a {
    /* grid-column-start */
    grid-column-start: <number> | <name> | span <number> | span <name> | auto;

    /* grid-column-end */
    grid-column-end: <number> | <name> | span <number> | span <name> | auto;

    /* grid-row-start */
    grid-row-start: <number> | <name> | span <number> | span <name> | auto;

    /* grid-row-end */
    grid-row-end: <number> | <name> | span <number> | span <name> | auto;

    /* grid-column shorthand */
    grid-column: <start-line> / <end-line> | <start-line> / span <value>;

    /* grid-row shorthand */
    grid-row: <start-line> / <end-line> | <start-line> / span <value>;

    /* grid-area */
    grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;

    /* justify-self */
    justify-self: start | end | center | stretch;

    /* align-self */
    align-self: start | end | center | stretch;

    /* place-self */
    place-self: center;
}


/***************************

------------ 15: MEDIA QUERIES -----------

Using media queries are a popular technique for delivering a tailored style sheet to
desktops, laptops, tablets, and mobile phones (such as iPhone and Android phones).

|----------------------------------------------------------|
|  Responsive Grid Media Queries - 1280, 1024, 768, 480    |
|   1280-1024   - desktop (default grid)                   |
|   1024-768    - tablet landscape                         |
|   768-480     - tablet                                   |
|   480-less    - phone landscape & smaller                |
|----------------------------------------------------------|

*******************************/


@media all and (min-width: 1024px) and (max-width: 1280px) { }
 
@media all and (min-width: 768px) and (max-width: 1024px) { }
 
@media all and (min-width: 480px) and (max-width: 768px) { }
 
@media all and (max-width: 480px) { }
 
/* Small screens - MOBILE */
@media only screen { } /* Define mobile styles - Mobile First */
 
@media only screen and (max-width: 40em) { } /* max-width 640px, mobile-only styles, use when QAing mobile issues */
 
/* Medium screens - TABLET */
@media only screen and (min-width: 40.063em) { } /* min-width 641px, medium screens */
 
@media only screen and (min-width: 40.063em) and (max-width: 64em) { } /* min-width 641px and max-width 1024px, use when QAing tablet-only issues */
 
/* Large screens - DESKTOP */
@media only screen and (min-width: 64.063em) { } /* min-width 1025px, large screens */
 
@media only screen and (min-width: 64.063em) and (max-width: 90em) { } /* min-width 1024px and max-width 1440px, use when QAing large screen-only issues */
 
/* XLarge screens */
@media only screen and (min-width: 90.063em) { } /* min-width 1441px, xlarge screens */
 
@media only screen and (min-width: 90.063em) and (max-width: 120em) { } /* min-width 1441px and max-width 1920px, use when QAing xlarge screen-only issues */
 
/* XXLarge screens */
@media only screen and (min-width: 120.063em) { } /* min-width 1921px, xlarge screens */
 
/*------------------------------------------*/
 
 
 
/* Portrait */
@media screen and (orientation:portrait) { /* Portrait styles here */ }
/* Landscape */
@media screen and (orientation:landscape) { /* Landscape styles here */ }
 
 
/* CSS for iPhone, iPad, and Retina Displays */
 
/* Non-Retina */
@media screen and (-webkit-max-device-pixel-ratio: 1) {
}
 
/* Retina */
@media only screen and (-webkit-min-device-pixel-ratio: 1.5),
only screen and (-o-min-device-pixel-ratio: 3/2),
only screen and (min--moz-device-pixel-ratio: 1.5),
only screen and (min-device-pixel-ratio: 1.5) {
}
 
/* iPhone Portrait */
@media screen and (max-device-width: 480px) and (orientation:portrait) {
} 
 
/* iPhone Landscape */
@media screen and (max-device-width: 480px) and (orientation:landscape) {
}
 
/* iPad Portrait */
@media screen and (min-device-width: 481px) and (orientation:portrait) {
}
 
/* iPad Landscape */
@media screen and (min-device-width: 481px) and (orientation:landscape) {
}

/* Make Sure you don't forgot to add */
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" /> /* within <head> tag  */
```