# JavaScript A→Z Revision

## Phase 3 — Scope + Execution ⭐⭐⭐

> **Goal:** Understand how JavaScript determines where variables are accessible and how JavaScript code is actually executed.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Intermediate → Advanced
**Interview Relevance:** Extremely High

> ⚠️ **Important:** This phase is more about understanding **how JavaScript works internally** than memorizing syntax.

---

## 📚 Contents

1. [Global Scope](#1-global-scope)
2. [Function Scope](#2-function-scope)
3. [Block Scope](#3-block-scope)
4. [Lexical Scope](#4-lexical-scope)
5. [Scope Chain](#5-scope-chain)
6. [Execution Context](#6-execution-context)
7. [Call Stack](#7-call-stack)
8. [Hoisting](#8-hoisting)
9. [Temporal Dead Zone — TDZ](#9-temporal-dead-zone--tdz)
10. [Closures](#10-closures)
11. [How Everything Connects](#11-how-everything-connects)
12. [Code → Execution Context → Scope → Call Stack → Result](#12-code--execution-context--scope--call-stack--result)
13. [Quick Revision](#13-quick-revision)
14. [Interview Questions](#14-interview-questions)
15. [Must Remember](#15-must-remember)

---

# 1. Global Scope

The **global scope** is the outermost scope in a JavaScript program.

Variables declared in the global scope can generally be accessed from other scopes inside that program.

### Example

```js
const name = "Aniket";

function greet() {
    console.log(name);
}

greet();
```

Output:

```text
Aniket
```

The function can access `name` because `name` exists in its outer scope.

### Real-World Example

Think of a **company notice board**.

Everyone inside the company can see information posted on the common notice board.

Similarly, variables in the global scope can be accessible to code inside nested scopes.

```text
Global Scope
│
├── name
├── age
│
└── Function
    └── Can access name and age
```

### Important

Avoid putting unnecessary variables in the global scope.

Too many global variables can cause:

* Name collisions
* Accidental modification
* Difficult debugging
* Unexpected dependencies

---

# 2. Function Scope

**Function scope** means a variable declared inside a function is accessible only within that function.

```js
function greet() {
    const message = "Hello";

    console.log(message);
}

greet();
```

This works.

But:

```js
function greet() {
    const message = "Hello";
}

console.log(message);
```

This causes:

```text
ReferenceError: message is not defined
```

### Real-World Example

Think of a **private office room**.

Anything inside the room is available to people inside that room, but someone outside cannot directly access it.

```text
Global Scope
│
└── greet()
    │
    └── message
```

`message` belongs to the function's scope.

---

## `var` and Function Scope

`var` is function-scoped.

```js
function test() {
    var value = 10;

    if (true) {
        var value = 20;
    }

    console.log(value);
}

test();
```

Output:

```text
20
```

The `if` block does not create a new scope for `var`.

---

# 3. Block Scope

A **block** is code surrounded by `{}`.

Examples:

```js
if (true) {
    // block
}

for (;;) {
    // block
}

{
    // block
}
```

Variables declared with `let` and `const` are **block-scoped**.

### Example

```js
if (true) {
    let age = 21;
    const name = "Aniket";

    console.log(age);
    console.log(name);
}
```

This works.

Outside the block:

```js
if (true) {
    let age = 21;
}

console.log(age);
```

Result:

```text
ReferenceError
```

### Real-World Example

Think of a **classroom**.

Information written inside one classroom is not automatically available in another classroom.

```text
Program
│
├── Block A
│   └── value
│
└── Block B
    └── cannot directly access Block A's value
```

---

## `var` vs `let` / `const`

```js
if (true) {
    var a = 10;
    let b = 20;
    const c = 30;
}

console.log(a); // 10
console.log(b); // ReferenceError
console.log(c); // ReferenceError
```

### Remember

| Keyword | Scope          |
| ------- | -------------- |
| `var`   | Function scope |
| `let`   | Block scope    |
| `const` | Block scope    |

---

# 4. Lexical Scope

**Lexical scope** means that the accessibility of variables is determined by **where the code is written**, not where the function is called.

This is one of the most important ideas in JavaScript.

### Example

```js
const name = "Aniket";

function outer() {
    function inner() {
        console.log(name);
    }

    inner();
}

outer();
```

`inner()` can access `name` because of where `inner()` is **defined**.

---

## Nested Scope

```js
const globalValue = "Global";

function outer() {
    const outerValue = "Outer";

    function inner() {
        const innerValue = "Inner";

        console.log(globalValue);
        console.log(outerValue);
        console.log(innerValue);
    }

    inner();
}

outer();
```

`inner()` can access:

```text
Global Scope
     ↓
Outer Scope
     ↓
Inner Scope
```

But `outer()` cannot directly access variables declared inside `inner()`.

```js
function outer() {
    function inner() {
        const secret = 123;
    }

    console.log(secret); // ReferenceError
}
```

### Key Rule

> **Inner scopes can access outer scopes, but outer scopes cannot access inner scopes.**

---

# 5. Scope Chain

When JavaScript tries to find a variable, it searches through the available scopes.

This is called the **scope chain**.

### Example

```js
const a = 10;

function outer() {
    const b = 20;

    function inner() {
        const c = 30;

        console.log(a);
        console.log(b);
        console.log(c);
    }

    inner();
}

outer();
```

When `inner()` looks for `a`:

```text
inner scope
    ↓
outer scope
    ↓
global scope
    ↓
a found
```

This is the **scope chain**.

---

## Variable Lookup

Suppose:

```js
const name = "Global";

function outer() {
    const name = "Outer";

    function inner() {
        const name = "Inner";

        console.log(name);
    }

    inner();
}

outer();
```

Output:

```text
Inner
```

JavaScript finds the nearest `name`.

```text
inner scope
    ↓
name = "Inner"  ← found
```

It stops searching once the variable is found.

---

## If the Variable Doesn't Exist

```js
function test() {
    console.log(username);
}

test();
```

JavaScript searches:

```text
test scope
    ↓
outer scope
    ↓
global scope
    ↓
not found
    ↓
ReferenceError
```

---

# 6. Execution Context

An **execution context** is the environment in which JavaScript code is evaluated and executed.

Whenever JavaScript executes code, it needs an execution context.

The major types you should know are:

1. Global Execution Context
2. Function Execution Context
3. Eval Execution Context

For modern JavaScript development, focus mainly on the first two.

---

## Global Execution Context

When JavaScript starts running a script, it creates a **Global Execution Context**.

Example:

```js
const name = "Aniket";

console.log(name);
```

Conceptually:

```text
Global Execution Context
│
├── Variables
│   └── name
│
└── Code Execution
    └── console.log(name)
```

---

## Function Execution Context

Whenever a function is called, JavaScript creates a new execution context for that function.

```js
function greet(name) {
    const message = `Hello ${name}`;

    console.log(message);
}

greet("Aniket");
```

Execution:

```text
Global Execution Context
        ↓
greet() called
        ↓
Function Execution Context
        ↓
message created
        ↓
console.log()
        ↓
function finishes
        ↓
context removed from stack
```

---

## Two Important Phases

An execution context can be understood using two broad phases:

### 1. Creation Phase

JavaScript prepares the environment.

It handles things such as:

* Variable bindings
* Function declarations
* Scope information
* `this` binding

### 2. Execution Phase

JavaScript executes the code line by line.

Example:

```js
console.log(name);

var name = "Aniket";
```

During the creation phase, the `var` binding exists.

During execution:

```text
name → undefined
```

So:

```text
undefined
```

is printed.

---

# 7. Call Stack

The **call stack** is a data structure used by JavaScript to keep track of function execution.

It follows:

> **LIFO — Last In, First Out**

### Example

```js
function first() {
    second();
}

function second() {
    third();
}

function third() {
    console.log("Hello");
}

first();
```

Execution:

```text
first()
   ↓
second()
   ↓
third()
```

Call stack:

```text
| third()  |
| second() |
| first()  |
| global   |
------------
```

When `third()` finishes:

```text
| second() |
| first()  |
| global   |
------------
```

Then `second()` finishes:

```text
| first() |
| global  |
-----------
```

Finally:

```text
| global |
----------
```

---

## Real-World Example

Think of a stack of plates.

You put plates on top:

```text
Plate 3
Plate 2
Plate 1
```

You remove the top plate first.

Similarly:

```text
third()
second()
first()
```

`third()` finishes first.

---

## Stack Overflow

If a function keeps calling itself without stopping:

```js
function infinite() {
    infinite();
}

infinite();
```

The call stack keeps growing until JavaScript cannot add another execution frame.

Result:

```text
RangeError: Maximum call stack size exceeded
```

---

# 8. Hoisting

**Hoisting** is the behavior where JavaScript's declarations are processed before the code executes.

However, different declarations behave differently.

---

## `var` Hoisting

```js
console.log(name);

var name = "Aniket";
```

Output:

```text
undefined
```

Conceptually:

```js
var name;

console.log(name);

name = "Aniket";
```

The declaration is available before the assignment.

---

## `let` and `const` Hoisting

`let` and `const` are also hoisted in the sense that their bindings are created during scope setup, but they cannot be accessed before initialization.

```js
console.log(name);

let name = "Aniket";
```

Result:

```text
ReferenceError
```

This happens because of the **Temporal Dead Zone**.

---

## Function Declaration Hoisting

Function declarations can be called before their declaration.

```js
greet();

function greet() {
    console.log("Hello");
}
```

Output:

```text
Hello
```

---

## Function Expression

```js
greet();

const greet = function () {
    console.log("Hello");
};
```

Result:

```text
ReferenceError
```

The variable exists as a lexical binding but cannot be accessed before initialization.

---

# 9. Temporal Dead Zone — TDZ

The **Temporal Dead Zone** is the period between entering a scope and initializing a `let`, `const`, or `class` binding.

### Example

```js
console.log(name);

let name = "Aniket";
```

The `name` binding exists, but it has not yet been initialized.

Therefore:

```text
ReferenceError
```

---

## TDZ Timeline

```js
{
    // TDZ starts

    console.log(name); // ReferenceError

    let name = "Aniket";

    // TDZ ends
}
```

Conceptually:

```text
Scope begins
     ↓
┌──────────────────────┐
│ Temporal Dead Zone   │
│ name cannot be used  │
└──────────────────────┘
     ↓
let name = "Aniket"
     ↓
Initialization
     ↓
TDZ ends
```

---

## `const`

The same applies to `const`.

```js
console.log(age);

const age = 21;
```

Result:

```text
ReferenceError
```

---

## Why TDZ Exists

The TDZ helps prevent accidental use of variables before they are properly initialized.

It also makes `let` and `const` behavior more predictable than `var`.

---

# 10. Closures

A **closure** is created when a function remembers and accesses variables from its outer lexical scope even after that outer function has finished executing.

Closures were introduced in Phase 2, but they are deeply connected to **scope** and **execution**.

### Example

```js
function counter() {
    let count = 0;

    return function () {
        count++;

        return count;
    };
}

const increment = counter();

console.log(increment());
console.log(increment());
console.log(increment());
```

Output:

```text
1
2
3
```

---

## Why Does `count` Survive?

When:

```js
const increment = counter();
```

runs:

```text
counter()
   ↓
count = 0
   ↓
inner function created
   ↓
inner function returned
```

Normally, we might expect the function's local variables to disappear after the function finishes.

But the returned function still references `count`.

Therefore, the lexical environment containing `count` remains reachable.

```text
increment()
     │
     ▼
┌─────────────────┐
│ Closure         │
│                 │
│ count = 0       │
└─────────────────┘
```

After calling:

```text
increment()
```

the closure updates:

```text
0 → 1
```

Next:

```text
1 → 2
```

Next:

```text
2 → 3
```

---

## Closure = Scope + Function

A useful mental model:

```text
Function
   +
Access to outer lexical environment
   =
Closure
```

### Common Uses

Closures are used in:

* Data privacy
* Counters
* Function factories
* Event handlers
* Callbacks
* Timers
* Memoization
* Currying
* React hooks

---

# 11. How Everything Connects

This is the most important part of Phase 3.

These concepts are not separate topics.

They work together.

```text
Scope
  ↓
Determines variable accessibility
  ↓
Lexical Scope
  ↓
Creates Scope Chain
  ↓
Execution Context
  ↓
Function calls create new contexts
  ↓
Call Stack
  ↓
Tracks those executions
  ↓
Hoisting + TDZ
  ↓
Determine when declarations can be accessed
  ↓
Closures
  ↓
Allow functions to remember outer scope
```

---

# 12. Code → Execution Context → Scope → Call Stack → Result

This is the mental model you should remember.

Consider:

```js
const name = "Aniket";

function greet() {
    const message = `Hello ${name}`;

    function printMessage() {
        console.log(message);
    }

    printMessage();
}

greet();
```

Let's understand what happens.

---

## Step 1 — Code Starts

JavaScript receives:

```js
const name = "Aniket";

function greet() {
    const message = `Hello ${name}`;

    function printMessage() {
        console.log(message);
    }

    printMessage();
}

greet();
```

---

## Step 2 — Global Execution Context

JavaScript creates the global execution context.

Conceptually:

```text
Global Execution Context
│
├── name
└── greet
```

---

## Step 3 — `greet()` Is Called

```js
greet();
```

A new function execution context is created.

```text
Global Execution Context
        ↓
greet() Execution Context
```

---

## Step 4 — `greet()` Creates `message`

```js
const message = `Hello ${name}`;
```

`greet()` can access `name` because the global scope is in its scope chain.

```text
greet scope
    ↓
global scope
    ↓
name found
```

---

## Step 5 — `printMessage()` Is Called

A new execution context is created.

```text
Global
   ↓
greet()
   ↓
printMessage()
```

Call stack:

```text
┌───────────────────┐
│ printMessage()    │
├───────────────────┤
│ greet()           │
├───────────────────┤
│ Global Context    │
└───────────────────┘
```

---

## Step 6 — `message` Is Found

`printMessage()` searches for `message`.

First:

```text
printMessage scope
```

Not found.

Then:

```text
greet scope
```

Found.

So:

```text
Hello Aniket
```

is printed.

---

## Step 7 — Function Finishes

`printMessage()` finishes and is removed from the call stack.

```text
┌───────────────────┐
│ greet()           │
├───────────────────┤
│ Global Context    │
└───────────────────┘
```

Then `greet()` finishes.

```text
┌───────────────────┐
│ Global Context    │
└───────────────────┘
```

Finally, the program finishes.

---

# 13. Quick Revision

## Global Scope

> Outermost scope of a JavaScript program.

```js
const name = "Aniket";
```

---

## Function Scope

> Variables declared with `var` inside a function are accessible within that function.

```js
function test() {
    var value = 10;
}
```

---

## Block Scope

> `let` and `const` are accessible only within their block.

```js
if (true) {
    let value = 10;
}
```

---

## Lexical Scope

> Variable accessibility is determined by where code is written.

---

## Scope Chain

> JavaScript searches from the current scope outward to find variables.

```text
Current Scope
     ↓
Outer Scope
     ↓
Global Scope
```

---

## Execution Context

> Environment in which JavaScript code is evaluated and executed.

---

## Call Stack

> LIFO data structure that tracks currently executing functions.

```text
Last In
First Out
```

---

## Hoisting

> Declarations are processed during execution-context setup before normal code execution.

Remember:

```text
var       → initialized as undefined
let       → TDZ
const     → TDZ
function  → callable before declaration
```

---

## TDZ

> Time between entering a scope and initializing a `let`, `const`, or `class` binding.

---

## Closure

> A function that retains access to its outer lexical environment.

---

# 14. Interview Questions

## Basic

1. What is scope in JavaScript?
2. What is global scope?
3. What is function scope?
4. What is block scope?
5. What is lexical scope?
6. What is the scope chain?
7. What is an execution context?
8. What is the call stack?
9. What is hoisting?
10. What is the Temporal Dead Zone?

---

## Intermediate

11. What is the difference between function scope and block scope?
12. What is the difference between `var`, `let`, and `const`?
13. Why can `var` be accessed before declaration?
14. Why does accessing `let` before initialization throw an error?
15. Are `let` and `const` hoisted?
16. Are function declarations hoisted?
17. What happens when a variable is not found in the current scope?
18. Explain the scope chain with an example.
19. What happens when a function is called?
20. What happens to an execution context after a function finishes?

---

## ⭐ Advanced

21. Explain the difference between lexical scope and dynamic scope.
22. Explain the creation and execution phases of an execution context.
23. How does the call stack work?
24. What causes a stack overflow?
25. Explain hoisting for `var`, `let`, `const`, and function declarations.
26. What exactly is the Temporal Dead Zone?
27. Why are `let` and `const` considered hoisted even though they cannot be accessed before declaration?
28. How does lexical scope create closures?
29. Why does a closure keep variables alive after the outer function finishes?
30. Explain the relationship between execution context, lexical environment, and scope chain.

---

# 15. Must Remember

Before moving to the next phase, you should be able to explain this entire flow without looking at your notes:

```text
                    JavaScript Code
                           │
                           ▼
                 Execution Context
                           │
                           ▼
                     Lexical Scope
                           │
                           ▼
                      Scope Chain
                           │
                           ▼
                    Variable Lookup
                           │
                           ▼
                      Function Call
                           │
                           ▼
                      Call Stack
                           │
                           ▼
                    Code Execution
                           │
                           ▼
                         Result
```

---

## 🔥 The Most Important Relationships

### 1. Scope

```text
Scope → Where a variable can be accessed
```

### 2. Lexical Scope

```text
Where the code is written
        ↓
Determines its scope
```

### 3. Scope Chain

```text
Current scope
      ↓
Outer scope
      ↓
Global scope
```

### 4. Execution Context

```text
Creates the environment required to execute code
```

### 5. Call Stack

```text
Tracks function execution
```

### 6. Hoisting

```text
Declarations are processed during context setup
```

### 7. TDZ

```text
let / const
     ↓
Scope entered
     ↓
TDZ
     ↓
Initialization
     ↓
Variable accessible
```

### 8. Closure

```text
Function
    +
Outer lexical environment
    ↓
Closure
```

---

# 🧠 Final Mental Model

When JavaScript runs code, think:

```text
CODE
 │
 ▼
Create Execution Context
 │
 ├── Set up scope
 ├── Process declarations
 └── Prepare variables/functions
 │
 ▼
Execute Code
 │
 ▼
Function Called?
 │
 ├── YES
 │    ↓
 │  Create Function Execution Context
 │    ↓
 │  Push onto Call Stack
 │    ↓
 │  Execute Function
 │    ↓
 │  Search Scope Chain when variables are needed
 │    ↓
 │  Function finishes
 │    ↓
 │  Remove from Call Stack
 │
 └── NO
      ↓
    Continue execution
 │
 ▼
RESULT
```

> **Core idea:** JavaScript doesn't simply "run code from top to bottom." It creates execution contexts, manages lexical scopes, resolves variables through scope chains, and uses the call stack to manage function execution.

---

## 🎯 Phase 3 Checklist

* [ ] I can explain global scope.
* [ ] I can explain function scope.
* [ ] I can explain block scope.
* [ ] I understand `var` vs `let` vs `const`.
* [ ] I understand lexical scope.
* [ ] I can explain the scope chain.
* [ ] I understand execution context.
* [ ] I understand the call stack.
* [ ] I can explain hoisting.
* [ ] I understand the TDZ.
* [ ] I can explain closures without memorizing the definition.
* [ ] I can trace code through the call stack.
* [ ] I can explain why a variable is accessible or inaccessible.
* [ ] I can explain **Code → Execution Context → Scope → Call Stack → Result**.

---

**Phase 3 Status:** 🟡 Revise + Trace Code

**Next:** Phase 4 — `this` + Objects + Prototypes
