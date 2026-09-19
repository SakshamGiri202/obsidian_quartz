# ES6+ Features (ECMAScript 2015+)

> **Comprehensive Reference** combining concepts from W3Schools, MDN, and modern JavaScript practices

---

## 📌 What is ES6?

**ES6 (ECMAScript 2015)** is the 6th edition of the ECMAScript standard — the specification that defines JavaScript. It was a **major revision** (the first since ES5 in 2009) that introduced significant new syntax and features to make JavaScript more powerful, readable, and maintainable.

### Why ES6 Matters

| Problem Before ES6                             | ES6 Solution                               |
| ---------------------------------------------- | ------------------------------------------ |
| `var` has function scope, causes hoisting bugs | `let` / `const` with **block scope**       |
| Verbose function syntax                        | **Arrow functions** (`=>`)                 |
| No native modules                              | **ES Modules** (`import`/`export`)         |
| Callback hell for async code                   | **Promises** + `async`/`await`             |
| No built-in data structures                    | **Map**, **Set**, **WeakMap**, **WeakSet** |
| String concatenation mess                      | **Template literals** (backticks)          |
| Manual object property assignment              | **Destructuring**, **spread/rest**         |
| Prototype-based inheritance confusion          | **Classes** (syntactic sugar)              |
| No unique property keys                        | **Symbol** primitive type                  |

---

## 1️⃣ Block-Scoped Declarations: `let` & `const`

### What
- `let` — mutable block-scoped variable
- `const` — immutable binding (reference), block-scoped

### Why
- Fixes `var` hoisting issues
- Prevents accidental re-declaration
- Enables temporal dead zone (TDZ) — catches bugs early

```javascript
// var (function-scoped, hoisted)
function varExample() {
  console.log(x); // undefined (hoisted)
  var x = 10;
}

// let/const (block-scoped, TDZ)
function letExample() {
  // console.log(y); // ReferenceError: Cannot access 'y' before initialization
  let y = 20;
  const z = 30; // Must initialize at declaration
}

// Block scope demonstration
if (true) {
  let blockScoped = "I exist only in this block";
  const alsoBlockScoped = "Me too";
}
// console.log(blockScoped); // ReferenceError
```

### Key Differences

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| Scope | Function | Block | Block |
| Hoisting | Yes (initialized undefined) | Yes (TDZ) | Yes (TDZ) |
| Re-declaration | Allowed | Not in same scope | Not in same scope |
| Re-assignment | Allowed | Allowed | Not allowed |
| Global object property | Yes | No | No |

---

## 2️⃣ Arrow Functions (`=>`)

### What
Concise syntax for function expressions with lexical `this` binding.

### Why
- Shorter syntax for callbacks
- **Lexical `this`** — inherits `this` from enclosing scope (no more `var self = this`)
- Implicit return for single expressions

```javascript
// Traditional
const add = function(a, b) { return a + b; };

// Arrow - explicit return
const add = (a, b) => { return a + b; };

// Arrow - implicit return (single expression)
const add = (a, b) => a + b;

// Single param - parentheses optional
const square = x => x * x;

// No params
const greet = () => "Hello";

// Object literal return - needs parentheses
const createUser = (name, age) => ({ name, age });

// ⚠️ Arrow functions DON'T have their own:
// - this
// - arguments
// - super
// - new.target
// Cannot be used as constructors (no new)
```

### When NOT to Use Arrow Functions
- Object methods (need `this` context)
- Prototype methods
- Constructor functions
- When you need `arguments` object

---

## 3️⃣ Template Literals (Template Strings)

### What
Strings using backticks (`` ` ``) supporting interpolation, multiline, and tagged templates.

### Why
- Clean string interpolation: `` `Hello ${name}` ``
- Multiline without `\n`
- Tagged templates for DSLs/sanitization

```javascript
const name = "Alice";
const age = 30;

// Interpolation
const greeting = `Hello, ${name}! You are ${age} years old.`;

// Multiline
const html = `
  <div>
    <h1>${name}</h1>
    <p>Age: ${age}</p>
  </div>
`;

// Expression interpolation
const price = 19.99;
const tax = 0.08;
const total = `Total: $${(price * (1 + tax)).toFixed(2)}`;

// Tagged template (advanced)
function highlight(strings, ...values) {
  return strings.reduce((acc, str, i) => 
    acc + str + (values[i] ? `<mark>${values[i]}</mark>` : '')
  , '');
}
const result = highlight`Hello ${name}, welcome to ${"ES6"}!`;
// "Hello <mark>Alice</mark>, welcome to <mark>ES6</mark>!"
```

---

## 4️⃣ Destructuring Assignment

### What
Unpack values from arrays/objects into distinct variables.

### Why
- Cleaner extraction of data
- Default values
- Renaming variables
- Nested destructuring

```javascript
// Object destructuring
const user = { name: "Alice", age: 30, city: "NYC" };
const { name, age } = user; // name="Alice", age=30

// Renaming
const { name: userName, age: userAge } = user;

// Default values
const { country = "USA" } = user; // country="USA" (default)

// Nested destructuring
const person = {
  name: "Bob",
  address: { city: "LA", zip: 90001 }
};
const { address: { city, zip } } = person;

// Array destructuring
const colors = ["red", "green", "blue"];
const [first, second] = colors; // first="red", second="green"

// Skip elements
const [, , third] = colors; // third="blue"

// Rest pattern
const [head, ...tail] = colors; // head="red", tail=["green", "blue"]

// Default values
const [a = 1, b = 2] = []; // a=1, b=2

// Swapping variables (no temp needed)
let x = 1, y = 2;
[x, y] = [y, x]; // x=2, y=1

// Function parameter destructuring
function greet({ name, age = 25 }) {
  console.log(`${name} is ${age}`);
}
greet({ name: "Carol" }); // "Carol is 25"
```

---

## 5️⃣ Spread (`...`) & Rest (`...`) Operators

### Spread Operator — Expands Iterables

```javascript
// Array copying (shallow)
const arr1 = [1, 2, 3];
const arr2 = [...arr1]; // [1, 2, 3] - new array

// Array concatenation
const combined = [...arr1, 4, 5, ...[6, 7]]; // [1,2,3,4,5,6,7]

// Object copying/merging (ES2018+)
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a:1, b:2, c:3 }
const merged = { ...obj1, ...{ b: 20, d: 4 } }; // { a:1, b:20, d:4 }

// String to array
const chars = [... "hello"]; // ["h","e","l","l","o"]

// Function arguments
const numbers = [1, 2, 3];
Math.max(...numbers); // 3
```

### Rest Operator — Collects Remaining Elements

```javascript
// Function parameters
function sum(...args) {
  return args.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3, 4); // 10

// Mixed with regular params
function multiply(multiplier, ...numbers) {
  return numbers.map(n => n * multiplier);
}
multiply(2, 1, 2, 3); // [2, 4, 6]

// Destructuring
const [first, ...rest] = [1, 2, 3, 4]; // first=1, rest=[2,3,4]
const { a, ...others } = { a: 1, b: 2, c: 3 }; // a=1, others={b:2,c:3}
```

---

## 6️⃣ Enhanced Object Literals

### What
Shorthand syntax for object creation.

### Why
- Less repetition
- Computed property names
- Method shorthand

```javascript
const name = "Alice";
const age = 30;

// Property shorthand (name: name → name)
const user = { name, age }; // { name: "Alice", age: 30 }

// Method shorthand
const calculator = {
  add(a, b) { return a + b; },
  subtract(a, b) { return a - b; }
};

// Computed property names
const prop = "dynamicKey";
const obj = {
  [prop]: "value",
  [`${prop}_suffix`]: "another"
};

// Getter/Setter
const person = {
  firstName: "John",
  lastName: "Doe",
  get fullName() { return `${this.firstName} ${this.lastName}`; },
  set fullName(value) {
    [this.firstName, this.lastName] = value.split(" ");
  }
};
```

---

## 7️⃣ Classes (Syntactic Sugar over Prototypes)

### What
Clean syntax for constructor functions and prototype inheritance.

### Why
- Familiar OOP syntax
- Clearer inheritance with `extends`/`super`
- Static methods, getters/setters

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  // Instance method
  speak() {
    console.log(`${this.name} makes a sound`);
  }
  
  // Getter
  get species() { return this.constructor.name; }
  
  // Static method
  static info() { console.log("Animal class"); }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Must call super() first!
    this.breed = breed;
  }
  
  // Override
  speak() {
    super.speak(); // Call parent
    console.log(`${this.name} barks!`);
  }
}

const dog = new Dog("Rex", "Labrador");
dog.speak(); 
// "Rex makes a sound"
// "Rex barks!"

Dog.info(); // "Animal class"
```

### Class Features Summary

| Feature | Syntax |
|---|---|
| Constructor | `constructor() {}` |
| Inheritance | `class Child extends Parent` |
| Parent call | `super()` / `super.method()` |
| Static method | `static methodName() {}` |
| Static property | `static prop = value` (ES2022) |
| Private field | `#fieldName` (ES2022) |
| Private method | `#methodName()` (ES2022) |

---

## 8️⃣ Modules (ESM — ECMAScript Modules)

### What
Native module system with `import`/`export`.

### Why
- Encapsulation (no global pollution)
- Explicit dependencies
- Tree-shaking (dead code elimination)
- Browser & Node.js support

```javascript
// math.js — Named exports
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }

// Default export
export default function multiply(a, b) { return a * b; }

// ---- OR combined ----
export { PI, add, subtract };
export default multiply;

// --- Importing ---
// Named imports
import { add, PI } from "./math.js";
import { add as addNumbers } from "./math.js"; // Alias

// Default import
import multiply from "./math.js";

// Combined
import multiply, { PI, add } from "./math.js";

// Namespace import
import * as MathUtils from "./math.js";
MathUtils.add(1, 2);

// Dynamic import (returns Promise)
const module = await import("./math.js");

// Re-exporting
export { add } from "./math.js";
export * from "./math.js";
```

### Module Characteristics
- **Strict mode** by default
- **Singleton** — same module imported multiple times shares one instance
- **Deferred** execution (like `defer` script)
- **CORS** required for cross-origin

---

## 9️⃣ Promises

### What
Object representing eventual completion/failure of async operation.

### Why
- Avoids callback hell
- Composable (`.then()`, `.catch()`, `.finally()`)
- Better error handling
- Enables `async`/`await`

```javascript
// Creating a Promise
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = Math.random() > 0.5;
    if (success) resolve("Data received!");
    else reject(new Error("Network error"));
  }, 1000);
});

// Consuming
fetchData
  .then(data => console.log(data))
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));

// Promise chaining
fetchUser(1)
  .then(user => fetchPosts(user.id))
  .then(posts => render(posts))
  .catch(handleError);

// Parallel execution
Promise.all([fetchUsers(), fetchPosts()])
  .then(([users, posts]) => { /* both ready */ });

Promise.race([fetchFast(), fetchSlow()])
  .then(firstResult => { /* first to settle */ });

// All settled (ES2020)
Promise.allSettled([p1, p2]).then(results => {
  results.forEach(r => r.status === "fulfilled" ? r.value : r.reason);
});

// Promise static methods
Promise.resolve(42);           // Resolved promise
Promise.reject(new Error());   // Rejected promise
Promise.all(iterable);         // All fulfill or first rejects
Promise.race(iterable);        // First to settle
Promise.allSettled(iterable);  // All settle
Promise.any(iterable);         // First to fulfill (ES2021)
```

---

## 🔟 Async / Await (ES2017)

### What
Syntactic sugar over Promises — makes async code look synchronous.

### Why
- Readable sequential async code
- Try/catch for error handling
- Easier debugging (stack traces)

```javascript
// Async function returns a Promise
async function fetchUserData(userId) {
  try {
    const user = await fetch(`/api/users/${userId}`).then(r => r.json());
    const posts = await fetch(`/api/users/${userId}/posts`).then(r => r.json());
    return { user, posts };
  } catch (error) {
    console.error("Failed:", error);
    throw error; // Re-throw or handle
  }
}

// Parallel await (not sequential!)
async function fetchAll() {
  // These run in PARALLEL
  const [users, posts, comments] = await Promise.all([
    fetchUsers(),
    fetchPosts(),
    fetchComments()
  ]);
  return { users, posts, comments };
}

// Top-level await (modules only)
const data = await fetchData();
```

---

## 1️⃣1️⃣ Map & Set

### Map — Key-Value Collections

```javascript
const map = new Map();

// Any type as key
map.set("string", "value");
map.set(123, "number key");
map.set({ id: 1 }, "object key");
map.set(() => {}, "function key");

map.get("string");     // "value"
map.has(123);          // true
map.size;              // 4
map.delete("string");  // true
map.clear();           // empties map

// Iteration (insertion order preserved)
for (const [key, value] of map) { /* ... */ }
map.forEach((value, key) => { /* ... */ });

// From array/object
const map2 = new Map([["a", 1], ["b", 2]]);
const map3 = new Map(Object.entries({ x: 10, y: 20 }));
```

### Set — Unique Values

```javascript
const set = new Set([1, 2, 2, 3, 3, 3]);
// Set {1, 2, 3}

set.add(4);      // Set {1,2,3,4}
set.has(2);      // true
set.delete(3);   // true
set.size;        // 3
set.clear();     // empty

// Iteration
for (const val of set) { /* ... */ }

// Array deduplication
const unique = [...new Set([1,2,2,3,3,3])]; // [1,2,3]

// Set operations
const a = new Set([1,2,3]);
const b = new Set([3,4,5]);
const union = new Set([...a, ...b]);        // {1,2,3,4,5}
const intersection = new Set([...a].filter(x => b.has(x))); // {3}
const difference = new Set([...a].filter(x => !b.has(x)));  // {1,2}
```

### WeakMap / WeakSet
- Keys must be objects (weak references)
- Not iterable
- Allow garbage collection of keys
- Use cases: private data, caching, metadata

---

## 1️⃣2️⃣ Symbol

### What
Unique, immutable primitive type for object property keys.

### Why
- **Collision-free** property keys
- "Hidden" properties (not in `for...in`, `Object.keys()`, `JSON.stringify`)
- Well-known symbols for protocol customization

```javascript
const sym1 = Symbol("description");
const sym2 = Symbol("description");
sym1 === sym2; // false — always unique!

// Global symbol registry
const gs1 = Symbol.for("app.user");
const gs2 = Symbol.for("app.user");
gs1 === gs2; // true — same global symbol

Symbol.keyFor(gs1); // "app.user"

// As object keys
const user = { name: "Alice" };
const id = Symbol("id");
user[id] = 12345; // Hidden from normal enumeration

Object.keys(user);        // ["name"]
Object.getOwnPropertySymbols(user); // [Symbol(id)]

// Well-known symbols
const iterable = {
  [Symbol.iterator]() {
    let i = 0;
    return { next: () => i < 3 ? { value: i++, done: false } : { done: true } };
  }
};
[...iterable]; // [0, 1, 2]

// Custom toStringTag
class MyClass {
  get [Symbol.toStringTag]() { return "MyClass"; }
}
Object.prototype.toString.call(new MyClass()); // "[object MyClass]"
```

---

## 1️⃣3️⃣ Iterators & Generators

### Iterator Protocol
```javascript
const iterable = {
  data: [1, 2, 3],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => index < this.data.length
        ? { value: this.data[index++], done: false }
        : { done: true }
    };
  }
};

for (const val of iterable) { /* 1, 2, 3 */ }
```

### Generators — Pause/Resume Functions

```javascript
function* generator() {
  yield 1;
  yield 2;
  yield 3;
  return 4; // Final value (done: true)
}

const gen = generator();
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
gen.next(); // { value: 3, done: false }
gen.next(); // { value: 4, done: true }

// Infinite sequences
function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}
const fib = fibonacci();
fib.next().value; // 0
fib.next().value; // 1
fib.next().value; // 1

// Generator delegation
function* outer() {
  yield 1;
  yield* inner(); // Delegate
  yield 4;
}
function* inner() { yield 2; yield 3; }

// Async generators (ES2018)
async function* asyncGen() {
  yield await fetch1();
  yield await fetch2();
}
for await (const item of asyncGen()) { /* ... */ }
```

---

## 1️⃣4️⃣ Default & Rest Parameters

### Default Parameters
```javascript
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}
greet();           // "Hello, Guest!"
greet("Alice");    // "Hello, Alice!"
greet("Bob", "Hi"); // "Hi, Bob!"

// Defaults evaluated at call time
function log(time = Date.now()) { console.log(time); }

// Destructuring with defaults
function connect({ host = "localhost", port = 3000 } = {}) { /* ... */ }
```

### Rest Parameters
```javascript
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10

// Must be last parameter
function log(prefix, ...messages) {
  messages.forEach(m => console.log(prefix, m));
}
```

---

## 1️⃣5️⃣ String Enhancements

```javascript
const str = "Hello World";

// includes/search
str.includes("World");     // true
str.startsWith("Hello");   // true
str.endsWith("World");     // true
str.startsWith("World", 6); // true (position)

// repeat
"ha".repeat(3); // "hahaha"

// padStart/padEnd (ES2017)
"5".padStart(2, "0");   // "05"
"5".padEnd(3, "0");     // "500"

// trimStart/trimEnd (ES2019)
"  hello  ".trimStart(); // "hello  "
"  hello  ".trimEnd();   // "  hello"

// replaceAll (ES2021)
"foo foo".replaceAll("foo", "bar"); // "bar bar"

// at() (ES2022) — negative index support
"abc".at(-1); // "c"
```

---

## 1️⃣6️⃣ Array Enhancements

```javascript
const arr = [1, 2, 3, 4, 5];

// find / findIndex
arr.find(x => x > 3);       // 4
arr.findIndex(x => x > 3);  // 3

// fill
[1,2,3].fill(0);        // [0,0,0]
[1,2,3].fill(9, 1);     // [1,9,9]
[1,2,3].fill(8, 1, 2);  // [1,8,3]

// copyWithin (mutates)
[1,2,3,4,5].copyWithin(0, 3); // [4,5,3,4,5]

// entries/keys/values (iterators)
[...arr.entries()]; // [[0,1],[1,2],[2,3],[3,4],[4,5]]
[...arr.keys()];    // [0,1,2,3,4]
[...arr.values()];  // [1,2,3,4,5]

// from / of
Array.from("abc");        // ["a","b","c"]
Array.from({length: 3}, (_, i) => i); // [0,1,2]
Array.of(1, 2, 3);        // [1,2,3] (vs Array(3) → [,,])

// flat / flatMap (ES2019)
[1, [2, [3]]].flat(2);        // [1,2,3]
["a", "b"].flatMap(x => [x, x.toUpperCase()]); // ["a","A","b","B"]

// at (ES2022)
arr.at(-1); // 5

// findLast / findLastIndex (ES2023)
[1,2,3,4].findLast(x => x % 2 === 0); // 4

// toSorted / toReversed / toSpliced / with (ES2023) — immutable
[3,1,2].toSorted();     // [1,2,3] (original unchanged)
[1,2,3].toReversed();   // [3,2,1]
[1,2,3].with(0, 99);    // [99,2,3]
```

---

## 1️⃣7️⃣ Object Enhancements

```javascript
// Object.assign (shallow merge)
const target = { a: 1 };
Object.assign(target, { b: 2 }, { c: 3 }); // { a:1, b:2, c:3 }

// Object.is (strict equality)
Object.is(NaN, NaN);       // true (vs NaN === NaN → false)
Object.is(0, -0);          // false (vs 0 === -0 → true)
Object.is(+0, -0);         // false

// Object.setPrototypeOf / getPrototypeOf
Object.setPrototypeOf(obj, proto);
Object.getPrototypeOf(obj);

// Object.keys/values/entries
Object.keys({a:1,b:2});      // ["a","b"]
Object.values({a:1,b:2});    // [1,2]
Object.entries({a:1,b:2});   // [["a",1],["b",2]]

// Object.fromEntries (ES2019) — reverse of entries
Object.fromEntries([["a", 1], ["b", 2]]); // {a:1,b:2}

// __proto__ accessors (standardized)
Object.prototype.__proto__; // Getter/setter for [[Prototype]]

// hasOwn (ES2022) — safer than hasOwnProperty
Object.hasOwn({a:1}, "a"); // true
```

---

## 1️⃣8️⃣ Number & Math Enhancements

```javascript
// Number properties
Number.EPSILON;                    // 2.22e-16
Number.MAX_SAFE_INTEGER;           // 9007199254740991 (2^53 - 1)
Number.MIN_SAFE_INTEGER;           // -9007199254740991
Number.isInteger(42);              // true
Number.isInteger(42.5);            // false
Number.isSafeInteger(9007199254740991); // true
Number.isSafeInteger(9007199254740992); // false
Number.isFinite(42);               // true
Number.isNaN(NaN);                 // true (vs global isNaN("x") → true!)

// Parse with radix
Number.parseInt("10", 2);  // 2
Number.parseFloat("3.14"); // 3.14

// Math methods
Math.trunc(4.9);    // 4
Math.sign(-5);      // -1
Math.cbrt(27);      // 3
Math.log2(8);       // 3
Math.log10(100);    // 2
Math.hypot(3, 4);   // 5 (√(3²+4²))
Math.expm1(1);      // e¹ - 1
Math.log1p(1);      // ln(1+1)
Math.sinh/cosh/tanh/asinh/acosh/atanh // Hyperbolic functions
```

---

## 1️⃣9️⃣ Proxy & Reflect (Meta-Programming)

### Proxy — Intercept Operations

```javascript
const target = { name: "Alice", age: 30 };

const handler = {
  get(target, prop, receiver) {
    console.log(`Getting ${prop}`);
    return Reflect.get(target, prop, receiver);
  },
  set(target, prop, value, receiver) {
    console.log(`Setting ${prop} = ${value}`);
    return Reflect.set(target, prop, value, receiver);
  },
  has(target, prop) {
    return prop in target;
  },
  deleteProperty(target, prop) {
    return Reflect.deleteProperty(target, prop);
  },
  ownKeys(target) {
    return Reflect.ownKeys(target).filter(k => !k.startsWith("_"));
  }
};

const proxy = new Proxy(target, handler);
proxy.name;     // Logs "Getting name", returns "Alice"
proxy.age = 31; // Logs "Setting age = 31"
"name" in proxy; // true
```

### Reflect — Default Operations

```javascript
const obj = { a: 1, b: 2 };

Reflect.get(obj, "a");           // 1
Reflect.set(obj, "c", 3);        // true, obj now {a:1,b:2,c:3}
Reflect.has(obj, "a");           // true
Reflect.deleteProperty(obj, "a"); // true
Reflect.ownKeys(obj);            // ["b", "c"]
Reflect.construct(Date, [2024, 0, 1]); // new Date(2024,0,1)
Reflect.apply(fn, thisArg, args); // fn.apply(thisArg, args)

// With Proxy — forward operations
const proxy = new Proxy({}, {
  get: (t, p) => Reflect.get(t, p),
  set: (t, p, v) => Reflect.set(t, p, v)
});
```

---

## 2️⃣0️⃣ Typed Arrays & ArrayBuffer (Binary Data)

```javascript
// ArrayBuffer — raw binary data
const buffer = new ArrayBuffer(16); // 16 bytes

// TypedArray views
const int8 = new Int8Array(buffer);     // 1-byte signed
const uint8 = new Uint8Array(buffer);   // 1-byte unsigned
const int16 = new Int16Array(buffer);   // 2-byte signed
const uint16 = new Uint16Array(buffer); // 2-byte unsigned
const int32 = new Int32Array(buffer);   // 4-byte signed
const float32 = new Float32Array(buffer); // 4-byte float
const float64 = new Float64Array(buffer); // 8-byte float

// Creation
new Uint8Array([1, 2, 3]);        // From array
new Uint8Array(10);               // Zero-filled length 10
new Uint8Array(arrayBuffer, 2, 4); // View subset

// DataView — heterogeneous access
const view = new DataView(buffer);
view.setInt8(0, 42);
view.setFloat64(1, 3.14);
view.getInt8(0);    // 42
view.getFloat64(1); // 3.14

// Endianness
view.setUint16(0, 0x1234, true);  // little-endian
view.setUint16(0, 0x1234, false); // big-endian
```

---

## 2️⃣1️⃣ RegExp Enhancements

```javascript
// Unicode flag (u)
/😀/.test("😀");           // false (surrogate pair issue)
/😀/u.test("😀");          // true
"😀".length;               // 2 (code units)
[..."😀"].length;          // 1 (code points)
/^.$/u.test("😀");        // true (matches one code point)

// Sticky flag (y) — match from lastIndex
const regex = /foo/y;
regex.lastIndex = 4;
regex.test("foo foo"); // true (matches at index 4)

// Named capture groups (ES2018)
const re = /(?<year>\d{4})-(?<month>\d{2})/;
const match = re.exec("2024-01");
match.groups.year;   // "2024"
match.groups.month;  // "01"

// Replace with named groups
"2024-01".replace(re, "$<month>/$<year>"); // "01/2024"

// Lookbehind (ES2018)
/(?<=\$)\d+/.exec("$100"); // ["100"]
/(?<!\$)\d+/.exec("100");  // ["100"]

// Unicode property escapes (ES2018)
/\p{Script=Greek}/u.test("α"); // true
/\p{Emoji}/u.test("😀");       // true
```

---

## 2️⃣2️⃣ Well-Known Symbols (Protocol Customization)

| Symbol | Purpose |
|---|---|
| `Symbol.iterator` | Makes object iterable (`for...of`, spread) |
| `Symbol.asyncIterator` | Async iteration (`for await...of`) |
| `Symbol.toStringTag` | `Object.prototype.toString` result |
| `Symbol.hasInstance` | `instanceof` behavior |
| `Symbol.isConcatSpreadable` | `Array.prototype.concat` flattening |
| `Symbol.species` | Constructor for derived objects |
| `Symbol.match` | `String.prototype.match` behavior |
| `Symbol.replace` | `String.prototype.replace` behavior |
| `Symbol.search` | `String.prototype.search` behavior |
| `Symbol.split` | `String.prototype.split` behavior |
| `Symbol.toPrimitive` | Object to primitive conversion |
| `Symbol.unscopables` | Properties hidden from `with` statement |

---

## 2️⃣3️⃣ Post-ES6 Features (Quick Reference)

### ES2016 (ES7)
- `Array.prototype.includes()`
- Exponentiation operator (`**`)

### ES2017 (ES8)
- `async`/`await`
- `Object.values()` / `Object.entries()`
- `String.prototype.padStart()` / `padEnd()`
- `Object.getOwnPropertyDescriptors()`
- Trailing commas in function params

### ES2018 (ES9)
- Rest/Spread for objects
- `Promise.prototype.finally()`
- Async iteration (`for await...of`)
- RegExp: named groups, lookbehind, `s` (dotAll), Unicode properties
- `Object.fromEntries()`

### ES2019 (ES10)
- `Array.prototype.flat()` / `flatMap()`
- `String.prototype.trimStart()` / `trimEnd()`
- `Object.fromEntries()`
- `Symbol.prototype.description`
- Optional catch binding (`catch {}`)

### ES2020 (ES11)
- Optional chaining (`?.`)
- Nullish coalescing (`??`)
- `Promise.allSettled()` / `Promise.any()`
- `String.prototype.matchAll()`
- `globalThis`
- Dynamic `import()`
- `BigInt`
- `import.meta`

### ES2021 (ES12)
- `String.prototype.replaceAll()`
- `Promise.any()` / `Promise.allSettled()`
- Logical assignment (`??=`, `||=`, `&&=`)
- Numeric separators (`1_000_000`)
- `WeakRef` / `FinalizationRegistry`

### ES2022 (ES13)
- `Array.prototype.at()`
- `Object.hasOwn()`
- Class fields (public `#x`, private `#x`)
- Private methods `#method()`
- Static class fields/methods
- `Error.prototype.cause`
- Top-level `await` (modules)

### ES2023 (ES14)
- `Array.prototype.findLast()` / `findLastIndex()`
- `Array.prototype.toSorted()` / `toReversed()` / `toSpliced()` / `with()`
- Hashbang (`#!`) for CLI scripts
- `Symbol.asyncDispose` / `Symbol.dispose` (using)

### ES2024 (ES15)
- `Object.groupBy()` / `Map.groupBy()`
- `Promise.withResolvers()`
- `Array.prototype.toSpliced()` (already in 2023)
- `ArrayBuffer.transfer()` / `resize()`

---

## 📚 Summary Cheatsheet

| Category | Key Features |
|---|---|
| **Variables** | `let`, `const`, TDZ |
| **Functions** | Arrow, default params, rest params, destructuring params |
| **Strings** | Template literals, `includes`, `startsWith`, `endsWith`, `padStart/End`, `repeat`, `replaceAll`, `at` |
| **Objects** | Shorthand, computed keys, `Object.assign`, `Object.entries/values/fromEntries`, destructuring, spread |
| **Arrays** | `find`, `findIndex`, `fill`, `copyWithin`, `flat`, `flatMap`, `from`, `of`, `at`, `toSorted`, `with` |
| **Classes** | `class`, `extends`, `super`, `static`, getters/setters, private fields (`#`) |
| **Modules** | `import`/`export`, dynamic `import()`, `import.meta` |
| **Async** | `Promise`, `async`/`await`, `Promise.all/race/allSettled/any` |
| **Collections** | `Map`, `Set`, `WeakMap`, `WeakSet` |
| **Primitives** | `Symbol`, `BigInt` |
| **Iteration** | Iterators, Generators, `for...of`, async iteration |
| **Meta** | `Proxy`, `Reflect`, Well-known symbols |
| **Binary** | `ArrayBuffer`, TypedArrays, `DataView` |
| **RegExp** | `u`, `y`, named groups, lookbehind, `\p{}` |

---

## 🔗 Resources

- [W3Schools ES6 Tutorial](https://www.w3schools.com/js/js_es6.asp)
- [MDN ECMAScript 2015+](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/ECMAScript_2015)
- [ECMAScript Spec](https://tc39.es/ecma262/)
- [ES6 Features Repo](https://github.com/lukehoban/es6features)
- [Exploring ES6 (Book)](https://exploringjs.com/es6/)
- [Node.js ES Modules Guide](https://nodejs.org/api/esm.html)

---

*Last updated: ES2024 (ES15) — Check MDN for latest browser support*