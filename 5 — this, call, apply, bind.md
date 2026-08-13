# JavaScript A→Z Revision

## Phase 5 — `this`, `call()`, `apply()`, `bind()` ⭐⭐⭐

> **Goal:** Understand exactly how JavaScript determines the value of `this` and how `call()`, `apply()`, and `bind()` can explicitly control it.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Intermediate → Advanced
**Interview Relevance:** Extremely High

> ⚠️ **Important:** `this` is **not determined simply by where a function is defined**. For regular functions, its value is primarily determined by **how the function is called**. Arrow functions are the major exception because they inherit `this` from their lexical scope.

---

## 📚 Contents

1. [`this` — What Is It?](#1-this--what-is-it)
2. [`this` in Global Context](#2-this-in-global-context)
3. [`this` in Object Methods](#3-this-in-object-methods)
4. [`this` in Regular Functions](#4-this-in-regular-functions)
5. [`this` in Arrow Functions](#5-this-in-arrow-functions)
6. [Regular Function vs Arrow Function](#6-regular-function-vs-arrow-function)
7. [`this` in Constructor Functions](#7-this-in-constructor-functions)
8. [`this` in Classes](#8-this-in-classes)
9. [`this` in Event Handlers](#9-this-in-event-handlers)
10. [`call()`](#10-call)
11. [`apply()`](#11-apply)
12. [`bind()`](#12-bind)
13. [`call()` vs `apply()` vs `bind()`](#13-call-vs-apply-vs-bind)
14. [Changing `this` with `call()`, `apply()`, and `bind()`](#14-changing-this-with-call-apply-and-bind)
15. [Important `this` Traps](#15-important-this-traps)
16. [Real-World Examples](#16-real-world-examples)
17. [Quick Revision](#17-quick-revision)
18. [Interview Questions](#18-interview-questions)
19. [Must Remember](#19-must-remember)

---

# 1. `this` — What Is It?

`this` is a special keyword that refers to a value determined by the **execution context of a function**.

The most important question when dealing with `this` is:

> **How was the function called?**

### Example

```js id="x8v7b1"
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

Output:

```text id="3c7j7s"
Aniket
```

Here:

```js id="x5n5r6"
this
```

refers to:

```js id="9p3w1f"
user
```

because the method was called as:

```js id="8ydx8r"
user.greet();
```

---

## The Core Rule

For a **regular function**, don't ask:

> "Where was this function written?"

Ask:

> **"How was this function called?"**

This rule explains most `this` behavior.

---

# 2. `this` in Global Context

The value of `this` at the top level depends on the JavaScript environment and whether the code is running as a script or an ES module.

In a browser script:

```js id="q8w5j1"
console.log(this);
```

At the top level, `this` refers to the global object, commonly:

```js id="9w2g4e"
window
```

In an ES module:

```js id="7n5r2k"
console.log(this);
```

top-level `this` is:

```text id="p6w4a9"
undefined
```

### Important

Do not memorize:

```text
this = window
```

as a universal rule.

The value depends on the execution context.

---

# 3. `this` in Object Methods

When a regular function is called as an object method:

```js id="6c0t0p"
object.method();
```

`this` refers to the object before the dot.

### Example

```js id="m3v4d2"
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

Output:

```text id="w7f4u8"
Aniket
```

Conceptually:

```text id="1h7v8p"
user.greet()
     │
     └── this → user
```

Therefore:

```js id="4e4h4u"
this.name
```

is equivalent to:

```js id="a7h2p5"
user.name
```

---

## Another Object

```js id="x5d7w3"
const admin = {
    name: "Rahul",

    greet() {
        console.log(this.name);
    }
};

admin.greet();
```

Output:

```text id="y4m3s7"
Rahul
```

The same function syntax can produce different `this` values depending on the calling object.

---

# 4. `this` in Regular Functions

For regular functions, `this` is determined by the **call site**.

Consider:

```js id="q5h2m6"
function greet() {
    console.log(this);
}
```

How we call `greet()` determines `this`.

---

## Method Call

```js id="4h9f8w"
const user = {
    name: "Aniket",
    greet
};

user.greet();
```

Here:

```text id="7w1m5x"
this → user
```

---

## Standalone Function Call

```js id="k8z4r2"
greet();
```

In strict mode:

```text id="3p8v5j"
this → undefined
```

In non-strict browser code, a standalone regular function call can resolve `this` to the global object.

### Strict Mode

```js id="j1x7q3"
"use strict";

function greet() {
    console.log(this);
}

greet();
```

Output:

```text id="5f6n9a"
undefined
```

### Important

Strict mode changes how `this` behaves for a standalone regular function call.

---

# 5. `this` in Arrow Functions

Arrow functions behave differently.

They **do not have their own `this`**.

Instead, they inherit `this` from their surrounding lexical scope.

### Example

```js id="r6p4w1"
const user = {
    name: "Aniket",

    greet: () => {
        console.log(this.name);
    }
};

user.greet();
```

You should **not** expect this to print `"Aniket"`.

Why?

Because the arrow function does not create its own `this`.

It inherits `this` from the surrounding scope.

---

## Correct Use Inside an Object Method

A common pattern is:

```js id="y3s8v2"
const user = {
    name: "Aniket",

    greet() {
        const printName = () => {
            console.log(this.name);
        };

        printName();
    }
};

user.greet();
```

Output:

```text id="d9q1s4"
Aniket
```

Why?

```text id="x3j6k8"
user.greet()
      ↓
this → user
      ↓
arrow function
      ↓
inherits this
      ↓
this → user
```

---

# 6. Regular Function vs Arrow Function

This distinction is extremely important.

### Regular Function

```js id="t2v7p4"
const user = {
    name: "Aniket",

    greet: function () {
        console.log(this.name);
    }
};

user.greet();
```

Here:

```text id="m9r5c2"
this → user
```

---

### Arrow Function

```js id="z8k3q6"
const user = {
    name: "Aniket",

    greet: () => {
        console.log(this.name);
    }
};
```

The arrow function does not create its own `this`.

---

## Comparison

| Feature                        | Regular Function | Arrow Function |
| ------------------------------ | ---------------- | -------------- |
| Own `this`                     | ✅                | ❌              |
| `this` determined by call site | ✅                | ❌              |
| Inherits lexical `this`        | ❌                | ✅              |
| Can be used with `new`         | ✅                | ❌              |
| Has its own `arguments`        | ✅                | ❌              |

### Easy Rule

> **Regular function → `this` depends on how it is called.**

> **Arrow function → `this` comes from the surrounding scope.**

---

# 7. `this` in Constructor Functions

Before JavaScript classes became common, constructor functions were widely used to create objects.

```js id="b5x8n1"
function User(name, age) {
    this.name = name;
    this.age = age;
}

const user1 = new User("Aniket", 21);

console.log(user1);
```

Result:

```js id="m1q7v4"
{
    name: "Aniket",
    age: 21
}
```

---

## What Does `new` Do?

When:

```js id="j4n8s2"
new User("Aniket", 21);
```

is executed, JavaScript roughly:

1. Creates a new object.
2. Connects that object to `User.prototype`.
3. Calls `User` with `this` pointing to the new object.
4. Returns the object unless the constructor explicitly returns another object.

Conceptually:

```text id="c8p2v6"
new User()
     ↓
new object created
     ↓
this → new object
     ↓
this.name = "Aniket"
this.age = 21
     ↓
object returned
```

---

# 8. `this` in Classes

Classes also use `this` to refer to the current instance.

```js id="f2m7x9"
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello ${this.name}`);
    }
}

const user = new User("Aniket");

user.greet();
```

Output:

```text id="q5k9d3"
Hello Aniket
```

Here:

```text id="n7v2p4"
user.greet()
     ↓
this → user
```

---

## Multiple Instances

```js id="w3c6r8"
const user1 = new User("Aniket");
const user2 = new User("Rahul");

user1.greet();
user2.greet();
```

Output:

```text id="h9m4x1"
Hello Aniket
Hello Rahul
```

Each instance gets its own `this` when its method is called as an instance method.

---

# 9. `this` in Event Handlers

With traditional DOM event handlers, when a regular function is used as the event listener, `this` commonly refers to the element on which the listener is registered.

```js id="s5d8k2"
const button = document.querySelector("button");

button.addEventListener("click", function () {
    console.log(this);
});
```

Here:

```text id="k3q7v9"
this → button
```

---

## Arrow Function Event Handler

With an arrow function:

```js id="x8m2p5"
button.addEventListener("click", () => {
    console.log(this);
});
```

The arrow function does not receive its own `this`.

It inherits `this` from its surrounding scope.

Therefore, if you specifically need the event target as `this`, use a regular function or use the event object:

```js id="v4n7c1"
button.addEventListener("click", function (event) {
    console.log(this);
    console.log(event.currentTarget);
});
```

---

## Better Modern Pattern

Instead of relying on `this`, you can explicitly use:

```js id="e9r3w6"
event.currentTarget
```

Example:

```js id="p7x5m2"
button.addEventListener("click", event => {
    console.log(event.currentTarget);
});
```

This makes the intention clearer.

---

# 10. `call()`

`call()` allows you to:

1. Immediately invoke a function.
2. Explicitly specify what `this` should refer to.
3. Pass arguments individually.

### Syntax

```js id="h3k8q5"
function.call(thisValue, arg1, arg2, arg3);
```

---

## Basic Example

```js id="n6p2v8"
const user = {
    name: "Aniket"
};

function greet() {
    console.log(this.name);
}

greet.call(user);
```

Output:

```text id="x8r4m1"
Aniket
```

Without `call()`:

```js id="q1m7s3"
greet();
```

`this` would be determined by the normal function-call rules.

With:

```js id="j5v9c2"
greet.call(user);
```

we explicitly tell JavaScript:

> "Execute `greet` with `this` set to `user`."

---

## Passing Arguments

```js id="b8x3n6"
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}

greet.call(user, 21);
```

Output:

```text id="m4q7z9"
Aniket 21
```

---

# 11. `apply()`

`apply()` works almost exactly like `call()`.

The main difference is **how arguments are passed**.

### `call()`

Arguments are passed individually:

```js id="r6n2p8"
greet.call(user, 21);
```

### `apply()`

Arguments are passed as an array-like value:

```js id="k4v9x1"
greet.apply(user, [21]);
```

---

## Example

```js id="q8m3s6"
const user = {
    name: "Aniket"
};

function greet(age, city) {
    console.log(this.name, age, city);
}

greet.apply(user, [21, "Pune"]);
```

Output:

```text id="f2w7k5"
Aniket 21 Pune
```

---

## Easy Memory Trick

```text id="a7x4p9"
call  → arguments separated by comma
apply → arguments inside array-like object
```

---

# 12. `bind()`

`bind()` is different from `call()` and `apply()`.

It does **not immediately execute the function**.

Instead, it returns a **new function** with `this` permanently bound to the specified value.

### Example

```js id="n5r8c2"
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}

const fn = greet.bind(user);

fn(21);
```

Output:

```text id="w3q6m1"
Aniket 21
```

---

## What Happens?

```js id="y8v4p2"
const fn = greet.bind(user);
```

creates a new function.

Conceptually:

```text id="b5m7x9"
greet
  │
  │ bind(user)
  ▼
new function
  │
  └── this → user
```

Then:

```js id="d3k9q6"
fn(21);
```

executes the bound function.

---

# 13. `call()` vs `apply()` vs `bind()`

This is one of the most common JavaScript interview questions.

| Method    | Executes immediately? | Arguments             | Returns         |
| --------- | --------------------: | --------------------- | --------------- |
| `call()`  |                     ✅ | Individually          | Function result |
| `apply()` |                     ✅ | Array-like            | Function result |
| `bind()`  |                     ❌ | Individually / preset | New function    |

### Example

```js id="v4x8m2"
function greet(age, city) {
    console.log(this.name, age, city);
}

const user = {
    name: "Aniket"
};
```

### `call()`

```js id="q7n3k5"
greet.call(user, 21, "Pune");
```

Immediately executes.

---

### `apply()`

```js id="m9c4r7"
greet.apply(user, [21, "Pune"]);
```

Immediately executes.

---

### `bind()`

```js id="x2p6v8"
const fn = greet.bind(user, 21, "Pune");

fn();
```

Creates a new function first.

---

## Easy Memory Trick

```text id="s4k8n1"
CALL
 ↓
Call now

APPLY
 ↓
Call now with array-like arguments

BIND
 ↓
Bind now, call later
```

---

# 14. Changing `this` with `call()`, `apply()`, and `bind()`

Given:

```js id="t8m3q6"
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}
```

---

## Using `call()`

```js id="v2x7p4"
greet.call(user, 21);
```

Output:

```text id="c9m5r1"
Aniket 21
```

---

## Using `apply()`

```js id="f6k2w8"
greet.apply(user, [21]);
```

Output:

```text id="j4n9x3"
Aniket 21
```

---

## Using `bind()`

```js id="q7v3m5"
const fn = greet.bind(user);

fn(21);
```

Output:

```text id="h8r2k6"
Aniket 21
```

---

## Complete Example

```js id="p5x9c4"
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}

greet.call(user, 21);

greet.apply(user, [21]);

const fn = greet.bind(user);

fn(21);
```

Output:

```text id="w3m7q8"
Aniket 21
Aniket 21
Aniket 21
```

---

# 15. Important `this` Traps

## Trap 1 — Losing `this`

Consider:

```js id="c8v2m6"
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

const fn = user.greet;

fn();
```

You might expect:

```text
Aniket
```

But that's not guaranteed.

The important point is that the function is no longer being called as:

```js id="j4x8p2"
user.greet();
```

Instead, it is called as:

```js id="m7q3v9"
fn();
```

So the receiver information is lost.

---

## Fix With `bind()`

```js id="r5n8c1"
const fn = user.greet.bind(user);

fn();
```

Now:

```text id="k2w6p4"
this → user
```

Output:

```text id="z9m3q7"
Aniket
```

---

# Trap 2 — Arrow Functions Cannot Be Rebound

Consider:

```js id="v6p2x8"
const greet = () => {
    console.log(this.name);
};

greet.call({
    name: "Aniket"
});
```

`call()` does not change the arrow function's lexical `this`.

The same applies to:

```js id="f4m8q1"
greet.apply(user);
```

and:

```js id="n7c3r5"
greet.bind(user)();
```

Arrow functions do not have their own dynamic `this`.

---

# Trap 3 — Extracting an Object Method

```js id="s8k4m2"
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

Works.

But:

```js id="q5v9x3"
const greet = user.greet;

greet();
```

The function is now called independently.

Therefore, it no longer gets `user` as its receiver.

Use:

```js id="m2c7p8"
const greet = user.greet.bind(user);

greet();
```

---

# 16. Real-World Examples

## Example 1 — Reusing One Function for Multiple Objects

Instead of creating separate functions:

```js id="h6w3n9"
const user1 = {
    name: "Aniket"
};

const user2 = {
    name: "Rahul"
};

function greet() {
    console.log(`Hello ${this.name}`);
}
```

We can reuse the same function:

```js id="x4p8m2"
greet.call(user1);
greet.call(user2);
```

Output:

```text id="v9q5k1"
Hello Aniket
Hello Rahul
```

The function stays the same.

Only `this` changes.

---

## Example 2 — Reusing Function With `apply()`

```js id="c3m7x8"
function introduce(age, city) {
    console.log(
        `I am ${this.name}, ${age} years old, from ${city}.`
    );
}

const user = {
    name: "Aniket"
};

introduce.apply(user, [21, "Pune"]);
```

Output:

```text id="w5r2n6"
I am Aniket, 21 years old, from Pune.
```

---

## Example 3 — Callback With `bind()`

Suppose a function will be passed somewhere else:

```js id="j8v4p1"
const user = {
    name: "Aniket",

    greet() {
        console.log(`Hello ${this.name}`);
    }
};
```

If we pass the method directly:

```js id="n6x2c9"
setTimeout(user.greet, 1000);
```

the method may lose its intended `this`.

Use:

```js id="q3m7k5"
setTimeout(user.greet.bind(user), 1000);
```

Now the function remembers:

```text id="p8v4r2"
this → user
```

---

# 17. Quick Revision

## `this`

> A special keyword whose value depends on the execution/call context.

---

## Object Method

```js id="w5m9c3"
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

```text id="r2x7k8"
this → user
```

---

## Regular Function

```js id="f4n8p2"
function greet() {
    console.log(this);
}
```

> `this` depends on how the function is called.

---

## Arrow Function

```js id="q6m3v9"
const greet = () => {
    console.log(this);
};
```

> Does not have its own `this`; inherits lexical `this`.

---

## Constructor

```js id="x8k2r5"
function User(name) {
    this.name = name;
}

const user = new User("Aniket");
```

> `this` refers to the newly created instance.

---

## Class

```js id="m5v7c1"
class User {
    constructor(name) {
        this.name = name;
    }
}
```

> `this` refers to the current instance when called as an instance method.

---

## Event Handler

```js id="p3n9x6"
button.addEventListener("click", function () {
    console.log(this);
});
```

> For a traditional DOM event listener, `this` commonly refers to the registered element.

---

## `call()`

```js id="k7r4m8"
greet.call(user, 21);
```

> Immediately calls the function with a specified `this`.

---

## `apply()`

```js id="v2x8q5"
greet.apply(user, [21]);
```

> Immediately calls the function with a specified `this` and array-like arguments.

---

## `bind()`

```js id="n4c6p1"
const fn = greet.bind(user);

fn(21);
```

> Returns a new function with `this` bound to the specified value.

---

# 18. Interview Questions

## Basic

1. What is `this` in JavaScript?
2. How is `this` determined?
3. What is `this` inside an object method?
4. What is `this` inside a regular function?
5. What is `this` inside an arrow function?
6. What is `call()`?
7. What is `apply()`?
8. What is `bind()`?
9. What is the difference between `call()` and `apply()`?
10. What is the difference between `call()` and `bind()`?

---

## Intermediate

11. Why does an arrow function not have its own `this`?
12. Why does `user.greet()` behave differently from `const fn = user.greet; fn()`?
13. How can you preserve `this` when passing an object method as a callback?
14. How does `this` work in constructor functions?
15. How does `this` work in classes?
16. How does `this` work in DOM event handlers?
17. Can `call()` change `this` of an arrow function?
18. Can `apply()` change `this` of an arrow function?
19. Can `bind()` change `this` of an arrow function?
20. What does `bind()` return?

---

## ⭐ Advanced

21. Explain `this` based on the call site.
22. Explain the difference between lexical `this` and dynamic `this`.
23. Why does extracting an object method sometimes cause `this` to be lost?
24. Explain how `new` determines `this`.
25. What happens when a bound function is called?
26. Can a bound function's `this` be changed using `call()`?
27. Why are arrow functions useful inside callbacks?
28. Why can using an arrow function as an object method cause unexpected behavior?
29. Explain the difference between `event.target` and `this` in a DOM event handler.
30. Explain `call()`, `apply()`, and `bind()` using one common example.

---

# 19. Must Remember

## ⭐ Rule 1 — Object Method

```js id="d7x3m9"
user.greet();
```

```text id="a4p8k2"
this → user
```

---

## ⭐ Rule 2 — Standalone Regular Function

```js id="q5v2n7"
greet();
```

In strict mode:

```text id="w8m3c6"
this → undefined
```

---

## ⭐ Rule 3 — Arrow Function

```js id="j2r6x9"
const greet = () => {};
```

```text id="v4k8p1"
this → inherited from surrounding lexical scope
```

---

## ⭐ Rule 4 — Constructor

```js id="m7c3q5"
new User();
```

```text id="x9n2r6"
this → newly created instance
```

---

## ⭐ Rule 5 — `call()`

```js id="p4v8m2"
greet.call(user, 21);
```

```text id="k6x3q9"
Execute now
+
Set this
+
Arguments individually
```

---

## ⭐ Rule 6 — `apply()`

```js id="n8r5c1"
greet.apply(user, [21]);
```

```text id="w2m7x4"
Execute now
+
Set this
+
Arguments as array-like value
```

---

## ⭐ Rule 7 — `bind()`

```js id="q9p3v6"
const fn = greet.bind(user);

fn(21);
```

```text id="m5x8k2"
Create new function
+
Set this
+
Execute later
```

---

# 🧠 The Most Important Mental Model

When you see:

```js id="c7r4n8"
someFunction();
```

Ask:

> **How is this function being called?**

When you see:

```js id="p2x6m9"
user.someFunction();
```

Think:

```text id="z8k3v5"
this → user
```

When you see:

```js id="f4q7n1"
someFunction.call(user);
```

Think:

```text id="y6m2c8"
this → user
execute immediately
```

When you see:

```js id="r9v3k5"
someFunction.apply(user, [args]);
```

Think:

```text id="h7x4p2"
this → user
execute immediately
arguments → array-like
```

When you see:

```js id="n5c8q1"
const fn = someFunction.bind(user);
```

Think:

```text id="m3r7v9"
new function created
this → user
execute later
```

---

# 🔥 One Example You MUST Know

```js id="x8m4p2"
const user = {
    name: "Aniket"
};

function greet(age) {
    console.log(this.name, age);
}

greet.call(user, 21);

greet.apply(user, [21]);

const fn = greet.bind(user);

fn(21);
```

### Output

```text id="q5v7n3"
Aniket 21
Aniket 21
Aniket 21
```

### Why?

```text id="k2r8c6"
call()
 │
 ├── this = user
 ├── age = 21
 └── executes immediately

apply()
 │
 ├── this = user
 ├── age = 21
 └── executes immediately

bind()
 │
 ├── creates new function
 ├── this = user
 └── executes when fn() is called
```

---

## 🎯 Phase 5 Checklist

* [ ] I can explain what `this` is.
* [ ] I understand `this` in the global context.
* [ ] I understand `this` in object methods.
* [ ] I understand `this` in regular functions.
* [ ] I understand `this` in arrow functions.
* [ ] I know why arrow functions don't have their own `this`.
* [ ] I understand `this` in constructor functions.
* [ ] I understand `this` in classes.
* [ ] I understand `this` in DOM event handlers.
* [ ] I can explain `call()`.
* [ ] I can explain `apply()`.
* [ ] I can explain `bind()`.
* [ ] I know the difference between `call()` and `apply()`.
* [ ] I know the difference between `call()` and `bind()`.
* [ ] I know that `call()`, `apply()`, and `bind()` cannot dynamically change an arrow function's lexical `this`.
* [ ] I understand how `this` can be lost when passing object methods as callbacks.
* [ ] I can solve `this` interview questions by looking at the call site.

---

**Phase 5 Status:** 🟡 Revise + Practice

**Next:** Phase 6 — Prototypes + Prototype Chain + Classes
