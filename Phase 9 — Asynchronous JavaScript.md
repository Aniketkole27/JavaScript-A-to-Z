<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 9 — Asynchronous JavaScript ⭐⭐⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-9%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Deserves%20Serious%20Revision-critical" />
</p>

> JavaScript is **single-threaded** — only one thing runs at a time. Async doesn't mean multi-threaded, it means the environment *around* JS hands work back at the right time.

## 📑 Contents
- [The Complete Chain ⭐](#the-complete-chain-)
- [1. Callbacks](#1-callbacks)
- [2. Promises](#2-promises)
- [3. Promise Methods — `then` / `catch` / `finally`](#3-promise-methods--then--catch--finally)
- [4. Static Methods](#4-static-methods)
- [5. Async / Await](#5-async--await)
- [6. Why `await` doesn't block the thread ⭐](#6-why-await-doesnt-block-the-thread-)
- [🧪 Self-Test](#-self-test)

---

## The Complete Chain ⭐

```
Synchronous JS
      ↓
Call Stack
      ↓
Web APIs           (setTimeout, fetch, DOM events — NOT part of JS engine, provided by browser/Node)
      ↓
Task/Microtask queues
      ↓
Event Loop
      ↓
Call Stack
```

**Understand, step by step**
1. Regular synchronous code runs on the **call stack** immediately, top to bottom.
2. When it hits something async (`setTimeout`, `fetch`, event listener), that work is **handed off to a Web API** (browser-provided, outside the JS engine) — the call stack does NOT wait for it.
3. Once the Web API finishes, its callback is placed into a **queue** — either the **macrotask queue** (`setTimeout`, `setInterval`, DOM events) or the **microtask queue** (Promise `.then`/`.catch`/`.finally`, `queueMicrotask`, `async/await` continuations).
4. The **Event Loop** constantly checks: *"is the call stack empty?"* If yes, it pulls the next task from the queue and pushes it onto the call stack to run.
5. **Microtasks always run before the next macrotask** — and the microtask queue is fully drained (including any new microtasks it schedules) before the event loop touches the macrotask queue again.

**Code — proves the order**
```js
console.log("1 — sync");

setTimeout(() => console.log("2 — macrotask (setTimeout)"), 0);

Promise.resolve().then(() => console.log("3 — microtask (promise)"));

console.log("4 — sync");

// Output:
// 1 — sync
// 4 — sync
// 3 — microtask (promise)
// 2 — macrotask (setTimeout)
```

**Explain**
Even with `setTimeout(fn, 0)`, the callback **never** runs before the current synchronous code finishes — it waits in the macrotask queue. Promise callbacks (microtasks) always cut in line **ahead of** any pending macrotask, no matter which was scheduled first.

---

## 1. Callbacks

**Code**
```js
function fetchUser(id, callback) {
  setTimeout(() => {
    callback({ id, name: "Aniket" }); // simulates a network delay
  }, 1000);
}

fetchUser(1, (user) => {
  console.log(user); // runs ~1s later, after the sync code below has already finished
});
console.log("Fetching...");
```

**Explain**
Callbacks were the **original** async pattern. Problem: nesting multiple dependent async calls leads to **callback hell** (deeply indented pyramid), and error handling has to be repeated manually at every level. Promises were built to solve exactly this.

```js
// Callback hell — the problem Promises solve
getUser(1, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      console.log(comments); // 3 levels deep, errors need checking at each level
    });
  });
});
```

---

## 2. Promises

**Understand**
A `Promise` is an object representing a value that will exist **eventually** — it has 3 states: `pending` → `fulfilled` or `rejected` (once settled, it never changes again).

**Code**
```js
const promise = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve("Data loaded"); // moves to "fulfilled"
    } else {
      reject("Failed to load"); // moves to "rejected"
    }
  }, 1000);
});

console.log(promise); // Promise { <pending> } -> immediately, before it settles
```

**Explain**
`resolve`/`reject` are functions **you** call inside the executor to settle the promise. Once settled (fulfilled or rejected), a promise is **immutable** — calling `resolve`/`reject` again does nothing.

---

## 3. Promise Methods — `then` / `catch` / `finally`

**Code**
```js
promise
  .then((data) => {
    console.log("Success:", data);
    return data.toUpperCase();       // value passed to the NEXT .then
  })
  .then((upper) => console.log(upper))
  .catch((err) => console.log("Error:", err))  // catches rejection from ANY earlier step
  .finally(() => console.log("Done — runs regardless of success/failure"));
```

**Explain**
- `.then(onFulfilled, onRejected)` — each `.then` returns a **new Promise**, enabling chaining.
- `.catch(fn)` is shorthand for `.then(undefined, fn)` — it catches a rejection from **any** point earlier in the chain, not just the immediately preceding step.
- `.finally(fn)` runs no matter what — no arguments passed in, used for cleanup (hiding a spinner, closing a connection).
- A `return` inside `.then` passes that value to the **next** `.then`; returning another Promise makes the chain **wait** for it too (auto-flattened, no nested `.then` needed).

---

## 4. Static Methods

**Recall**

| Method | Resolves when | Rejects when | Result |
|---|---|---|---|
| `Promise.all` | ALL succeed | ANY one rejects (fails fast) | array of values, same order |
| `Promise.allSettled` | ALWAYS resolves | never rejects | array of `{status, value/reason}` |
| `Promise.race` | first one settles (success OR fail) | — | that single result |
| `Promise.any` | first one **succeeds** | ALL reject | that success, or `AggregateError` |

**Code**
```js
const p1 = new Promise(res => setTimeout(() => res("P1"), 1000));
const p2 = new Promise(res => setTimeout(() => res("P2"), 500));
const p3 = new Promise((_, rej) => setTimeout(() => rej("P3 failed"), 300));

Promise.all([p1, p2]).then(console.log);           // ["P1","P2"] after 1000ms — waits for slowest
Promise.all([p1, p3]).catch(console.log);            // "P3 failed" after 300ms — fails fast on first rejection

Promise.allSettled([p1, p3]).then(console.log);
// after 1000ms: [{status:"fulfilled", value:"P1"}, {status:"rejected", reason:"P3 failed"}]

Promise.race([p1, p2, p3]).then(console.log).catch(console.log);
// "P3 failed" after 300ms -> first to SETTLE, win or lose

Promise.any([p1, p3]).then(console.log);
// "P1" after 1000ms -> first to SUCCEED, ignores p3's earlier rejection
```

**Explain**
- Use `Promise.all` when you need **everything** to succeed (e.g. loading multiple required resources) — one failure kills the whole batch.
- Use `Promise.allSettled` when you want results **regardless of individual failures** (e.g. sending analytics to 5 endpoints, some may fail, don't care).
- `race` vs `any`: `race` cares about **speed** (first to finish, win or lose), `any` cares about **success** (first winner, ignores losers unless everyone loses).

---

## 5. Async / Await

**Understand**
Syntactic sugar over Promises — makes async code **read like synchronous code**, while still being non-blocking underneath.

**Code**
```js
async function getData() {
    try {
        const response = await fetch("https://api.example.com/data");
        const data = await response.json();
        console.log(data);
        return data;
    } catch (error) {
        console.log("Error:", error);
    } finally {
        console.log("Request finished");
    }
}

getData();

// Sequential (each waits for the previous — slower if independent)
async function sequential() {
  const a = await fetchA(); // waits ~1s
  const b = await fetchB(); // then waits ~1s more -> total ~2s
  return [a, b];
}

// Parallel (fire both at once, then wait — faster if independent)
async function parallel() {
  const [a, b] = await Promise.all([fetchA(), fetchB()]); // both run together -> total ~1s
  return [a, b];
}
```

**Explain**
- An `async function` **always returns a Promise** — even a plain `return "hi"` becomes `Promise.resolve("hi")` automatically.
- `await` pauses execution **of that function only**, waiting for the Promise to settle, then unwraps its value (or throws, if rejected — catchable with `try/catch`).
- If your `await` calls **don't depend on each other**, start them together with `Promise.all` instead of `await`-ing one at a time — awaiting sequentially when unnecessary is a common performance bug.

---

## 6. Why `await` doesn't block the thread ⭐

**Understand**
`await` only pauses the **local execution of the `async function`** — it does **not** freeze the JS engine, the call stack, or the rest of the program.

**Code — proof**
```js
async function slowTask() {
  console.log("A - start");
  await new Promise(res => setTimeout(res, 1000)); // pauses HERE, but only within slowTask
  console.log("C - resumed after 1s");
}

slowTask();
console.log("B - runs immediately, does NOT wait for slowTask");

// Output:
// A - start
// B - runs immediately, does NOT wait for slowTask
// C - resumed after 1s
```

**Explain**
- When execution hits `await`, the `async function` is essentially **suspended and popped off the call stack**, exactly like reaching a `.then()` — control returns immediately to whoever called it.
- The rest of the **synchronous code outside** the function (`console.log("B...")`) keeps running without waiting.
- Once the awaited Promise settles, the rest of the `async function` is scheduled as a **microtask** — it resumes only when the call stack is empty again, via the event loop, just like a `.then()` callback would.
- In short: `async/await` is just `.then()` chains **rewritten to look synchronous** — same non-blocking mechanics under the hood, better readability on top.

<details>
<summary>❓ Revisit</summary>

```js
console.log("1");

async function foo() {
  console.log("2");
  await null;             // even awaiting a non-promise value still yields to microtask queue
  console.log("3");
}
foo();

console.log("4");
```
```
1
2
4
3   // code AFTER await always runs as a microtask, even for instantly-resolved values
```
</details>

---

## 🧪 Self-Test

```js
console.log("start");

setTimeout(() => console.log("timeout"), 0);

Promise.resolve()
  .then(() => console.log("promise 1"))
  .then(() => console.log("promise 2"));

async function test() {
  console.log("async start");
  await null;
  console.log("async end");
}
test();

console.log("end");
```

<details>
<summary>Show answers</summary>

```
start
async start
end
promise 1
async end
promise 2
timeout
```

Reasoning: all synchronous code (`start`, `async start`, `end`) runs first.
Then the microtask queue drains in order: `promise 1` was queued before `test()`'s
post-`await` continuation, so it runs first — then `async end` — then `promise 2`
(queued only after `promise 1`'s `.then` ran). The macrotask (`timeout`) runs last, always.
</details>

---

<p align="center"><i>Next → <code>10-error-handling.md</code></i></p>
