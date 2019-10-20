How to create a jQuery plugin:
------------------------------
Tips:
-----
- This isn't really a summary, it's me explaining this in as easy terms as possible.
- Anything between //// and //// is code.
- This is a summary of the tutorial in this link:
https://learn.jquery.com/plugins/basic-plugin-creation/
and this link:
https://learn.jquery.com/plugins/advanced-plugin-concepts/
-------------------------------------------------------------------------------------------------------------------------------------------------
What is a plugin?
-----------------
It's a piece of functionality available throughout your code, and can be exported to the public.
In order to understand how to make a plugin, we need to understand some stuff about how jQuery works.

////
$('a').css('color', 'red');
////
This is a basic jQuery code, in details this is what happens:
- When we use the $ function, it returns a jQuery object.
- This object gets some methods from '$.fn' object, these methods are what we use e.g. .css(), .click(), etc.

So, if we want to write owr own methods, we need to contain these.

Our example will be a plugin that makes a text green.

-----------------
Step 1: creation:
-----------------
We need to add a function 'makeGreen' to '$.fn'.
////
$.fn.makeGreen = function() {
    this.css('color', 'green');
};
////
Note that we use 'this', not '$('this')'.
Because our function is part of the same object as '.css'.

-----------------
Step 2: Chaining:
-----------------
Chaining is a feature in jQuery, essentially we can apply many actions on one selector e.g.
////
$('a').doThis().doThat().blaBla().blaBlo();
////
We can only do that if each function returns the same jQuery object.
So, we need to do the same in order to enable chaining for our plugin:
////
$.fn.makeGreen = function() {
    this.css('color', 'green');
    return this; //here we enable chaining by returning the same jQuery object.
};
////

----------------------------------------
Step 3: protecting '$' and adding scope:
----------------------------------------
If we use jQuery and another library that uses '$' in the same code, we can make jQuery not use the '$'  by using:
////
jQuery.noConflict();
////
But, this does not mean that our plugin is treated the same way.
Our plugin is written using the '$'.

To solve this, we use an IIFE and pass the alias 'jQuery' to the function, naming it '$':
////
(function($){
    $.fn.makeGreen = function() {
        this.css('color', 'green');
        return this;
    };
}(jQuery));
////
Now, we can use both '$' and 'jQuery'.

Also, using an IIFE provides a scope where we can have our own private variables.

------------------------------------
Step 4: minimizing plugin footprint:
------------------------------------
Think of jQuery as a hotel, that has rooms, when someone reserves a room, the number of empty rooms decreases.
- someone reserving a room: a plugin.
- room: a slot in $.fn.
When we write plugins, it's better to take ONE slot only.

Example:
--------
We can do this:
////
(function($) {
    $.fn.makeGreen() = function() {
        ...
    };

    $.fn.makeBlue() = function() {
        ...
    }
}(jQuery));
////
But, this means we now have TWO functions for ONE plugin, that is not good practice.
A much better approach is to just do this:
////
(function($) {
    $.fn.colorize = function(color) {
        if(color === "green") {
            this.css('color', 'green');
            return this;
        }

        if(color === "blue") {
            this.css('color', 'blue');
            return this;
        }
    }
}(jQuery));
////

Generally, just make one generic function and pass everything as parameters if needed:
////
(function($) {
    $.fn.colorize = function(color) {
        this.css('color', color);
        return this;
    }
}(jQuery));
//// we just passed the color as parameter and apply it, that's because THIS example allows that, not everything.

----------------------------------
Step 5: using the 'each()' method:
----------------------------------
A jQuery object can refer to one DOM element, or many.
If it refers to many elements, we need to use '.each()' in order to loop through elements.
////
$.fn.makeGreen = function() {
    return this.each(function() {
        //do something for each element here.
    });
};
////
.each() is already chainable, it returns this, so when we return this.each(...), we still maintain chainability of our plugin.

--------------------------
Step 6: accepting options:
--------------------------
Making plugins customizable is a very good idea.
How to do this?

We use OBJECTS.
We make an object called 'settings', this object holds default settings.
Then, we use the function '$.extend()', 
    this function takes an object argument,
    then uses it to merge it to the jQuery prototype 
        i.e. add new properties with new names,
             and override existing ones if we use existing names.
So, if new properties are in the passed object, they're added to the jQuery prototype.
and if properties with existing names e.g. 'color' are added, they override the default ones.
So, we can use this to define default behaviors in an object passed:
////
{
    color: 'blue',
    backgroundColor: 'blue'
}
////

Then, override this default if some options are passed:
////
(function($) {
    $.fn.colorize = function(options) {
        var settings = $.extend( {
                color: "blue",
                backgroundColor: "white"
            }, options);
        
        return this.each(function() {
            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
            });
        });
    }
}(jQuery));
////

Now, we can do this:
////
$('.normal-divs').colorize(); //will maintain the default behavior here

$('.to-be-colored-divs').colorize({
    color: "orange"
}); //will override the default behavior here
////
------------------------
Putting it all together:
------------------------
Now, here's the full plugin:
////
(function($) {
    $.fn.colorize = function(options) {
        var settings = $.extend( {
                color: "blue",
                backgroundColor: "white"
            }, options);
        
        return this.each(function() {
            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
            });
        });
    }
}(jQuery));
////
-------------------------------------------------------------------------------------------------------------------------------------------------
Advanced concepts:
------------------
Step 7: Provide public access to default plugin settings:
---------------------------------------------------------
Why do that? because it makes it easy for users to override these default settings with minimal code.
////
(function($) {
    $.fn.colorize = function(options) {
        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
            });
        });
    };

    $.fn.colorize.defaults = { //see here we exposed them as a property.
        color: 'blue',
        backgroundColor: 'white'
    };
}(jQuery));

//Now users can access this property like this e.g.:
$.fn.colorize.defaults.color = 'yellow';
////
-----------------------------------------------------
Step 8: Provide public access to secondary functions:
-----------------------------------------------------
This helps you extend your plugin, and others to extend your plugin.
For example, let's say we have a function that puts a border around the modified element:
////
(function($) {
    $.fn.colorize = function(options) {
        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            
            $(this).addBorder(); //this function is a secondary function called here

            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
            });
        });
    };

    $.fn.colorize.defaults = { //see here we exposed them as a property.
        color: 'blue',
        backgroundColor: 'white'
    };

    $.fn.colorize.addBorder = function() { //here's the definition
        return this.css({
            border: '1px solid black'
        });
    }
}(jQuery));
////

Exposing this secondary function like this adds a new property to the plugin, but also,
now people can override and redefine that function, which means people can now build custom plugins over your plugin!

---------------------------------------
Step 9: Keep private functions private:
---------------------------------------
Step 8 is powerful because it allows customization.
BUT, there are some main steps that MUST be executed in the way you've defined them, in order for the plugin to work at all!
So, methods that do these steps should NOT BE EXPOSED.

Along with these methods, any methods that:
    Provide backwards compatibility,
    calling arguments,
    semantics,
all of that should be private, make your plugin customizable, not changeable.

Rule: if you're not sure if a method should be exposed or not, go with NO.
    i.e. if you have a strong concrete reason to expose a method, then do, otherwise don't.

But, that means we have to use more names thereby filling the namespace or the 'slots' (Remember the hotel example?).

So how do we add private functions without doing this?
We use CLOSURE!

We'll simply add a function called "debug" which logs the number of selected elements.
Just add a function inside the implementation normally, using 'function funcName()'.

For the closure, we can wrap the entire plugin definition in a function to provide the closure we want.
////
(function($) {
    $.fn.colorize = function(options) {
        debug(this); //here's the call

        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            
            $(this).addBorder(); //this function is a secondary function called here

            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor
            });
        });
    };

    $.fn.colorize.defaults = { //see here we exposed them as a property.
        color: 'blue',
        backgroundColor: 'white'
    };

    $.fn.colorize.addBorder = function() { //here's the definition
        return this.css({
            border: '1px solid black'
        });
    }

    function debug(obj) { //and here's the definition
        window.console.log(`Selected elements: ${obj.length}`);
    }
}(jQuery));
////

Notice that we didn't need to do the second part (wrapping the plugin in a function),
because it's already wrapped in the IIFE we added in step 3.

Now, the 'debug' method cannot be accessed from outside of the closure ;)

-------------------------------------------------------------------------------------------------------------------------------------------------
Customizability:
----------------
Look at this plugin:
////
(function($) {
    jQuery.fn.superGallery = function( options ) {
 
    // Bob's default settings:
    var defaults = {
        textColor: "#000",
        backgroundColor: "#fff",
        fontSize: "1em",
        delay: "quite long",
        getTextFromTitle: true,
        getTextFromRel: false,
        getTextFromAlt: false,
        animateWidth: true,
        animateOpacity: true,
        animateHeight: true,
        animationDuration: 500,
        clickImgToGoToNext: true,
        clickImgToGoToLast: false,
        nextButtonText: "next",
        previousButtonText: "previous",
        nextButtonTextColor: "red",
        previousButtonTextColor: "red"
    };
 
    var settings = $.extend( {}, defaults, options );
 
    return this.each(function() {
        // Plugin code would go here...
    });
 
}(jQuery));
////

This plugin 'looks' really customizable.
But, it's not that customizable.
Why?

Well, if you wanted to use it with a gallery for example, and you'd like to control the animation speed, there's no method or property!
The creator of the plugin has made many many many 'specific' options to customize, that does not mean it's really customizable.

So, we need to think of customizability in a different way:

---------------------------------------------
Step 10: Don't create plugin-specific syntax:
---------------------------------------------
Users using the plugin should NOT use new syntax to use your plugin (What? xD)
Developers using your plugin, should not need to learn a new set of keywords to get your plugin to do the job (that's easier xD).

Let's add a property 'Opacity' and a method 'changeOpacity' to illustrate this.
////
(function($) {
    $.fn.colorize = function(options) {
        debug(this); //here's the call

        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            
            $(this).addBorder(); //this function is a secondary function called here

            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor,
            opacity: '1' //the new property
            });
        });
    };

    $.fn.colorize.changeOpacity(level) { //the new method
        switch(level) {
            case "transparent":
                opacity = 0;
                break;

            case "semiTransparent":
                opacity = 0.25;
                break;

            case "halfTransparent":
                opacity = 0.5;
                break;

            case "semiOpaque":
                opacity = 0.75;
                break;
            
            case "opaque":
            default:
                opacity = 1;
        }
    }

    $.fn.colorize.defaults = { //see here we exposed them as a property.
        color: 'blue',
        backgroundColor: 'white'
    };

    $.fn.colorize.addBorder = function() { //here's the definition
        return this.css({
            border: '1px solid black'
        });
    }

    function debug(obj) { //and here's the definition
        window.console.log(`Selected elements: ${obj.length}`);
    }
}(jQuery));
////

Yes, we can set opacity level, but we limited it to only 5 levels, developers may want 0.35 for example!
Also, this takes some space since these are about 15 lines of code to customize ONE property.

So, a better way is to let users pass a NUMBER for the opacity:
////
(function($) {
    $.fn.colorize = function(options) {
        debug(this); //here's the call

        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            
            $(this).addBorder(); //this function is a secondary function called here

            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor,
            opacity: '1' //the new property
            });
        });
    };

    $.fn.colorize.changeOpacity(level_number) { //the new method
        opacity = level_number;
    }

    $.fn.colorize.defaults = { //see here we exposed them as a property.
        color: 'blue',
        backgroundColor: 'white'
    };

    $.fn.colorize.addBorder = function() { //here's the definition
        return this.css({
            border: '1px solid black'
        });
    }

    function debug(obj) { //and here's the definition
        window.console.log(`Selected elements: ${obj.length}`);
    }
}(jQuery));
////

So, make sure developers using this plugin still have access to low-level control
(This does NOT mean remove all abstractions, keep abstractions but give control to low-level properties USING abstractions, functions, etc.)

---------------------------------------
Step 11: Give full control of elements:
---------------------------------------
If the plugin CREATES elements in the DOM, make a way to access these elements so that the users can do that.
This may mean giving some elements IDs and classes,
BUT making a plugin depend on these internally isn't good.

Note: the plugin colorize doesn't create new elements, so we'll use different examples.

Bad code:
////
$( "<div class='gallery-wrapper' />" ).appendTo( "body" );
$( ".gallery-wrapper" ).append( "..." );
////

Yes, we gave a class 'gallery-wrapper' but that is not good code design, also we directly access the DOM, which isn't really good.

It's better to store these into a variable containing the settings for the plugin.
A better code would be:
////
var wrapper = $("<div/>)
    .attr(settings.wrapperAttributes)
    .appendTo(settings.container);

//Now, we can append it like this:
wrapper.append("...");
////

Note that we can add that to the 'extend' function in our plugin:
////
var defaults = {
    wrapperAttributes: {
        //attributes here
    },
};

//the first parameter 'true' signifies a DEEP COPY
//i.e. the function will recurse through ALL NESTED OBJECTS
//and merge it.
var settings = $.extend(true, {}, defaults, option);
////

Now, the plugin user can specify ANY attribute of the wrapper element.
They can hook any css styles, add classes, change ID, etc.
All of that without going to the plugin source, which is what we need ;)

Manipulating css:
////
var defaults = {
    wrapperCSS: {},
    // ... rest of settings ...
};
 
// Later on in the plugin where we define the wrapper:
var wrapper = $( "<div />" )
    .attr( settings.wrapperAttrs )
    .css( settings.wrapperCSS ) // ** Set CSS!
    .appendTo( settings.container );
////

Now users can even override your default css styles ^_^

---------------------------------------
Step 12: Provide callback capabilities:
---------------------------------------
Callback: a function to be called later and can be passed as an argument.

If our plugin is triggered by events e.g. it has buttons and when we click something happens,
it's a good idea to make it callback i.e. can be passed as argument.
We can even create our custom events and make them callback.


Let's say our plugin makes an element violet when we click on a button.
First, we'll add a button.
In the defaults property, we'll add an empty anonymous function.
Then, we'll add the event, and the handler:
////
(function($) {
    $.fn.colorize = function(options) {
        debug(this); //here's the call

        var settings = $.extend($.fn.colorize.defaults, options);
        
        return this.each(function() {
            
            $(this).addBorder();

            return $(this).css({
            color: settings.color,
            backgroundColor: settings.backgroundColor,
            opacity: '1'
            });
        });
    };

    $.fn.colorize.changeOpacity(level_number) {
        opacity = level_number;
    }

    $.fn.colorize.defaults = {
        color: 'blue',
        backgroundColor: 'white',
        onClick: function() {} //we add an empty anonymous function so we don't need to check its existance before callback.
    };

    $.fn.colorize.addBorder = function() {
        return this.css({
            border: '1px solid black'
        });
    }

    function debug(obj) {
        window.console.log(`Selected elements: ${obj.length}`);
    }

    button.on("click", colorViolet); //here we add event listener and handler

    function colorViolet() {
        //we write the handler here.
    }
}(jQuery));
////

-------------------------------------------------------------------------------------------------------------------------------------------------
Final notes:
------------
Any plugin has these three factors:
1. Flexibility: how many situations will it fit into.
2. Size: in memory.
3. Performance: in speed.

When you design a plugin, you should consider these three and
make compromises according to your objective of the plugin.

-------------------------------------------------------------------------------------------------------------------------------------------------
Summary of steps:
-----------------
1- Create a function and add it to $.fn.
2- Make it return this so it supports chaining.
3- Put it in an IIFE with argument '$' and pass 'jQuery' to protect the '$' alias.
4- Make sure the plugin takes ONE name in the $.fn.
5- Make it return this.each so it supports 'each()' function.
6- Make a var defaults and make it accept external options that override the defaults. 
7- Provide public access to default settings.
8- Provide public access to secondary functions.
9- But hide any functions that affect main structure.
10- Design your plugin to support customizability but don't make specific custimizable options only.
11- Don't make plugin-specific syntax.
12- Give full control of elements and css styles if the plugin creates elements.
13- Provide callback options if the plugin deals with events.

And we're DONEEEEEEEEEEE
-------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------