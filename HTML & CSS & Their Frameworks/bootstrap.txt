Bootstrap 3:
============
Grid:
-----
- Boostrap grid has 4 classes:
    xs: for phones (xs < 768px)
    sm: for tablets (768px <= sm < 992px)
    md: for small laptops (992px <= md < 1200px)
    lg: for laptops and desktops (1200px <= lg)

How to use the grid:
- Create a row div.
- In it, we can create col-*-* divs up to 12
    e.g. col-sm-4 (takes 4 columns in small class)
////
<div class="container">
    <div class="row">
        <div class="col-*-*"></div>
        <div class="col-*-*"></div>
    </div>
</div>
////

- All rows must be placed in a .container or .container-fluid class div.
- Contents are placed in columns, and only columns can be placed in rows.
- Column widths are in percentage, so they are fluid and sized relative to parent element.

Bootstrap features:
- <small> a tag can be used to create secondary text in headings.
- <mark> to highlight a certain part of a text.
- <abbr> to write an abbreviation and when the user hovers over it, the full name appears (magic!).
- <blockquote> to write a quote but style it in a certain block (search for pictures and you'll get it).
- .blockquote-reverse class: to make blockquote be written from right to left.
- <kbd> used to highlight keyboard shortcuts in black and color text in white.
- 