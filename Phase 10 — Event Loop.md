<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 10 — Event Loop ⭐⭐⭐⭐</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-10%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Priority-Output%20Prediction%20Mastery-critical" />
</p>

> This phase is 80% practice. Read the rules once, then grind the output-prediction questions until you stop getting them wrong.

## 📑 Contents
- [The Core Rule](#the-core-rule)
- [Worked Example](#worked-example)
- [1. Microtasks vs Macrotasks](#1-microtasks-vs-macrotasks)
- [2. `setTimeout`](#2-settimeout)
- [3. `setInterval`](#3-setinterval)
- [4. `queueMicrotask`](#4-queuemicrotask)
- [5. Rendering — where it fits in](#5-rendering--where-it-fits-in)
- [6. Full Browser Event Loop Diagram](#6-full-browser-event-loop-diagram)
- [🧪 Output Prediction Practice](#-output-prediction-practice)

---

## The Core Rule

1. Run **all** synchronous code first (call stack empties).
2. Drain the **entire microtask queue** — including any new microtasks added while draining. Microtask queue must be **fully empty** before moving on.
3. Run **one** macrotask from the macrotask queue.
4. Go back to step 2. Repeat forever.

**Priority order:** `sync code` → `all microtasks (fully drained, even newly added ones)` → `one macrotask` → `all microtasks again` → `one macrotask` → ...

---

## Worked Example

```js
console.log("A");

setTimeout(() => {
    console.log("B");
}, 0);

Promise.resolve().then(() => {
    console.log("C");
});

console.log("D");
```

**Trace**
| Step | Action | Output so far |
|---|---|---|
| 1 | `console.log("A")` runs sync | `A` |
| 2 | `setTimeout` callback handed to Web API, timer starts, **macrotask queue** gets it once timer fires | — |
| 3 | `Promise.resolve().then(...)` — callback added to **microtask queue** | — |
| 4 | `console.log("D")` runs sync | `A D` |
| 5 | Call stack empty → drain microtask queue → runs `console.log("C")` | `A D C` |
| 6 | Microtask queue empty → event loop takes next macrotask → runs `console.log("B")` | `A D C B` |

**Result**
```
A
D
C
B
```

---

## 1. Microtasks vs Macrotasks

**Recall**

| | Microtasks | Macrotasks (a.k.a. "tasks") |
|---|---|---|
| Sources | `Promise.then/catch/finally`, `async/await` continuations, `queueMicrotask` | `setTimeout`, `setInterval`, `setImmediate` (Node), DOM events, `fetch` callback dispatch |
| When they run | Immediately after current sync code, **before** any macrotask | One at a time, only after the microtask queue is fully empty |
| Queue drained fully each cycle? | ✅ yes, entirely, before moving on | ❌ only ONE macrotask runs per event loop cycle |

**Code**
```js
console.log("1");
setTimeout(() => console.log("2 - macro"), 0);
Promise.resolve().then(() => console.log("3 - micro"));
Promise.resolve().then(() => console.log("4 - micro"));
console.log("5");
// 1, 5, 3 - micro, 4 - micro, 2 - macro
// BOTH microtasks run before the ONE macrotask, regardless of order registered
```

**Explain**
Microtasks can **starve** macrotasks if they keep scheduling more microtasks — the event loop won't touch the macrotask queue until the microtask queue is completely empty, even if that takes a while.

---

## 2. `setTimeout`

**Code**
```js
setTimeout(() => console.log("fires after ~0ms, but AFTER all sync + microtasks"), 0);
console.log("sync");
Promise.resolve().then(() => console.log("micro"));
// sync, micro, fires after ~0ms...
```

**Explain**
`setTimeout(fn, 0)` does **not** mean "run immediately" — it means "run as soon as possible **after** the minimum delay, once the call stack is clear and all microtasks are drained." The delay is a **minimum**, not a guarantee — if the stack is busy, the callback waits longer. Browsers also enforce a **minimum ~4ms** clamp for nested timeouts.

---

## 3. `setInterval`

**Code**
```js
let count = 0;
const id = setInterval(() => {
  console.log("tick", ++count);
  if (count === 3) clearInterval(id); // always clear intervals, or they run forever
}, 1000);
```

**Explain**
- Each tick is a **separate macrotask**, queued the same way as `setTimeout`.
- If the main thread is busy (a long synchronous task) when a tick is due, that tick is **delayed**, not queued twice — `setInterval` does not guarantee perfectly even spacing, only "at least this much time has passed."
- Forgetting `clearInterval` is a classic **memory leak** source, especially in React `useEffect` without cleanup.

---

## 4. `queueMicrotask`

**Code**
```js
console.log("1");
setTimeout(() => console.log("2 - macro"), 0);
queueMicrotask(() => console.log("3 - explicit micro"));
Promise.resolve().then(() => console.log("4 - promise micro"));
console.log("5");
// 1, 5, 3 - explicit micro, 4 - promise micro, 2 - macro
```

**Explain**
`queueMicrotask(fn)` schedules a microtask directly, without needing a Promise wrapper — useful when you want microtask-priority timing (run right after current code, before rendering/macrotasks) without the overhead of creating a Promise.

---

## 5. Rendering — where it fits in

**Understand**
The browser tries to **paint** the updated UI at specific points — never in the middle of running JS (JS is single-threaded, so paint has to wait its turn).

**Recall — simplified per-cycle order**
```
1. Run one macrotask (or handle one event)
2. Drain ALL microtasks completely
3. Browser MAY render a frame here (roughly every ~16.6ms for 60fps, if a repaint is due)
4. Repeat
```

**Code**
```js
button.addEventListener("click", () => {
  element.style.color = "red";     // change 1
  // browser does NOT repaint yet — still inside this synchronous handler
  element.style.color = "blue";     // change 2 — overwrites change 1 before any paint happened
});
// Only "blue" is ever visibly painted — the browser batches this into ONE repaint after the handler finishes
```

**Explain**
Rendering happens **between** macrotasks — never mid-script. This is why rapid synchronous DOM changes in a single event handler don't cause visible flickering: the browser only paints the **final** state once the handler (and its microtasks) finish. It's also why a long synchronous loop **freezes the UI** — no repaint can happen until the call stack clears.

---

## 6. Full Browser Event Loop Diagram

```
        ┌─────────────────────────────────────────────┐
        │                 Call Stack                   │
        │   (sync code runs here, one frame at a time)  │
        └───────────────────┬───────────────────────────┘
                             │  stack empties
                             ▼
        ┌─────────────────────────────────────────────┐
        │             Microtask Queue                   │
        │  Promise .then/.catch/.finally, async/await,  │
        │  queueMicrotask                                │
        │  → drained COMPLETELY, incl. newly added ones   │
        └───────────────────┬───────────────────────────┘
                             │  queue fully empty
                             ▼
        ┌─────────────────────────────────────────────┐
        │        Browser MAY render a frame here         │
        └───────────────────┬───────────────────────────┘
                             ▼
        ┌─────────────────────────────────────────────┐
        │             Macrotask Queue                    │
        │  setTimeout, setInterval, DOM events, fetch     │
        │  dispatch                                        │
        │  → ONE task picked, pushed to call stack          │
        └───────────────────┬───────────────────────────┘
                             │
                             └──────────────► back to Call Stack, repeat forever
```

**Explain**
Web APIs (`setTimeout`, `fetch`, DOM event listeners) live **outside** the JS engine entirely — provided by the browser (or Node's libuv). They do the actual waiting; once done, they simply drop a callback into the correct queue. The Event Loop's only job is: *"is the stack empty? drain microtasks, maybe render, then grab one macrotask."*

---

## 🧪 Output Prediction Practice

Predict each before checking the answer.

**Q1 — Basics**
```js
console.log(1);
setTimeout(() => console.log(2));
Promise.resolve().then(() => console.log(3));
console.log(4);
```
<details><summary>Answer</summary>

```
1
4
3
2
```
</details>

---

**Q2 — Chained microtasks**
```js
console.log("start");
Promise.resolve()
  .then(() => console.log("A"))
  .then(() => console.log("B"));
Promise.resolve().then(() => console.log("C"));
console.log("end");
```
<details><summary>Answer</summary>

```
start
end
A
C
B
```
Reasoning: two separate `.then` chains — first links of both chains (`A`, `C`) queue before either chain's second link (`B`). `A` was registered first, so it runs first; its `.then(B)` only gets queued AFTER `A` runs, landing behind `C` which was already waiting.
</details>

---

**Q3 — Nested setTimeout vs Promise**
```js
setTimeout(() => console.log("timeout 1"), 0);
setTimeout(() => {
  console.log("timeout 2");
  Promise.resolve().then(() => console.log("micro inside timeout"));
}, 0);
Promise.resolve().then(() => console.log("micro 1"));
```
<details><summary>Answer</summary>

```
micro 1
timeout 1
timeout 2
micro inside timeout
```
Reasoning: all microtasks queued during sync execution run first. Then macrotasks run ONE AT A TIME, and each macrotask's own microtasks are drained before the NEXT macrotask starts.
</details>

---

**Q4 — async/await mixed in**
```js
console.log("1");

async function foo() {
  console.log("2");
  await Promise.resolve();
  console.log("3");
}
foo();

setTimeout(() => console.log("4"), 0);

console.log("5");
```
<details><summary>Answer</summary>

```
1
2
5
3
4
```
Reasoning: `foo()` runs synchronously up to `await`. Code after `await` becomes a microtask, so it waits behind nothing else queued yet, but AFTER the remaining synchronous code (`5`) finishes. `4` is a macrotask — runs last.
</details>

---

**Q5 — setInterval + microtask starvation risk**
```js
let i = 0;
function loop() {
  i++;
  if (i < 3) Promise.resolve().then(loop); // recursively schedules microtasks
  console.log("i =", i);
}
loop();
console.log("after loop()");
```
<details><summary>Answer</summary>

```
i = 1
after loop()
i = 2
i = 3
```
Reasoning: first call is synchronous. Each subsequent call is scheduled as a microtask,
which runs before ANY macrotask, but still after the initially running sync code (`after loop()`).
This is exactly how microtask recursion can "starve" macrotasks like setTimeout if it never stops.
</details>

---

**Q6 — Mixed everything (hardest)**
```js
console.log("A");

setTimeout(() => console.log("B"), 0);

new Promise((resolve) => {
  console.log("C");     // executor runs SYNCHRONOUSLY, immediately
  resolve();
}).then(() => console.log("D"));

queueMicrotask(() => console.log("E"));

async function f() {
  console.log("F");
  await null;
  console.log("G");
}
f();

console.log("H");
```
<details><summary>Answer</summary>

```
A
C
F
H
D
E
G
B
```
Reasoning:
- Sync pass: A → C (Promise executor runs immediately, not deferred) → F (async fn body before await) → H
- Microtask queue filled in this order during sync pass: [D, E, G]
- Macrotask (B) runs only after ALL of those drain.
</details>

---

<p align="center"><i>Next → <code>11-error-handling.md</code></i></p>
