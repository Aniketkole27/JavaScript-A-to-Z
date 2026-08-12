<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 5 — <code>this</code>, call, apply, bind ⭐⭐⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-5%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Top%20Interview%20Topic-critical" />
</p>

> `this` is decided by **how a function is called**, not where it's written (except arrow functions — those break the rule on purpose).

## 📑 Contents
- [The One Rule](#the-one-rule)
- [1. Global Context](#1-global-context)
- [2. Object Methods](#2-object-methods)
- [3. Regular Functions](#3-regular-functions)
- [4. Arrow Functions](#4-arrow-functions)
- [5. Constructor Functions](#5-constructor-functions)
- [6. Classes](#6-classes)
- [7. Event Handlers](#7-event-handlers)
- [8. `call()`, `apply()`, `bind()` ⭐](#8-call-apply-bind-)
- [🧪 Self-Test](#-self-test)

---

## The One Rule

**`this` = whatever is to the left of the dot at call time.**

```js
obj.method();   // `this` = obj
fn();            // `this` = undefined (strict) / global object (non-strict)
```

Arrow functions are the one exception — they have **no own `this`**, they inherit it lexically from where they were *written*, not called.

---

## 1. Global Context

**Code**
```js
console.log(this); // in a browser module: undefined (strict mode) / {} in plain script -> window

function showThis() {
  console.log(this); // non-strict: window/global | strict mode: undefined
}
showThis();

"use strict";
function strictThis() {
  console.log(this); // undefined
}
strictThis();
```

**Explain**
At the top level, `this` refers to the global object (`window` in browsers, `{}`/`module.exports` in Node modules) in non-strict mode. Inside any function called **without** an owning object, `this` is `undefined` in strict mode (ES modules and classes are strict by default).

---

## 2. Object Methods

**Code**
```js
const user = {
  name: "Aniket",
  greet() {
    console.log(this.name); // `this` = whatever object called greet
  }
};
user.greet(); // "Aniket" -> called as user.greet()

const detached = user.greet;
detached(); // undefined -> called alone, no owning object, `this` lost
```

**Explain**
`this` is bound **at call time**, not at definition time. Assigning a method to a plain variable and calling it "detaches" it from its object — this is the #1 cause of "`this` is undefined" bugs (e.g. passing `onClick={obj.method}` in React without binding).

---

## 3. Regular Functions

**Code**
```js
function standalone() {
  console.log(this);
}
standalone(); // undefined (strict) — no object called it

const obj = {
  outer() {
    function inner() {
      console.log(this); // undefined -> inner is a REGULAR function, called plainly
    }
    inner();
  }
};
obj.outer();
```

**Explain**
A nested regular function does **not** inherit `this` from its containing method — it gets its own `this`, determined again by how *it* was called (plainly, so `undefined`/global). This surprises people coming from other languages.

---

## 4. Arrow Functions

**Code**
```js
const obj = {
  name: "Aniket",
  outer() {
    const inner = () => {
      console.log(this.name); // arrow -> inherits `this` from outer() lexically
    };
    inner();
  }
};
obj.outer(); // "Aniket"

const arrowAtTopLevel = () => console.log(this);
arrowAtTopLevel(); // `this` = whatever `this` is at the module/global level
```

**Explain**
Arrow functions capture `this` from their **enclosing lexical scope at definition time**, permanently. No `call`/`apply`/`bind` can change an arrow function's `this` — it's fixed forever. This is exactly why arrow functions fixed the classic "nested callback loses `this`" problem.

---

## 5. Constructor Functions

**Code**
```js
function Person(name) {
  this.name = name; // `this` = the newly created object
}
const p1 = new Person("Aniket");
console.log(p1.name); // "Aniket"

function Broken(name) {
  this.name = name;
}
const p2 = Broken("Oops"); // called WITHOUT `new`
console.log(p2);            // undefined -> function returned nothing
console.log(this.name);      // "Oops" leaked onto global object! (non-strict)
```

**Explain**
`new` does four things: (1) creates a new empty object, (2) sets `this` to that object, (3) links its prototype, (4) returns it automatically (unless the function explicitly returns another object). Forgetting `new` is a classic bug — `this` silently falls back to global/undefined instead of erroring.

---

## 6. Classes

**Code**
```js
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hi, I'm ${this.name}`); // `this` = instance, when called correctly
  }
  greetArrow = () => {
    console.log(`Hi, I'm ${this.name}`); // arrow class field -> `this` permanently bound to instance
  };
}
const p = new Person("Aniket");
p.greet(); // "Hi, I'm Aniket"

const detachedGreet = p.greet;
detachedGreet(); // TypeError / undefined -> same detachment problem as §2

const detachedArrow = p.greetArrow;
detachedArrow(); // "Hi, I'm Aniket" -> arrow field still works, `this` was locked in
```

**Explain**
Class methods behave exactly like object methods — `this` depends on the call site, and classes are **always strict mode**, so a detached method's `this` is `undefined` (not global). The **arrow function class field** pattern (`greetArrow = () => {}`) is the common React fix for passing methods as callbacks (e.g. `onClick={p.greetArrow}`) without needing `.bind()`.

---

## 7. Event Handlers

**Code**
```js
const button = document.querySelector("button");

button.addEventListener("click", function () {
  console.log(this); // `this` = the button element (DOM sets it to the element handling the event)
});

button.addEventListener("click", () => {
  console.log(this); // arrow -> `this` from surrounding scope, NOT the button
});

class Toggle {
  constructor() {
    this.on = false;
    document.querySelector("button").addEventListener("click", this.handleClick); // ⚠️ detached
    document.querySelector("button").addEventListener("click", this.handleClick.bind(this)); // ✅ fixed
    document.querySelector("button").addEventListener("click", () => this.handleClick()); // ✅ also fixed
  }
  handleClick() {
    this.on = !this.on;
    console.log(this.on);
  }
}
```

**Explain**
Regular function handlers get `this` = the DOM element that triggered the event (useful for toggling classes, reading `this.value`, etc.). Arrow function handlers ignore that and use the surrounding scope's `this` instead — usually what you want in a class/component, not in a plain `addEventListener`.

---

## 8. `call()`, `apply()`, `bind()` ⭐

**Understand**
All three let you **explicitly set `this`** for a function, instead of relying on the call site.

| Method | Invokes immediately? | Arguments | Returns |
|---|---|---|---|
| `call(thisArg, a, b, ...)` | ✅ yes | listed individually | function's return value |
| `apply(thisArg, [a, b])` | ✅ yes | as an **array** | function's return value |
| `bind(thisArg, a, ...)` | ❌ no | listed individually | a **new function**, callable later |

**Code**
```js
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}

greet.call(user, 21);   // "Aniket" 21  -> calls immediately, args passed one by one

greet.apply(user, [21]); // "Aniket" 21  -> calls immediately, args passed as an array

const fn = greet.bind(user);
fn(21);                    // "Aniket" 21  -> bind returns a NEW function, called later
```

**Explain**
- `call`/`apply` do the exact same thing — the only difference is **how arguments are passed** (comma list vs array). `apply` is handy when you already have an array (e.g. `Math.max.apply(null, arr)`, though spread `Math.max(...arr)` replaced this).
- `bind` doesn't call the function — it returns a **new permanently-bound copy**. Classic use: `this.handleClick = this.handleClick.bind(this)` in React class constructors, so passing the method as a prop callback doesn't lose `this`.
- Once bound with `bind`, a function's `this` **cannot be changed again** — even calling `.call()` on a bound function has no effect on `this`.

```js
const bound = greet.bind(user);
bound.call({ name: "Someone Else" }, 30); // still "Aniket" 30 -> bind wins, can't be overridden
```

<details>
<summary>❓ Revisit</summary>

```js
const a = { val: 1 };
const b = { val: 2 };

function show() { console.log(this.val); }

const boundA = show.bind(a);
boundA();                 // ?
boundA.call(b);            // ?
show.call(b);               // ?

const arrowShow = () => console.log(this.val);
arrowShow.call(a);           // ?
```
```
1     // bound to a, permanent
1     // .call() cannot override an already-bound function
2     // plain .call() sets this = b
undefined  // arrow ignores call/apply/bind entirely, uses lexical `this`
```
</details>

---

## 🧪 Self-Test

```js
const car = { brand: "Tata" };

function describe(model) {
  console.log(`${this.brand} ${model}`);
}

describe.call(car, "Nexon");
describe.apply(car, ["Punch"]);
const describeCar = describe.bind(car);
describeCar("Tiago");

const obj = {
  name: "X",
  regular: function () { return this.name; },
  arrow: () => { return this.name; }
};
console.log(obj.regular());
console.log(obj.arrow());

const detached = obj.regular;
console.log(detached());
```

<details>
<summary>Show answers</summary>

```
Tata Nexon
Tata Punch
Tata Tiago

"X"          // regular -> this = obj
undefined     // arrow -> this from outer/module scope

undefined      // detached -> called plainly, this lost (or TypeError in strict mode)
```
</details>

---

<p align="center"><i>Next → <code>06-arrays.md</code></i></p>
