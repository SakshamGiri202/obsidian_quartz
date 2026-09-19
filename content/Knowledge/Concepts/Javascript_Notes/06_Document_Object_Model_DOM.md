# Document Object Model (DOM)

## Overview
1. What is the DOM
2. Structure of the DOM (Tree)
3. Requirement / Working of DOM
4. Types of DOM
5. Properties of DOM
6. The Document, Window (BOM), Objects & Interfaces
7. Accessing data from DOM
8. DOM Data Types
9. Selecting DOM Elements
10. Modifying DOM (Content, Styles, Attributes)
11. Event Handling (addEventListener)
12. Custom Events
13. Common Misunderstandings About the DOM
14. Common Interview Questions

## Key Concepts

### What is the DOM?
- **DOM** = **Document Object Model** — a structured, tree-like representation of a web page that lets JavaScript access, modify, and control the page's content and structure.
- Powers most dynamic website interactions: real-time updates, form validation, interactive UIs.
- Each HTML tag becomes a **node** in the hierarchy.
- `<html>` is the root element, containing `<head>` and `<body>` as children.
- Those children have their own children: `<title>`, `<div>`, `<h1>`, `<p>`, `<ul>`, `<li>`, etc.
- **Container elements** hold other elements; elements that hold nothing are simply **child elements**.
- The hierarchy allows traversal parent → child and child → parent.

### Structure of the HTML DOM (The Tree)
- The **document** is the root.
- HTML tags like `<html>`, `<head>`, `<body>` are the branches.
- Attributes, text, and other elements are the **leaves**

```
document (root)
└── <html>
    ├── <head>
    │   ├── <title>Page Title</title>
    │   └── <meta>
    └── <body>
        ├── <h1>Heading</h1>
        ├── <p>Paragraph</p>
        └── <ul>
            ├── <li>Item 1</li>
            └── <li>Item 2</li>
        </ul>
```

### Requirement of the DOM
- **Dynamic Content Updates**: update content without reloading the page (form validation, AJAX responses).
- **User Interaction**: makes a webpage interactive (respond to clicks, form submissions).
- **Flexibility**: add, modify, or remove elements and styles in real-time.
- **Cross-Platform Compatibility**: standard way for scripts to interact with documents across browsers.

### Working of the DOM
The DOM connects the webpage to JavaScript, allowing you to:
- **Access** elements (find an `<h1>` tag)
- **Modify** content (change text of a `<p>` tag)
- **React** to events (button click)
- **Create or remove** elements dynamically

### Types of DOM
| Type     | Description                                                                 |
| -------- | --------------------------------------------------------------------------- |
| Core DOM | Standard model for all document types; all implementations must support it  |
| XML DOM  | Standard model for XML documents; all XML elements accessed through it      |
| HTML DOM | Standard model for HTML documents; how to get, change, add, delete elements |
|          |                                                                             |
|          |                                                                             |
|          |                                                                             |
|          |                                                                             |

### Properties of the DOM
- **Node-Based**: everything is a node (element nodes, text nodes, attribute nodes).
- **Hierarchical**: parent-child relationships forming a tree.
- **Live**: DOM changes made with JS are immediately reflected on the page.
- **Platform-Independent**: works across platforms, browsers, and languages.

### The Document
- The **Document interface** represents a web page loaded in the browser and is the main entry point to its content.
- The DOM tree consists of nodes like `<body>`, `<table>`, and other HTML elements.
- Provides global access to page info such as the URL.
- Enables creating, modifying, and managing elements.
- HTML documents served as `text/html` also implement the `HTMLDocument` interface; XML/SVG documents implement the `XMLDocument` interface.

### The Window (BOM)
- **BOM** = **Browser Object Model** — lets JavaScript "talk to" the browser. No official standard, but browsers implement near-identical methods/properties.
- The `window` object represents the browser window.
- All global JS objects, functions, and variables automatically become members of `window`.
- Even the `document` object (HTML DOM) is a property of `window`:

```javascript
window.document.getElementById("header");
```

### The Object
- All properties, methods, and events for manipulating web pages are organized into objects inside the DOM (e.g., `document`, `table` implementing `HTMLTableElement`, etc.).
- The modern DOM is built using multiple APIs working together.
- The core DOM defines fundamental objects; HTML DOM API adds HTML-document support on top.

### The Interface
- An **interface** is a blueprint defining the properties/methods available for a type of element.
- Example: `<table>` uses the `HTMLTableElement` interface, giving table-specific methods like `insertRow()`.
- Inheritance chain (general → specific):

```
EventTarget         → base interface; allows listening/responding to events
  └── Node          → any point in the document tree (element, text, comment)
       ├── Document → entry point to the whole DOM tree (the page)
       └── Element
            └── HTMLElement       → any HTML element (<div>, <p>)
                 └── HTMLTableElement → table-specific methods (insertRow())
```

### Accessing Data from the DOM
JavaScript uses the DOM to access the document and elements via the `document`/`window` API.

```javascript
// Standard DOM spec: returns all <p> elements
const paragraphs = document.getElementsByTagName("p");
// paragraphs[0] is the first <p> element
```

**Access by:**
- **id**: `var x = document.getElementById("intro");` (single element)
- **class name**: `var x = document.getElementsByClassName("intro");`
- **CSS selectors**: `var x = document.querySelectorAll("p.intro");`
- **HTML object collections**:

```javascript
document.anchors.length;
document.forms.length;
document.images.length;
document.links.length;
document.scripts.length;
```

### DOM Data Types
| Type     | Everyday meaning                                                                          |
| -------- | ----------------------------------------------------------------------------------------- |
| Document | The entire webpage; the starting point / big box holding everything                       |
| Node     | Anything on the page (heading, paragraph, image, hidden parts); a piece of the whole page |
| Element  | A specific type of node representing actual HTML tags: `<div>`, `<p>`, `<img>`            |
| NodeList | A list/collection of nodes, e.g., all buttons or paragraphs returned at once              |

### Commonly Used DOM Methods
| Method                       | Description                                             |
| ---------------------------- | ------------------------------------------------------- |
| `getElementById(id)`         | Selects an element by its unique ID                     |
| `getElementsByClassName(cl)` | Retrieves all elements sharing a given class name       |
| `getElementsByTagName(tag)`  | Retrieves all elements with a given tag name            |
| `querySelector(selector)`    | Returns the FIRST element matching a CSS selector       |
| `querySelectorAll(selector)` | Selects ALL elements matching a CSS selector (NodeList) |
| `createElement(tag)`         | Creates a new HTML element dynamically                  |
| `appendChild(node)`          | Adds a child node to an existing element                |
| `remove()`                   | Removes an element from the DOM                         |
| `addEventListener(evt, fn)`  | Attaches an event handler function to an element        |

### Common DOM Properties
| Property                                                       | Description                                         |
| -------------------------------------------------------------- | --------------------------------------------------- |
| `innerHTML`                                                    | Gets/sets the HTML content inside an element        |
| `innerText`                                                    | Gets/sets the visible text content of an element    |
| `textContent`                                                  | Gets/sets all text content (ignores styling)        |
| `style`                                                        | Inline styles (e.g., `element.style.color = "red"`) |
| `value`                                                        | Current value of an input/form field                |
| `className` / `classList`                                      | Manage CSS classes                                  |
| `id`                                                           | Element's id attribute                              |
| `attributes`                                                   | Collection of an element's attributes               |
| `parentNode` / `childNodes` / `children`                       | DOM traversal                                       |
| `firstChild` / `lastChild` / `nextSibling` / `previousSibling` | Sibling traversal                                   |

### Selecting DOM Elements (Detailed)

**1. getElementById()** — single element by unique ID
```javascript
document.getElementById('id')
```
```html
<h1 id="gfg">GeeksForGeeks</h1>
<script>
  const element = document.getElementById('gfg');
  element.style.color = "green";
  element.style.textAlign = "center";
  element.style.margin = "30px";
  element.style.fontSize = "30px";
</script>
```

**2. getElementsByClassName()** — HTMLCollection of all elements with the class
```javascript
document.getElementsByClassName('class')
```
```html
<h1 class="selector">GeeksForGeeks</h1>
<h2 class="selector">DOM selector in JavaScript</h2>
<script>
  const elements = document.getElementsByClassName('selector');
  elements[0].style.color = "green";
  elements[1].style.color = "red";
  elements[0].style.textAlign = "center";
  elements[1].style.textAlign = "center";
  elements[0].style.marginTop = "60px";
</script>
```

**3. getElementsByTagName()** — HTMLCollection of all elements with the tag
```javascript
document.getElementsByTagName('tag')
```
```html
<p>Thanks for visiting GFG</p>
<p>This is showing how to select DOM element using tag name</p>
<script>
  const paragraphs = document.getElementsByTagName('p');
  paragraphs[0].style.color = "green";
  paragraphs[1].style.color = "blue";
  paragraphs[0].style.fontSize = "25px";
  paragraphs[0].style.textAlign = "center";
  paragraphs[1].style.textAlign = "center";
  paragraphs[0].style.marginTop = "60px";
</script>
```

**4. querySelector()** — FIRST element matching a CSS selector (returns one)
```javascript
document.querySelector('selector')
```
```html
<div class="gfg">GeeksForGeeks</div>
<script>
  const element = document.querySelector('.gfg');
  element.style.color = "green";
  element.style.textAlign = "center";
  element.style.margin = "30px";
  element.style.fontSize = "30px";
</script>
```

**5. querySelectorAll()** — NodeList of ALL elements matching a CSS selector
```javascript
document.querySelectorAll('selector')
```
```html
<h1 class="selector">GeeksForGeeks</h1>
<p class="selector">This is showing how to select DOM element using tag name</p>
<script>
  const elements = document.querySelectorAll('.selector');
  elements[0].style.color = "green";
  elements[1].style.color = "red";
  elements[0].style.textAlign = "center";
  elements[1].style.textAlign = "center";
  elements[0].style.marginTop = "60px";
</script>
```

**Selector comparison table:**


| Method                   | Return type    | Selects               | Static/Live |
| ------------------------ | -------------- | --------------------- | ----------- |
| `getElementById`         | Element/null   | First by ID           | —           |
| `getElementsByClassName` | HTMLCollection | All by class          | **Live**    |
| `getElementsByTagName`   | HTMLCollection | All by tag            | **Live**    |
| `querySelector`          | Element/null   | First by CSS selector | Static      |
| `querySelectorAll`       | NodeList       | All by CSS selector   | Static      |

### Modifying DOM Elements (Content & Styles)
```html
<html>
<body>
  <h2>GeeksforGeeks</h2>
  <p id="intro">A Computer Science portal for geeks.</p>
  <p>This example illustrates the <b>getElementById</b> method.</p>
  <p id="demo"></p>
  <script>
    const element = document.getElementById("intro");
    document.getElementById("demo").innerHTML =
      "GeeksforGeeks introduction is: " + element.innerHTML;
  </script>
</body>
</html>
```

### Creating and Removing Elements Dynamically
```javascript
// Create a new element
const newDiv = document.createElement("div");
newDiv.innerHTML = "Hello, I am new!";
newDiv.style.color = "blue";

// Attach it to the page
document.body.appendChild(newDiv);

// Remove an element
const old = document.getElementById("old");
old.remove();
```

### Full Working Example: DOM + Input Values + Styles
```html
<html>
<head></head>
<body>
  <label>Enter Value 1: </label>
  <input type="text" id="val1" />
  <br /><br />
  <label>Enter Value 2: </label>
  <input type="text" id="val2" />
  <br />
  <button onclick="getAdd()">Click To Add</button>
  <p id="result"></p>

  <script type="text/javascript">
    function getAdd() {
      // Fetch the value of input with id val1
      const num1 = Number(document.getElementById("val1").value);
      // Fetch the value of input with id val2
      const num2 = Number(document.getElementById("val2").value);

      const add = num1 + num2;
      console.log(add);

      // Displays the result in paragraph using DOM
      document.getElementById("result").innerHTML = "Addition : " + add;
      // Changes the color of paragraph tag with red
      document.getElementById("result").style.color = "red";
    }
  </script>
</body>
</html>
```

### HTML Table Structure Through the DOM
The table element contains `<tr>` (rows), which contain `<td>` (cells), each cell holding content.
```html
<table>
  <tr>
    <td>Car</td>
    <td>Scooter</td>
  </tr>
  <tr>
    <td>MotorBike</td>
    <td>Bus</td>
  </tr>
</table>
```

### Event Handling with addEventListener()

**Syntax:**
```javascript
element.addEventListener(event, function, useCapture);
```
- **element**: the DOM element to listen on (e.g., `document`, `button`, `div`)
- **event**: type of event — `'click'`, `'keydown'`, `'submit'`, etc.
- **function**: the function to run when the event fires (anonymous or named reference)
- **useCapture** (optional): boolean — `true` uses event capturing, `false` (default) uses bubbling

**How it works:**
1. Choose an element to attach the event to.
2. Specify the event type (`click`, `keyup`, `hover`, etc.).
3. Define the action (function to run).
4. The listener continuously watches for the event; when it happens, the function executes.

**Basic example:**
```html
<html>
<head>
  <title>JavaScript addEventListener</title>
</head>
<body>
  <button id="try">Click here</button>
  <h1 id="text"></h1>

  <script>
    document.getElementById("try").addEventListener("click", function () {
      document.getElementById("text").innerText = "GeeksforGeeks";
    });
  </script>
</body>
</html>
```

**Multiple event listeners on the same element:**
```html
<html>
<head>
  <title>Multiple Event Listeners Example</title>
</head>
<body>
  <div>
    <button id="myButton">Click me!</button>
    <h1 id="message"></h1>
  </div>

  <script>
    const button = document.getElementById("myButton");
    const message = document.getElementById("message");

    button.addEventListener("click", function () {
      button.style.backgroundColor = "lightblue";
      message.innerText = "Button was clicked!";
    });

    button.addEventListener("mouseenter", function () {
      message.innerText = "Mouse is over the button!";
    });

    button.addEventListener("mouseleave", function () {
      message.innerText = "Mouse left the button!";
    });

    document.addEventListener("keydown", function (event) {
      if (event.key === "Enter") {
        message.style.color = "green";
        message.innerText = "Enter key pressed!";
      }
    });
  </script>
</body>
</html>
```

**Adding event handlers to the `window` object:**
```javascript
const message = document.getElementById('message');
const keyPressDisplay = document.getElementById('keyPressDisplay');

// Window Resize Event
window.addEventListener('resize', function () {
  const width = window.innerWidth;
  const height = window.innerHeight;
  message.innerText = `Window resized! New dimensions: ${width}x${height}`;
  message.style.fontSize = (width / 50) + 'px';
});

// Scroll Event
window.addEventListener('scroll', function () {
  const scrollY = window.scrollY;
  document.body.style.backgroundColor = `rgb(${scrollY % 255}, ${255 - (scrollY % 255)}, 150)`;
  message.innerText = `You have scrolled! Scroll position: ${scrollY}px`;
});

// Keydown Event
window.addEventListener('keydown', function (event) {
  keyPressDisplay.innerText = `You pressed the "${event.key}" key!`;
  if (event.key === 'Enter') {
    keyPressDisplay.style.color = 'green';
  } else if (event.key === 'Escape') {
    keyPressDisplay.style.color = 'red';
  } else {
    keyPressDisplay.style.color = '#333';
  }
});
```

**Reasons to use addEventListener():**
- Cleaner code — keeps event handling separate from HTML
- Multiple listeners on the same element
- Control over event propagation (bubbling vs capturing)
- Can remove listeners with `removeEventListener()`
- Cross-browser consistent behavior
- Better performance than inline event handlers
- Access to the event object (mouse position, pressed key, etc.)

**Removing a listener:**
```javascript
function handler() {
  console.log("Clicked!");
}
button.addEventListener("click", handler);
button.removeEventListener("click", handler); // listener no longer fires
```

### Custom Events
- **Custom events** are developer-defined events used to perform specific actions.
- Allow different parts of an application to communicate without tight coupling.

**How to create and trigger custom events:**
1. **Create an Event** using the `Event` / `CustomEvent` constructor.
2. **Add an Event Listener** on an element or `document`.
3. **Dispatch the Event** using `dispatchEvent()`.

**Syntax:**
```javascript
const eventName = new Event('eventName');

const eventWithData = new CustomEvent('eventWithData', {
  detail: {
    key: 'value'
  }
});
```

**Example (HTML + JS files):**
```html
<button id="startButton">Start Process</button>
<script src="app.js"></script>
```
```javascript
const startProcessEvent = new Event('startProcess');

document.addEventListener('startProcess', () => {
  console.log('Custom event "startProcess" has been triggered!');
  alert('Process Started!');
});

const startButton = document.getElementById('startButton');
startButton.addEventListener('click', () => {
  console.log('Button clicked. Triggering custom event...');
  document.dispatchEvent(startProcessEvent);
});
```

**Condition-based event dispatching** (fire only when a condition is met):
```html
<button id="clickButton">Click Me</button>
<script>
  const customEvent = new Event('customEventTriggered');

  document.addEventListener('customEventTriggered', () => {
    alert('Custom event triggered after 5 clicks!');
    console.log('Event triggered after 5 clicks!');
  });

  let clickCount = 0;
  const button = document.getElementById('clickButton');

  button.addEventListener('click', () => {
    clickCount++;

    if (clickCount === 5) {
      document.dispatchEvent(customEvent);
      clickCount = 0;
    }
    console.log(`Button clicked ${clickCount} times`);
  });
</script>
```

**Why use custom events:**
- **Modularity**: separate parts of the app → easier to maintain
- **Flexibility**: define your own events for behaviors standard events don't cover
- **Communication**: loosely-coupled communication between components/modules

**Real-world use cases:**
- Modular applications (components talk without tight coupling)
- Form validation (trigger validation on input value changes)
- Animations/transitions (one animation triggers another)
- Inter-component communication in frameworks (React, Vue)

### Common Misunderstandings About the DOM
- The DOM is **not** a binary description — it does not define binary source code in its interfaces.
- It does **not** describe objects *in* XML/HTML — it **describes XML and HTML documents as objects**.
- It is **not** represented by a set of data structures — it is an **interface** that specifies object representation.
- It does **not** show the criticality of objects in documents (it has no info about which object is contextually appropriate).

## Summary
- The DOM is a tree-structured interface that represents an HTML/XML document as objects (nodes).
- Everything (elements, attributes, text) is a node; the document is the root.
- JS accesses the page via `document` (part of `window`/BOM) using selectors: `getElementById`, `getElementsByClassName`, `getElementsByTagName`, `querySelector`, `querySelectorAll`.
- DOM is **live**: changes reflect immediately.
- `addEventListener(event, fn, useCapture)` is the modern way to handle events (multiple listeners, removeable, propagation control).
- Custom events via `new Event()` / `new CustomEvent()` + `dispatchEvent()` enable decoupled, modular communication.
- Key interfaces: `EventTarget` → `Node` → `Document` / `Element` → `HTMLElement` → specific ones like `HTMLTableElement`.

## Resources
- [GeeksforGeeks — HTML DOM (Document Object Model)](https://www.geeksforgeeks.org/javascript/dom-document-object-model/)
- [GeeksforGeeks — How to select DOM Elements in JavaScript](https://www.geeksforgeeks.org/javascript/how-to-select-dom-elements-in-javascript/)
- [GeeksforGeeks — addEventListener() with Examples](https://www.geeksforgeeks.org/javascript/javascript-addeventlistener-with-examples/)
- [GeeksforGeeks — removeEventListener()](https://www.geeksforgeeks.org/javascript/javascript-removeeventlistener-method-with-examples/)
- [GeeksforGeeks — JavaScript Custom Events](https://www.geeksforgeeks.org/javascript/javascript-custom-events/)
- [GeeksforGeeks — Introduction to JavaScript](https://www.geeksforgeeks.org/javascript/introduction-to-javascript/)
