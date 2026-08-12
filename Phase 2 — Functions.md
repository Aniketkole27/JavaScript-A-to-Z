<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 2 — Functions ⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-2%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Interviews%20%2B%20React-red" />
</p>

> Extremely important for interviews and React. Spend extra time on **Closures** at the end.

## 📑 Contents
- [1. Function Declaration vs Expression](#1-function-declaration-vs-expression)
- [2. Arrow Functions](#2-arrow-functions)
- [3. Parameters, Default & Rest](#3-parameters-default--rest)
- [4. Spread Operator](#4-spread-operator)
- [5. Callback Functions](#5-callback-functions)
- [6. Higher-Order Functions](#6-higher-order-functions)
- [7. Pure Functions](#7-pure-functions)
- [8. First-Class Functions](#8-first-class-functions)
- [9. IIFE](#9-iife)
- [10. Recursion](#10-recursion)
- [11. Closures ⭐](#11-closures-)
- [🧪 Self-Test](#-self-test)

---

## 1. Function Declaration vs Expression

**Understand**
A **declaration** is hoisted fully (usable before defined). An **expression** assigns a function to a variable — only the variable is hoisted, not the function body.

**Code**
```js
sayHi(); // ✅ "Hi" — declarations are fully hoisted
function sayHi() { console.log("Hi"); }

sayBye(); // ❌ TypeError: sayBye is not a function
var sayBye = function () { console.log("Bye"); };
```

**Explain**
- Function declarations are hoisted **with their body**.
- Function expressions are hoisted **only as a variable** (`undefined` for `var`, TDZ for `let`/`const`) — the assignment happens at runtime.

<details>
<summary>❓ Revisit</summary>

```js
console.log(typeof greet);
var greet = function () { return "hi"; };
```
```
"undefined"   // var is hoisted, function assignment isn't yet
```
</details>

---

## 2. Arrow Functions

**Understand**
Shorter syntax. **No own `this`, `arguments`, or `prototype`** — arrow functions inherit `this` lexically from the enclosing scope.

**Code**
```js
const add = (a, b) => a + b;              // implicit return
const square = x => x * x;                 // single param, no parens needed
const greet = () => { return "hello"; };     // block body needs explicit return

const obj = {
  name: "Bhavin",
  normalFn: function () { console.log(this.name); },  // "Bhavin"
  arrowFn: () => { console.log(this.name); }            // undefined — `this` from outer scope
};
obj.normalFn();
obj.arrowFn();
```

**Explain**
- Use **normal functions** for object methods / anything needing dynamic `this`.
- Use **arrow functions** for callbacks where you want `this` to stay bound to the surrounding context (very common in React class methods, `setTimeout`, array callbacks).
- Arrow functions **cannot** be used as constructors (`new ArrowFn()` → error) and have no `arguments` object.

<details>
<summary>❓ Revisit</summary>

```js
const timer = {
  seconds: 0,
  start() {
    setInterval(() => { console.log(++this.seconds); }, 1000); // arrow: `this` = timer
  },
  startBroken() {
    setInterval(function () { console.log(++this.seconds); }, 1000); // normal: `this` = undefined/window
  }
};
```
```
start()        -> logs 1, 2, 3...   correctly increments timer.seconds
startBroken()   -> TypeError or NaN, `this` isn't `timer` inside a plain function callback
```
</details>

---

## 3. Parameters, Default & Rest

**Understand**
- **Default parameters**: fallback value if `undefined` is passed.
- **Rest parameters**: collect remaining arguments into a real array.

**Code**
```js
function greet(name = "Guest") {
  console.log(`Hello, ${name}`);
}
greet();          // Hello, Guest
greet("Bhavin");   // Hello, Bhavin
greet(undefined);  // Hello, Guest -> default triggers only on undefined
greet(null);        // Hello, null -> default does NOT trigger on null

function sum(...nums) {
  return nums.reduce((acc, n) => acc + n, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

function logAll(first, ...rest) {
  console.log(first, rest);
}
logAll(1, 2, 3, 4); // 1 [2, 3, 4]
```

**Explain**
- Default params trigger **only** for `undefined` — not for `null`, `0`, or `""`.
- Rest params must be the **last** parameter, and gather args into a real `Array` (unlike the old `arguments` object, which is array-*like*).

<details>
<summary>❓ Revisit</summary>

```js
function test(a = 10, b = a + 5) { return [a, b]; }
console.log(test());        // ?
console.log(test(1));       // ?
console.log(test(1, 2));    // ?
```
```
[10, 15]   // default params can reference earlier params
[1, 6]
[1, 2]
```
</details>

---

## 4. Spread Operator

**Understand**
`...` **expands** an iterable into individual elements — the visual opposite of rest (which **collects**).

**Code**
```js
// Arrays
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

// Copy (shallow)
const copy = [...arr1];

// Merge objects
const defaults = { theme: "dark", lang: "en" };
const userPrefs = { lang: "hi" };
const final = { ...defaults, ...userPrefs };
console.log(final); // { theme: "dark", lang: "hi" } -> later keys override

// Function args
function multiply(a, b, c) { return a * b * c; }
console.log(multiply(...[2, 3, 4])); // 24
```

**Explain**
- Spread creates a **shallow copy** — nested objects/arrays are still shared by reference.
- In object spread, **later keys win** (left-to-right merge order).
- Same `...` syntax as rest — direction (collect vs expand) depends on context (function param vs literal).

<details>
<summary>❓ Revisit</summary>

```js
const original = { a: 1, nested: { x: 10 } };
const clone = { ...original };
clone.nested.x = 99;
console.log(original.nested.x); // ?
```
```
99   // shallow copy -> nested object still shared
```
</details>

---

## 5. Callback Functions

**Understand**
A function passed as an argument to another function, to be **called later**.

**Code**
```js
function processData(data, callback) {
  const result = data.toUpperCase();
  callback(result);
}
processData("hello", (res) => console.log(res)); // HELLO

// Real world: array methods, setTimeout, event listeners
[1, 2, 3].forEach(n => console.log(n * 2));
setTimeout(() => console.log("done"), 1000);
```

**Explain**
Callbacks power async patterns (before Promises/async-await existed) and are still everywhere in array methods, event handlers, and Node.js APIs.

---

## 6. Higher-Order Functions

**Understand**
A function that **takes a function as an argument**, or **returns a function**.

**Code**
```js
// Takes a function
function repeat(n, action) {
  for (let i = 0; i < n; i++) action(i);
}
repeat(3, i => console.log("Run", i));

// Returns a function
function multiplier(factor) {
  return function (num) { return num * factor; };
}
const double = multiplier(2);
console.log(double(5)); // 10

// Built-ins: map, filter, reduce are all HOFs
const nums = [1, 2, 3, 4];
console.log(nums.map(n => n * 2));           // [2,4,6,8]
console.log(nums.filter(n => n % 2 === 0));  // [2,4]
console.log(nums.reduce((a, b) => a + b, 0)); // 10
```

**Explain**
HOFs are possible because JS treats functions as **first-class citizens** (see §8). `map`/`filter`/`reduce` are the HOF trio you'll use constantly in React.

---

## 7. Pure Functions

**Understand**
Same input **always** produces same output, and **no side effects** (doesn't mutate external state).

**Code**
```js
// ❌ Impure — mutates external state
let total = 0;
function addImpure(n) { total += n; return total; }

// ✅ Pure — no external dependency, no mutation
function addPure(a, b) { return a + b; }

// ❌ Impure — mutates input array
function pushImpure(arr, item) { arr.push(item); return arr; }

// ✅ Pure — returns new array
function pushPure(arr, item) { return [...arr, item]; }
```

**Explain**
Pure functions are predictable, testable, and safe to memoize/parallelize — this is exactly why React state updates should be pure (`setState(prev => prev + 1)` pattern).

---

## 8. First-Class Functions

**Understand**
Functions are treated like any other value: can be **stored in variables**, **passed as arguments**, **returned from functions**, **stored in arrays/objects**.

**Code**
```js
const fn = function () { return "I'm a value"; }; // stored in a variable
const arr = [fn, () => "another"];                  // stored in an array
const obj = { greet: () => "hi" };                   // stored as object property

function callIt(f) { return f(); }
console.log(callIt(fn)); // "I'm a value"
```

**Explain**
This is *why* callbacks, HOFs, and closures are possible in JS — functions aren't a special syntax-only construct, they're regular values.

---

## 9. IIFE

**Understand**
**I**mmediately **I**nvoked **F**unction **E**xpression — runs the moment it's defined, creating a private scope.

**Code**
```js
(function () {
  const secret = "hidden";
  console.log("IIFE ran:", secret);
})();

// Arrow version
(() => console.log("Arrow IIFE"))();

// Module pattern — classic pre-ES6 way to avoid polluting global scope
const counterModule = (function () {
  let count = 0;
  return {
    increment: () => ++count,
    get: () => count
  };
})();
console.log(counterModule.increment()); // 1
console.log(counterModule.increment()); // 2
```

**Explain**
Used historically to avoid leaking variables into the global scope before ES6 modules/`let`/`const` existed. Still useful for one-time setup or creating a private closure scope.

---

## 10. Recursion

**Understand**
A function that **calls itself**, with a **base case** to stop.

**Code**
```js
function factorial(n) {
  if (n <= 1) return 1;        // base case
  return n * factorial(n - 1); // recursive case
}
console.log(factorial(5)); // 120

// Traversing nested structures — a common real use
function flatten(arr) {
  return arr.reduce((flat, item) =>
    Array.isArray(item) ? flat.concat(flatten(item)) : flat.concat(item), []);
}
console.log(flatten([1, [2, [3, [4, 5]]]])); // [1,2,3,4,5]
```

**Explain**
Every recursive call adds a **stack frame**. Missing/wrong base case → `RangeError: Maximum call stack size exceeded`. Prefer recursion for naturally nested data (trees, JSON, DOM), iteration for simple counting loops.

<details>
<summary>❓ Revisit</summary>

```js
function sumTo(n) {
  if (n === 0) return 0;
  return n + sumTo(n - 1);
}
console.log(sumTo(5)); // ?
```
```
15   // 5+4+3+2+1+0
```
</details>

---

## 11. Closures ⭐

**Understand**
A closure is a function that **remembers the variables from its outer scope**, even after the outer function has finished executing.

**Code**
```js
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const increment = counter();
console.log(increment()); // 1
console.log(increment()); // 2
console.log(increment()); // 3

const increment2 = counter(); // fresh, independent closure
console.log(increment2()); // 1
```

**Explain — why does `count` survive?**
- When `counter()` runs, JS creates a new **execution context** with its own `count` variable.
- The inner function keeps a **reference** to that outer scope (its **lexical environment**) — not a copy.
- Normally, `counter()`'s scope would be garbage-collected once it returns. But because the returned inner function still references `count`, JS keeps that scope **alive in memory**.
- Each call to `counter()` creates a **brand-new, independent** `count` — that's why `increment` and `increment2` don't interfere with each other.

**Recall — closures also explain:**
```js
// Private variables (data hiding / encapsulation)
function createBankAccount(balance) {
  return {
    deposit: (amt) => balance += amt,
    withdraw: (amt) => balance -= amt,
    getBalance: () => balance
  };
}
const acc = createBankAccount(100);
acc.deposit(50);
console.log(acc.getBalance()); // 150
console.log(acc.balance);       // undefined -> truly private, no direct access

// The classic loop bug
function buggyLoop() {
  for (var i = 1; i <= 3; i++) {
    setTimeout(() => console.log(i), 100); // 4 4 4 -> var is shared, closure captures final value
  }
}
function fixedLoop() {
  for (let i = 1; i <= 3; i++) {
    setTimeout(() => console.log(i), 100); // 1 2 3 -> let creates new binding per iteration
  }
}
```

<details>
<summary>❓ Revisit</summary>

```js
function makeMultiplier(x) {
  return function (y) {
    return x * y;
  };
}
const double = makeMultiplier(2);
const triple = makeMultiplier(3);
console.log(double(5)); // ?
console.log(triple(5)); // ?
console.log(double(10)); // ?
```
```
10   // double remembers x=2
15   // triple remembers x=3, independent closure
20   // double still remembers x=2
```
</details>

---

## 🧪 Self-Test
Mix of Phase 2 concepts.

```js
function outer() {
  let x = 10;
  function inner() {
    x++;
    return x;
  }
  return inner;
}
const fn = outer();
console.log(fn());
console.log(fn());

const arrowObj = {
  val: 42,
  getValNormal: function () { return this.val; },
  getValArrow: () => { return this.val; }
};
console.log(arrowObj.getValNormal());
console.log(arrowObj.getValArrow());

console.log([1, 2, 3].map(n => n * n));
```

<details>
<summary>Show answers</summary>

```
11              // closure remembers x, increments each call
12
42              // normal function -> `this` = arrowObj
undefined       // arrow function -> `this` from outer (module/global) scope
[1, 4, 9]
```
</details>

---

<p align="center"><i>Next → <code>03-this-prototypes.md</code></i></p>
