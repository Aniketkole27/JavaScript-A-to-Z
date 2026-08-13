# JavaScript A→Z Revision

## Phase 10 — Event Loop ⭐⭐⭐⭐

> **Goal:** Understand how the JavaScript Event Loop schedules synchronous code, microtasks, tasks, Promise callbacks, timers, and browser rendering—and confidently predict asynchronous output.

**Importance:** ⭐⭐⭐⭐⭐  
**Difficulty:** Intermediate → Advanced  
**Interview Relevance:** Extremely High

> ⚠️ **Important:** The Event Loop is not what makes JavaScript itself multi-threaded. JavaScript execution is single-threaded, while the browser runtime provides APIs, queues, and scheduling mechanisms that allow asynchronous work to be coordinated.

---

## 📚 Contents

1. [Why the Event Loop Exists](#1-why-the-event-loop-exists)
2. [JavaScript Execution Model](#2-javascript-execution-model)
3. [Call Stack](#3-call-stack)
4. [Browser Runtime and Web APIs](#4-browser-runtime-and-web-apis)
5. [Task Queue](#5-task-queue)
6. [Microtask Queue](#6-microtask-queue)
7. [Macrotasks / Tasks](#7-macrotasks--tasks)
8. [Event Loop](#8-event-loop)
9. [Basic Execution Order](#9-basic-execution-order)
10. [Promise Callbacks](#10-promise-callbacks)
11. [setTimeout](#11-settimeout)
12. [setInterval](#12-setinterval)
13. [queueMicrotask](#13-queuemicrotask)
14. [Microtasks vs Macrotasks](#14-microtasks-vs-macrotasks)
15. [Browser Rendering](#15-browser-rendering)
16. [Event Loop and Rendering](#16-event-loop-and-rendering)
17. [Complete Browser Event Loop Model](#17-complete-browser-event-loop-model)
18. [Output-Based Questions](#18-output-based-questions)
19. [Output Question 1](#19-output-question-1)
20. [Output Question 2](#20-output-question-2)
21. [Output Question 3](#21-output-question-3)
22. [Output Question 4](#22-output-question-4)
23. [Output Question 5](#23-output-question-5)
24. [Output Question 6](#24-output-question-6)
25. [Output Question 7](#25-output-question-7)
26. [Output Question 8](#26-output-question-8)
27. [Output Question 9](#27-output-question-9)
28. [Output Question 10](#28-output-question-10)
29. [Nested Microtasks](#29-nested-microtasks)
30. [Nested setTimeout](#30-nested-settimeout)
31. [Common Event Loop Mistakes](#31-common-event-loop-mistakes)
32. [Interview-Level Mental Model](#32-interview-level-mental-model)
33. [Quick Revision](#33-quick-revision)
34. [Interview Questions](#34-interview-questions)

---

# 1. Why the Event Loop Exists

JavaScript executes code using a **single Call Stack**.

If JavaScript waited synchronously for every slow operation:

```text
Network request
Database request
Timer
File operation
User interaction
```

the application could become unresponsive.

Instead, the runtime allows asynchronous operations to be handled outside the JavaScript execution stack and schedules their callbacks for later execution.

Simplified:

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
    ↓
Call Stack
```

The Event Loop coordinates when queued work can execute.

---

# 2. JavaScript Execution Model

A useful browser model is:

```text
                 JavaScript Engine
                       │
                       ▼
                 ┌───────────┐
                 │Call Stack │
                 └─────┬─────┘
                       │
                       │
              Browser Runtime
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
    Timers           DOM            Network
       │               │               │
       └───────────────┼───────────────┘
                       ▼
                    Queues
                       │
                       ▼
                  Event Loop
                       │
                       ▼
                 Call Stack
```

The important components are:

- Call Stack
- Browser APIs
- Microtask Queue
- Task Queue
- Event Loop
- Rendering pipeline

---

# 3. Call Stack

## Definition

The Call Stack keeps track of currently executing JavaScript functions.

It follows:

```text
LIFO
Last In → First Out
```

Example:

```js
function first() {
  second();
}

function second() {
  console.log("Hello");
}

first();
```

Execution:

```text
first()
  ↓
second()
  ↓
console.log()
```

Conceptually:

```text
┌───────────────┐
│ console.log() │
├───────────────┤
│ second()      │
├───────────────┤
│ first()       │
└───────────────┘
```

When a function finishes, it is removed from the stack.

---

# 4. Browser Runtime and Web APIs

The JavaScript engine does not directly implement every browser capability.

The browser provides APIs such as:

```js
setTimeout()

setInterval()

fetch()

addEventListener()

document.querySelector()

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
setTimeout()
     ↓
Browser Timer API
     ↓
Wait until timer expires
     ↓
Callback becomes eligible
     ↓
Task Queue
     ↓
Event Loop
     ↓
Call Stack
```

> ⚠️ `setTimeout()` does not put the callback directly onto the Call Stack.

---

# 5. Task Queue

The **Task Queue** contains callbacks that are ready to execute as tasks.

It is also commonly called the:

```text
Macrotask Queue
```

Examples can include callbacks associated with:

```js
setTimeout()

setInterval()

DOM events

Message events
```

Simplified:

```text
Browser API
    ↓
Callback ready
    ↓
Task Queue
```

The Event Loop eventually moves an eligible task onto the Call Stack when appropriate.

---

# 6. Microtask Queue

The **Microtask Queue** contains microtasks that need to run after the current synchronous execution completes.

Common sources include:

```js
Promise.then()

Promise.catch()

Promise.finally()

queueMicrotask()
```

Example:

```js
Promise.resolve().then(() => {
  console.log("Promise");
});
```

The callback goes to the Microtask Queue.

```text
Promise resolves
      ↓
.then() callback
      ↓
Microtask Queue
```

---

# 7. Macrotasks / Tasks

The term **macrotask** is commonly used in interview discussions, while the HTML specification generally uses the term **task**.

For practical interview purposes:

```text
Macrotask ≈ Task
```

Examples:

```js
setTimeout()

setInterval()

DOM events
```

The important distinction is:

```text
Microtasks
    ↓
Higher scheduling priority after current task

Tasks
    ↓
Next unit of event-loop work
```

> ⚠️ Do not memorize "microtasks always execute before everything." The precise model is that after a task's JavaScript execution completes, the microtask queue is processed before the browser proceeds to the next task, with rendering occurring according to the browser's scheduling rules.

---

# 8. Event Loop

## Definition

The Event Loop coordinates the execution of queued asynchronous work with the Call Stack.

A simplified model:

```text
           Call Stack
               │
               ▼
       Current task finishes
               │
               ▼
       Drain Microtasks
               │
               ▼
      Browser may render
               │
               ▼
        Next eligible task
               │
               ▼
           Call Stack
```

The Event Loop repeatedly performs this type of coordination.

---

# 9. Basic Execution Order

The most important interview rule:

```text
1. Execute synchronous JavaScript
2. Finish the current task
3. Drain the Microtask Queue
4. Browser may perform rendering/update steps
5. Execute the next eligible task
6. Repeat
```

For basic interview questions, remember:

```text
Synchronous code
      ↓
Microtasks
      ↓
Tasks / Macrotasks
```

---

# 10. Promise Callbacks

Promise reactions such as:

```js
then()

catch()

finally()
```

run as **microtasks** when the corresponding Promise settles.

Example:

```js
console.log("A");

Promise.resolve().then(() => {
  console.log("B");
});

console.log("C");
```

Output:

```text
A
C
B
```

Why?

```text
console.log("A")
      ↓
Promise callback queued
      ↓
console.log("C")
      ↓
Synchronous code finishes
      ↓
Microtask runs
      ↓
console.log("B")
```

---

# 11. setTimeout

`setTimeout()` schedules a callback as a task after the specified delay has elapsed.

Example:

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

Output:

```text
A
C
B
```

Even with:

```js
setTimeout(fn, 0);
```

the callback does not execute immediately.

It must wait for:

```text
Current synchronous execution
        ↓
Microtasks
        ↓
Eligible task
```

> ⚠️ **Important:** `0ms` means no intentional timer delay, not "execute immediately."

---

# 12. setInterval

`setInterval()` schedules a callback repeatedly.

Example:

```js
setInterval(() => {
  console.log("Hello");
}, 1000);
```

Conceptually:

```text
Timer expires
    ↓
Callback becomes eligible
    ↓
Task
    ↓
Execute callback
    ↓
Wait for next interval
    ↓
Repeat
```

Clear it using:

```js
const id = setInterval(() => {
  console.log("Running");
}, 1000);

clearInterval(id);
```

> ⚠️ `setInterval()` does not guarantee that callbacks execute exactly every N milliseconds. The callback must still wait for the event loop and available execution time.

---

# 13. queueMicrotask

`queueMicrotask()` explicitly schedules a microtask.

```js
console.log("A");

queueMicrotask(() => {
  console.log("B");
});

console.log("C");
```

Output:

```text
A
C
B
```

Compare:

```js
queueMicrotask(() => {
  console.log("Microtask");
});

setTimeout(() => {
  console.log("Task");
}, 0);
```

Output:

```text
Microtask
Task
```

---

# 14. Microtasks vs Macrotasks

| Microtask | Task / Macrotask |
|---|---|
| Promise callbacks | `setTimeout()` |
| `catch()` | `setInterval()` |
| `finally()` | Many DOM events |
| `queueMicrotask()` | Other task sources |
| Runs after current task | Runs as a later task |
| Queue is drained before next task | Next task waits for microtasks |

Example:

```js
setTimeout(() => {
  console.log("Timer");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

queueMicrotask(() => {
  console.log("Microtask");
});
```

Output:

```text
Promise
Microtask
Timer
```

---

# 15. Browser Rendering

The browser does more than execute JavaScript.

It also needs to update what the user sees.

A simplified rendering pipeline:

```text
JavaScript
    ↓
Style calculation
    ↓
Layout
    ↓
Paint
    ↓
Composite
```

The exact browser pipeline is more complex and implementation-dependent.

The important interview concept is:

> The browser must get opportunities to perform rendering between pieces of work.

---

## Rendering Is Not a JavaScript Queue

Do not think:

```text
Microtask Queue
Task Queue
Render Queue
```

as three equivalent JavaScript queues.

Rendering is part of the browser's rendering/update process and is scheduled by the browser around event-loop work.

---

# 16. Event Loop and Rendering

Consider:

```js
button.addEventListener("click", () => {
  element.textContent = "Loading...";

  heavyCalculation();

  element.textContent = "Done";
});
```

The browser may not visually show `"Loading..."` before `heavyCalculation()` finishes because the JavaScript task is still running.

Conceptually:

```text
Click task starts
      ↓
DOM changed to "Loading..."
      ↓
Heavy JavaScript
      ↓
DOM changed to "Done"
      ↓
Task finishes
      ↓
Browser gets opportunity to render
```

The user may therefore see only:

```text
Done
```

rather than visually seeing:

```text
Loading...
```

Long-running JavaScript can cause:

- UI freezes
- Delayed animations
- Delayed input response
- Poor responsiveness

---

# 17. Complete Browser Event Loop Model

A useful mental model:

```text
                         BROWSER
                            │
                            ▼
                     JavaScript Task
                            │
                            ▼
                      Call Stack
                            │
                            ▼
                   Synchronous Code
                            │
                            ▼
                    Task completes
                            │
                            ▼
                  Drain Microtasks
                            │
                            ▼
                  Browser Update /
                     Rendering
                            │
                            ▼
                  Next Eligible Task
                            │
                            ▼
                      Call Stack
```

The simplified cycle is:

```text
Task
 ↓
Synchronous JavaScript
 ↓
Microtasks
 ↓
Rendering opportunity
 ↓
Next Task
 ↓
Repeat
```

> ⚠️ Rendering is browser-scheduled. Do not assume the browser renders after every single JavaScript statement or after every microtask.

---

# 18. Output-Based Questions

For interviews, the most important skill in this phase is **output prediction**.

When you see:

```js
console.log()

Promise.resolve().then()

queueMicrotask()

setTimeout()

setInterval()
```

don't guess.

Trace the execution.

## Output Question Strategy

### Step 1 — Mark synchronous code

```text
SYNC
```

### Step 2 — Identify microtasks

Look for:

```js
Promise.then()
Promise.catch()
Promise.finally()
queueMicrotask()
```

### Step 3 — Identify tasks

Look for:

```js
setTimeout()
setInterval()
DOM events
```

### Step 4 — Execute synchronous code first

### Step 5 — Drain microtasks

### Step 6 — Process the next task

---

# 19. Output Question 1

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

### Answer

```text
A
D
C
B
```

### Explanation

Synchronous:

```text
A
D
```

Promise callback:

```text
C
```

Timer task:

```text
B
```

Therefore:

```text
A
D
C
B
```

---

# 20. Output Question 2

```js
console.log("1");

Promise.resolve().then(() => {
  console.log("2");
});

console.log("3");

setTimeout(() => {
  console.log("4");
}, 0);

console.log("5");
```

### Answer

```text
1
3
5
2
4
```

Synchronous:

```text
1
3
5
```

Microtask:

```text
2
```

Task:

```text
4
```

---

# 21. Output Question 3

```js
setTimeout(() => {
  console.log("A");
}, 0);

Promise.resolve().then(() => {
  console.log("B");
});

queueMicrotask(() => {
  console.log("C");
});

console.log("D");
```

### Answer

```text
D
B
C
A
```

Execution:

```text
Synchronous
    ↓
D

Microtasks
    ↓
B
C

Task
    ↓
A
```

---

# 22. Output Question 4

```js
console.log("A");

setTimeout(() => {
  console.log("B");

  Promise.resolve().then(() => {
    console.log("C");
  });
}, 0);

Promise.resolve().then(() => {
  console.log("D");
});

console.log("E");
```

### Answer

```text
A
E
D
B
C
```

When the timer task runs:

```text
B
```

is synchronous inside that task.

Then the Promise callback becomes a microtask:

```text
C
```

So:

```text
A
E
D
B
C
```

---

# 23. Output Question 5

```js
console.log("A");

Promise.resolve().then(() => {
  console.log("B");

  Promise.resolve().then(() => {
    console.log("C");
  });
});

console.log("D");
```

### Answer

```text
A
D
B
C
```

The second Promise callback is queued while the first microtask is running. It runs when the microtask queue continues draining.

---

# 24. Output Question 6

```js
console.log("1");

setTimeout(() => {
  console.log("2");

  queueMicrotask(() => {
    console.log("3");
  });

  console.log("4");
}, 0);

Promise.resolve().then(() => {
  console.log("5");
});

console.log("6");
```

### Answer

```text
1
6
5
2
4
3
```

Execution:

```text
First task:
1
6

Microtask:
5

Timer task:
2
4

Microtask created inside timer:
3
```

---

# 25. Output Question 7

```js
setTimeout(() => {
  console.log("A");
}, 0);

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

Promise.resolve().then(() => {
  console.log("D");
});

console.log("E");
```

### Answer

```text
E
C
D
A
B
```

---

# 26. Output Question 8

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

queueMicrotask(() => {
  console.log("C");

  queueMicrotask(() => {
    console.log("D");
  });
});

console.log("E");
```

### Answer

```text
A
E
C
D
B
```

A microtask can create another microtask. The newly created microtask is processed during the same microtask-draining phase.

---

# 27. Output Question 9

```js
setTimeout(() => {
  console.log("A");
}, 0);

Promise.resolve().then(() => {
  console.log("B");

  setTimeout(() => {
    console.log("C");
  }, 0);
});

console.log("D");
```

### Answer

```text
D
B
A
C
```

The first timer was scheduled before the second timer was created.

---

# 28. Output Question 10

```js
console.log("1");

setTimeout(() => {
  console.log("2");

  Promise.resolve().then(() => {
    console.log("3");
  });

  setTimeout(() => {
    console.log("4");
  }, 0);
}, 0);

Promise.resolve().then(() => {
  console.log("5");

  queueMicrotask(() => {
    console.log("6");
  });
});

console.log("7");
```

### Answer

```text
1
7
5
6
2
3
4
```

### Trace

Initial task:

```text
1
7
```

Microtask:

```text
5
```

Microtask created by that microtask:

```text
6
```

First timer task:

```text
2
```

Microtask created inside timer:

```text
3
```

Second timer task:

```text
4
```

Final:

```text
1
7
5
6
2
3
4
```

---

# 29. Nested Microtasks

Consider:

```js
Promise.resolve().then(() => {
  console.log("A");

  Promise.resolve().then(() => {
    console.log("B");
  });

  console.log("C");
});
```

Output:

```text
A
C
B
```

Why?

```text
Microtask 1
   ↓
A
   ↓
queue Microtask 2
   ↓
C
   ↓
Microtask 1 finishes
   ↓
Microtask 2
   ↓
B
```

---

## Microtask Starvation

Because microtasks can schedule more microtasks:

```js
function loop() {
  queueMicrotask(loop);
}

loop();
```

the microtask queue can continuously generate more work.

```text
Microtask
   ↓
Microtask
   ↓
Microtask
   ↓
Microtask
   ↓
...
```

This can cause **microtask starvation**.

> ⚠️ Do not create unbounded recursive microtasks in real applications.

---

# 30. Nested setTimeout

Consider:

```js
setTimeout(() => {
  console.log("A");

  setTimeout(() => {
    console.log("B");
  }, 0);
}, 0);
```

Conceptually:

```text
Initial task
    ↓
Timer A becomes eligible
    ↓
Task A executes
    ↓
Timer B is scheduled
    ↓
Task A finishes
    ↓
Later task
    ↓
B
```

Output:

```text
A
B
```

The second timer does not execute during the first timer's callback.

---

# 31. Common Event Loop Mistakes

## Mistake 1 — Thinking setTimeout(..., 0) Runs Immediately

Incorrect:

```text
setTimeout(..., 0)
↓
Immediately execute
```

Correct:

```text
setTimeout(..., 0)
↓
Timer becomes eligible
↓
Task Queue
↓
Event Loop
↓
Call Stack
```

## Mistake 2 — Thinking Promises Run Immediately

Incorrect:

```text
Promise.resolve().then(callback)
↓
Run callback now
```

Correct:

```text
Promise settles
     ↓
Callback scheduled as microtask
     ↓
Current synchronous execution finishes
     ↓
Microtask executes
```

## Mistake 3 — Thinking `await` Creates a Normal Synchronous Pause

```js
async function test() {
  console.log("A");

  await Promise.resolve();

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

The continuation after `await` runs asynchronously as a Promise reaction/microtask.

## Mistake 4 — Thinking All Async Operations Are Equal

For interview questions, distinguish:

```text
Promise callback → Microtask
queueMicrotask() → Microtask
setTimeout() → Task
setInterval() → Task
```

## Mistake 5 — Thinking Rendering Happens After Every Statement

The browser does not render after every JavaScript statement.

A long-running JavaScript task can prevent rendering from happening during that task.

## Mistake 6 — Thinking setInterval() Is Perfectly Precise

```js
setInterval(callback, 1000);
```

does not guarantee:

```text
Exactly every 1000ms
```

The callback still depends on:

- Event loop availability
- Other JavaScript work
- Browser scheduling
- Runtime constraints

## Mistake 7 — Ignoring Microtasks Created Inside Tasks

```js
setTimeout(() => {
  console.log("A");

  Promise.resolve().then(() => {
    console.log("B");
  });

  console.log("C");
}, 0);
```

Output:

```text
A
C
B
```

---

# 32. Interview-Level Mental Model

When an interviewer gives you:

```js
console.log()

Promise.resolve().then()

queueMicrotask()

setTimeout()

setInterval()
```

build this table mentally:

| Code | Category | When does it run? |
|---|---|---|
| Normal `console.log()` | Synchronous | Immediately on Call Stack |
| `Promise.then()` | Microtask | After current task |
| `Promise.catch()` | Microtask | After current task |
| `Promise.finally()` | Microtask | After current task |
| `queueMicrotask()` | Microtask | After current task |
| `setTimeout()` | Task | Later task |
| `setInterval()` | Task | Repeated eligible tasks |

Then follow:

```text
1. Synchronous
2. Microtasks
3. Rendering opportunity
4. Next task
```

---

# 33. Quick Revision

## The Core Chain

```text
Synchronous JavaScript
        ↓
Call Stack
        ↓
Browser APIs
        ↓
Queues
        ↓
Event Loop
        ↓
Call Stack
```

## Microtasks

```js
Promise.then()

Promise.catch()

Promise.finally()

queueMicrotask()
```

Mental model:

```text
Current task finishes
        ↓
Drain microtasks
```

## Tasks / Macrotasks

Common examples:

```js
setTimeout()

setInterval()

DOM event callbacks
```

Mental model:

```text
Task
 ↓
Synchronous code
 ↓
Microtasks
 ↓
Next task
```

## Output Prediction Formula

When you see:

```js
console.log();

Promise.resolve().then();

queueMicrotask();

setTimeout();
```

think:

```text
SYNC
 ↓
MICROTASKS
 ↓
TASKS
```

Example:

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

Answer:

```text
A
D
C
B
```

## Event Loop Mental Model

```text
                 ┌─────────────────┐
                 │   Call Stack    │
                 └────────┬────────┘
                          │
                          ▼
                 Current Task Runs
                          │
                          ▼
                   Task Finishes
                          │
                          ▼
                ┌──────────────────┐
                │ Microtask Queue  │
                └────────┬─────────┘
                         │
                         ▼
                  Drain All Ready
                     Microtasks
                         │
                         ▼
                 Browser May Render
                         │
                         ▼
                Next Eligible Task
                         │
                         ▼
                    Call Stack
```

---

# 34. Interview Questions

Before considering Phase 10 complete, you should be able to answer these without looking at the notes.

## Event Loop Fundamentals

1. What is the Event Loop?
2. Why does JavaScript need an Event Loop?
3. Is JavaScript single-threaded?
4. What is the Call Stack?
5. What are Web APIs?
6. What is the Task Queue?
7. What is the Microtask Queue?
8. What is a macrotask?
9. What is the difference between a task and a microtask?
10. What role does the Event Loop play?

## Promises and Microtasks

11. Why do Promise callbacks run asynchronously?
12. Which queue handles Promise callbacks?
13. Is `Promise.then()` a microtask or a macrotask?
14. What is `queueMicrotask()`?
15. What happens if a microtask creates another microtask?
16. What is microtask starvation?
17. Why do Promise callbacks generally execute before timer callbacks?

## Timers

18. What does `setTimeout(fn, 0)` actually mean?
19. Does `setTimeout(fn, 0)` execute immediately?
20. Is `setTimeout()` a microtask or a task?
21. Is `setInterval()` guaranteed to execute exactly at its specified interval?
22. What happens when a timer callback schedules a Promise callback?
23. What happens when a timer callback schedules another timer?

## Rendering

24. How does JavaScript execution affect browser rendering?
25. Can a long-running JavaScript task block rendering?
26. Does the browser render after every JavaScript statement?
27. What is the relationship between the Event Loop and rendering?
28. Why can heavy synchronous JavaScript make a UI appear frozen?

## Output Questions

29. Predict the output:

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

30. Predict the output:

```js
console.log("1");

Promise.resolve().then(() => {
  console.log("2");
});

console.log("3");

setTimeout(() => {
  console.log("4");
}, 0);

console.log("5");
```

31. Predict the output:

```js
setTimeout(() => {
  console.log("A");
}, 0);

queueMicrotask(() => {
  console.log("B");
});

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

32. Predict the output:

```js
setTimeout(() => {
  console.log("A");

  Promise.resolve().then(() => {
    console.log("B");
  });
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

33. Predict the output:

```js
console.log("A");

queueMicrotask(() => {
  console.log("B");

  queueMicrotask(() => {
    console.log("C");
  });
});

setTimeout(() => {
  console.log("D");
}, 0);

console.log("E");
```

34. Predict the output:

```js
setTimeout(() => {
  console.log("A");
}, 0);

Promise.resolve().then(() => {
  console.log("B");

  setTimeout(() => {
    console.log("C");
  }, 0);
});

console.log("D");
```

---

# 🧠 Final Mental Model

The most important thing to remember from Phase 10 is:

```text
                JAVASCRIPT
                    │
                    ▼
               Call Stack
                    │
                    ▼
            Execute Current Task
                    │
                    ▼
             Task Finishes
                    │
                    ▼
          Drain Microtask Queue
                    │
                    ▼
          Browser May Render
                    │
                    ▼
          Next Eligible Task
                    │
                    ▼
               Call Stack
```

And the most important priority rule:

```text
┌─────────────────────────┐
│ 1. Synchronous JavaScript│
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ 2. Microtasks           │
│    Promise callbacks    │
│    queueMicrotask()     │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ 3. Rendering opportunity│
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ 4. Next Task            │
│    setTimeout()         │
│    setInterval()        │
│    DOM events           │
└─────────────────────────┘
```

> ⭐ **For interviews, don't memorize output answers. Learn to classify every operation as synchronous, microtask, or task, then trace the queues step by step.**

> 🔥 **The key interview skill is being able to look at 10–20 lines of asynchronous JavaScript and explain exactly why every line executes in its particular order.**
