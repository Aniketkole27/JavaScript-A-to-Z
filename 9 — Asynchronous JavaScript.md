# JavaScript A→Z Revision

## Phase 9 — Asynchronous JavaScript ⭐⭐⭐

> **Goal:** Understand how JavaScript handles asynchronous operations using the Call Stack, Web APIs, queues, Event Loop, Callbacks, Promises, and `async/await`.

**Importance:** ⭐⭐⭐⭐⭐
**Difficulty:** Intermediate → Advanced
**Interview Relevance:** Extremely High

> ⚠️ **Important:** JavaScript is single-threaded, but asynchronous operations allow it to handle long-running tasks without blocking the main JavaScript execution thread.

---

## 📚 Contents

1. [Synchronous vs Asynchronous JavaScript](#1-synchronous-vs-asynchronous-javascript)
2. [JavaScript Runtime Model](#2-javascript-runtime-model)
3. [Call Stack](#3-call-stack)
4. [Web APIs](#4-web-apis)
5. [Task Queue and Microtask Queue](#5-task-queue-and-microtask-queue)
6. [Event Loop](#6-event-loop)
7. [Complete Async Execution Flow](#7-complete-async-execution-flow)
8. [Callbacks](#8-callbacks)
9. [Callback Hell](#9-callback-hell)
10. [Promises](#10-promises)
11. [Promise States](#11-promise-states)
12. [Creating a Promise](#12-creating-a-promise)
13. [Consuming a Promise](#13-consuming-a-promise)
14. [Promise Chaining](#14-promise-chaining)
15. [Error Handling with Promises](#15-error-handling-with-promises)
16. [Promise Methods](#16-promise-methods)
17. [Promise.all](#17-promiseall)
18. [Promise.allSettled](#18-promiseallsettled)
19. [Promise.race](#19-promiserace)
20. [Promise.any](#20-promiseany)
21. [Async Functions](#21-async-functions)
22. [Await](#22-await)
23. [Why await Does Not Block JavaScript](#23-why-await-does-not-block-javascript)
24. [Error Handling with async/await](#24-error-handling-with-asyncawait)
25. [Sequential vs Parallel Execution](#25-sequential-vs-parallel-execution)
26. [Microtask vs Macrotask Execution Order](#26-microtask-vs-macrotask-execution-order)
27. [Common Interview Traps](#27-common-interview-traps)
28. [Quick Revision](#28-quick-revision)

---

# 1. Synchronous vs Asynchronous JavaScript

## Definition

### Synchronous JavaScript

Code executes **one statement at a time**, in order.

```js
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

The next operation waits for the current operation to finish.

### Asynchronous JavaScript

An asynchronous operation can be started without making JavaScript wait for it to finish.

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 1000);

console.log("C");
```

Output:

```text
A
C
B
```

## Why?

Asynchronous behavior is necessary because operations such as:

* Network requests
* Timers
* File operations
* User interactions
* Database requests

may take an unpredictable amount of time.

If JavaScript waited synchronously for every slow operation, the application could become unresponsive.

---

# 2. JavaScript Runtime Model

JavaScript itself provides the language, but asynchronous behavior in browsers is enabled by the **JavaScript runtime environment**.

A simplified browser runtime looks like:

```text
                JavaScript Runtime

             ┌──────────────────┐
             │    Call Stack     │
             └────────┬─────────┘
                      │
              JavaScript Engine
                      │
       ┌──────────────┴──────────────┐
       │                             │
       ▼                             ▼
   Web APIs                  Callback Queues
       │                             │
       └──────────────┬──────────────┘
                      ▼
                  Event Loop
```

### Important distinction

**JavaScript engine**

Responsible for executing JavaScript.

Examples:

* V8
* SpiderMonkey
* JavaScriptCore

**Browser runtime**

Provides additional capabilities such as:

* `setTimeout`
* `fetch`
* DOM APIs
* Event listeners
* Web APIs

> ⚠️ JavaScript itself does not contain the browser's Web APIs. The host environment provides them.

---

# 3. Call Stack

## Definition

The **Call Stack** keeps track of currently executing JavaScript functions.

It follows:

**LIFO — Last In, First Out**

```js
function one() {
  two();
}

function two() {
  three();
}

function three() {
  console.log("Hello");
}

one();
```

Execution:

```text
one()
  ↓
two()
  ↓
three()
  ↓
console.log()
```

Conceptually:

```text
┌──────────────┐
│    three()   │
├──────────────┤
│     two()    │
├──────────────┤
│     one()    │
└──────────────┘
```

When a function finishes, it is removed from the stack.

## Why?

The Call Stack determines **what JavaScript is executing right now**.

> ⚠️ JavaScript executes synchronous JavaScript on the Call Stack. The Event Loop does not execute JavaScript itself; it coordinates when queued callbacks can be placed onto the Call Stack.

---

# 4. Web APIs

## Definition

Web APIs are browser-provided capabilities that allow JavaScript to interact with the environment.

Examples:

```js
setTimeout()
fetch()
addEventListener()
DOM APIs
localStorage
```

Example:

```js
setTimeout(() => {
  console.log("Hello");
}, 1000);
```

Conceptually:

```text
JavaScript
    │
    ▼
setTimeout()
    │
    ▼
Browser Web API
    │
    ▼
Wait 1000ms
    │
    ▼
Queue callback
```

The timer does **not** sit on the Call Stack for one second.

> ⚠️ `setTimeout()` does not mean "execute this function exactly after N milliseconds." It means "make this callback eligible to run after at least N milliseconds, once the Call Stack is available."

---

# 5. Task Queue and Microtask Queue

Asynchronous callbacks eventually need to wait somewhere before they can execute.

JavaScript runtimes have different queues, with two important categories:

### Task Queue

Also commonly called the **Macrotask Queue**.

Examples include callbacks from:

```js
setTimeout()
setInterval()
```

### Microtask Queue

Examples:

```js
Promise.then()
Promise.catch()
Promise.finally()
queueMicrotask()
```

Simplified model:

```text
             Event Loop
                 │
       ┌─────────┴─────────┐
       │                   │
       ▼                   ▼
 Microtask Queue       Task Queue
       │                   │
 Promise callbacks     Timer callbacks
```

> ⚠️ Microtasks generally receive priority over tasks. After the current synchronous code finishes, the runtime drains the microtask queue before taking the next task.

---

# 6. Event Loop

## Definition

The **Event Loop** coordinates asynchronous callbacks and determines when queued work can execute on the Call Stack.

Simplified model:

```text
        ┌──────────────┐
        │  Call Stack  │
        └──────┬───────┘
               │
               ▼
       Is Stack Empty?
               │
               ▼
      Process Microtasks
               │
               ▼
       Take Next Task
               │
               ▼
        Call Stack
```

## Why?

JavaScript is single-threaded, so only one piece of JavaScript executes at a time.

The Event Loop allows JavaScript to coordinate asynchronous work without requiring JavaScript to remain blocked while external operations complete.

---

# 7. Complete Async Execution Flow

Consider:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

Output:

```text
Start
End
Promise
Timer
```

### Why?

Execution:

```text
1. console.log("Start")
       ↓
2. setTimeout() → Web API
       ↓
3. Promise.then() → Microtask Queue
       ↓
4. console.log("End")
       ↓
5. Synchronous code finishes
       ↓
6. Microtask Queue runs
       ↓
7. "Promise"
       ↓
8. Task Queue runs
       ↓
9. "Timer"
```

Complete chain:

```text
Synchronous JS
      ↓
Call Stack
      ↓
Web APIs
      ↓
Task / Microtask Queues
      ↓
Event Loop
      ↓
Call Stack
```

### Core rule

```text
Synchronous code
      ↓
Microtasks
      ↓
Next task
```

---

# 8. Callbacks

## Definition

A **callback** is a function passed to another function to be executed later.

```js
function greet(name, callback) {
  console.log(`Hello ${name}`);
  callback();
}

greet("Aniket", () => {
  console.log("Done");
});
```

## Asynchronous Callback

```js
setTimeout(() => {
  console.log("Executed later");
}, 1000);
```

The callback is not executed immediately.

It becomes eligible for execution after the timer completes and the runtime schedules the callback.

## Why?

Callbacks provide a way to say:

> "When this operation finishes, execute this function."

---

# 9. Callback Hell

## Problem

When asynchronous operations depend on one another, callbacks can become deeply nested.

```js
getUser(userId, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      saveComments(comments, () => {
        console.log("Done");
      });
    });
  });
});
```

This creates:

* Deep nesting
* Difficult error handling
* Poor readability
* Difficult maintenance

This pattern is commonly called **Callback Hell** or the **Pyramid of Doom**.

## Solution

Promises provide a cleaner abstraction:

```js
getUser(userId)
  .then(user => getPosts(user.id))
  .then(posts => getComments(posts[0].id))
  .then(comments => saveComments(comments))
  .then(() => console.log("Done"))
  .catch(error => console.error(error));
```

And `async/await` can make the flow easier to read:

```js
async function execute() {
  try {
    const user = await getUser(userId);
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);

    await saveComments(comments);
  } catch (error) {
    console.error(error);
  }
}
```

---

# 10. Promises

## Definition

A **Promise** is an object representing the eventual result of an asynchronous operation.

It represents a value that may be:

* available now
* available later
* unavailable because the operation failed

```js
const promise = fetch("/api/users");
```

A Promise provides a standardized way to handle asynchronous results.

## Why?

Promises solve major problems associated with callback-based asynchronous code:

* Better composition
* Cleaner chaining
* Centralized error handling
* Easier asynchronous control flow

---

# 11. Promise States

A Promise has three states:

```text
             Pending
             /     \
            /       \
           ▼         ▼
      Fulfilled   Rejected
```

### Pending

Operation is still in progress.

### Fulfilled

Operation completed successfully.

### Rejected

Operation failed.

Example:

```js
const promise = new Promise((resolve, reject) => {
  // asynchronous operation
});
```

Once settled:

```text
Pending → Fulfilled
```

or:

```text
Pending → Rejected
```

> ⚠️ A Promise can settle only once. After fulfillment or rejection, its state cannot change again.

---

# 12. Creating a Promise

## Syntax

```js
const promise = new Promise((resolve, reject) => {
  // operation

  if (success) {
    resolve(value);
  } else {
    reject(error);
  }
});
```

Example:

```js
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Operation successful");
  } else {
    reject("Operation failed");
  }
});
```

### `resolve()`

Marks the Promise as fulfilled.

### `reject()`

Marks the Promise as rejected.

> ⚠️ The Promise executor function runs immediately when `new Promise()` is created. The Promise itself represents the eventual result.

---

# 13. Consuming a Promise

Use:

```js
then()
catch()
finally()
```

Example:

```js
promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error);
  })
  .finally(() => {
    console.log("Finished");
  });
```

### `then()`

Runs when the Promise fulfills.

### `catch()`

Handles rejection.

### `finally()`

Runs regardless of whether the Promise fulfills or rejects.

---

# 14. Promise Chaining

Promises can be chained.

```js
getUser()
  .then(user => {
    return getPosts(user.id);
  })
  .then(posts => {
    return getComments(posts[0].id);
  })
  .then(comments => {
    console.log(comments);
  })
  .catch(error => {
    console.error(error);
  });
```

The important rule:

> If a `.then()` callback returns a value, the next `.then()` receives that value.

```js
Promise.resolve(10)
  .then(value => value * 2)
  .then(value => value + 5)
  .then(console.log);
```

Output:

```text
25
```

### Returning a Promise

```js
Promise.resolve()
  .then(() => {
    return fetch("/api/users");
  })
  .then(response => {
    return response.json();
  })
  .then(data => {
    console.log(data);
  });
```

The chain waits for the returned Promise to settle.

---

# 15. Error Handling with Promises

Use:

```js
.catch()
```

Example:

```js
fetch("/api/users")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error(error);
  });
```

Errors can propagate through the Promise chain until they are handled.

```js
Promise.resolve()
  .then(() => {
    throw new Error("Something went wrong");
  })
  .then(() => {
    console.log("This will not run");
  })
  .catch(error => {
    console.log(error.message);
  });
```

Output:

```text
Something went wrong
```

---

# 16. Promise Methods

The most important Promise methods for revision are:

### Instance methods

```js
then()
catch()
finally()
```

### Static methods

```js
Promise.all()
Promise.allSettled()
Promise.race()
Promise.any()
```

The static methods are especially important when handling **multiple asynchronous operations**.

---

# 17. Promise.all

## Definition

`Promise.all()` waits for **all Promises to fulfill**.

```js
const result = await Promise.all([
  fetchUsers(),
  fetchPosts(),
  fetchComments()
]);
```

If all succeed:

```text
Promise.all()
      ↓
Wait for all
      ↓
Return array of results
```

If one rejects:

```text
Promise.all()
      ↓
One Promise rejects
      ↓
Returned Promise rejects
```

Example:

```js
const results = await Promise.all([
  Promise.resolve("A"),
  Promise.resolve("B"),
  Promise.resolve("C")
]);

console.log(results);
```

Output:

```js
["A", "B", "C"]
```

### Important

`Promise.all()` is useful when:

* All operations are required
* Operations can run concurrently
* Failure of one means the overall operation should fail

> ⚠️ `Promise.all()` does not cancel the other underlying operations when one Promise rejects. The aggregate Promise rejects, but already-started operations may continue.

---

# 18. Promise.allSettled

## Definition

`Promise.allSettled()` waits for **every Promise to settle**, regardless of whether it fulfills or rejects.

```js
const results = await Promise.allSettled([
  Promise.resolve("Success"),
  Promise.reject("Failed")
]);
```

Result conceptually:

```js
[
  {
    status: "fulfilled",
    value: "Success"
  },
  {
    status: "rejected",
    reason: "Failed"
  }
]
```

## Why?

Use it when you want the result of **every operation**, even if some fail.

Example use cases:

* Multiple independent API requests
* Batch processing
* Dashboard widgets
* Uploading multiple files

---

# 19. Promise.race

## Definition

`Promise.race()` settles as soon as the **first input Promise settles**.

```js
const result = await Promise.race([
  request1(),
  request2()
]);
```

The first Promise to:

```text
Fulfill OR Reject
```

determines the result.

Example:

```js
const result = await Promise.race([
  new Promise(resolve => {
    setTimeout(() => resolve("Slow"), 2000);
  }),

  new Promise(resolve => {
    setTimeout(() => resolve("Fast"), 500);
  })
]);

console.log(result);
```

Output:

```text
Fast
```

> ⚠️ `race()` reacts to the first settled Promise, not necessarily the first fulfilled Promise.

---

# 20. Promise.any

## Definition

`Promise.any()` fulfills as soon as the **first Promise fulfills**.

```js
const result = await Promise.any([
  request1(),
  request2(),
  request3()
]);
```

If one succeeds:

```text
First fulfillment
      ↓
Promise.any()
      ↓
Success
```

Rejected Promises are ignored until all inputs reject.

If all reject, `Promise.any()` rejects with an:

```js
AggregateError
```

Example:

```js
const result = await Promise.any([
  Promise.reject("A"),
  Promise.resolve("B"),
  Promise.resolve("C")
]);

console.log(result);
```

Output:

```text
B
```

---

# 21. Async Functions

## Definition

An `async` function always returns a Promise.

```js
async function getData() {
  return "Hello";
}
```

Conceptually:

```js
getData()
```

returns:

```js
Promise<string>
```

Therefore:

```js
getData().then(data => {
  console.log(data);
});
```

works.

## Example

```js
async function getUser() {
  return {
    id: 1,
    name: "Aniket"
  };
}
```

The returned object is automatically wrapped in a fulfilled Promise.

---

# 22. Await

## Definition

`await` pauses the execution of the **current async function** until the awaited Promise settles.

```js
async function getData() {
  const response = await fetch("/api/users");

  const data = await response.json();

  return data;
}
```

`await` makes asynchronous code look synchronous while preserving non-blocking behavior.

> ⚠️ `await` does not block the entire JavaScript thread.

---

# 23. Why await Does Not Block JavaScript

Consider:

```js
async function test() {
  console.log("A");

  await new Promise(resolve => {
    setTimeout(resolve, 2000);
  });

  console.log("B");
}

test();

console.log("C");
```

Output:

```text
A
C
B
```

### What happens?

When execution reaches:

```js
await promise;
```

the current `async` function pauses.

The JavaScript engine can continue executing other available work.

Conceptually:

```text
test()
 ↓
console.log("A")
 ↓
await Promise
 ↓
pause test()
 ↓
Call Stack becomes available
 ↓
console.log("C")
 ↓
Promise settles
 ↓
Continuation of test() becomes a microtask
 ↓
console.log("B")
```

Therefore:

```text
await ≠ block entire JavaScript thread
```

Instead:

```text
await
 ↓
pause current async function
 ↓
continue other work
 ↓
resume function later
```

This is one of the most important concepts in asynchronous JavaScript.

---

# 24. Error Handling with async/await

Use `try...catch`.

```js
async function getData() {
  try {
    const response = await fetch("/api/users");

    const data = await response.json();

    return data;
  } catch (error) {
    console.error(error);
  }
}
```

The `catch` block handles rejected Promises and exceptions thrown inside the `try` block.

### Alternative

You can handle the returned Promise:

```js
getData()
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error(error);
  });
```

---

# 25. Sequential vs Parallel Execution

This distinction is extremely important in real applications.

## Sequential

```js
const user = await getUser();
const posts = await getPosts();
const comments = await getComments();
```

Execution:

```text
getUser
  ↓
getPosts
  ↓
getComments
```

Each operation waits for the previous one.

Use this when operations depend on each other.

---

## Parallel / Concurrent Start

If operations are independent:

```js
const [users, posts, comments] = await Promise.all([
  getUsers(),
  getPosts(),
  getComments()
]);
```

Conceptually:

```text
        ┌── getUsers()
        │
Start ──┼── getPosts()
        │
        └── getComments()
                 ↓
            Wait for all
                 ↓
              Results
```

This can significantly reduce total waiting time.

### Important optimization

Avoid:

```js
const users = await getUsers();
const posts = await getPosts();
```

when `getUsers()` and `getPosts()` are independent.

Prefer:

```js
const [users, posts] = await Promise.all([
  getUsers(),
  getPosts()
]);
```

---

# 26. Microtask vs Macrotask Execution Order

Consider:

```js
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
});

console.log("4");
```

Output:

```text
1
4
3
2
```

### Why?

Synchronous code runs first:

```text
1
4
```

Then the microtask:

```js
Promise.then()
```

runs:

```text
3
```

Then the timer task:

```js
setTimeout()
```

runs:

```text
2
```

Therefore:

```text
Synchronous code
      ↓
Microtask Queue
      ↓
Task Queue
```

### Another example

```js
setTimeout(() => {
  console.log("timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("promise");
});

queueMicrotask(() => {
  console.log("microtask");
});
```

Output:

```text
promise
microtask
timeout
```

Both Promise callbacks and `queueMicrotask()` use the microtask queue.

> ⚠️ Never assume `setTimeout(..., 0)` executes immediately. `0` means the timer has no intentional delay requirement beyond the minimum scheduling rules; it still waits for the current execution and higher-priority queued work.

---

# 27. Common Interview Traps

## 27.1 JavaScript is Single-Threaded

Correct:

> JavaScript execution is single-threaded in the traditional browser execution model.

But asynchronous behavior is enabled by the runtime and its APIs.

```text
JavaScript
    ↓
Call Stack
    ↓
Runtime APIs
    ↓
Queues
    ↓
Event Loop
```

---

## 27.2 `setTimeout(fn, 0)` Is Not Immediate

```js
setTimeout(() => {
  console.log("Hello");
}, 0);

console.log("World");
```

Output:

```text
World
Hello
```

The callback cannot execute until the current synchronous work completes.

---

## 27.3 `await` Does Not Block Everything

Incorrect:

> `await` blocks JavaScript.

Correct:

> `await` pauses the current async function's continuation while allowing the runtime to continue processing other work.

---

## 27.4 `async` Always Returns a Promise

```js
async function test() {
  return 10;
}
```

Equivalent conceptually to:

```js
function test() {
  return Promise.resolve(10);
}
```

Therefore:

```js
test().then(console.log);
```

prints:

```text
10
```

---

## 27.5 `Promise.all()` Fails Fast

```js
Promise.all([
  promise1,
  promise2,
  promise3
]);
```

If one rejects, the aggregate Promise rejects.

But:

> ⚠️ "Fails fast" does not mean the other underlying operations are automatically cancelled.

---

## 27.6 `Promise.race()` vs `Promise.any()`

### `Promise.race()`

First Promise to **settle** wins.

```text
Fulfilled OR Rejected
```

### `Promise.any()`

First Promise to **fulfill** wins.

```text
Fulfilled only
```

---

## 27.7 Promise Executor Runs Immediately

```js
console.log("A");

new Promise(() => {
  console.log("B");
});

console.log("C");
```

Output:

```text
A
B
C
```

The executor passed to `new Promise()` runs synchronously.

---

# 28. Quick Revision

## Complete Async JavaScript Model

```text
                JavaScript
                    │
                    ▼
               Call Stack
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
      Synchronous         Web APIs
        execution             │
                              ▼
                         Async work
                              │
                 ┌────────────┴────────────┐
                 │                         │
                 ▼                         ▼
          Microtask Queue             Task Queue
                 │                         │
                 └────────────┬────────────┘
                              ▼
                          Event Loop
                              │
                              ▼
                         Call Stack
```

## Core Concepts

| Concept                | Remember                                              |
| ---------------------- | ----------------------------------------------------- |
| Call Stack             | Executes JavaScript functions                         |
| Web APIs               | Runtime-provided asynchronous capabilities            |
| Callback               | Function executed later                               |
| Promise                | Represents eventual async result                      |
| Microtask              | Higher-priority queued work such as Promise reactions |
| Task                   | Queued work such as timer callbacks                   |
| Event Loop             | Coordinates queued work with the Call Stack           |
| `async`                | Makes a function return a Promise                     |
| `await`                | Pauses the current async function until settlement    |
| `Promise.all()`        | All must fulfill                                      |
| `Promise.allSettled()` | Wait for everything to settle                         |
| `Promise.race()`       | First settled Promise wins                            |
| `Promise.any()`        | First fulfilled Promise wins                          |

## Must-Know Execution Order

```text
1. Synchronous JavaScript
2. Current execution completes
3. Microtasks are processed
4. Next task is selected
5. Repeat
```

## Must-Know Promise Methods

```js
promise.then()
promise.catch()
promise.finally()

Promise.all()
Promise.allSettled()
Promise.race()
Promise.any()
```

## Interview-Level Questions

Before considering this phase complete, you should be able to explain:

1. Why is JavaScript called single-threaded?
2. How does JavaScript perform asynchronous operations?
3. What is the Call Stack?
4. What are Web APIs?
5. What is the Event Loop?
6. What is the difference between a task and a microtask?
7. Why does a Promise callback execute before a `setTimeout(..., 0)` callback?
8. What problem do Promises solve?
9. What are the three states of a Promise?
10. What is Promise chaining?
11. What happens when a `.then()` callback returns another Promise?
12. Difference between `Promise.all()` and `Promise.allSettled()`?
13. Difference between `Promise.race()` and `Promise.any()`?
14. Does `async` return a Promise?
15. Why doesn't `await` block the entire JavaScript thread?
16. When should you use sequential `await`?
17. When should you use `Promise.all()`?
18. What happens when an async function throws an error?
19. What is Callback Hell?
20. Predict the output of code involving `console.log`, `setTimeout`, `Promise.then`, and `queueMicrotask`.

---

## 🧠 Final Mental Model

When you see asynchronous JavaScript, think:

```text
"Who executes it?"
        ↓
Call Stack

"Who handles the external operation?"
        ↓
Runtime / Web APIs

"Where does the callback wait?"
        ↓
Microtask / Task Queue

"Who decides when it can execute?"
        ↓
Event Loop

"Which runs first?"
        ↓
Synchronous code
        ↓
Microtasks
        ↓
Tasks
```

The most important chain to remember:

```text
Synchronous JS
      ↓
Call Stack
      ↓
Web APIs
      ↓
Task / Microtask Queues
      ↓
Event Loop
      ↓
Call Stack
```

And for modern asynchronous code:

```text
Callbacks
    ↓
Promises
    ↓
Promise chaining
    ↓
async / await
    ↓
Concurrent async operations
    ↓
Promise.all / allSettled / race / any
```

> ⭐ **If you understand the Call Stack + Web APIs + Queues + Event Loop + Promises + `async/await`, you understand the core execution model behind asynchronous JavaScript.**
