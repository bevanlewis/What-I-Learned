# JavaScript DOM

The Document Object Model (DOM) is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style, and content.

## Selecting Elements

To select an element, use the `document.querySelector()` method.

```javascript
let element = document.querySelector("h1");
```

for id use #

```javascript
let element = document.querySelector("#my-id");
```

for class use .

```javascript
let element = document.querySelector(".my-class");
```

### Selecting Multiple Elements

To select multiple elements, use the `document.querySelectorAll()` method.

```javascript
let elements = document.querySelectorAll("p");
```

## Changing Styles

To change the style of an element, use the `element.style` property.

```javascript
element.style.color = "red";
```

## Changing Content

To change the content of an element, use the `element.textContent` property.

```javascript
element.textContent = "Hello, world!";
```

## Adding and Removing Elements

To add an element, use the `document.createElement()` method.

```javascript
let newElement = document.createElement("p");
newElement.textContent = "This is a new paragraph.";
document.body.appendChild(newElement);
```

### Removing Elements

To remove an element, use the `element.remove()` method.

```javascript
element.remove();
```

## Inner Methods

### innerHTML

The `innerHTML` property is used to get or set the HTML content of an element.

```javascript
let element = document.querySelector("p");
element.innerHTML = "<a>This is a new link.</a>";
// The link will be added to the paragraph.
```

### innerText

The `innerText` property is used to get or set the text content of an element.

```javascript
let element = document.querySelector("p");
console.log(element.innerText);
element.innerText = "This is a new paragraph.";
```

## Manipulating Attributes

To manipulate attributes, use the `element.getAttribute()` and `element.setAttribute()` methods.

```javascript
let element = document.querySelector("a");
console.log(element.getAttribute("href")); // return the href attribute
element.setAttribute("href", "https://www.example.com");
```

## ClassList

The `classList` property is used to manipulate the class attribute of an element.

```javascript
let element = document.querySelector("div");
element.classList.add("my-class");
element.classList.remove("my-class");
element.classList.toggle("my-class");
```

## Event Listeners

To add an event listener, use the `element.addEventListener()` method.

```javascript
let element = document.querySelector("button");
element.addEventListener("click", function () {
  console.log("The button was clicked!");
});
```

## Prevent Default

To prevent the default action of an event, use the `event.preventDefault()` method.

```javascript
let link = document.querySelector("a");
link.addEventListener("click", function (event) {
  event.preventDefault();
  console.log("The link was clicked.");
};
```

## Stop Propogation

To stop an event from propogating (prevent event bubbling), use the `event.stopPropagation()` method.

```javascript
let div = document.querySelector("div");
div.addEventListener("click", function (event) {
  event.stopPropagation();
  console.log("The div was clicked.");
}
```
