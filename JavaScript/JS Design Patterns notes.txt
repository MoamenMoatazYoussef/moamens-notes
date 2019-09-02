JS Design Patterns:
===================
Tips:
-----
- anything between //// and //// is code.
- I assume that you're not a noob at programming so some stuff is skipped e.g. what are loops, etc., 
    not that being a noob is bad or anything, we've all been noobs xD
- I also assume that you're good at JS i.e. you know the basics AND how JS works i.e. closure, IIFEs, hoisting, etc.
- the book may not be written in order of chapters because I study one or two chosen patterns at a time,
    but I'll do my best to rearrange them like the book, not guaranteed though.
-------------------------------------------------------------------------------------------------------------------------------
Module Pattern:
---------------
There are 5 ways to create modules in JS:
    - module pattern i.e. using IIFEs
    - objects
    - AMD modules
    - CommonJS modules
    - ECMAScript harmony modules
We'll explore the second, then the first one now.

Objects:
--------
We all know them:
////
var pokemon = {
    species: 'totodile',
    type: 'water',
    weapon: 'water gun',
    secretWeapon: 'tackle',
    attack: function() {
        //code to execute
        //probably uses internal variables
        //e.g. weapons
    }
};
////
Using objects as modules is not good since anything inside them can be accessed anywhere
i.e. their internal data is not private.
So, we opted for IIFEs.

Module Pattern using IIFEs:
---------------------------
This method uses IIFEs to "emulate" encapsulation and abstraction i.e. private and classes.
How?
////
var pokemon = (function() {
    var species      = 'tododile';
    var type         = 'water';
    var weapon       = 'water gun';
    var secretWeapon = 'tackle';

    function attack() {
        //some code here
    }

    var publicAPI = {
        species: species,
        type: type,
        weapon: weapon,
        attack: attack
    };

    return publicAPI;
})();
////

Let's say we want to make the previous object but using an IIFE.
First, we make an IIFE, and inside it we declare our variables and functions.

Question 1: isn't IIFE a function? how will it create an object?
Answer: Well, it will execute and return an object, kinda like a constructor or factory pattern.

Question 2: How will it emulate privacy?
Answer: When the function finishes executing, the scope is gone, so declarations inside can't be accessed from outside,
    except things that we put in the object returned.

So, it's like this:
    The IIFE creates a protecting scope for everything, that's how we protect private variables.
    But, there are public members as well, we make them public by putting them in an object and returning that object.

Question 3: But how will the public members access private ones if the scope is gone at the end?
Answer: CLOSURE, that's how ;)

So, now "pokemon" has an object that contains only four members.
    But the function 'attack' for example can STILL access other elements and 'secretWeapon' through closure.
    So secretWeapon is private, but can be manipulated by public methods with no problem ;)

It's as if the module has private implementation and public API to use, that's why the object returned is usually named publicAPI (Not mandatory).

This pattern is also called Revealing Module Pattern.

Frameworks implementing the module pattern:
-------------------------------------------
Dojo, ExtJS, YUI, jQuery.

-------------------------------------------------------------------------------------------------------------------------------
Observer Pattern:
-----------------
This pattern consists of an Object 'subject', and other objects 'Observers'.
When a change happens in the subject, all the observers are notified of the change (Like the PubSub pattern and I think Observer is the base of PubSub anyway).

Objects can 'subscribe' to the observer to get notified, or 'unsubscribe'.

The pattern usually includes these components:
- Subject: maintains a list of observers, can add/remove them.
- Observer: provides an interface that is notified by the subject.
- concreteSubject: broadcasts notifications to observers.
- concreteObserver: stores a reference to concreteSubject and implementing the interface for the observer.

in JS:
////
function observerList() {
    this.observerList = [];
}

ObserverList.prototype.add = function(obj) { 
    return this.observerList.push(obj);
}

ObserverList.prototype.removeAt = function(index) {
    this.observerList.splice(index, 1);
}

ObserverList.prototype.count = function() {
    return this.observerList.length;
}
////
First, we create a this-aware function that adds an empty list for observers to 'this'.
Then, we add two functions 'add' and 'remove' to add/remove from that list, we add these two functions to the prototype of the first function.

Now, we need to create the Subject:
////
function Subject() {
    this.observers = new ObserverList();
}

Subject.prototype.addObserver = function(observer) {
    this.observers.add(observer);
};

Subject.prototype.removeObserver = function(observer) {
    this.observers.remove(this.observers.indexOf(observer, 0));
};

Subject.prototype.notify = function(context) {
    var observerCount = this.observers.count();
    for(let i = 0 ; i < observerCount ; i++) {
        this.observers.get(i).update(context);
    }
};
////
Now, we create a function 'Subject', that uses the new keyword to create and add an observer list to 'this'.
Remember that new does:
    - Creates a new object.
    - Links this object to the prototype of the function used.
    - Returns that object or returns 'this'.
Then, for the prototype of the function 'Subject', we add methods to add, remove, and update observers.

NOTE: the update function will be overridden later.
////
function Observer() {
    this.update = function() {
        //custom behavior for this observer
    };
}
////
Finally we can create this 'Observer' function to use to create new observers.

Working code:
////
/* <<<<<<<<<<<<<<<<<<<< ObserverList >>>>>>>>>>>>>>>>>>>> */
function ObserverList() {
    this.observerList = [];
}

ObserverList.prototype.count = function () {
    return this.observerList.length;
};

ObserverList.prototype.add = function (obj) {
    this.observerList.push(obj);
};

ObserverList.prototype.get = function (index) {
    if (index > -1 && index < this.observerList.length) {
        return this.observerList[index];
    }
};

ObserverList.prototype.removeAt = function (index) {
    this.observerList.splice(index, 1);
};

ObserverList.prototype.indexOf = function (obj, startIndex) {
    var i = startIndex;
    while (i < this.observerList.length) {
        if (this.observerList[i] === obj) {
            return i;
        }
        i++;
    }
    return -1;
};

/* <<<<<<<<<<<<<<<<<<<< Subject >>>>>>>>>>>>>>>>>>>> */
function Subject() {
    this.observers = new ObserverList();
}

Subject.prototype.addObserver = function (observer) {
    this.observers.add(observer);
};

Subject.prototype.removeObserver = function (observer) {
    this.observers.removeAt(this.observers.indexOf(observer, 0));
};

Subject.prototype.update = function (context) {
    let count = this.observers.count();
    for (let i = 0; i < count; i++) {
        this.observers.get(i).update(context);
    }
};

/* <<<<<<<<<<<<<<<<<<<< Observer >>>>>>>>>>>>>>>>>>>> */
function Observer(i) {
    this.update = function (context) {
        console.log(`Observer ${this.id} called, context is ${context}`);
    }
    this.id = i;
}

/* <<<<<<<<<<<<<<<<<<<< Main code >>>>>>>>>>>>>>>>>>>> */
function init() {
    var newSubject = new Subject();
    var observer1 = new Observer(1);
    var observer2 = new Observer(2);
    var observer3 = new Observer(3);

    newSubject.addObserver(observer1);
    newSubject.addObserver(observer2);
    newSubject.addObserver(observer3);

    newSubject.update('Slim Shady');
    console.log('<<<< >>>>');

    var observer64 = new Observer(64);
    newSubject.addObserver(observer64);

    newSubject.update('Marshall Mathers');
    console.log('<<<< >>>>');

    newSubject.removeObserver(observer2);

    newSubject.update('Eminem');
    console.log('<<<< >>>>');
}
////

In JS, we usually use a variation of the observer called PubSub pattern.
Whilst very similar to Observer, there are some differences:
PubSub uses an event object between subject(or publisher) and observers(or subscribers) 
    to remove dependency between observers and subject.

