<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 3 — Scope + Execution ⭐⭐⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-3%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Core%20Mental%20Model-critical" />
</p>

> This is where your JS understanding becomes **strong**. More important than memorizing 100 array methods.
> Mental model for every snippet: **Code → Execution Context → Scope → Call Stack → Result**

## 📑 Contents
- [1. Global Scope](#1-global-scope)
- [2. Function Scope](#2-function-scope)
- [3. Block Scope](#3-block-scope)
- [4. Lexical Scope](#4-lexical-scope)
- [5. Scope Chain](#5-scope-chain)
- [6. Execution Context](#6-execution-context)
- [7. Call Stack](#7-call-stack)
- [8. Hoisting](#8-hoisting)
- [9. TDZ (Temporal Dead Zone)](#9-tdz-temporal-dead-zone)
- [10. Closures — quick recap](#10-closures--quick-recap)
- [🧩 Putting It All Together](#-putting-it-all-together)
- [🧪 Self-Test](#-self-test)

---

## 1. Global Scope

**Understand**
Variables declared outside any function/block. Accessible **everywhere** in the file. In browsers, `var`/function globals attach to the `window` object; `let`/`const` do not.

**Code**
```js
var globalVar = "I'm global (var)";
let globalLet = "I'm global (let)";

function show() {
  console.log(globalVar); // accessible
  console.log(globalLet);  // accessible
}
show();

console.log(window.globalVar); // "I'm global (var)"  -> attached to window
console.log(window.globalLet); // undefined            -> let/const don't attach
```

**Explain**
Too many globals = **global namespace pollution**, a common source of naming collisions in large apps. Modern JS (modules, closures, block scope) exists partly to avoid this.

---

## 2. Function Scope

**Understand**
Variables declared with `var` inside a function are visible **anywhere in that function**, regardless of nested blocks.

**Code**
```js
function demo() {
  if (true) {
    var insideBlock = "still function-scoped";
  }
  console.log(insideBlock); // ✅ accessible — var ignores block boundaries
}
demo();
console.log(typeof insideBlock); // "undefined" -> not accessible outside the function
```

**Explain**
`var` respects only **function** boundaries, never `{}` blocks (`if`, `for`, `while`). This is exactly why `let`/`const` were introduced.

---

## 3. Block Scope

**Understand**
`let`/`const` are scoped to the nearest enclosing `{}` — `if`, `for`, `while`, or a bare block.

**Code**
```js
{
  let blockScoped = "only visible in this block";
  const alsoBlock = "same here";
}
console.log(typeof blockScoped); // "undefined"

for (let i = 0; i < 3; i++) {
  // i is a NEW binding each iteration, scoped to this block
}
console.log(typeof i); // "undefined" -> i doesn't leak out
```

**Explain**
Block scope makes code more predictable — variables die exactly where you'd visually expect (`}`), unlike `var` which leaks upward to the function.

---

## 4. Lexical Scope

**Understand**
A function's scope is determined by **where it is written in the code**, not where/how it is called. "Lexical" = fixed at write-time.

**Code**
```js
function outer() {
  const name = "Outer";
  function inner() {
    console.log(name); // inner can see outer's variables — written inside outer
  }
  inner();
}
outer(); // "Outer"

function a() {
  const x = 1;
  b(); // b is NOT lexically inside a, so it can't see x
}
function b() {
  console.log(typeof x); // "undefined" -> b's lexical parent is global, not a
}
a();
```

**Explain**
Lexical scope is decided by the **physical nesting of code**, resolved once at write-time. This is the foundation closures are built on.

---

## 5. Scope Chain

**Understand**
When a variable isn't found in the current scope, JS looks **outward**, one level at a time, until it hits global scope. If not found anywhere → `ReferenceError`.

**Code**
```js
const level1 = "L1";

function levelTwo() {
  const level2 = "L2";

  function levelThree() {
    const level3 = "L3";
    console.log(level1, level2, level3); // resolves outward: L3 own -> L2 parent -> L1 global
  }
  levelThree();
}
levelTwo();

function broken() {
  console.log(notDefinedAnywhere); // ReferenceError — chain exhausted, hits global, not found
}
```

**Explain**
The scope chain is a **one-way street** — inner scopes can read outer variables, but outer scopes can **never** read inner ones.

<details>
<summary>❓ Revisit</summary>

```js
let x = "global";
function a() {
  let x = "a-scope";
  function b() {
    console.log(x); // ?
  }
  b();
}
a();
```
```
"a-scope"   // b looks in its own scope (empty) -> parent a() -> finds x there, stops
```
</details>

---

## 6. Execution Context

**Understand**
The environment created every time code runs. Two phases:
1. **Creation phase** — hoisting happens; variables (`var`→`undefined`), function declarations, and `this` get set up.
2. **Execution phase** — code runs line by line, values get assigned.

**Recall**

| Type | Created when |
|---|---|
| **Global Execution Context (GEC)** | once, when the script starts |
| **Function Execution Context (FEC)** | every time a function is called |
| **Eval Execution Context** | inside `eval()` (rare, avoid) |

**Code**
```js
console.log(a); // undefined -> creation phase already hoisted `a`
var a = 5;
greet();          // works -> function declarations fully hoisted in creation phase

function greet() {
  console.log("hi");
}
```

**Explain**
Every function call = a **brand new execution context**, with its own local variables, its own `this`, and its own `arguments`. This is why recursive calls don't overwrite each other's variables.

---

## 7. Call Stack

**Understand**
JS is single-threaded — it uses a **stack (LIFO — last in, first out)** to track which execution context is currently running.

**Code**
```js
function first() {
  console.log("first start");
  second();
  console.log("first end");
}
function second() {
  console.log("second start");
  third();
  console.log("second end");
}
function third() {
  console.log("third");
}
first();

// Call stack order:
// push: first()
//   push: second()
//     push: third()
//     pop:  third()
//   pop:  second()
// pop:  first()
```
Output:
```
first start
second start
third
second end
first end
```

**Explain**
Each function call **pushes** a new frame onto the stack; when it returns, that frame is **popped**. Infinite/unterminated recursion → stack overflow (`RangeError: Maximum call stack size exceeded`).

---

## 8. Hoisting

**Understand**
During the creation phase of an execution context, declarations are registered in memory **before** any code runs.

**Code**
```js
console.log(fnDecl());  // ✅ "I'm hoisted" -> full function hoisted
console.log(typeof fnExpr); // "undefined"  -> only var is hoisted, not the assignment
console.log(typeof arrowFn); // "undefined"

function fnDecl() { return "I'm hoisted"; }
var fnExpr = function () { return "not hoisted"; };
var arrowFn = () => "not hoisted";
```

**Explain**
- **Function declarations** → hoisted fully (name + body).
- **`var`** → hoisted, initialized to `undefined`.
- **`let`/`const`** → hoisted, but **not initialized** (stuck in TDZ, see next).
- **Classes** → hoisted like `let`/`const` — in TDZ until defined.

---

## 9. TDZ (Temporal Dead Zone)

**Understand**
The period between entering a scope and the line where a `let`/`const`/`class` is actually declared. Accessing the variable during this window throws.

**Code**
```js
{
  console.log(typeof x); // "undefined" -> var, safe (hoisted + initialized)
  var x = 1;
}
{
  console.log(typeof y); // ReferenceError -> y is in TDZ (let)
  let y = 2;
}

class Person {}
function test() {
  console.log(Employee); // ReferenceError -> classes are TDZ too
  class Employee extends Person {}
}
```

**Explain**
TDZ exists to catch bugs early — using a variable before its declaration line is almost always a mistake, so `let`/`const` fail loudly instead of silently returning `undefined` like `var` does.

---

## 10. Closures — quick recap

*(Full deep-dive was Phase 2 — this is the scope-chain lens on it.)*

**Understand**
A closure = a function **+** the lexical scope it was created in, kept alive even after the outer function returns — because the scope chain still points to it.

**Code**
```js
function outer() {
  let secret = "🔒";
  return function inner() {
    return secret; // inner's scope chain: own scope -> outer's scope -> global
  };
}
const getSecret = outer();
console.log(getSecret()); // "🔒" -> outer() finished long ago, but its scope survives
```

**Explain**
Closures work *because of* the scope chain — the inner function's chain still includes its creator's variable environment, so garbage collection can't clean it up while a reference exists.

---

## 🧩 Putting It All Together

**The full mental trace for any snippet:**

```js
function multiplyBy(factor) {
  return function (num) {
    return num * factor;
  };
}
const double = multiplyBy(2);
console.log(double(5));
```

| Step | What happens |
|---|---|
| **Code** | JS parses the script top to bottom |
| **Execution Context** | GEC created → creation phase hoists `multiplyBy`, `double` |
| **multiplyBy(2) called** | New FEC pushed → `factor = 2` in its own scope |
| **Scope** | Returned inner function's lexical scope chain: own → `multiplyBy`'s scope (`factor=2`) → global |
| **Call Stack** | `multiplyBy(2)` pushed → runs → returns inner fn → popped |
| **double(5) called** | New FEC pushed → looks up `factor` via scope chain → finds `2` |
| **Result** | `10` |

---

## 🧪 Self-Test

```js
function a() {
  console.log(b); // ?
  let b = 10;
}
a();

var x = 1;
function outer() {
  console.log(x); // ?
  var x = 2;
  console.log(x); // ?
}
outer();

function makeCounter() {
  let count = 0;
  return () => ++count;
}
const c1 = makeCounter();
const c2 = makeCounter();
console.log(c1(), c1(), c2()); // ?
```

<details>
<summary>Show answers</summary>

```
ReferenceError     // TDZ — b declared with let, accessed before its line

undefined           // function-scoped x shadows global x, hoisted as undefined
2                    // now assigned

1 2 1                // c1 and c2 have independent closures/counts
```
</details>

---

<p align="center"><i>Next → <code>04-this-prototypes-classes.md</code></i></p>
