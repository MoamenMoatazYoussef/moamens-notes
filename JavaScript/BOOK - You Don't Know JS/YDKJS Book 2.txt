YDKJS Book 2:
============
Tips:
-----
- Anything between //// and //// is code.
- I skipped YDKJS Book 1 for now, I don't think it has major details that are new to us, but I may get back to it later.
- This does NOT cover appendix A, B, or C.
------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 1: What is scope?
------------------------------------
Compiler of JS:
--------------------
- JS is a COMPILED language, not interpreted, compiler is a Just-In-Time compiler or JIT, it performs many of compiler's steps.

Normal compiler:
1. Tokenizing.
2. Parsing the tokens to make a syntax tree.
3. Using the tree to generate executable code.
Note: there are some sub-steps in the middle that perform compiler optimizations, intermediate language generation, etc.

JS engine uses tricks to make compiling as fast as possible, like using JITs, hot re-compiles, etc.

Scopes:
-----------
We have many things "talking" to each other in JS:
1. Engine: responsible for compilation and execution.
2. Compiler: does the compiling steps in the engine.
3. Scope: collects and maintains a lookup list of all declared variables, and enforces rules about how to access them.

Remember that any code is translated to simple assembly instructions, so:
////
var a = 2;
////
May actually be like this:
//// Assembly
LOAD A, 0x1000
MOVE A, 2
STR A, 0x1000
////

We're not diving into assembly of course, but the concept is similar, when we run JS code:
- Compiler sees var a.
- Compiler asks scope: "Is variable a already declared in this scope?"
    - If yes, compiler ignores declaration.
    - If no, compiler asks Scope to declare a new variable a.
- Compiler generates code for Engine to execute the 'a = 2' part, then when the code is run, engine asks scope if there's a variable 'a' in this scope, if not it looks elsewhere.

The compiler performs what's know as LHS lookup: it looks for the variable container, THEN assigns the value to it, as we've seen.

Sometimes, it's RHS lookup, which means the compiler gets the value of the variable then uses it or manipulates it e.g. console.log(x);

LHS: who's the target of assignment?
RHS: who's the source of assignment?

////
function whatever(a) {
    console.log(a);
}

whatever(2); //here, an RHS call to the function, and an LHS assignment a = 2;
////

As we can see, the compiler does the assignments of functions and whatever so that Engine can only look them up and use them.

Scopes are nested, Engine looks at the immediate scope, if not found then looks at next outer scope, and so on until global scope.

Errors with scopes:
--------------------------
- in an RHS assignment or statement, if engine doesn't find the variable, this will be an "ReferenceError: Undeclared identifier", a ReferenceError is thrown by the engine.

- in an LHS lookup, if you're not using strict mode, the engine WILL DECLARE a global variable and assign it.

- but if in strict mode, it will throw a ReferenceError.
- if the variable WAS FOUND, but you're trying to use it as function, or reference a property that is null or undefined, it will throw a TypeError.

--------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 2: Lexical Scope:
-----------------------------------
There are two models of how scope works, one of them is the Lexical scope, the other is the Dynamic scope, used in Bash, perl, etc., JS uses the Lexical scope.

Lexical scope is the scope that is defined when the compiler is tokenizing, or lexing, the string of code, it's based on the blocks of scope WE wrote.
////
//outer scope
function foo(a) {
  //inner scope 1
  var b = a * 2;
  function bar(c) {
    //inner scope 2
    console.log(a, b, c);
  }
  bar(b * 3);
}
foo(2);
////
The function bar is contained inside foo just because we defined it there.

The Engine now understands all the places it needs to look for any identifier.
- First, it looks for a, doesn't find it in bar, so looks in inner scope 1, where it finds a.
- Then, looks for b, where it finds it also in the inner scope 1.
- Finally, c is found in the inner scope 2.

If there was a var c inside inner scope 1, remember that engine looks in the LOCAL scope first, which is inner scope 2, so it will STILL use the c inside inner scope 2 ignoring the one outside.

i.e. Scope looks for first-match.

Note: any global variable is a property of the global object e.g. window, so you can use 'window.a' to reference it from an inner scope if you have another variable named a inside.

Note: the scope of a function is defined where it's declared, without dependency on how it's called or when it's called.

Note: lexical scope lookup (i.e. this process), only looks for first-class identifiers, so it can't lookup object1.attribute1 even if object1 is defined in the scope.

How to make JS developers pissed off by cheating the lexical scope:
--------------------------------------------------------------------
You can cheat on the lexical scope using two statements in JS: eval, and with.
- Both are frowned upon.
- This process slows down performance.

eval():
-------
It takes a string argument, and treats it as a JS code, and runs it.

Lines after eval() will suffer because the Engine doesn't know if the code inputted to eval() has changed the lexical scope or not!

In strict mode, eval has its own lexical scope, so it doesn't modify the enclosing scope. But still don't use it.

Same goes for setTimeout, setInterval, etc.
Also don't use 'new Function(...)', even if it's safer than eval, don't use it.

Dynamically generating code use cases are very rare anyways.

with:
-----
- This is deprecated anyway.
- with is used to make many references to properties of an object without writing the object many times:
////
var o = {
	a: 1,
	b: 2,
	c = 3
}

//normal way
o.a = 5;
o.b = 6;
o.c = 7;

//with way
with(o) {
	a = 5;
	b = 6;
	c = 7;
}
////
It takes an object and treats it as if it's a wholly separated lexical scope, so things inside that scope may vanish with it!

In trict mode, you CANNOT use with.

Both eval and with affect performance:
- Engine optimizes by being able to analyze the code as it lexes,
- but if it finds eval or with, it assumes that all previous analysis is invalid, so it has to wait.

In short: DO NOT USE with OR eval().
--------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3: Function VS Block Scope:
--------------------------------------------------
Function scopes:
----------------
Each function creates a scope for itself.
////
function foo(a) {
    var b = 2;
    function bar() {
        //code
    }
    var c = 3;
}
////
From the global scope, we can't call bar or access b or c, because they're local to foo.

We can extract a section of our code and hide it in a function, this adds a local scope to them.
This helps first of all in good design, as you only expose what is needed to expose, and hide the implementation details.
When doing this, it's better to keep any declarations of variable inside the function also inside it i.e. make variables local to the function.
Why? in order to avoid these variables changing outside the function, thereby introducing different values than expected.

Variable collision:
-------------------
Look at this:
////
function foo() {
    function bar(a) {
        i = 3;
        console.log(a + i);
    }

    for(var i = 0 ; i < 10 ; i++) {
        bar(i * 2); //when this is called, the local 'i' overwrites the loop's counter, so everytime the loop's 'i' updates, it's reassigned as 3, resulting in an infinite loop
    }
}
foo();
////
This is called collision.
How to avoid collision?
1) Declare i as local variable for bar.
////
var i = 3;
////
2) Just use another name for this variable, similar names are bad coding anyways.

Global namespaces:
------------------
Collision is likely to occur if you're using many libraries, they can collide with ech other if they don't properly hide their private variables and functions.
Usually to solve this they make an object which we declare it, and any functions/vars/etc. are accessed as properties of that object.
As if it's a namespace for that library.
So we can get something like this:
////
Object1.printMessage(Object1.a);
Object2.printMessage(Object2.a);
////
Even if the variables and functions have similar names, we access them using their objects so they are easily differentiated.

Functions as scopes:
--------------------
We can do the function trick as we said above i.e. wrapping blocks of code into functions.
BUT, now we have ANOTHER name in the global scope.
What if we remove that name or at least not let it be in the global scope?
////
var a = 2;
(function foo() {
    var a = 3;
    console.log(a);
})();
console.log(a);
////
When we wrote '(function)()' this means we treat this like a function EXPRESSION, not as a standard declaration.
This makes the function hide its name to its scope, it's hidden to itself!
This achieves our goal it doesn't pollute the global scope, but how do we use such a thing?
//The last two empty brackers makes this a function call and executes the function immediately.
(therefore they don't have to be empty and can have arguments if we want) 
(but only if the function itself has arguments which may not make sense since we just defined it)
That also means, we may not be able to call this function anywhere else in the code!

These are called Immediately Invoked Function Expressions, or IIFE.

We can use IIFEs in:
- Anonymous functions: unnamed functions defined only to be passed as parameters or to return some values.
- Inline functions: anonymous functions but are given a name, which helps in readibility and if we want recursion or something.

Check out IIFE's styles because it has a lot of styles of writing in the code ('styles' here don't refer to html or whatever).

Blocks as scopes:
-----------------
What the block is a block scope?
It's declaring variables as local as possible, like in a for loop:
////
for(var i = 0 ; i < 10 ; i++) {
    //bla bla
}
//// We declare the i INSIDE the loop's head, it's ONLY local to the loop.

When we declare variables using 'var', they are local to the enclosing scope.
But you know what the problem is?
JS does NOT have facility for block scopes :'(

But, not all hope is lost, we have:
1) with: well, all hope is lost for that keyword, just avoid it.
2) try/catch:
    Although uncanny, any variables declared in the catch block are local to the catch block.

3) let:
    this keyword attaches the variable declaration to the enclosing block.
    ////
    var foo = true;
    if(foo) {
        var bar1 = 7;
        let bar2 = 8; 
    }
    console.log(bar1); //output: 7
    console.log(bar2); //ReferenceError, that's what we need, now block scope is achieved ^_^
    ////
    because it does this implicitly, some people may not notice it when working on the project,
    so it's better to do this:
    ////
    var foo = true;
    if(foo) {
        {
            var bar1 = 7;
            let bar2 = 8; 
        } //Just adding these two braces is valid syntax, AND makes it more readable
    }
    console.log(bar1); //output: 7
    console.log(bar2);
    //// 

Garbage collection:
-------------------
////
function foo(data) {
    //do stuff
}

var aLotOfData = {...}; //a big data structure or whatever
foo(aLotOfData);
//Then, we don't need the aLotOfData variable anymore, but it still is kept in JS Engine!
////
To solve this, we can use let:
////
function foo(data) {
    //do stuff
}
{
    let aLotOfData = {...};
    foo(aLotOfData);
}
////
That will erase aLotOfData after the block finishes ^_^

let in loops:
-------------
////
for (let i=0; i<10; i++) {
	console.log( i );
} //Not only is it local to the loop, it's actually ERASED and created every iteration with the new value, this will be important later but it's a good thing.
////

Note: Refactoring your code using let instead of var would expose any unwanted dependencies and pollutions between scopes.

const:
------
Same as let, but fixed value.

--------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 4: Hoisting:
--------------------
First of all, what is the output of this:
////
a = 2;
var a;
console.log( a );
////
- Undefined because we declared a variable then printed it? No.
- Syntax error because a is not defined? No.
- The answer is 2? YES!

What is the output of this:
////
console.log(a);
var a = 2;
////
- ReferenceError, because we're printing an undeclared variable? No.
- The answer is 2? No.
- Undefined? yes!

Wait what? what's going on here?
--------------------------------
Well... 
When JS compiles, there are TWO rounds:
Round 1: all declarations (Variables and functions, except function expressions and some other things) are processed first.
Round 2: all assignments and operations are executed (any declarations of function expressions and other stuff are declared now).

Btw this means that JS is a COMPILED language, not interpreted like python :o
So, this statement:
///
var a = 2;
///
Is processed twice:
    - var a is declared in round 1.
    - a is assigned 2 in round 2.
So, back to the first example:
////
a = 2;
var a;
console.log(a);
////
Round 1: 
    - 1st statement: NOT declaration, skipped.   
    - 2nd statement: declaration, a is defined in current scope (which is the global scope).
    - 3rd statement: NOT declaration, skipped.
//So now we have a defined :o

Round 2:
    - 1st statement: assignment:
        - Engine looks for 'a'.
        - Engine asks current scope if it knows 'a'.
        - Scope replies YES, it was defined here at round 1.
        - Engine executes assignment, now a = 2;
    - 2nd statement: already processed.
    - 3rd statement: log:
        - Engine looks for a.
        - Engine asks current scope if it knows 'a'.
        - Scope replies YES, it was defined here at round 1.
        - Engine retrieves value of a.
        - Engine executes printing statement.
//now the value of a, which is 2, is printed.

Second example:
////
console.log(a);
var a = 2;
////
Round 1:
    - 1st statement: NOT declaration, skipped.
    - 2nd statement: declaration AND assignment:
        - Execute declaration.
        - Skip assignment.
//Now a is defined in global scope, but has value 'undefined'.
Round 2:
    - 1st statement: logging:
        - Engine looks for a.
        - Engine asks current scope if it knows 'a'.
        - Scope replies YES, it was defined here at round 1.
        - Engine retrieves value of a, which is now 'undefined'.
        - Engine executes printing statement.
    //That's why undefined is printed.
    - 2nd statement: declaration AND assignment:
        - Declaration already executed.
        - Assignment:
            - Engine looks for 'a'.
            - Engine asks current scope if it knows 'a'.
            - Scope replies YES, it was defined here at round 1.
            - Engine executes assignment, now a = 2;
// So, at the end of the program a has a value of 2
// BUT, undefined is printed.

Because declarations (variables and functions) are processed first, it's as if they are all declared at the top of the program BEFORE any execution statements.
i.e. THIS
////
console.log(a);
var a = 2;

b = 3;
var b;
console.log(b);

c = addSeventeen(a);
console.log(c);
var c;


function addSeventen(num) {
    return num + 17;
}
////

is equivalent to this:
////
var a;
var b;
var c;
function addSeventen(num) {
    return num + 17;
}

console.log(a);
a = 2; //note that ONLY the declaration is executed first i.e. moved up.
b = 3;
console.log(b);
c = addSeventeen(a);
console.log(c);
////
That, is called 'Hoisting'.

Hoisting is like lifting all declarations up to the top of the current scope.
Hoisting is per-scope, so variables defined in a function will be hoisted to the top of THAT function.
////
c = addSeventeen(3);
var c;
console.log(c);

function addSeventeen(num) {
    seventeen = 17;
    num += seventeen;
    
    var seventeen;
    return num;
}
////
Is equivalent to this:
////
var c;
function addSeventeen(num) {
    var seventeen; //note that this is hoisted to the top of THIS SCOPE only.
    seventeen = 17;

    num += seventeen;
    return num;
}

c = addSeventeen(3);
console.log(c);
////

THIS DOES NOT APPLY ON FUNCTION EXPRESSIONS!
////
var c = 2;
addSeventeen(c); //executes normally;
add18(c); //TypeError.
addEighteen(c); //ReferenceError.

function addSeventeen(num) {
    return num + 17;
}

var add18 = function addEighteen(num) {
    return num + 18;
}
////

Why?
----
Round 1:
    1st statement: declaration and assignment, skip assignment, declare c in global scope.
    2nd->5th statements: not declarations, skipped.
    6th statement: function declaration, declared in global scope.
    7th statement (after the first function): declaration of add18 as VARIABLE, declared in global scope.
//now we have add18 declared as a VARIABLE, that has not been assigned to the function expression yet.
//and addEighteen is NOT declared because it's in the assignment.
Round 2: 
    1st statement: executes normally.
    2nd statement:
        - Engine asks scope for add18 and scope says YES.
        - Engine finds '()' after add18, which means it's executable.
        - Engine asks scope to retrieve the executable code of add18.
        - But scope says no, add18 is a VARIABLE, not an executable, a different TYPE.
        - Engine produces a TypeError.
    3rd statement(won't be executed because of TypeError of previous statement, but if it was):
        - Engine asks scope about addEighteen.
        - Scope says NO.
        - Engine produces ReferenceError.

So, functions are hoisted.
function EXPRESSIONS are NOT hoisted.

Functions are hoisted first:
----------------------------
If something is declared more than once, the duplicate declarations are ignored.
A very small detail in hoisting, functions are hoisted FIRST.
////
foo();

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
}
////
Round 1:
    - 1st statement: skipped.
    - 2nd statement: skipped for now :o.
    - 3rd statement: foo as a function is declared in global scope.
    - 4th statement: foo(the variable) is declared and NOT ASSIGNED yet.
    - Back to 2nd statement: declaration of variable, but foo has been declared, so this is ignored.
Round 2:
    - 1st statement: executable:
        foo(the executable function), is defined, so it executes:
        '1' is printed.
    - 2nd and 3rd statements: already processed.
    - 4th statement: foo is assigned to the new function.

If something is declared more than once, the duplicate declarations are ignored.
So, the declaration of foo as a FUNCTION is hoisted first, the the var foo is ignored, even if it came before the function declaration.
Duplicate declarations are a bad coding style anyway so don't do it.

Also, function declarations that appear inside of normal blocks e.g. if conditions, are hoisted to the enclosing scope.
////
if(some_condition) {
    function foo() {    //this function is hoisted to the global scope, not the block scope.
        //bla bla bla
    }
}
////
BUT, JS's behavior is likely to be changed in the future, so avoid declaring functions inside blocks,
the use cases of that are few anyway (I think), so don't do that.
--------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 5: Scope Closure:
-------------------------
Closure is: when a function can access its lexical scope(bucket) even if it's executing outside that bucket.
What?
////
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
////
foo() can access a.
bar() can also access a by looking in the scope one level up, which is the scope of foo().
The ability of bar() to access a, is part of what closure is.
Consider this example:
////
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar;
}

var c = foo();

c();
////
foo() can see a.
bar() can see a;
but foo can return the REFERENCE to bar().
so when we execute the statement:
////
var c = foo();
////
foo() is executed and returns bar, a reference to bar().
now c holds that value.

Now we execute c(), which executes bar() directly without executing foo!
We would expect that after foo() finishes, its scope dies, and that's because we know JS Engine has a garbage collector that collects unused stuff.
But, the scope is NOT unused!

The function bar() uses it :o
bar STILL can reference this scope, and that is called Closure ;)

So, when we execute c(), this internally executes bar(), which has access to foo()'s scope i.e. author-time scope.

////
var fn;
function foo() {
    var a = 2;
    function sup() {
        console.log(a);
    }
    fn = sup;
}

function bar() {
    fn();
}

foo();
bar();
////

Round 1:
    foo, sup, bar, fn, a are declared.

Round 2:
    foo is executed:
        a assigned 2,
        fn assigned reference to sup,
        foo returns and ends.
    bar is executed:
        it executes fn() which executes sup():
            accesses 'a' even if foo() is finished //closure
            logs a correctly '2'.

O.M.G. dude!

Now let's play around:
----------------------
////
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "slim shady" );
////
Round 1:
    wait is declared.
    message, the local variable made as argument, is also declared.
Round 2:
    wait is executed with argument "slim shady":
        setTimeout is executed:
            function timer() is declared.
            time set to 1000.
            function wait ENDS.
    //after 1000 ms.
        timer() is executed:
            accesses message EVEN IF WAIT has ended!
            logs "slim shady".

A jQuery example:
//// html
<html>

<head>
</head>

<body>
    <button id="button1">Slim Shady</button>
    <button id="button2">Marshall Mathers</button>
    <button id="button3">Eminem</button>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.0/jquery.min.js"></script>
    <script src="./main.js"></script> //your JS/jQuery file
</body>

</html>
////
//// jQuery
$(document).ready(function() {
    function setupButton(name, selector) {
        $(selector).click(function() {
            alert(`Hi, my name is-What?, my name is-Who?, my name is-*chika chika* ${name}`);
        })
    }

    setupButton("Slim Shady", '#button1');
    setupButton("Marshall Mathers", '#button2');
    setupButton("Eminem", '#button3');
});
////
Round 1:
    setupButton is declared.
    name, selector, the local variables made as arguments, are also declared.

Round 2:
    setupButton executes with "Slim Shady and "#button1" as args:
        $ selector function is executed and gets "#button1",
        then sets event click and a handler (the anonymous function) is passed:
        setupBot ends.
    setupButton executes again with button2 and 3,
    same thing happens.
    setupButton ends.

//So now, the function is OVER, and we have three buttons, if we click on one of them,
//it will display an alert with the CORRECT NAME as it was passed in the argument :o
//that is because of closure ;) 

Generally, when you deal with:
    timers
    event handlers
    AJAX requests
    cross-window messages
    web workers
    asynchronous and synchronous tasks
    callback functions
    IIFE,
then closure is involved.
So yeah, it's really important to understand it xD

Loops + Closure:
----------------
////
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
////
We would expect that we get 1, 2, 3, 4, 5 printed.
But the output would be 6, 6, 6, 6, 6 :o

Wait whaaaaat?
--------------
First, when the last loop executes, the engine goes back to the loop's head to check the condition,
it increments i to 6, then sees that conditoon is false, that's why i = 6;

All the timers start AFTER the loop finishes, but that's not the main problem.
THE MAIN PROBLEM is:
    we assumed that each iteration has its own copy of i and each timer will print that copy!
    BUT, i is in the loop's scope, so it's actually SHARED among all timers.

That is why they all print 6 not 5!
How can we fix that?
Well, we could try to do this:
////
for (var i=1; i<=5; i++) {
	(function(){
		setTimeout( function timer(){
			console.log( i );
		}, i*1000 );
	})();
}
////
We used an IIFE to create a new scope each iteration in order to let each timer close over a new scope.
But that's not going to work, even if the function closes over it's own scope.
We need a copy of i at EACH ITERATION:
////
for(var i = 1 ; i <= 5 ; i++) {
    (function() {
        var j = i;
        setTimeout(function timer() {
            console.log(j);
        }, j*1000);
    })();
}
////
NOW, that works and it prints 1, 2, 3, 4, 5 ^_^

Making a scope each iteration is called a per-iteration block scope.
But remember, we can already do this by the keyword 'let':
////
for(var i = 1 ; i <= 5 ; i++) {
    let j = i; //now j is per-iteration ^_^
    setTimeout(function timer() {
        console.log(j);
    }, j*1000);
}
////
'let' means that the variable will be declared at EACH iteration.

Modules:
--------
////
function cool() {
    var a = 2;
    var b = 3;

    function doThis() {
        console.log(a);
    }
    
    function doThat() {
        console.log(b);
    }
}
////
This is a normal function with private variables to it, and functions closing over the bigger function.

But now consider this:
////
function cool() {
    var a = 2;
    var b = 3;

    function doThis() {
        console.log(a);
    }
    
    function doThat() {
        console.log(b);
    }
    return { //returns object
        doThis: doThis,
        doThat, doThat
    };
}

var foo = cool();
foo.doThis(); //output: 2
foo.doThat(); //output: 3
////

This style of writing functions is called "modules".
First, it's just a function.
It has to be executed to create an instance of an object or "module" and return it.
(Kinda like the factory pattern).
But if that function wasn't executed, we wouldn't get the object and the inner scope of it won't be created.

Also, the object ONLY references the inner functions, not the inner variables.
That means, we can't access the variables using the object, we can only get them using the inner functions because the inner functions close over the outer function's scope.

i.e. the object returned is a public API for our module :o
Note: we can just return the inner functions directly we don't have to return an object.
    An example of this is jQuery, the 'jQuery' and '$' identifiers are the public API for jQuery module, but they are just functions.

To make a module:
1. There must be an outer enclosing function and invoked to return an object.
2. The enclosing function must return at least ONE inner function so that it has closure over the private scope and can access it.

////
var foo = (function cool() {
    var a = 2;
    var b = 3;

    function doThis() {
        console.log(a);
    }
    
    function doThat() {
        console.log(b);
    }
    return { //returns object
        doThis: doThis,
        doThat, doThat
    };
})();

foo.doThis(); //output: 2
foo.doThat(); //output: 3
////
We just made it into an IIFE and the return value is returned into foo immediately.
This variation only creates ONE instance of that module to be used.
(Kinda like the singleton pattern)

Modules are just functions, you can send them arguments if the object created depend on them, which is powerful.
A more powerful variation:
////
var foo = (function cool() {
    var a = 2;
    var b = 3;

    function doThis() {
        console.log(a);
    }
    
    function doThat() {
        console.log(b);
    }

    var publicAPI = { //returns object
        doThis: doThis,
        doThat, doThat
    }

    return publicAPI;
})();

foo.doThis(); //output: 2
foo.doThat(); //output: 3
////
By retaining an inner refernce to that public API, you can modify it from inside, you can add/remove/update methods, props, values, etc.

Look at this too:
////
var foo = (function moduleManager() {
    var modules = {};

    function define(name, dependencies, implementation) {
        for(var i = 0; i < dependencies.length ; i++) {
            dependencies[i] = modules[dependencies[i]];
        }
        modules[name] = implementation(apply(implementation, dependencies);)
    }

    function get(name) {
        return modules[name];
    }

    var publicAPI = { //returns object
        define: define,
        get: get
    }

    return publicAPI;
})();
////
This is an illustration example, you can use this module to make many instances and track them by name from the arrays inside here!
You can define them using the function 'define', and you can get them by 'get'.
You pass in a name, dependencies if any, and an implementation of that module.

i.e. instead of doing this to make an instance:
////
var foo = (function cool() {
    ...
})();
////
You can do this:
////
moduleManager.define("instance1", [], function cool() {
    ...
});
////

Then, you can access this module like this:
////
var instanceOne = moduleManager.get("instance1");
////

All of that uses the power of closure, and what this means, essentially, is that
we can make modules as plugins without DEPENDENCIES.
This is good design at its best.

Note: function-based modules can be modified in runtime.
Note: ES6's modules are static, they can't be modified at runtime.

ES6 Modules have to be defined in separate files, one per module.

DONEEEEEE
Note: Read the appendices.
--------------------------------------------------------------------------------------------------------------------------------------------------------
