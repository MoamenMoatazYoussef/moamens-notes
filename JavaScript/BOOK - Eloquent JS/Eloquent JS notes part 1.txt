Eloquent JS notes part 1:
=========================
Tips:
-----
- anything between //// and //// is code.
- notes will be divided according to projects in the book, so:
    - part 1: chapters 1 until 6.
    - part 2: chapters 8 until 11.
    - part 3: chapters 13, 14, 15 (we read these before).
    - part 4: chapters 17 until 20.
- chapters 7, 12, 16, 21 are projects, I'm not sure if there will be time to do them honestly so I don't know if I'll include them :'D
- I assume that you're not a noob at programming so some stuff is skipped e.g. what are loops, etc., not that being a noob is bad or anything, we've all been noobs xD
-------------------------------------------------------------------------------------------------------------------------------
Chapter 1: values, types, operators:
------------------------------------
- number: 64-bit numbers. Overflow is considered only if you're dealing with truly astronomical numbers.
- (Infinity, -Infinity, NaN): Special numbers, NaN is when we divide 0/0, Infinity - Infinity, etc. undefined numbers.
- the condition (NaN == NaN) returns FALSE, and that's the only number that's not equal to itself, because it really is a result of nonsensical computation.
- type coercion.
- The or operator || returns the left operand if it can be converted into True, or the value on the right otherwise, this can be used to set default values for example:
    if we have an input value we put it on the LEFT SIDE, and put a default value on the RIGHT SIDE, so if the input is empty we get the default value.
- The And operator && does the same but the other way around: returns the left value if it's false, otherwise return right.
- Both operators are efficient in some cases:
    - if the left operand of the || is true, JS doesn't compute the right one because the result is certainly True.
    - Same for && but false.
-------------------------------------------------------------------------------------------------------------------------------
Chapter 2: Program Structure:
-----------------------------
Nothing special, if you're not a noob at programming you know everything here.
-------------------------------------------------------------------------------------------------------------------------------
Chapter 3: Functions:
---------------------
- Functions that don't return anything or "return;", return undefined.
- Obviously, the computer uses a Call Stack to track which functions it was in.
- If we use too much recursion, the stack may fill and the computer will be out of memory, this is called Stack Overflow Error, and yes, our glorious guide Stackoverflow.com is named after this error.
- If we have a function that takes one argument, and we call it with three arguments, it won't make an error, it would use the first one.
- If we have a function that takes three arguments, and we give it one, the other two will be "undefined".
- Recursion in JS is slower than iteration(loops)
    And I hate recursion anyways even if it's useful in DnC and dynamic programming, debugging it is impossible and I just hate it.
-------------------------------------------------------------------------------------------------------------------------------
Chapter 4: Data structures: objects and arrays:
-----------------------------------------------
(read later)
I skimmed it and found nothing new more than what we know from codecademy but I'll read it later in details.
-------------------------------------------------------------------------------------------------------------------------------
Chapter 5: Higher-order functions:
----------------------------------
- Basically they are functions that take other functions as arguments, or return functions.
- They're named after mathematics terms, that's why they're called higher-order functions.
- but that doesn't really mean anything.
////
function doSomething(n) {
    //code for doing something
}

function repeatAction(n, action) {
    for(var i = 0 ; i < n ; i++) {
        action(i);
    }
}

repeatAction(5, doSomething); //Here, we pass the name of the function as a REFERENCE of the function, now it will replace the code
                              //inside and operate on the argument given to it (Just like C).
//Example, we can do this:
repeatAction(7, console.log); //we give it number of iterations: 7, and reference to console.log function, so now it will print 0, 1, 2, ... 7.

//Example, We can make anonymous functions and pass them:
repeatAction(9, function(n) {
    //code for this function using n as argument;
});

repeatAction(9, (n) => {
    //code for this function using n as argument
});
////

- Higher-order functions help us abstract ACTIONS, not just values.
////
//Example, we can make functions that create NEW functions:
function greaterThan(n) {
    return function(m) {
        return m > n;
    };
}

console.log(greaterThan(7, 3)); //prints True

//Or functions that return functions:
function doStuff(function_argument) {
    return function(...args) {
        return function_argument(...args);
    };
}

var result = doStuff(Math.max)(1, 3, 5, 7, 10, 9); //The first bracket contains the function argument i.e. the argument of the higher-order function.
                                                   //The second bracket has arguments for the inner function.
                                                   //Now, the inner function is specified as Math.max, and is referenced,
                                                   //But it needs arguments, so 1, 3, 5, 7, 10, 9 are passed to it.
                                                   //Now it will operate normally and return the result, which is the maximum of the arguments, which is 10.

console.log(result); //prints the maximum of the arguments passed to the inner function 10

//NOTE: the ...args is called a Rest Parameter, it's basically an array of indefinite number of parameters, so we can pass any number of args into it.
//Example, the Max function used above can take two numbers, or 75 numbers.

//Example, we can make control flow functions:
function unless(test, then) {
    if (!test) then();
}

unless( 1 < 2, function() {
    console.log("false condition");
});
////

- forEach, the function used on arrays, is a higher-order function, and has a function argument passed to it.

- Higher-order functions are used in Data processing, we have:
    map: a function for arrays, which takes a function as argument and applies it to all elements, then returns a new array with mapped elements.
        the difference between it and forEach is that 'map' does NOT modify the OG array.
        ////
        map(array, transform);
            //transform: a function to be executed on each element.
        ////
    reduce: this function that combines all elements of an array into ONE value.
        ////
        reduce(array, combine, start);
            //combine: a function passed to determine how to combine elements e.g. sum, multiply, etc.
            //start: where to start in the array.
            //start has a default value of 0, so we can not pass it if we want.
        //Example:
        reduce([1, 2, 3], function(a, b) { return a + b; }); //we didn't pass 'start', it's as if we passed 0
        ////

- Higher-order functions are also used to compose operations and maintain their readability.
-------------------------------------------------------------------------------------------------------------------------------
Chapter 6: Objects part 2:
--------------------------
- It basically talks about how OOP principles apply to JS.

A) Encapsulation:
-----------------
JS don't have access modifiers (i.e. public, private, protected).
So there's some workarounds for this:
    - put an underscore'_' at the start of any property you want to be private.
        this does NOT make it private, it just indicates to anyone reading this code to not access this directly.

In JS objects can have methods (obviously), which are functions that operate on stuff inside the object.
If, in a method, we use the keyword 'this', it refers to the parent object.
////
function speak() {
    console.log(`This ${this.species} says: ${this.catchphrase}`);
}

let pikachu = {
    species: "Pikachu",
    catchphrase: "pika pika",
    speak //this refers to the function above
};

let meowth = {
    species: "Meowth",
    catchphrase: "Meowth, that's right!",
    speak
};

pikachu.speak(); //'This Pikachu says: pika pika'
meowth.speak();  //'This Meowth says: Meowth, that's right!'

//notice that the function is define OUTSIDE both objects, and it has the keyword 'this'
//when we called it from each object, 'this' referred to THAT object and got the property values from THAT object.
//also notice that the Meowth I was talking about is Team Rocket's meowth.

function motto() {
    console.log(`
		Prepare for trouble,
		make it double,

		to protect the world from devastation,
		to unite all people within our nation,

		to denounce the evils of truth and love,
		to extend our reach to the stars above,

		${this.member1}, ${this.member2},

		team rocket blasts off at the speed of light,
		surrender now, or prepare to fight,
		${this.pokemon.catchphrase}!
	`);
}

let teamRocket = {
    member1: "Jessie",
    member2: "James",
    pokemon: {
        species: "Wobuffet",
        catchphrase: "Wuuuu..buffet!" //whoever gets this reference is a LEGEND
    },
  	motto
}

teamRocket.motto();

//We can also do this

motto.call(teamRocket); //the function 'call' is called by a function, and takes the first argument as the OBJECT calling the function i.e. it takes 'this' as first argument but instead of passing 'this' we pass the object itself.
                        //it may take more than that, it may take more arguments, any other arguments are passed to the 'motto' function
//Example, if motto takes two arguments, we do this:
motto.call(teamRocket, argument_1, argument_2);
////

Watch closely this example:
////
function normalize() {
  console.log(this.coords.map( //'this' refers to the parent object, we're trying to access its coords property
      n => n / this.length     //'this' refers to the parent object also, even if it's in an INNER scope.
      ));
}

normalize.call({coords: [0, 2, 3], length: 5});
//output → [0, 0.4, 0.6]

//Now, normal functions have their own 'this' binding, so when we use 'this' we do NOT refet to the outside scope, or "wrapping scope", this means that we can't use 'this' inside a function to refer to a parent object unless:
//  1) the function is defined INSIDE the object.
//  2) the function is an ARROW function.
//So, if we rewrote the example above, but with the keyword 'function', the 'this' keyword inside it will refer to the function, which does NOT have a length property, so the code will NOT work!
 
function normalize() {
  console.log(this.coords.map( //'this' refers to the parent object, we're trying to access its coords property
      function(n) { return n / this.length }     //'this' refers to the parent object also, even if it's in an INNER scope.
      ));
}

normalize.call({coords: [0, 2, 3], length: 5});
//output → [NaN, Infinity, Infinity] //I think it assumed that length = 0 that's why we got NaN (0/0), and two infinities, but if we don't use 'return', it will be [undefined, undefined, undefined].
////

Prototypes:
-----------
In JS, objects have something called a 'Prototype', this is another object that is used as a source of any properties that are not directly defined in another object.
It's like a parent object.

So each object has a prototype object, it can have many (I think?).
All objects in JS have a common prototype, it's the parent object of everything, it's the great prototype, it's called 'Object.prototype'.

For example:
////
let empty = {};
console.log(empty.toString); //this will have an output, because the definition of toString is retrieved from this object's Prototype. 

console.log(Object.getPrototypeOf(empty) == Object.prototype);
//output: true
////

So, basically, JS is like a tree of objects, each object gets properties and methods from its parents, the root of this tree is 'Object.prototype', so ANY object can use its properties.
For example, the toString property is defined in Object.prototype, so ANY object created can use it.

Not all objects have Object.prototype as their DIRECT prototype, they instead have other prototypes and these prototypes have Object.prototype as their prototype (I feel I made that complicated :'D).
Example:
    In JS all functions are objects, their prototype is Function.prototype, which has Object.prototype as its prototype.
    Arrays have Array.prototype, same concept because I'm tired of using the word prototype already ;'D

We can create an object with a specific prototype:
////
let protoPokemon = {
    speak() {
        console.log(`This ${this.species} says: ${this.catchphrase}`);
    }
}

let pikachu = Object.create(protoPokemon);
pikachu.species = "Pikachu";
pikachu.catchphrase = "pika pika";
pikachu.speak();
////

This feels like classes, doesn't it? ;)

Classes:
--------
Prototypes are useful to define properties with values that are shared among all instances.
To create an instance, we have to make an object that derives from a prototype, AND has the property values that it should have.
To do that, we use a constructor function:
////
let protoPokemon = {
    speak() {
        console.log(`This ${this.species} says: ${this.catchphrase}`);
    }
}

function makePokemon(species, catchphrase) {
    let pokemon = Object.create(protoPokemon);
    pokemon.species = species;
    pokemon.catchphrase = catchphrase;
  	return pokemon; 
}

let p = makePokemon("Pikachu", "pika pika");
p.speak();
////

Or, we can use 'new', this makes the function called a constructor i.e.:
    1- an object with the right prototype is created,
    2- that object is bound to 'this' in the function,
    3- and then returned at the end of that function.
////
function Pokemon(species, catchphrase) {
    this.species = species;
    this.catchphrase = catchphrase;
}

Pokemon.prototype.speak = function() {
    console.log(`This ${this.species} says: ${this.catchphrase}`);
}

let chikorita = new Pokemon("Chikorita", "chiku chikuuu");
chikorita.speak();
////

Constructors by default have a property "prototype", which by default is 'Object.prototype'.
We can overwrite it if we want, and we can add properties to the new object like in the last example.

Btw, there is a difference between:
- the prototype that the constructor uses to make new objects, here it is Object,prototype. We can get that by Object.getPrototypeOf(Pokemon);
- the prototype OF the constructor itself, which is Function.prototype since it's just a function anyway. we can get that by Object.getPrototypeOf(chikorita);

Basically, constructors with properties are how classes are created in JS until 2015, now we have a better notation:
////
class Pokemon {
    constructor(species, catchphrase) {
        this.species = species;
        this.catchphrase = catchphrase;
    }
    speak() {
        console.log(`This ${this.species} says: ${this.catchphrase}`);
    }
}

let totodile = new Pokemon("Totodile", "totodile");
let psyduck  = new Pokemon("Psyduck", "Psi yui yuiii);
////
This is the same as above, but in a nicer look, the object created still has a prototype assigned by the constructor, and since we don't overwrite it, the default is Object.prototype.
Although, JS classes allow ONLY methods to be added to the prototype :(

We can use 'class' in statements like this:
////
let object = new class { getWord() { return "hello"; } };
console.log(object.getWord());
////

If a default property is present in the prototype/class and is overwritten, it's overwritten in the instance itself, not the class or prototype (just like any language).
We can also override methods of prototypes or classes.

Map data structure:
-------------------
It's basically a key-value pair data structure.
It's just like dictionary in python, unordered map in C++, hashMap in java, etc.

////
let pokemon_types = {
    pikachu: 'electric',
    vulpix: 'fire',
    totodile: 'water',
    chikorita: 'grass',
    haunter: 'ghost',
    onyx: 'rock',
    beedril: 'bug',
    kadabra: 'psycic',
    entei: 'legendary/fire',
    lugia: 'legendary/psychic/water',
    ho_oh: 'legendary/fire',
    kyogre: 'legendary/water',
    mewtwo: 'legendary/psychic',
    palkia: 'legendary',
    dialga: 'legendary',
    missingNo: 'glitch'
}

console.log(`Lugia is a ${pokemon_types['lugia']} pokemon.`);
console.log(`Vulpix is a ${pokemon_types['vulpix']} pokemon. `);
////

BUT, using objects as maps is DANGEROUS, because they still have Object.prototype's properties and methods, which may overwrite some methods if we use their names e.g. if we made an entry whose key is 'toString' for example.
So, we have some workarounds:
1) create an object with NO prototype using Object.create(null);, then using that object as a map.
2) using the Map class:
////
let pokemon_types = new Map();
pokemon_types.set('pikachu', 'electric');
pokemon_types.set('onyx', 'rock');
pokemon_types.set('dialga', 'legendary');

//we can get the value of a key using .get function
console.log(pokemon_types.get("pikachu"));

//We can check if a key exists in the map using .has function
console.log(pokemon_types.has("dialga")); //true
console.log(pokemon_types.has("vulpix")); //false
////

B) Polymorphism:
----------------
The same as polymorphism in any language.
Some objects override the toString method of Object.prototype, so when we call toString they call the suitable code for them.
So, we can make a class support any predefined method in a prototype (or interface) and override it if necessary, plug it in, and it will work ^_^

e.g. for/of loop, it loops on many data structures, but each data structure has its own method of iteration, all override the basic for/of loop.

Symbols:
--------
We can have many interfaces that use the same property name for different things, that is a bad idea but it can happen in JS, although not common.
But, there's a solution, property names are not always strings, they can be Symbols.

Symbols are created using the Symbol function:
Symbols are unique i.e. we can't create the same symbol twice:
////
let poke = Symbol("pokemon");
////

You can use symbols as property names, for example:
////
//Here we define the symbol
const speakSymbol = Symbol("speak");

//Then, we override the main function of 'speak' in the prototype with our own function using the symbol
Object.prototype[speakSymbol] = function() {
  console.log(this.catchphrase);
}

class Pokemon {
  constructor(species, catchphrase) {
    this.species 		 = species;
    this.catchphrase = catchphrase;
  }
  speak() {
    console.log(this.catchphrase);
  }
}

//here we use the normal function name to call it
let chikorita = new Pokemon("Chikorita", "chiku chikuu!");
chikorita.speak();

//here we use the symbol to call the function.
let cyndaquil = new Pokemon("Cyndaquil", "cynda...QUIIIIIL!");
cyndaquil[speakSymbol]();

//Note: cyndaquil is the cutest fire pokemon ever <3
////

Iterators:
----------
(Honestly I didn't quite understand that part so I skipped it for now, I'll return to it later, sorry :'D)

Getters, setters, statics:
--------------------------
The concept is the same as getters and setters, JS doesn't have access modifiers but getters and setters are still used to get and set properties.
i.e. we can get/set properties directly but it's better (for design reasons) to use getters and setters.

For the syntax, JS has preferred minimalistic approach:
////
let pokemon = {
  	species: "Magnemite",
    get pokemonSpecies() {
        return this.species;
    },
    set pokemonSpecies(species) {
        this.species = species;
    }
}

console.log('Who\'s that pokemon?');
//here we use the getter to get the species
//notice that we don't have to use the function syntax.
let whos_that_pokemon = pokemon.pokemonSpecies; 
console.log(`It's ${whos_that_pokemon}!`);

//here we just assigned the pokemonSpecies to a value
//we used the setter to do that
//notice that the syntax is really similar to any property
//which is very convenient
pokemon.pokemonSpecies = "Magneton";
console.log(`Congrats! your ${whos_that_pokemon} has evolved into ${pokemon.pokemonSpecies}!`);
console.log('Tadadadaaa daa daa da dadaaaaa!');
////

We can do that in classes too:
////
class Pokemon {
    constructor(species) {
        this.species = species;
    }

    get pokemonSpecies() {
        return this.species;
    }

    set pokemonSpecies(species) {
        this.species = species;
    }
}

let goldeen = new Pokemon("Goldeen");
console.log(goldeen.pokemonSpecies); //getter
goldeen.pokemonSpecies = "staryu"; //setter, you gotta be weird if you confuse a goldeen and a staryu honestly xD
////

Static:
-------
If we want to attach some properties to the Constructor itself, not the prototype, we can do that using static functions.
Static functions, having 'static' keyword before their name, are stored on the constructor, we can use them to make new instances i.e. like copy constructors.
////
class Pokemon {
    constructor(species) {
        this.species = species;
    }

    get pokemonSpecies() {
        return this.species;
    }

    set pokemonSpecies(species) {
        this.species = species;
    }
  
  	static fromPokemon(species) {
    	return new Pokemon(species);
    }
}

let goldeen = new Pokemon("Goldeen");
let staryu = Pokemon.fromPokemon("Staryu");

console.log(goldeen.pokemonSpecies);
console.log(staryu.pokemonSpecies);
////

C) Inheritance:
---------------
Just like java:
////
class Pokemon {
    constructor(species) {
        this.species = species;
    }

    get pokemonSpecies() {
        return this.species;
    }

    set pokemonSpecies(species) {
        this.species = species;
    }
  
  	static fromPokemon(species) {
    	return new Pokemon(species);
    }
}

class FirePokemon extends Pokemon {
    constructor(species, attack) {
      	super(species); //here we call the parent's constructor, it's mandatory if we want this object to behave roughly like the parent, we need to create an instance of the super for the child.
        this.species = species;
      	this.attack  = attack;
    }
    
    get fireAttack() {
        return this.attack;
    }
    
    set fireAttack(attack) {
        this.attack = attack;
    }
}

let charmander = new FirePokemon("Charmander", "Flamethrower");
let vulpix	   = new FirePokemon("Vulpix", "Fireball");
console.log(charmander.fireAttack);
console.log(vulpix.pokemonSpecies);
////

The keyword 'extends' means that the prototype of the derives class is NOT the default Object.prototype, it will be the inherited class/parent class/superclass.
The keyword 'super' is used to access and call methods of the superclass inside the subclass.

Inheritance, unlike encapsulation and polymorphism, is not always seen as good, some see it good and some see it bad,
this is because while polymorphism and encapsulation separate things and dependencies 
(which is one of the principles of good design), inheritance CREATES dependencies, so it's a tool that should be used wisely.

We have an operator like typeof, called instanceof, used to check if an object is derived from a specific class or not:
////
let newPokemon = new Pokemon("No species");
let magmar     = new FirePokemon("Magmar");

console.log(newPokemon instanceof Pokemon); //true
console.log(newPokemon instanceof FirePokemon); //false
console.log(magmar instanceof Pokemon); //true, since it's a subclass of Pokemon
console.log(magmar instanceof FirePokemon); //true
////

Doneeeeeeeeeeee
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------