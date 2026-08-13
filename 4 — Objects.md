# JavaScript A→Z Revision

## Phase 4 — Objects ⭐⭐⭐⭐⭐

> **Goal:** Understand how JavaScript objects store, access, modify, copy, and organize data.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Intermediate
**Interview Relevance:** Extremely High

> ⚠️ **Important:** Objects are one of the most frequently used data structures in JavaScript. Understanding **references, mutation, shallow copies, and deep copies** is especially important for React and state management.

---

## 📚 Contents

1. [Creating Objects](#1-creating-objects)
2. [Accessing Properties](#2-accessing-properties)
3. [Adding Properties](#3-adding-properties)
4. [Removing Properties](#4-removing-properties)
5. [Computed Properties](#5-computed-properties)
6. [Destructuring](#6-destructuring)
7. [Nested Objects](#7-nested-objects)
8. [Object Methods](#8-object-methods)
9. [`Object.keys()`](#9-objectkeys)
10. [`Object.values()`](#10-objectvalues)
11. [`Object.entries()`](#11-objectentries)
12. [`Object.assign()`](#12-objectassign)
13. [Spread Operator with Objects](#13-spread-operator-with-objects)
14. [References in JavaScript](#14-references-in-javascript)
15. [Shallow Copy](#15-shallow-copy)
16. [Deep Copy](#16-deep-copy)
17. [Why `{ ...user }` Is Not a Deep Copy](#17-why--user--is-not-a-deep-copy)
18. [Shallow Copy vs Deep Copy](#18-shallow-copy-vs-deep-copy)
19. [Quick Revision](#19-quick-revision)
20. [Interview Questions](#20-interview-questions)
21. [Must Remember](#21-must-remember)

---

# 1. Creating Objects

An **object** is a collection of key-value pairs.

### Basic Syntax

```js
const user = {
    name: "Aniket",
    age: 21,
    role: "Developer"
};
```

Here:

```text
name  → "Aniket"
age   → 21
role  → "Developer"
```

* `name`, `age`, `role` → keys/properties
* `"Aniket"`, `21`, `"Developer"` → values

---

## Real-World Example

Think about a **user profile**.

A real user has:

```text
Name
Age
Email
Address
Skills
```

We can represent this information using an object:

```js
const user = {
    name: "Aniket",
    age: 21,
    email: "aniket@example.com",
    skills: ["JavaScript", "React", "Node.js"]
};
```

Objects allow us to group related information together.

---

## Empty Object

```js
const user = {};
```

Properties can be added later.

---

## Using `new Object()`

You can also create an object using:

```js
const user = new Object();

user.name = "Aniket";
```

However, the object literal syntax is preferred:

```js
const user = {
    name: "Aniket"
};
```

---

# 2. Accessing Properties

There are two primary ways to access object properties:

1. Dot notation
2. Bracket notation

---

## Dot Notation

```js
const user = {
    name: "Aniket",
    age: 21
};

console.log(user.name);
console.log(user.age);
```

Output:

```text
Aniket
21
```

---

## Bracket Notation

```js
console.log(user["name"]);
console.log(user["age"]);
```

Output:

```text
Aniket
21
```

---

## When to Use Bracket Notation

Bracket notation becomes important when the property name is stored in a variable.

```js
const user = {
    name: "Aniket",
    age: 21
};

const property = "name";

console.log(user[property]);
```

Output:

```text
Aniket
```

This does **not** work as expected:

```js
console.log(user.property);
```

That looks for a property literally named `"property"`.

---

## Properties With Special Names

Bracket notation can access properties that cannot be conveniently accessed with dot notation.

```js
const user = {
    "first-name": "Aniket"
};

console.log(user["first-name"]);
```

---

# 3. Adding Properties

Properties can be added after creating an object.

```js
const user = {
    name: "Aniket"
};

user.age = 21;
user.role = "Developer";

console.log(user);
```

Result:

```js
{
    name: "Aniket",
    age: 21,
    role: "Developer"
}
```

---

## Updating Properties

If the property already exists, assigning a new value updates it.

```js
const user = {
    name: "Aniket",
    age: 20
};

user.age = 21;
```

Now:

```js
console.log(user.age);
```

Output:

```text
21
```

---

# 4. Removing Properties

Use the `delete` operator.

```js
const user = {
    name: "Aniket",
    age: 21,
    role: "Developer"
};

delete user.role;

console.log(user);
```

Result:

```js
{
    name: "Aniket",
    age: 21
}
```

### Important

`delete` removes the property from the object.

```js
delete user.age;
```

It does not set the property to `undefined`.

Compare:

```js
user.age = undefined;
```

with:

```js
delete user.age;
```

The first keeps the property:

```js
{
    age: undefined
}
```

The second removes it completely.

---

# 5. Computed Properties

Computed properties allow us to dynamically create object property names.

Use square brackets:

```js
const key = "name";

const user = {
    [key]: "Aniket"
};

console.log(user);
```

Output:

```js
{
    name: "Aniket"
}
```

---

## Real-World Example

Suppose a form dynamically updates different fields.

```js
const field = "email";
const value = "aniket@example.com";

const user = {
    [field]: value
};

console.log(user);
```

Result:

```js
{
    email: "aniket@example.com"
}
```

This is extremely useful when working with:

* Forms
* Dynamic APIs
* Redux state
* React state updates

---

## Dynamic Property Name

```js
const property = "age";

const user = {
    name: "Aniket",
    [property]: 21
};
```

Result:

```js
{
    name: "Aniket",
    age: 21
}
```

---

# 6. Destructuring

**Destructuring** allows you to extract values from objects into variables.

### Without Destructuring

```js
const user = {
    name: "Aniket",
    age: 21
};

const name = user.name;
const age = user.age;
```

### With Destructuring

```js
const user = {
    name: "Aniket",
    age: 21
};

const { name, age } = user;
```

Now:

```js
console.log(name);
console.log(age);
```

Output:

```text
Aniket
21
```

---

## Rename During Destructuring

```js
const user = {
    name: "Aniket"
};

const { name: userName } = user;

console.log(userName);
```

Output:

```text
Aniket
```

Here:

```text
name → property name
userName → local variable name
```

---

## Default Values

```js
const user = {
    name: "Aniket"
};

const { name, age = 21 } = user;
```

If `age` doesn't exist, `21` is used.

---

## Function Parameters

Destructuring is commonly used in function parameters.

```js
function printUser({ name, age }) {
    console.log(name);
    console.log(age);
}

printUser({
    name: "Aniket",
    age: 21
});
```

This is very common in React components:

```js
function UserCard({ name, age }) {
    return (
        <div>
            <h2>{name}</h2>
            <p>{age}</p>
        </div>
    );
}
```

---

# 7. Nested Objects

Objects can contain other objects.

```js
const user = {
    name: "Aniket",

    address: {
        city: "Pune",
        state: "Maharashtra"
    }
};
```

Here:

```text
user
│
├── name
│
└── address
    ├── city
    └── state
```

---

## Accessing Nested Properties

```js
console.log(user.address.city);
```

Output:

```text
Pune
```

Or:

```js
console.log(user["address"]["city"]);
```

---

## Nested Destructuring

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const {
    name,
    address: { city }
} = user;

console.log(name);
console.log(city);
```

Output:

```text
Aniket
Pune
```

---

# 8. Object Methods

When a function is stored inside an object, it is commonly called a **method**.

```js
const user = {
    name: "Aniket",

    greet() {
        console.log(`Hello, ${this.name}`);
    }
};

user.greet();
```

Output:

```text
Hello, Aniket
```

---

## `this` in Object Methods

Inside a normal object method:

```js
const user = {
    name: "Aniket",

    greet() {
        console.log(this.name);
    }
};

user.greet();
```

`this` refers to the object used to call the method.

```text
user.greet()
     ↓
this → user
```

So:

```js
this.name
```

means:

```js
user.name
```

> ⚠️ The behavior of `this` deserves its own phase because it has many important rules.

---

# 9. `Object.keys()`

`Object.keys()` returns an array containing an object's **own enumerable property names**.

```js
const user = {
    name: "Aniket",
    age: 21,
    role: "Developer"
};

console.log(Object.keys(user));
```

Output:

```js
["name", "age", "role"]
```

---

## Real-World Use

Suppose you want to know how many properties an object contains:

```js
const user = {
    name: "Aniket",
    age: 21,
    role: "Developer"
};

console.log(Object.keys(user).length);
```

Output:

```text
3
```

---

## Iterating

```js
Object.keys(user).forEach(key => {
    console.log(key);
});
```

Output:

```text
name
age
role
```

---

# 10. `Object.values()`

`Object.values()` returns an array containing an object's values.

```js
const user = {
    name: "Aniket",
    age: 21,
    role: "Developer"
};

console.log(Object.values(user));
```

Output:

```js
["Aniket", 21, "Developer"]
```

---

## Example

```js
Object.values(user).forEach(value => {
    console.log(value);
});
```

---

# 11. `Object.entries()`

`Object.entries()` returns an array containing `[key, value]` pairs.

```js
const user = {
    name: "Aniket",
    age: 21
};

console.log(Object.entries(user));
```

Output:

```js
[
    ["name", "Aniket"],
    ["age", 21]
]
```

---

## Iterating With `Object.entries()`

```js
Object.entries(user).forEach(([key, value]) => {
    console.log(key, value);
});
```

Output:

```text
name Aniket
age 21
```

---

## Easy Comparison

```text
Object.keys()
     ↓
["name", "age"]

Object.values()
     ↓
["Aniket", 21]

Object.entries()
     ↓
[["name", "Aniket"], ["age", 21]]
```

---

# 12. `Object.assign()`

`Object.assign()` copies properties from one or more source objects into a target object.

### Example

```js
const user = {
    name: "Aniket"
};

const details = {
    age: 21
};

Object.assign(user, details);

console.log(user);
```

Result:

```js
{
    name: "Aniket",
    age: 21
}
```

---

## Creating a New Object

A common pattern is:

```js
const user = {
    name: "Aniket"
};

const details = {
    age: 21
};

const result = Object.assign({}, user, details);

console.log(result);
```

Result:

```js
{
    name: "Aniket",
    age: 21
}
```

The empty object becomes the target.

---

## Important

`Object.assign()` performs a **shallow copy**.

This means nested objects are still shared.

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const copy = Object.assign({}, user);

copy.address.city = "Mumbai";

console.log(user.address.city);
```

Output:

```text
Mumbai
```

Why?

Because both objects reference the same nested `address` object.

---

# 13. Spread Operator with Objects

The spread operator can copy enumerable properties into a new object.

```js
const user = {
    name: "Aniket",
    age: 21
};

const copy = {
    ...user
};
```

Now:

```js
console.log(copy);
```

Result:

```js
{
    name: "Aniket",
    age: 21
}
```

---

## Adding Properties

Spread is commonly used to create a new object while changing or adding properties.

```js
const user = {
    name: "Aniket",
    age: 21
};

const updatedUser = {
    ...user,
    role: "Developer"
};
```

---

## Updating a Property

```js
const user = {
    name: "Aniket",
    age: 21
};

const updatedUser = {
    ...user,
    age: 22
};
```

The later `age` property overrides the earlier one.

---

## Important

Object spread creates a **shallow copy**, not a deep copy.

This is one of the most important concepts in this phase.

---

# 14. References in JavaScript

Before understanding shallow and deep copies, you must understand **references**.

Consider:

```js
const user = {
    name: "Aniket"
};

const copy = user;
```

You might think:

```text
user → Object A

copy → Object B
```

But that is not what happens.

Both variables point to the **same object**.

```text
user ─────┐
          ↓
       Object
          ↑
copy ─────┘
```

Therefore:

```js
copy.name = "Rahul";

console.log(user.name);
```

Output:

```text
Rahul
```

Because there is only one object.

---

## Why?

Objects are reference values.

When you assign:

```js
const copy = user;
```

JavaScript does not create another object.

It copies the reference to the existing object.

---

# 15. Shallow Copy

A **shallow copy** creates a new outer object, but nested objects/arrays are still shared.

Example:

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const copy = {
    ...user
};
```

Now:

```text
user
 │
 ├── name → "Aniket"
 │
 └── address ─────┐
                  ↓
               Object A
                  ↑
                  │
copy              │
 │                │
 ├── name         │
 │                │
 └── address ─────┘
```

The outer objects are different:

```text
user !== copy
```

But their nested `address` objects are the same:

```text
user.address === copy.address
```

Therefore:

```js
console.log(user === copy);
```

Output:

```text
false
```

But:

```js
console.log(user.address === copy.address);
```

Output:

```text
true
```

---

# 16. Deep Copy

A **deep copy** creates a completely independent copy of the object, including nested objects and arrays.

Example:

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const copy = structuredClone(user);
```

Now:

```js
copy.address.city = "Mumbai";

console.log(user.address.city);
```

Output:

```text
Pune
```

The nested object is independent.

```js
console.log(user === copy);
```

Output:

```text
false
```

And:

```js
console.log(user.address === copy.address);
```

Output:

```text
false
```

---

## `structuredClone()`

Modern JavaScript provides:

```js
structuredClone()
```

for deep cloning many kinds of structured data.

Example:

```js
const original = {
    name: "Aniket",
    address: {
        city: "Pune"
    },
    skills: ["JavaScript", "React"]
};

const copy = structuredClone(original);
```

Now nested objects and arrays are independently cloned.

---

## JSON Deep Copy

You may also see:

```js
const copy = JSON.parse(JSON.stringify(user));
```

This can work for simple JSON-compatible data, but it has important limitations.

For example, JSON serialization does not preserve every JavaScript value/type correctly, including things such as:

* `undefined`
* Functions
* `Symbol`
* `BigInt`
* `Date` objects as `Date` instances
* `Map`
* `Set`

Therefore, don't blindly use JSON serialization as a universal deep-copy solution.

For modern JavaScript, prefer:

```js
structuredClone(value);
```

when its supported data types fit your use case.

---

# 17. Why `{ ...user }` Is Not a Deep Copy

This is the **most important concept of Phase 4**.

Consider:

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};
```

Now:

```js
const copy = {
    ...user
};
```

It is tempting to think:

```text
user
 ↓
complete copy
 ↓
copy
```

But that's not what happens.

The spread operator only creates a new **outer object**.

For primitive values:

```js
name: "Aniket"
```

the value is copied.

For the nested object:

```js
address: {
    city: "Pune"
}
```

the reference is copied.

---

## Visual Representation

Original:

```text
user
 │
 ├── name ──────→ "Aniket"
 │
 └── address ───→ Object A
                    │
                    └── city → "Pune"
```

After:

```js
const copy = { ...user };
```

we get:

```text
user
 │
 ├── name ──────→ "Aniket"
 │
 └── address ─────────┐
                      │
                      ▼
                   Object A
                      │
                      └── city → "Pune"

copy
 │
 ├── name ──────→ "Aniket"
 │
 └── address ─────────┘
```

So:

```js
user !== copy
```

but:

```js
user.address === copy.address
```

---

## Demonstration

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const copy = {
    ...user
};

copy.address.city = "Mumbai";

console.log(copy.address.city);
console.log(user.address.city);
```

Output:

```text
Mumbai
Mumbai
```

Changing `copy.address.city` also changed `user.address.city`.

Why?

Because:

```js
copy.address
```

and:

```js
user.address
```

point to the **same nested object**.

---

## But What About the Top Level?

Consider:

```js
copy.name = "Rahul";

console.log(user.name);
```

Output:

```text
Aniket
```

Why didn't `user.name` change?

Because the outer object was copied.

```text
user.name
     ↓
"Aniket"

copy.name
     ↓
"Aniket"
```

They are separate properties on separate outer objects.

---

## The Key Rule

> **Spread creates a new object at the first level, but nested objects and arrays remain references to the original objects.**

This is exactly what **shallow copy** means.

---

# 18. Shallow Copy vs Deep Copy

## Shallow Copy

```js
const copy = {
    ...user
};
```

Creates:

```text
New outer object
       ↓
Nested objects shared
```

---

## Deep Copy

```js
const copy = structuredClone(user);
```

Creates:

```text
New outer object
       ↓
New nested objects
       ↓
New nested arrays
       ↓
Independent structure
```

---

## Comparison

| Feature                         | Shallow Copy  | Deep Copy               |
| ------------------------------- | ------------- | ----------------------- |
| New outer object                | ✅             | ✅                       |
| Nested object copied            | ❌             | ✅                       |
| Nested array copied             | ❌             | ✅                       |
| Nested references shared        | ✅             | ❌                       |
| `user === copy`                 | `false`       | `false`                 |
| `user.address === copy.address` | `true`        | `false`                 |
| Example                         | `{ ...user }` | `structuredClone(user)` |

---

## Why This Matters in React

This concept is extremely important in React.

Suppose:

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};
```

You should avoid directly mutating state:

```js
user.address.city = "Mumbai";
```

Instead, create new objects at the levels you modify:

```js
setUser(prev => ({
    ...prev,
    address: {
        ...prev.address,
        city: "Mumbai"
    }
}));
```

Here:

```text
new user object
      ↓
new address object
      ↓
city updated
```

The unchanged data can continue to be reused.

This pattern is called **immutable updating** and is fundamental to React state management.

---

# 19. Quick Revision

## Creating Object

```js
const user = {
    name: "Aniket"
};
```

> Collection of key-value pairs.

---

## Accessing

```js
user.name;
user["name"];
```

> Dot and bracket notation.

---

## Adding

```js
user.age = 21;
```

---

## Removing

```js
delete user.age;
```

---

## Computed Property

```js
const key = "name";

const user = {
    [key]: "Aniket"
};
```

> Dynamically creates a property name.

---

## Destructuring

```js
const { name, age } = user;
```

> Extracts properties into variables.

---

## Nested Object

```js
const user = {
    address: {
        city: "Pune"
    }
};
```

---

## Object Method

```js
const user = {
    greet() {
        console.log("Hello");
    }
};
```

---

## `Object.keys()`

```js
Object.keys(user);
```

> Returns property names.

---

## `Object.values()`

```js
Object.values(user);
```

> Returns property values.

---

## `Object.entries()`

```js
Object.entries(user);
```

> Returns `[key, value]` pairs.

---

## `Object.assign()`

```js
Object.assign({}, user);
```

> Copies enumerable properties into a target object. The copy is shallow.

---

## Spread

```js
const copy = {
    ...user
};
```

> Creates a shallow copy of an object.

---

## Reference

```js
const copy = user;
```

> Both variables reference the same object.

---

## Shallow Copy

```js
const copy = {
    ...user
};
```

> New outer object, shared nested references.

---

## Deep Copy

```js
const copy = structuredClone(user);
```

> Creates an independent clone of supported structured data.

---

# 20. Interview Questions

## Basic

1. What is an object in JavaScript?
2. How do you create an object?
3. How do you access an object property?
4. What is the difference between dot notation and bracket notation?
5. How do you add a property to an object?
6. How do you remove a property from an object?
7. What are computed properties?
8. What is object destructuring?
9. What are nested objects?
10. What is an object method?

---

## Intermediate

11. What does `Object.keys()` return?
12. What does `Object.values()` return?
13. What does `Object.entries()` return?
14. What is `Object.assign()`?
15. What is the difference between `Object.assign()` and spread syntax?
16. What is the difference between copying an object and referencing an object?
17. What is a shallow copy?
18. What is a deep copy?
19. Why is `{ ...object }` only a shallow copy?
20. How can you create a deep copy in modern JavaScript?

---

## ⭐ Advanced

21. What happens internally when you write `const copy = user`?
22. Why does changing `copy.name` not affect `user.name` after using spread?
23. Why does changing `copy.address.city` affect `user.address.city` after using spread?
24. Explain `user === copy` vs `user.address === copy.address`.
25. What is the difference between value copying and reference copying?
26. What are the limitations of `JSON.parse(JSON.stringify(obj))`?
27. When would you use `structuredClone()`?
28. Why is immutability important in React?
29. How would you update a deeply nested React state object without mutation?
30. Explain shallow copy vs deep copy using a real-world example.

---

# 21. Must Remember

## ⭐ Object Reference

```js
const user = {
    name: "Aniket"
};

const copy = user;
```

Both point to the same object.

```text
user ────┐
         ↓
      Object
         ↑
copy ────┘
```

---

## ⭐ Shallow Copy

```js
const copy = {
    ...user
};
```

New outer object:

```text
user  → Object A
copy  → Object B
```

But nested references can still be shared:

```text
user.address ───┐
                ↓
             Object C
                ↑
copy.address ───┘
```

---

## ⭐ Deep Copy

```js
const copy = structuredClone(user);
```

Creates independent nested objects:

```text
user.address → Object A

copy.address → Object B
```

Therefore:

```js
user.address !== copy.address;
```

---

# 🧠 Final Mental Model

Remember objects using this hierarchy:

```text
OBJECT
 │
 ├── Properties
 │     ├── Primitive values
 │     └── Objects / Arrays
 │
 ├── Access
 │     ├── Dot notation
 │     └── Bracket notation
 │
 ├── Modification
 │     ├── Add
 │     ├── Update
 │     └── Delete
 │
 ├── Extraction
 │     └── Destructuring
 │
 ├── Inspection
 │     ├── Object.keys()
 │     ├── Object.values()
 │     └── Object.entries()
 │
 └── Copying
       │
       ├── Reference
       │     └── same object
       │
       ├── Shallow Copy
       │     └── { ...obj }
       │
       └── Deep Copy
             └── structuredClone()
```

---

# 🎯 One Concept You MUST Be Able to Explain

Given:

```js
const user = {
    name: "Aniket",
    address: {
        city: "Pune"
    }
};

const copy = {
    ...user
};
```

You should immediately know:

```js
user === copy
// false

user.address === copy.address
// true
```

And therefore:

```js
copy.address.city = "Mumbai";

console.log(user.address.city);
// "Mumbai"
```

Because:

> **Spread copies the top-level properties, but nested objects are copied as references.**

For a deep clone:

```js
const copy = structuredClone(user);
```

Now:

```js
user === copy
// false

user.address === copy.address
// false
```

---

## 🎯 Phase 4 Checklist

* [ ] I can create objects.
* [ ] I can access properties using dot notation.
* [ ] I can access dynamic properties using bracket notation.
* [ ] I can add, update, and remove properties.
* [ ] I understand computed properties.
* [ ] I understand object destructuring.
* [ ] I can work with nested objects.
* [ ] I understand object methods.
* [ ] I know `Object.keys()`.
* [ ] I know `Object.values()`.
* [ ] I know `Object.entries()`.
* [ ] I know `Object.assign()`.
* [ ] I understand object spread.
* [ ] I understand references.
* [ ] I can explain shallow copy.
* [ ] I can explain deep copy.
* [ ] I can explain why `{ ...user }` is not a deep copy.
* [ ] I understand why this matters in React state updates.
* [ ] I can explain `user === copy`.
* [ ] I can explain `user.address === copy.address`.

---

**Phase 4 Status:** 🟡 Revise + Practice

**Next:** Phase 5 — `this` + Prototypes + Classes
