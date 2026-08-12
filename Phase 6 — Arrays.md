<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 6 — Arrays ⭐⭐⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-6%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Know%20Without%20Docs-critical" />
</p>

> Know these without needing documentation. Then reimplement the iteration methods yourself — that's what actually proves you understand them.

## 📑 Contents
- [Basic Methods](#basic-methods)
- [Mutates vs Returns New — Cheat Table](#mutates-vs-returns-new--cheat-table)
- [Iteration Methods](#iteration-methods)
- [The Difference — Core Mental Model ⭐](#the-difference--core-mental-model-)
- [Advanced Methods](#advanced-methods)
- [🔧 Practice — Reimplement Them Yourself](#-practice--reimplement-them-yourself)
- [🧪 Self-Test](#-self-test)

---

## Basic Methods

**Code**
```js
const arr = [1, 2, 3];

arr.push(4);           // [1,2,3,4]        -> adds to end, MUTATES, returns new length
arr.pop();               // [1,2,3]          -> removes from end, MUTATES, returns removed item
arr.unshift(0);            // [0,1,2,3]        -> adds to start, MUTATES, returns new length
arr.shift();                 // [1,2,3]          -> removes from start, MUTATES, returns removed item

const sliced = arr.slice(1, 3);    // [2,3]  -> extracts, does NOT mutate, returns new array
console.log(arr);                    // [1,2,3] -> untouched

const spliced = arr.splice(1, 1, "X"); // removes 1 item at index 1, inserts "X" — MUTATES original
console.log(arr);      // [1, "X", 3]
console.log(spliced);   // [2] -> returns the removed items

const combined = [1, 2].concat([3, 4]); // [1,2,3,4] -> does NOT mutate, returns new array

console.log([1, 2, 3].includes(2));    // true  -> boolean, checks value
console.log([1, 2, 3].indexOf(2));      // 1     -> index, or -1 if not found
console.log([1, 2, 3].indexOf(99));      // -1
```

**Explain**
- **Mutating**: `push`, `pop`, `shift`, `unshift`, `splice` — change the original array.
- **Non-mutating**: `slice`, `concat` — always return a **new** array, original untouched.
- `slice(start, end)` extracts (end excluded). `splice(start, deleteCount, ...items)` cuts and/or inserts in place.
- `includes` → does it contain the value (boolean)? `indexOf` → where is it (index, or `-1`)?

---

## Mutates vs Returns New — Cheat Table

| Method | Mutates original? | Returns |
|---|---|---|
| `push` / `pop` / `shift` / `unshift` | ✅ | new length / removed item |
| `splice` | ✅ | array of removed items |
| `sort` / `reverse` / `fill` | ✅ | the same array (mutated) |
| `slice` / `concat` / `map` / `filter` | ❌ | new array |
| `flat` / `flatMap` | ❌ | new array |

<details>
<summary>❓ Revisit</summary>

```js
const original = [3, 1, 2];
const result = original.sort();
console.log(original === result); // ?
```
```
true   // sort() mutates AND returns the same array reference
```
</details>

---

## Iteration Methods

**Code**
```js
const nums = [1, 2, 3, 4, 5];

nums.forEach(n => console.log(n));                    // just runs an action, returns undefined
console.log(nums.map(n => n * 2));                       // [2,4,6,8,10] -> transformed new array
console.log(nums.filter(n => n % 2 === 0));                // [2,4] -> subset that passed the test
console.log(nums.reduce((acc, n) => acc + n, 0));            // 15 -> single accumulated value
console.log(nums.find(n => n > 3));                            // 4 -> first matching VALUE
console.log(nums.findIndex(n => n > 3));                         // 3 -> first matching INDEX
console.log(nums.some(n => n > 4));                                // true -> at least one passes?
console.log(nums.every(n => n > 0));                                 // true -> ALL pass?
console.log(nums.every(n => n > 4));                                  // false
```

**Explain**
- All of these take a **callback** `(element, index, array) => ...`.
- `map`/`filter` always return an array **of the same or smaller length** — `map` never changes length, `filter` only removes.
- `find`/`findIndex`/`some` **short-circuit** — they stop as soon as they get an answer, unlike `forEach`/`map` which always visit every element.

---

## The Difference — Core Mental Model ⭐

| Method | Purpose | Returns | Stops early? |
|---|---|---|---|
| `map` | **transform** each element | new array, same length | ❌ |
| `filter` | **select** matching elements | new array, ≤ length | ❌ |
| `reduce` | **accumulate** into one value | anything (number, object, array...) | ❌ |
| `find` | first matching **element** | the value, or `undefined` | ✅ |
| `findIndex` | first matching **index** | the index, or `-1` | ✅ |
| `some` | at least one match? | `true`/`false` | ✅ |
| `every` | all match? | `true`/`false` | ✅ (on first `false`) |
| `forEach` | perform an **action** per item | `undefined` | ❌ |

```js
// Same array, six different questions:
const people = [
  { name: "A", age: 17 },
  { name: "B", age: 25 },
  { name: "C", age: 30 }
];

people.map(p => p.name);                  // ["A","B","C"] -> transform
people.filter(p => p.age >= 18);            // [{B},{C}] -> select
people.reduce((sum, p) => sum + p.age, 0);   // 72 -> accumulate
people.find(p => p.age >= 18);                // {B} -> first match
people.some(p => p.age < 18);                  // true -> any minors?
people.every(p => p.age >= 18);                 // false -> all adults?
people.forEach(p => console.log(p.name));         // just logs, returns undefined
```

**Explain**
If you're transforming → `map`. Selecting → `filter`. Boiling down to one value → `reduce`. Everything else (`find`/`some`/`every`) is a **question about existence**, and only `reduce` can build something that isn't a same-shaped array.

---

## Advanced Methods

**Code**
```js
console.log([1, [2, 3], [4, [5, 6]]].flat());        // [1,2,3,4,[5,6]] -> flattens 1 level deep
console.log([1, [2, 3], [4, [5, 6]]].flat(2));         // [1,2,3,4,5,6]   -> depth 2
console.log([1, [2, [3, [4]]]].flat(Infinity));         // [1,2,3,4]        -> fully flat

console.log([1, 2, 3].flatMap(n => [n, n * 2]));          // [1,2,2,4,3,6] -> map then flat(1), combined

const nums = [3, 1, 4, 1, 5];
console.log([...nums].sort());                              // [1,1,3,4,5] -> default: STRING sort!
console.log([10, 2, 33].sort());                              // [10, 2, 33] -> ⚠️ wrong! sorted as strings
console.log([10, 2, 33].sort((a, b) => a - b));                 // [2, 10, 33] -> correct numeric sort

console.log([1, 2, 3].reverse());                                 // [3,2,1] -> mutates in place

console.log(new Array(3).fill(0));                                  // [0,0,0]
console.log([1, 2, 3, 4].fill(9, 1, 3));                              // [1,9,9,4] -> fill(value, start, end)

console.log(Array.from({ length: 5 }, (_, i) => i * 2));               // [0,2,4,6,8]
console.log(Array.from("abc"));                                          // ["a","b","c"]
console.log(Array.from(new Set([1, 2, 2, 3])));                            // [1,2,3]

console.log(Array.isArray([1, 2]));    // true
console.log(Array.isArray({ length: 0 })); // false -> array-like isn't a real array
```

**Explain**
- `sort()` **without a comparator** converts elements to strings first — `[10, 2, 33]` becomes `["10","2","33"]` sorted lexicographically → wrong for numbers. Always pass `(a, b) => a - b` for numeric sort.
- `flat(depth)` default depth is `1`. `flatMap` = `map` + `flat(1)`, but more efficient (one pass).
- `Array.from(arrayLike, mapFn)` is the go-to way to generate a range or convert iterables (`Set`, `NodeList`, strings) into real arrays.
- `Array.isArray()` is the **only reliable** array check — `typeof [] === "object"` (see Phase 1).

---

## 🔧 Practice — Reimplement Them Yourself

**Understand**
Implement these as `Array.prototype` extensions to see exactly what each method does under the hood.

```js
Array.prototype.customForEach = function (callback) {
  for (let i = 0; i < this.length; i++) {
    callback(this[i], i, this);
  }
};

Array.prototype.customMap = function (callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};

Array.prototype.customFilter = function (callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) result.push(this[i]);
  }
  return result;
};

Array.prototype.customReduce = function (callback, initialValue) {
  let acc = initialValue;
  let startIndex = 0;

  if (acc === undefined) {          // no initial value -> use first element
    acc = this[0];
    startIndex = 1;
  }
  for (let i = startIndex; i < this.length; i++) {
    acc = callback(acc, this[i], i, this);
  }
  return acc;
};

Array.prototype.customFind = function (callback) {
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) return this[i]; // short-circuits
  }
  return undefined;
};

Array.prototype.customIncludes = function (target) {
  for (let i = 0; i < this.length; i++) {
    if (this[i] === target) return true; // short-circuits
  }
  return false;
};

Array.prototype.customFlat = function (depth = 1) {
  const result = [];
  for (const item of this) {
    if (Array.isArray(item) && depth > 0) {
      result.push(...item.customFlat(depth - 1)); // recursion, one level less each time
    } else {
      result.push(item);
    }
  }
  return result;
};
```

**Test them**
```js
console.log([1, 2, 3].customMap(n => n * 2));          // [2,4,6]
console.log([1, 2, 3, 4].customFilter(n => n % 2 === 0)); // [2,4]
console.log([1, 2, 3].customReduce((a, b) => a + b, 0));    // 6
console.log([1, 2, 3].customFind(n => n > 1));               // 2
console.log([1, 2, 3].customIncludes(2));                      // true
console.log([1, [2, [3, 4]], 5].customFlat(2));                  // [1,2,3,4,5]
```

**Explain**
- `customReduce` — the tricky part is handling a **missing initial value**: real `reduce` then uses index `0` as the starting accumulator and starts looping from index `1`.
- `customFind`/`customIncludes` — the key insight is the early `return` **inside** the loop, which is exactly what makes them short-circuit (unlike `map`/`filter`, which never `return` early).
- `customFlat` — recursion: for each item, if it's an array **and** we still have depth budget, flatten it further; otherwise keep it as-is.

---

## 🧪 Self-Test

```js
const nums = [5, 12, 8, 3, 20, 15];

console.log(nums.filter(n => n > 10).map(n => n * 2));

console.log(nums.reduce((max, n) => n > max ? n : max, nums[0]));

console.log(nums.some(n => n > 100));
console.log(nums.every(n => n > 0));

console.log([1, [2, 3, [4, 5]], 6].flat(Infinity));

console.log([...nums].sort((a, b) => b - a));
console.log(nums);
```

<details>
<summary>Show answers</summary>

```
[24, 40, 30]        // filter > 10 -> [12,20,15] -> map *2

20                   // reduce finds max

false                 // no element > 100
true                   // all elements > 0

[1, 2, 3, 4, 5, 6]      // fully flattened, arbitrary depth

[20, 15, 12, 8, 5, 3]    // sorted copy, descending
[5, 12, 8, 3, 20, 15]     // original untouched — [...nums] made a copy first
```
</details>

---

<p align="center"><i>Next → <code>07-strings-json.md</code></i></p>
