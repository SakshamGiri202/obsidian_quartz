# Advanced JavaScript Concepts

> Deep dive into prototypes, `this` binding, event loop, and memory management

---

## 1️⃣ Prototypes & Prototypal Inheritance

### What is a Prototype?

Every JavaScript object has an internal `[[Prototype]]` property (accessible via `__proto__` or `Object.getPrototypeOf()`) that references another object. When you access a property on an object, JS first looks on the object itself, then walks up the **prototype chain** until it finds the property or reaches `null`.

### Why Prototypes?

- **Memory efficiency** — methods shared across instances (not copied)
- **Dynamic inheritance** — change prototype at runtime, all instances reflect it
- **Foundation of JS** — no classes in ES5; everything built on prototypes

---

### The Prototype Chain

```javascript
// Object literal → Object.prototype → null
const obj = { a: 1 };
obj.__proto__ === Object.prototype; // true
Object.prototype.__proto__;         // null

// Array → Array.prototype → Object.prototype → null
const arr = [1, 2, 3];
arr.__proto__ === Array.prototype;  // true
Array.prototype.__proto__ === Object.prototype; // true

// Function → Function.prototype → Object.prototype → null
function fn() {}
fn.__proto__ === Function.prototype; // true
```

---

### Constructor Functions & `prototype` Property

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Methods on prototype (shared)
Person.prototype.greet = function() {
  return `Hi, I'm ${this.name}`;
};

Person.prototype.species = "Human";

const alice = new Person("Alice", 30);
const bob = new Person("Bob", 25);

alice.greet();        // "Hi, I'm Alice"
bob.greet();          // "Hi, I'm Bob"
alice.greet === bob.greet; // true — SAME function reference

// Instance has own properties, prototype has methods
alice.hasOwnProperty("name");     // true
alice.hasOwnProperty("greet");    // false
"greet" in alice;                 // true (found in chain)
```

### Prototype Diagram

```
alice (instance)
  │
  ├── name: "Alice" (own)
  ├── age: 30 (own)
  └── __proto__ ──────► Person.prototype
                        │
                        ├── greet() (shared)
                        ├── species: "Human" (shared)
                        └── __proto__ ──────► Object.prototype
                                              │
                                              ├── toString()
                                              ├── hasOwnProperty()
                                              └── __proto__ ──────► null
```

---

### `Object.create()` — Direct Prototype Linking

```javascript
const animal = {
  breathe() { console.log("Breathing..."); }
};

const dog = Object.create(animal);
dog.bark = function() { console.log("Woof!"); };

dog.breathe(); // "Breathing..." (inherited)
dog.bark();    // "Woof!" (own)

// Creates object with NO prototype (dictionary-like)
const dict = Object.create(null);
dict.key = "value"; // No inherited properties!
```

---

### Prototypal Inheritance Patterns

#### 1. Constructor + Prototype (ES5 style)

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  console.log(`${this.name} makes a sound`);
};

function Dog(name, breed) {
  Animal.call(this, name); // Call parent constructor
  this.breed = breed;
}

// Link prototypes
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // Fix constructor

Dog.prototype.speak = function() {
  console.log(`${this.name} barks!`);
};

const rex = new Dog("Rex", "Labrador");
rex.speak(); // "Rex barks!"
```

#### 2. ES6 Classes (Syntactic Sugar)

```javascript
class Animal {
  constructor(name) { this.name = name; }
  speak() { console.log(`${this.name} makes a sound`); }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Must call super() first!
    this.breed = breed;
  }
  speak() {
    super.speak(); // Call parent method
    console.log(`${this.name} barks!`);
  }
}
```

#### 3. Factory Functions + `Object.assign`

```javascript
const canWalk = { walk() { console.log("Walking..."); } };
const canSwim = { swim() { console.log("Swimming..."); } };

function createDuck(name) {
  return Object.assign(
    Object.create({ quack() { console.log("Quack!"); } }),
    { name },
    canWalk,
    canSwim
  );
}
const duck = createDuck("Donald");
```

---

### Prototype Gotchas

| Issue | Example | Fix |
|---|---|---|
| **Shared mutable state** | `Person.prototype.friends = []` → all instances share array | Initialize in constructor: `this.friends = []` |
| **Modifying built-ins** | `Array.prototype.myMethod = ...` | Avoid! Use composition or utility functions |
| **`__proto__` vs `prototype`** | `obj.__proto__` = instance link; `Fn.prototype` = constructor's template | Use `Object.getPrototypeOf()` / `Object.setPrototypeOf()` |
| **Arrow functions as methods** | `Person.prototype.greet = () => this.name` — `this` is lexical | Use regular functions for prototype methods |

---

### `instanceof` & `isPrototypeOf`

```javascript
function Cat() {}
const kitty = new Cat();

kitty instanceof Cat;        // true
kitty instanceof Object;     // true
Cat.prototype.isPrototypeOf(kitty); // true
Object.prototype.isPrototypeOf(kitty); // true

// Symbol.hasInstance customization
class MyClass {
  static [Symbol.hasInstance](instance) {
    return instance.hasCustomFlag;
  }
}
const obj = { hasCustomFlag: true };
obj instanceof MyClass; // true!
```

---

## 2️⃣ `this` Keyword — Binding Rules

### What is `this`?

`this` is a **runtime binding** determined by **how a function is called** (call-site), not where it's defined. It enables function reuse across different contexts.

### Four Binding Rules (Priority Order)

| # | Rule | Call-Site Pattern | `this` Value |
|---|---|---|---|
| 1 | **New Binding** | `new Fn()` | Newly created object |
| 2 | **Explicit Binding** | `fn.call(obj)`, `fn.apply(obj)`, `fn.bind(obj)` | Specified `obj` |
| 3 | **Implicit Binding** | `obj.fn()` | `obj` (owner object) |
| 4 | **Default Binding** | `fn()` | `globalThis` (or `undefined` in strict) |

---

### Rule 1: New Binding

```javascript
function Person(name) {
  this.name = name; // this = new object
}
const alice = new Person("Alice"); // this bound to alice
```

---

### Rule 2: Explicit Binding

```javascript
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const user = { name: "Alice" };

// call - arguments individually
greet.call(user, "Hello", "!"); // "Hello, Alice!"

// apply - arguments as array
greet.apply(user, ["Hi", "."]); // "Hi, Alice."

// bind - returns NEW function with this fixed
const greetUser = greet.bind(user);
greetUser("Hey", "?"); // "Hey, Alice?"

// Partial application with bind
const sayHello = greet.bind(user, "Hello");
sayHello("!"); // "Hello, Alice!"
```

**Hard Binding** — `bind` creates permanent binding that cannot be overridden:

```javascript
function foo() { console.log(this.a); }
const obj = { a: 2 };
const bar = foo.bind(obj);
bar();              // 2
bar.call({ a: 3 }); // 2 (ignored!)
new bar();          // 2 (ignored! but still creates object)
```

---

### Rule 3: Implicit Binding

```javascript
const user = {
  name: "Alice",
  greet() { console.log(this.name); }
};

user.greet(); // "Alice" — this = user

// ⚠️ Lost binding when assigned/detached
const greet = user.greet;
greet(); // undefined (strict) or global (sloppy) — LOST!
```

**Common Loss Scenarios:**

```javascript
const user = { name: "Alice", greet() { return this.name; } };

// 1. Callback
setTimeout(user.greet, 100); // this = global/undefined

// 2. Event handler
button.addEventListener("click", user.greet); // this = button

// 3. Method passed as argument
["a", "b"].forEach(user.greet); // this = global/undefined

// 4. Destructuring
const { greet } = user;
greet(); // lost!
```

**Fixes:**

```javascript
// Arrow function (lexical this)
setTimeout(() => user.greet(), 100);

// bind
setTimeout(user.greet.bind(user), 100);

// Wrapper function
setTimeout(function() { user.greet(); }, 100);
```

---

### Rule 4: Default Binding

```javascript
function foo() { console.log(this); }
foo(); // globalThis (window/global) — sloppy mode
       // undefined — strict mode ("use strict")
```

---

### Arrow Functions — Lexical `this`

```javascript
const obj = {
  name: "Alice",
  regular: function() { console.log(this.name); },
  arrow: () => console.log(this.name) // this from OUTER scope!
};

obj.regular(); // "Alice"
obj.arrow();   // undefined (or global) — this is module/window!

// Use case: preserving this in callbacks
class Timer {
  constructor() {
    this.seconds = 0;
  }
  start() {
    // Arrow captures this from Timer instance
    setInterval(() => {
      this.seconds++;
      console.log(this.seconds);
    }, 1000);
  }
}
```

### `this` Priority Summary

```
new Fn()                    → new object (highest)
fn.call(obj) / fn.bind(obj) → obj
obj.fn()                    → obj
fn()                        → global/undefined (lowest)

Arrow functions: IGNORE all above, use lexical scope this
```

---

### `this` in Class Context

```javascript
class Counter {
  count = 0;
  
  // Instance method (on prototype)
  increment() { this.count++; }
  
  // Arrow property (own, per-instance)
  incrementArrow = () => { this.count++; }
  
  // Static method
  static reset() { /* this = Counter class */ }
}

const c = new Counter();
const inc = c.increment;     // Lost binding!
const incArrow = c.incrementArrow; // Bound to instance!
inc();         // TypeError (strict) or NaN
incArrow();    // Works! count = 1
```

---

## 3️⃣ Event Loop & Concurrency Model

### JavaScript is Single-Threaded

> **One call stack, one thread of execution** — but non-blocking via event loop.

---

### Runtime Components

```
┌─────────────────────────────────────────────────────────┐
│                    JAVASCRIPT ENGINE                    │
│  ┌─────────────┐     ┌─────────────────────────────┐  │
│  │  CALL STACK │     │         HEAP (memory)        │  │
│  │  (frames)   │     │  Objects, functions, etc.   │  │
│  └──────┬──────┘     └─────────────────────────────┘  │
└─────────┼─────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────┐
│                  WEB APIs (Browser) / C++ APIs (Node)   │
│  setTimeout, DOM, fetch, fs, HTTP, timers, events...   │
└─────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────┐
│                    CALLBACK QUEUE                       │
│  ┌──────────┬──────────┬──────────┬──────────┐         │
│  │ Macro    │ Micro    │ Macro    │ Micro    │ ...     │
│  │ Task     │ Task     │ Task     │ Task     │         │
│  └──────────┴──────────┴──────────┴──────────┘         │
└─────────────────────────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────────────────────┐
│                    EVENT LOOP                           │
│  while (stack empty) { run oldest microtask queue;     │
│                         run oldest macrotask queue; }  │
└─────────────────────────────────────────────────────────┘
```

---

### Call Stack — LIFO Execution

```javascript
function first() { second(); }
function second() { third(); }
function third() { console.log("Hi"); }

first();
// Stack: first → second → third → console.log → pop → pop → pop → pop
```

---

### Task Queues: Macro vs Micro

| Queue Type | Examples | Priority |
|---|---|---|
| **Microtask** | `Promise.then/catch/finally`, `queueMicrotask`, `MutationObserver`, `process.nextTick` (Node) | **Higher** — drain completely before next macrotask |
| **Macrotask** | `setTimeout`, `setInterval`, `setImmediate` (Node), I/O, UI rendering, `postMessage` | Lower — one per loop iteration |

---

### Event Loop Algorithm

```javascript
// Simplified event loop
while (true) {
  // 1. Run oldest macrotask (if stack empty)
  const macrotask = macrotaskQueue.shift();
  if (macrotask) run(macrotask);
  
  // 2. Run ALL microtasks until queue empty
  while (microtaskQueue.length) {
    const microtask = microtaskQueue.shift();
    run(microtask);
  }
  
  // 3. Render (browser only)
  if (browser) render();
  
  // 4. Repeat
}
```

---

### Execution Order Examples

```javascript
console.log("1. Sync start");

setTimeout(() => console.log("2. setTimeout (macro)"), 0);

Promise.resolve()
  .then(() => console.log("3. Promise (micro)"));

queueMicrotask(() => console.log("4. queueMicrotask (micro)"));

console.log("5. Sync end");

// Output:
// 1. Sync start
// 5. Sync end
// 3. Promise (micro)
// 4. queueMicrotask (micro)
// 2. setTimeout (macro)
```

---

### Complex Example

```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve().then(() => console.log("C"));

queueMicrotask(() => console.log("D"));

setTimeout(() => {
  console.log("E");
  Promise.resolve().then(() => console.log("F"));
}, 0);

console.log("G");

// Output:
// A
// G
// C
// D
// B
// E
// F
```

**Step-by-step:**
1. Sync: A, G
2. Microtasks: C, D
3. Macrotask 1 (setTimeout B): runs B
4. Microtasks from B: none
5. Macrotask 2 (setTimeout E): runs E
6. Microtasks from E: F

---

### `async`/`await` & Microtasks

```javascript
async function foo() {
  console.log("1");
  await Promise.resolve();
  console.log("2"); // Runs as microtask!
}
async function bar() {
  console.log("3");
  await Promise.resolve();
  console.log("4");
}

foo();
bar();
console.log("5");

// Output: 1, 3, 5, 2, 4
// await creates microtask for continuation
```

---

### Node.js Specifics

```javascript
// process.nextTick — runs BEFORE other microtasks (highest priority)
process.nextTick(() => console.log("nextTick"));

// setImmediate — runs AFTER I/O callbacks, before next timers
setImmediate(() => console.log("setImmediate"));

// Order in Node:
setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
process.nextTick(() => console.log("nextTick"));
Promise.resolve().then(() => console.log("promise"));

// Output:
// nextTick
// promise
// timeout/immediate (order varies by Node version)
```

---

### Practical Implications

```javascript
// ❌ Blocking the main thread
function heavy() {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) sum += i;
  return sum;
}
heavy(); // UI freezes!

// ✅ Yield to event loop
async function heavyAsync() {
  let sum = 0;
  for (let i = 0; i < 1e9; i++) {
    sum += i;
    if (i % 1e6 === 0) await Promise.resolve(); // Yield!
  }
  return sum;
}

// ✅ Web Workers for true parallelism
const worker = new Worker("heavy.js");
worker.postMessage(data);
worker.onmessage = (e) => console.log(e.data);
```

---

## 4️⃣ Memory Management & Garbage Collection

### Memory Lifecycle

```
┌─────────────┐    Allocate    ┌─────────────┐    Use    ┌─────────────┐
│  Unused     │ ─────────────► │  Allocated  │ ───────► │  In Use     │
│  Memory     │                │  (Heap)     │          │  (Reachable)│
└─────────────┘                └─────────────┘          └──────┬──────┘
                                                               │
                                                               ▼
┌─────────────┐    Free        ┌─────────────┐    Collect ┌─────────────┐
│  Unused     │ ◄───────────── │  Unreachable│ ◄───────── │  Garbage    │
│  Memory     │                │  (Garbage)  │            │  Collection │
└─────────────┘                └─────────────┘            └─────────────┘
```

---

### Reachability — Core Concept

An object is **reachable** (kept alive) if:
1. **Root references** — global variables, local variables in active stack frames
2. **Chain of references** — reachable from roots via properties

```javascript
// Roots: global object, current execution context

let user = { name: "Alice" };     // user → object (reachable)
let admin = user;                 // admin → same object (reachable)
user = null;                      // admin still references it (reachable)
admin = null;                     // NO references → UNREACHABLE → GC eligible

// Circular references — still collected if unreachable from roots!
function marry(man, woman) {
  man.wife = woman;
  woman.husband = man;
}
let alice = { name: "Alice" };
let bob = { name: "Bob" };
marry(alice, bob); // Circular!
alice = null;
bob = null; // Both unreachable → collected together
```

---

### Garbage Collection Algorithms

#### 1. Mark-and-Sweep (Standard)

```
Phase 1: MARK
  Start from roots, traverse all references, mark visited objects

Phase 2: SWEEP
  Free memory of unmarked objects

Phase 3: COMPACT (optional)
  Move surviving objects to eliminate fragmentation
```

```javascript
// Simplified mental model
function gc() {
  const marked = new Set();
  
  function mark(obj) {
    if (marked.has(obj) || obj === null) return;
    marked.add(obj);
    for (const key of Object.keys(obj)) {
      mark(obj[key]);
    }
  }
  
  // Mark from roots
  mark(globalThis);
  mark(currentStackFrame);
  
  // Sweep
  for (const obj of allHeapObjects) {
    if (!marked.has(obj)) freeMemory(obj);
  }
}
```

#### 2. Generational GC (V8, SpiderMonkey, etc.)

- **Young Generation (Nursery)** — newly allocated, frequent GC, fast
- **Old Generation** — survived multiple GCs, infrequent GC, slower

```
Allocation → Young Gen (Scavenge/Minor GC)
    │
    ├── Dies young → Freed immediately
    │
    └── Survives 2+ cycles → Promoted to Old Gen
                              │
                              ├── Major GC (Mark-Sweep-Compact)
                              └── Incremental/Concurrent GC
```

#### 3. Incremental & Concurrent GC

- **Incremental** — break GC work into small chunks, interleave with JS
- **Concurrent** — GC runs on separate thread (V8 Orinoco)
- **Idle-time GC** — run during browser idle callbacks

---

### VGC (V8) Specifics

| Generation | Algorithm | Trigger |
|---|---|---|
| Young (New Space) | Scavenge (semi-space copy) | Allocation limit reached |
| Old (Old Space) | Mark-Sweep-Compact | Heap limit, allocation rate |
| Large Objects | Separate space | Direct allocation |

---

### Memory Leaks — Common Patterns

| Leak Type | Example | Fix |
|---|---|---|
| **Global pollution** | `leak = "data"` (no var/let/const) | Use strict mode, linting |
| **Forgotten timers** | `setInterval(() => {}, 1000)` never cleared | `clearInterval(id)` in cleanup |
| **Event listeners** | `el.addEventListener("click", handler)` not removed | `removeEventListener`, `AbortController` |
| **Closures** | `const bigData = ...; btn.onclick = () => bigData` | Nullify refs, use WeakMap |
| **Detached DOM** | `const el = doc.createElement(); el.remove()` but ref kept | Nullify: `el = null` |
| **Caches without eviction** | `cache.set(key, hugeObject)` never deleted | `WeakMap`, LRU, TTL |

---

### Leak Detection Tools

```javascript
// Chrome DevTools: Memory tab
// 1. Heap snapshot → compare
// 2. Allocation timeline
// 3. Allocation sampling

// Node.js: --inspect + Chrome DevTools
// node --inspect app.js

// Programmatic (Node)
const v8 = require("v8");
v8.writeHeapSnapshot(); // .heapsnapshot file

// WeakRef/FinalizationRegistry (ES2021) — observe GC
const registry = new FinalizationRegistry((heldValue) => {
  console.log("GC'd:", heldValue);
});
const obj = { data: "large" };
registry.register(obj, "obj-id");
obj = null; // Eventually logs "GC'd: obj-id"
```

---

### WeakMap / WeakSet — GC-Friendly

```javascript
// WeakMap: keys are weakly held (don't prevent GC)
const privateData = new WeakMap();

class Component {
  constructor() {
    privateData.set(this, { secret: 42 }); // Key = this
  }
  getSecret() { return privateData.get(this).secret; }
}

const comp = new Component();
// When comp is unreachable, entry auto-removed!

// WeakSet: weakly held objects
const tracked = new WeakSet();
tracked.add(component);
// component = null → removed from WeakSet
```

---

### Memory Optimization Patterns

```javascript
// 1. Object pooling (reuse instead of allocate)
class Pool {
  constructor(factory, reset) {
    this.factory = factory;
    this.reset = reset;
    this.free = [];
  }
  acquire() { return this.free.pop() || this.factory(); }
  release(obj) { this.reset(obj); this.free.push(obj); }
}

// 2. Avoid creating objects in hot loops
// ❌ Bad
for (let i = 0; i < 10000; i++) {
  const point = { x: i, y: i * 2 }; // New object each iteration
}

// ✅ Good
const point = { x: 0, y: 0 };
for (let i = 0; i < 10000; i++) {
  point.x = i; point.y = i * 2;
  use(point);
}

// 3. Use typed arrays for numeric data
const buffer = new Float32Array(1_000_000); // 4MB vs ~24MB for objects

// 4. Nullify references when done
function process(data) {
  const result = heavyCompute(data);
  data = null; // Help GC if data is large
  return result;
}
```

---

### GC Performance Metrics

| Metric | Target | Tool |
|---|---|---|
| **GC Pause Time** | < 10ms (ideal < 4ms) | Chrome Performance tab |
| **GC Frequency** | Minimal major GCs | `node --trace-gc` |
| **Heap Size** | Stable, not growing | `process.memoryUsage()` |
| **Allocation Rate** | Low in hot paths | DevTools allocation profiler |

```javascript
// Monitor in Node
setInterval(() => {
  const usage = process.memoryUsage();
  console.log({
    rss: (usage.rss / 1024 / 1024).toFixed(2) + " MB",
    heapUsed: (usage.heapUsed / 1024 / 1024).toFixed(2) + " MB",
    heapTotal: (usage.heapTotal / 1024 / 1024).toFixed(2) + " MB",
    external: (usage.external / 1024 / 1024).toFixed(2) + " MB"
  });
}, 5000);
```

---

## 📚 Summary Cheatsheet

| Topic | Key Takeaways |
|---|---|
| **Prototypes** | Objects link to prototype; chain ends at `null`; `Object.create()` for direct linking; ES6 classes = sugar |
| **`this` Binding** | 4 rules (new > explicit > implicit > default); arrow = lexical; `bind` creates hard binding |
| **Event Loop** | Single-threaded; call stack + Web APIs + queues; microtasks drain before each macrotask |
| **Memory/GC** | Reachability from roots; mark-sweep; generational (young/old); leaks = unintentional retention; WeakMap/WeakRef for caches |

---

## 🔗 Resources

- [MDN: Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [You Don't Know JS: this & Object Prototypes](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/this-object-prototypes)
- [Event Loop Talk (Philip Roberts)](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [V8 GC Blog](https://v8.dev/blog/trash-talk)
- [Memory Management (MDN)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)
- [WeakRef/FinalizationRegistry](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef)