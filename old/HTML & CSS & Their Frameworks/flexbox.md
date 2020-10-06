CSS Flexbox:
------------
- It's a feature in CSS where we can make a container "flex", when we do that, the items inside it can grow to fill unused space, or shrink to avoid overflow.
- And it's easier to align elements.
- So it's a very easy way to use float and align properties.

An example of properties of a flexible container, or flexbox:
//// start of code
div {
    display: flex; /* lets children take unused space, shrink to fit in, and stack horizontally. */
    flex-direction: column; /* this lets the children stack vertically */
}
//// end of code
Of course, we can select individual div elements by id or by class and make them flex, we can also do this:
//// start of code
.FLEX {
    display: flex;
}
//// end of code
Now we can assign this class to any div element that we want to be flexible ^_^.

Important: In a flex container (a container with display: flex, we have a Main axis(horizontal), and Cross axis(vertical)), flex properties will use these axes so remember this.
(Search for an image of the axes so you get the idea)

Properties of flex containers:
------------------------------
The container is called a flex container, any item inside it is called a flex item, for each there are properties.

A) Container properties:
    - display: flex; this is not specific to flexbox, it sets the container to be flexible i.e. now we can use flex properties.
    - flex-direction: determines how flex items are placed, possible values are:
        i. row: default value, from left to right.
        ii. row-reverse: right to left. (Main axis inverted)
        iii. column: from top to bottom (Main axis now vertical down, cross horizontal LR)
        iv. column-reverse: bottom to top (Main axis vertical up, cross axis LR)
    
    - flex-wrap: determines if items take many lines in a row or not, possible values are:
        i. no-wrap: default value, no wrapping. If total size of items is > width of container, they will be squished to fit in one line.
        ii. wrap: if total size of items is > width of container, they will wrap around.
        iii. wrap-reverse: same but wraps ABOVE the initial line, from left to right (???)
    
    - flex-flow: this is a shortcut to use both flex-direction and flex-wrap, for example instead of:
            flex-direction: row;
            flex-wrap: wrap;
        We can just do this:
            flex-flow: row wrap;
    
    - justify-content: used to spread the items along the MAIN axis (so if it's column, the justification will be vertical), possible values are:
        i. flex-start: from the left.
        ii. flex-end: from the right (without reversing).
        iii. center: in the center.
        iv: space-between: take full width, don't stretch elements, put spaces between them.
        v. space-around: same but put spaces between elements and edges as well (these spaces may be smaller than spaces between elements).
        vi. space-evenly: same as space-around but the spaces are equal.
    
    - align-items: same as justify content but along the CROSS axis, possible values are:
        i, ii, iii, iv, v. All the values in justify content are available here as well.
        vi. baseline: like flex-end but starting from where top element touches top of container.
        vii. stretch: strech across the height of the container.
    
    - align-content: determines how space between rows should be distributed across cross-axis, possible values are:
        i. stretch: items will wrap and stretch to fill height (but only until the first element in a row reaches end of container).
        ii. flex-start: from start without regard to spacing.
        iii. center:
        iv. flex-end:
        v. space-between: leave spaces between rows.
        vi. space-around: same but with spaces at start and end.

B) Item properties:
    
    - align-self: aligns the item along the cross-axis, overrides the align items property, so for example if we have align-items: flex-start, the elements will be at the top, then if we choose an element an set align-self: flex-end, this item ALONE will be on the bottom.
    , possible values are the same as align-items.
    
    - Order: this property takes a number, for each element we can set Order: *specific number like 4, 5*, then the elements are arranged according to these number from lowest to highest, kind of like a priority.
        for example if we have three elements, #first-elem{ Order: 7; }, #second-elem{ Order: 1; }, #third-elem{ Order: 3000; }, the order will be: Second, First, Third.
        We can give negative numbers.
        The default value is 0.
        The ordering is from left to right but can change according to the flex-direction property (Not sure so I'll try it).
        Possible values are NUMBERS.
    
    - flex-basis: this is a size, if there is space available, the item will take that size.
        if there is not enough space available, it will shrink.
        So this property is equivalent "width" if there's space available, and "max-width" if there isn't.
        Also, this property is equivalent to "height" and "max-height" if the flex direction is top to bottom or el3aks.
        Possible values are SIZES, just like width, height, etc.
    
    - flex-grow: this property is used to determine HOW the item grows to fill any available space.
        For example if we have two items, we set flex-grow: 1 for the first, and flex-grow: 2 for the second, and there is unused space, then:
            the second element will take TWICE as much space as the first element.
        Default value is 1, so if we don't change anything the items will take unused space evenly.
        Possible values are NUMBERS.
    
    - flex-shrink: this property is used to determine HOW the item shrinks when there's not enough space for its full size:
        For example, if we have three items:
            1) flex-shrink: 1;
            2) flex-shrink: 2;
            3) flex-shrink: 0; /* Zero */
        Then if we don't have enough space, the second item will shrink twice as much as the first item.
        But the third item will NEVER shrink (zero).
        Possible values are NUMBERS.
    
    - flex: you know when we do padding, we can do this:
            padding-top: 1px;
            padding-bottom: 1px;
            padding-left: 1px;
            padding-right: 1px;
        Or we can just do this:
            padding: 1px 1px 1px 1px;
        This property is the same, instead of:
            flex-grow: 1;
            flex-shrink: 0;
            flex-basis: 200px;
        We can just do this:
            flex: 1 0 200px;

Main axis and cross axis directions in different flex directions:
- row: 
    - Main axis: Horizontal from LEFT to RIGHT.
    - Cross axis: Vertical from TOP to BOTTOM.
- row-reverse:
    - Main axis: Horizontal from RIGHT to LEFT.
    - Cross axis: Vertical from TOP to BOTTOM.
- column:
    - Main axis: VERTICAL from TOP to BOTTOM.
    - Cross axis: HORIZONTAL from LEFT to RIGHT.
- column-reverse: 
    - Main axis: VERTICAL from BOTTOM to TOP.
    - Cross axis: HORIZONTAL from LEFT to RIGHT.