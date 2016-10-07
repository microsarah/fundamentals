**WDI Fundamentals Unit 10**

---

#Accessing the DOM

#### A Closer Look at the DOM

We mentioned before that when a browser retrieves the HTML for a page it makes a model of that page in memory. This model is called the DOM.

![](/assets/chapter5/dom.png)

The DOM is a *language independent* model of the page. It is not a part of HTML or JavaScript, but follows a separate set of rules.

The difference between HTML and the DOM can often be a little hard to grasp for those just starting out in programming. An HTML page is simply a text document written in a language the browser understands. It uses opening and closing tags to provide structure and give meaning to the content of the page.

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>To Do List</title>
	<link rel="stylesheet" href="style.css">
</head>
<body>
	<h1>Things To Do</h1>
	<ul>
		<li>Call Mom</li>
		<li>Take out the trash</li>
		<li>Return library books</li>
	</ul>
</body>
</html>
```

The browser retrieves this document and creates a model of the page in memory. Most modern browsers come with tools that developers can use to take a look at the DOM structure.

In Chrome, you can go to *View* > *Developer* > *Developer Tools* and click on the *Elements* panel to take a look at the DOM tree.

![](/assets/chapter5/developer.png)

![](/assets/chapter5/thingstodo.png)

You might notice that, at this point, the DOM closely resembles our original HTML document. However, this is a *living* model of the page, made up of node objects that can be manipulated with JavaScript.

Maybe we want to use JavaScript to add a fourth list item to the page – "Feed the Cat." Then perhaps we want to change the background-color of the first list item to yellow using JavaScript. And lastly, maybe we want to change the text content of the third list item to "Return library books - DONE!"

Now the DOM is looking pretty different from our original HTML file.

![](/assets/chapter5/thingstodo2.png)

JavaScript comes with some "built-in" functions that will allow us to access and update these nodes. We can call on these functions using dot notation. These functions are no different than the functions we just covered, but they are more commonly known as **methods**.

Let's take a look at some of the methods we have available to us!

##Accessing Elements

First things first! Before we can work with an element, we first need to find, or *select* the DOM node that represents the element that we want to work with.

In order to find an element, we need to search through the document for that element. The syntax for searching for an element looks something like this:

```js
document.getElementById('main')
```

* `document` - Refers to the document object. Any time we want to find an element or elements, we'll need to access them through the document object. This will allow us to search the entire page for an element.

* `.` - The dot ties the method on the right hand side (`getElementById`) with the object on the left hand side (`document`).

* `getElementById()` - This is the method we want to use to find an element. We'll take a look at the methods that are available to us shortly. This method in particular lets us locate an element by the value of its `id` attribute.

* `'main'` - Just like with the functions we've learned about earlier in this unit, we can pass in parameters for these methods to use. In this case, we want to find an element that has an `id` of `main`.

> **NOTE** Note that using proper syntax here is important! These methods are case sensitive. `getElementByID` (with a capital D) will throw an error and will not work.


###Selecting an Individual Element

There are a couple of methods we can use to find an individual element.



####`getElementById()`

The fastest route to finding any single element is `getElementById()`. Since `id`s are unique, meaning two elements cannot have the same value for an `id` attribute in any given HTML page, this query will allow us to quickly find an individual element.

HTML:

```html
<div id="sidebar">
	<h1>Related Articles</h1>
	<ul>
		<li>Article One</li>
		<li class="special">Article Two</li>
		<li>Article Three</li>
	</ul>
</div>
```


JavaScript:

```js
var sidebar = document.getElementById('sidebar');
```
This will find the element that has an `id` of `sidebar` and save it to a variable.

> **NOTE** If we'd like to work with that element multiple times, a variable should be used to store, or **cache**, the results of our query. When we store an element in a variable, what we are doing behind the scenes is storing a reference to the location of the element in the DOM tree. We can then use any methods we would normally use on an element on that variable.

---

####`querySelector()`

This method allows us to use our CSS selector syntax to find an element. If there are multiple elements on the page that match the selector, it will return the *first* of the matching elements. It is important to note that this a recent addition to the DOM and is not supported by older browsers.

```js
document.querySelector('.special')
```

This will return the first element on the page that has a `class` of `special`. You can use any of your CSS-style selectors as a parameter.

Other examples:

```js
document.querySelector('#sidebar') // This will find the element that has an id of sidebar.
document.querySelector('ul > li') // This will find the first li that is a direct child of the ul.
```


###Selecting Multiple Elements

Sometimes we'll want to find and work with several elements at once. There are several methods we can use that will return a NodeList, or list of node objects, we can work with.

For each example below, you can use this HTML snippet as a reference:

```html
<div id="sidebar">
	<h1>Days of the Week</h1>
	<ul>
		<li>Monday</li>
		<li class="special">Tuesday</li>
		<li class="special">Wednesday</li>
		<li>Thursday</li>
		<li>Friday</li>
		<li>Saturday</li>
		<li>Sunday</li>
	</ul>
</div>
```

___

####`document.getElementsByClassName()`

This will select all elements that have a given class attribute.

```js
document.getElementsByClassName('special')
```
This will return any elements that have the class `special`. In the above example, this will return a NodeList containing the second and third list items, since they both have the class `special`.

---


####`document.getElementsByTagName()`

This method will locate all elements that match a given tag name.

```js
document.getElementsByTagName('li')
```
Here, this query will return all `<li>` elements. In the case, the NodeList will contain all seven `<li>`.

___



####`document.querySelectorAll()`

Similar to our `querySelector()` method, this method lets us use our CSS selector syntax to select one or more elements.

```js
document.querySelectorAll('.special')
```
This will return any elements that have the class `special`. In the above example, this will return a NodeList containing the second and third list items, since they both have the class `special`.

___

####Working with NodeLists

Any time there is even the *potential* for a method to return more than one element, such as with `getElementsByClassName()`, `getElementsByTagName()`, and `querySelectorAll`, a NodeList will be returned, *even if only one element is found that matches the query*.

These NodeLists are **collections** that are numbered similar to the arrays we will be looking at in Unit 11.

When we get this list, we can select a single item from that list to work with using array syntax – a set of square brackets.

For example, above we saw that `document.getElementsByTagName('li')` returned seven list items. The NodeList that was returned would look like this:

|  Index      |  Element  |
|:-------:    |:-------:|
| 0      | `<li>Monday</li>` |
| 1     | `<li class="special">Tuesday</li> `  |
| 2   | `<li class="special">Wednesday</li>`    |
| 3      | `<li>Thursday</li>`   |
| 4     | `<li>Friday</li>`   |
| 5   | `<li>Saturday</li>`  |
| 6      | `<li>Sunday</li>`  |

Note how each node has an index associated with it. These indexes are *zero based* – meaning the first node has an index of 0, the second node an index of 1, etc.


To locate the fourth item in that NodeList, `<li>Thursday</li>`, we could use the following syntax:

```js
document.getElementsByTagName('li')[3].className = 'special'
```
Directly after the `getElementsByTagName('li')` we have the index number of the item we want to locate within square brackets, `[3]`, which would locate the item at index 3.

This would find the fourth list item and change the class attribute to `special` (we'll take a look at the className method shortly).

We could also use a loop to iterate through each element in the NodeList and do something with each item.

For example:

```js

var listItems = document.getElementsByTagName('li');

for (var i = 0; i < listItems.length; i++) {
	listItems[i].className = 'day';
}

```
This would loop through the NodeList and change the class name for each item to `day`.

##Traversing the DOM

Once we've selected an individual element, we can then either do something with this element or select another element based on its relationship to it. You'll often hear this referred to as **traversing the DOM**.

In an HTML document, elements can be nested inside of other elements.
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>To Do List</title>
</head>
<body>
	<h1>Things To Do</h1>
	<ul>
		<li>Call Mom</li>
		<li>Take out the trash</li>
		<li>Return library books</li>
	</ul>
</body>
</html>
```

Here we have three `<li>` elements that are enclosed within an opening `<ul>` tag and closing `</ul>` tag.

In programming, relationships between the document and elements are often described in similar terms as one would use to describe a family tree.

Here we could describe the relationship between the `<ul>` element and the three `<li>` elements as that of *parent* and *children* since the `<li>` elements are enclosed within the `<ul>`.

JavaScript has methods we can use to find an element based on an initial selection. For example:

####`parentNode()`

This method will locate the parent of an initial selection.

```js
document.getElementsByTagName('li')[0].parentNode()
```
THis will return the parent of the first `<li>` element, which, in this case, is the `<ul>` element since it wraps all the `<li` elements.

Other methods available to us include:
* `previousSibling()` – This will find the previous sibling of a selected element.
* `nextSibling()` – This will find the next sibling of a selected element.
* `firstChild()` – This will find the first child of a selected element.
* `lastChild()` – This will find the last child of a selected element.


##Manipulating the DOM

This is where the fun starts! Now that we've done all that hard work finding elements to work with, we can actually do something with them! The possibilities are endless.

In the following examples, we'll be using this HTML page as a reference:

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>To Do List</title>
	<link rel="stylesheet" href="style.css">
</head>
<body>
	<h1>Things To Do</h1>
	<ul>
		<li>Call Mom</li>
		<li>Take out the trash</li>
		<li id="important">Return library books</li>
	</ul>
</body>
</html>
```

Which will look like this when it is first loaded into the browser:

![](/assets/elkwebdesign/example_lesson_08.png)

### Accessing and Updating Content

There are lots of properties and methods that allow us to read from or update the contents of a DOM node. Let's take a look at a couple now and then we'll expand upon these in the next lesson.

####**`innerHTML`**

We can use the `innerHTML` property to get and set content for an element.

For example, if we want to change the HTML content for the first `<li>`, we could execute the following:

```js
document.getElementsByTagName('li')[0].innerHTML = 'Email <a href="mom@gmail.com">Mom</a>.';
```
This would find the first `<li>` and change the HTML content to `Email <a href="mom@gmail.com">Mom</a>.` The result will look like this:

![](/assets/elkwebdesign/innerHTML.png)

If we simply want to retrieve the HTML content to use later, we can grab the HTML content and save it in a variable like so:

```js
var firstListItem = document.getElementsByTagName('li')[0].innerHTML;
```

####**`textContent`**

This property allows us to get and set the *text* content for an element.
For example:
```js
document.getElementById('important').textContent = 'Done!'
```

This code would change the text content of the `<li>` that has the id `important` to 'Done!'.

![](/assets/chapter5/thingstodo5.png)

---

[On to the next lesson.](10_lesson.md)
