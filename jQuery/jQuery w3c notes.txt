jQuery w3schools notes:
=======================
- jQuery is tailored for selecting html elements and performing stuff on them.

Main Selector:
$(css-selector)

Running jQuery:
---------------
////
$(document).ready(function() {
    //all the jQuery code is written here.
    //so that it runs after the document is loaded.
});

//shorthand version
$(function() {

});
////

Selectors:
----------
////
$(document).ready(function() {
    $('p').hide(); //selects all p elements and hides them.
    $('.that-class').hide();
    $('#that-id').hide();
    $('p:first').hide(); //the first p element.
    $('[href]').hide(); //all elements with an href attribute.
    $("a[target!='_blank']").hide(); //all elements whose target attribute value is NOT equal to _blank.
});
////

It's good practice to put your jQuery functions in a separate file.

Events:
-------
Just like normal JS events.
////
document.querySelector('.that-class').addEventListener("click", function() {
    //bla bla bla
});

//this is the jQuery syntax
$('.that-class').click(function() {
    //bla bla bla
});
////

- the 'on()' method attaches one or more event handlers for the element.
////
$('.that-class').on("click", function() {
    $(this).hide();
});

$('.that-class').on({
    mouseenter: function() {
        //do stuff
    },
    mouseleave: function() {
        //do other stuff
    },
    click: function() {
        //do even more stuff
    }
});
////

Some effects by jQuery:
-----------------------
A) Hide/show:

.hide(speed, callback);
.show(speed, callback);
.toggle(speed, callback); //toggle between show and hide.

Both arguments are optional, 
    speed can be: slow, fast, or milliseconds.
    callabck can be a function executed after hide/show method completes.

B) Fading:
.fadeIn();
.fadeOut();
.fadeToogle();
.fadeTo(); //fade to a given opacity

C) Sliding:
.slideDown()
.slideUp()
.slideToggle()

D) Animation and stop:
$(selector).animate({params},speed,callback);
//this selector is used to create custom animations.

Callback functions:
-------------------
A function executed AFTER the current effect is 100% finished.
This allows us to CHAIN actions in jQuery.

Chaining:
---------
Running many jQuery methods on the same elements with ONE statement.
////
$("#p1").css("color", "red").slideUp(2000).slideDown(2000);
////
Instead of writing the selector and executing each method in a statement, we can do this, they take effect in the order they're chained in.


jQuery DOM manipulation:
------------------------
$(".that-class").text(): sets/returns text content.
$(".that-class").html(): sets/returns html contant i.e. text including markup.
$(".that-class").val(): sets/returns value.
$(".that-class").attr("href"): returns value of the attribute 'href'.

Example
////
var text_of_stuff = $(".that-class").text(); //getter
$(".that-class").text("Hello mo2men"); //setter

$(".that-class").attr("href", "www.google.com"); //setting an attribute href with .attr() function.
$(".that-class").attr({
    "href": "www.google.com",
    "title": "That awesome class",
    "value": "green"
    }); //setting many attributes at once.
////

All of these functions above come with a callback function:
////
$("#this-element").text(function(i, origText) {
    return `Old Text: ${origText}, new text: Hello World! (index: ${i})`;
});
////
attributes of the callback function:
    i: index of current element in the list of selected elements. 
    origText: original/old value i.e. value the element had before you change it.



