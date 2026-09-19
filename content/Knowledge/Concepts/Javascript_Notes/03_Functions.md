 # Functions

## Overview
Functions are reusable blocks of code designed to perform specific tasks. They take inputs (parameters), perform actions, and return outputs. Functions are first-class objects in JavaScript — they can be assigned to variables, passed as arguments, and returned from other functions.

## Key Concepts

### Function Declaration vs. Expression

**Function Declaration** - hoisted, can be called before definition
```javascript
function greet(name) {
    return "Hello, " + name + "!";
}
console.log(greet("Alice"));  // "Hello, Alice!"
```

**Function Expression** - not hoisted, created when the assignment is reached
```javascript
const greet = function(name) {
    return "Hello, " + name + "!";
};
console.log(greet("Bob"));  // "Hello, Bob!"
```

### Arrow Functions (ES6)
Shorter syntax, no own `this` binding, no `arguments` object, implicit return for single expressions
```javascript
// Single parameter, single expression — implicit return
const square = n => n * n;

// Multiple parameters
const add = (a, b) => a + b;

// Multiple statements require {} and explicit return
const sum = (a, b) => {
    const result = a + b;
    return result;
};

// No `this` binding — inherits from surrounding scope
const obj = {
    name: "GFG",
    arrowFn: () => console.log(this.name),  // undefined (global this)
    regularFn: function() { console.log(this.name); }  // "GFG"
};
```

### Parameters and Arguments

**Default Parameters**
```javascript
function greet(name = "Guest") {
    return "Hello, " + name;
}
greet();          // "Hello, Guest"
greet("Aman");    // "Hello, Aman"
```

**Rest Parameters (`...`)** - collects remaining arguments into an array
```javascript
function sum(...nums) {
    return nums.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4));  // 10
```

**Spread Operator (`...`)** - expands iterable into individual elements
```javascript
const nums = [1, 2, 3];
console.log(Math.max(...nums));  // 3

const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];    // [1, 2, 3, 4]
```

**Arguments Object** (only in regular functions)
```javascript
function showAll() {
    console.log(arguments);  // array-like object of all args
}
showAll(1, "hello", true);
```

### Scope and Closures

**Scope** determines where variables are accessible:
- **Global scope** — accessible everywhere
- **Function scope** — accessible inside the function (`var`)
- **Block scope** — accessible inside `{ }` (`let`, `const`)

```javascript
let global = "global";

function outer() {
    let outerVar = "outer";
    
    function inner() {
        let innerVar = "inner";
        console.log(global);   // "global"
        console.log(outerVar); // "outer"
        console.log(innerVar); // "inner"
    }
    
    inner();
}
```

**Closure** — a function that "remembers" its lexical scope even when executed outside that scope
      A function defined inside of another function, 
     the inner function has access to the variables 
     and scope of the outer function. 
     Allow for private variables and state maintenance 
      Used frequently in JS frameworks: React, Vue, Angular
```javascript
function createCounter() {
    let count = 0;  // closed-over variable
    
    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter());  // 1
console.log(counter());  // 2
console.log(counter());  // 3
// count is not accessible from outside — data privacy


// --------- EXAMPLE 1 --------- 
function outer(){ 
const message = "Hello"; 
function inner(){ 
console.log(message); 
 } inner(); }
message = "Goodbye"; 
outer(); 
// --------- EXAMPLE 2 ---------
 function createCounter() { 
 let count = 0; 
 function increment() {
  count++; 
  console.log(`Count increased to ${count}`); 
  } 
  function getCount() {
   return count; 
   } 
   return {increment, getCount}; 
   } 
   
   const counter = createCounter(); 
   counter.increment(); 
   counter.increment(); 
   counter.increment(); 
   console.log(`Current count: ${counter.getCount()}`); 
 // --------- EXAMPLE 3 --------- 
 function createGame(){
  let score = 0; 
  function increaseScore(points){
   score += points; 
   console.log(`+${points}pts`); 
   } 
  function decreaseScore(points){ 
  score -= points; 
  console.log(`-${points}pts`); 
  } 
  function getScore(){
   return score;
    } 
    return {increaseScore, decreaseScore, getScore}; } 
    const game = createGame(); 
    game.increaseScore(5); 
    game.increaseScore(6); 
    game.decreaseScore(3); 
    console.log(`The final score is ${game.getScore()}pts`)
```

**Practical use cases for closures:**
- Data encapsulation / private variables
- Function factories
- Event handlers maintaining state
- Module pattern

### Higher-Order Functions
Functions that take other functions as arguments or return functions

**Built-in array methods:**
```javascript
const numbers = [1, 2, 3, 4, 5];

// map — transform each element
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10]

// filter — keep elements that pass test
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4]

// reduce — accumulate values
const total = numbers.reduce((acc, n) => acc + n, 0);
// 15

// forEach — execute for each element
numbers.forEach(n => console.log(n));
```

**Custom higher-order function:**
```javascript
function applyOperation(a, b, operation) {
    return operation(a, b);
}

const result = applyOperation(5, 3, (x, y) => x * y);
console.log(result);  // 15
```

**Function returning a function:**
```javascript
function multiplyBy(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = multiplyBy(2);
const triple = multiplyBy(3);

console.log(double(5));  // 10
console.log(triple(5));  // 15
```

### IIFE (Immediately Invoked Function Expression)
Function that runs as soon as it is defined — creates an isolated scope
```javascript
(function() {
    const privateVar = "secret";
    console.log("Runs immediately!");
})();
// privateVar is not accessible outside
```

### Callback Functions
Functions passed as arguments to be executed later
```javascript
function fetchData(callback) {
    // simulate async operation
    setTimeout(() => {
        callback("Data received");
    }, 1000);
}

fetchData(message => {
    console.log(message);  // "Data received" (after 1s)
});
```

## Summary
Functions are the building blocks of JavaScript. They can be declared or expressed, written as arrow functions, accept parameters (default, rest, spread), create closures that remember their lexical scope, and serve as higher-order functions that enable functional programming patterns like `map`, `filter`, and `reduce`.

## Resources
- [Functions in JavaScript - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/functions-in-javascript/)
- [Arrow Functions - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/arrow-functions-in-javascript/)
- [Closures - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/closure-in-javascript/)
- [Higher-Order Functions - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-higher-order-functions/)
- [Scope - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-scope/)
