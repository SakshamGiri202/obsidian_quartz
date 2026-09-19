# Control Flow

## Overview
Control flow statements determine the order in which code executes. They allow decision-making, looping, branching, and error handling based on conditions.

## Key Concepts

### Conditional Statements

**if statement** - executes block only if condition is true
```javascript
const age = 18;
if (age >= 18) {
    console.log("You are an adult.");
}
```

**if...else** - provides alternate block when condition is false
```javascript
const score = 40;
if (score >= 50) {
    console.log("You passed.");
} else {
    console.log("You failed.");
}
```

**if...else if...else** - handles multiple conditions
```javascript
const temp = 25;
if (temp > 30) {
    console.log("It's hot.");
} else if (temp >= 20) {
    console.log("It's warm.");
} else {
    console.log("It's cold.");
}
```

**Ternary Operator** - shorthand for if...else
```javascript
const status = age >= 18 ? "Adult" : "Minor";
```

**switch statement** - matches expression against multiple cases
```javascript
const day = "Monday";
switch (day) {
    case "Monday":
        console.log("Start of the week.");
        break;
    case "Friday":
        console.log("End of the workweek.");
        break;
    default:
        console.log("It's a regular day.");
}
// Without break, execution falls through to the next case
```

### Loops

**for loop** - when iteration count is known
```javascript
for (let i = 1; i <= 5; i++) {
    console.log(i);  // 1, 2, 3, 4, 5
}
```

**while loop** - runs while condition is true (checked before each iteration)
```javascript
let i = 1;
while (i <= 5) {
    console.log(i);
    i++;
}
```

**do...while loop** - runs at least once (checked after each iteration)
```javascript
let i = 1;
do {
    console.log(i);
    i++;
} while (i <= 5);
```

**for...of loop** - iterates over iterable values (arrays, strings, etc.)
```javascript
const arr = [10, 20, 30];
for (const val of arr) {
    console.log(val);  // 10, 20, 30
}
```

**for...in loop** - iterates over enumerable property keys (objects)
```javascript
const obj = { a: 1, b: 2, c: 3 };
for (const key in obj) {
    console.log(key, obj[key]);  // a 1, b 2, c 3
}
```

**Loop Control:**
- `break` - exits the loop entirely
- `continue` - skips to the next iteration

```javascript
for (let i = 0; i < 10; i++) {
    if (i === 3) continue;  // skip 3
    if (i === 7) break;    // stop at 7
    console.log(i);         // 0, 1, 2, 4, 5, 6
}
```

### Error Handling

**try...catch...finally** - handles runtime errors gracefully
```javascript
try {
    let result = riskyOperation();
    console.log(result);
} catch (error) {
    console.error("An error occurred:", error.message);
} finally {
    console.log("This always runs (cleanup).");
}
```

**throw** - creates custom errors
```javascript
function divide(a, b) {
    if (b === 0) {
        throw new Error("Division by zero is not allowed.");
    }
    return a / b;
}

try {
    divide(10, 0);
} catch (err) {
    console.log(err.message);  // Division by zero is not allowed.
}
```

**Error Types:**
- `Error` - generic error
- `SyntaxError` - invalid syntax
- `TypeError` - wrong type used
- `ReferenceError` - accessing undefined variable
- `RangeError` - value out of range

## Summary
Control flow in JavaScript includes conditionals (`if`, `switch`, ternary), loops (`for`, `while`, `do...while`, `for...of`, `for...in`), and error handling (`try...catch...finally` with `throw`). These constructs enable decision-making, repetition, and robust error management.

## Resources
- [Control Flow Statements - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-control-flow-statements/)
- [Error Handling - GeeksforGeeks](https://www.geeksforgeeks.org/javascript/javascript-error-and-exceptional-handling-with-examples/)
