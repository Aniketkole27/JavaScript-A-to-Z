<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 4 — Objects</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-4%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Focus-Shallow%20vs%20Deep%20Copy-orange" />
</p>

> Objects are reference types — almost every React/JS bug about "state not updating" traces back to a shallow-copy misunderstanding here.

## 📑 Contents
- [1. Creating Objects](#1-creating-objects)
- [2. Accessing Properties](#2-accessing-properties)
- [3. Adding / Removing Properties](#3-adding--removing-properties)
- [4. Computed Properties](#4-computed-properties)
- [5. Destructuring](#5-destructuring)
- [6. Nested Objects](#6-nested-objects)
- [7. Object Methods & `this`](#7-object-methods--this)
- [8. `Object.keys()` / `values()` / `entries()`](#8-objectkeys--values--entries)
- [9. `Object.assign()`](#9-objectassign)
- [10. Spread on Objects](#10-spread-on-objects)
- [11. Shallow Copy vs Deep Copy ⭐](#11-shallow-copy-vs-deep-copy-)
- [🧪 Self-Test](#-self-test)

---

## 1. Creating Objects

**Recall**

| Method | Example |
|---|---|
| Object literal | `{ name: "A" }` |
| `new Object()` | `new Object()` — rarely used |
| `Object.create(proto)` | custom prototype chain |
| Constructor function | `new Person("A")` |
| `class` | `new Person("A")` |

**Code**
```js
const literal = { name: "Aniket" };

const fromNew = new Object();
fromNew.name = "Aniket";

const fromCreate = Object.create(null); // no prototype at all — no inherited methods
fromCreate.name = "Aniket";

function Person(name) { this.name = name; }
const fromConstructor = new Person("Aniket");

class PersonClass { constructor(name) { this.name = name; } }
const fromClass = new PersonClass("Aniket");
```

**Explain**
Object literals cover 95% of real-world use. `Object.create(null)` is handy for a "pure dictionary" with zero inherited methods (no `.toString`, `.hasOwnProperty`, etc.).

---

## 2. Accessing Properties

**Code**
```js
const user = { name: "Aniket", "full-name": "Aniket Patil" };

console.log(user.name);          // dot notation — key must be a valid identifier
console.log(user["full-name"]);   // bracket notation — required for special chars/spaces

const key = "name";
console.log(user[key]);            // bracket notation — required for dynamic keys

console.log(user.missing);          // undefined, no error
console.log(user.missing?.length);   // undefined, safe with optional chaining
```

**Explain**
Use **dot notation** by default; switch to **bracket notation** when the key is dynamic, has spaces/hyphens, or comes from a variable.

---

## 3. Adding / Removing Properties

**Code**
```js
const user = { name: "Aniket" };

user.age = 25;              // add
user["city"] = "Pune";       // add via bracket

delete user.age;              // remove
console.log(user); // { name: "Aniket", city: "Pune" }

console.log("name" in user);        // true
console.log(user.hasOwnProperty("city")); // true
```

**Explain**
Objects are **mutable by reference** — adding/deleting a property changes the same object in memory, affecting every variable pointing to it.

---

## 4. Computed Properties

**Understand**
Use `[expression]` as a key inside an object literal — the key is computed at creation time.

**Code**
```js
const field = "email";
const value = "a@test.com";

const user = {
  name: "Aniket",
  [field]: value,          // computed key -> becomes "email": "a@test.com"
  [`${field}Verified`]: true // "emailVerified": true
};
console.log(user); // { name: "Aniket", email: "a@test.com", emailVerified: true }
```

**Explain**
Very common when building objects dynamically — e.g. form state keyed by input `name`, or grouping data by a variable key.

---

## 5. Destructuring

**Code**
```js
const user = { name: "Aniket", age: 25, city: "Pune" };

const { name, age } = user;
console.log(name, age); // Aniket 25

const { city: userCity } = user;    // rename while destructuring
console.log(userCity); // Pune

const { country = "India" } = user; // default if key missing
console.log(country); // India

const { name: n, ...rest } = user;   // rest — collects remaining keys
console.log(rest); // { age: 25, city: "Pune" }

// Nested destructuring
const { address: { city: nestedCity } } = { address: { city: "Mumbai" } };
console.log(nestedCity); // Mumbai

// Function parameter destructuring — extremely common in React props
function greet({ name, age = 18 }) {
  console.log(`${name} is ${age}`);
}
greet({ name: "Aniket" }); // Aniket is 18
```

**Explain**
Destructuring doesn't copy the object — it just pulls **values** out into new variables. Nested/renamed/defaulted destructuring are all interview favorites.

---

## 6. Nested Objects

**Code**
```js
const user = {
  name: "Aniket",
  address: {
    city: "Pune",
    pin: 411001
  }
};

console.log(user.address.city);        // Pune
console.log(user.address?.pin);         // 411001
console.log(user.contact?.phone);        // undefined — safe, no crash

user.address.city = "Mumbai";              // mutates the nested object directly
console.log(user.address.city); // Mumbai
```

**Explain**
Nested objects are references **within** a reference. This is the root cause behind the shallow-copy problem in §11 — copying the outer object does **not** copy the inner ones.

---

## 7. Object Methods & `this`

**Code**
```js
const user = {
  name: "Aniket",
  greet() {                     // shorthand method syntax
    console.log(`Hi, I'm ${this.name}`);
  },
  greetArrow: () => {
    console.log(`Hi, I'm ${this.name}`); // `this` NOT bound to user
  }
};
user.greet();       // Hi, I'm Aniket
user.greetArrow();   // Hi, I'm undefined
```

**Explain**
Inside a regular method, `this` = the object it was called on. Arrow functions as object methods are almost always a bug — they capture `this` from the surrounding (often global/module) scope instead.

---

## 8. `Object.keys()` / `values()` / `entries()`

**Code**
```js
const user = { name: "Aniket", age: 25, city: "Pune" };

console.log(Object.keys(user));    // ["name", "age", "city"]
console.log(Object.values(user));   // ["Aniket", 25, "Pune"]
console.log(Object.entries(user));   // [["name","Aniket"], ["age",25], ["city","Pune"]]

// Common pattern: loop over an object
Object.entries(user).forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});

// Build an object back from entries
const doubled = Object.fromEntries(
  Object.entries({ a: 1, b: 2 }).map(([k, v]) => [k, v * 2])
);
console.log(doubled); // { a: 2, b: 4 }
```

**Explain**
Objects aren't directly iterable with `for...of` — these three methods convert an object into an **array**, unlocking `.map`/`.filter`/`.forEach`.

---

## 9. `Object.assign()`

**Code**
```js
const target = { a: 1 };
const source1 = { b: 2 };
const source2 = { b: 3, c: 4 };

const merged = Object.assign(target, source1, source2);
console.log(merged); // { a: 1, b: 3, c: 4 } -> later sources override
console.log(target);  // { a: 1, b: 3, c: 4 } -> ⚠️ target itself was mutated!

// Safer: use an empty object as target to avoid mutation
const safeMerge = Object.assign({}, source1, source2);
```

**Explain**
`Object.assign(target, ...)` **mutates the first argument** — a common gotcha. Prefer spread (`{ ...a, ...b }`) which always creates a new object.

---

## 10. Spread on Objects

**Code**
```js
const base = { theme: "dark" };
const extended = { ...base, lang: "en" };
console.log(extended); // { theme: "dark", lang: "en" }

// Overriding — order matters, last one wins
const override = { ...base, theme: "light" };
console.log(override); // { theme: "light" }

// Copying (shallow!)
const copy = { ...base };
```

**Explain**
Functionally similar to `Object.assign({}, ...)` but shorter and doesn't mutate anything. **Still a shallow copy** — see next section.

---

## 11. Shallow Copy vs Deep Copy ⭐

**Understand**
```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};
```
```js
const copy = { ...user };
```

**Why `{ ...user }` is NOT a deep copy:**

Spread copies properties **one level deep only**. For primitive values (`name`), it copies the actual value. But for `address` — a **reference type** — spread copies the **reference (memory address)**, not the object itself. So `copy.address` and `user.address` point to the **exact same object** in memory.

```js
const copy = { ...user };
copy.name = "Rahul";           // ✅ independent — primitive was copied by value
copy.address.city = "Mumbai";   // ⚠️ mutates the SHARED nested object

console.log(user.name);          // "Aniket"  -> untouched, primitives are independent
console.log(user.address.city);   // "Mumbai"  -> changed! same object as copy.address
console.log(user.address === copy.address); // true -> literally the same reference
```

**Recall — how to actually deep copy**

| Method | Notes |
|---|---|
| `structuredClone(obj)` | ✅ modern, built-in, handles most cases (no functions) |
| `JSON.parse(JSON.stringify(obj))` | Works but drops `undefined`, functions, `Date`→string, `Symbol` |
| Lodash `_.cloneDeep(obj)` | Battle-tested, handles edge cases |
| Manual recursive copy | Full control, more code |

```js
const user = { name: "Aniket", address: { city: "Pune" } };

const deep1 = structuredClone(user);
deep1.address.city = "Nagpur";
console.log(user.address.city); // "Pune" -> untouched, truly independent

const deep2 = JSON.parse(JSON.stringify(user));
deep2.address.city = "Nashik";
console.log(user.address.city); // still "Pune"
```

**Explain**
- Spread/`Object.assign`/`Array.slice` are all **shallow** — they only copy the **first level** of keys.
- "Copy by value" only applies to primitives. Anything that's an object/array is copied **by reference**, one level down.
- This is exactly why React docs insist: never mutate nested state directly — `{...state, nested: {...state.nested, x: 1}}` (spread at **every level** you're changing), or use `structuredClone`/immutable helper libraries (Immer).

<details>
<summary>❓ Revisit</summary>

```js
const original = {
  name: "A",
  scores: [10, 20],
  meta: { active: true }
};
const shallow = { ...original };

shallow.scores.push(30);
shallow.meta.active = false;
shallow.name = "B";

console.log(original.scores);   // ?
console.log(original.meta);      // ?
console.log(original.name);       // ?
```
```
[10, 20, 30]        // arrays are reference types too — shared, mutated
{ active: false }    // nested object shared, mutated
"A"                   // primitive — independent, untouched
```
</details>

---

## 🧪 Self-Test

```js
const product = { name: "Pen", price: 10, tags: ["stationery"] };

const { name, ...details } = product;
console.log(name, details);

const copy1 = { ...product };
copy1.tags.push("cheap");
console.log(product.tags);

const copy2 = structuredClone(product);
copy2.tags.push("blue");
console.log(product.tags);

const merged = Object.assign({}, product, { price: 15 });
console.log(product.price, merged.price);
```

<details>
<summary>Show answers</summary>

```
"Pen" { price: 10, tags: ["stationery"] }

["stationery", "cheap"]     // shallow copy -> shared array reference

["stationery", "cheap"]     // unaffected by copy2 -> structuredClone is a true deep copy

10 15                        // Object.assign({}, ...) doesn't mutate product, only merged
```
</details>

---

<p align="center"><i>Next → <code>05-arrays.md</code></i></p>
