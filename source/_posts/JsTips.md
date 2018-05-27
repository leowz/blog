---
title: JsTips
date: 2018-05-16 00:52:35
tags: javascript
categories: web
---

# Data types

Primitive Data types
* string
* number  
* boolean (true| false lowercase)
* undefined 

Complex Data types
* function (Function object)
* object

# Modules
## ES6 Module Singleton
ES6 Modules are singletons.

Export the new instance as default.

If you simply export a class instead of an instance, you can create other instance obviously. In this scenario, there is no singleton.

## `export` Key Word
`export` statement is used when creating JS modules to export functions, objects, etc. So they can be used by other programs with `import` statement.

syntax:
```javascript
export {name1, var2 = name2};
export let(const) name1;
export function name(){};
export class ClassName {};

export default expression;

export * from …;

```
## `import` Key Word

syntax:
```javascript
import defaultExport from ‘module-name’;
import * as name from ‘module’;
import { export1, export2 } from ‘module’;
```

# Inheritance and the prototype chain
Javascript uses the prototypal inheritance form. Object inherit directly from other objects. 

Each object has a private property which holds a link to another object called its `prototype`. That prototype object has a prototype of its own and so on util an object is reached with `null` as its prototype. `null` here acts as the final link in this prototype chain.

Nearly, all object in JS are instances of `Object` which sits on the top of a prototype chain.

<!--more-->

## Inheritance
JS objects are dynamic bags of properties. JS objects have a link to a prototype object. When trying to access a property of an object, the property will not only search the object but on the property of the object also, until either a property with a bathing name is found or the end of the prototype chain is reached.

ways to create the prototype chain:
* syntax constructs
* constructor (new keyword)
* Object.create
* class, extends

example:
```javascript
var b = ["yo", "whadup", "?"];
/* Arrays inherit from Array.prototype 
 (which has methods indexOf, forEach, etc.)
 The prototype chain looks like:
 b ---> Array.prototype ---> Object.prototype ---> null;
*/

function f() {
  return 2;
}
/* Functions inherit from Function.prototype 
 (which has methods call, bind, etc.)
 f ---> Function.prototype ---> Object.prototype ---> null
*/

function Graph() {
  this.vertices = [];
  this.edges = [];
}
var g = new Graph();
/* g is an object with own properties 'vertices' and 'edges'.
 g.[[Prototype]] is the value of Graph.prototype when new Graph() is executed.
*/

var a = {a: 1}; 
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null

class Polygon {
    ...
}

class Square extends Polygon {
 …
}

```


# JavaScript Functions
Every function in JS is a `Function` object, they have properties and methods just like any other object. However, functions can be called.

Properties:
* Function.arguments
* Function.caller
* Function.length
* Function.name
* Function.prototype

Methods:
* Function.prototype.apply()
* Function.prototype.bind()
* Function.prototype.call()
* Function.prototype.toString()

## Create functions

function declaration and function expression
```javascript
function1(width, height) {
    return width * height;
}

var getRectArea = function2(width, height) {
    return width * height;
}
```

arrow function expression
```javascript
([param]) => {
    statements
}
```

# JavaScript Objects
Objects are variable that contain many key value pars. Name and blues is spared by a colon(:) and each pair is separated by coma(,).
Object properties can be primitive values, other objects or functions.

Properties:
* Object.prototype
* Object.prototype.constructor

Methods:
* Object.assign() : Copies the values from one object to a targe object
* Object.create() : Create object
* Object.is() : compare two object are the same
* Object.keys() : return an array of all key of the object
* Object.values() : return an array of all values

creating a JS object:
1. object literal ({key1: val1, key2: val2})
2. using keyword new  (var person = new Object())

Objects are mutable; they are addressed by reference.

## object properties
accessing the property of an object:
* objectName.property
* objectName[“property"]

adding new properties:
add new properties by simply giving it a value
`person, nationality = “English”;`

deleting properties:
use delete keyword
`delete person.age;`

## this Keyword
Every new function has its own this value, this is the new object in case of a constructor, undefined in strict mode function calls, this represents the context object if the function is called as an “object method” etc.

Note that, this holds the value of undefined in global functions and in anonymous functions that are not bound to any object.

### this in the global scope
In the global scope, when the code is executing in the browser, a functions with this, this refers to the global `window` object(not in strict mode) that is the main container of the entire JS application.

### context
The object that invokes a function is in the context, we can change the context by invoking a function with another object, then this new object is in context.

### this in a callback function
When we execute a method passed from another object as a callback function. This keyword refers to current object and not the original object that defines the callback function. It refers to the object where the method is invoked.
So if the callback function contains `this` key word. The behaviour of function will not be correct.

**fix:**
Use `Bind()` , `Apply()` or `Call()` method to specifically set the value of this.

### this inside closure
It is important to note that closures cannot access the outer function’s this variable by using `this` keyword. Inside an anonymous function, `this` is bound to the global window(or undefined in strict mode).

**fix:**
Just set `this` value to another variable so that closure can capture the variable.

example:
```javascript
var user = { 
tournament:"The Masters",
data :[ 
{name:"T. Woods", age:37}, 
{name:"P. Mickelson", age:43} 
], 

clickHandler:function (event) {
    var theUserObj = this; 
    this.data.forEach (function (person) {
        console.log (person.name + " is playing at " + theUserObj.tournament); 
        }) 
    }
}
```

### this passed as a variable
When pass a method as a variable that contains `this`, it is the same case with the callback function.
The fix is by setting `this` value using `bind()` method.

