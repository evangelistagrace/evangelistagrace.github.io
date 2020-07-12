---
layout: post
title: "How to create a draggable and resizable div"
author: "Evangelista Grace"
categories: tutorials
tags: [JavaScript]
image: 31.gif
---

Most of the time you will find dialogs that are able to resize and be dragged. But this feature can also be applied to a shape in an editor or collapsible stacks. Whatever the element you wish to make resizable and/or draggable, it all starts with a simple div. 

### Demo
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="chevvycherokee" data-slug-hash="YzwvmMz" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Draggable and Resizable div with Vanilla JS">
  <span>See the Pen <a href="https://codepen.io/chevvycherokee/pen/YzwvmMz">
  Draggable and Resizable div with Vanilla JS</a> by Chevvycherokee (<a href="https://codepen.io/chevvycherokee">@chevvycherokee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


### The Markup
The div we will be working with will have a few divs nested inside of it - the first div being the responsive content, and the four other divs being the resizers that will enable the div to be resized.

```html
<div class="item">
    <div class="content">
        <h3>Draggable & Resizable</h3>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugiat explicabo minima molestiae excepturi assumenda! Quaerat.
    </div>
    <div class="resizer top-left"></div>
    <div class="resizer top-right"></div>
    <div class="resizer bottom-left"></div>
    <div class="resizer bottom-right"></div>
</div>
```

### The CSS

```css
.item {
  width: 200px;
  height: 200px;
  position: fixed;
  background-color: lightsalmon;
  padding: 20px;
  box-sizing: border-box;
  cursor: move;
}
.item .content {
  height: 100%;
  overflow: auto;
  margin: 0 auto;
  box-sizing: border-box;
  cursor: default;
  user-select: none;
  font-family: Inconsolata, sans-serif;
}
.item .content h3 {
  text-align: center;
  font-family: Merriweather, serif;
}

.resizer {
  position: absolute;
  width: 10px;
  height: 10px;
  background: black;
  z-index: 2;
}
.resizer.top-left {
  top: -1px;
  left: -1px;
  cursor: nw-resize;
}
.resizer.top-right {
  top: -1px;
  right: -1px;
  cursor: ne-resize;
}
.resizer.bottom-left {
  bottom: -1px;
  left: -1px;
  cursor: sw-resize;
}
.resizer.bottom-right {
  bottom: -1px;
  right: -1px;
  cursor: se-resize;
}

```

We will use the class name of each resizer to determine what kind of resizing needs to be done.

Now here comes the fun part.

### The JavaScript

#### Part 1: Making the div draggable

First, we will select the div and set its defaults. We will also add a mousedown eventlistener.

```javascript
let div = document.querySelector('.item'),
    minWidth = 100,
    minHeight = 100,
    maxWidth = 900,
    maxHeight = 900,
    isResizing = false;

div.addEventListener('mousedown', mousedown);
```

Next, we define the mousedown function and fire the mousemove and mouseup events. The idea is to capture the x and y coordinate of the mouse on mousedown and capture the x and y coordinate of the mouse again on mouse move. Once we have these two pairs of values, we can calculate the difference and get the distance and direction of the mouse move to displace the div accordingly.

But before we get to the formula, what we need to know about the page coordinate system is that its origin (0,0) is at the top left of the page. Therefore, at any point in the page, if the mouse moves toward the left, its x value is decreasing. When the mouse moves towards the right of the page, its x value is increasing. Likewise, the mouse's y value decreases when the mouse moves towards the top of the page and increases when the mouse moves towards the bottom of the page.

Having this in mind, if we get the difference in the mouse coordinates by minusing the current mouse coordinates from the previous mouse coordinates, we will get the following:

`newX = previousX - currentX`
`newY = previousY - currentY`

| Direction        | Difference   |
| ------------- |:-------------:|
| ⬅️ left, ⬆️ top      | positive | 
| ➡️ right, ⬇️ down     | negative     | 

_Tip: Direction is positive when moving toward the origin (top-left of the page) and vice-versa._

```javascript
function mousedown(e) {
    //get the initial mouse corrdinates and the position coordinates of the element
    let prevX = e.clientX,
        prevY = e.clientY,
        rect = div.getBoundingClientRect(),
        prevLeft = rect.left,
        prevTop  = rect.top;

    if (!isResizing && !e.target.classList.contains('content')) {
        window.addEventListener('mousemove', mousemove);
        window.addEventListener('mouseup', mouseup);
    }

    function mousemove(e) {
        //get horizontal and vertical distance of the mouse move
        let newX = prevX - e.clientX, //negative to the right, positive to the left
            newY = prevY - e.clientY; //negative to the bottom, positive to the top

        //set coordinates of the element to move it to its new position
        div.style.left = prevLeft - newX + 'px';
        div.style.top = prevTop - newY + 'px';
    }

    function mouseup() {}
}
```
Note that we check if `isResizing` is `false` and only then we allow mousemove and mouseup for drag to fire. Otherwise, the div will start getting dragged around while being resized! 

To move the div, we use getBoundingClientRect() to get its original x coordinate (left value) and y coordinate (top value). Then we minus these values by the difference in the x and y coordinates of the mouse move.

When we lift a click, the mousemove event should be off and the div should no longer be moving. For this, we define the mouseup callback function.

```javascript
function mouseup() {
    window.removeEventListener('mousemove', mousemove);
    window.removeEventListener('mouseup', mouseup);
}
```
Note that we add the mousemove and mouseup eventlisteners to the window because sometimes the cursor can get out of the div while moving.

And that's it for draggable div! Are you up for more fun stuff? Yeah? Let's now make the div resizable.

#### Part 2: Making the div resizable

The resizable function works very much like the draggable function. It will have mousedown, mousemove and mouseup events. However, to resize, we not only displace the div, but we will also need to change the width and height of the div. 

Let's first add a mousedown event to the resizers and define the the mousedown callback function.

```javascript
//select the resizers
let resizers = document.querySelectorAll('.resizer');

resizers.forEach(function(resizer) {
    resizer.addEventListener('mousedown', mousedown);

    function mousedown(e) {
        let prevX = e.clientX,
            prevY = e.clientY,
            currentResizer = e.target,
            rect = div.getBoundingClientRect(),
            prevLeft = rect.left,
            prevTop  = rect.top,
            newWidth,
            newHeight;
        
        isResizing = true;
        
        window.addEventListener('mousemove', mousemove);
        window.addEventListener('mouseup', mouseup);

        function mousemove(e) {}

        function mouseup() {}
    }
}
```

Using the formula from the draggable function, we know that any mouse movement toward the origin (top-left of the page) will result in a positive difference and a negative difference when moving away from the origin. 

Hence, when dragging a resize handler toward the left, we will simply need to add the width with the difference.

`newWidth = previousWidth + newX`

When dragging a resize handler toward the bottom, we minus the height with the difference in order to increase the height (-(-) = +).

`newHeight = previouseHeight - newY`

You get the pattern.

Let's now define the mousemove callback function.

```javascript
function mousemove(e){
    let newX = prevX - e.clientX, //negative to the right, positive to the left
        newY = prevY - e.clientY; //negative to the bottom, positive to the top
    if (currentResizer.classList.contains('bottom-right')) {
        newWidth = rect.width - newX;
        newHeight = rect.height - newY;
        if (newWidth > minWidth && newWidth < maxWidth) {
            div.style.width = newWidth + 'px';
        }
        if (newHeight > minHeight && newHeight < maxHeight) {
            div.style.height = newHeight + 'px';
        }
        
    }
    else if (currentResizer.classList.contains('bottom-left')) {
        newWidth = rect.width + newX;
        newHeight = rect.height - newY;

        if (newWidth > minWidth && newWidth < maxWidth) {
            div.style.left = prevLeft - newX + 'px';
            div.style.width = newWidth + 'px';
        } 
        if (newHeight > minHeight && newHeight < maxHeight) {
            div.style.height = newHeight + 'px';
        }
    }
    else if (currentResizer.classList.contains('top-right')) {
        newWidth = rect.width - newX;
        newHeight = rect.height + newY;
        
        if (newWidth > minWidth && newWidth < maxWidth) {
            div.style.width = newWidth + 'px';
        }
        if (newHeight > minHeight && newHeight < maxHeight) {
            div.style.top = prevTop - newY + 'px';
            div.style.height = newHeight + 'px';
        }
        
    }
    else if (currentResizer.classList.contains('top-left')) {
        newWidth = rect.width + newX;
        newHeight = rect.height + newY;
        
        if (newWidth > minWidth && newWidth < maxWidth) {
            div.style.left = prevLeft - newX + 'px';
            div.style.width = newWidth + 'px';
        }
        if (newHeight > minHeight && newHeight < maxHeight) {
            div.style.top = prevTop - newY + 'px';
            div.style.height = newHeight + 'px';
        }
    }
}
```

Phew, we've made it this far. Now there's only one more thing. We need to turn of the mousemove and mouseup events on mouseup.

```javascript
function mouseup() {
    isResizing = false;
    window.removeEventListener('mousemove', mousemove);
    window.removeEventListener('mouseup', mouseup);
}
```

That's it! You now have yourself a draggable and resizable div. Don't worry if you didn't get all the math the first time. Take some time to digest it and you'll start to see a pattern that you can remember.

Cheers!