Practice guide jQuery:
======================
iii. jQuery plugins:
--------------------
The steps in brief:
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