## Overview
Objects and arrays are the two primary non-primitive (reference) data structures in JavaScript. Objects store key-value pairs for structured data, while arrays store ordered, index-based collections of values.

## Key Concepts

### Object Literals and Properties
Objects are dynamic collections of key-value pairs. Keys are strings (or Symbols), values can be any type.

```javascript
// Object literal syntax (preferred)
const user = {
    name: "Alice",
    age: 25,
    "full name": "Alice Johnson",  // multi-word key needs quotes
    greet() {                       // method shorthand
        return "Hello, " + this.name;
    }
};

// Accessing properties
console.log(user.name);          // dot notation
console.log(user["full name"]);  // bracket notation (for dynamic/multi-word keys)

// Modifying
user.age = 26;

// Adding new properties
user.email = "alice@example.com";

// Deleting
delete user.email;

// Checking existence
console.log("name" in user);                    // true
console.log(user.hasOwnProperty("name"));       // true

// Getting keys/values/entries
console.log(Object.keys(user));     // ["name", "age", "full name"]
console.log(Object.values(user));   // ["Alice", 26, "Alice Johnson"]
console.log(Object.entries(user));  // [["name","Alice"], ["age",26], ...]
console.log(Object.keys(user).length);  // number of properties

// Merging objects
const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const merged = { ...obj1, ...obj2 };  // { a: 1, b: 3, c: 4 }

// Object.assign
const copy = Object.assign({}, obj1);
```

### Array Creation and Basics
Arrays are ordered, zero-indexed collections. They can hold mixed types and are dynamically resizable.

```javascript
// Creation
let arr = [1, 2, 3, 4, 5];           // literal (preferred)
let mixed = [1, "hello", true, { x: 1 }];
let empty = new Array(5);            // creates array of length 5 (empty slots)

// Accessing
console.log(arr[0]);        // first element
console.log(arr[arr.length - 1]);  // last element

// Adding/Removing
arr.push(6);                // add to end      → [1,2,3,4,5,6]
arr.pop();                  // remove from end → [1,2,3,4,5]
arr.unshift(0);             // add to front    → [0,1,2,3,4,5]
arr.shift();                // remove front    → [1,2,3,4,5]

// splice — insert/remove at any index
arr.splice(2, 1);           // remove 1 element at index 2
arr.splice(2, 0, 99);       // insert 99 at index 2 (remove 0)

// slice — extract portion (does NOT mutate)
const sub = arr.slice(1, 4);  // elements at index 1,2,3

// Concatenation
const combined = arr.concat([6, 7]);
const spread = [...arr, 6, 7]; // same result

// Check if array
console.log(Array.isArray(arr));    // true
console.log(arr instanceof Array);  // true
```

### Array Methods (map, filter, reduce, find, etc.)

**Iteration methods:**
```javascript
const nums = [1, 2, 3, 4, 5];

// forEach — execute side effect for each element
nums.forEach(n => console.log(n));

// map — transform each element into new array
const doubled = nums.map(n => n * 2);       // [2, 4, 6, 8, 10]

// filter — keep elements passing a test
const evens = nums.filter(n => n % 2 === 0); // [2, 4]

// reduce — accumulate to single value
const sum = nums.reduce((acc, n) => acc + n, 0);    // 15
const product = nums.reduce((acc, n) => acc * n, 1); // 120

// find — first element passing test (or undefined)
const firstEven = nums.find(n => n % 2 === 0);  // 2

// findIndex — index of first element passing test
const idx = nums.findIndex(n => n > 3);         // 3

// some — does any element pass?
const hasLarge = nums.some(n => n > 4);         // true

// every — do ALL elements pass?
const allPositive = nums.every(n => n > 0);     // true

// includes — check if value exists
console.log(nums.includes(3));                  // true
```

**Other useful methods:**
```javascript
// sort — mutates! sorts as strings by default
let arr = [3, 1, 20, 5];
arr.sort((a, b) => a - b);      // numeric ascending: [1, 3, 5, 20]

// reverse — mutates
arr.reverse();                  // [20, 5, 3, 1]

// join — string with separator
console.log(["a", "b", "c"].join("-"));  // "a-b-c"

// flat — flatten nested arrays
const nested = [1, [2, [3, 4]]];
console.log(nested.flat(2));    // [1, 2, 3, 4] (depth 2)
console.log(nested.flat(Infinity)); // flatten completely

// toString
console.log([1, 2, 3].toString());  // "1,2,3"
```

### Destructuring

**Array Destructuring:**
```javascript
const colors = ["red", "green", "blue", "yellow"];

// Basic
const [first, second] = colors;
console.log(first);   // "red"
console.log(second);  // "green"

// Skip elements
const [a, , c] = colors;
console.log(a, c);    // "red" "blue"

// Rest pattern
const [head, ...tail] = colors;
console.log(head);    // "red"
console.log(tail);    // ["green", "blue", "yellow"]

// Default values
const [x = 10, y = 20] = [5];
console.log(x, y);    // 5, 20

// Swapping variables
let p = 1, q = 2;
[p, q] = [q, p];      // p=2, q=1
```

**Object Destructuring:**
```javascript
const person = { name: "Bob", age: 30, city: "NYC" };

// Basic — variable names must match property names
const { name, age } = person;
console.log(name, age);     // "Bob" 30

// Renaming
const { name: userName, age: userAge } = person;
console.log(userName);      // "Bob"

// Default values
const { country = "Unknown" } = person;
console.log(country);       // "Unknown"

// Nested destructuring
const data = { user: { id: 1, name: "Alice" }, meta: { role: "admin" } };
const { user: { id, name: userName2 }, meta: { role } } = data;
console.log(id, userName2, role);  // 1 "Alice" "admin"

// Rest in objects
const { name: n, ...rest } = person;
console.log(n);             // "Bob"
console.log(rest);          // { age: 30, city: "NYC" }

// Function parameter destructuring
function printUser({ name, age }) {
    console.log(`${name} is ${age} years old`);
}
printUser(person);          // "Bob is 30 years old"
```

### Spread and Rest Operators (`...`)

**Spread — expands iterable into individual elements:**
```javascript
// Arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined2 = [...arr1, ...arr2];       // [1,2,3,4,5,6]
const copy2 = [...arr1];                     // shallow copy

// Objects
const objA = { a: 1, b: 2 };
const objB = { c: 3 };
const merged2 = { ...objA, ...objB };        // { a:1, b:2, c:3 }
const copy3 = { ...objA };                   // shallow copy

// Function calls
const numbers = [5, 10, 15];
console.log(Math.max(...numbers));           // 15

// String to array
console.log([..."hello"]);                   // ["h","e","l","l","o"]
```

**Rest — collects remaining arguments/properties into array/object:**
```javascript
// Function rest parameters
function sumAll(...nums) {
    return nums.reduce((a, b) => a + b, 0);
}
sumAll(1, 2, 3, 4);  // 10

// Array destructuring rest
const [first2, ...rest2] = [10, 20, 30, 40];
console.log(first2);  // 10
console.log(rest2);   // [20, 30, 40]

// Object destructuring rest
const { x: xVal, ...rest3 } = { x: 1, y: 2, z: 3 };
console.log(xVal);    // 1
console.log(rest3);   // { y: 2, z: 3 }
```

### Common Object Patterns

```javascript
// Shorthand property names
const x = 10, y = 20;
const point = { x, y };       // { x: 10, y: 20 }

// Computed property keys
const key = "dynamicKey";
const obj = { [key]: "value" };
console.log(obj.dynamicKey);  // "value"

// Method shorthand
const calculator = {
    add(a, b) { return a + b; },
    subtract(a, b) { return a - b; }
};

// Object iteration
const student = { name: "Sam", grade: "A", subject: "Math" };
for (const key in student) {
    console.log(key, student[key]);  // name Sam, grade A, subject Math
}

// Object.entries + forEach
Object.entries(student).forEach(([key, value]) => {
    console.log(`${key}: ${value}`);
});

// Freeze vs Seal
const frozen = Object.freeze({ a: 1 });   // cannot add/delete/modify
const sealed = Object.seal({ b: 2 });     // can modify, cannot add/delete
```

### Array vs Object — When to Use

| Use Case                       | Structure    |
| ------------------------------ | ------------ |
| Ordered collection of items    | Array        |
| Keyed data / named fields      | Object       |
| Unique values                  | Set          |
| Key-value with any key type    | Map          |
| Need iteration order guarantee | Array or Map |

## Summary
Objects store structured data as key-value pairs with dot/bracket access, dynamic property manipulation, and methods like `Object.keys()`/`Object.values()`. Arrays store ordered, index-based collections with powerful methods (`map`, `filter`, `reduce`, `find`, `push`, `pop`, `splice`, `slice`). Destructuring unpacks both objects and arrays concisely. The spread (`...`) operator expands iterables, while rest collects remaining values — both are essential for clean, modern JavaScript.

## Resources
- [Objects in JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/objects-in-javascript/)
- [JavaScript Arrays - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-arrays/)
- [JavaScript Array Methods - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-array-methods/)
- [Destructuring Assignment - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-destructuring-assignment/)
