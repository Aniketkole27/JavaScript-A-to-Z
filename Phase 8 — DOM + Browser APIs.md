# JavaScript A→Z Revision

## Phase 8 — DOM + Browser APIs ⭐⭐⭐

> **Goal:** Understand how JavaScript interacts with the browser through the DOM and Browser APIs, manipulates elements, handles events, and builds interactive UI behavior.

**Importance:** ⭐⭐⭐⭐⭐  
**Difficulty:** Beginner → Intermediate  
**Interview Relevance:** Very High

> ⚠️ **Important:** The DOM is **not JavaScript itself**. The DOM is a browser-provided API that JavaScript uses to read, modify, create, and remove elements from an HTML document.

---

## 📚 Contents

1. [What Is the DOM?](#1-what-is-the-dom)
2. [DOM Tree](#2-dom-tree)
3. [DOM Element Selection](#3-dom-element-selection)
   - [querySelector()](#queryselector)
   - [querySelectorAll()](#queryselectorall)
   - [getElementById()](#getelementbyid)
4. [Creating DOM Elements](#4-creating-dom-elements)
5. [Adding Elements to the DOM](#5-adding-elements-to-the-dom)
   - [append()](#append)
   - [appendChild()](#appendchild)
6. [Removing DOM Elements](#6-removing-dom-elements)
7. [Working with Classes](#7-working-with-classes)
8. [Working with Attributes](#8-working-with-attributes)
9. [DOM Content and Properties](#9-dom-content-and-properties)
10. [DOM Traversal](#10-dom-traversal)
11. [Events](#11-events)
12. [Event Listeners](#12-event-listeners)
13. [Event Object](#13-event-object)
14. [Event Bubbling](#14-event-bubbling)
15. [Event Capturing](#15-event-capturing)
16. [Event Propagation](#16-event-propagation)
17. [Event Delegation](#17-event-delegation)
18. [preventDefault()](#18-preventdefault)
19. [stopPropagation()](#19-stoppropagation)
20. [Common Browser Events](#20-common-browser-events)
21. [Keyboard Events](#21-keyboard-events)
22. [Keyboard Shortcuts](#22-keyboard-shortcuts)
23. [Forms and Form Events](#23-forms-and-form-events)
24. [Drag and Drop API](#24-drag-and-drop-api)
25. [Browser APIs](#25-browser-apis)
26. [Todo App](#26-todo-app)
27. [Modal](#27-modal)
28. [Dropdown](#28-dropdown)
29. [Search Box](#29-search-box)
30. [Keyboard Shortcut Project](#30-keyboard-shortcut-project)
31. [Drag and Drop Project](#31-drag-and-drop-project)
32. [Common Interview Traps](#32-common-interview-traps)
33. [Quick Revision](#33-quick-revision)
34. [Interview Questions](#34-interview-questions)

---

# 1. What Is the DOM?

## Definition

**DOM = Document Object Model**

The DOM is the browser's **object representation of an HTML document**.

```html
<body>
  <h1>Hello</h1>
  <button>Click Me</button>
</body>
```

The browser parses this HTML and creates a tree of objects:

```text
Document
   │
   └── html
        │
        └── body
             ├── h1
             │    └── "Hello"
             │
             └── button
                  └── "Click Me"
```

JavaScript can interact with these objects.

```text
HTML
 ↓
Browser parses HTML
 ↓
DOM Tree
 ↓
JavaScript
 ↓
Read / Modify / Create / Remove
 ↓
Updated UI
```

## Why?

The DOM provides JavaScript with APIs for:

- Selecting elements
- Changing content
- Changing classes
- Creating elements
- Removing elements
- Adding event listeners
- Reading user input
- Manipulating attributes

Example:

```js
const heading = document.querySelector("h1");

heading.textContent = "Welcome";
```

> ⚠️ **Remember:** HTML is the document markup. The DOM is the browser's runtime representation of that document.

---

# 2. DOM Tree

The DOM represents the document as a tree structure.

```html
<div class="container">
  <h1>Products</h1>

  <ul>
    <li>Apple</li>
    <li>Mango</li>
  </ul>
</div>
```

DOM:

```text
div
│
├── h1
│    └── "Products"
│
└── ul
     │
     ├── li
     │    └── "Apple"
     │
     └── li
          └── "Mango"
```

Important DOM relationships:

```js
element.parentElement

element.children

element.firstElementChild

element.lastElementChild

element.nextElementSibling

element.previousElementSibling
```

---

# 3. DOM Element Selection

The most important selection methods are:

```js
querySelector()

querySelectorAll()

getElementById()
```

## querySelector()

Returns the **first element** matching a CSS selector.

```js
const heading = document.querySelector("h1");

const button = document.querySelector(".submit-btn");

const container = document.querySelector("#container");
```

It accepts CSS selectors:

```js
document.querySelector(".card button");

document.querySelector("ul > li");

document.querySelector("[data-id='123']");
```

If nothing matches:

```js
document.querySelector(".unknown");
```

returns:

```js
null
```

## querySelectorAll()

Returns all matching elements as a **NodeList**.

```js
const items = document.querySelectorAll(".item");

items.forEach((item) => {
  console.log(item.textContent);
});
```

> ⚠️ `querySelectorAll()` returns a NodeList, not a normal Array.

Convert it when needed:

```js
const array = [...items];
```

or:

```js
const array = Array.from(items);
```

## getElementById()

Selects an element using its `id`.

```html
<div id="app"></div>
```

```js
const app = document.getElementById("app");
```

Do not write:

```js
document.getElementById("#app");
```

`getElementById()` expects the ID itself, not a CSS selector.

## Selection Comparison

| Method | Returns | Input |
|---|---|---|
| `querySelector()` | First matching element | CSS selector |
| `querySelectorAll()` | NodeList | CSS selector |
| `getElementById()` | Single element | ID |

---

# 4. Creating DOM Elements

`document.createElement()` creates a new DOM element.

```js
const button = document.createElement("button");
```

At this point, the element exists in memory but has not been inserted into the document.

```text
createElement()
      ↓
Element created
      ↓
Exists in memory
      ↓
Not visible on page yet
```

Example:

```js
const card = document.createElement("div");

card.classList.add("card");

card.textContent = "Product";

document.body.append(card);
```

---

# 5. Adding Elements to the DOM

## append()

Adds one or more nodes or strings to the end of an element.

```js
parent.append(child);
```

Example:

```js
const list = document.querySelector("ul");

const item = document.createElement("li");

item.textContent = "Apple";

list.append(item);
```

Multiple values:

```js
container.append(title, paragraph, button);
```

It can also append text:

```js
container.append("Hello");
```

## appendChild()

```js
parent.appendChild(child);
```

Example:

```js
const item = document.createElement("li");

item.textContent = "Apple";

list.appendChild(item);
```

### Difference

`append()`:

- Can append multiple nodes
- Can append strings
- Returns `undefined`

`appendChild()`:

- Accepts a Node
- Appends one child
- Returns the appended Node

---

# 6. Removing DOM Elements

## remove()

```js
element.remove();
```

Example:

```js
const button = document.querySelector(".delete-btn");

button.remove();
```

## removeChild()

```js
parent.removeChild(child);
```

For modern code:

```js
item.remove();
```

is generally simpler.

---

# 7. Working with Classes

The `classList` API provides methods for manipulating classes.

```js
element.classList
```

Important methods:

```js
classList.add()

classList.remove()

classList.toggle()

classList.contains()

classList.replace()
```

## classList.add()

```js
modal.classList.add("open");
```

Multiple classes:

```js
element.classList.add("active", "visible");
```

## classList.remove()

```js
modal.classList.remove("open");
```

## classList.toggle()

Adds the class if it doesn't exist and removes it if it already exists.

```js
menu.classList.toggle("open");
```

Useful for:

- Dropdowns
- Modals
- Mobile menus
- Accordions
- Dark mode
- Sidebars

## classList.contains()

```js
if (modal.classList.contains("open")) {
  console.log("Modal is open");
}
```

## classList.replace()

```js
element.classList.replace("old-class", "new-class");
```

---

# 8. Working with Attributes

HTML attributes provide additional information about elements.

```html
<img
  src="profile.jpg"
  alt="User Profile"
  width="200"
/>
```

## setAttribute()

```js
element.setAttribute(name, value);
```

Example:

```js
image.setAttribute("src", "profile.jpg");

image.setAttribute("alt", "User profile");
```

## getAttribute()

```js
const src = image.getAttribute("src");
```

## removeAttribute()

```js
button.removeAttribute("disabled");
```

## hasAttribute()

```js
if (button.hasAttribute("disabled")) {
  console.log("Button is disabled");
}
```

## Attribute vs Property

Many HTML attributes are also exposed as DOM properties:

```js
input.value

checkbox.checked

button.disabled

image.src

element.id

element.className
```

For interactive form state, DOM properties such as `input.value`, `checkbox.checked`, and `button.disabled` are often what you actually want to manipulate.

---

# 9. DOM Content and Properties

Important properties:

```js
textContent

innerHTML

innerText

value
```

## textContent

Gets or sets text content.

```js
heading.textContent = "Welcome";
```

For plain user-provided text, prefer:

```js
element.textContent = userInput;
```

## innerHTML

Gets or sets HTML markup.

```js
container.innerHTML = `
  <h2>Hello</h2>
  <p>Welcome</p>
`;
```

> ⚠️ Never blindly put untrusted user input into `innerHTML`. This can create **Cross-Site Scripting (XSS)** vulnerabilities.

Unsafe:

```js
container.innerHTML = userInput;
```

Safer for plain text:

```js
container.textContent = userInput;
```

## innerText

Represents rendered text and is affected by CSS/rendering behavior.

```js
console.log(element.innerText);
```

For straightforward text manipulation, `textContent` is usually preferable.

## value

Used heavily with form controls.

```js
const input = document.querySelector("input");

console.log(input.value);
```

Set a value:

```js
input.value = "Hello";
```

---

# 10. DOM Traversal

DOM traversal means moving between related nodes/elements.

Important properties:

```js
parentElement

children

firstElementChild

lastElementChild

nextElementSibling

previousElementSibling
```

Example:

```html
<div id="parent">
  <button>One</button>
  <button>Two</button>
</div>
```

```js
const parent = document.querySelector("#parent");

console.log(parent.children);
console.log(parent.firstElementChild);
console.log(parent.lastElementChild);
```

## parentElement

```js
const button = document.querySelector("button");

console.log(button.parentElement);
```

## children

```js
console.log(parent.children);
```

Returns an `HTMLCollection`.

## firstElementChild

```js
console.log(parent.firstElementChild);
```

## lastElementChild

```js
console.log(parent.lastElementChild);
```

## nextElementSibling

```js
console.log(button.nextElementSibling);
```

## previousElementSibling

```js
console.log(button.previousElementSibling);
```

## closest()

Finds the nearest ancestor matching a selector, including the element itself.

```js
const button = event.target.closest("button");
```

Very useful for event delegation.

---

# 11. Events

An **event** represents something that happens in the browser.

Examples:

```text
User clicks
User types
Form submits
Mouse moves
Keyboard key is pressed
Element receives focus
Element loses focus
User drags an element
Page scrolls
```

The browser detects these events and JavaScript can respond.

```text
User Action
     ↓
Browser detects event
     ↓
Event created
     ↓
Event propagation
     ↓
Event listener executes
     ↓
JavaScript logic
     ↓
UI changes
```

---

# 12. Event Listeners

## addEventListener()

Registers a function to run when an event occurs.

```js
element.addEventListener(eventType, callback);
```

Example:

```js
const button = document.querySelector("button");

button.addEventListener("click", () => {
  console.log("Button clicked");
});
```

## Named Event Handler

```js
function handleClick() {
  console.log("Clicked");
}

button.addEventListener("click", handleClick);
```

## removeEventListener()

```js
button.removeEventListener("click", handleClick);
```

This will not remove an anonymous function created separately:

```js
button.addEventListener("click", () => {
  console.log("Clicked");
});

button.removeEventListener("click", () => {
  console.log("Clicked");
});
```

Use the same function reference:

```js
function handleClick() {
  console.log("Clicked");
}

button.addEventListener("click", handleClick);

button.removeEventListener("click", handleClick);
```

---

# 13. Event Object

When an event occurs, the browser passes an **Event object** to the listener.

```js
button.addEventListener("click", (event) => {
  console.log(event);
});
```

Important properties:

```js
event.target

event.currentTarget

event.type

event.key

event.code

event.clientX

event.clientY
```

## event.target

The element from which the event originated.

```js
container.addEventListener("click", (event) => {
  console.log(event.target);
});
```

## event.currentTarget

The element whose listener is currently executing.

```js
container.addEventListener("click", (event) => {
  console.log(event.currentTarget);
});
```

## target vs currentTarget

```text
event.target
      ↓
Where the event originated

event.currentTarget
      ↓
Where the current listener is attached
```

---

# 14. Event Bubbling

**Event bubbling** means an event travels from the target element upward through its ancestors during the bubbling phase.

```html
<div id="parent">
  <button id="child">Click</button>
</div>
```

```js
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

parent.addEventListener("click", () => {
  console.log("Parent");
});

child.addEventListener("click", () => {
  console.log("Child");
});
```

Clicking the button:

```text
Child
Parent
```

Conceptually:

```text
button
  ↓
div
  ↓
body
  ↓
html
  ↓
document
```

Bubbling enables:

- Event delegation
- Parent-level event handling
- Dynamic element handling

---

# 15. Event Capturing

**Event capturing** is the phase where the event travels from the outer ancestor toward the target.

```text
document
   ↓
html
   ↓
body
   ↓
parent
   ↓
target
```

To listen during capturing:

```js
parent.addEventListener(
  "click",
  () => {
    console.log("Parent capture");
  },
  true
);
```

Or:

```js
parent.addEventListener("click", handler, {
  capture: true
});
```

---

# 16. Event Propagation

Event propagation consists conceptually of three phases:

```text
1. Capturing Phase
        ↓
2. Target Phase
        ↓
3. Bubbling Phase
```

Complete flow:

```text
                Capturing
                    ↓
document
    ↓
html
    ↓
body
    ↓
parent
    ↓
target
    ↑
parent
    ↑
body
    ↑
html
    ↑
document
                    ↑
                Bubbling
```

Most event listeners are registered for the bubbling phase.

---

# 17. Event Delegation

**Event delegation** means attaching an event listener to a parent instead of adding separate listeners to every child.

Example:

```html
<ul id="todo-list">
  <li>Learn JavaScript</li>
  <li>Learn React</li>
  <li>Build Project</li>
</ul>
```

Instead of:

```js
document.querySelectorAll("li").forEach((item) => {
  item.addEventListener("click", handleClick);
});
```

Use:

```js
const list = document.querySelector("#todo-list");

list.addEventListener("click", (event) => {
  if (event.target.matches("li")) {
    console.log(event.target.textContent);
  }
});
```

## Why Event Delegation?

Without delegation:

```text
100 buttons
    ↓
100 listeners
```

With delegation:

```text
100 buttons
    ↓
1 parent listener
```

Benefits:

- Fewer event listeners
- Better handling of dynamic elements
- Cleaner code
- Centralized event handling

## Dynamic Elements

If a new `<li>` is added later:

```js
const item = document.createElement("li");

item.textContent = "New Todo";

list.append(item);
```

The parent listener can still handle it.

## closest() + Delegation

```html
<button data-action="delete">
  <span>Delete</span>
</button>
```

If the user clicks `<span>`, use:

```js
list.addEventListener("click", (event) => {
  const button = event.target.closest("[data-action='delete']");

  if (!button) return;

  button.closest(".todo").remove();
});
```

---

# 18. preventDefault()

`preventDefault()` prevents the browser's **default action** associated with an event.

## Form Example

```js
form.addEventListener("submit", (event) => {
  event.preventDefault();

  console.log("Handle submission using JavaScript");
});
```

## Link Example

```js
link.addEventListener("click", (event) => {
  event.preventDefault();

  console.log("Navigation prevented");
});
```

`preventDefault()`:

```text
Stops default browser behavior
```

It does **not** stop event propagation.

---

# 19. stopPropagation()

`stopPropagation()` prevents the event from continuing through the propagation path.

```js
parent.addEventListener("click", () => {
  console.log("Parent");
});

child.addEventListener("click", (event) => {
  event.stopPropagation();

  console.log("Child");
});
```

Clicking the child produces:

```text
Child
```

## Difference

| Method | Purpose |
|---|---|
| `preventDefault()` | Prevent browser's default action |
| `stopPropagation()` | Stop event propagation |

> ⚠️ Avoid using `stopPropagation()` unnecessarily. It can break event delegation and other parent-level event handling.

---

# 20. Common Browser Events

| Event | Purpose |
|---|---|
| `click` | Element clicked |
| `dblclick` | Double click |
| `input` | Input value changes |
| `change` | Value change is committed |
| `submit` | Form submitted |
| `keydown` | Keyboard key pressed |
| `keyup` | Keyboard key released |
| `focus` | Element receives focus |
| `blur` | Element loses focus |
| `mouseenter` | Pointer enters element |
| `mouseleave` | Pointer leaves element |
| `mouseover` | Pointer moves over element |
| `mouseout` | Pointer moves out |
| `scroll` | Element/document scrolls |
| `resize` | Window size changes |
| `dragstart` | Drag begins |
| `dragover` | Dragged item is over target |
| `drop` | Item is dropped |
| `dragend` | Drag operation ends |

---

# 21. Keyboard Events

The most important keyboard events are:

```js
keydown

keyup
```

Example:

```js
document.addEventListener("keydown", (event) => {
  console.log(event.key);
});
```

Important properties:

```js
event.key

event.code

event.ctrlKey

event.shiftKey

event.altKey

event.metaKey
```

## key

Represents the logical key.

Examples:

```text
a
Enter
Escape
```

## code

Represents the physical keyboard key.

Examples:

```text
KeyA
Enter
Escape
Space
```

---

# 22. Keyboard Shortcuts

Keyboard shortcuts are commonly implemented with `keydown`.

```js
document.addEventListener("keydown", (event) => {
  if (event.ctrlKey && event.key.toLowerCase() === "s") {
    event.preventDefault();

    console.log("Save");
  }
});
```

## Cross-Platform Shortcut

```js
document.addEventListener("keydown", (event) => {
  const commandKey = event.ctrlKey || event.metaKey;

  if (
    commandKey &&
    event.key.toLowerCase() === "k"
  ) {
    event.preventDefault();

    console.log("Open command menu");
  }
});
```

Useful shortcuts:

```text
Ctrl/Cmd + K → Command menu
Ctrl/Cmd + S → Save
Escape       → Close modal
Enter        → Submit
```

---

# 23. Forms and Form Events

Forms are one of the most common places where DOM and events interact.

```html
<form id="login-form">
  <input
    id="email"
    type="email"
  />

  <button type="submit">
    Login
  </button>
</form>
```

```js
const form = document.querySelector("#login-form");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const email = document.querySelector("#email").value;

  console.log(email);
});
```

## input Event

Runs when the input value changes as the user types.

```js
input.addEventListener("input", (event) => {
  console.log(event.target.value);
});
```

Useful for:

- Search
- Live validation
- Character counters
- Filters
- Autocomplete

## change Event

Tr