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
