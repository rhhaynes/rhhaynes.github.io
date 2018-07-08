---
layout: post
title:      "Creating JavaScript Model Objects"
date:       2018-07-08 15:20:56 -0400
permalink:  creating_javascript_model_objects
---

Given the requirement for utilizing JavaScript (JS) Model Objects in the rails app with a jQuery front end, I've decided to dedicate this post to reviewing the basics of objects and specifically how they're created.  In short, JS objects are standalone entities containing a collection of properties, i.e., key (property name) / value associations.  The properties represent characteristics of the object and can be thought of as standard JS variables attached to the object.  If the property happens to be a function, it is referred to as a *method*.  Thus methods are simply functions that are associated with the object.  All object properties including methods are accessed via the dot-notation as shown below.  Lastly it should be noted that all unassigned properties of the object are *undefined*.
```javascript
object.property
```

## Constructor Functions
A constructor function defines a new object type by using a function to specify the name, properties, and methods.  In the example shown below, a new object type called ```Travel``` is created with properties of ```destinationName```, ```startDate```, ```endDate```, and ```purpose```.  The properties are assigned using the keyword *this* along with the values passed to the function.  The method ```displayTravel()``` is subsequently defined on the ```Travel.prototype``` such that all ```Travel``` instances share the same function.  For this reason methods defined on the prototype can be changed universally for all instances.
```javascript
function Travel(destinationName, startDate, endDate, purpose){
  this.destinationName = destinationName;
  this.startDate = startDate;
  this.endDate = endDate;
  this.purpose = purpose;
}
```
```javascript
Travel.prototype.displayTravel = function(){
  return destinationName + ': ' + startDate + ' - ' + endDate;
}
```

## *Class* Construct
A new way to create prototype-based objects (introduced in ES6 or ECMAScript 2015) is through the *class* construct.  While it may seem like this approach creates some abstract class entity, the construct is really just special syntax to wrap a constructor together with its prototype methods.  Thus it looks very similar to the constructor function described previously albeit with a cleaner syntax.
```javascript
class Travel {
  constructor(destinationName, startDate, endDate, purpose){
	  this.destinationName = destinationName;
	  this.startDate = startDate;
	  this.endDate = endDate;
	  this.purpose = purpose;
  }

  displayTravel(){
    return destinationName + ': ' + startDate + ' - ' + endDate;
  }
}
```
