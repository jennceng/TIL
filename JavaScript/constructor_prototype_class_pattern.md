

![Alt text](http://i.imgur.com/1FeP1Af.png)

I have this property… should it be in the constructor or the prototype?

* think of the constructor as the initialize function in ruby

* the constructor should contain only things **unique **to the instance of the object, such as properties (aka attributes, similar to instance variables in ruby)
* it is only executed upon the creation of the object, if you add methods to the constructor after the object is created, the already created object will not have access to those methods

* the **prototype **is shared by all instances of that type

* things that are common amongst all instances of that object belong here, such as methods that all instances of the object should have access to
* BUT WAIT, isn’t the constructor the same for all instances of the custom object?

###

* YES, AND THAT’S WHY THE PROTOTYPE HAS AN ATTRIBUTE CALLED CONSTRUCTOR

* this is done behind the scenes by JS when defining a constructor function and using it to create new objects (see below in Things to Keep in Mind)

* this avoids redefining the function every single time an object is created
* this also can happen on the fly, if you add a method to the prototype, it is IMMEDIATELY AVAILABLE to all instances of the object, EVEN ONES ALREADY CREATED!
* if you need to alter the function and want that change to effect all instances of the object, changing it on the prototype makes this possible, BOOM!
* Things to keep in mind:

  * When you create an object via an object literal, it will assign the prototype of the new object to **Object.prototype**
  * When defining a constructor function(let’s say a Person), behind the scenes it is assigning creating a prototype Person with a new Person prototype that has a property of constructor, with the value of the construct you just defined. The constructor is an attribute (property) of a prototype.

![Alt text](http://i.imgur.com/68L0aPS.png)

---

Let’s take a look at this block of code

```js
function Ship(name) {
  this.name = name;
  this.crew = [];
  this.propulsion = null;
}

Ship.prototype.loadCrew = function(trainedCrew) {
  for(var i = 0; i < trainedCrew.length; i++) {
    this.crew.push(trainedCrew[i]);
    console.log("Welcome aboard, " + trainedCrew[i].name);
  }
}

Ship.prototype.captain = function() {
  var crewCount = this.crew.length;
  return this.crew[Math.floor(Math.random() * crewCount)]
}

Ship.prototype.mountPropulsion = function(unmountedPropulsion) {
  this.propulsion = unmountedPropulsion;
  console.log("Propulsion mounted!");
}
```

This could have been rewritten in a more concise and easily read way:

```js
function Ship(name) {
  this.name = name;
  this.crew = [];
  this.propulsion = null;
}

Ship.prototype = {
  constructor: Ship,
  loadCrew: function(trainedCrew) {
    for(var i = 0; i < trainedCrew.length; i++) {
      this.crew.push(trainedCrew[i]);
      console.log("Welcome aboard, " + trainedCrew[i].name);
    }
  },
  captain: function() {
    var crewCount = this.crew.length;
    return this.crew[Math.floor(Math.random() * crewCount)]
  },
  mountPropulsion: function(unmountedPropulsion) {
    this.propulsion = unmountedPropulsion;
    console.log("Propulsion mounted!");
  },
  takeoff: function() {
    if (this.propulsion.fire()) {
      console.log("To infinity, and BEYOND");
    } else {
      console.log("takeoff unsuccessful!");
    }
  }
}
```

Now that we have ES6 classes, which underneath the hood is just a function, the below does the same thing for us but saves us the trouble of writing out `prototype`

```js
class Ship {

  constructor(name) {
    this.name = name;
    this.crew = [];
    this.propulsion = null;
  }

  let loadCrew = (trainedCrew) => {
    for(var i = 0; i < trainedCrew.length; i++) {
      this.crew.push(trainedCrew[i]);
      console.log("Welcome aboard, " + trainedCrew[i].name);
    }
  }

  let captain = () => {
    var crewCount = this.crew.length;
    return this.crew[Math.floor(Math.random() * crewCount)]
  }

  let mountPropulsion = (unmountedPropulsion) => {
    this.propulsion = unmountedPropulsion;
    console.log("Propulsion mounted!");
  }

  let takeoff = () => {
    if (this.propulsion.fire()) {
      console.log("To infinity, and BEYOND");
    } else {
      console.log("takeoff unsuccessful!");
    }
  }
}

// you'll notice some of this is using old school es5 such as var and for iterations, these can easily be refactored to ES6 syntax, this is just an example to display what new class syntax in ES6 is giving is in terms of constructor / prototype syntax
```





---
