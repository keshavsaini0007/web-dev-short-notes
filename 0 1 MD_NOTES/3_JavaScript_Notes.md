# JavaScript Notes - For B.Tech Placements

## Table of Contents
1. [Variables & Data Types](#variables--data-types)
2. [Operators](#operators)
3. [Control Flow](#control-flow)
4. [Loops](#loops)
5. [Functions](#functions)
6. [Arrays](#arrays)
7. [Objects](#objects)
8. [DOM Manipulation](#dom-manipulation)
9. [Classes & OOP](#classes--oop)
10. [Async JavaScript](#async-javascript)
11. [Error Handling](#error-handling)
12. [Interview Questions](#interview-questions)

---

## 1. Variables & Data Types

### Variables
```javascript
// let - block scoped, reassignable
let name = "John";
name = "Jane";

// const - block scoped, not reassignable
const PI = 3.14159;

// var - function scoped (avoid in modern JS)
var old = "legacy";
```

**Interview Point**: Use `const` by default, `let` when needed, avoid `var`.

### Data Types
```javascript
// Primitive Types
let str = "Hello";        // String
let num = 42;             // Number
let bool = true;          // Boolean
let empty = null;         // Null
let notDefined;           // Undefined
let sym = Symbol("id");  // Symbol
let big = 123n;           // BigInt

// Reference Types
let arr = [1, 2, 3];      // Array
let obj = {key: "value"}; // Object
let func = function(){};  // Function

// Type Checking
typeof "hello"  // "string"
typeof 42       // "number"
typeof true     // "boolean"
typeof []       // "object" (careful!)
typeof {}       // "object"
```

### Type Coercion
```javascript
// Implicit
"5" + 2        // "52" (string)
"5" - 2        // 3 (number)
"5" * 2        // 10 (number)
Boolean(1)     // true

// Explicit
Number("5")    // 5
String(5)      // "5"
Boolean("")    // false
```

---

## 2. Operators

### Comparison
```javascript
// == vs ===
"5" == 5    // true (loose)
"5" === 5   // false (strict)

// Other comparisons
> < >= <= !== 

// Nullish
??          // Nullish coalescing (only null/undefined)
?.          // Optional chaining
```

### Spread & Rest
```javascript
// Spread - expand
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];

// Rest - collect
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
```

---

## 3. Control Flow

```javascript
// If-else
if (condition) {
    // code
} else if (condition2) {
    // code
} else {
    // code
}

// Ternary
const status = age >= 18 ? "adult" : "minor";

// Switch
switch (day) {
    case 1:
        console.log("Monday");
        break;
    case 2:
        console.log("Tuesday");
        break;
    default:
        console.log("Other day");
}
```

---

## 4. Loops

```javascript
// For
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// While
while (condition) {
    // code
}

// Do-while (runs at least once)
do {
    // code
} while (condition);

// For-of (arrays, strings)
for (const item of array) {
    console.log(item);
}

// For-in (objects)
for (const key in object) {
    console.log(key, object[key]);
}

// forEach, map, filter, reduce
arr.forEach((item, index) => {});
arr.map(item => item * 2);
arr.filter(item => item > 5);
arr.reduce((acc, item) => acc + item, 0);
```

---

## 5. Functions

```javascript
// Function Declaration (hoisted)
function greet(name) {
    return `Hello, ${name}`;
}

// Function Expression
const greet = function(name) {
    return `Hello, ${name}`;
};

// Arrow Function
const greet = (name) => `Hello, ${name}`;

// Default Parameters
function greet(name = "Guest") {
    return `Hello, ${name}`;
}

// Arrow vs Regular
// - Arrow doesn't have 'this'
// - Arrow can't be used as object method
// - Arrow not hoisted
```

---

## 6. Arrays

### Methods
```javascript
const arr = [1, 2, 3, 4, 5];

// Add/Remove
arr.push(6);         // End
arr.pop();           // End
arr.unshift(0);      // Start
arr.shift();         // Start
arr.splice(2, 1);    // Remove at index
arr.splice(2, 0, 9);// Insert at index

// Search
arr.indexOf(3);      // 2
arr.includes(3);     // true
arr.find(x => x > 3);// First match
arr.findIndex(x => x > 3);

// Transform
arr.map(x => x * 2);
arr.filter(x => x > 2);
arr.reduce((acc, x) => acc + x, 0);

// Other
arr.slice(1, 3);     // Copy
arr.concat([6, 7]); // Merge
arr.reverse();
arr.sort();
arr.join("-");
```

### Destructuring
```javascript
const [a, b, ...rest] = [1, 2, 3, 4, 5];
// a=1, b=2, rest=[3,4,5]

const [first, , third] = arr;  // Skip elements
```

---

## 7. Objects

```javascript
const person = {
    name: "John",
    age: 30,
    city: "NYC",
    greet() {
        return `Hi, I'm ${this.name}`;
    }
};

// Access
person.name;        // "John"
person["name"];     // "John"

// Destructuring
const { name, age } = person;
const { name: n, age: a } = person;  // Rename

// Spread
const updated = { ...person, age: 31 };

// Methods
Object.keys(person);   // ["name", "age", "city", "greet"]
Object.values(person); // ["John", 30, "NYC", f]
Object.entries(person); // [["name","John"], ...]
```

---

## 8. DOM Manipulation

### Select
```javascript
document.getElementById("id");
document.getElementsByClassName("class");
document.getElementsByTagName("tag");
document.querySelector(".class");
document.querySelectorAll(".class");
```

### Create/Modify
```javascript
// Create
const div = document.createElement("div");
div.textContent = "Hello";
div.innerHTML = "<span>World</span>";
div.className = "container";
div.setAttribute("data-id", "1");

// Add to DOM
parent.appendChild(div);
parent.insertBefore(new, existing);
parent.innerHTML = "";  // Clear

// Remove
element.remove();
parent.removeChild(child);
```

### Events
```javascript
// Add Event
element.addEventListener("click", function(e) {
    console.log(e.target);
});

// Arrow function version
element.addEventListener("click", (e) => {
    console.log(e.target);
});

// Common Events
click, dblclick, mouseover, mouseout
keydown, keyup, keypress
submit, focus, blur
change, input
load, DOMContentLoaded
```

### Event Object Properties
```javascript
e.target        // Element that triggered
e.currentTarget // Element with listener
e.preventDefault()  // Stop default
e.stopPropagation() // Stop bubbling
```

---

## 9. Classes & OOP

```javascript
class Person {
    // Constructor
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Method
    greet() {
        return `Hi, I'm ${this.name}`;
    }
    
    // Static method
    static species() {
        return "Homo Sapiens";
    }
    
    // Getter
    get isAdult() {
        return this.age >= 18;
    }
}

// Inheritance
class Student extends Person {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }
    
    // Override
    greet() {
        return `Hi, I'm ${this.name} and I'm in grade ${this.grade}`;
    }
}

// Usage
const john = new Person("John", 25);
john.greet();           // "Hi, I'm John"
john.isAdult;          // true
Person.species();      // "Homo Sapiens"

const student = new Student("Jane", 15, 10);
```

---

## 10. Async JavaScript

### Callbacks
```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback("Data received");
    }, 1000);
}

fetchData(data => console.log(data));
```

### Promises
```javascript
const promise = new Promise((resolve, reject) => {
    // Async operation
    const success = true;
    if (success) resolve("Data");
    else reject("Error");
});

promise
    .then(data => console.log(data))
    .catch(err => console.error(err))
    .finally(() => console.log("Done"));
```

### Async/Await
```javascript
function fetchData() {
    return new Promise((resolve) => {
        setTimeout(() => resolve("Data"), 1000);
    });
}

async function getData() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

### Fetch API
```javascript
// GET
fetch("https://api.example.com/data")
    .then(res => res.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));

// POST
fetch("https://api.example.com/data", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ name: "John" })
})
    .then(res => res.json())
    .then(data => console.log(data));
```

### Async/Await with Fetch
```javascript
async function getData() {
    try {
        const res = await fetch("https://api.example.com/data");
        const data = await res.json();
        return data;
    } catch (err) {
        console.error(err);
    }
}
```

---

## 11. Error Handling

```javascript
try {
    // Risky code
    const result = JSON.parse("invalid");
} catch (error) {
    console.error(error.message);
} finally {
    // Always runs
    console.log("Done");
}

// Custom Error
throw new Error("Custom error message");
```

---

## 12. Important Interview Questions

### Q1: Difference between == and ===?
- `==` compares value only (type coercion)
- `===` compares value AND type (strict)

### Q2: What is hoisting?
- Function declarations are hoisted entirely
- `var` declarations are hoisted (initialized as undefined)
- `let` and `const` are hoisted but not initialized (Temporal Dead Zone)

### Q3: What is closure?
```javascript
function outer() {
    let count = 0;
    return function inner() {
        count++;
        return count;
    };
}
const counter = outer();
counter(); // 1
counter(); // 2
```
- Inner function accessing outer function's variables
- Interview asked frequently!

### Q4: What is the difference between null and undefined?
- `undefined`: Variable declared but not assigned
- `null`: Intentionally assigned empty value
- `typeof null` returns "object" (JS bug!)

### Q5: What is event bubbling?
- Events propagate from target up to root
- `e.stopPropagation()` stops it
- `e.preventDefault()` stops default behavior

### Q6: What is debouncing vs throttling?
- **Debounce**: Wait until user stops typing (e.g., search)
- **Throttle**: Limit function calls (e.g., scroll)

### Q7: What is the difference between map and forEach?
- `map`: Returns new array
- `forEach`: Returns undefined

### Q8: What is the spread operator used for?
- Copying arrays/objects: `[...arr]`, `{...obj}`
- Function arguments: `fn(...args)`
- Destructuring: `const [a, ...rest] = arr`

### Q9: What is the purpose of async/await?
- Cleaner syntax than Promises
- Makes async code look synchronous

### Q10: What is the difference between var, let, and const?
| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes | TDZ | TDZ |
| Reassign | Yes | Yes | No |
| Global | Yes | No | No |

### Q11: What is callback hell?
- Nested callbacks making code unreadable
- Solved with Promises or async/await

### Q12: What is a Promise?
- Object representing eventual completion of async operation
- States: pending, fulfilled, rejected

### Q13: What is the purpose of 'this'?
- Refers to current execution context
- Arrow functions don't have their own `this`

### Q14: What is event loop?
- Continuously checks if call stack is empty
- Moves callback queue to stack when empty

### Q15: What is DOM?
- Document Object Model - tree representation of HTML

### Q16: What is the difference between localStorage and sessionStorage?
- localStorage: Persists until manually cleared
- sessionStorage: Cleared when tab closes

### Q17: How does event delegation work?
- Single event listener on parent
- Handles events from children via event bubbling
- Efficient for dynamic elements

### Q18: What are template literals?
```javascript
const name = "John";
const msg = `Hello, ${name}`;  // Backticks
```

### Q19: What is destructuring?
```javascript
const [a, b] = [1, 2];
const { name, age } = person;
```

### Q20: What is a pure function?
- Same input = same output
- No side effects

---

## Quick JavaScript Cheat Sheet

```javascript
// Arrow function
const add = (a, b) => a + b;

// Ternary
const status = age >= 18 ? "Adult" : "Minor";

// Optional chaining
const city = user?.address?.city;

// Nullish
const name = user.name ?? "Guest";

// Spread
const newArr = [...arr1, ...arr2];

// Object shorthand
const name = "John", age = 30;
const person = { name, age };  // {name: "John", age: 30}

// Array find
arr.find(x => x.id === 1);

// Array filter
arr.filter(x => x.active);

// Array some/every
arr.some(x => x > 10);
arr.every(x => x > 0);
```

---
**End of JavaScript Notes**