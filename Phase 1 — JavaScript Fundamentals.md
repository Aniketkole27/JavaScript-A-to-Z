<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 1 — Fundamentals</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-1%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Method-Understand→Recall→Code→Explain→Revisit-9cf" />
</p>

> Predict the output **before** running each snippet. Revisit only the ❓ sections when you come back.

## 📑 Contents
- [1. Variables](#1-variables)
- [2. Data Types](#2-data-types)
- [3. Operators](#3-operators)
- [4. Type Conversion](#4-type-conversion)
- [🧪 Self-Test](#-self-test)

---

## 1. Variables

**Understand**
Three ways to declare a variable — differ in scope, redeclaration, reassignment, hoisting.

**Recall**

| Keyword | Scope | Redeclare | Reassign | Hoisted | TDZ |
|---|---|---|---|---|---|
| `var` | function | ✅ | ✅ | ✅ | inits as `undefined` |
| `let` | block | ❌ | ✅ | ✅ | ❌ throws until declared |
| `const` | block | ❌ | ❌ | ✅ | ❌ throws until declared |

**Code**
```js
function scopeTest() {
  if (true) {
    var a = 1;
    let b = 2;
  }
  console.log(a); // 1  -> var leaks out of block
  console.log(b); // ReferenceError -> let is block scoped
}

console.log(x); // undefined (hoisted, not yet assigned)
var x = 10;

console.log(y); // ReferenceError (Temporal Dead Zone)
let y = 20;

const obj = { name: "Bhavin" };
obj.name = "Raj";     // ✅ allowed — binding not reassigned
obj = {};              // ❌ TypeError: Assignment to constant variable
```

**Explain**
- **Hoisting**: declarations move to the top of their scope at compile time. `var` → `undefined`. `let`/`const` → hoisted but uninitialized.
- **TDZ**: gap between scope start and the declaration line. Access there → `ReferenceError`.
- `const` locks the **binding**, not the value — object/array contents can still mutate.

<details>
<summary>❓ Revisit</summary>

```js
console.log(a); var a = 5;
console.log(b); let b = 5;
for (var i = 0; i < 3; i++) { setTimeout(() => console.log(i)); }
for (let i = 0; i < 3; i++) { setTimeout(() => console.log(i)); }
```

```
undefined
ReferenceError
3 3 3   // var shared across loop
0 1 2   // let — new binding per iteration
```
</details>

---

## 2. Data Types

**Understand**
JS has **primitive** (copied by value) and **reference** (copied by reference) types.

**Recall**
- **Primitives (7):** `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint`
- **Reference:** `object` — includes arrays, functions, dates, maps, sets…

**Code**
```js
// Primitive — copied by value
let a = 10;
let b = a;
b = 20;
console.log(a, b); // 10 20

// Reference — copied by reference
let obj1 = { val: 10 };
let obj2 = obj1;
obj2.val = 20;
console.log(obj1.val); // 20 -> same object in memory

typeof "hi";          // "string"
typeof 42;             // "number"
typeof true;            // "boolean"
typeof undefined;        // "undefined"
typeof null;              // "object"  <- famous JS bug
typeof Symbol();            // "symbol"
typeof 10n;                  // "bigint"
typeof {};                    // "object"
typeof [];                     // "object"
typeof function(){};            // "function"
```

**Explain**
- `typeof null === "object"` → historic spec bug, kept for backward compatibility.
- Arrays are objects — use `Array.isArray(arr)`, not `typeof`.
- `bigint` handles integers beyond `Number.MAX_SAFE_INTEGER` (2⁵³−1), written with an `n` suffix.

<details>
<summary>❓ Revisit</summary>

```js
let arr = [1, 2, 3];
let arrCopy = arr;
arrCopy.push(4);
console.log(arr.length);

console.log(typeof NaN);
console.log(typeof typeof 5);
```

```
4          // reference copy
"number"
"string"   // typeof always returns a string
```
</details>

---

## 3. Operators

**Understand**
Know short-circuiting, nullish coalescing, and optional chaining — asked in almost every interview.

**Recall**

| Operator | Meaning | Example |
|---|---|---|
| `??` | right side only if left is `null`/`undefined` | `0 ?? 5 → 0` |
| `\|\|` | right side if left is any falsy value | `0 \|\| 5 → 5` |
| `?.` | safe nested access, short-circuits to `undefined` | `user?.address?.city` |
| `?.()` | safe optional function call | `obj.fn?.()` |

**Code**
```js
let count = 0;
console.log(count || 10); // 10  (0 is falsy)
console.log(count ?? 10); // 0   (0 is not null/undefined)

const user = { profile: { name: "Bhavin" } };
console.log(user?.profile?.name);   // "Bhavin"
console.log(user?.address?.city);   // undefined (no error!)
console.log(user.address.city);     // TypeError (without ?.)

let x = 5;
let y = x > 2 ? "big" : "small";    // ternary
console.log(y); // "big"
```

**Explain**
- `||` checks **falsy** (`0, "", null, undefined, NaN, false`) — bug source when `0`/`""` is a valid value.
- `??` checks **only** `null`/`undefined` — safer for defaults.
- `?.` prevents "Cannot read property of undefined" crashes.

<details>
<summary>❓ Revisit</summary>

```js
console.log("" || "default");
console.log("" ?? "default");
console.log(null ?? undefined ?? "final");
```

```
"default"
""
"final"
```
</details>

---

## 4. Type Conversion

**Understand**
JS auto-converts types (**coercion**) during operations — the #1 source of "weird JS" questions.

**Recall**
- **Implicit:** JS converts automatically → `"5" + 1`
- **Explicit:** you convert manually → `Number("5")`, `String(5)`, `Boolean(0)`
- `==` loose equality — coerces types before comparing
- `===` strict equality — no coercion
- **Falsy (only 6):** `false, 0, "", null, undefined, NaN`
- Everything else, including `"0"`, `[]`, `{}`, is **truthy**

**Code**
```js
console.log(1 + "2");        // "12"   -> number coerced to string
console.log("5" - 1);         // 4      -> string coerced to number
console.log(true + 1);         // 2      -> true becomes 1
console.log(false + "1");       // "false1"

console.log(null == undefined);  // true  -> special case rule
console.log(null === undefined); // false -> different types

console.log(NaN == NaN);          // false -> NaN never equals itself
console.log([] == false);          // true  -> [] -> "" -> 0 -> false

console.log(Boolean(""));           // false
console.log(Boolean("0"));           // true  -> non-empty string!
console.log(Boolean([]));             // true  -> empty array is truthy
```

**Explain**
- `==` follows Abstract Equality — converts operands to a common type first, causing surprises like `[] == false`.
- `===` never converts — safer, use by default.
- Empty containers (`[]`, `{}`) are objects → always **truthy**.

<details>
<summary>❓ Revisit</summary>

```js
console.log(1 + "2");
console.log(true + 1);
console.log(null == undefined);
console.log(null === undefined);
console.log("5" + 3 - 2);
console.log([1,2] + [3,4]);
```

```
"12"
2
true
false
51        // "53" - 2 = 51
"1,23,4"  // arrays -> strings, then concatenated
```
</details>

---

## 🧪 Self-Test
Mix of all four topics above.

```js
console.log(typeof null);
console.log(0 == "0");
console.log(0 === "0");
let arr = []; console.log(Boolean(arr));
console.log([] + []);
console.log([] + {});
console.log({} + []);
```

<details>
<summary>Show answers</summary>

```
"object"
true
false
true
""
"[object Object]"
"[object Object]"   // or 0 if {} is parsed as a block — context dependent
```
</details>

---

<p align="center"><i>Next → <code>02-functions-closures-this.md</code></i></p>
