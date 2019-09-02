Eloquent JS notes part 2:
=========================
Tips:
-----
- anything between //// and //// is code.
- notes will be divided according to projects in the book, so:
    - part 1: chapters 1 until 6. (Done)
    - part 2: chapters 8 until 11.
    - part 3: chapters 13, 14, 15 (we read these before).
    - part 4: chapters 17 until 20.
- chapters 7, 12, 16, 21 are projects, I'm not sure if there will be time to do them honestly so I don't know if I'll include them :'D
- I assume that you're not a noob at programming so some stuff is skipped e.g. what are loops, etc., not that being a noob is bad or anything, we've all been noobs xD
-------------------------------------------------------------------------------------------------------------------------------
Chapter 8: Bugs and errors:
---------------------------
The problem with JS is that some syntax errors will actually pass and be computed, they'll produce NaN.
So, computing something like:
    true * "pikachu" - 3;
will actually work, it will produce NaN, sure, but that also means it won't be detected as syntax error.

So, this garbage value may be detected at a later function calling it, you might debug the function a lot when really the error is in the value.
And the garbage value may not be detected at all and you just see a strange output.

So how do we solve this?
------------------------
Strict mode:
---------------
At the top of the file or a function, write "use strict";.
////
"use strict";
//normal JS code
////

Without strict mode VS with strict mode:
1. If you forget the "let" keyword while declaring a variable in a function:
    - normal mode: JS creates a global variable quietly.
    - strict mode: an error is reported.
2. If a function
2. If a function is called but it's not a method of an object, the 'this' keyword holds 'undefined', so using it:
    - normal mode: refers to the global scope object i.e. an object whose properties are the global bindings.
    - strict mode: will produce an error.
    ////
    function Pokemon(species) {
        this.species = species;
    }
    let pikachu = Pokemon("Pikachu"); //where's the 'new' keyword?
    console.log(species);
    ////

    Here, we forgot to use 'new' to call the constructor and make a new object.
    This means, the constructor was called incorrectly here, and 'this' referred to the global object.
    This is why we were able to log 'species', now it's a global object!
    But, if we use strict mode here, we'll get an error "Cannt set property 'species' of undefined".

Note: when we say it will produce an error, that is a GOOD THING, errors tell you where the problems are, without them your time debugging will triple,
    so remember that our main goal here is to GET errors, to PRODUCE errors, since JS doesn't do that often by its own, having errors in the console sure
    is annoying and frustrating if the error persist, but not having them and seeing a bad output or worse, not catching a bad output in QA and shipping it
    to the customer then the customer finds the error and reports it to you, affects you, your company, your CV, etc.
    So errors are good, they're like pain we feel when we get hurt, it's unpleasent but it's there to urge us to treat the injury/fix the bug.

Note: constructors created with 'class' notation will always produce an error about that, strict mode or not.
Note: do NOT use the keyword 'with', it's buggy and a lot of people recommend against it.

Types:
------
JS doesn't require you to specify the datatype.
This is flexible, but some confusion may arise about what kind of value goes into/comes out of a function.
So, there are some conventions for writing types in JS, and there are some dialect(form of language) to do that,
one popular dialect is TypeScript.

Testing:
--------
Running the program over and over to test it is not a good idea.
Instead, we can AUTOMATE testing.
Why? because computers are made for repititive tasks, and testing is a repetitive task.

This is why QA people are awesome, they are the brain that produces test cases and scenarios, then they let the computer handle the donkeywork.
(Shoutout for all QA people, without you we developers are just people beating around bushes).

Writing tests may make you write more code, but saves SO MUCH time and bugs.
Note: try Test Driven Development, or TDD, it's a development method, in brief it is:
    1- Before you write code to achieve something e.g. a function or whatever, write ONE TESTCASE(doesn't have to cover all functionality) for that piece of code.
    2- Run the test, it will fail.
    3- Now, write code to JUST pass this testcase.
    4- Run the test, it should succeed.
    5- Write another test case(doesn't have to cover what's left of the functionality)
    6- go to step 2 and repeat until you cover all functionality.

    Cons: you write more code.
    Pros:
        - you catch and fix bugs before they go into bigger modules, thereby much easier debugging.
        - this method enforces you to divide your code into smaller modules and functions in order to test each on its own i.e. you enforce good design.
        - you understand your code thoroughly so if any error happens after that, you know where to look.
        - you save a lot of the QA people's time so they can focus on business testcases, much less bugs in the finished product(s), much less customer complains.
        - you can communicate efficiently with QA people.
        - you can easily document your code using examples, the examples can be easily got from testcases.
        - you gain experience in bugs and now you can anticipate them when you see the design of the code or the software as a whole.
        - you become friends with QA people, which is an uncommon case among developers for some reason.
        - you write more code, you get more experience.
        - if your company is in busy times and the QA people are torn between many projects so they're not available all the time, you can test yourself.
        - and you can help the QA people if you want,
        - doing both of these things without taking credit off QA people will make THEM give you credit, and you'll be friends with QA people.
        - you'll be awesome <3
    
So how to write test cases anyway?
----------------------------------
////
//this is the function we want to test
function addTen(number) {
    return number + 10;
}

//first, we write a simple function that logs 'failed' if the testcase failed.
//label: just a name for the testcase, optional argument.
//body: this is the body of the testcase itself, we'll see how it's written
//but basically it is a FUNCTION should return true or false.
function test(label, body) {
    if(!body()) {
        console.log(`failed: ${label}`);
    } //we can log 'success' if the test case succeeded if we want.
}

//now, we'll write the body of the testcase, it can be written like this:
function testcase1() {
    let function_output = addTen(5);
    let expected_result = 5 + 10;
    return function_output === expected_result;
}

//as you can see, it's very straightforward, of course if the tested function is complicated, the testcase may have some complications, but not much honestly, the same concept.

//now to test, we just do this:
test("Test Case 1", testcase1);

//notice that we pass the reference to the testcase function in order to test it.
//writing a function for EACH testcase can be tedious, also using function_output and expected_result can be avoided I just wrote them to make things simple,
//so we can just do this:

test("Test Case 2", () => {
    return addTen(20) === (20 + 10);
});

test("Test Case 3", () => {
    return addTen(750) === (750 + 10);
});

test("Test Case 4", () => {
    return addTen(-2) === (-2 + 10);
});

//That is how you write test cases, this type of testing is part of unit testing.
//Now you're not a noob in QA, at least not a literal noob xD
////

Test runners:
-------------
Because the software community is awesome, people have noticed that writing tests like this may be really repetitive.
So, some SOFTWARES are made just to make and run testcases fast, they are called test runners.

Note: in my opinion, writing test cases by hand is more fun, especially at competitions and Hackerrank <3

Debugging:
----------
This is finding out WHAT the real problem is, you know this is the hardest thing about programming xD
One way is what we all do: using print statements everywhere xD
But not really everywhere, just place them in strategic places.

An alternative is to use the browser debugger, setting breakpoints, inspecting current value of bindings, etc.
Learn the debugger you're using e.g. chrome debugger, eclipse debugger, VS debugger, etc.

Note: has anyone tried Qt before? it's a c++ framework that has the BEST debugger ever <3

Crashes:
--------
Sometimes, errors may not be caused by bugs e.g. wrong user input, overloaded work, etc.
If these cases are likely, you must handle how to respond to them:
- You may try to solve the problem and continue.
- If it's unsolvable, maybe you ignore the input and alert the user.

Anything is better than crashing.

Exceptions:
-----------
- When a function cannot proceed normally, we want it to stop, go to a place that knows how to handle the problem.
- Stack unwinding: return from not only this function, but all the callers too, until the first call that started the current execution.
- Stack unwinding happens when an exception is thrown.

- As it's unwinding, we can set some points where we STOP the unwinding and catch the exception, this is what "catch" does.
////
function catchPokemon(player_level, pokemon_level) {
    if(typeof pokemon_level != 'number')  {
        throw new Error(`pokemon_level must be a number`);
    }
    if(typeof player_level != 'number') {
        throw new Error(`player_level must be a number`);
    }

    if(player_level > pokemon_level)    {
        return "Easily caught";
    }
    if(player_level === pokemon_level) {
        return "Hardly caught";
    }
    else {
        return 'You need more training';
    }
}

try {
    catchPokemon(9, 11);
} catch(error) {
    console.log(`Something is wrong: ${error}`);
}
////

- The call doesn't pass either conditions of the function, so it 'throws' an error using the keyword 'throw'.
- This means, we're intentionally raising an error, here we do this to handle cases that should not happen because they're just wrong, 
    like inputting pokemon_level or player_level as letters or strings, not numbers.
- Then, below we have a 'try-catch block', we use this block at the point of our code where we want to stop the stack unwinding and catch the error,
    once we choose this part of the code, we do this :
    1. the function to be monitored for errors is put in a 'try block', this is surrounded by:
    ////
    try {
        //whatever code to be monitored for errors
    }
    ////
    2. below it, we use a 'catch block', this block contains what happens WHEN we catch an error.
    ////
    catch {
        //do something now that the error has happened e.g. warn the user, etc.
    }
    //// 
    3. you may want to use a 'finally' block, a block that will execute NO MATTER what happens when you run the code in the 'try' block.
    ////
    finally {
        //
    }
    ////
    After this, the stack continues unwinding.

Note: it's very very useful code-wise, business-wise, QA-wise, and even career-wise, to do this when implementing a function that's expected to have errors:
    Step 1: GET AWAY FROM THE COMPUTER, DO NOT WRITE CODE.
    Step 2: get a pen and paper, write down the function (in simple words or diagrams) in this sequence:
        - what is the expected input and the datatype expected.
        - what is the expected output and the datatype expected.
        - what are the major operations done on the input to go to the output.
            High-level operations, not detailed operations e.g. "transform array into bla bla".
    Step 3: (optional) use diagrams, draw a flowchart, state diagram, and sequence diagram for the functionality.
    Step 4: use what you did in step 2 and three to think of as many cases for the input, output, process flow, etc.
            - as many as possible: don't be a perfectionist, but don't wing it either.
    Step 5: decide how you will behave if each case occurs.
        Step 6: decide how you will implement the normal flow among these corner cases.
            - While you're doing that, think about if there's additional corner cases you think of later, 
            how you'll design the code so that it's easy to add new corner cases.
        Step 6.1: (optonal but strongly recommended), write testcases for each case, and write testcases for the normal flow.
        Step 7: NOW go and write code for the function.
        Step 8: use the testcases created in 6.1 to test all the corner cases you thought of.

    This method is very good, but it's time consuming, so you may prefer not to use it on small function, which is fine.
    But I recommend getting used to it by using it in every function for a while, it'll become easier and faster for you.

What if we DON'T catch the exception?
-------------------------------------
When the stack unwinds completely, the environment handles the exception, which differs according to each environment.
e.g. the JS debugger in chrome just writes it in the console.
    but Node.js aborts the whole process. i.e it crashes.

In other languages, we can specify what does a catch block catch, e.g.
//// In java
    try {

    } catch (NullPointerException ex) {
        //handling code for NullPointerExceptions
        System.out.println(ex.message());
    } catch (ArithmeticException ex) {
        //handling code for ArithmeticExceptions
        System.out.println(ex.message());
    } catch (IOException ex) {
        //handling code for IOExceptions
    } catch (FileNotFoundException ex) {
        //handling code for FileNotFoundExceptions
    }
////
Here, each exception has a Type, and we can define catch blocks to catch specific types of exceptions.

JS doesn't have that, it's a catch block and you catch an error, so you will either catch all exceptions, or none of them.
"Gotta catch'em all"
"Gotta catch'em aaaall"
"exceptions!"

"who's that exception?" xD

A workaround of this is to check the message of the exception we caught, is it an exception we're interested in or no?
But that's kind of bad.

We can do this:
////
class InputError extends Error {}

function promptDirection(question) {
  let result = prompt(question);
  if (result.toLowerCase() == "left") return "L";
  if (result.toLowerCase() == "right") return "R";
  throw new InputError("Invalid direction: " + result);
}
////

Here, we DEFINE a new class that extends the Error object, it doesn't have its own constructor so it gets Error's constructor,
actually it doesn't define anything new, it just is a different class just for us to know.
Then, we can now do this:
////
try {
    //code to be tested
} catch (e) {
    if (e instanceof InputError) {
        //handle this error.
    } else {
        //handle other errors, or throw the error to be handled by another try/catch block in a higher scope.
        throw e;
    }
}
////

Assertions:
-----------
These are like if conditions that if failed, throw an error.
These are used to detect programmer mistakes.

Usually they're not used except for some few things.

-------------------------------------------------------------------------------------------------------------------------------
Chapter 9: Regular Expressions:
-------------------------------
Regular expressions(regexp) are patterns we can use to search and process strings.
They are strings that are used to classify strings into two sets:
    - Strings that satisfy the regular expression, or has the pattern.
    - Strings that don't

For example:
"abc": this is a regular expression, it means "any string that has the letters a, followed by b, followed by c"
    Strings that has this pattern, or satisfy it: "abcabc", "hjsieopteabc", "I can sing the alphabet, abcdefg, htjkl-o-mn-o-p"
    Strings that don't satisfy it: "a-b-c", "a-bc", "I can even sing the alphabet backwards, zyxwvut, ... cba".

"[abc]": the square brackets means any character in this set of characters, this expression means "any string that has a or b or c.
    Strings that satisfy it: "attack on titan", "bleach", "claymore", "code geass", "death note", "nasser", "manar", "mira", "tarek".
    Strings that don't satisfy it: "hunter x hunter", "berserk", "mo2men", "mohd", "younes", "ghoroud",

"[A-Z]|[a-z]": the square brackets are used to specify ranges, this expression means "any string that has any uppercase character from A to Z, or any lowercase character from a to z.
    Strings that satisfy it: "just think of any word"
    Strings that don't: "911", "62934780", "!@#$%^&*()"

"a*": the star means 0 or more of this character, the expression means "Any string having 0 or more 'a'"
    String that satisfy it: every string possible.

"a+": the + means 1 or more.

"a_": the underscore means any ONE character, this expression means "any string that has the character 'a' followed by any one character"
    Strings that satisfy it: "blablab", "sup man", "how're you man", "gimme fuel gimme fire gimme that"
    Strings that don't: strings that don't have 'a', and I think strings that have 'a' followed by more than one character (not sure)

"a%": the % means any NUMBER of characters, this expression means "Any string that has a, followed by any number of characters"
    Strings that satisfy it: "gimme fuel gimme fire gimme that which I desire"
    Strings that don't: strings that don't have a.

There is much more to regexps, it's a CS subject on its own so go read a book about them if you want.
They are very useful and very awkward.

Creating a regexp:
------------------
In JS there's an object RegExp, used to create regular expressions.
Or we can use the slash '/' to create them
////
let r1 = new RegExp("abc");
let r2 = /abc/;
////

(I'll skip the rest of this chapter until I finish the book, regexps are very useful but I'll skip them for now, if they don't end up here they'll have a summary of their own)
-------------------------------------------------------------------------------------------------------------------------------
Chapter 10: Modules:
--------------------
Module: a piece of program that specified:
    - what functionality it provides for other pieces of the program. (Interfaces).
    - what it depends on.

How can we design our JS code in modules?
-----------------------------------------
First of all, dividing code into files, but that doesn't do the trick.

Package: a piece of code that can be copied, installed, etc.
    It can have one or more modules, and info about what it depends on.
    And documentation on how to use it.
    When a problem is found in the package, the package is updated, and programs that depend on it can upgrade to a new version.

In order to do this, we need to store packages in a place where it's easy to find, install, and upgrade them.
This infrastructure is provided by NPM

NPM: (https://npmjs.org)
----
It's two things:
- An online service where we can download and upload packages.
- A program bundled with Node.js that helps us install and manage packages.

How to evaluate data as code:
-----------------------------
Sometimes, we can expect an input string to be a string of actual JS code and we're supposed to run it.
////
var input = "
    var z = x + y;
    return z;
"
////

We can use the Function constructor, it takes two arguments:
    - a string: comma- separated list of arguments.
    - a string: the function body.

////
var input = ` \ //we use \ after a line if we break it in two.
 	var z = x + y; \
    return z;`;

var sumOfTwo = Function("x,y", input);
console.log(sumOfTwo(14, 5));
////

CommonJS:
---------
The concept is a function called 'require', which is essentially like '#include' in C++ or 'import' in java and python.
It loads the module and returns its interface.

And it gets their intefaces and puts them in an object bound to 'exports'.
'exports' is a special object included in Node.js in every JS file

////
const ordinal = require("ordinal");
const {days, months} = require("date-names");

exports.formatDate = function(date, format) {
  return format.replace(/YYYY|M(MMM)?|Do?|dddd/g, tag => {
    if (tag == "YYYY") return date.getFullYear();
    if (tag == "M") return date.getMonth();
    if (tag == "MMMM") return months[date.getMonth()];
    if (tag == "D") return date.getDate();
    if (tag == "Do") return ordinal(date.getDate());
    if (tag == "dddd") return days[date.getDay()];
  });
};
////

(Not finished yet!)
-------------------------------------------------------------------------------------------------------------------------------
Chapter 11: Async Programming:
-----------------------------

