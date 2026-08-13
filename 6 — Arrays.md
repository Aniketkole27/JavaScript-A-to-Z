# JavaScript A→Z Revision

## Phase 6 — Arrays ⭐⭐⭐

> **Goal:** Become comfortable working with JavaScript arrays and understand the purpose, behavior, return values, mutation, and use cases of the most important array methods.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Beginner → Advanced
**Interview Relevance:** Extremely High

> ⚠️ **Important:** Don't just memorize array methods. You should know **what problem each method solves, what it returns, whether it mutates the original array, and when to use it.**

---

## 📚 Contents

1. [What Is an Array?](#1-what-is-an-array)
2. [Basic Array Methods](#2-basic-array-methods)

   * [push()](#push)
   * [pop()](#pop)
   * [shift()](#shift)
   * [unshift()](#unshift)
   * [slice()](#slice)
   * [splice()](#splice)
   * [concat()](#concat)
   * [includes()](#includes)
   * [indexOf()](#indexof)
3. [Iteration Methods](#3-iteration-methods)

   * [forEach()](#foreach)
   * [map()](#map)
   * [filter()](#filter)
   * [reduce()](#reduce)
   * [find()](#find)
   * [findIndex()](#findindex)
   * [some()](#some)
   * [every()](#every)
4. [Advanced Array Methods](#4-advanced-array-methods)

   * [flat()](#flat)
   * [flatMap()](#flatmap)
   * [sort()](#sort)
   * [reverse()](#reverse)
   * [fill()](#fill)
   * [Array.from()](#arrayfrom)
   * [Array.isArray()](#arrayisarray)
5. [Mutation vs Non-Mutation](#5-mutation-vs-non-mutation)
6. [The Most Important Array Methods](#6-the-most-important-array-methods)
7. [Implement Custom Array Methods](#7-implement-custom-array-methods)
8. [Custom `map()`](#custom-map)
9. [Custom `filter()`](#custom-filter)
10. [Custom `reduce()`](#custom-reduce)
11. [Custom `forEach()`](#custom-foreach)
12. [Custom `find()`](#custom-find)
13. [Custom `includes()`](#custom-includes)
14. [Custom `flat()`](#custom-flat)
15. [Array Method Decision Guide](#15-array-method-decision-guide)
16. [Quick Revision](#16-quick-revision)
17. [Interview Questions](#17-interview-questions)
18. [Must Remember](#18-must-remember)
19. [Practice Checklist](#19-practice-checklist)

---

# 1. What Is an Array?

An **array** is an ordered collection of values.

```js
const fruits = ["Apple", "Banana", "Mango"];
```

Each value has an index.

```text
Index:    0         1         2
          ↓         ↓         ↓
        Apple     Banana    Mango
```

Access an element:

```js
console.log(fruits[0]);
```

Output:

```text
Apple
```

---

## Arrays Can Store Different Types

JavaScript arrays can contain different types of values.

```js
const data = [
    "Aniket",
    21,
    true,
    null,
    { role: "Developer" },
    [1, 2, 3]
];
```

However, in practical application code, arrays usually contain related types of data.

---

## Array Length

```js
const numbers = [10, 20, 30, 40];

console.log(numbers.length);
```

Output:

```text
4
```

The last index is:

```js
numbers.length - 1;
```

---

# 2. Basic Array Methods

These methods are used for:

* Adding elements
* Removing elements
* Copying portions
* Combining arrays
* Searching arrays

---

# `push()`

Adds one or more elements to the **end** of an array.

```js
const numbers = [1, 2, 3];

numbers.push(4);

console.log(numbers);
```

Output:

```js
[1, 2, 3, 4]
```

### Return Value

`push()` returns the **new array length**.

```js
const result = numbers.push(5);

console.log(result);
```

Output:

```text
5
```

### Mutation

`push()` **mutates** the original array.

```text
[1, 2, 3]
     ↓ push(4)
[1, 2, 3, 4]
```

---

# `pop()`

Removes the **last element**.

```js
const numbers = [1, 2, 3];

const removed = numbers.pop();

console.log(removed);
console.log(numbers);
```

Output:

```text
3
[1, 2]
```

### Return Value

Returns the removed element.

### Mutation

`pop()` mutates the original array.

---

# `shift()`

Removes the **first element**.

```js
const numbers = [1, 2, 3];

const removed = numbers.shift();

console.log(removed);
console.log(numbers);
```

Output:

```text
1
[2, 3]
```

### Mutation

Yes.

---

# `unshift()`

Adds one or more elements to the **beginning**.

```js
const numbers = [2, 3];

numbers.unshift(1);

console.log(numbers);
```

Output:

```js
[1, 2, 3]
```

### Return Value

Returns the new array length.

### Mutation

Yes.

---

# `slice()`

`slice()` creates a **shallow copy of a portion of an array**.

### Syntax

```js
array.slice(start, end);
```

The `end` index is **not included**.

```js
const numbers = [10, 20, 30, 40, 50];

const result = numbers.slice(1, 4);

console.log(result);
```

Output:

```js
[20, 30, 40]
```

Indexes:

```text
10   20   30   40   50
0    1    2    3    4
     └────────┘
       slice(1,4)
```

### Important

`slice()` does **not mutate** the original array.

```js
const numbers = [1, 2, 3];

const copy = numbers.slice();

copy.push(4);

console.log(numbers);
```

Output:

```js
[1, 2, 3]
```

---

# `splice()`

`splice()` can:

* Remove elements
* Add elements
* Replace elements

### Syntax

```js
array.splice(start, deleteCount, item1, item2, ...);
```

---

## Remove

```js
const numbers = [1, 2, 3, 4];

numbers.splice(1, 2);

console.log(numbers);
```

Output:

```js
[1, 4]
```

---

## Add

```js
const numbers = [1, 4];

numbers.splice(1, 0, 2, 3);

console.log(numbers);
```

Output:

```js
[1, 2, 3, 4]
```

---

## Replace

```js
const numbers = [1, 2, 3];

numbers.splice(1, 1, 20);

console.log(numbers);
```

Output:

```js
[1, 20, 3]
```

### Mutation

`splice()` **mutates** the original array.

---

## ⭐ `slice()` vs `splice()`

| `slice()`              | `splice()`                  |
| ---------------------- | --------------------------- |
| Does not mutate        | Mutates                     |
| Creates a portion/copy | Adds/removes/replaces       |
| `slice(start, end)`    | `splice(start, count, ...)` |
| End index excluded     | Uses delete count           |

### Memory Trick

> **slice = take a piece**

> **splice = modify the original**

---

# `concat()`

Combines arrays and/or values into a **new array**.

```js
const frontend = ["HTML", "CSS"];
const backend = ["Node.js", "Express"];

const skills = frontend.concat(backend);

console.log(skills);
```

Output:

```js
["HTML", "CSS", "Node.js", "Express"]
```

The original arrays remain unchanged.

---

## Modern Alternative

```js
const skills = [...frontend, ...backend];
```

Both approaches create a new array.

---

# `includes()`

Checks whether an array contains a value.

```js
const skills = ["JavaScript", "React", "Node.js"];

console.log(skills.includes("React"));
```

Output:

```text
true
```

If not found:

```js
console.log(skills.includes("Python"));
```

Output:

```text
false
```

### Return Value

Always:

```text
true
```

or:

```text
false
```

### Important

`includes()` uses **SameValueZero** comparison.

This means it can correctly detect `NaN`:

```js
console.log([NaN].includes(NaN));
```

Output:

```text
true
```

---

# `indexOf()`

Returns the index of the first matching element.

```js
const skills = ["JavaScript", "React", "Node.js"];

console.log(skills.indexOf("React"));
```

Output:

```text
1
```

If not found:

```js
console.log(skills.indexOf("Python"));
```

Output:

```text
-1
```

### `indexOf()` vs `includes()`

```text
includes()
    ↓
Does it exist?
    ↓
true / false

indexOf()
    ↓
Where is it?
    ↓
index / -1
```

---

# 3. Iteration Methods

Iteration methods are extremely important for interviews and real-world JavaScript.

The most important ones are:

```text
forEach
map
filter
reduce
find
findIndex
some
every
```

---

# `forEach()`

`forEach()` executes a function once for each array element.

```js
const numbers = [1, 2, 3];

numbers.forEach(number => {
    console.log(number);
});
```

Output:

```text
1
2
3
```

### Important

`forEach()` does **not create a new transformed array**.

```js
const result = numbers.forEach(number => number * 2);

console.log(result);
```

Output:

```text
undefined
```

### Use `forEach()` When

You want to:

> **Perform an action for every element.**

Example:

```js
users.forEach(user => {
    console.log(user.name);
});
```

---

# `map()`

`map()` creates a **new array** by transforming every element.

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

Output:

```js
[2, 4, 6]
```

### Important

`map()` returns an array with the same number of elements as the original array.

```text
[1, 2, 3]
 ↓  ↓  ↓
[2, 4, 6]
```

### Use `map()` When

You want to:

> **Transform every element.**

Example:

```js
const users = [
    { name: "Aniket" },
    { name: "Rahul" }
];

const names = users.map(user => user.name);
```

Result:

```js
["Aniket", "Rahul"]
```

---

# `filter()`

`filter()` creates a new array containing only elements that satisfy a condition.

```js
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers);
```

Output:

```js
[2, 4]
```

### Use `filter()` When

You want to:

> **Select some elements.**

Example:

```js
const users = [
    { name: "Aniket", active: true },
    { name: "Rahul", active: false },
    { name: "Amit", active: true }
];

const activeUsers = users.filter(user => user.active);
```

Result:

```js
[
    { name: "Aniket", active: true },
    { name: "Amit", active: true }
]
```

---

# `reduce()`

`reduce()` processes an array and combines its elements into a **single accumulated result**.

### Basic Example

```js
const numbers = [1, 2, 3, 4];

const total = numbers.reduce(
    (sum, number) => sum + number,
    0
);

console.log(total);
```

Output:

```text
10
```

Conceptually:

```text
0 + 1 → 1
1 + 2 → 3
3 + 3 → 6
6 + 4 → 10
```

---

## `reduce()` Can Return Anything

The final result does not have to be a number.

It can return:

* Number
* String
* Object
* Array
* Map
* Set
* Any other value

### Example — Grouping

```js
const users = [
    { name: "Aniket", role: "developer" },
    { name: "Rahul", role: "designer" },
    { name: "Amit", role: "developer" }
];

const grouped = users.reduce((result, user) => {
    if (!result[user.role]) {
        result[user.role] = [];
    }

    result[user.role].push(user);

    return result;
}, {});
```

Result:

```js
{
    developer: [
        { name: "Aniket", role: "developer" },
        { name: "Amit", role: "developer" }
    ],
    designer: [
        { name: "Rahul", role: "designer" }
    ]
}
```

### Use `reduce()` When

You want to:

> **Accumulate multiple values into one result.**

---

# `find()`

`find()` returns the **first element** that satisfies a condition.

```js
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number > 20);

console.log(result);
```

Output:

```text
30
```

Why?

```text
10 → false
20 → false
30 → true ← STOP
40 → not checked
```

If no element matches:

```js
const result = numbers.find(number => number > 100);

console.log(result);
```

Output:

```text
undefined
```

---

# `findIndex()`

`findIndex()` returns the index of the **first matching element**.

```js
const numbers = [10, 20, 30, 40];

const index = numbers.findIndex(number => number > 20);

console.log(index);
```

Output:

```text
2
```

If no element matches:

```text
-1
```

---

# `some()`

`some()` checks whether **at least one** element satisfies a condition.

```js
const numbers = [1, 3, 5, 6];

const result = numbers.some(number => number % 2 === 0);

console.log(result);
```

Output:

```text
true
```

Because `6` is even.

### Mental Question

> **"Is there at least one?"**

---

# `every()`

`every()` checks whether **all** elements satisfy a condition.

```js
const numbers = [2, 4, 6, 8];

const result = numbers.every(number => number % 2 === 0);

console.log(result);
```

Output:

```text
true
```

### Mental Question

> **"Do all of them satisfy this condition?"**

---

# 4. Advanced Array Methods

---

# `flat()`

`flat()` creates a new array by removing nested array levels.

```js
const numbers = [1, [2, 3], [4, 5]];

console.log(numbers.flat());
```

Output:

```js
[1, 2, 3, 4, 5]
```

---

## Multiple Levels

```js
const numbers = [1, [2, [3, [4]]]];

console.log(numbers.flat(2));
```

Output:

```js
[1, 2, 3, [4]]
```

By default:

```js
array.flat()
```

uses depth `1`.

To flatten all levels:

```js
array.flat(Infinity);
```

---

# `flatMap()`

`flatMap()` is essentially:

```text
map()
  +
flat(1)
```

Example:

```js
const numbers = [1, 2, 3];

const result = numbers.flatMap(number => [number, number * 2]);

console.log(result);
```

Output:

```js
[1, 2, 2, 4, 3, 6]
```

Conceptually:

```js
numbers.map(number => [number, number * 2]).flat();
```

---

# `sort()`

`sort()` sorts the elements of an array.

### Important

`sort()` **mutates the original array**.

Also, by default, values are converted to strings and compared lexicographically.

```js
const numbers = [10, 2, 30, 5];

numbers.sort();

console.log(numbers);
```

Output:

```js
[10, 2, 30, 5]
```

This can surprise you because string comparison is being used.

For numeric sorting:

```js
numbers.sort((a, b) => a - b);
```

Output:

```js
[2, 5, 10, 30]
```

### Descending

```js
numbers.sort((a, b) => b - a);
```

---

## Why `a - b`?

The comparator tells JavaScript the ordering.

```text
negative → a comes before b
zero     → same order
positive → b comes before a
```

Therefore:

```js
(a, b) => a - b
```

sorts numbers in ascending order.

---

# `reverse()`

Reverses an array **in place**.

```js
const numbers = [1, 2, 3];

numbers.reverse();

console.log(numbers);
```

Output:

```js
[3, 2, 1]
```

### Mutation

Yes.

---

## Non-Mutating Reverse

If you want to preserve the original:

```js
const reversed = [...numbers].reverse();
```

Or:

```js
const reversed = numbers.toReversed();
```

when supported by the target environment.

---

# `fill()`

`fill()` replaces array elements with a specified value.

```js
const numbers = [1, 2, 3, 4];

numbers.fill(0);

console.log(numbers);
```

Output:

```js
[0, 0, 0, 0]
```

---

## Range

```js
const numbers = [1, 2, 3, 4, 5];

numbers.fill(0, 1, 4);

console.log(numbers);
```

Output:

```js
[1, 0, 0, 0, 5]
```

Syntax:

```js
array.fill(value, start, end);
```

### Mutation

Yes.

---

# `Array.from()`

`Array.from()` creates a new array from an **iterable or array-like object**.

### String

```js
const chars = Array.from("Hello");

console.log(chars);
```

Output:

```js
["H", "e", "l", "l", "o"]
```

---

## NodeList

```js
const elements = document.querySelectorAll("div");

const array = Array.from(elements);
```

Now `array` is a normal JavaScript array.

---

## Generate Arrays

`Array.from()` can also accept a mapping function.

```js
const numbers = Array.from(
    { length: 5 },
    (_, index) => index + 1
);

console.log(numbers);
```

Output:

```js
[1, 2, 3, 4, 5]
```

---

# `Array.isArray()`

Checks whether a value is an array.

```js
console.log(Array.isArray([1, 2, 3]));
```

Output:

```text
true
```

```js
console.log(Array.isArray({}));
```

Output:

```text
false
```

### Why Use It?

Remember:

```js
typeof [];
```

returns:

```text
object
```

Therefore:

```js
Array.isArray(value)
```

is the correct way to check for arrays.

---

# 5. Mutation vs Non-Mutation

Understanding mutation is extremely important, especially in React.

## Mutating Methods

These directly modify the original array:

```text
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
fill()
```

---

## Non-Mutating Methods

These do not modify the original array:

```text
slice()
concat()
includes()
indexOf()
forEach()
map()
filter()
reduce()
find()
findIndex()
some()
every()
flat()
flatMap()
```

> Note: `map()`, `filter()`, `flat()`, etc. create new arrays, but if the elements are objects, those object references are still shallowly copied.

---

## Example

```js
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(numbers);
console.log(doubled);
```

Output:

```js
[1, 2, 3]
[2, 4, 6]
```

The original array remains unchanged.

---

# 6. The Most Important Array Methods

These methods are especially important for interviews.

```text
map
filter
reduce
find
some
every
forEach
```

The easiest way to remember them is by the **question you're trying to answer**.

---

## `map()` → Transform

> "I want to change every element."

```js
const numbers = [1, 2, 3];

const result = numbers.map(number => number * 2);
```

```text
[1, 2, 3]
     ↓ transform
[2, 4, 6]
```

---

## `filter()` → Select

> "I only want elements that satisfy this condition."

```js
const numbers = [1, 2, 3, 4];

const result = numbers.filter(number => number % 2 === 0);
```

```text
[1, 2, 3, 4]
     ↓ select
[2, 4]
```

---

## `reduce()` → Accumulate

> "I want to combine the elements into one result."

```js
const numbers = [1, 2, 3, 4];

const result = numbers.reduce(
    (sum, number) => sum + number,
    0
);
```

```text
1 + 2 + 3 + 4
      ↓
     10
```

---

## `find()` → First Match

> "Give me the first element that satisfies this condition."

```js
const numbers = [10, 20, 30, 40];

const result = numbers.find(number => number > 20);
```

```text
30
```

---

## `some()` → At Least One?

> "Does at least one element satisfy the condition?"

```js
const result = [1, 2, 3].some(number => number > 2);
```

```text
true
```

---

## `every()` → All?

> "Do all elements satisfy the condition?"

```js
const result = [2, 4, 6].every(number => number % 2 === 0);
```

```text
true
```

---

## `forEach()` → Perform Action

> "I want to perform an action for every element."

```js
numbers.forEach(number => {
    console.log(number);
});
```

It is not designed to transform the array.

---

# 7. Implement Custom Array Methods

This is an excellent interview exercise because it forces you to understand:

* Callbacks
* Higher-order functions
* Iteration
* Return values
* Accumulators
* Array construction
* Edge cases
* `this`
* Generic behavior

Before looking at the built-in implementation, try creating your own.

---

## Practice Order

Implement them in this order:

```text
1. customForEach()
2. customMap()
3. customFilter()
4. customFind()
5. customIncludes()
6. customReduce()
7. customFlat()
```

Start simple, then handle edge cases.

---

# 8. Custom `map()`

### Goal

Create:

```js
customMap()
```

that behaves similarly to:

```js
Array.prototype.map()
```

### Example

Expected behavior:

```js
const numbers = [1, 2, 3];

const result = numbers.customMap(number => number * 2);

console.log(result);
```

Output:

```js
[2, 4, 6]
```

### Basic Implementation

```js
Array.prototype.customMap = function (callback) {
    const result = [];

    for (let i = 0; i < this.length; i++) {
        result.push(callback(this[i], i, this));
    }

    return result;
};
```

### Important Concepts

Your implementation should:

* Create a new array.
* Iterate through the original array.
* Call the callback.
* Pass the current value.
* Pass the index.
* Pass the original array.
* Return the new array.

---

# 9. Custom `filter()`

### Goal

Create:

```js
customFilter()
```

Example:

```js
const numbers = [1, 2, 3, 4];

const result = numbers.customFilter(
    number => number % 2 === 0
);

console.log(result);
```

Output:

```js
[2, 4]
```

### Basic Implementation

```js
Array.prototype.customFilter = function (callback) {
    const result = [];

    for (let i = 0; i < this.length; i++) {
        if (callback(this[i], i, this)) {
            result.push(this[i]);
        }
    }

    return result;
};
```

### Key Difference From `map()`

```text
map
 ↓
Always produces one result for each visited element.

filter
 ↓
Only adds an element when callback returns true.
```

---

# 10. Custom `reduce()`

`reduce()` is slightly more difficult.

### Example

```js
const numbers = [1, 2, 3, 4];

const result = numbers.customReduce(
    (sum, number) => sum + number,
    0
);

console.log(result);
```

Output:

```text
10
```

### Basic Implementation

```js
Array.prototype.customReduce = function (callback, initialValue) {
    let accumulator = initialValue;
    let startIndex = 0;

    if (arguments.length < 2) {
        accumulator = this[0];
        startIndex = 1;
    }

    for (let i = startIndex; i < this.length; i++) {
        accumulator = callback(
            accumulator,
            this[i],
            i,
            this
        );
    }

    return accumulator;
};
```

### Core Idea

```text
initial value
     ↓
callback(accumulator, currentValue)
     ↓
new accumulator
     ↓
next element
     ↓
callback again
     ↓
final accumulator
```

---

# 11. Custom `forEach()`

### Goal

Execute a callback for every element.

```js
const numbers = [1, 2, 3];

numbers.customForEach(number => {
    console.log(number);
});
```

### Basic Implementation

```js
Array.prototype.customForEach = function (callback) {
    for (let i = 0; i < this.length; i++) {
        callback(this[i], i, this);
    }
};
```

### Important

`customForEach()` should not return a transformed array.

The built-in `forEach()` returns:

```text
undefined
```

---

# 12. Custom `find()`

### Goal

Return the first element for which the callback returns `true`.

```js
const numbers = [10, 20, 30, 40];

const result = numbers.customFind(
    number => number > 20
);

console.log(result);
```

Output:

```text
30
```

### Basic Implementation

```js
Array.prototype.customFind = function (callback) {
    for (let i = 0; i < this.length; i++) {
        if (callback(this[i], i, this)) {
            return this[i];
        }
    }

    return undefined;
};
```

### Important

Unlike `filter()`:

```text
find()
 ↓
Returns ONE element
```

not:

```text
filter()
 ↓
Returns an array
```

---

# 13. Custom `includes()`

### Goal

Check whether an array contains a value.

```js
const numbers = [1, 2, 3];

console.log(numbers.customIncludes(2));
```

Output:

```text
true
```

### Basic Implementation

```js
Array.prototype.customIncludes = function (value) {
    for (let i = 0; i < this.length; i++) {
        if (this[i] === value) {
            return true;
        }
    }

    return false;
};
```

### Important

This simplified implementation uses `===`.

The real `includes()` uses **SameValueZero**, which means behavior differs for special cases such as `NaN`.

A more accurate implementation would account for that.

---

# 14. Custom `flat()`

This is a more advanced implementation.

### Example

```js
const numbers = [1, [2, [3, 4]]];

console.log(numbers.customFlat(2));
```

Expected:

```js
[1, 2, 3, 4]
```

### Basic Recursive Implementation

```js
Array.prototype.customFlat = function (depth = 1) {
    const result = [];

    function flatten(array, currentDepth) {
        for (const value of array) {
            if (
                Array.isArray(value) &&
                currentDepth < depth
            ) {
                flatten(value, currentDepth + 1);
            } else {
                result.push(value);
            }
        }
    }

    flatten(this, 0);

    return result;
};
```

### How It Works

```text
[1, [2, [3, 4]]]
       ↓
flatten
       ↓
[1, 2, [3, 4]]
       ↓
flatten again
       ↓
[1, 2, 3, 4]
```

This connects directly to the recursion concepts from earlier phases.

---

# 15. Array Method Decision Guide

When solving a problem, ask what you actually need.

### Need to change every element?

```text
→ map()
```

### Need only some elements?

```text
→ filter()
```

### Need one final accumulated result?

```text
→ reduce()
```

### Need the first matching element?

```text
→ find()
```

### Need the index of the first matching element?

```text
→ findIndex()
```

### Need to know whether at least one matches?

```text
→ some()
```

### Need to know whether all match?

```text
→ every()
```

### Need to perform an action for each element?

```text
→ forEach()
```

### Need to check whether a value exists?

```text
→ includes()
```

### Need the position of a value?

```text
→ indexOf()
```

### Need to remove/insert elements?

```text
→ splice()
```

### Need a portion without changing the original?

```text
→ slice()
```

### Need to flatten nested arrays?

```text
→ flat()
```

---

# 16. Quick Revision

## Basic Methods

```text
push()      → add to end
pop()       → remove from end
shift()     → remove from beginning
unshift()   → add to beginning

slice()     → copy/extract portion
splice()    → add/remove/replace

concat()    → combine arrays
includes()  → value exists?
indexOf()   → value position?
```

---

## Iteration Methods

```text
forEach()   → perform action
map()       → transform
filter()    → select
reduce()    → accumulate
find()      → first matching element
findIndex() → first matching index
some()      → at least one?
every()     → all?
```

---

## Advanced Methods

```text
flat()        → flatten nested arrays
flatMap()     → map + flat(1)
sort()        → sort elements
reverse()     → reverse array
fill()        → replace elements
Array.from()  → create array from iterable/array-like
Array.isArray() → check if value is an array
```

---

# 17. Interview Questions

## Basic

1. What is an array?
2. How do you add an element to an array?
3. How do you remove the last element?
4. What is the difference between `push()` and `unshift()`?
5. What is the difference between `pop()` and `shift()`?
6. What does `slice()` do?
7. What does `splice()` do?
8. What is the difference between `slice()` and `splice()`?
9. What does `concat()` do?
10. What is the difference between `includes()` and `indexOf()`?

---

## Intermediate

11. What is the difference between `map()` and `forEach()`?
12. What is the difference between `map()` and `filter()`?
13. What is the purpose of `reduce()`?
14. What does `find()` return?
15. What is the difference between `find()` and `filter()`?
16. What is the difference between `find()` and `findIndex()`?
17. What is the difference between `some()` and `every()`?
18. Which array methods mutate the original array?
19. Does `map()` mutate the original array?
20. What is the difference between `flat()` and `flatMap()`?

---

## ⭐ Advanced

21. How does `reduce()` work internally?
22. What happens if no initial value is provided to `reduce()`?
23. How does `sort()` work with numbers?
24. Why does `[10, 2, 30].sort()` produce an unexpected order?
25. What is the difference between shallow copying an array and deep copying it?
26. How does `Array.from()` work?
27. Why does `typeof []` return `"object"`?
28. Why should `Array.isArray()` be used to check for arrays?
29. Implement your own `map()`.
30. Implement your own `filter()`.
31. Implement your own `reduce()`.
32. Implement your own `forEach()`.
33. Implement your own `find()`.
34. Implement your own `includes()`.
35. Implement your own `flat()`.

---

# 18. Must Remember

## ⭐ The Big Seven

```text
forEach → DO something

map     → CHANGE every element

filter  → KEEP some elements

reduce  → COMBINE elements

find    → GET first match

some    → DOES AT LEAST ONE match?

every   → DO ALL match?
```

---

## ⭐ Example

Given:

```js
const numbers = [1, 2, 3, 4, 5];
```

### Double Everything

```js
numbers.map(number => number * 2);
```

Result:

```js
[2, 4, 6, 8, 10]
```

### Get Even Numbers

```js
numbers.filter(number => number % 2 === 0);
```

Result:

```js
[2, 4]
```

### Calculate Total

```js
numbers.reduce(
    (sum, number) => sum + number,
    0
);
```

Result:

```text
15
```

### Find First Number Greater Than 3

```js
numbers.find(number => number > 3);
```

Result:

```text
4
```

### Check If Any Number Is Greater Than 4

```js
numbers.some(number => number > 4);
```

Result:

```text
true
```

### Check If Every Number Is Positive

```js
numbers.every(number => number > 0);
```

Result:

```text
true
```

### Print Every Number

```js
numbers.forEach(number => {
    console.log(number);
});
```

---

# 🧠 Final Mental Model

Don't memorize array methods as random names.

Think in terms of **what you want to accomplish**:

```text
                    ARRAY
                      │
          ┌───────────┴───────────┐
          │                       │
       MODIFY                  INSPECT
          │                       │
   ┌──────┴──────┐          ┌─────┴─────┐
   │             │          │           │
Add/Remove     Transform   Search      Test
   │             │          │           │
push          map         find        some
pop           filter      findIndex   every
shift         reduce      includes
unshift       flat        indexOf
splice
sort
reverse
fill
```

---

# 🔥 The Interview Mental Model

Given:

```js
const users = [
    { name: "Aniket", age: 21, active: true },
    { name: "Rahul", age: 17, active: false },
    { name: "Amit", age: 25, active: true }
];
```

### Get Names

```js
users.map(user => user.name);
```

> **Transform**

---

### Get Active Users

```js
users.filter(user => user.active);
```

> **Select**

---

### Get Total Age

```js
users.reduce(
    (total, user) => total + user.age,
    0
);
```

> **Accumulate**

---

### Find Aniket

```js
users.find(user => user.name === "Aniket");
```

> **First Match**

---

### Is Anyone Under 18?

```js
users.some(user => user.age < 18);
```

> **At Least One?**

---

### Is Everyone an Adult?

```js
users.every(user => user.age >= 18);
```

> **All?**

---

### Print Every User

```js
users.forEach(user => {
    console.log(user.name);
});
```

> **Perform Action**

---

# 🎯 Phase 6 Practice Challenge

Without looking at documentation, implement:

```text
customForEach()
customMap()
customFilter()
customFind()
customIncludes()
customReduce()
customFlat()
```

Then test them with:

```js
const numbers = [1, 2, 3, 4, 5];
```

and:

```js
const users = [
    { name: "Aniket", age: 21 },
    { name: "Rahul", age: 17 },
    { name: "Amit", age: 25 }
];
```

---

## ⭐ Extra Challenge

After implementing the custom methods, answer these without running the code:

### Question 1

```js
const result = [1, 2, 3, 4].map(
    number => number * 2
);
```

What is `result`?

---

### Question 2

```js
const result = [1, 2, 3, 4].filter(
    number => number > 2
);
```

What is `result`?

---

### Question 3

```js
const result = [1, 2, 3, 4].reduce(
    (sum, number) => sum + number,
    0
);
```

What is `result`?

---

### Question 4

```js
const result = [1, 2, 3, 4].find(
    number => number > 2
);
```

What is `result`?

---

### Question 5

```js
const result = [1, 2, 3, 4].some(
    number => number > 3
);
```

What is `result`?

---

### Question 6

```js
const result = [1, 2, 3, 4].every(
    number => number > 0
);
```

What is `result`?

---

### Question 7

```js
const result = [1, 2, 3, 4].forEach(
    number => number * 2
);
```

What is `result`?

> ⭐ **Hint:** `forEach()` does not return the transformed array.

---

## 🎯 Phase 6 Checklist

* [ ] I know `push()`.
* [ ] I know `pop()`.
* [ ] I know `shift()`.
* [ ] I know `unshift()`.
* [ ] I know `slice()`.
* [ ] I know `splice()`.
* [ ] I know `concat()`.
* [ ] I know `includes()`.
* [ ] I know `indexOf()`.
* [ ] I know `forEach()`.
* [ ] I know `map()`.
* [ ] I know `filter()`.
* [ ] I know `reduce()`.
* [ ] I know `find()`.
* [ ] I know `findIndex()`.
* [ ] I know `some()`.
* [ ] I know `every()`.
* [ ] I know `flat()`.
* [ ] I know `flatMap()`.
* [ ] I understand `sort()`.
* [ ] I understand `reverse()`.
* [ ] I understand `fill()`.
* [ ] I know `Array.from()`.
* [ ] I know `Array.isArray()`.
* [ ] I can identify mutating vs non-mutating methods.
* [ ] I can explain `map` vs `filter` vs `reduce`.
* [ ] I can explain `find` vs `filter`.
* [ ] I can explain `some` vs `every`.
* [ ] I can implement `customMap()`.
* [ ] I can implement `customFilter()`.
* [ ] I can implement `customReduce()`.
* [ ] I can implement `customForEach()`.
* [ ] I can implement `customFind()`.
* [ ] I can implement `customIncludes()`.
* [ ] I can implement `customFlat()`.

---

**Phase 6 Status:** 🟡 Revise + Implement

**Next:** Phase 7 — Strings + Numbers + Math + Date/Time
