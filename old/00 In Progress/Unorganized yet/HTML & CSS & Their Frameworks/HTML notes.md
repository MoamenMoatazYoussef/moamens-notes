HTML fundamentals:
==================
Intro:
------
tags:
- doctype: which standard are we following.
- head: metadata that is not shown.
- title: title of the document.
- meta: specify metadata about the document.
- script: to specify inline scripts to manipulate the page (e.g. JS)
- stype: same, css.
- link: we tell the client there are other docs that link to here.

Text elements in HTML:
----------------------
- Headings:
    - H1: primary heading of the doc, used by search engines, one per page.
    - H2-H6: sub-headings.
- Block elements: they go to a new line. e.g. p, div
- Inline elements: on the same line. e.g. span
- In some HTML standards, inline elements can't contain block elements.

- In block and inline elements, browsers ignore many whitespaces or newlines.
- If you want to keep the extra spaces, use the <pre> tag "preformatted text".
- <br/>: this is an explicit line breaK
- <hr/>: horizontal line.
- Some more text elements:
    - <sub> subscript
    - <sup> superscript
    - <cite> cite work by another
    - <abbr> abbreviation
    - <acronym>
    - <em> italic
    - <strong> bold
    - <code> code block
    - <samp> code sample
    - <kbd> keybaord input
    - <var> code var
    - <blockquote> block level quotes
    - <q> inline quotes

Lists in HTML:
--------------
Types of lists:
- Unordered e.g. bullet.
- Ordered e.g. number or alpha labeled.
- Definition list: term + definition.

- <ul> for unordered:
    - <li> for line item.
- <ol> for ordered:
    - <li>

Links:
------
- <a href="link here">Blablabla</a>
- We can also link to specific targets in our page.

Images:
-------
- We can use images.
    <img src="path to image.jpg" alt="text to replace if image can't be rendered" height="93" width="125"/>

- We can also use plugins (flash, siverlight, etc.), applets, videos.
    <object data="path to data" type="type of object" width="65" height="23">
        <param name="source" value="path to base code of the object"/>
        <param name="initParams" value="stuff" /> //these are parameters that we can use to give to the application we're running e.g. silverlight.
    </object>

Tables:
-------
- Rows and columns.
- <table border="1" summary="summary of table">
    <caption>awesome table</caption>
    <thead>
        <tr>
            <th>1st col</th>
            <th>2nd col</th>
        <tr>
    </thead>
    <tfoot>
        <tr> 
            <td> footer <td>
        </tr>
    </tfoot>
    <tbody>
        ....
    </tbody>
  </table>


Pro tips:
---------
If you want to make a generic CSS property, make a class containing that property, then whenever you want, assign elemetns in HTML to this class.
////
.m-b-20 { //this is a class for margin bottom that is 20
    margin-bottom: 20px;
}

<html>
    ...
    <body>
        ...
        <img class="images m-b-20" ... />
        <div class="divisions m-b-20" ...> //now any assignment like this will add a margin botton 20 to the element
            ...
        </div>
    </body>
</html>

////