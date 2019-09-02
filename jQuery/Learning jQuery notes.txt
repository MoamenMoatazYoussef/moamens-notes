Learning jQuery notes:
======================
A general purpose abstraction layer for common web scripting.
It can:
    - access elements in DOM.
    - modify CSS.
    - modify HTML.
    - animations.
    - 7agat tanya kteer 7elwa.

Starting jQuery code:
---------------------
The jQuery code can be started when the DOM has been loaded but before images are fully rendered, and befor CSS has been applied.
This is done by wrapping jQuery code in $(() => { //here });
////
S(() => {
    //here we write our jQuery code.
    $('.selected-class').addClass('new-class);
}); //Now CSS is applied and images are rendered.
//// this code selects ALL elements having 'selected-class', and adds 'new-class' to their class lists.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 2:Selectors in jQuery:
------------------------------
$(css_selector);

Anything selected by jQuery return in a jQuery object, which are easy to work with.
jQuery elements are not the same as DOM elements or nodeLists.

The $() function acts as a factory, returning a new jQuery object pointing to the corresponding element(s).
The $ is just an alias for jQuery, and can be replaced by jQuery.

Manipulating CSS classes:
    .addClass();
    .removeClass();

Note: study advanced CSS selectors.
Note: study regular expressions because they can be used in CSS selectors.

(This chapter has some more, but most of it is focused on CSS selectors and how to use them with jQuery, so I'll skip it for now, but it's VERY important)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3: Handling events:
---------------------------
On page load:
////
$(() => {
    //Here a code that will run once the HTML loads, this is useful to run code that depends on HTML.
});
////

'window.onload' event, it does the same things, BUT there is a difference.
- $(() => {}): this fires when the whole DOM is ready for use, this means that all elements are accessible, but doesn't necessarily mean that all the HTML and all associated files are downloaded.
- window.onload: this fires when the WHOLE document is downloaded to the browser.
e.g. if we have a page with lots of images,
    - $(() => {}) will fire when the HTML is downloaded.
    - window.load will fire when the HTML is downloaded AND all the images are loaded.

$(() => {}) is almost always preferred, but keep in mind that when it fires, properties related to images and other unloaded files may NOT be available.
Note: both can exist.
Note: it's good practice to place all stylesheets in the HTML elements BEFORE any script runs, so that JS and jQuery operate on styled elements.

we can do this:
function doStuff() {
    //bla bla bla
}

window.onload = doStuff;

Handling many scripts:
----------------------
First, we can assign functions to the event 'window.onload':
////
window.onload = doStuff;
window.onload = doOtherStuff;
////
But, now window.onload will execute the second function only once fired..

In contrast, the event $(() => {}) doesn't do that, it adds functions to an internal queue and once fired will execute them in order of their registration.
