---
layout: post
title:      "JavaScript Cheatsheet"
date:       2018-06-18 03:01:46 +0000
permalink:  javascript_cheatsheet
---

JavaScript (JS) is an object-oriented scripting language primarily used for generating interactivity on web-pages.  Given its status as one of three languages used for creating web pages (i.e., HTML for structure, CSS for presentation, and JavaScript for behavior), the basics of JS are reviewed in this post to serve as a future reference.  Thus while this post is not meant to be comprehensive or all-inclusive review, it should make for a nice cheatsheet.
## Declarations
Variables in JS can be declared using the keywords ```var```, ```const```, and ```let```.  Without going into detail, ```var``` declarations are hoisted and produce function-scope variables, whereas ```const``` and ```let``` are not hoisted and provide variables with block-scope (implicit declarations result in global scope).  It should be noted that ```const``` declarations also prevent variables from being reassigned.  The format for declaring variables in JS can be seen below.

```javascript
var variableName
const variableName
let variableName
```

Function declarations in JS are hoisted and can be made using the ```function``` keyword with the following syntax.

```javascript
function functionName(){}
```
## Conditionals
Conditionals in JS are used to perform different actions based on the truthiness of one or more conditions.  The syntax for commonly used ```if..else``` statements is shown below along with a shorthand form known as the ternary operator.

```javascript
if (condition){
  expr1
} else {
  expr2
}
```
```javascript
condition ? expr1 : expr2
```
## Loops and iteration
Loops provide a mechanism for executing tasks repeatedly, and JS offers a variety of ways to perform such iterations.  Shown below are the most commonly used looping mechanisms and their associated syntax.  For clarification and without going into detail, the ```for..of``` loop should be used with Arrays and Strings since ```for..in``` cannot be guaranteed to perform iterations in sequence.

```javascript
while (condition){}
for (let i=0; i<n; i++){}
for (const key in object){}
for (const element of array){}
for (const character of string){}
```
### Array methods
In addition to the aforementioned loops, JS also provides methods specifically for iterating through Arrays.  A few of the crowd favorites are shown below, with ```map```, ```filter```, and ```reduce``` also being extremely useful for data manipulation.

```javascript
array.forEach( element => {} )
array.map( element => {} )
array.filter( element => {} )
array.reduce( (accumulator, element) => {} )
```
## jQuery and working with the DOM
JS is used in web applications for its ability to interact with the Document Object Model (DOM).  The DOM is simply a model of an HTML page that dictates how JS can access and modify content.  Thus working with the DOM typically involves two steps:  (1) selecting elements and (2) performing actions on those elements.  The syntax for a simple DOM interaction appears as follows.

```javascript
document.getElementById('id').innerHTML
```

It should be noted that a JS library called jQuery is currently the preferred approach for DOM interactions because it simplifies several JS tasks and enables cross-browser compatibility.  Although many confuse JS and jQuery as separate programming languages, it is important to remember that jQuery is simply JS that has been optimized to perform common tasks in fewer lines of code and across all major browsers.  For example the jQuery function and shorthand notation for performing the previous, simple DOM interaction is shown below.

```javascript
jQuery('#id').html()
```
```javascript
$('#id').html()
```
### DOM queries and selecting elements
Methods for finding elements in the DOM are known as DOM queries.  Such queries enable the selection of DOM elements for accessing and/or updating content.  It's worth noting that query returns are not actually the elements themselves but rather DOM representations of the elements which are referred to as nodes (or nodelist if it's a collection of nodes).  The most commonly used methods for accessing elements via plain or vanilla JS are shown below.

```javascript
document.getElementById('id')
document.getElementsByClassName('class')
document.getElementsByTagName('tag')
document.querySelector('css selector')
document.querySelectorAll('css selector')
```

When one or more elements are selected using jQuery, a jQuery object is returned.  The following syntax is used for selecting elements via the jQuery function.

```javascript
$('#id')
$('.class')
$('tag')
$('css selector')
```
### Get and set actions
Once elements have been selected a variety of methods exist for performing tasks on those elements.  Such tasks include but are not limited to retrieving and updating content as well as adding page effects and animations.  The following list includes commonly used JS methods for working with element nodes.

```javascript
elementNode.innerHTML
elementNode.textContent
elementNode.getAttribute('attribute')
elementNode.setAttribute('attribute', 'value')
```

The equivalent methods in jQuery are shown below.

```javascript
jqueryObject.html()
jqueryObject.text()
jqueryObject.attr()
```
### Event listeners
For web pages to be interactive, methods like the ones described previously are attached to *events* that fire when users interact with a page.  Furthermore different code can be triggered to run, and thus different changes can be made to the DOM, when different elements on a page are provoked.  The process of selecting an element node for user interaction, binding an event to that selected node, and specifying code to run when that event fires is known as event handling.  The following code represents the basic JS  procedure for attaching event listeners.

```javascript
elementNode.addEventListener('event', functionName)
```

The equivalent jQuery syntax is shown below.

```javascript
jqueryObject.on('event', functionName)
```
### Page readiness
To ensure that a page is ready to work with (i.e., the DOM content has finished loading), the following event listener can be attached to the document object.

```javascript
document.addEventListener('DOMContentLoaded', functionName)
```

As with plain JS, if the DOM is not fully constructed then jQuery will not be able to select elements from it.  Thus the equivalent method in jQuery for ensuring page readiness is shown below along with its shorthand form.

```javascript
$(document).ready(function(){
  // script
});
```
```javascript
$(function(){
  //script
});
```
## jQuery and AJAX
Without going into detail, AJAX is a technique that enables data to be requested from a server and loaded into a page without having to refresh the entire page.  Additionally it uses an asynchronous processing model such that the browser does not need to wait for a response from the server, thus enabling a user to continue using the rest of a page while waiting for the server's response (data) to load.  As a brief overview, browsers use XMLHttpRequest objects to create AJAX requests, and when a server responds to the browser's requests the same objects are used to process the results.  Below are two patterns for performing AJAX requests plus a bonus technique specific only to Rails.

### Client-side pattern
Response is HTML, JSON, etc. to be loaded into the page.

```javascript
$(function(){
  $('#id').on('event', function(e){
    e.preventDefault();
    xhr_post('/post/path', {key: 'value'}).done( data => {} );
  });
});
```
```javascript
function xhr_post(url, dataObject){
  return $.ajax({
    url: url,
    method: 'post',
    data: dataObject
  })
  .always(function(){
  })
  .fail(function(){
  });
}
```
### Server-side pattern
Response is JS such that a \*.js.erb file is required.

```javascript
$(function(){
  $('#id').on('event', function(e){
    e.preventDefault();
    $.ajax({
      url: '/get/path',
      method: 'get',
      dataType: 'script'
    });
  });
});
```
### Rails remote-true pattern
Including ```:remote => true``` in select elements automatically creates an event listener and AJAX request for the appropriate url.  Because the auto-generated code follows a server-side pattern, a \*.js.erb file is required to handle the JS response.

