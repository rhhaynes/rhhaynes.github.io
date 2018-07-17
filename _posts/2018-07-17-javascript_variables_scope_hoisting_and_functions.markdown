---
layout: post
title:      "JavaScript Variables, Scope, Hoisting, and Functions"
date:       2018-07-17 07:25:27 +0000
permalink:  javascript_variables_scope_hoisting_and_functions
---

Variables in JavaScript (JS), like most programming languages, are storage containers that hold information for future referencing and/or modification.  Such information can be any JS data type like numbers, strings, objects, etc.  Given the significance of variables, this blog post serves to review several basic concepts including declarations, initializations, scope, and hoisting.  These concepts are then discussed in regard to JS functions.

## Declarations and initializations
To create or *declare* variables in JS, the ```var```, ```const```, and ```let``` keywords are used in conjunction with variable names.  Ignoring keyword differences for now, the syntax of a simple variable declaration using ```var``` is shown below.

```javascript
var cityName;
console.log(cityName); // undefined
```

In addition to specifying one of the aforementioned keywords, basic rules for declaring and naming variables include
* variable names may only consist of letters, numbers, the dollar symbol ( ```$``` ), and the underscore symbol ( ```_``` )
* variable names may not begin with numbers
* variable names may not contain spaces
* variable names may not be reserved JS keywords.

Although declarations bring variables into existence, their default values are ```undefined``` until assignments are made.  For this reason and because it provides insight into their intended use and data type, *initializing* variables when they are declared is considered good practice.  Revisiting the previous example, the syntax for a joint variable declaration and initialization is shown below.

```javascript
var cityName = "Atlanta";
console.log(cityName); // Atlanta
```

Lastly it should be noted that there are important differences between the three declaration keywords based on scope, hoisting, and reassignment.  These differences are presented in the table below and are discussed in the subsequent sections.

**Keyword** | **Scope** | **Hoisting** | **Reassignable**
--- | --- | --- | ---
```var``` | Functional | Yes | Yes
```const``` | Block | No | No
```let``` | Block | No | Yes

## Scope
Scope refers to the context in which a variable exists and thus can be accessed.  The two types of scope in JS are global and local, where global refers to variables declared outside of all functions and blocks as well as variables initialized without one of the keywords.  Because global variables can be accessed by all scripts on a page, using local variables is the preferred option to safeguard against overwriting variable values.

Unlike global variables, locally-scoped variables are only accessible inside the functions or blocks where they are defined.  While all variables declared with a keyword are *function-scoped* (meaning they recognize functions as having separate scope), variables declared using ```const``` and ```let``` are also *block-scoped*.  For block-scoped variables a new local scope is created by any code block (not just functions) including ```if``` and ```switch``` statements as well as ```for``` and ```while``` loops.

To illustrate the difference between function- and block-scoped variables, an attempt is made to initialize new variables inside the ```if``` statement below using ```var``` and ```let```.  Because ```let``` exhibits block scope, a new local variable with the same name as the original variable is created without impacting the original value.  However because ```var``` only has function scope, it does not recognize the ```if``` block as having a new local scope.  Consequently instead of creating a new local variable, the original ```var``` variable is reassigned in the same scope.

```javascript
(function(){
  //local, function-scope initializations
  var cityNameVar = "Atlanta";
  let cityNameLet = "Atlanta";

  if (true){
    // local, block-scope initializations
    var cityNameVar = "Chicago";
    let cityNameLet = "Chicago";
		
    console.log(cityNameVar); // Chicago
    console.log(cityNameLet); // Chicago
  }

  console.log(cityNameVar); // Chicago
  console.log(cityNameLet); // Atlanta
}());
```

Lastly it should be noted that ```const``` behaves in a similar way to ```let```, however variables created with ```const``` cannot be reassigned.  As such ```const``` variables must always be initialized when they are declared.

## Hoisting
Hoisting refers to the automatic shifting of ```var``` declarations to the top their scope such that variables can seemingly be used before they are declared.  To illustrate this behavior, the example below provides a JS interpretation of code in which a variable is used prior to its declaration and initialization.

```javascript
console.log(cityName); // undefined
var cityName = "Atlanta";
console.log(cityName); // Atlanta
```
```javascript
// JS interpretation

var cityName;
console.log(cityName); // undefined
cityName = "Atlanta";
console.log(cityName); // Atlanta
```

As is evident, only the declaration is hoisted, not the initialization.  Consequently the variable's value is ```undefined``` until the actual assignment is reached.  Nevertheless its usage does not throw a ```ReferenceError```.

Failing to take hoisting into account can lead to unexpected results as demonstrated in the following example.  Because the ```if``` statement evaluates to ```false```, one might expect the ```console.log()``` output to be "Atlanta."  However since the ```var cityName``` declaration is hoisted to the top of the function, and because the assignment inside the ```if``` statement never executes, a value of ```undefined``` is actually output.

```javascript
var cityName = "Atlanta";

(function(){
  if (false){
    var cityName = "Chicago";
  }
  console.log(cityName); // undefined
}());
```

Because of hoisting, it is considered best practice to always declare variables at the top of their respective scopes.

## Functions
Like variables declared with ```var```, JS function declarations begin with a keyword ( ```function``` ) and experience hoisting.  The following example illustrates both a simple declaration as well as the ability to invoke a function prior to its declaration (hoisting).

```javascript
console.log(cityName()); // Atlanta

function cityName(){
  return "Atlanta";
}
```

In much the same way that variable declarations are hoisted but their initializations are not, function expressions are not hoisted.  Function expressions refer to the JS syntax whereby either a named function declaration or an anonymous function is stored in a variable.  In the example below, a function expression based on an anonymous function is invoked prior to its definition.

```javascript
console.log(cityName()); // TypeError: cityName is not a function

var cityName = function(){
  return "Atlanta";
}
```

Because only the ```var cityName``` declaration is hoisted from the function expression, invoking the function prior to its definition results in a ```TypeError``` since the value of ```cityName``` at this point is ```undefined```.

It's worth mentioning that a shorthand version of the function expression, known as the arrow function, also exists and is shown below.  While there are other differences, the major advantages offered by arrow functions are a shorter syntax and the non-binding of *this*.  Until arrow functions every new function defined its own *this* value, however arrow functions simply inherit the value from their enclosing context or function.

```javascript
console.log(cityName()); // TypeError - cityName is not a function

var cityName = () => "Atlanta";
```
