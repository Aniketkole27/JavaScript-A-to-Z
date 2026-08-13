# JavaScript A→Z Revision

## Phase 2 — Functions ⭐

> **Goal:** Understand how functions work in JavaScript, from basic syntax to closures and recursion.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Intermediate
**Interview Relevance:** Very High

---

## 📚 Contents

1. [Function Declaration](#1-function-declaration)
2. [Function Expression](#2-function-expression)
3. [Arrow Functions](#3-arrow-functions)
4. [Parameters & Arguments](#4-parameters--arguments)
5. [Default Parameters](#5-default-parameters)
6. [Rest Parameters](#6-rest-parameters)
7. [Spread Operator](#7-spread-operator)
8. [Callback Functions](#8-callback-functions)
9. [Higher-Order Functions](#9-higher-order-functions)
10. [Pure Functions](#10-pure-functions)
11. [First-Class Functions](#11-first-class-functions)
12. [IIFE](#12-iife)
13. [Recursion](#13-recursion)
14. [Closures](#14-closures)
15. [Quick Revision](#15-quick-revision)
16. [Interview Questions](#16-interview-questions)

---

# 1. Function Declaration

A **function declaration** is a way to define a named function using the `function` keyword.

### Syntax

```js
function functionName() {
    // code
}
```

### Example

```js
function greet() {
    console.log("Hello!");
}

greet();
```

### Real-World Example

Think of a function as a **machine**.

You give the machine an instruction:

> "Make coffee."

Whenever you need coffee, you use the machine again instead of writing the entire process again.

```js
function makeCoffee() {
    console.log("Grinding beans");
    console.log("Adding hot water");
    console.log("Coffee ready!");
}

makeCoffee();
makeCoffee();
```

### Important Point

Function declarations are **hoisted**, so they can be called before they are defined.

```js
greet();

function greet() {
    console.log("Hello");
}
```

This works because function declarations are hoisted with their function body.

---

# 2. Function Expression

A **function expression** stores a function inside a variable.

```js
const greet = function () {
    console.log("Hello");
};

greet();
```

The function can also have a name:

```js
const greet = function sayHello() {
    console.log("Hello");
};
```

### Function Declaration vs Function Expression

```js
// Declaration
function add(a, b) {
    return a + b;
}

// Expression
const add = function (a, b) {
    return a + b;
};
```

### Important Difference

Function expressions are **not callable before initialization**.

```js
greet(); // ReferenceError

const greet = function () {
    console.log("Hello");
};
```

---

# 3. Arrow Functions

Arrow functions provide a shorter syntax for writing functions.

### Normal Function

```js
function add(a, b) {
    return a + b;
}
```

### Arrow Function

```js
const add = (a, b) => {
    return a + b;
};
```

### Implicit Return

If the function contains only one expression, `{}` and `return` can be removed.

```js
const add = (a, b) => a + b;
```

### One Parameter

Parentheses can be omitted for a single parameter.

```js
const square = x => x * x;
```

### Important Difference: `this`

Arrow functions **do not have their own `this`**.

They inherit `this` from their surrounding lexical scope.

```js
const user = {
    name: "Aniket",

    greet() {
        const sayName = () => {
            console.log(this.name);
        };

        sayName();
    }
};

user.greet();
```

Output:

```text
Aniket
```

### Interview Point

> Arrow functions are not just shorter syntax. Their handling of `this`, `arguments`, `super`, and `new` differs from normal functions.

---

# 4. Parameters & Arguments

### Parameters

Parameters are variables defined when creating a function.

```js
function greet(name) {
    console.log(`Hello ${name}`);
}
```

Here, `name` is a **parameter**.

### Arguments

Arguments are the actual values passed when calling the function.

```js
greet("Aniket");
```

Here, `"Aniket"` is an **argument**.

### Example

```js
function add(a, b) {
    return a + b;
}

add(10, 20);
```

* `a`, `b` → parameters
* `10`, `20` → arguments

### Easy Rule

> **Parameter = placeholder**
> **Argument = actual value**

---

# 5. Default Parameters

Default parameters provide a fallback value when an argument is not provided.

```js
function greet(name = "Guest") {
    console.log(`Hello ${name}`);
}

greet();
```

Output:

```text
Hello Guest
```

If a value is provided:

```js
greet("Aniket");
```

Output:

```text
Hello Aniket
```

### Real-World Example

```js
function createUser(name, role = "user") {
    return {
        name,
        role
    };
}

console.log(createUser("Aniket"));
```

Output:

```js
{
    name: "Aniket",
    role: "user"
}
```

---

# 6. Rest Parameters

The **rest parameter** collects multiple arguments into an array.

It uses `...`.

```js
function sum(...numbers) {
    console.log(numbers);
}

sum(10, 20, 30, 40);
```

Output:

```js
[10, 20, 30, 40]
```

### Example

```js
function sum(...numbers) {
    return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20, 30));
```

Output:

```text
60
```

### With Normal Parameters

```js
function introduce(name, ...skills) {
    console.log(name);
    console.log(skills);
}

introduce(
    "Aniket",
    "JavaScript",
    "React",
    "Node.js"
);
```

Output:

```text
Aniket
["JavaScript", "React", "Node.js"]
```

### Important Rule

The rest parameter must be the **last parameter**.

```js
function test(a, ...rest) {
    // valid
}
```

This is invalid:

```js
function test(...rest, a) {
    // SyntaxError
}
```

---

# 7. Spread Operator

The spread operator also uses `...`, but its purpose is different.

It **expands** an iterable or object into individual elements/properties.

### Array Example

```js
const numbers = [1, 2, 3];

console.log(...numbers);
```

Output:

```text
1 2 3
```

### Copy an Array

```js
const original = [1, 2, 3];

const copy = [...original];
```

### Merge Arrays

```js
const frontend = ["HTML", "CSS"];
const backend = ["Node.js", "Express"];

const skills = [...frontend, ...backend];

console.log(skills);
```

Output:

```js
["HTML", "CSS", "Node.js", "Express"]
```

### Object Spread

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

### Rest vs Spread

| Rest                        | Spread                                  |
| --------------------------- | --------------------------------------- |
| Collects values             | Expands values                          |
| Used in function parameters | Used with arrays/objects/function calls |
| Creates an array            | Expands an iterable/object              |
| `function sum(...numbers)`  | `sum(...numbers)`                       |

### Easy Rule

> **Rest = collect**
> **Spread = expand**

---

# 8. Callback Functions

A **callback function** is a function passed as an argument to another function.

```js
function greet(name) {
    console.log(`Hello ${name}`);
}

function processUser(callback) {
    callback("Aniket");
}

processUser(greet);
```

Here:

* `greet` → callback
* `processUser` → function receiving the callback

### Real-World Example

Imagine ordering food.

You tell the restaurant:

> "When my food is ready, call me."

The callback is like the instruction:

```js
function foodReady() {
    console.log("Food is ready!");
}
```

The restaurant can execute it later.

### JavaScript Example

```js
setTimeout(() => {
    console.log("Food is ready!");
}, 2000);
```

The arrow function is a callback.

---

# 9. Higher-Order Functions

A **higher-order function** is a function that:

1. Takes another function as an argument, or
2. Returns another function.

### Example 1 — Takes Function

```js
function calculate(a, b, operation) {
    return operation(a, b);
}

const add = (a, b) => a + b;

console.log(calculate(10, 20, add));
```

### Example 2 — Returns Function

```js
function multiplier(factor) {
    return function (number) {
        return number * factor;
    };
}

const double = multiplier(2);

console.log(double(5));
```

Output:

```text
10
```

### Common Higher-Order Functions

JavaScript array methods such as:

```js
map()
filter()
reduce()
forEach()
some()
every()
find()
sort()
```

use functions as arguments.

Example:

```js
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(number => number * 2);
```

`map()` is a higher-order function because it receives a function.

---

# 10. Pure Functions

A **pure function** always:

1. Produces the same output for the same input.
2. Does not cause side effects.

### Pure Function

```js
function add(a, b) {
    return a + b;
}
```

```js
add(10, 20); // 30
add(10, 20); // 30
```

Same input → same output.

### Impure Function

```js
let count = 0;

function increment() {
    count++;
}
```

This function changes external state.

Therefore, it is impure.

### Another Impure Example

```js
function updateUser(user) {
    user.name = "Aniket";
    return user;
}
```

The original object is mutated.

### Why Pure Functions Matter

Pure functions are easier to:

* Test
* Debug
* Predict
* Reuse
* Reason about

### React Connection

Pure functions are especially important in React because predictable state transformations make rendering and state management easier to reason about.

---

# 11. First-Class Functions

In JavaScript, functions are **first-class citizens**.

This means functions can be treated like values.

A function can be:

* Stored in a variable
* Passed as an argument
* Returned from another function
* Stored in an object
* Stored in an array

### Store in Variable

```js
const greet = function () {
    console.log("Hello");
};
```

### Pass as Argument

```js
function execute(callback) {
    callback();
}

execute(greet);
```

### Return from Function

```js
function createGreeting() {
    return function () {
        console.log("Hello");
    };
}

const greet = createGreeting();

greet();
```

### Key Idea

> Functions in JavaScript behave like normal values.

This is the foundation of:

* Callbacks
* Higher-order functions
* Closures
* Functional programming

---

# 12. IIFE

**IIFE** stands for:

> **Immediately Invoked Function Expression**

It is a function that is created and executed immediately.

### Syntax

```js
(function () {
    console.log("Hello");
})();
```

The function runs immediately after it is created.

### Arrow Function IIFE

```js
(() => {
    console.log("Hello");
})();
```

### Real-World Use

Before ES modules became common, IIFEs were often used to create a private scope and avoid polluting the global scope.

```js
(function () {
    const secret = "12345";

    console.log(secret);
})();
```

This variable cannot be accessed outside the function.

```js
console.log(secret); // ReferenceError
```

### Modern JavaScript

IIFEs are less common today because we have:

* `let`
* `const`
* ES Modules
* Block scope

But they are still important for interviews and understanding older JavaScript patterns.

---

# 13. Recursion

**Recursion** is when a function calls itself.

A recursive function normally contains:

1. **Base case** → stops recursion
2. **Recursive case** → calls itself again

### Example

```js
function countdown(number) {
    if (number === 0) {
        return;
    }

    console.log(number);

    countdown(number - 1);
}

countdown(5);
```

Output:

```text
5
4
3
2
1
```

### Factorial Example

Mathematically:

```text
5! = 5 × 4 × 3 × 2 × 1
```

JavaScript:

```js
function factorial(n) {
    if (n === 1) {
        return 1;
    }

    return n * factorial(n - 1);
}

console.log(factorial(5));
```

Output:

```text
120
```

### How It Works

```text
factorial(5)
    ↓
5 × factorial(4)
    ↓
5 × 4 × factorial(3)
    ↓
5 × 4 × 3 × factorial(2)
    ↓
5 × 4 × 3 × 2 × factorial(1)
    ↓
120
```

### Important

Always make sure recursion has a condition that eventually stops it.

Otherwise:

```js
function infinite() {
    infinite();
}
```

This eventually causes:

```text
Maximum call stack size exceeded
```

---

# 14. Closures ⭐⭐⭐⭐⭐

Closures are one of the **most important JavaScript concepts for interviews**.

A closure happens when a function **remembers and can access variables from its lexical scope even after the outer function has finished executing**.

---

## Basic Example

```js
function outer() {
    let message = "Hello";

    function inner() {
        console.log(message);
    }

    return inner;
}

const greet = outer();

greet();
```

Output:

```text
Hello
```

The important question is:

> How can `inner()` still access `message` after `outer()` has finished?

Because `inner()` forms a **closure** over its surrounding lexical environment.

---

## Counter Example

```js
function counter() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}
```

Now:

```js
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

## Why Does `count` Remain Available?

When we execute:

```js
const increment = counter();
```

`counter()` creates:

```js
let count = 0;
```

Then it returns the inner function.

The returned function still has a reference to the variables in its surrounding lexical environment.

So even though:

```js
counter();
```

has finished executing, the `count` variable is still reachable through the returned function.

Conceptually:

```text
counter()
   │
   ├── count = 0
   │
   └── returns inner function
            │
            └── remembers count
```

When we call:

```js
increment();
```

the inner function accesses the remembered `count`.

Then:

```js
count++;
```

changes it from:

```text
0 → 1
```

Next call:

```text
1 → 2
```

Next call:

```text
2 → 3
```

---

## Closure in Simple Words

Think of a closure as:

> **A function carrying its surrounding environment with it.**

The function remembers the variables it needs from its lexical scope.

---

## Real-World Example — Private Data

Closures can be used to create private variables.

```js
function bankAccount(initialBalance) {
    let balance = initialBalance;

    return {
        deposit(amount) {
            balance += amount;
        },

        getBalance() {
            return balance;
        }
    };
}

const account = bankAccount(1000);

account.deposit(500);

console.log(account.getBalance());
```

Output:

```text
1500
```

But this does not work:

```js
console.log(account.balance);
```

Because `balance` is not directly accessible.

The closure keeps it private.

---

## Multiple Closures

Each invocation gets its own closure.

```js
function counter() {
    let count = 0;

    return function () {
        count++;
        return count;
    };
}

const counter1 = counter();
const counter2 = counter();

console.log(counter1());
console.log(counter1());

console.log(counter2());
console.log(counter2());
```

Output:

```text
1
2

1
2
```

`counter1` and `counter2` have **different `count` variables**.

Conceptually:

```text
counter1 → count = 2

counter2 → count = 2
```

They do not share the same `count`.

---

## Closure + Loop Example

Closures are commonly asked with loops.

### Problem

```js
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}
```

Output:

```text
3
3
3
```

Why?

Because `var` is function-scoped, so all callbacks refer to the same `i`.

Using `let`:

```js
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000);
}
```

Output:

```text
0
1
2
```

With `let`, each iteration gets its own block-scoped binding.

---

## Where Are Closures Used?

Closures appear in many real JavaScript applications:

* Data privacy
* Function factories
* Callbacks
* Event handlers
* Timers
* Memoization
* Currying
* React hooks
* State management

### React Connection

Closures are important when understanding:

```js
useState()
useEffect()
useCallback()
event handlers
```

For example:

```js
function Component() {
    const [count, setCount] = useState(0);

    function handleClick() {
        console.log(count);
    }

    return <button onClick={handleClick}>Click</button>;
}
```

`handleClick` can access `count` from the surrounding scope because of JavaScript's lexical scoping and closures.

---

# 15. Quick Revision

## Function Declaration

```js
function add(a, b) {
    return a + b;
}
```

> Named function declaration; function declarations are hoisted.

---

## Function Expression

```js
const add = function (a, b) {
    return a + b;
};
```

> Function stored inside a variable.

---

## Arrow Function

```js
const add = (a, b) => a + b;
```

> Shorter function syntax with different `this` behavior.

---

## Parameters

```js
function greet(name) {}
```

> Variables defined by the function.

---

## Arguments

```js
greet("Aniket");
```

> Actual values passed to the function.

---

## Default Parameters

```js
function greet(name = "Guest") {}
```

> Provides a fallback value.

---

## Rest Parameter

```js
function sum(...numbers) {}
```

> Collects multiple arguments into an array.

---

## Spread Operator

```js
const copy = [...numbers];
```

> Expands an iterable or object.

---

## Callback

```js
setTimeout(() => {
    console.log("Done");
}, 1000);
```

> Function passed to another function.

---

## Higher-Order Function

```js
function execute(callback) {
    callback();
}
```

> Function that accepts or returns another function.

---

## Pure Function

```js
function add(a, b) {
    return a + b;
}
```

> Same input → same output, with no side effects.

---

## First-Class Function

```js
const greet = () => console.log("Hello");
```

> Functions can be treated like values.

---

## IIFE

```js
(() => {
    console.log("Runs immediately");
})();
```

> Function that executes immediately after creation.

---

## Recursion

```js
function count(n) {
    if (n === 0) return;

    count(n - 1);
}
```

> Function that calls itself.

---

## Closure

```js
function outer() {
    let value = 10;

    return function () {
        return value;
    };
}
```

> Function that remembers variables from its lexical scope.

---

# 16. Interview Questions

### Basic

1. What is a function in JavaScript?
2. What is the difference between function declaration and function expression?
3. What is an arrow function?
4. What are parameters and arguments?
5. What are default parameters?
6. What are rest parameters?
7. What is the difference between rest and spread?
8. What is a callback function?
9. What is a higher-order function?
10. What is a pure function?
11. What does it mean that functions are first-class citizens?
12. What is an IIFE?
13. What is recursion?

### Important

14. Are function declarations hoisted?
15. Are function expressions hoisted?
16. What is the difference between normal functions and arrow functions?
17. Why can't the rest parameter appear before another parameter?
18. What is the difference between a callback and a higher-order function?
19. Why are pure functions useful?
20. Explain recursion with a real example.

### ⭐ Advanced Interview Questions

21. What is a closure?
22. Why does a closure remember variables after the outer function finishes?
23. Explain the counter example using closures.
24. How are closures related to lexical scope?
25. Explain closures with `setTimeout`.
26. Explain the `var` vs `let` loop closure problem.
27. How can closures be used to create private variables?
28. Can multiple closures have separate copies of the same variable?
29. Where are closures used in React?
30. Explain how a closure is different from simply accessing a global variable.

---

# 🎯 Phase 2 — Must Remember

Before moving to Phase 3, you should be able to explain these **without looking at your notes**:

```text
Function Declaration
        ↓
Function Expression
        ↓
Arrow Function
        ↓
Parameters & Arguments
        ↓
Default Parameters
        ↓
Rest & Spread
        ↓
Callback
        ↓
Higher-Order Function
        ↓
Pure Function
        ↓
First-Class Function
        ↓
IIFE
        ↓
Recursion
        ↓
🔥 Closures
```

### The most important concepts

> **1. Callback**
> **2. Higher-Order Function**
> **3. First-Class Function**
> **4. Pure Function**
> **5. Recursion**
> **6. Closures ⭐⭐⭐⭐⭐**

---

## 🧠 One-Line Memory Trick

```text
Function → reusable logic

Callback → function passed to another function

Higher-Order → function takes/returns a function

First-Class → functions behave like values

Pure → same input → same output

IIFE → execute immediately

Recursion → function calls itself

Closure → function remembers its outer scope
```

---

## 🔗 Related JavaScript Concepts

After completing this phase, the next concepts that become much easier are:

* Lexical Scope
* Execution Context
* `this`
* Call Stack
* Event Loop
* Promises
* Async/Await
* Functional Programming
* React Hooks
* Memoization
* Currying

---

**Phase 2 Status:** 🟡 Revise + Practice

**Next:** Phase 3 — Objects & Prototypes
