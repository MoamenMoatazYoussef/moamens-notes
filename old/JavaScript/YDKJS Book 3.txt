YDKJS Book 3:
=============
Tips:
-----
- Anything between //// and //// is code in the language whose name is written beside the first ////.
- This does NOT cover appendix A, B, or C.
- To distinguish between using "this" to refer to the JS keyword and 'this' the actual word, 
    I'll write the keyword between DOUBLE QUOTES IN LOWERCASE like so: "this".
------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 1: "this" or that?
--------------------------
A) Why "this"?
--------------
//// JS
function identify() {
    return this.avenger;
}

var tony = {
    name: "Tony Stark",
    avenger: "Iron Man"
};

var bucky = {
    name: "Bucky Barnes",
    avenger: "Winter Soldier"
};

identify.call(tony);  //Output: Iron Man.
identify.call(bucky); //Output: Winter Soldier.
//// ENDCODE

Notice that we used the same function for two objects and it worked!
This is achieved using the "this" keyword, how?
First, we'll explore things that "this" doesn't do:
- "this" does NOT refer to the function itself. You can refer to the function you're in by using its name, but not "this".
- "this" does NOT refer to the function's LEXICAL SCOPE, you can't use "this" to mix two lexical scopes like this:
//// JS
function avengers() {
    a = "Assemble!";
    this.assemble();
}

function assemble() {
    console.log(this.a);
}

foo();  //The output is "Assemble!", but this is NOT because "this" made bar's lexical scope inside foo
        //it's because the reference bar normally references to the other function and the call site
        //is inside the lexical scope of foo, but not because of "this" at all! 
//// ENDCODE
Actually, making a bridge between two lexical scopes is impossible in JS.

So how does "this" work?
------------------------
- "this" doesn't care about where the function is declared, but it cares about HOW it's called.
- When a function is called, a record known as an "execution context" is created, which contains
    things like where the function is in the call stack, how it was invoked, parameters, etc.
- one of these records is what "this" refers to.
- "this" is a runtime binding, not compile-time binding, it happens at runtime.

"this" is so important that we have a whole chapter explaining how it works, next chapter ;)

------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 2: "this" all makes sense now!
--------------------------------------

Call-site:
----------
WHERE the function is CALLED.
"this" depends on the call site, so we need to get it.
To correctly get the call site, we need to get the call STACK.

Check this:
//// JS
function sayQuote() {
    console.log("Thanos: I am inevitable");
    snap(); //this is the call site for snap.
}

function snap() {
    console.log("*snap*");
    decimate(); //this is the call site for decimate.
}

function weLost() {
    console.log("We lost..");
}

sayQuote(); //call site for sayQuote.
//// ENDCODE

sayQuote is called, which calles snap, which calls decimate, which calls weLost.
NOTE: use the debugger in the browser to see the call stack, this is easier.
    To see the binding of "this", use the debugger to get the call stack, 
    then find the second one from the top, that'll help find the real call site.

Now, "this" works as follows:
-----------------------------
There are rules to determine what "this" refers to:
1- Default binding: this comes from standalone function call i.e. just calling a function in the global scope,
    like when we called sayQuote.
    //// JS
    function assemble() {
        console.log(`${this.a}...Assemble!`);
    }

    var a = "Avengers";
    assemble();
    //// ENDCODE

    Here, "this" refers to the GLOBAL OBJECT, which is why this code works.
    "this.a" is not a copy of "var a" declared, they are THE SAME, because
    "var a" declares a variable in the global scope of the global object.
    "this.a" refers to the global object so it searches for "a" in the global object's
    scope i.e .the global scope, so they are the same.

    Binding "this" to the global object, is called 'Default Binding'.

    How to know that default binding is the case:
    - Look at the call site: the function is called very normally in the global scope.
        So no other rules will apply.

>>>>> VERY IMPORTANT NOTE: default binding DOES NOT WORK if strict mode is applied on the CONTENTS!
    The global object won't be eligible for binding, so "this" will be 'undefined'!
    //// JS
    function assemble() {
        "use strict";
        console.log(`${this.a}...Assemble!`);
    }

    var a = "Avengers";
    assemble(); //Error: cannot resolve property 'a' of 'undefined'.
    //// ENDCODE

    But, strict mode on the CALL SITE is no problem:
    //// JS
    function assemble() {
        console.log(`${this.a}...Assemble!`);
    }

    var a = "Avengers";
    (function() {
        "use strict";
        assemble(); //Works!
    })();
    //// ENDCODE

>>>>> VERY IMPORTANT NOTE: DO NOT MIX strict and non-strict(sloppy) modes, the entire program should be either
    strict or sloppy.

2- Implicit Binding:
    Does the call site have an object owning or containing it?
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var a = {
        name: 'Avengers',
        assemble: assemble //reference to the function above
    };

    a.assemble(); //Output: Avengers...Assemble! i.e. it works.
    //// ENDCODE

    What happend here?
    ------------------
    as we said, HOW the function is called, determines what "this" refers to.
    Here, the function is added as a property TO the object 'a'.
    Then, it's called USING the object 'a' to reference the function.
    i.e. the object 'a' OWNS or Contains the function reference when it's called.

    So, here, there's another rule: Implicit Binding:
        "this" is bound to the owning object. (bound is the past simple of bind btw).
    
    This means, "this" now points to THE OBJECT 'a', which is why "this.name" referred to 
    the PROPERTY defined inside the object 'a'.

    So, the object calling the function that has the "this" is the OWNER and "this" points to it,
    so we need to trace where the call site of the function is and the object calling it, regardless
    of the call stack.
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var a = {
        name: 'Superheroes',
        avengers: 'b',
        justiceLeague: 'c'
    };

    var b = {
        name: 'Avengers',
        assemble: assemble //reference to the function above
    };

    var c = {
        name: 'Justice League'
    };

    a.avengers.assemble(); //Output: Avengers...Assemble! i.e. it works.
    //// ENDCODE
    Here, we called assemble using reference to avengers, and we got 'avengers' as 'b' using 'a'.
    So the owner of the function is 'b' and "this" refers to 'b', regardless of where we got 'b'.

    Implicitly Lost:
    ----------------
    Sometimes this happens:
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var a = {
        name: 'Avengers',
        assemble: assemble //reference to the function above
    };

    var b = a.assemble; //function reference

    var name = "NO THIS IS WRONG";

    b(); //Output: NO THIS IS WRONG...Assemble!
    //// ENDCODE
    What happened?
        The global variable 'name' was used instead of the property 'name'.
    Why?
        Because, when you do this 'a.assemble', you return a reference to the function 'assemble' through 'a',
        you do this without the object, we just used it to access and get the function.
        So, 'b' doesn't refer to 'a.assemble', it refers to 'assemble' only.
        So, 'b()' is equivalent to 'assemble()' which is called without an owner,
        so "this" follows the DEFAULT BINDING and is bound to global object,
        that's why it look at the global 'name'.

    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    function doThis(fn) {
        fn(); //this is the call site.
    }

    var a = {
        name: 'Avengers',
        assemble: assemble //reference to the function above
    };

    var name = "NO THIS IS WRONG";

    doThis(a.assemble); //Output: NO THIS IS WRONG...Assemble!
    //// ENDCODE
    What happened here?
        Same concept, when we pass a.assemble as an argument we do this:
            - get 'assemble' from 'a'.
            - fn = that reference we got.
            - pass 'fn' as argument.
        So, now 'fn' looks at the function regardless of the owner, and the same thing applies.
        (Remember how arguments are declared as local variables in the compilation of JS).
    
    This applies to built-in functions as well.

3- Explicit Binding:
    All functions are objects, and they all have properties, some of them are:
        .call() and .apply()
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var a = {
        name: 'Avengers',
    };

    var b = {
        name: 'Justice League'
    };

    assemble.call(a); //Output: Avengers...Assemble!
    assemble.call(b); //Output: Justice League...Assemble!
    /// ENDCODE

    So what happened?
        .call takes an object as an argument.
        This object is the object that will be referred to by "this", so:
        When we passed 'a', "this" is bound to 'a'.
        When we passed 'b', "this" is bound to 'b'.

        Simple as that ^_^.
        NOTE: If you pass a primitive value e.g. string or whatever, it will be wrapped in an object String().
        NOTE: .call and .apply behave differently INTERNALLY, but that doesn't matter at all for us here.

4- Hard Binding:
    Same concept as explicit, except it binds "this" to the object passed FOREVER, so:
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var b = function() {
        return assemble.call(a, arguments);
    }

    var a = {
        name: 'Avengers',
    };

    var c = b();
    b(); //Output: Avengers...Assemble!
    c(); //Output: Avengers...Assemble!
    //// ENDCODE

    This is binding it manually, there's a predefined function in the function object
    itself, Function.prototype.bind:
    //// JS
    function assemble() {
        console.log(`${this.name}...Assemble!`);
    }

    var b = assemble.bind(a);

    var a = {
        name: 'Avengers',
    };

    var c = b();
    b(); //Output: Avengers...Assemble!
    c(); //Output: Avengers...Assemble!
    //// ENDCODE

5- new Binding:
    This binding is different, it requires us to look at constructors in JS.

Constructors:
-------------
In any language, they're functions that sets defaults and stuff e.g.:
//// C++
class Avenger 
{
public:
    Avenger() 
    { //a constructor
        name       = "";
        powerLevel = 0;
    }

    Avenger(_name, _powerLevel) 
    { //another constructor
        name       = _name;
        powerLevel = _powerLevel;
    }

    bool attack(enemyPowerLevel) 
    {
        return (enemyPowerLevel < powerLevel);
    }

private:
    String name;
    int powerLevel;
}
//// ENDCODE

Then we make objects from that class like this:
//// C++
Avenger ironMan = new Avenger("Iron Man", 100);
//// ENDCODE

But in JS, this is not the case..
Constructors in JS are just very completely normal functions.
They're just called using the keyword 'new', and 'new' is very 
different from any 'new' keyword on other languages.

So there is no 'constructor function' in JS, there is just 'constructor call'
i.e. a function called using the 'new' keyword.

What does 'new' do in JS?
-------------------------
Every object in JS has a property 'prototype', this prototype points to an object.
and it inherits from that prototype. (Or rather, is LINKED to that prototype).
The object Object has a prototype Object.prototype, which is the top object in JS.

Now, every function is an object that has Function.prototype as its prototype as an object,
BUT, as a function, it has a ANOTHER property called 'prototype', which is just an object.

When we say "the function's prototype", we mean the second one, the object created WITH the function.
When we say "the prototype OF the function", we mean the FIRST one, Function.prototype, but we won't use it anyway.

When we call a function using 'new', this happenes:
1. Creates a brand 'new' object (pun intended, peace ya man peace xD).
2. This new object is linked to the function's prototype (i.e. the object created WITH the function),
    so now the new object "inherits from" (is linked to) the function's prototype.
3. Then, this new object is assigned to "this", now "this" points to the new object.
4. If the function doesn't return anything, it will return "this".

//// JS
function Avenger(name, powerLevel) {
    this.name       = name;
    this.powerLevel = powerLevel;
}

var ironMan = new Avenger("Iron Man", 100);
//A new object {} is created.
//this new object is linked to the function's prototype (The object created with the function)
//this object is bound to "this"
//the code is executed, so two new properties are added to "this", which is the new object.
//the properties are set to their respective values.
//finally, the function returns the object.

console.log(ironMan); //Output: {name: "Iron Man", powerLevel: 100}
//// ENDCODE

This is the last way to bind "this" to something.

NOTE: the second step may seem to not have effect, but it will when we discuss inheritance in JS.

Recap of "this" binding rules:
------------------------------
1. Default binding: "this" is bound to global object.
2. Implicit binding: "this" is bound to the called function's owner.
3. Explicit binding: "this" is bound to the object passed to call() or apply().
4. Hard binding: "this" is bound to the object passed to bind() forever (until we hard bind it again).
5. new binding: "this" is bound to the new object created when we call a function with 'new'.

If many rules apply on one "this", there's a precedence:
1) new binding: it overrides explicit because it makes a new object and returns it, chack the book because this is very detailed in it.
2) Explicit binding and Hard binding.
3) Implicit binding.
4) Default binding.

Exceptions to these rules (Not exceptions as in try catch):
-----------------------------------------------------------
- If you pass null to call() or apply() or bind(), explicit binding is ignored and default binding applies.
    Why would you do that?
    Well, you can pass parameters in all three like this:
    //// JS
    assemble.call(null, "a", "b");
    //// ENDCODE

    Any parameters passed after the first object are passed to the function assemble as its own parameters.
    So we can use this to pass parameters to set default values or whatever.

    But be careful if you call the function using an object, "this" will STILL refare to the global object!
    That would be a nightmare in debugging.

- Indirection: remember that when we do this:
    //// JS
    var a = b.assemble;
    //// ENDCODE
    where assemble is a function, a refers to THE FUNCTION ALONE without the object,
    Default binding applies.

Arrow functions:
----------------
Let's imagine a scenario together, a scene from a Shakespeare play:
*soft, chill, elevator music*
Imagine we're preparing for lunch and we have a table,
and each topic we've talked about so far is a tasty meal put on the table,
and the binding rules are the plates and utensiles carefully put on the table,

*metal music*
THEN ARROW FUNCTIONS BUSTED THROUGH THE DOOR, ENTERED, AND FLIPPED THE ENTIRE TABLE!!
\m/ O \m/

Arrow functions don't understand "this", they think of it as a variable.
So, it goes UP ONE LEVEL IN THE LEXICAL SCOPE and grabs the "this" binding there,

so, if we do this:
//// JS
function assemble() {
    return (a) => { console.log(this.a); };
}

var avengers = {
    a: "Assemble"
};

var justice = {
    a: "Gather"
};

var assemble2 = assemble.bind(avengers);
assemble2.call(justice); //Output: Assemble, not Gather!
//// ENDCODE

Why, because in the arrow function "this" is not known, so it jumps to previous lexical scope,
and the previous lexical scope is boind to avengers using bind, so it uses that as "this".

------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3: Objects:
-------------------
We know what objects are, there are two ways to make them:
//// JS
var a  = { //first way: object literal
    name: 'avengers
};

var b  = new Object(); //second way: constructed form
b.name = 'avengers'; 
//// ENDCODE

Both output in the same thing: an object (duh).
The constructed form is rarely used, so just use the literal form.

Types:
------
We know we have primitive types in JS:
string, number, boolean, object, null, undefined.

Note that these primitive types are NOT objects, they are their own thing.
NOTE: even if console.log(typeof null) prints object, null is referred to as an object TYPE,
    but it still isn't an object.

Misconception:
    "Everything in JS is an object"
This is not true.

Also, there are COMPLEX PRIMITIVE TYPES in JS, which are:
    'function'

'function' is a sub-type of object, i.e. a callable object.
Arrays are also objects with extra behavior in that its contents are more structured than a normal object.

BUT, there are some built-in OBJECTS in JS, they are NOT the same as primitive types:
String, Number, Boolean, Object, Function, Array, Date, RegExp, Error.

These are just built-in functions, each can be used as a constructor 
that return a new OBJECT of the sub-type used.
i.e.:
//// JS
var s1 = 'avengers';                //this is a string
console.log(typeof s1);             //Output: string
console.log(s1 instanceof String);  //Output: false

var s2 = new String('avengers');    //this is a string OBJECT, not a primitive string
console.log(typeof s2);             //Output: object
console.log(s2 instanceof String);  //Output: true
//// ENDCODE

s1: a string created normally. (literal form)
s2: an object created by the String constructor call. (constructed form)

s1 is a primitive immutable literal, i.e. it DOESN'T have properties of String like toString and etc.
, you need the object String to do this.

But JS automatically transforms s1 to a String Object whenever we need to (type coercion),
so we don't have to do this manually, we can just create things in literal form and JS will
deal with it.


- number and boolean also get converted to Number and Boolean objects.
- null & undefined have no object wrappers, only the primitives.
- Date doesn't have a primitive, it can be made only by constructed form.
- Object, Array, Function, RegExp, these are all objects, but also use the 
    literal form.
- Error objects also have no literal form, only constructed form.

NOTE: it's STRONGLY RECOMMENDED to always use the literal form, ESPECIALLY with primitives.
    i.e. never use these:
    //// JS
    var a = new String('avengers');
    var b = new Number(7);
    var c = new Boolean(false);
    
    var arr     = new Array(1, 2, 3);
    var regexp  = new RegExp(thor[i]);
    try {
        //something
        throw new Error('This is an object');
    } catch(error) {
        alert(error);
    }
    //// ENDCODE

    , always use these:
    //// JS
    var a = 'avengers';
    var b = 7;
    var c = false;

    var arr     = [1, 2, 3];
    var regexp  = /thor/i;

    var d = new Date(<write here arguments>); //because there's no literal form
    try {
        //something
        throw new Error('This is an object'); //because there's no literal form
    } catch(error) {
        alert(error);
    }
    //// ENDCODE

Contents of an object:
----------------------
Objects don't store actual content, they store references to specific locations,
and when we call them we retrieve whatever value in these locations.
these locations are what we call 'properties'.

Properties' names are always strings.
If you use numbers, they will be converted to strings.

Look at this:
//// JS
var a = {
    name: 'avengers'
};

console.log(a['name']); //we know this syntax
//// ENDCODE
The power of this syntax of accessing properties shines when we want to 
COMPUTE the name i.e. concatenate strings or whatever then get the property 
or set a new one.

//// JS
var a = {
    part1: 'Steve?',
    part2: 'On your left',
    part3: '*Portals open and everyone arrives*',
    part4: '*EPIC MUSIC and everyone ready for war*',
    part5: 'Cap: Avengers!',
    part6: '...',
    part7: 'Assemble!',
    part8: 'Fat Thor: AAAAAAAAAH',
    part9: '*everyone does a war cry*',
    part10: 'Scarlett Witch: you took everything from me',
    part11: `Thanos: I don't even know who the hell you are`,
    part12: `Scarlett Witch: you'll know soon enough`
};

var awesomeMoment = '';
for(let i = 1 ; i < 13 ; i++) {
    awesomeMoment += a[`part${i}`];
    awesomeMoment += '\n';
}
//// ENDCODE
NOTE: That was an awesome moment <3

Property VS Method:
-------------------
Functions declared inside objects are not 'methods', they are still locations
accessed by the object when we need, but we can return a reference and access them
from outside, they are normal functions.

Whenever we access something in an object, it's a Property call,
regardless of what we get(was it a function or a value).

Technically there are no distinct 'methods' in JS, they're all functions which we 
reference inside or outside objects however we want.

Arrays:
-------
Arrays are objects that assume numerical index, don't use them for name-value pairs.
Be careful when adding properties to the array Object, because if it was a number or 
a string having only numbers, it will end up being a real index.
i.e.
//// JS
var a = [1, 2, 3];
console.log(a.length); //3

a["4"] = 7;
console.log(a.length); //4 !
//// ENDCODE

Duplicating objects:
--------------------
Two types of copying objects:
- Shallow copy: copying the REFERENCES, so we have two objects looking at THE SAME properties.
- Deep copy: copying the references AND values, so we have two objects looking at DIFFERENT properties.
//// JS ES6
var b = Object.assign({}, a); //shallow copy
//// ENDCODE

Property Descriptors:
---------------------
Properties have some "settings", for example:
    - value: its value (duh).
    - writable: can you change its value? (boolean)
    - enumerable: will this property be accessed in certain enumerations e.g.
        for...in loop, (boolean, usually this is true).
    - configurable: can you change these settings? (boolean)
    And Others (whoever gets this reference is a legend <3 and let's watch vanossgaming together xD)

1) Describing:
    Ways to describe a property i.e. value, writable or not, enumerable or not, etc.
    //// JS ES5
    var a = {
        name: 'avengers'
    };

    Object.getOwnPropertyDescriptor(a, 'name');
    //this will display the settings of the property 'name' of object 'a'.
    //// ENDCODE

2) Defining new properties/modifying existing properties:
    If we want to set these settings ourselves:
    //// JS ES5
    var a = {};
    Object.defineProperty(a, 'name', {
        value: 'avengers',
        writable: true,
        configurable: true,
        enumerable: true
    });
    //// ENDCODE

    NOTE: this way of adding properties is not preferred unless you REALLY want
        to configure them while adding them.

    If configurable: false, defineProperty would throw a TypeError.

3) Deleting:
    //// JS
    var a = {
        name: 'avengers',
        villain: 'thanos'
    };

    delete a.villain;

    console.log(a); //Output: { name: 'avengers' }
    //// ENDCODE

    'delete' will delete (duh, again) a property from an object.
    If the property 'villain' is not configurable (configurable: false),
    'delete a.villain' would fail, but won't throw an error, we will 
    still find the property.

    NOTE: 'delete' in JS is not freeing memory like 'delete' in C or C++.

Immutability:
-------------
- Shallow immutability: we can make an object immutable, but if it has references to
    other stuff, that stuff is NOT made immutable.
- ...no there's no deep immutability, only shallow, but there are ways you can do it.


1) writable: false, configurable: false.
    This basically makes the property constant.

2) PreventExtensions: to prevent adding new properties to an object.
    //// JS
    var a = {
        name: 'avengers'
    };

    Object.preventExtensions(a);
    a.number = 7;
    console.log(a.number); //Output: undefined
    //// ENDCODE

3) Seal: preventExtensions + make all properties's configurable: false
    i.e you can't add properties, and you can't configure or delete existing ones.
        You can still modify their values, though.
    //// JS
    var a = {
        name: 'avengers'
    };

    Object.seal(a);
    //// ENDCODE

4) Freeze:
    All three above i.e. configurable: false, writable: false, preventExtensions.
    Now your object is completely constant.
    //// JS
    var a = {
        name: 'avengers'
    };

    Object.freeze(a);
    //// ENDCODE

    Deep freeze:
        calling freeze on an object, then calling freeze recursively over all objects 
        it references. (This is deep immutability!).

        This is dangerous to use and could affect shared objects!

NOTE: if you find yourself needing to make an object completely immutable,
    this probably indicates a problem with your design, so think about your design
    and how you can account for potential changes in values.

[[Get]]:
--------
When you do this:
//// JS
var a = {
    name: 'avengers'
};

var theName = a.name;
//// ENDCODE

the code performs a [[Get]] operations on 'a',
it's a built-in operation that:
    1- inspects if the object has the requested name of the property (which is why it should be a string).
    2- if it finds it, it returns the value.
        else, it returns undefined.

This is different from when you reference a variable by the identifier's name,
if the variable is not find within the lexical scope, the result is not undefined,
it's a ReferenceError.

[[Put]]:
--------
The same for assigning a property or changing one:
    1- inspects of the object has the requested name,
    2- if it finds it, it calls the setter if there is.
            if writable: false:
                if strict mode, raise TypeError.
                else, nothing.
            else,
                set the value normally.
        else, 
            <Something complex we'll see at Chapter 5>

Getters & Setters:
------------------
If a property has a getter or setter, its definition becomes an accessor descriptor.
for accessor descriptors the writable and value settings are ignored.
//// JS 
var a = {
    name: 'avengers',
    get num1() {
        return this._num1_;
    },
    set num1(n) {
        this._num1_ = n;
    }
}; //now a.num1 returns undefined since it's not set yet

Object.defineProperty(a, 'num2', {
    get: function() {
        return this._num2_ * 2; //we can modify the getter behavior as shown
    },
    set: function(n) {
        this._num2_ = n;
    },
    enumerable: true
}); //now a.num2 returns NaN since it's not set yet, why NaN and not undefined?

console.log(a.num1); //2
console.log(a.num2); //4 :o
//// ENDCODE

Does this property exist?
-------------------------
To check for a property, we can do two things:
//// JS
var a = {
    name: 'avengers'
}
console.log('num1' in a);               //false
console.log(a.hasOwnProperty('num1'));  //false
//// ENDCODE

in: checks this object AND its prototypes.
hasOwnProperty: checks this object only.

NOTE: remember that both operations check for existance of PROPERTIES, not values.

Enumeration:
------------
Enumerable: will be included into properties and iterated through.

Object.keys(a): returns all enumerable property names.
Object.getOwnpropertyNames(a): returns ALL property names.

Iteration:
----------
To iterate over properties: the for...in loop.
To iterate over values:
    Arrays: normal loops, forEach(...), every(...), some(...), 
        for...of loop (directly over values)
        //// JS ES6
        var shield = [
            'Iron Man',
            'Thor',
            'Captain America',
            'The Hulk',
            'Spider-Man',
            'Black Widow',
            'Hawkeye',
            'Black Panther',
            'Captain Marvel',
            'Nick Fury',
            'Doctor Strange',
            'Guardians Of The Galaxy'
        ];
        for(var v of shield) {
            console.log(v);
        }
        //// ENDCODE
    Objects:
        - for...in loops, but access the properties to get the values (indirectly).
        
How does the for...of loop work:
--------------------------------
Arrays have a built-in iterator object @@iterator which has a property, the function .next(),
, for...of gets that iterator object and then calls .next() to go to the next element.

//// JS
var shield = [
            'Iron Man',
            'Thor',
            'Captain America',
            'The Hulk',
            'Spider-Man',
            'Black Widow',
            'Hawkeye',
            'Black Panther',
            'Captain Marvel',
            'Nick Fury',
            'Doctor Strange',
            'Guardians Of The Galaxy'
        ];

var it = shield[Symbol.iterator]();
console.log(it.next()); //Output: {value: 'Iron Man', done: false}
//// ENDCODE

Objects don't have an iterator but we can define it:
----------------------------------------------------
//// JS
var a = {
    name: 'avengers',
    boss: 'Nick Fury'
};

Object.defineProperty( a, Symbol.iterator, {
	enumerable: false, //because it's a property we normally don't want to go through
	writable: false,
	configurable: true,
	value: function() {
		var o = this; //this will be used to get the keys
		var idx = 0;  //this will be used to Index the keys
		var ks = Object.keys( o ); //this is the array of keys
		return {
			next: function() {
				return { //we return an object that contains value, which is current item, and 'done' which specifies that we've finished iterating
					value: o[ks[idx++]],    //get index++, then get key, then get next item
					done: (idx > ks.length) //this becomes true when we finish iteration
				};
			}
		};
	}
} );

var it = a[Symbol.iterator]();
it.next();
it.next();
//// ENDCODE
Now we made our own iterator.
Or we can just use for...of loop xD

But we can modify and make our iterators complex if we want, we can customize it.

------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 4: Classes:
-------------------
Here we'll talk about OOP, classes, and how both are implemented (by you) in JS.
NOTE: The first part of this chapter discusses the core theory of OOP and classes,
    so I won't summarize the first part, but I'll write any notes I find "new" to these topics.

JS classes:
-----------
Some languages like Java force you to put everything in classes.
Some languages like C/C++, PHP gives you the freedom of choosing procedural or OO programming.

JS is different, it has "some" class-like functionalities.
These functionalities are used to 'emulate' classes in JS.

But, as a main functionality:
    <<<< JS DOES NOT HAVE CLASSES >>>>

Classes are a design pattern, and are the foundation of other design patterns.
So, you can implement approximates for much of the class's functionality.

NOTE (educational): this has been done in C too, you can implement all OOP principles,
    One-level inheritance can be implemented as redefinition of functions, 
    encapsulation and abstraction are done using structs and header files,
    and polymorphism is just pointers to functions.
    For more info read the book 'Clean Architecture' by Robert C. Marion a.k.a. Uncle Bob,
    Actually read his entire 'clean' series, he is a superior software engineer <3

How to emulate classes:
=======================
To do that, we need to implement the following functionalities:
A) construction:
----------------
We need to make a 'blueprint' and construct 'instances' from it.
'instances' are objects that COPY the features described in the 'blueprint'.

Obviously, from now on we'll call:
blueprints: classes
instances: instaces or objects whatever xD

B) inheritance:
--------------
We need to be able to define other classes that inherit from a parent class.

C) polymorphism:
----------------
NOTE: while a lot of people think that inheritance is the defining pillar of OOP, 
    in my opinion POLYMORPHISM is the defining pillar of OOP.

We need to be able to override parent class functions with child class functions,
    and be able to call these functions using a parent object (to really exploit the power of polymorphism).
We also need to be able to references the overridden function from the child class.

D) multiple inheritance:
------------------------
This may have some problems:
1) if a class inherits from two parents, both have the function 'doThis()',
    when the child class instance calls it, which one is called?
2) Diamond problem, the same problem as above but the two parents inherit from
    a grandparent class having the same method .doThis(), they override it.

+--------------------------------------------------+
|                                                  |
|        Now, what does JS provide for OOP?        |
|                                                  |
+--------------------------------------------------+

Mixins:
-------
When you 'inherit' in JS, it's not like normal inheritance,
i.e. you don't 'copy' features from parents to children, you LINK them.

You don't have classes in JS, you only have Objects,
so when you make an object 'inherit' from another object,
you 'link them' together, we'll explore that in the next Chapter.

To 'emulate' the copy behavior of inheritance, JS developers use 'Mixins'
>> Mixin: a class/object/function that has methods used by other classes, but it's not strictly their parent class.

Explicit mixins:
----------------
We implement a function to COPY things from a source object to a target object:
//// JS
function mixin(source, target) {
    for(var key in source) {
        if(!(key in target)) {
            target[key] = source[key];
        }
    }
    return target;
}

var IronMan = {
    powerLevel: 100,
    attackUsingRepulsors: function() {
        console.log('repulsor attack from Iron Man');
    },
    attackUsingChestpiece: function() {
        console.log('chestpiece attack from Iron Man');
    }
}

var targetObject = {
    additionalWeapons: 'minigun',
    attackUsingRepulsors: function() {
        console.log('repulsor attack from War Machine');
    }
};

var WarMachine = mixin(IronMan, targetObject);
//// ENDCODE

If we run this, we'll find that WarMachine has taken copies of the features in IronMan,
    but we don't overwrite the already present functionality i.e. overriding methods stay.
    i.e. attackUsingRepulsors in WarMachine is not replaced by the one in IronMan.

NOTE: we're dealing with OBJECTS, not classes, as you can see.

Polymorphism in JS:
-------------------
Notice the change in targetObject:
//// JS
function mixin(source, target) {
    for(var key in source) {
        if(!(key in target)) {
            target[key] = source[key];
        }
    }
    return target;
}

var IronMan = {
    powerLevel: 100,
    attackUsingRepulsors: function() {
        console.log('repulsor attack from Iron Man');
    },
    attackUsingChestpiece: function() {
        console.log('chestpiece attack from Iron Man');
    }
}

var targetObject = {
    additionalWeapons: 'minigun',
    attackUsingRepulsors: function() {
        IronMan.attackUsingRepulsors.call(this);
        console.log('repulsor attack from War Machine');
    }
};

var WarMachine = mixin(IronMan, targetObject);

WarMachine.attackUsingRepulsors();
// Output: repulsor attack from Iron Man
//         repulsor attack from War Machine
//// ENDCODE
We can call the parent object's method from the child object

In fact, there is NO relative polymorphism functiionality in JS,
to call a parent's function, we need to make an Absolute reference to it.
i.e. we had to explicitly specify IronMan object, THEN call the method.

And, we used the .call(this) to ensure that the function executes in the 
context of WarMachine, not in the context of IronMan.

This approach to emulate polymorphism may be more costly than beneficial,
i.e. this will lead us to make manual/explicit linkage in EVERYT FUNCTION.
This also increases complexity of the code.

So, this approach to polymorphism is avoided.

In the mixin:
Functions are not copied, only their references.
So, even manual copying of functions doesn't really 'emulate' the real
duplication from class to instance that occurs in OOP languages.

In short, JS function can't be duplicated (in a standard reliable way).
So, if you modify a shared function e.g. attackUsingChestpiece,
the function in BOTH classes WarMachine and IronMan will be modified!

You can emulate multiple inheritance by using a mixing on many sources and one target,
but that doesn't handle collisions at all, so that may cause a lot of problems.

Explicit mixins are STILL useful, they're just not as powerful as they seem.

If mixins are making your code more readable, then use them, otherwise don't.
If after using mixins the code gets harder, don't use them.

Parasitic Inheritance (ew):
---------------------------
This is a variation of mixins:
//// JS
//this is a JS class, we'll see how to do this later.
function IronMan() {
    this.powerLevel = 100;
}

IronMan.prototype.attackUsingRepulsors  = function() {
    console.log('repulsor attack from Iron Man');
};

IronMan.prototype.attackUsingChestpiece = function() {
    console.log('chestpiece attack from Iron Man');
};

function WarMachine() {
    //make an instance of IronMan
    var warMachine = new IronMan();

    //specify its data
    warMachine.powerLevel = 50;

    //save a reference to IronMan::attackUsingRepulsors()
    var repulsorAttack = warMachine.attackUsingRepulsors;
    
    //override IronMan::attackUsingRepulsors()
    warMachine.attackUsingRepulsors = function() {
        repulsorAttack.call(this);
        console.log('repulsor attack from War Machine');
    };

    return warMachine;
}

var warMachine = new WarMachine();

WarMachine.attackUsingRepulsors();
//// ENDCODE
This copies the definitions from the parent object, then merges them with child object, and returns child object.

Implicit Mixins:
----------------
These are similar to explicit mixins, with the same pros and cons:
//// JS
var IronMan = {
    attackUsingRepulsors: function() {
        console.log('repulsor attack from Iron Man');
    }
};

var WarMaching = {
    attackUsingRepulsors: function() {
        IronMan.attackUsingRepulsors.call(this); //as you can see, we still have the cons of explicit mixin.
        console.log('repulsor attack from War Machine');
    }
};

WarMachine.attackUsingRepulsors();
//// ENDCODE

As you can see, we still needed to do this:
//// JS
IronMan.attackUsingRepulsors.call(this);
//// ENDCODE
So we still have the same disadvantage with Explicit mixin:
- we have to do this for EVERY FUNCTION to achieve polymorphism.
- i.e. we have code that is harder to maintain :(

------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 5: Prototypes:
----------------------