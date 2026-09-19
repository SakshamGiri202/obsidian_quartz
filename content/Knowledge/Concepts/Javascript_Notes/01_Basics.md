# Basics

## Overview
1. Introduction to JavaScript
2. Variables (let, const, var)
3. Data Types (String, Number, Boolean, Null, Undefined, Symbol, BigInt)
4. Operators (Arithmetic, Assignment, Comparison, Logical)
5. Basic Input/Output (console.log, prompt, alert)

## Key Concepts
### Introduction to JavaScript
- Lightweight, cross-platform, single-threaded, dynamically-typed programming language
- Brings life to web pages by making them interactive
- **Interpreted language**: code is compiled and executed line by line
- **Dynamically typed**: variable types are determined at runtime
- **Single-threaded**: executes one task at a time (but supports asynchronous operations via event loop)
- **Client-side**: works with HTML (structure) and CSS (style) to add interactivity
- **Server-side**: Node.js enables file handling, database access, and backend logic

### Variables (let, const, var)
| Keyword | Scope           | Hoisted                          | Reassignable | Redeclarable |
| ------- | --------------- | -------------------------------- | ------------ | ------------ |
| `var`   | Function-scoped | Yes (initialized as `undefined`) | Yes          | Yes          |
| `let`   | Block-scoped    | Yes (Temporal Dead Zone)         | Yes          | No           |
| `const` | Block-scoped    | Yes (Temporal Dead Zone)         | No           | No           |

```javascript
var a = 10;       // Old way, function-scoped
let b = 20;       // Modern, block-scoped, can reassign
const c = 30;     // Block-scoped, cannot reassign

// const objects/arrays can still be mutated
const obj = { x: 1 };
obj.x = 2;        // Allowed
```

### Data Types

**Primitive (7 types):**

| Type            | Example                     | Description                                |
| --------------- | --------------------------- | ------------------------------------------ |
| Number          | `let n = 42`                | Integers and floats (both are Number type) |
| String          | `let s = "hello"`           | Series of characters in quotes             |
| Boolean         | `let b = true`              | `true` or `false`                          |
| Null            | `let x = null`              | Intentional absence of value               |
| Undefined       | `let y`                     | Declared but not initialized               |
| Symbol (ES6)    | `let s = Symbol("id")`      | Unique, immutable identifier               |
| BigInt (ES2020) | `let b = 9007199254740991n` | Numbers beyond 2^53                        |


**Non-Primitive (Reference types):**
- Object: key-value pairs `{ name: "John" }`
- Array: ordered collection `[1, 2, 3]`
- Function: reusable block of code
- Date: date/time handling
- RegExp: pattern matching

```javascript
// Dynamic typing
let x = 42;        // number
x = "hello";       // string - no error

// typeof operator
console.log(typeof 42);       // "number"
console.log(typeof "hello");  // "string"
console.log(typeof null);     // "object" (historical bug)
console.log(typeof undefined);// "undefined"
```

### Operators
**Arithmetic:** `+`, `-`, `*`, `/`, `%`, `++`, `--`
**Assignment:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`
**Comparison:** `==`, `===` (strict), `!=`, `!==`, `>`, `<`, `>=`, `<=`
**Logical:** `&&` (AND), `||` (OR), `!` (NOT)
**Ternary:** `condition ? exprIfTrue : exprIfFalse`
**Optional Chaining:** `?.` (safe nested access)
**Nullish Coalescing:** `??` (returns RHS if LHS is null/undefined)

```javascript
// Strict vs loose equality
console.log(5 == "5");   // true (type coercion)
console.log(5 === "5");  // false (no coercion)

// Optional chaining
const user = { address: { city: "NYC" } };
console.log(user.address?.city);     // "NYC"
console.log(user.contact?.phone);    // undefined (no error)
```

### Basic Input/Output
```javascript
console.log("Hello World");       // Output to console
console.error("Error message");   // Error output
console.table([1, 2, 3]);         // Table format

// Browser-only (not in Node.js)
// alert("Hello!");               // Popup dialog
// let name = prompt("Name?");    // User input
// let confirm = confirm("OK?");  // Confirm dialog
```

## Summary
JavaScript is a versatile, dynamically-typed language that powers both client-side and server-side web development. It uses `let`/`const` for modern variable declaration, has 7 primitive data types plus objects, supports a full range of operators, and uses `console` methods for debugging output.

## Resources
- [JavaScript Tutorial - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-tutorial/)
- [JavaScript Variables - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-variables/)
- [JavaScript Data Types - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-data-types/)
- [JavaScript Operators - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-operators/)
