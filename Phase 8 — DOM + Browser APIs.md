<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 8 — DOM + Browser APIs</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-8%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Focus-Frontend%20Core-orange" />
</p>

> DOM + events is the foundation everything React abstracts away — knowing it raw makes React's synthetic events and refs make sense.

## 📑 Contents
- [DOM Selection & Manipulation](#dom-selection--manipulation)
- [Events](#events)
  - [Event Bubbling & Capturing](#event-bubbling--capturing)
  - [Event Delegation](#event-delegation)
  - [`preventDefault()` vs `stopPropagation()`](#preventdefault-vs-stoppropagation)
- [🔧 Build: Todo App](#-build-todo-app)
- [🔧 Build: Modal](#-build-modal)
- [🔧 Build: Dropdown](#-build-dropdown)
- [🔧 Build: Search Box (debounced)](#-build-search-box-debounced)
- [🔧 Build: Keyboard Shortcut](#-build-keyboard-shortcut)
- [🔧 Build: Drag & Drop](#-build-drag--drop)
- [🧪 Self-Test](#-self-test)

---

## DOM Selection & Manipulation

**Code**
```js
// Selecting
const byId = document.getElementById("app");            // single element, no "#"
const single = document.querySelector(".item");           // first match, CSS selector syntax
const all = document.querySelectorAll(".item");             // NodeList (static) of ALL matches

all.forEach(el => console.log(el)); // NodeList supports forEach directly

// Creating & inserting
const div = document.createElement("div");
div.textContent = "Hello";
document.body.append(div);          // append() -> adds at the end, accepts multiple nodes/strings
document.body.prepend(div);          // adds at the start
div.remove();                         // removes itself from the DOM directly

// classList — the preferred way to toggle styling
const el = document.querySelector(".box");
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");        // adds if absent, removes if present
el.classList.contains("active");        // boolean check

// Attributes
el.setAttribute("data-id", "42");
console.log(el.getAttribute("data-id")); // "42"
console.log(el.dataset.id);               // "42" -> shortcut for data-* attributes
```

**Explain**
- `querySelector`/`querySelectorAll` accept **any CSS selector** (`.class`, `#id`, `[attr]`, `div > p`) — prefer these over `getElementById`/`getElementsByClassName` for consistency.
- `querySelectorAll` returns a **static NodeList** (snapshot at call time) — it won't auto-update if you add more matching elements later.
- `classList` > manually editing `element.className` string — no risk of accidentally wiping other classes.

---

## Events

### Event Bubbling & Capturing

**Understand**
An event on a nested element travels through the DOM tree in two phases:
1. **Capturing** — top (`document`) → down to the target.
2. **Bubbling** — target → back up to the top. (default phase for listeners)

**Code**
```html
<div id="outer">
  <button id="inner">Click me</button>
</div>
```
```js
document.getElementById("outer").addEventListener("click", () => console.log("outer (bubble)"));
document.getElementById("inner").addEventListener("click", () => console.log("inner (bubble)"));

document.getElementById("outer").addEventListener("click", () => console.log("outer (capture)"), { capture: true });

// Clicking the button logs:
// "outer (capture)"  -> capturing phase, runs first
// "inner (bubble)"    -> reaches the target
// "outer (bubble)"     -> bubbles back up
```

**Explain**
By default, `addEventListener` listens in the **bubbling** phase (3rd argument `false`/omitted). Passing `{ capture: true }` listens during the capturing phase instead — rarely needed, but explains *why* a parent's bubble-phase listener always fires **after** the child's.

---

### Event Delegation

**Understand**
Instead of attaching a listener to every child, attach **one listener to a parent** and use `event.target` to figure out which child was actually clicked. Relies on bubbling.

**Code**
```html
<ul id="list">
  <li>Apple</li>
  <li>Banana</li>
  <li>Cherry</li>
</ul>
```
```js
// ❌ Inefficient — a listener per item, and new items added later won't have one
document.querySelectorAll("#list li").forEach(li => {
  li.addEventListener("click", () => console.log(li.textContent));
});

// ✅ Delegation — one listener, works even for items added AFTER this runs
document.getElementById("list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log(e.target.textContent);
  }
});
```

**Explain**
Delegation is a **must-know performance/architecture pattern**: fewer listeners, automatically works for dynamically-added elements, and is exactly how React's synthetic event system works under the hood (one root listener, not thousands).

---

### `preventDefault()` vs `stopPropagation()`

**Code**
```js
document.querySelector("form").addEventListener("submit", (e) => {
  e.preventDefault(); // stops the browser's default action (page reload on submit)
  console.log("form handled by JS instead");
});

document.querySelector("a").addEventListener("click", (e) => {
  e.preventDefault(); // stops navigation
});

document.getElementById("inner").addEventListener("click", (e) => {
  e.stopPropagation(); // stops the event from bubbling further up
  console.log("inner clicked — outer will NOT hear this");
});
```

**Explain**
| Method | Stops | Does NOT stop |
|---|---|---|
| `preventDefault()` | the browser's default behavior (navigation, form submit, checkbox toggle) | propagation — event still bubbles |
| `stopPropagation()` | the event from reaching parent listeners | the default browser action |

They solve **different problems** and are often needed together (e.g. a custom dropdown item: prevent link navigation *and* stop the click from closing the dropdown via an outer listener).

---

## 🔧 Build: Todo App

```html
<input id="todoInput" placeholder="Add a task" />
<button id="addBtn">Add</button>
<ul id="todoList"></ul>
```
```js
const input = document.getElementById("todoInput");
const list = document.getElementById("todoList");

document.getElementById("addBtn").addEventListener("click", addTodo);
input.addEventListener("keydown", (e) => { if (e.key === "Enter") addTodo(); });

function addTodo() {
  const text = input.value.trim();
  if (!text) return;

  const li = document.createElement("li");
  li.textContent = text;

  const removeBtn = document.createElement("button");
  removeBtn.textContent = "✕";
  removeBtn.addEventListener("click", () => li.remove());

  li.append(removeBtn);
  list.append(li);
  input.value = "";
}

// Delegation version for toggling "done" — one listener for all current AND future items
list.addEventListener("click", (e) => {
  if (e.target.tagName === "LI") e.target.classList.toggle("done");
});
```

**Explain**
Combines: element creation, `append`, event delegation (toggle-done), and direct listeners (remove button) — a small app that touches almost every DOM concept above.

---

## 🔧 Build: Modal

```html
<button id="openModal">Open</button>
<div id="overlay" class="hidden">
  <div id="modal">
    <button id="closeModal">✕</button>
    <p>Modal content</p>
  </div>
</div>
```
```js
const overlay = document.getElementById("overlay");

document.getElementById("openModal").addEventListener("click", () => overlay.classList.remove("hidden"));
document.getElementById("closeModal").addEventListener("click", () => overlay.classList.add("hidden"));

// Close on overlay click, but NOT when clicking inside the modal itself
overlay.addEventListener("click", (e) => {
  if (e.target === overlay) overlay.classList.add("hidden");
});

// Close on Escape key
document.addEventListener("keydown", (e) => {
  if (e.key === "Escape") overlay.classList.add("hidden");
});
```

**Explain**
`e.target === overlay` check is the key trick — without it, clicking anywhere *inside* the modal would also close it (since clicks inside bubble up to the overlay listener too).

---

## 🔧 Build: Dropdown

```html
<div class="dropdown">
  <button id="dropBtn">Menu ▾</button>
  <ul id="dropMenu" class="hidden">
    <li>Profile</li>
    <li>Settings</li>
    <li>Logout</li>
  </ul>
</div>
```
```js
const dropBtn = document.getElementById("dropBtn");
const dropMenu = document.getElementById("dropMenu");

dropBtn.addEventListener("click", (e) => {
  e.stopPropagation();              // don't let this click reach the document listener below
  dropMenu.classList.toggle("hidden");
});

// Close when clicking anywhere else on the page
document.addEventListener("click", () => dropMenu.classList.add("hidden"));

dropMenu.addEventListener("click", (e) => console.log("Selected:", e.target.textContent));
```

**Explain**
The "click outside to close" pattern relies on `stopPropagation()` on the button — otherwise its own click would bubble to `document` and immediately re-close the menu it just opened.

---

## 🔧 Build: Search Box (debounced)

```html
<input id="search" placeholder="Search..." />
<ul id="results"></ul>
```
```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

const data = ["Apple", "Banana", "Cherry", "Mango", "Orange"];
const results = document.getElementById("results");

function search(query) {
  results.innerHTML = "";
  data
    .filter(item => item.toLowerCase().includes(query.toLowerCase()))
    .forEach(item => {
      const li = document.createElement("li");
      li.textContent = item;
      results.append(li);
    });
}

document.getElementById("search").addEventListener("input", debounce((e) => {
  search(e.target.value);
}, 300));
```

**Explain**
Without debouncing, `search()` (and a real API call) would fire on **every single keystroke**. `debounce` waits until the user pauses typing for `300ms` before running — the single most common performance pattern for search/autocomplete inputs.

---

## 🔧 Build: Keyboard Shortcut

```js
document.addEventListener("keydown", (e) => {
  // Ctrl/Cmd + K -> focus search (like GitHub, Slack, etc.)
  if ((e.ctrlKey || e.metaKey) && e.key === "k") {
    e.preventDefault();               // stop browser's default (e.g. address bar focus)
    document.getElementById("search")?.focus();
  }

  // Save shortcut
  if ((e.ctrlKey || e.metaKey) && e.key === "s") {
    e.preventDefault();
    console.log("Saved!");
  }
});
```

**Explain**
`e.metaKey` = Cmd on Mac, `e.ctrlKey` = Ctrl on Windows/Linux — check both for cross-platform shortcuts. Always `preventDefault()` first, since many key combos have a built-in browser action you're overriding.

---

## 🔧 Build: Drag & Drop

```html
<div class="box" draggable="true" id="dragItem">Drag me</div>
<div class="dropzone" id="dropZone">Drop here</div>
```
```js
const dragItem = document.getElementById("dragItem");
const dropZone = document.getElementById("dropZone");

dragItem.addEventListener("dragstart", (e) => {
  e.dataTransfer.setData("text/plain", e.target.id); // stash the dragged element's id
});

dropZone.addEventListener("dragover", (e) => {
  e.preventDefault(); // ⚠️ REQUIRED — drop is blocked by default without this
});

dropZone.addEventListener("drop", (e) => {
  e.preventDefault();
  const id = e.dataTransfer.getData("text/plain");
  dropZone.append(document.getElementById(id));
});
```

**Explain**
`dragover` **must** call `preventDefault()`, or the browser refuses to allow a drop at all — the single most common "drag & drop isn't working" bug. Data is passed between the two events via `dataTransfer`, not directly.

---

## 🧪 Self-Test

```js
// 1. What's the difference in output order?
document.body.addEventListener("click", () => console.log("body"));
document.body.addEventListener("click", () => console.log("body-capture"), true);
document.getElementById("btn").addEventListener("click", () => console.log("btn"));
// clicking #btn logs in what order?

// 2. Why does this fail to close only the clicked dropdown among many?
document.querySelectorAll(".dropdown-btn").forEach(btn => {
  btn.addEventListener("click", () => {
    document.querySelectorAll(".menu").forEach(m => m.classList.add("hidden"));
  });
});
```

<details>
<summary>Show answers</summary>

```
1. "body-capture" -> "btn" -> "body"
   (capture phase fires top-down first, then target, then bubble phase bottom-up)

2. It doesn't actually fail — this correctly closes ALL menus, including
   the one just opened, unless you re-open the specific one AFTER closing all:
   e.g. store target menu, closeAll(), then toggle just that one.
   Classic bug: closing everything wipes out the menu you meant to open.
```
</details>

---

<p align="center"><i>Next → <code>09-async-js.md</code></i></p>
