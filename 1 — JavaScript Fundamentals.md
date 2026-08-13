# JavaScript Revision — Phase 1: Fundamentals

> **Goal:** Understand → Recall → Code → Explain → Revisit

---

# 1. Variables

Variables store data that can be used and changed during program execution.

## `var`

* Function scoped
* Can be redeclared
* Can be reassigned
* Hoisted with `undefined`

```js
var age = 20;
var age = 21; // allowed

age = 22; // allowed
```

## `let`

* Block scoped
* Can be reassigned
* Cannot be redeclared
* Hoisted but remains in TDZ

```js
let age = 20;

age = 21; // allowed
```

## `const`

* Block scoped
* Cannot be reassigned
* Cannot be redeclared
* Must be initialized

```js
const age = 20;

// age = 21; // TypeError
```

### Quick rule

```text
const → default choice
let   → when reassignment is needed
var   → generally avoid in modern JavaScript
```

---

# 2. Scope

Scope determines **where a variable can be accessed**.

Main types:

```text
Global Scope
Function Scope
Block Scope
Module Scope
```

## Global Scope

```js
const name = "Aniket";

function greet() {
  console.log(name);
}

greet();
```

## Function Scope

```js
function test() {
  var message = "Hello";

  console.log(message);
}

test();

// console.log(message); // ReferenceError
```

## Block Scope

`let` and `const` are block scoped.

```js
{
  let age = 21;
  const name = "Aniket";

  console.log(age);
}

// console.log(age); // ReferenceError
```

### Remember

```text
var   → function scoped
let   → block scoped
const → block scoped
```

---

# 3. Hoisting

Hoisting is JavaScript's behavior where declarations are processed before code execution.

## `var`

```js
console.log(age);

var age = 21;
```

Output:

```text
undefined
```

Conceptually:

```js
var age;

console.log(age);

age = 21;
```

## `let` and `const`

```js
console.log(age);

let age = 21;
```

Output:

```text
ReferenceError
```

They are hoisted but cannot be accessed before initialization.

---

# 4. Temporal Dead Zone (TDZ)

TDZ is the period between entering a scope and initializing a `let` or `const` variable.

```js
console.log(age); // ReferenceError

let age = 21;
```

### Remember

```text
var         → hoisted + initialized as undefined
let/const   → hoisted + TDZ
```

---

# 5. Data Types

JavaScript data types are divided into:

```text
Primitive
Reference
```

## Primitive Types

JavaScript has 7 primitive types:

```text
string
number
boolean
undefined
null
symbol
bigint
```

Example:

```js
const name = "Aniket";       // string
const age = 21;             // number
const loggedIn = true;      // boolean
let value;                  // undefined
const data = null;          // null
const id = Symbol("id");    // symbol
const big = 123n;           // bigint
```

---

# 6. Objects

An object stores data as key-value pairs.

```js
const user = {
  name: "Aniket",
  age: 21
};

console.log(user.name);
```

Output:

```text
Aniket
```

Access using bracket notation:

```js
console.log(user["age"]);
```

Modify:

```js
user.age = 22;
```

Add:

```js
user.city = "Nashik";
```

Delete:

```js
delete user.city;
```

---

# 7. Arrays

An array stores an ordered collection of values.

```js
const fruits = ["Apple", "Mango", "Banana"];

console.log(fruits[0]);
```

Output:

```text
Apple
```

Arrays are zero-indexed:

```text
Index:   0        1        2
       Apple    Mango    Banana
```

Arrays can contain different data types:

```js
const data = ["Aniket", 21, true, null];
```

---

# 8. Primitive vs Reference

## Primitive

Primitive values are copied by value.

```js
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
console.log(b); // 20
```

## Reference

Objects and arrays are reference values.

```js
const user1 = {
  name: "Aniket"
};

const user2 = user1;

user2.name = "Rahul";

console.log(user1.name);
```

Output:

```text
Rahul
```

Both variables refer to the same object.

### Remember

```text
Primitive  → copied by value
Object     → reference to the same object
Array      → reference to the same array
```

---

# 9. Arithmetic Operators

Used for mathematical operations.

```js
const a = 10;
const b = 3;

console.log(a + b);  // 13
console.log(a - b);  // 7
console.log(a * b);  // 30
console.log(a / b);  // 3.333...
console.log(a % b);  // 1
console.log(a ** b); // 1000
```

Increment/decrement:

```js
let count = 10;

count++;
count--;

console.log(count);
```

---

# 10. Comparison Operators

Used to compare values and return a boolean.

```js
console.log(10 > 5);   // true
console.log(10 < 5);   // false
console.log(10 >= 10); // true
console.log(10 <= 5);  // false
```

Equality:

```js
console.log(5 == "5");  // true
console.log(5 === "5"); // false
```

---

# 11. Logical Operators

Used to combine or reverse conditions.

## AND `&&`

Returns true when both conditions are truthy.

```js
const age = 21;
const hasID = true;

console.log(age >= 18 && hasID);
// true
```

## OR `||`

Returns the first truthy value.

```js
const username = "";

const name = username || "Guest";

console.log(name);
// Guest
```

## NOT `!`

Reverses a boolean value.

```js
console.log(!true);
// false
```

---

# 12. Assignment Operators

Used to assign or update values.

```js
let count = 10;

count += 5; // 15
count -= 2; // 13
count *= 2; // 26
count /= 2; // 13
count %= 5; // 3
```

Equivalent to:

```js
count += 5;

// same as
count = count + 5;
```

---

# 13. Ternary Operator

A short form of `if...else`.

### Syntax

```js
condition ? valueIfTrue : valueIfFalse
```

Example:

```js
const age = 21;

const result = age >= 18
  ? "Adult"
  : "Minor";

console.log(result);
```

Output:

```text
Adult
```

### Use when

The condition is simple.

Avoid deeply nested ternaries.

---

# 14. Nullish Coalescing `??`

Returns the right-hand value when the left-hand value is:

```text
null
undefined
```

```js
const username = null;

const name = username ?? "Guest";

console.log(name);
// Guest
```

### `??` vs `||`

```js
const count = 0;

console.log(count || 10); // 10
console.log(count ?? 10); // 0
```

`||` considers `0`, `false`, and `""` as falsy.

`??` only checks `null` and `undefined`.

---

# 15. Optional Chaining `?.`

Safely accesses properties that might not exist.

Without optional chaining:

```js
const user = {};

console.log(user.profile.name);
// TypeError
```

With optional chaining:

```js
console.log(user.profile?.name);
// undefined
```

### Nested example

```js
const user = {
  profile: {
    address: {
      city: "Nashik"
    }
  }
};

console.log(user.profile?.address?.city);
// Nashik
```

### Function call

```js
const user = {};

user.login?.();
```

No error occurs if `login` doesn't exist.

---

# 16. Type Conversion

Type conversion means converting one data type into another.

Two types:

```text
Explicit Conversion
Implicit Coercion
```

---

# 17. Explicit Conversion

We manually convert the type.

## String → Number

```js
const value = "100";

const number = Number(value);

console.log(number);
console.log(typeof number);
```

```text
100
number
```

## Number → String

```js
const value = 100;

const string = String(value);

console.log(typeof string);
// string
```

## Value → Boolean

```js
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
```

---

# 18. Implicit Type Coercion

JavaScript automatically converts types during some operations.

```js
console.log("5" + 2);
// "52"
```

Because `+` with a string performs concatenation.

But:

```js
console.log("5" - 2);
// 3
```

Here `"5"` is converted to a number.

### Important examples

```js
console.log(1 + "2");      // "12"
console.log("5" - 2);      // 3
console.log(true + 1);     // 2
console.log(false + 1);    // 1
console.log(null + 1);     // 1
console.log(undefined + 1); // NaN
```

---

# 19. `==` vs `===`

## `==` — Loose Equality

Allows type coercion.

```js
console.log(5 == "5");
// true
```

## `===` — Strict Equality

Checks both value and type.

```js
console.log(5 === "5");
// false
```

### Remember

```text
==  → value comparison + coercion
=== → value + type comparison
```

### Best practice

Prefer:

```js
===
```

in most cases.

---

# 20. Truthy and Falsy

JavaScript converts values to boolean when used in a condition.

## Falsy values

```text
false
0
-0
0n
""
null
undefined
NaN
```

Everything else is generally truthy.

Example:

```js
if ("hello") {
  console.log("Truthy");
}

if (0) {
  console.log("Won't execute");
}
```

### Important

Empty arrays and objects are truthy:

```js
Boolean([]); // true
Boolean({}); // true
```

---

# 21. `null` vs `undefined`

## `undefined`

Usually means a value has not been assigned.

```js
let username;

console.log(username);
// undefined
```

## `null`

Represents an intentional absence of a value.

```js
const selectedUser = null;
```

### Difference

```text
undefined → value is not assigned
null      → intentionally no value
```

Interesting JavaScript behavior:

```js
typeof null;
// "object"
```

This is a historical JavaScript quirk.

---

# 22. `typeof`

`typeof` returns the type of a value.

```js
typeof "hello";   // "string"
typeof 10;        // "number"
typeof true;      // "boolean"
typeof undefined; // "undefined"
typeof null;      // "object"
typeof {};        // "object"
typeof [];        // "object"
typeof Symbol();  // "symbol"
typeof 10n;       // "bigint"
```

For arrays, use:

```js
Array.isArray([]);
```

Output:

```text
true
```

---

# 23. Quick Revision Table

| Concept     | Remember                                               |
| ----------- | ------------------------------------------------------ |
| `var`       | Function scoped                                        |
| `let`       | Block scoped + reassignable                            |
| `const`     | Block scoped + not reassignable                        |
| Hoisting    | Declarations processed before execution                |
| TDZ         | `let`/`const` cannot be accessed before initialization |
| Primitive   | Copied by value                                        |
| Object      | Reference value                                        |
| `==`        | Loose equality                                         |
| `===`       | Strict equality                                        |
| `??`        | Checks `null` / `undefined`                            |
| `?.`        | Safe property access                                   |
| Truthy      | Behaves like `true`                                    |
| Falsy       | Behaves like `false`                                   |
| `null`      | Intentional absence                                    |
| `undefined` | No assigned value                                      |

---

# 24. Output Prediction Practice

Before running these, predict the output.

### 1

```js
console.log(1 + "2");
```

### 2

```js
console.log(true + 1);
```

### 3

```js
console.log(null == undefined);
console.log(null === undefined);
```

### 4

```js
console.log(Boolean([]));
console.log(Boolean({}));
console.log(Boolean(""));
console.log(Boolean(0));
```

### 5

```js
console.log(typeof null);
console.log(typeof []);
```

### 6

```js
var a = 10;

{
  var a = 20;
}

console.log(a);
```

### 7

```js
let a = 10;

{
  let a = 20;
  console.log(a);
}

console.log(a);
```

### 8

```js
console.log(a);

var a = 10;
```

### 9

```js
console.log(a);

let a = 10;
```

### 10

```js
const count = 0;

console.log(count || 10);
console.log(count ?? 10);
```

---

# Phase 1 — One-Minute Revision

```text
Variables
→ var / let / const

Scope
→ global / function / block / module

Hoisting
→ declarations processed before execution

TDZ
→ let/const unavailable before initialization

Data Types
→ 7 primitives + objects/arrays

Operators
→ arithmetic / comparison / logical / assignment

Ternary
→ short if/else

??
→ fallback for null/undefined

?.
→ safe property access

Type Conversion
→ explicit + implicit

==
→ loose equality

===
→ strict equality

Truthy/Falsy
→ boolean behavior of values
```

> **Revision rule:** First predict the output, then run the code. Don't memorize the outputs—understand **why** JavaScript produces them.
