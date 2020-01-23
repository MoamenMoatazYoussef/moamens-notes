# OOP in JS

## Review on the four pillars of OOP
### Encapsulation
Related data + functions that operate on them 
Are all put in a single unit, they're "encapsulated" in it, like a capsule.

"The best functions are those with no parameters"  - Uncle Bob (Robert C. Martin)

### Abstraction
Hiding complexity of properties and methods from the developer, so they just USE simple interfaces without needing to LEARN.
Also, we can change these complexities without worrying about the rest of the app's code.

### Inheritance

### Polymorphism

## Objects in JS
Creating objects the normal way:
``` js
let circle = {
	radius: 1,
	location: {
		x: 3,
		y: 4
	},
	draw: function() {
		console.log(`drawing a circle with radius: ${this.radius} at (${this.location.x}, ${this.location.y})`);
	}
};

circle.draw();
```

Creating objects the Factories way:
``` js
// to avoid code copying, we'll convert *circle* to a function that returns a circle.
// this is a factory function.
// NOTE: for simplicity, we'll remove location

function createCircle(radius) {
	return {
		radius,
		draw: function() {
			console.log(`drawing a circle with radius: ${this.radius}`);
		}
	};
}

let circle1 = createCircle(1);
let circle2 = createCircle(2);

circle1.draw();
circle2.draw();
```

Creating objects the Constructor way:
``` js
// We'll use the *new* keyword
// when a function is called using "new":
//	1. An empty object is created.
//	2. The "this" keyword is bound to that object i.e. points to it.
//	3. If nothing specific is returned from the function, it returns "this" 
//		i.e. reference to the empty object.

// so, calling the factory function using "new" will create a new object, so
// we can use this.radius because it will simply set properties of that object.
// we don't even need to return something.

// that function is not a factory anymore, it's a *Constructor*

function Circle(radius) {
	this.radius = radius;
	this.draw = function() {
		console.log("draw");
	};
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

circle1.draw();
circle2.draw();
```

What's the difference between Factory and Constructor?
- The constructor has the benefit of making it look like creating an instance of a class (in JS we don't have classes).
- The factory has the benefit of not making things seem like true OOP.

Don't get hung up on which to use, just use whatever you want, or use what's already used in the project you're in.

## Constructors
All objects in JS has a property "constructor", which points to the Constructor function that created them.
``` js
function Circle(radius) {
	this.radius = radius;
	this.draw = function() {
		console.log("draw");
	};
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.constructor); // Will point to the constructor function
```

If you create an object using an object literal or a factory, JS uses an internal built-in constructor function to make it. If you try to log that object's .constructor property, this internal constructor will be logged.

We can call functions using .call(), that way we can access that empty object created by "new", it's the first arg of the function, the rest of the arguments of .call() are the arguments of the function to be called.
``` js
createCircle.call({}, 2); 
createCircle.apply({}, [2]); // the same as .call, but args are passed in array 
```

Why does that work?
Because actually, in JS, functions are OBJECTS.

## Functions are Objects
EVERYTHING in JS is an object.
Even functions, they are objects of the Function object, created by the Function() built-in constructor.
``` js
// the factory code here

console.log(createCircle.name); // Output will be the name of the function "createCircle"
console.log(createCircle.constructor); // Output will be Function()
```

Function() takes two args:
	name (string): the name of the function,
	code (string): the code of the function, is passed kinda like eval() (DO NOT USE EVAL)
	
Don't use Function() to create functions, just use the normal way.

## Value vs Reference
The age-old question.
In JS, **these are passed by value**: Number, String, Boolean, undefined, null, Symbol
And **these are passed by reference**: Object, Function, Array
	
**Primitives -> Value** <br/>
**Objects -> Reference** <br/>

## Working with Objects
### Adding/Removing properties
``` js
let coordinates = {
	x: 10,
	y: 20
};

// Adding properties
coordinates.z = 30;

// Another way of adding properties
coordinates["time"] = "50s";

console.log(coordinates.x, coordinates.y, coordinates.z, coordinates.time); //Output: 10 20 30 50s

// Removing properties
delete coordinates.time; //This sets coordinates.time to undefined, and removes the property altogether
```
Why use brackets instaed of . ?
- When you want to access properties dynamically.
- When property names contain invalid variable characters e.g. "-".

### Enumerating (Going over) Properties
``` js
for(let key in coordinates) {
	console.log(key, coordinates[key]);
}
/*
Output: 
x 10
y 20
z 30
time 50(if you haven't deleted the time property)
*/

// Getting an array of the keys
const keys = Object.keys(coordinates);
console.log(keys); //Output: ["x", "y", "z", "time"]

// Checking if an object contains a certain key:
if("third-dimension" in coordinates) {
	console.log("The coordinates are out of this universe!")
};
```

## Abstraction
Remember the circle example?
``` js
function Circle(r) {
	this.r = r;
	this.location = { x: 0, y: 0};
	
	this.computeOptimumLocation = function() {
		// some code here
	};
	
	this.draw = function() {
		this.computeOptimumLocation();
		console.log('draw');
	};
}

const c = new Circle(1);
c.computeOptimumLocation(); // that is wrong here...
```
The function computeOptimumLocation should be private, we can't invoke it from outside.
So, we want to hide it, and expose only the essentials.

To do this, we do this: (haha, JS)
- Instead of this.computeOptimumLocation = function() {...}, we declare it as a local variable.
- Why? because CLOSURE will enable any object of instance Circle to access it, but we can't access it directly.
``` js
function Circle(r) {
	this.r = r;
	this.location = { x: 0, y: 0};
	
	let computeOptimumLocation = function() {
		// some code here
	};
	
	this.draw = function() {
		computeOptimumLocation();
		console.log('draw');
	};
}

const c = new Circle(1);
c.computeOptimumLocation(); //ReferenceError: computeOptimumLocation is not a function (because we're out of scope)
c.draw(); //No error, because of CLOSURE, this.draw CAN ACCESS computeOptimumLocation, scope play.
```

### Getters and Setters
``` js
function Circle(r) {
	this.r = r;
	let location = { x: 0, y: 0};
	
	this.getLocation = function() {
		return location;
	}
	
	let computeOptimumLocation = function() {
		// some code here
	};
	
	this.draw = function() {
		computeOptimumLocation();
		console.log('draw');
	};
}

const c = new Circle(1);
c.getLocation();
c.draw();
```

We can also do this in a cleaner way:
``` js
function Circle(r) {
	this.r = r;
	let location = { x: 0, y: 0};
	
	Object.defineProperty(this, "location", 
		get: function() { 
			// We can do stuff here before we return, like authenticate maybe?
			return location; 
		},
		set: function(value) {
			// We can perform some validation here
			if(!value.x || !value.y) {
				throw new Error("Invalid location");
			}
			
			location = value;
		}
	);
	
	let computeOptimumLocation = function() {
		// some code here
	};
	
	this.draw = function() {
		computeOptimumLocation();
		console.log('draw');
	};
}

const c = new Circle(1);
c.location; //this executes the getter function that gets the location property
c.location = { x: 10, y: 20 };
c.location = 7; // Throws error, it's not an object that has x or y properties.
```

## Inheritance in JS
In JS, we don't have classes, we have objects.
So inheritance here is NOT the normal, *classical* inheritance we're used to.

It's another type called ***Prototypical Inheritance*** <br/>


### Prototype?
Superclass/Parent -> Prototype
Subcalss/Child -> subclass or child brdo.

A class inherits all the members defined in its parent or *prototype*.
The ```__proto__``` object that appears in the console when you log any object points to the object's Prototype.

The object ```Object.prototype``` is the base object in JS i.e. all created objects have it as *prototype*, and ```Object.prototype``` itself doesn't have a *prototype*.

Also, the ```Object.prototype``` object has ONE instance in memory, so all objects have Object as prototype, this means they refer to the SAME INSTANCE.

Note: don't use __proto__ in production, it's deprecated.

So what is inheritance here?

### Prototypical inheritance
When we access a property or method of an object, JS works like this:
1. Looks at current object for the called property/method.
2. If found, use it.
3. If not, walk up one level to the first immediate prototype and repeat step 1.
Repeat until either property/method is found at one of the prototypes, or we reach ```Object.prototype``` and still not find it.

For example, all created arrays have the object Array as prototype, so:
``` js
let a = [1, 2, 3];
a.push(4);
a.toString();
```
When we call a.push, the a.push isn't defined in our array, so JS goes to the immediate prototype, which is ```Array``` object, and it finds the method .push there, and executes it.

When we call toString, the a.toString isn't defined in our array, so JS does the same thing, but it's also not defined in ```Array```, so JS repeats and reaches the object ```Object.prototype```, which has the method toString, so it's executed.

Also:
``` js
function Circle(r) {
	//code
}

const c1 = new Circle(1);
const c2 = new Circle(2);
```

If we log c1, we'll find that it has a prototype that consists of:
- constructor: this points to the constructor we used.
- __proto__: which points to ```Object.prototype```.

So, c1 and c2 have the same value of the __proto__ property. In other words: <br/>
**Objects created by a given constructor will have THE SAME prototype**

### Property attributes
Let's try something new:
``` js
let m = { name: "Moamen" };
console.log(m.toString()); // Output: "[object Object]"

for(let key in m) {
	console.log(key); // Output will NOT have the toString method
}
```
Why can't we iterate over toString()?
Because **Properties have attributes attached to them**, some of these attributes prevent enumeration over them.

For example:
``` js
let objectBase = Object.getPrototypeOf(m);
console.log(
	Object.getOwnPropertyDescriptor(objectBase, "toString");
);
```
Object.getOwnPropertyDescriptor(the_object, "the property's name");

The outpue will be:
``` js
{
	configurable: true,
	enumerable: false,
	value: *f* toString(),
	writable: true
}
```
See? The *enumerable* property is set to false i.e. we can't iterate over this proerty.
What about the rest? <br/>
- Configurable: means we can delete the member if we want to.
- Enumerable: false.
- Writable: we can overwrite it.
- Value: the default implementation of the method.

In our own objects, we can set these attributes. <br/>

For example:
``` js
Object.defineProperty(person, 'name', {
	writable: false, //i.e. this is read-only, if we try to write it, nothing happens
	enumerable: false, //i.e. this won't show up in Object.keys()
	configurable: false, //i.e. we can't delete this property
});
```

### Prototypes of functions
``` js
function Circle(r) {
	//code
}

const c1 = new Circle(7);

console.log(c1.__proto__); 
//prints the prototype of c1 (deprecated)

console.log(Object.getPrototypeOf(c1)); 
//prints the prototype of c1

console.log(Circle.prototype); 
//prints the prototype associated with any object created using this
//constructor i.e. the prototype of c1

//so, these 3 logs output the same thing..
```

### Prototype vs Instance
``` js
function Circle(r) {
	//code
	this.draw = function() {
		//code
	}
	//code
}

const c1 = new Circle(1);
const c2 = new Circle(2);
```

If we have 1000 circles, we'll have 1000 copies of the draw method i.e. memory is wasted.
Why not use prototypical inheritance?
``` js
function circle(r) {
	//code without the draw method
}

Circle.prototype.draw = function() {
	//code of the draw method.
};

const c1 = new Circle(1);
const c2 = new Circle(2);
```
Now, both c1 and c2 don't have a draw method, but their PROTOTYPE has the draw method.
```draw``` is now a **prototype member**, if we have 1000 circle objects, all of them will reference ONE draw method, the one in their prototype.

Also, because of how the prototypical inheritance works (look for sth, go up one level, repeat), we can override methods in higher prototypes by defining them in lower prototypes e.g. let's override the toString method:
``` js
function circle(r) {
	//code without the draw method
}

Circle.prototype.draw = function() {
	//code of the draw method.
};

Circle.prototype.toString = function() {
	return 'Circle with radius' + this.r;
}

const c1 = new Circle(1);
const c2 = new Circle(2);
```

Now, when we call c1.toString() method, JS won't go to the toString method defined in ```Object.prototype```, because it will find the toString method defined in Circle.prototype, so it will use that method.

Also, we can do this:
``` js
function circle(r) {
	//code without the draw method
	
	this.move = function() => {
		this.draw();
		console.log("move");
	}
}

Circle.prototype.draw = function() {
	//code of the draw method.
};

Circle.prototype.toString = function() {
	return 'Circle with radius' + this.r;
}

const c1 = new Circle(1);
const c2 = new Circle(2);
```
The this.draw() will call the method, which will be found by JS in the Circle.prototype.

### Changing the prototype
We can change the prototype anytime i.e. we can do this:
``` js
const c1 = new Circle(7);
Circle.prototype.draw = function() {
	//code of the draw method.
};

c1.draw(); //will execute the new method we just defined.
```

And we can do this:
``` js
Circle.prototype.draw = function() {
	//code of the draw method.
};
const c1 = new Circle(7);

c1.draw(); //will execute the new method too.
```

### Iterating over prototype members
``` js
Object.keys(c1); //This will contain r and move, but will not contain draw
for(let key in c1) {
	//will iterate over r, move, AND draw 
	//i.e. over all members of instance + prototype
}
c1.hasOwnProperty('draw'); //output: false
```