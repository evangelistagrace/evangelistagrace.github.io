---
layout: post
title: "Simple Accordion with JavaScript"
author: "Evangelista Grace"
categories: tutorials
tags: [Tutorial, JavaScript, programming]
image: 23.jpg
---

Here's a very quick tutorial on how to create a toggling accordion.

This is what we will be creating: 

<!-- Codepen preview -->
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="chevvycherokee" data-slug-hash="yLLaaoo" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Simple Accordion">
  <span>See the Pen <a href="https://codepen.io/chevvycherokee/pen/yLLaaoo">
  Simple Accordion</a> by Chevvycherokee (<a href="https://codepen.io/chevvycherokee">@chevvycherokee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

<br>
### The HTML

First, we create a list item.

```html
<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>

```
Then, we add some chevrons on the side of each list item. To use FontAwesome icons, paste the following in side your ```<head>``` tag.

```html
https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.min.css
```

Now, add the chevron icon before each ```</li>``` tag.

```html
<ul>
  <li>One<i class="fas fa-chevron-down"></i></li>
  <li>Two<i class="fas fa-chevron-down"></i></li>
  <li>Three<i class="fas fa-chevron-down"></i></li>
</ul>
```
[you can head to [FontAwesome](https://fontawesome.com/icons) to choose any other icon that you like.]

Next, add ```<span>YOUR TEXT</span>``` under each ```<li>``` item. This will be the content under your tabs.

```html
<ul>
  <li>One<i class="fas fa-chevron-down"></i></li>
  <span>Knock, knock</span>
  <li>Two<i class="fas fa-chevron-down"></i></li>
  <span>Who's there</span>
  <li>Three<i class="fas fa-chevron-down"></i></li>
  <span>Your mum</span>
</ul>
```

And that's it, that's all we need for the HTML. Now let's add some styles and transform this plain list into an accordion.

Preview:
![](/assets/img/simple-accordion/1.jpg)


### The CSS

First, let's remove the default list styling and turn the list items into accordion tabs. 

```css
ul {
  list-style-type: none;
  padding: 0;
  width: 300px;
  text-align: center;
  font-family: Roboto, sans-serif;
  margin: 20px auto;
}
ul li {
  background: #0097A7;
  padding: 10px;
  border: 1px solid white;
  cursor: pointer;
}
```

Preview:
![](/assets/img/simple-accordion/2.jpg)

Next, let's position the chevron to the right and add some hover effect to the accordion tabs.

```css
ul li:hover {
  background: #006064;
  color: white;
}

ul li i {
  float: right;
  padding: 2px 2px 2px 0;
}
```

Preview:
![](/assets/img/simple-accordion/3.jpg)

And finally, we want to hide the accordion content by default and reveal the content when the tab is clicked. 

```css
ul li + span {
  display: none;
  width: inherit;
  background: #eee;
  padding: 20px;
}
```

Preview:
![](/assets/img/simple-accordion/4.jpg)

Now let's add some function to toggle the tab contents when the user clicks.

### The JavaScript

First, let's select the tabs (the list items) and the tab contents (the span items).

```javascript
const li = Array.from(document.querySelectorAll('li'));
const span = Array.from(document.querySelectorAll('span'));
```

Next, we add an event listener to each of the tab to listen for a click event. 

```javascript
li.forEach(tab => {
  tab.addEventListener('click', toggleAccordion);
  tab.toggle = 0;
});
```
Notice here that we added a ```toggle``` attribute to the tab. This value will act as the state of the tab. ```0``` for untoggled state and ```1``` for toggled state.

 ```toggleAccordion```will be our callback function that will fire when there is a click on the tab.

 Let's now define the ```toggleAccordion()```.

 First, we capture the active tab by storing it in a variable called ```li```. Then, we select the tab's content and it's arrow and store it in a variable called ```span``` and ```arrow```.

 ```javascript
 function toggleAccordion(){
  const li = this; //active accordion tab
  const span = li.nextElementSibling; //tab content
  const arrow = li.firstElementChild;

 }
  ```

Now that we have defined our variables, let's add a toggling functionality to the tabs. 

```javascript
if(li.toggle == 0){//closed
    span.style.display = "block"; //show contet
    arrow.classList.remove("fa-chevron-down");
    arrow.classList.add("fa-chevron-up");
    li.toggle = 1;
}else if(li.toggle == 1){ //opened
    span.style.display = "none"; //hide content
    arrow.classList.remove("fa-chevron-up");
    arrow.classList.add("fa-chevron-down");
    li.toggle = 0;
}
```
The idea here is to unhide the tab content on user click when the tab's toggle state is ```0``` (untoggled) and hide the tab content on user click when the tab's toggle state is ```1``` (toggled).

And that's it! Now you have your own accordion tab.

The completed JavaScript:
```javascript
const li = Array.from(document.querySelectorAll('li'));
const span = Array.from(document.querySelectorAll('span'));

li.forEach(tab => {
  tab.addEventListener('click', toggleAccordion);
  tab.toggle = 0;
});
           
function toggleAccordion(){
  const li = this; //active accordion tab
  const span = li.nextElementSibling; //tab content
  const arrow = li.firstElementChild; 
  
  if(li.toggle == 0){//closed
    span.style.display = "block"; //show contet
    arrow.classList.remove("fa-chevron-down");
    arrow.classList.add("fa-chevron-up");
    li.toggle = 1;
  }else if(li.toggle == 1){ //opened
    span.style.display = "none"; //hide content
    arrow.classList.remove("fa-chevron-up");
    arrow.classList.add("fa-chevron-down");
    li.toggle = 0;
  }
}
```

<!-- Codepen preview -->
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="result" data-user="chevvycherokee" data-slug-hash="yLLaaoo" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Simple Accordion">
  <span>See the Pen <a href="https://codepen.io/chevvycherokee/pen/yLLaaoo">
  Simple Accordion</a> by Chevvycherokee (<a href="https://codepen.io/chevvycherokee">@chevvycherokee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Hope you enjoyed this tutorial. Leave a comment or share and stay tuned for more :)
