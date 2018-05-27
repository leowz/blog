---
title: CssTips
date: 2018-05-12 23:43:31
tags: css
categories: web
---

## Position property
* static (default)
* relative
* fixed
* absolute
* sticky

elements are then positioned using the: top, bottom, left, right properties. However, position must be set first to let the others to work.

**relative**:
    relative to its normal position
    top, right, bottom, left will cause it to be adjusted away from its original position

**fixed**:
    stay relative to the viewport(the screen)
    will not move even the page is scrolled

**absolute**:
    positioned relative to the nearest positioned ancestor

**sticky**:
    is positioned based on the user's scroll position. sticky when scroll to it.

<!--more-->

## Overflow property
* visible: will render out side of element box
* hidden: overflow clipped, the rest of the content will be invisible
* scroll: a scroll bar is added to the overflow part
* auto: 

When the content of an element is too big to display in the specified area, add scrollbar to this element. 

overflow-x, overflow-y two specific properties for overflow

## Float & Clear property
* left
* right
* none (default)
* inherit

define how an element could float in its container

### clear:
* none
* left: no floating allowed on the left side of the element
* right: no floating allowed on the right side of the element
* both: no floating allowed on the both side of the element
* inherit

### clear fix
If an element is taller than the element containing it and it floats, it will overflow outside of its container
Add overflow property to the container to fix this problem.

- clear-fix::after
Another common used way to clear the float for the container, add empty string after the element to cancel the float.
```
.clearfix::after
{
    content: ""';
    clear: both;
    display: table;
}
```

### display: Inline-block property

display: inline-block display the element as a block inline, meaning width and height properties will work. Where as the display: inline will not.
Difference between display: block and display: inline-block is that inline-block does not add a line-break after the element where as the block will.

- inline-block replace float:
The old way to create a grid of boxes layout is to use, float and clear property.
The new way. inline-block. clear is not needed for inline-block.  


## CSS Combinators
4 combinators
* descendant (space)
* child  (>)    
* adjacent sibling (+)
* general sibling (~)

descendant selector matches all elements that are descendants of an element

```css
// select all <p> elements inside <div>
div  p
{
    .....
}
```

child selector selects all elements that are the immediate children of an element
    
```css
// select all <p> that are immediate children of <div>
div > p
{
    ...
}
```

adjacent sibling selector: select the element just after.
general sibling selector: select all elements that are siblings of an element.


### Pseudo-classes & Pseudo-element
Pseudo-class is used to define a special state of an element

syntax:
```
selector: pseudo-class
{
    property:....
}
```
examples:
* :active
* :enabled
* :hover

pseudo-element if used to style specified parts of an element.
syntax:
```
selector::pseudo-element
{
    .....
}
```
examples:
* ::after
* ::before

## opacity property

opacity take a value from 0.0 - 1.0. The lower, the more transparent.

## Css attribute selector
Its is possible to style elements with specific attributes or attributes values.

The syntax is like using json object:
```css
// styling anchor with target attribute
a[target] // or a[target="value"] to select specific value
{
    ....
}
```

## Css shadow effects
* text-shadow
* box-shadow

specify the horizontal diff and vertical diff and a blur and color
example:
```css
h1
{
    text-shadow: 2px 2px 5px red;
    box-shadow: 2px 2px 2px grey;
}
```
## box-sizing property

can add padding and border to the element's total width and height
* border-box (add padding and border to total width and height)
* content-box (default, width and height only affects content)

## Flex box layout
One dimension table views in either horizontal or vertical way. By using `flex-wrap`, can wrap it to two dimensional.
To use flexbox model, it needs a flex container and its items.

In flex container, setting the display property to flex to enable flex module.
In flex container, the following properties can be configured:
* flex-direction ([row| column])
* flex-wrap       ([wrap| nowrap])
* flex-flow         (shorthand for flex-dirction and flex-wrap)
* justify-content (used to align flex item [center| flex-start| flex-end| stretch])
* align-itmes       (align items vertically)
* align-content     (align echo row of the flex items)

example:
```css
.flex-container
{
    display: flex;
    flex-wrap: wrap;
    align-content: sapce-between;
}
```

For child elements:
* order            (specify the order of flex items)
* flex-grow      (specify how much the item will grow relative to the rest of items)
* flex-shrink     (specify how much the item will shrink)
* flex-basis       (the length of the item)
* flex                 (shorthand for flex-grow, flew-shrink, flex-basis)
* align-self        (alignment of the item [auto | stretch | center | flex-start| flex-end])

## Grid layout module
grid layout consists of a parent element, the container and the child elements.
To use grid module, set the container display property to `grid` or `inline-grid`

### Terminology:
* grid container:  containing a grid, with `display: grid`
* grid item : direct descendant of the grid container
* grid line: horizontal and vertical line separating the grid into sections
* grid cell: intersection between row and column, same as a table cell
* grid track: row tracks or column tracks, strip of cells
* grid area: arear between four grid lines, which covers more than one cell
* grid gap: space between cells, gutters

### How to use
1. Define a grid
2. Place items within the grid

#### Defince a grid

First configure the grid container by defining: 
1. `display: grid`
2. the template for the columns and rows

In defining the columns and rows, we can use `fr` (fraction)  to denote how to divide the whole space.
After defining the grid, the gird will automatically place all items into the grid, from left to right, from top to bottom.

example:
```css
.grid-container
{
    display: grid;
    grid-template-columns: auto auto auto; // also can be 2fr 1fr 1fr
    grid-template-rows: 80px 200px; 
    grid-gap: 10px;
}
```

#### Grid item
For each grid item, we can define how much space it will take.

```css
grid-column: 2/4;
grid-row: 2/3
```
This specific this item to take column from line 2 to line 4, and row from line2 to line3

#### Grid template areas
A way to manage grid areas easily.
`grid-template-areas` defines how each areas block should be placed in the grid.
`grid-area` in items defines which item correspond to which area.


First, declare the template in the grid container:
```css
.site {
    display: grid;
    //defines a 3*2 grid
    grid-template-rows: auto 1fr 3fr; 
    grid-template-columns: 1fr 1fr;
    //define the areas in the grid
    grid-template-areas:
        “title title”
        “main header”
        “main sidebar”;
}

// give each item a corresponding name for area
.masthead {
    grid-area: header;
}
.page-title {
    grid-area: title;
}
.main-content {
    grid-area: main;
}
```

### Test support grid
To test if the browser support the grid layout, in case of IE.
```css
@supports (display: grid)
{
    ...
}
```

### CSS grid RWD(responsive web design) approach for today
1. build accessible mobile-first layout without grid
2. use mobile-first layout as fallback for all browsers.
3. user @supports to detect gird support
4. At the appropriate point, create layout with grid and grid-areas.
5. export new layout as viewport widens

## Css Media Queries
The `@media` rule made it possible to define different style for different media types.

syntax
```css
@media [not| only] mediatype and (expressions)
{
    ...
}
```
