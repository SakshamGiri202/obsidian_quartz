# Asynchronous JavaScript

## Overview
JavaScript is single-threaded and synchronous by default — code executes line by line. Asynchronous JavaScript allows non-blocking execution, enabling tasks like API calls, timers, and file I/O to run without freezing the main thread. This is achieved via the **event loop**, which delegates blocking operations to the browser/Node.js and processes callbacks when the call stack is empty.

## Key Concepts

### Callbacks
A function passed as an argument to another function, executed after an asynchronous operation completes.

```javascript
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: "Alice" };
        callback(data);
    }, 2000);
}

fetchData((data) => {
    console.log("Data received:", data);
});

// Callbacks in higher-order functions
function calculate(a, b, operation) {
    return operation(a, b);
}
console.log(calculate(5, 3, (x, y) => x + y));  // 8
```

**Problem — Callback Hell:** Deeply nested callbacks become unreadable.
```javascript
step1(() => {
    step2(() => {
        step3(() => {
            step4(() => {
                console.log("Done");
            });
        });
    });
});
```

**Problem — Error handling:** Must check errors manually in each callback.

### Promises (ES6)
A Promise represents the eventual completion (or failure) of an async operation. It has three states: **Pending**, **Fulfilled**, **Rejected**.

```javascript
// Creating a Promise
const fetchUser = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true;
        if (success) resolve({ id: 1, name: "Bob" });
        else reject("Failed to fetch user");
    }, 1000);
});

// Consuming a Promise
fetchUser
    .then(user => console.log(user))
    .catch(err => console.error(err))
    .finally(() => console.log("Done"));  // always runs
```

**Promise Chaining** — sequential async operations
```javascript
fetchUser
    .then(user => fetchPosts(user.id))        // returns a promise
    .then(posts => fetchComments(posts[0].id))
    .then(comments => console.log(comments))
    .catch(err => console.error("Any error in chain:", err));
```

**Promise Static Methods:**

| Method                      | Behavior                                                         |
| --------------------------- | ---------------------------------------------------------------- |
| `Promise.all([...])`        | Waits for ALL to resolve; rejects immediately if ANY rejects     |
| `Promise.allSettled([...])` | Waits for ALL to settle (resolve or reject); never rejects       |
| `Promise.race([...])`       | Settles as soon as the FIRST promise settles (resolve or reject) |
| `Promise.any([...])`        | Resolves with the FIRST fulfilled; rejects only if ALL reject    |

```javascript
const p1 = Promise.resolve("A");
const p2 = new Promise(resolve => setTimeout(() => resolve("B"), 100));
const p3 = Promise.reject("C failed");

Promise.all([p1, p2])
    .then(results => console.log(results));        // ["A", "B"]

Promise.allSettled([p1, p2, p3])
    .then(results => console.log(results));
// [{status:"fulfilled", value:"A"}, {status:"fulfilled", value:"B"}, {status:"rejected", reason:"C failed"}]

Promise.race([p2, p3])
    .then(result => console.log(result))            // "B" (resolves first)
    .catch(err => console.error(err));

// Timeout pattern with race
const data = fetch("/api/data");                    // takes 3s
const timeout = new Promise((_, reject) =>
    setTimeout(() => reject("Timeout!"), 2000)
);
Promise.race([data, timeout])
    .then(res => console.log(res))
    .catch(err => console.error(err));              // "Timeout!" (2s < 3s)
```

**Wrapping callbacks into promises (promisification):**
```javascript
function readFilePromise(path) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, "utf8", (err, data) => {
            if (err) reject(err);
            else resolve(data);
        });
    });
}
```

### Async/Await (ES2017)
Syntactic sugar over Promises — makes async code read like synchronous code.

```javascript
async function getUserData(userId) {
    try {
        const user = await fetchUser(userId);
        const posts = await fetchPosts(user.id);
        const comments = await fetchComments(posts[0].id);
        return comments;
    } catch (error) {
        console.error("Error:", error);
        throw error;  // re-throw if caller needs to handle
    }
}

// async functions always return a Promise
getUserData(1).then(comments => console.log(comments));
```

**Key rules:**
- `async` makes a function return a Promise (non-promise values are auto-wrapped)
- `await` can only be used inside `async` functions
- `await` pauses execution until the Promise settles
- Error handling uses `try...catch` instead of `.catch()`

**Parallel execution with async/await:**
```javascript
async function loadDashboard() {
    try {
        // Run in parallel (don't await individually)
        const [user, posts, notifications] = await Promise.all([
            fetchUser(1),
            fetchPosts(1),
            fetchNotifications(1)
        ]);
        return { user, posts, notifications };
    } catch (err) {
        console.error("Dashboard load failed:", err);
    }
}
```

**Sequential vs Parallel:**
```javascript
// Sequential (one by one) — slower
async function sequential() {
    const a = await taskA();   // 2s
    const b = await taskB();   // 2s  → total ~4s
}

// Parallel — faster
async function parallel() {
    const [a, b] = await Promise.all([taskA(), taskB()]);  // total ~2s
}
```

### Fetch API
Modern browser API for making HTTP requests — returns a Promise.

```javascript
// GET request
async function getUsers() {
    try {
        const response = await fetch("https://api.example.com/users");
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const data = await response.json();  // parse JSON body
        return data;
    } catch (err) {
        console.error("Fetch failed:", err);
    }
}

// POST request
async function createUser(user) {
    const response = await fetch("https://api.example.com/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(user)
    });
    return response.json();
}
```

### Event Loop (Conceptual)
```
 ┌─────────────────────────┐
 │        Call Stack        │  ← JS executes here (one thing at a time)
 ├─────────────────────────┤
 │     Web APIs / Node.js   │  ← setTimeout, fetch, DOM events
 ├─────────────────────────┤
 │     Callback Queue       │  ← callbacks wait here
 ├─────────────────────────┤
 │    Microtask Queue       │  ← Promise.then/catch, queueMicrotask
 └─────────────────────────┘
```

**Execution order:**
1. All synchronous code in Call Stack
2. Microtask queue (Promise callbacks) — drained completely
3. One task from Callback Queue (macrotask queue)
4. Repeat (step 2 → 3 → 2 → 3...)

```javascript
console.log("1");                         // sync

setTimeout(() => console.log("2"), 0);    // macrotask → callback queue

Promise.resolve().then(() => console.log("3"));  // microtask

console.log("4");                         // sync

// Output: 1, 4, 3, 2
```

## Summary
Asynchronous JavaScript evolved from **callbacks** (prone to callback hell), to **Promises** (cleaner chaining with `.then()`/`.catch()`), to **async/await** (synchronous-looking code with `try...catch`). The **event loop** enables non-blocking concurrency on a single thread by managing the call stack, microtask queue (Promise callbacks), and macrotask queue (timers, I/O). Use `Promise.all()` for parallel tasks, `Promise.race()` for timeouts, and always handle errors with `try...catch` or `.catch()`.

## Resources
- [Asynchronous JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/asynchronous-javascript/)
- [JavaScript Callbacks - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-callbacks/)
- [JavaScript Promise - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-promise/)
- [Async/Await in JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/async-await-function-in-javascript/)
- [Event Loop - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/what-is-an-event-loop-in-javascript/)
