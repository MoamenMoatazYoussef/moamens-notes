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

**Note:** Avoid extending built-in objects e.g. Array, String, etc.

## Prototypical Inheritance
``` js
function Circle(r) {
	this.r = r;
}

Circle.prototype.draw = function() {
	//code of the draw method.
};

Circle.prototype.duplicate = function() {
	//code of the duplicate method.
};
```

What if we want to create an object Square, having the same method ```duplicate``` having the same implementation?

We'll make an object ```Shape```, define the method ```duplicate``` there, then INHERIT IT by Circle and Square.
``` js
// here's the Shape constructor
function Shape() {

}

// now we define the method "duplicate" in the Shape prototype
Shape.prototype.duplicate = function () {
	console.log("duplicate");
}

// here's the Circle constructor
function Circle(r) {
	this.r = r;
}

// so far, Circle inherits from Circle.prototype, which inherits from Object.prototype
// and Shape inherits from Shape.prototype, which inherits from Object.prototype

// so, to set up inheritance here, we need Circle.prototype inherit from Shape.prototype
// we'll use Object.create:
// Object.create(Shape.prototype);
// this method creates an object whose prototype is whatever we pass as argument.
// so, to make Circle.prototype inherit from Shape.prototype, we need to assign the
// return value of this function to Circle.prototype, like this:

Circle.prototype = Object.create(Shape.prototype);

// now let's try it:
const c = new Circle(7);
c.duplicate(); // Output: duplicate
```
This is how we inherit using prototypes. <br/>

### Constructor resetting problem

...but, there's a problem, this method erases the Circle constructor function, we no longer can create Circles dynamically using the constructor.

If we do this like that:
``` js
new Circle.prototype.constructor(67); // This will return a SHAPE
```

So, we need to reset our constructor again (reset as in re-set):
``` js
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;
```

This is a best practice, and it's strongly recommended to do this everytime we re-set the prototype of any object.

### Calling the super constructor
How do we call the Shape constructor from the Circle object?
This will NOT work:
``` js
function Circle(radius, color) {
	Shape(color);
	this.radius = radius;
}
```
Because, remember how the ```new``` keyword works:
- Creates a new object,
- Binds *this* to that object,
- returns this object, or *this*.

Here, calling the Shape constructor will also have *this*, but we didn't call it using new (and we don't want to, because now there's a new object other than the object returned from the Circle operator), so that means the color property is set on the GLOBAL object, the *window*.

To solve this, we use the .call method:
``` js
function Circle(radius, color) {
	Shape.call(this, color);
	this.radius = radius;
}
```
the .call method takes a reference to an object on which to call the function, which is the Shape constructor, so now the color property is created in that object passed (As reference), which is *this*, which is THE new object created by the keyword ```new```.

i.e. now we are setting radius and color on the same object and returning it.

### Can we get a little cleaner?
Do we really have to do this everytime?
``` js
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;
```

Can we do this:
``` js
function extend(Child, Parent) {
	Child.prototype = Object.create(Parent.prototype);
	Child.prototype.constructor = Child;
}

extend(Circle, Shape);
```

Yes, we can do that :)

### Overriding
``` js
function extend(Child, Parent) {
	Child.prototype = Object.create(Parent.prototype);
	Child.prototype.constructor = Child;
}

function Shape() {}

Shape.prototype.duplicate = function() {
	console.log("duplicate shape");
}

function Circle(radius) {
	this.radius = radius;
}

extend(Circle, Shape);

// overriding
Circle.prototype.duplicate = function() {
	console.log("duplicate circle");
}
```
Why does this work? <br/>
Because of how prototypical inheritance works in JS:
- Walks up the prototype chain.
- Picks the first implementation found for a function.
So, the Circle's duplicate will be found first if we're executing it using a Circle object.

What if we want to call the original implementation using the overriding implementation?
``` js
Circle.prototype.duplicate = function() {
	Shape.prototype.duplicate.call(this);
	console.log("duplicate circle");
}
```

### Polymorphism
``` js
function extend(Child, Parent) {
	Child.prototype = Object.create(Parent.prototype);
	Child.prototype.constructor = Child;
}

// Shape
function Shape() {}

Shape.prototype.duplicate = function() {
	console.log("duplicate shape");
}

// Circle
function Circle(radius) {
	this.radius = radius;
}

extend(Circle, Shape);

Circle.prototype.duplicate = function() {
	console.log("duplicate circle");
}


// Square
function Square(sideLength) {
	this.sideLength = sideLength;
}

extend(Square, Shape);

Square.prototype.duplicate = function() {
	console.log("duplicate square");
}

const c1 = new Circle(7);
const s1 = new Square(8);
const o1 = new Shape();

const shapes = [
	c1,
	s1,
	o1
];

// this will call the implementation
// 
for(let shape of shapes) {
	shape.duplicate();
}
```
Now THAT is awesome :)

### When to use prototypical inheritance
Imagine we create Animal which has eat() and walk(), from it we inherit by Person and Dog.
If we create Shark (baby shark do doo do do do do), we can't inherit from Animal because fish can't walk.

A solution may be to create Animal, then inherit by Mammel and Fish, then inherit Person and Dog from Mammel, then inherit Shark from Fish.

That means we changed our entire hierarchy.

So, as a rule of thumb:
- Don't create inheritance for more than *one* level.

Another solution: ***Composition*** <br/>, define the features as independent objects:
- Define canWalk, canEat, canSwim as Objects containing the respective features.
- Define Person, this person contains the three objects.
- Define Shark(do doo do do do do), that shark contains only canEat and canSwim.

How?
``` js
const canEat = {
	eat: function() {
		this.hunger--;
		console.log("eating");
	}
}

const canWalk = {
	walk: function() {
		console.log("walking");
	}
}

// now, we'll use Object.assign, a method that copy the properties of 
// a certain object to another object.
// for example, let's try this with an empty object.
const person = Object.assign({}, canEat, canWalk);
```
Now, person has both methods defined in canEat and canWalk i.e. eat and walk

We can do that using a constructor too by assigning the PROTOTYPE of Person, so whenever we create a person object, it has the methods already:
``` js
function Person() {}
Object.assign(Person.prototype, canEat, canWalk);

const person1 = new Person();
```

Let's define canSwim and Shark:
``` js
const canEat = {
	eat: function() {
		this.hunger--;
		console.log("eating");
	}
}

const canWalk = {
	walk: function() {
		console.log("walking");
	}
}

const canSwim = {
	swim: function() {
		console.log("swimming");
	}
}

function Person() {}
Object.assign(Person.prototype, canEat, canWalk);

function Shark() {}
Shark.prototype.sing = function() {
	console.log("Baby shark do doo do do do do");
}
Object.assign(Shark.prototype, canEat, canSwim);

const person = new Person();
const shark = new Shark();

// now, person has the methods eat and walk, 
// and shark has the methods eat and swim
```

We can extract the Object.assign in a function called *mixin*:
``` js
function mixin(target, ...mixin) {
	Object.assign(target, ...sources);
}
```
This is called a Mixin.

## ES6 Syntactic Sugar
### Classes
This is pre-es6:
``` js
function Circle(r) {
	this.r = r;
	this.draw = function() {
		console.log('draw');
	}
}

const c = new Circle(7);
c.move = function() { 
	console.log("move"); 
};
```

This is the same code, in es6:
``` js
class Circle {
	constructor(r) {
		this.r = r;
		this.move = function() { 
			console.log("move"); 
		};
	}
	
	draw() {
		console.log('draw', this.r);
	}
}

const c = new Circle(7);
```

See how it looks more familiar to those who used OOP or structured languages? <br/>

***Note:*** Classes here are NOT the same as classes in Java, C++, etc., They are the same as objects and prototypes we talked about before, this is just syntactic sugar.
i.e. the method draw will be on the PROTOTYPE, not on the object itself.

If you want a method to be on the object itself, put it in the constructor, like the move function.

Classes in ES6 have a small enhancement: they enforce you to use the "new" keyword.

There are two ways to define classes (like functions):
``` js
// class declaration
class Circle {

}

// class expression
const Square = class {

}
```
Unlike functions, though, class declarations are **NOT** hoisted.
That's why class expressions are rarely used, and that's what you should do.

### Static methods
Just like any static method in any language, we can define static methods here, we use them by calling them from the reference of the Class itself, not an instance of it, like Java.
``` js
class Circle {
	constructor(r) {
		this.r = r;
		this.move = function() { 
			console.log("move"); 
		};
	}
	
	draw() {
		console.log('draw', this.r);
	}
	
	static tryMe(str) {
		console.log(str);
	}
	
	// this is a static function that acts like
	// a factory, you give it a valid JSON string 
	// containing a radius property and it returns a
	// circle whose radius is the value of that
	// property in the JSON
	static parse(str) {
		const radius = JSON.parse(str).radius;
		return new Circle(radius);
	} 
}

const c1 = new Circle(7);
const c2 = new Circle(8);

// we can't do this:
// c1.tryMe("this won't work")

// but we can do this (using the CLASS)
Circle.tryMe("this will work");

const c3 = Circle.parse('{ "radius": 1 }');
```

### The bane of every JS developer's existence: "this"
``` js
const Circle = function() {
	this.draw = function() {
		console.log(this);
	}
}

const c = new Circle();
// the "this" keyword inside the function points to the object c
// this is the normal behavior we're used to
c.draw();

const draw = c.draw;

// the "this" keyword inside the function points to the window object,
// the global object, because we didn't call it using an object reference
draw();
```

The second behavior can be changed if you enable **strict mode**, the "this" keyword will **NOT** point to the window global object, it will be *undefined*.

This is done to avoid modifying the global object by accident, which is a BAD thing.

By default, **the bodies of classes in ES6 are executed in Strict mode.**

### Private members
To make a member private, some developers do one of two (or both):
- prefix its name with an underscore "_", note that **THIS IS NOT ABSTRACTION**, this just tells other developers working on the code that this should not be used in public, but it **CAN** be used in public.
- Use ES6 symbols to implement REAL abstraction.
Symbols? (Basically they are unique identifiers);
If you compare Symbol() with Symbol(), you get *false*, because each one is unique.
``` js
const _radius = Symbol();

class Circle {
	constructor(r) {
		this[_radius] = radius;
	}
}

const c = new Circle(7);

// this won't work, and that's what we want
// now it's REALLY abstracted.
c.radius;
c._radius;

// however, we can hack it by using Object.getOwnPropertySymbols
// this returns an array of symbols containing the symbols used in the
// class, now we can use them like this:
const radiusName = Object.getOwnPropertySymbols(c))[0];

// now this works, sadly :(
c[radiusName];


// there's no current way to make it more private
// so we'll work with symbols for now
```

How do we implement private methods?
``` js
const _radius = Symbol();
const _draw = Symbol();

class Circle {
	constructor(r) {
		this[_radius] = radius;
	}
	
	// this is called a computed property name,
	// basically whatever's between the square brackets is
	// evaluated and used as property name:
	[_draw]() {
		console.log("draw, private mode");
	}
}

const c = new Circle(7);
```

### Using maps to make Private






















