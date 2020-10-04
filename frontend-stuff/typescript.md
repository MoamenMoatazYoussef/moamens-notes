Typescript:
===========
Typescript is a superset of JS i.e. any JS code will work in TS apps.

Typescript has additional features:
- Strong typing
- OOP features e.g. classes, interfaces, constructors, access modifiers, etc.
- Compile-time error handling.
- Is supported by great tools e.g. intellisense, etc.

Browsers don't support Typescript, and is unlikely for that to happen in the future.
So, typescript is transpiled to JS.

Installing typescript:
>> npm install -g typescript
- The command tsc runs the typescript compiler
>> mkdir ts-hello
>> cd ts-hello
>> code main.ts
	- this will create a .ts file and open it with vscode.
``` js
function log(message) {
	console.log(message);
}

var message = 'Hello World';
log(message);
```
Notice that this is just plain old JS.
>> tsc main.ts
- A file main.js will be generated i.e. we transpiled the .ts file.
- If you open it, you'll see the same code we wrote.
- You can execute the code using nodejs
>> node main.js

tsc generates a .js transpiled file EVEN if there are errors in the .ts code, so be careful.
But, tsc has intellisense i.e. it tells you warnings and errors at COMPILE time or even at...um, "code writing" time?.
 
Variable declarations in TS:
- main.ts:
``` js
// This is a ts type annotation, basically we tell ts what type this variable is
let a: number;
a = 1;

// because of the type annotation, this line will give us an intellisense warning
a = 'a';
``` 
- Types in JS: number, boolean, string, any, number[] (this is an array of numbers), and Enum <3
``` js
const ColorRed = 0;
const ColorGreen = 1;
const ColorBlue = 2;

enum Color {
	Red,
	Green,
	Blue
};

let backgroundColor = Color.Blue;

// this works normally like any enum
```
- You can tsc this code to see how enum can be implemented in JS.

Type assertions:
``` ts
let message; // the message is declared as any
message = 'abc';
let endsWithC = message.endsWith('c');

// because message is declared as any (the default if we don't give a type annotation)
// even if we give it a string value later, we don't get the string intelliSense here
// so, to solve this, we can use Type Assertions:
let endsWithC_firstWay = (<string>message).endsWith('c');
let endsWithC_secondWay = (message as string).endsWith('c');
```
This does NOT change the variable's type at runtime, it just tells the compiler the type of the variable.

Type annotations in functions to restrict the structure and type of arguments passed to the function:
``` js
let drawPoint(point) => {
	// the code here
}

drawPoint({
	x: 1,
	y: 2,
}); //this is valid

drawPoint({
	name: 'moamen',
	age: 25
}); // this is invalid, but JS won't complain :(

// so the solution is to re-declare the function but then 
// use type annotations to specify the type and structure of arguments we're looking for
```
Like this:
``` js
let drawPoint = (point: {x: number, y: number}) => {
	// stuff
}
```
That's okay, but is kinda verbose.

So, ts comes to the rescue by giving us the blessing of INTERFACES (like Java, C#, etc.)
Which are just like interfaces in Java: they only have declarations.
``` js
interface Point {
	x: number,
	y: number
}

let drawPoint = (point: Point) => {
	// stuff
}
```

As with any language, introducing types or classes is best written by the Pascal case.

But there's a problem...
Cohesion: things that are related should be part of one unit.

In the last code, we have a standalone interface of the Point, and a standalone function drawPoint.
These shouldn't be separate like this, it's better to encapsulate them in one unit.

In OOP, that's a Class.

So, let's use that:
- Change the interface to a class.
``` js
class Point {
	x: number;
	y: number;
	
	draw () {
		// the code of drawPoint
	}
	
	// any other function related to Point is written here
}

// now, we can do this:
let point: Point;

// but we can't yet do this
// point.draw();

// That's because, when defining an instance of a custom type, 
// we need to explicitly allocate memory to it.
// we do this using the new keyword, just like a constructor :)
// we can also remove the type annotation, tsc will infer that this is of a Point type.
let point = new Point();
point.draw();

// So far so good, but if we'll access the properties x and y, they will be undefined
// We can do this:
let point = new Point();
point.x = 1;
point.y = 2;
point.draw();
```

- Constructor:
``` js
class Point {
	x: number;
	y: number;
	
	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
	
	draw () {
		// the code of drawPoint
	}
	
	// any other function related to Point is written here
}

let point = new Point(1, 2);
point.draw();
```

But, in JS we only can have one constructor, so what if we don't want to provide x and y values?
Easy, we make x and y OPTIONAL parameters.
``` js
class Point {
	x: number;
	y: number;
	
	constructor(x?: number, y?: number) {
		this.x = x;
		this.y = y;
	}
	
	draw () {
		// the code of drawPoint
	}
	
	// any other function related to Point is written here
}
```
A rule in many programming languages, if a parameter is optional, all parameters on its right should also be optional.

Access modifiers:
``` js
class Point {
	private x: number;
	private y: number;
	
	constructor(x?: number, y?: number) {
		this.x = x;
		this.y = y;
	}
	
	draw () {
		// the code of drawPoint
	}
	
	// any other function related to Point is written here
}
```

We can also make it shorter like this:
``` js
class Point {

	// nothing inside the constructor, this shorthand version
	// declares private x and y, and assigns the incoming values to x and y
	constructor(private x?: number, private y?: number) {}
	
	draw () {
		// the code of drawPoint
	}
	
	// any other function related to Point is written here
}
```

Getters & Setters:
``` js
class Point {
	constructor(private x?: number, private y?: number) {}
	
	draw () {
		// the code of drawPoint
	}
	
	getX() {
		return this.x;
	}
	
	setX(x) {
		// validation can be written here
		this.x = x;
	}
	
	// any other function related to Point is written here
}
```

Or, we can do this:
``` js
class Point {
	constructor(private x?: number, private y?: number) {}
	
	draw () {
		// the code of drawPoint
	}
	
	get X() {
		return this.x;
	}
	
	set X(x) {
		// validation can be written here
		this.x = x;
	}
	
	// any other function related to Point is written here
}

let point = new Point(1, 2);
point.X = 7;

console.log(point.X); // it will be 7, not 1
```

Notice that we called the getter an uppercase X, because if it was a lowercase X it will clash with the private field x.

A convension is to name any private variables using a prefix of an underscore _, so:
``` js
class Point {
	constructor(private _x?: number, private _y?: number) {}
	
	draw () {
		// the code of drawPoint
	}
	
	get x() {
		return this._x;
	}
	
	set x(x) {
		// validation can be written here
		this._x = x;
	}
	
	// any other function related to Point is written here
}
```

That's it, basics of Typescript :)
