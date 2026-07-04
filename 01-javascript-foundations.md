# 📚 JavaScript Foundations — A Concept Reference

> **Purpose**: This document covers every core JavaScript concept you'll encounter while working through the 100 coding problems. Each section explains the concept, shows examples, highlights pitfalls, and includes a **Functional Perspective** showing how FP practitioners approach the same idea.
>
> **Cross-references**: Problem numbers from [02-javascript-coding-problems.md](./02-javascript-coding-problems.md) are marked as **JS #N**. References to the FP guide use **→ FP Guide §Section**.

---

## Table of Contents

1. [Variables, Scoping & Hoisting](#1-variables-scoping--hoisting)
2. [Data Types & Type Coercion](#2-data-types--type-coercion)
3. [Operators](#3-operators)
4. [Functions](#4-functions)
5. [Control Flow](#5-control-flow)
6. [Strings](#6-strings)
7. [Arrays](#7-arrays)
8. [Objects](#8-objects)
9. [Closures & Scope Chains](#9-closures--scope-chains)
10. [Error Handling](#10-error-handling)
11. [Regular Expressions](#11-regular-expressions)
12. [Classes & Prototypal Inheritance](#12-classes--prototypal-inheritance)
13. [Asynchronous JavaScript](#13-asynchronous-javascript)
14. [ES6+ Features](#14-es6-features)
15. [DOM & Browser APIs](#15-dom--browser-apis)
16. [Data Structures in JavaScript](#16-data-structures-in-javascript)
17. [Generators & Iterators](#17-generators--iterators)
18. [Proxy & Reflect](#18-proxy--reflect)
19. [Metaprogramming & Advanced Patterns](#19-metaprogramming--advanced-patterns)

---

## 1. Variables, Scoping & Hoisting

**Used in**: JS #1, #2, #3, #11

### Three Ways to Declare Variables

> **📦 Mental Model: The Boxes and Labels**
> - `var`: An old, leaky cardboard box. Things might spill over to places they shouldn't (function scope).
> - `let`: A sturdy, modern plastic bin. It stays exactly where you put it (block scope), but you can swap out what's inside.
> - `const`: A locked safe. Once you put something in, you can't replace the item itself (though if it's an object, you can still alter its contents).

```js
var name = "Alice";    // function-scoped, hoisted, can be redeclared
let age = 25;          // block-scoped, hoisted (TDZ), cannot be redeclared
const PI = 3.14159;    // block-scoped, hoisted (TDZ), cannot be reassigned
```

### Scope Rules

```js
function scopeDemo() {
  var x = 1;       // visible throughout the entire function
  if (true) {
    var x = 2;     // SAME variable — var ignores block boundaries
    let y = 3;     // only visible inside this if-block
    const z = 4;   // only visible inside this if-block
  }
  console.log(x);  // 2 — var was overwritten
  // console.log(y); // ReferenceError — y is not defined here
}
```

### Hoisting

> **🛗 Mental Model: The Elevator**
> Imagine your code is a building. When JS compiles it, it takes all variable and function *declarations* and sends them up the elevator to the top floor of their scope. However, for variables, it only sends the *names* (the labels) up. The actual *values* (the assignments) stay on their original floor.

JavaScript "hoists" declarations to the top of their scope, but behaves differently for each keyword:

```js
console.log(a);    // undefined — var is hoisted, but the assignment is not
var a = 10;

console.log(b);    // ReferenceError — let is hoisted but in the "Temporal Dead Zone"
let b = 20;

console.log(c);    // ReferenceError — same TDZ behavior
const c = 30;
```

**The Temporal Dead Zone (TDZ)**: The period between entering a scope and the actual `let`/`const` declaration. The variable exists but cannot be accessed.

### Best Practices

| Rule | Reason |
|:-----|:-------|
| Default to `const` | Prevents accidental reassignment; signals intent |
| Use `let` only when reassignment is needed | Minimizes mutable state |
| Avoid `var` entirely | Block-scoping bugs are too easy with `var` |

> **🔀 Functional Perspective**: FP strongly prefers `const` for everything. If you need a "changed" value, create a new binding rather than reassigning. This aligns with the immutability principle. → FP Guide §Immutability

---

## 2. Data Types & Type Coercion

**Used in**: JS #4, #11

### Primitive Types (7 total)

```js
typeof 42;            // "number"   — all numbers are 64-bit floats
typeof "hello";       // "string"
typeof true;          // "boolean"
typeof undefined;     // "undefined"
typeof null;          // "object"   — this is a historic bug in JS!
typeof Symbol("id");  // "symbol"
typeof 9007199254740991n; // "bigint"
```

### Reference Types

```js
typeof {};            // "object"
typeof [];            // "object"   — arrays are objects!
typeof function(){};  // "function" — functions are objects too
```

To properly check for arrays: `Array.isArray([1,2,3]) // true`

### Type Coercion — The Full Picture

JavaScript auto-converts types in certain contexts. This is one of the most error-prone parts of the language.

```js
// String concatenation wins with +
"5" + 3         // "53"     — number is coerced to string
"5" - 3         // 2        — string is coerced to number (- is only numeric)
"5" * "2"       // 10       — both coerced to numbers

// Boolean coercion
true + true     // 2        — true is 1, false is 0
false + "1"     // "false1" — boolean coerced to string (+ with string)

// null and undefined
null + 1        // 1        — null becomes 0
undefined + 1   // NaN      — undefined becomes NaN

// Empty string
"" + 0          // "0"      — 0 coerced to string
```

### Truthy & Falsy Values

**Falsy values** (only 8 — memorize these):
```js
false, 0, -0, 0n, "", null, undefined, NaN
```

**Everything else is truthy**, including:
```js
"0";        // truthy! (non-empty string)
[];         // truthy! (empty array is an object)
{};         // truthy! (empty object)
"false"     // truthy! (non-empty string)
```

### Equality: `==` vs `===`

```js
5 == "5"      // true  — coerces types before comparing
5 === "5"     // false — no coercion, checks type AND value
null == undefined  // true  — special case in the spec
null === undefined // false
```

**Rule**: Always use `===` unless you specifically intend type coercion.

> **🔀 Functional Perspective**: Pure functions need predictable behavior. Type coercion introduces hidden behavior that makes functions harder to reason about. FP code uses explicit conversions: `Number("5")` instead of relying on `"5" - 0`. → FP Guide §Pure Functions

---

## 3. Operators

**Used in**: JS #5, #6, #8, #9, #10, #13, #18

### Arithmetic

```text
+   // addition / string concatenation / unary plus (converts to number)
-   // subtraction / unary negation
*   // multiplication
/   // division (always returns float: 7 / 2 === 3.5)
%   // remainder (modulo): 7 % 3 === 1
**  // exponentiation: 2 ** 3 === 8
```

### Remainder vs. Modulo

```js
-7 % 3    // -1 in JavaScript (remainder keeps the sign of the dividend)
// True modulo: ((-7 % 3) + 3) % 3 === 2
```

### Short-Circuit Evaluation

```js
// && returns the first falsy value, or the last value
0 && "hello"        // 0
"hello" && "world"  // "world"

// || returns the first truthy value, or the last value
0 || "default"      // "default"
"hello" || "world"  // "hello"

// ?? (nullish coalescing) — only checks null/undefined
0 ?? "default"      // 0          (0 is not null/undefined)
null ?? "default"   // "default"
```

### Optional Chaining

```js
const user = { address: { city: "NYC" } };
user?.address?.city     // "NYC"
user?.phone?.number     // undefined (no error thrown)
user?.getName?.()       // undefined (safely call a method that might not exist)
```

> **🔀 Functional Perspective**: Short-circuit operators enable expression-based logic (returning values rather than using if-statements), which aligns with FP's preference for expressions over statements. The `??` operator is JS's lightweight version of the `getOrElse` pattern from the `Maybe` monad. → FP Guide §Maybe Monad

---

## 4. Functions

**Used in**: JS #4–10, #12–20, #21–40 (nearly every problem!)

### Four Ways to Define Functions

```js
// 1. Function Declaration — hoisted, has its own `this`
function greet(name) {
  return `Hello, ${name}!`;
}

// 2. Function Expression — NOT hoisted
const greet = function(name) {
  return `Hello, ${name}!`;
};

// 3. Arrow Function — concise, lexical `this`, no `arguments`
const greet = (name) => `Hello, ${name}!`;

// 4. Method Shorthand (inside objects)
const obj = {
  greet(name) { return `Hello, ${name}!`; }
};
```

### Arrow Functions — Key Differences

```js
// Implicit return (single expression, no braces)
const double = x => x * 2;

// Explicit return (multiple statements require braces + return)
const doubleExplicit = x => {
  const result = x * 2;
  return result;
};

// No own `this` — inherits from enclosing scope
const timer = {
  seconds: 0,
  start() {
    setInterval(() => {
      this.seconds++;  // `this` refers to timer, not the callback
    }, 1000);
  }
};
```

### Default Parameters

```js
function greet(name = "World") {
  return `Hello, ${name}!`;
}
greet();        // "Hello, World!"
greet("Alice"); // "Hello, Alice!"
```

### Rest Parameters & Spread

```js
// Rest: collect remaining arguments into an array
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4);  // 10

// Spread: expand an array into individual arguments
const nums = [1, 2, 3];
Math.max(...nums);  // 3
```

### The `arguments` Object (legacy)

```js
function legacy() {
  console.log(arguments);     // { 0: "a", 1: "b", length: 2 }
  console.log(arguments[0]);  // "a"
}
legacy("a", "b");
// Note: arguments is NOT a real array — use Array.from(arguments) or rest params instead
// Arrow functions do NOT have their own arguments object
```

### Function `.length` and `.name`

```js
function add(a, b, c) {}
add.length;  // 3 — the number of declared parameters (before default/rest)
add.name;    // "add"

const fn = (a, b = 0) => {};
fn.length;   // 1 — default params don't count
```

> **🔀 Functional Perspective**: Arrow functions are the preferred style in FP JavaScript — they're concise, avoid `this` ambiguity, and encourage small, single-purpose functions. FP treats functions as the primary building block: they're created, passed, returned, and composed constantly. → FP Guide §Higher-Order Functions

---

## 5. Control Flow

**Used in**: JS #8, #10, #12, #14, #16, #21, #32

### if / else if / else

```js
function classify(score) {
  if (score >= 90) return "A";
  else if (score >= 80) return "B";
  else if (score >= 70) return "C";
  else return "F";
}
```

### Ternary Operator (Expression-Based)

```js
// Single condition
const label = age >= 18 ? "adult" : "minor";

// Nested (use sparingly — readability suffers)
const grade = score >= 90 ? "A" : score >= 80 ? "B" : "F";
```

### switch

```js
function dayType(day) {
  switch (day) {
    case "Saturday":
    case "Sunday":
      return "Weekend";
    default:
      return "Weekday";
  }
}
```

### for / while / do...while

```js
// Classic for
for (let i = 0; i < 5; i++) { /* ... */ }

// for...of (iterates over values — arrays, strings, Maps, Sets)
for (const item of [1, 2, 3]) { /* ... */ }

// for...in (iterates over keys — mainly for objects)
for (const key in { a: 1, b: 2 }) { /* ... */ }

// while
let n = 5;
while (n > 0) { n--; }

// do...while (always runs at least once)
do { n++; } while (n < 5);
```

### Labeled Statements & Break/Continue

```js
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer;  // breaks BOTH loops
  }
}
```

> **🔀 Functional Perspective**: FP avoids imperative loops entirely. Every `for` or `while` can be replaced with `map`, `filter`, `reduce`, or recursion. The key insight: loops are *statements* (they don't return values), while FP operations are *expressions* (they always return something). → FP Guide §Declarative Style
>
> ```js
// Imperative: for loop
const doubled = [];
for (let i = 0; i < nums.length; i++) {
  doubled.push(nums[i] * 2);
}
// Functional: map expression
const doubled = nums.map(n => n * 2);
```

---

## 6. Strings

**Used in**: JS #7, #15, #17, #22, #24, #27, #29, #33, #35, #65

### String Basics

Strings are **immutable** in JavaScript — every string method returns a new string.

```js
const str = "Hello, World!";
str[0]             // "H" (bracket notation)
str.charAt(0)      // "H"
str.length         // 13
```

### Essential String Methods

```js
// Searching
str.indexOf("World")      // 7
str.includes("World")     // true
str.startsWith("Hello")   // true
str.endsWith("!")         // true

// Extracting
str.slice(7, 12)          // "World"
str.slice(-6)             // "orld!"
str.substring(7, 12)      // "World"

// Transforming
str.toUpperCase()         // "HELLO, WORLD!"
str.toLowerCase()         // "hello, world!"
str.trim()                // removes whitespace from both ends
str.padStart(20, "-")     // "-------Hello, World!"
str.repeat(2)             // "Hello, World!Hello, World!"

// Splitting & Joining
str.split(", ")           // ["Hello", "World!"]
["a", "b", "c"].join("-") // "a-b-c"

// Replacing
str.replace("World", "JS")     // "Hello, JS!" (first occurrence)
str.replaceAll("l", "L")       // "HeLLo, WorLd!"
```

### Template Literals

```js
const name = "Alice";
const age = 25;

// String interpolation
`My name is ${name} and I am ${age} years old.`

// Multi-line strings
const html = `
  <div>
    <h1>${name}</h1>
    <p>Age: ${age}</p>
  </div>
`;

// Tagged templates (advanced)
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => 
    result + str + (values[i] ? `**${values[i]}**` : ""), "");
}
highlight`Hello ${name}, you are ${age}`;
// "Hello **Alice**, you are **25**"
```

### Character Codes

```js
"A".charCodeAt(0)         // 65
String.fromCharCode(65)   // "A"

// Useful for cipher problems (JS #33):
const shifted = String.fromCharCode(
  ((char.charCodeAt(0) - 65 + shift) % 26) + 65
);
```

### Iterating Over Strings

```js
// Spread into array (handles Unicode correctly)
;[..."hello"]              // ["h", "e", "l", "l", "o"]
;[..."🎉👍"]              // ["🎉", "👍"] — correct!

// for...of (also handles Unicode)
for (const char of "hello") { /* ... */ }
```

> **🔀 Functional Perspective**: Because JS strings are already immutable, they're naturally FP-friendly. Prefer `split` + array methods + `join` over character-by-character loops. Chain transformations: `str.toLowerCase().split(" ").map(capitalize).join(" ")`. → FP Guide §Declarative Style

---

## 7. Arrays

**Used in**: JS #25–28, #31, #34, #36–38, #40, #42–45 + many more

Arrays are the most important data structure in JavaScript and the foundation of functional programming in the language.

### Creating Arrays

```js
const a = [1, 2, 3];                        // literal
const b = new Array(3);                      // [empty × 3] — careful!
const c = Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
const d = Array.of(3);                       // [3] — unlike new Array(3)
const e = [...Array(5).keys()];              // [0, 1, 2, 3, 4]
```

### Destructuring

```js
const [first, second, ...rest] = [1, 2, 3, 4, 5];
// first = 1, second = 2, rest = [3, 4, 5]

// Swap variables (JS #3)
let a = 5, b = 10;
[a, b] = [b, a];  // a = 10, b = 5

// Skip elements
const [, , third] = [1, 2, 3];  // third = 3

// Default values
const [x = 0, y = 0] = [1];  // x = 1, y = 0
```

### Mutating Methods (⚠️ avoid in FP)

These methods change the original array:

```js
const arr = [1, 2, 3];
arr.push(4);        // [1, 2, 3, 4] — adds to end, returns new length
arr.pop();          // [1, 2, 3]    — removes from end, returns removed element
arr.unshift(0);     // [0, 1, 2, 3] — adds to front
arr.shift();        // [1, 2, 3]    — removes from front
arr.splice(1, 1);   // [1, 3]       — removes/inserts at index
arr.sort();         // sorts IN PLACE
arr.reverse();      // reverses IN PLACE
arr.fill(0);        // fills IN PLACE
```

### Non-Mutating Methods (✅ FP-friendly)

These return new arrays/values without modifying the original:

```js
const arr = [1, 2, 3, 4, 5];

// Transformation
arr.map(x => x * 2)              // [2, 4, 6, 8, 10]
arr.filter(x => x > 3)           // [4, 5]
arr.reduce((sum, x) => sum + x, 0) // 15
arr.flatMap(x => [x, x * 10])    // [1, 10, 2, 20, 3, 30, 4, 40, 5, 50]

// Searching
arr.find(x => x > 3)             // 4 (first match)
arr.findIndex(x => x > 3)        // 3 (index of first match)
arr.indexOf(3)                    // 2
arr.includes(3)                   // true
arr.some(x => x > 4)             // true (at least one passes)
arr.every(x => x > 0)            // true (all pass)

// Extraction
arr.slice(1, 3)                   // [2, 3] (start inclusive, end exclusive)
arr.concat([6, 7])                // [1, 2, 3, 4, 5, 6, 7]
arr.flat()                        // flattens one level
arr.flat(Infinity)                // flattens all levels

// Iteration (no return value)
arr.forEach(x => console.log(x)) // side effect only — avoid in FP!

// New in ES2023 — non-mutating versions of mutating methods
arr.toSorted()                    // returns sorted copy
arr.toReversed()                  // returns reversed copy
arr.toSpliced(1, 1)               // returns copy with splice applied
arr.with(2, 99)                   // returns copy with index 2 set to 99
```

### The Big Three: `map`, `filter`, `reduce`

These three methods are the workhorses of functional JavaScript. Understanding them deeply is essential.

#### `map` — Transform Every Element

```js
// Signature: array.map(callback(element, index, array))
// Returns: new array of same length

[1, 2, 3].map(x => x * 2)         // [2, 4, 6]
["a", "b"].map((el, i) => el + i)  // ["a0", "b1"]

// Common pattern: extract a property
users.map(user => user.name)       // ["Alice", "Bob"]
```

#### `filter` — Select Elements That Pass a Test

```js
// Signature: array.filter(callback(element, index, array))
// Returns: new array (possibly shorter)

[1, 2, 3, 4, 5].filter(x => x > 3)   // [4, 5]
["", "hello", "", "world"].filter(Boolean)  // ["hello", "world"]

// Remove duplicates (with indexOf)
arr.filter((item, i) => arr.indexOf(item) === i)
```

#### `reduce` — Collapse to a Single Value

```js
// Signature: array.reduce(callback(accumulator, current, index, array), initialValue)
// Returns: single value (any type)

// Sum
[1, 2, 3].reduce((sum, n) => sum + n, 0)  // 6

// Count occurrences
["a", "b", "a"].reduce((counts, item) => {
  return { ...counts, [item]: (counts[item] || 0) + 1 };
}, {});
// { a: 2, b: 1 }

// Flatten one level
[[1, 2], [3, 4]].reduce((flat, arr) => flat.concat(arr), [])
// [1, 2, 3, 4]

// Build any structure — reduce is the most powerful array method
```

> **🔀 Functional Perspective**: `map`, `filter`, and `reduce` ARE functional programming. They replace loops with declarative, composable operations. Master these three and you have 80% of FP in JavaScript. → FP Guide §The FP Trinity

---

## 8. Objects

**Used in**: JS #14, #16, #20, #41, #45, #51–52, #54, #73, #77

### Object Basics

```js
// Object literal
const user = {
  name: "Alice",
  age: 25,
  "has-dog": true,     // quoted keys for special characters
  greet() {            // method shorthand
    return `Hi, I'm ${this.name}`;
  }
};

// Access
user.name              // "Alice"
user["has-dog"]        // true (bracket notation for special keys)

// Computed property names
const key = "email";
const obj = { [key]: "alice@example.com" };  // { email: "alice@example.com" }
```

### Object Destructuring

```js
const { name, age } = user;           // name = "Alice", age = 25
const { name: userName } = user;       // rename: userName = "Alice"
const { phone = "N/A" } = user;       // default value
const { address: { city } } = user;    // nested destructuring
```

### Spread Operator with Objects

```js
// Shallow clone
const copy = { ...user };

// Merge objects (later properties win)
const merged = { ...defaults, ...overrides };

// Immutable update (crucial for FP!)
const updated = { ...user, age: 26 };
// { name: "Alice", age: 26 } — original user is unchanged
```

### Object Methods

```js
Object.keys(user)       // ["name", "age", "has-dog", "greet"]
Object.values(user)     // ["Alice", 25, true, [Function]]
Object.entries(user)    // [["name", "Alice"], ["age", 25], ...]
Object.fromEntries([["a", 1], ["b", 2]])  // { a: 1, b: 2 }

Object.assign({}, user, { age: 30 })      // shallow merge/clone
Object.freeze(user)     // prevents modification (shallow)
Object.isFrozen(user)   // true

Object.hasOwn(user, "name")  // true (modern replacement for hasOwnProperty)
```

### Property Descriptors

```js
Object.defineProperty(obj, "readOnly", {
  value: 42,
  writable: false,      // cannot be changed
  enumerable: true,     // shows up in for...in
  configurable: false   // cannot be deleted or reconfigured
});
```

> **🔀 Functional Perspective**: Objects in FP are treated as immutable data records. Instead of mutating properties, create new objects with the spread operator. Object.entries + map/filter + Object.fromEntries is the FP way to transform objects. → FP Guide §Immutability
>
> ```js
// FP-style object transformation
const upperKeys = Object.fromEntries(
  Object.entries(obj).map(([k, v]) => [k.toUpperCase(), v])
);
```

---

## 9. Closures & Scope Chains

**Used in**: JS #46, #47, #48, #49, #50

A **closure** is a function that remembers the variables from the scope where it was created, even after that scope has exited.

### How Closures Work

> **🎒 Mental Model: The Backpack**
> When a function is created, it's given a "backpack" containing all the variables from the scope where it was born. Wherever that function travels in the code, it always carries its backpack. Even if the surrounding code has finished executing, the function can still reach into its backpack and use those variables.

```js
function createCounter() {
  let count = 0;              // enclosed variable
  return {
    increment: () => ++count, // closure over count
    getCount: () => count     // closure over count
  };
}

const counter = createCounter();
counter.increment();  // 1
counter.increment();  // 2
counter.getCount();   // 2
// `count` is not accessible from outside — it's truly private
```

### The Classic Loop Problem

```js
// Bug: all callbacks share the same `i`
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Prints: 3, 3, 3  (not 0, 1, 2!)

// Fix 1: use `let` (block-scoped, creates new binding per iteration)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Prints: 0, 1, 2 ✅

// Fix 2: IIFE (pre-ES6 pattern)
for (var i = 0; i < 3; i++) {
  ((j) => setTimeout(() => console.log(j), 100))(i);
}
```

### Closures for Encapsulation

```js
function createWallet(initialBalance) {
  let balance = initialBalance;
  return {
    deposit: (amount) => { balance += amount; return balance; },
    withdraw: (amount) => {
      if (amount > balance) throw new Error("Insufficient funds");
      balance -= amount;
      return balance;
    },
    getBalance: () => balance
  };
}
```

### Closure Patterns Used in FP

```js
// Memoization (JS #48)
function memoize(fn) {
  const cache = new Map();     // enclosed in the closure
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

// Debounce (JS #46)
function debounce(fn, delay) {
  let timer;                    // enclosed in the closure
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

> **🔀 Functional Perspective**: Closures are the mechanism that makes higher-order functions, currying, and partial application possible. In FP, closures replace mutable state: instead of a mutable variable, you close over data and return functions that know how to use it. → FP Guide §Closures in FP

---

## 10. Error Handling

**Used in**: JS #57, #62, #83

### try / catch / finally

```js
try {
  const result = riskyOperation();
  console.log(result);
} catch (error) {
  console.error("Something went wrong:", error.message);
} finally {
  cleanup();  // always runs, even if an error occurred
}
```

### Error Types

```js
new Error("Generic error")
new TypeError("Expected a number")
new RangeError("Index out of bounds")
new ReferenceError("x is not defined")
new SyntaxError("Unexpected token")
```

### Custom Errors

```js
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

throw new ValidationError("email", "Invalid email format");
```

### Error Handling in Async Code

```js
// With Promises
fetch(url)
  .then(response => response.json())
  .catch(error => console.error(error));

// With async/await
async function fetchData() {
  try {
    const response = await fetch(url);
    return await response.json();
  } catch (error) {
    console.error("Fetch failed:", error);
    return null;
  }
}
```

> **🔀 Functional Perspective**: FP avoids `throw` and `try/catch` in favor of explicit error values. The `Either` type (also called `Result`) wraps success as `Right(value)` and failure as `Left(error)`, allowing errors to flow through a pipeline without exceptions. → FP Guide §Either Monad

---

## 11. Regular Expressions

**Used in**: JS #35, #58, #61, #91

### Syntax

```js
const regex = /pattern/g;            // literal syntax
const regex = new RegExp("pattern", "flags"); // constructor syntax
```

### Common Flags

| Flag | Meaning |
|:-----|:--------|
| `g` | Global — find all matches |
| `i` | Case-insensitive |
| `m` | Multiline — `^` and `$` match line boundaries |
| `s` | Dotall — `.` matches `\n` |

### Character Classes

```text
\d     // digit [0-9]
\w     // word character [a-zA-Z0-9_]
\s     // whitespace [ \t\n\r\f]
\D     // NOT a digit
.      // any character except newline
```

### Quantifiers

```text
*      // 0 or more
+      // 1 or more
?      // 0 or 1
{3}    // exactly 3
{2,5}  // 2 to 5
{3,}   // 3 or more
```

### Methods

```js
/hello/.test("say hello")           // true
"say hello".match(/hello/)          // ["hello", index: 4, ...]
"hello world".replace(/hello/, "hi") // "hi world"
"a,b,,c".split(/,+/)               // ["a", "b", "c"]
```

### Practical Examples

```js
// Email validation (simplified)
/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test("user@example.com")  // true

// Extract query parameters
"?name=Alice&age=25".match(/[?&](\w+)=([^&]*)/g)

// Split on multiple spaces
"hello   world".split(/\s+/)  // ["hello", "world"]
```

> **🔀 Functional Perspective**: Regex matching is a pure operation — same input always gives the same output (assuming no `g` flag state issues). Use `match` and `replace` inside `map`/`filter` pipelines for declarative text processing.

---

## 12. Classes & Prototypal Inheritance

**Used in**: JS #50, #59, #60, #63, #80, #82, #89, #90, #94, #95

### ES6 Class Syntax

```js
class Animal {
  #sound;                        // private field (ES2022)

  constructor(name, sound) {
    this.name = name;
    this.#sound = sound;
  }

  speak() {
    return `${this.name} says ${this.#sound}`;
  }

  static create(name, sound) {    // static method
    return new Animal(name, sound);
  }
}
```

### Inheritance

```js
class Dog extends Animal {
  constructor(name) {
    super(name, "Woof");          // must call super() first
    this.tricks = [];
  }

  learnTrick(trick) {
    this.tricks.push(trick);
  }

  speak() {                       // override parent method
    return `${super.speak()}! ${super.speak()}!`;
  }
}
```

### Prototypal Inheritance (Under the Hood)

```js
// Classes are syntactic sugar over prototypes
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  return `${this.name} makes a sound`;
};

// Object.create — set up prototype chain directly
const dog = Object.create(Animal.prototype);
dog.name = "Rex";
dog.speak();  // "Rex makes a sound"

// Prototype chain
dog.__proto__ === Dog.prototype          // true
Dog.prototype.__proto__ === Animal.prototype  // true
```

### Getters and Setters

```js
class Circle {
  constructor(radius) {
    this.radius = radius;
  }

  get area() {
    return Math.PI * this.radius ** 2;
  }

  set diameter(d) {
    this.radius = d / 2;
  }
}

const c = new Circle(5);
c.area;         // 78.54 (accessed like a property, not a method)
c.diameter = 20;
c.radius;       // 10
```

> **🔀 Functional Perspective**: FP avoids classes and `this` in favor of plain objects + functions. Where OOP says "create a Dog class with a speak method," FP says "create a `speak(animal)` function that works on any object with a `name` property." This makes code more composable and testable. → FP Guide §FP in Practice
>
> ```js
// OOP: method on an object
class Stack { push(item) { /* mutates this.items */ } }
// FP: function that returns a new stack
const push = (stack, item) => [...stack, item];
```

---

## 13. Asynchronous JavaScript

**Used in**: JS #57, #62, #68, #69, #79, #86, #87, #97

### The Event Loop

> **🍳 Mental Model: The Restaurant Kitchen**
> - **Call Stack**: The head chef cooking one dish at a time.
> - **Web APIs**: The kitchen appliances (ovens, timers). The chef delegates tasks to them.
> - **Callback Queue**: The line of tickets for new orders waiting to be cooked.
> - **Microtask Queue**: VIP tickets (Promises). The chef checks these immediately after finishing the current dish, before grabbing a standard ticket.

JavaScript is single-threaded. Async operations use the event loop:

1. **Call Stack**: Executes synchronous code
2. **Web APIs**: Handle async operations (setTimeout, fetch, etc.)
3. **Callback Queue (Macrotasks)**: setTimeout, setInterval, I/O
4. **Microtask Queue**: Promise callbacks, queueMicrotask
5. Microtasks always run before macrotasks

```js
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Output: 1, 4, 3, 2
// (synchronous first, then microtask, then macrotask)
```

### Callbacks (The Old Way)

```js
function fetchData(callback) {
  setTimeout(() => {
    callback(null, { data: "hello" });
  }, 1000);
}

// "Callback hell" — deeply nested callbacks
fetchData((err, data1) => {
  fetchData((err, data2) => {
    fetchData((err, data3) => {
      // ...nightmare to read and maintain
    });
  });
});
```

### Promises

> **📟 Mental Model: The Restaurant Buzzer**
> When you order food at a busy restaurant, they give you a buzzer. The buzzer is a "Promise" that you'll eventually get food. While you wait (Pending), you can do other things. When it buzzes, you either get your food (Resolved/Fulfilled) or they tell you they ran out of ingredients (Rejected).

```js
// Creating a Promise
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) resolve("Data loaded");
    else reject(new Error("Failed to load"));
  }, 1000);
});

// Consuming a Promise
promise
  .then(data => console.log(data))      // "Data loaded"
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));

// Chaining
fetch("/api/user")
  .then(res => res.json())
  .then(user => fetch(`/api/posts/${user.id}`))
  .then(res => res.json())
  .then(posts => console.log(posts));
```

### Promise Combinators

```js
// All must succeed
Promise.all([p1, p2, p3])
  .then(([r1, r2, r3]) => { /* all results */ });

// All settle (no short-circuit)
Promise.allSettled([p1, p2, p3])
  .then(results => results.map(r => r.status)); // ["fulfilled", "rejected", ...]

// First to settle
Promise.race([p1, p2])
  .then(firstResult => { /* winner */ });

// First to succeed
Promise.any([p1, p2])
  .then(firstSuccess => { /* first non-rejection */ });
```

### async / await

```js
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) throw new Error(`HTTP ${response.status}`);
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Failed:", error);
    return null;
  }
}

// Parallel async operations
async function loadDashboard() {
  const [user, posts, notifications] = await Promise.all([
    fetchUser(1),
    fetchPosts(1),
    fetchNotifications(1)
  ]);
}
```

### AbortController (for cancelation)

```js
const controller = new AbortController();

fetch(url, { signal: controller.signal })
  .then(res => res.json())
  .catch(err => {
    if (err.name === "AbortError") console.log("Cancelled");
  });

// Cancel the request
controller.abort();
```

> **🔀 Functional Perspective**: Promises are actually monadic — `.then()` is essentially `flatMap`. You can chain async operations in a pipeline, and errors propagate automatically (like the `Either` monad). FP treats async operations as values that represent future computations, making them composable. → FP Guide §Monads

---

## 14. ES6+ Features

**Used in**: JS #3, #55, #70, #72, #76, #88

### Destructuring (Deep Dive)

```js
// Array destructuring with rest
const [first, ...rest] = [1, 2, 3, 4];  // first=1, rest=[2,3,4]

// Object destructuring with renaming and defaults
const { name: userName = "Anonymous", age = 0 } = user;

// In function parameters
function greet({ name, greeting = "Hello" }) {
  return `${greeting}, ${name}!`;
}
greet({ name: "Alice" });  // "Hello, Alice!"

// Nested destructuring
const { address: { city, zip } } = user;
```

### Symbol

```js
const id = Symbol("id");
const user = { [id]: 123, name: "Alice" };

user[id]          // 123
Object.keys(user) // ["name"] — symbols are not enumerable

// Well-known symbols
Symbol.iterator   // makes objects iterable
Symbol.toPrimitive // controls type coercion
```

### Map and Set

```js
// Map — key-value pairs with any type of key
const map = new Map();
map.set("name", "Alice");
map.set(42, "a number key");
map.set({}, "an object key");
map.get("name")   // "Alice"
map.has(42)       // true
map.size          // 3

// Set — unique values
const set = new Set([1, 2, 2, 3, 3]);
set.size          // 3
set.has(2)        // true
;[...set]          // [1, 2, 3]

// WeakMap / WeakSet — keys must be objects, allow garbage collection
const weakMap = new WeakMap();
```

### Optional Chaining & Nullish Coalescing

```js
// Safe property access (returns undefined instead of throwing)
user?.address?.city

// Combined with nullish coalescing
const city = user?.address?.city ?? "Unknown";

// Safe method calls
user?.getName?.()

// Safe array access
arr?.[0]
```

### Modules

```js
// Named exports
export const PI = 3.14;
export function double(x) { return x * 2; }

// Default export
export default class Calculator { /* ... */ }

// Importing
import Calculator, { PI, double } from "./math.js";
import * as math from "./math.js";

// Dynamic import (code splitting)
const module = await import("./heavy-module.js");
```

> **🔀 Functional Perspective**: ES6+ features are a goldmine for FP. Destructuring enables pattern-matching-like code, spread makes immutability ergonomic, `Map`/`Set` provide efficient immutable-friendly collections, and arrow functions make lambdas concise. → FP Guide §Immutability

---

## 15. DOM & Browser APIs

**Used in**: JS #66, #67, #75, #78, #84, #85

### Selecting Elements

```js
document.getElementById("app")           // single element by ID
document.querySelector(".card")           // first match (CSS selector)
document.querySelectorAll(".card")        // all matches (NodeList)
document.getElementsByClassName("card")   // HTMLCollection (live)
```

### Creating & Modifying Elements

```js
const div = document.createElement("div");
div.textContent = "Hello";
div.innerHTML = "<strong>Hello</strong>";  // parses HTML — XSS risk!
div.classList.add("active");
div.classList.remove("active");
div.classList.toggle("active");
div.setAttribute("data-id", "42");
div.style.color = "red";

document.body.appendChild(div);
parent.insertBefore(newNode, referenceNode);
element.remove();
```

### Event Handling

```js
// addEventListener (preferred)
button.addEventListener("click", (event) => {
  event.preventDefault();   // prevent default behavior
  event.stopPropagation();  // stop event bubbling
  console.log(event.target);
});

// Event Delegation — handle events on a parent
list.addEventListener("click", (event) => {
  if (event.target.matches("li")) {
    console.log("Clicked:", event.target.textContent);
  }
});
```

### IntersectionObserver (Lazy Loading)

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;  // load the actual image
      observer.unobserve(img);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll("img[data-src]").forEach(img => {
  observer.observe(img);
});
```

### Web Workers

```js
// main.js
const worker = new Worker("worker.js");
worker.postMessage({ type: "COMPUTE", data: bigArray });
worker.onmessage = (event) => console.log("Result:", event.data);

// worker.js
self.onmessage = (event) => {
  const result = heavyComputation(event.data);
  self.postMessage(result);
};
```

> **🔀 Functional Perspective**: DOM manipulation is inherently imperative and side-effectful. FP frameworks (React, Elm) solve this with virtual DOMs and declarative rendering: you describe WHAT the UI should look like, and the framework figures out HOW to update the DOM. When doing vanilla DOM work, isolate side effects into a thin rendering layer and keep your business logic pure. → FP Guide §FP in Practice

---

## 16. Data Structures in JavaScript

**Used in**: JS #56, #59, #60, #63, #89, #90, #95

### Stack (LIFO)

```js
class Stack {
  #items = [];
  push(item) { this.#items.push(item); }
  pop() { return this.#items.pop(); }
  peek() { return this.#items[this.#items.length - 1]; }
  size() { return this.#items.length; }
  isEmpty() { return this.#items.length === 0; }
}
```

### Queue (FIFO)

```js
class Queue {
  #items = [];
  enqueue(item) { this.#items.push(item); }
  dequeue() { return this.#items.shift(); }
  peek() { return this.#items[0]; }
  size() { return this.#items.length; }
}
```

### Linked List

```js
class Node {
  constructor(value, next = null) {
    this.value = value;
    this.next = next;
  }
}

class LinkedList {
  constructor() { this.head = null; }

  append(value) {
    if (!this.head) { this.head = new Node(value); return; }
    let current = this.head;
    while (current.next) current = current.next;
    current.next = new Node(value);
  }
}
```

### Hash Map (using Map)

```js
// JavaScript's Map is already a hash map
const cache = new Map();
cache.set("key", "value");
cache.get("key");  // "value"
cache.has("key");  // true

// LRU Cache trick: Map preserves insertion order
// Delete and re-insert to "move to end"
cache.delete("key");
cache.set("key", "value");
```

### Binary Search Tree

```js
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
// insert, search, delete, traversals → see JS #89
```

### Graph (Adjacency List)

```js
class Graph {
  constructor() {
    this.adjacencyList = new Map();
  }
  addVertex(v) { this.adjacencyList.set(v, []); }
  addEdge(v1, v2) {
    this.adjacencyList.get(v1).push(v2);
    this.adjacencyList.get(v2).push(v1);
  }
}
// BFS, DFS, shortest path → see JS #90
```

> **🔀 Functional Perspective**: FP uses persistent (immutable) data structures that share structure between versions. An FP linked list is just `{ value, next }` where `cons(newValue, oldList)` creates a new list that shares the tail with the old one — O(1) prepend with full history preservation. → FP Guide §Persistent Data Structures

---

## 17. Generators & Iterators

**Used in**: JS #70, #87, #97

### The Iterator Protocol

Any object with a `next()` method that returns `{ value, done }` is an iterator:

```js
const myIterator = {
  current: 0,
  next() {
    if (this.current < 3) {
      return { value: this.current++, done: false };
    }
    return { value: undefined, done: true };
  }
};
```

### The Iterable Protocol

An object is iterable if it has a `[Symbol.iterator]` method returning an iterator:

```js
const range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next() {
        return current <= last
          ? { value: current++, done: false }
          : { done: true };
      }
    };
  }
};

for (const n of range) console.log(n);  // 1, 2, 3, 4, 5
[...range]  // [1, 2, 3, 4, 5]
```

### Generator Functions

Generators simplify creating iterators:

```js
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;  // pauses execution and returns the value
  }
}

const gen = range(1, 5);
gen.next();  // { value: 1, done: false }
gen.next();  // { value: 2, done: false }
[...range(1, 5)]  // [1, 2, 3, 4, 5]
```

### Infinite Generators

```js
function* naturals() {
  let n = 1;
  while (true) yield n++;
}

// Take first 5
function* take(gen, n) {
  let count = 0;
  for (const val of gen) {
    if (count++ >= n) return;
    yield val;
  }
}

[...take(naturals(), 5)]  // [1, 2, 3, 4, 5]
```

### Async Generators

```js
async function* fetchPages() {
  let page = 1;
  while (true) {
    const response = await fetch(`/api/data?page=${page}`);
    const data = await response.json();
    if (data.length === 0) return;
    yield data;
    page++;
  }
}

for await (const page of fetchPages()) {
  console.log(page);
}
```

> **🔀 Functional Perspective**: Generators implement lazy evaluation — values are computed only when needed. This enables FP patterns like infinite sequences, lazy map/filter, and memory-efficient data processing pipelines. → FP Guide §Lazy Evaluation

---

## 18. Proxy & Reflect

**Used in**: JS #72, #88

### Proxy Basics

A `Proxy` wraps an object and intercepts operations on it:

```js
const handler = {
  get(target, property, receiver) {
    console.log(`Reading ${property}`);
    return Reflect.get(target, property, receiver);
  },
  set(target, property, value, receiver) {
    console.log(`Writing ${property} = ${value}`);
    return Reflect.set(target, property, value, receiver);
  }
};

const proxy = new Proxy({ name: "Alice" }, handler);
proxy.name;        // logs "Reading name", returns "Alice"
proxy.age = 25;    // logs "Writing age = 25"
```

### Available Traps

| Trap | Intercepts |
|:-----|:-----------|
| `get` | Property access |
| `set` | Property assignment |
| `has` | `in` operator |
| `deleteProperty` | `delete` operator |
| `apply` | Function calls |
| `construct` | `new` operator |
| `ownKeys` | `Object.keys()`, `for...in` |
| `getPrototypeOf` | `Object.getPrototypeOf()` |

### Practical Example: Validation

```js
const validated = new Proxy({}, {
  set(target, prop, value) {
    if (prop === "age" && (typeof value !== "number" || value < 0)) {
      throw new TypeError("Age must be a non-negative number");
    }
    target[prop] = value;
    return true;
  }
});

validated.age = 25;   // OK
validated.age = -5;   // TypeError!
```

### Reflect API

`Reflect` provides methods that mirror Proxy traps, making it easy to implement default behavior:

```js
Reflect.get(obj, "name")          // same as obj.name
Reflect.set(obj, "name", "Bob")   // same as obj.name = "Bob"
Reflect.has(obj, "name")          // same as "name" in obj
Reflect.ownKeys(obj)              // Object.keys + symbols
```

> **🔀 Functional Perspective**: Proxies enable reactive programming — automatically rerunning effects when data changes (like Vue's reactivity system, JS #88). This is the bridge between FP's immutable world and JS's mutable reality: the Proxy can enforce constraints or trigger reactions while the consumer code writes naturally. → FP Guide §Functional Reactive Programming

---

## 19. Metaprogramming & Advanced Patterns

**Used in**: JS #81, #86, #91, #92, #98, #99, #100

### Well-Known Symbols

```js
// Make an object iterable
class Range {
  constructor(from, to) { this.from = from; this.to = to; }
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    return {
      next: () => current <= last
        ? { value: current++, done: false }
        : { done: true }
    };
  }
}

// Custom type coercion
class Money {
  constructor(amount) { this.amount = amount; }
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.amount;
    if (hint === "string") return `$${this.amount}`;
    return this.amount;
  }
}
```

### Design Patterns in JavaScript

A brief overview — the problems will ask you to implement several of these:

| Pattern | Description | Problem |
|:--------|:------------|:--------|
| Observer/Pub-Sub | Objects subscribe to events | JS #50, #74 |
| State Machine | Objects transition between states | JS #80 |
| Iterator | Sequential access to elements | JS #70 |
| Strategy | Swap algorithms at runtime | JS #82 |
| Singleton | One instance of a class | — |
| Factory | Create objects without `new` | JS #82 |
| Decorator | Add behavior to objects dynamically | — |

### Parser Building Blocks

For JS #96 (JSON parser) and JS #98 (JS interpreter):

```js
// Tokenizer pattern
function tokenize(input) {
  const tokens = [];
  let i = 0;
  while (i < input.length) {
    // Skip whitespace
    if (/\s/.test(input[i])) { i++; continue; }
    // Match numbers
    if (/\d/.test(input[i])) {
      let num = "";
      while (i < input.length && /\d/.test(input[i])) num += input[i++];
      tokens.push({ type: "NUMBER", value: Number(num) });
      continue;
    }
    // ... more token types
  }
  return tokens;
}

// Recursive descent parser pattern
function parseExpression(tokens, pos) {
  let [left, pos2] = parseTerm(tokens, pos);
  while (pos2 < tokens.length && tokens[pos2].value === "+") {
    const [right, pos3] = parseTerm(tokens, pos2 + 1);
    left = { type: "Add", left, right };
    pos2 = pos3;
  }
  return [left, pos2];
}
```

> **🔀 Functional Perspective**: Parsers are a natural fit for FP. Recursive descent parsers are just mutually recursive pure functions. Each parser takes `(tokens, position)` and returns `[result, newPosition]` — no mutation needed. This is exactly the State monad pattern! → FP Guide §State Monad

---

## 📎 Quick Reference: Which Section for Which Problem?

| Problem Range | Primary Sections |
|:-------------|:-----------------|
| JS #1–10 | §1 Variables, §2 Data Types, §3 Operators, §4 Functions |
| JS #11–20 | §2 Data Types, §3 Operators, §5 Control Flow, §6 Strings |
| JS #21–30 | §5 Control Flow, §6 Strings, §7 Arrays |
| JS #31–40 | §6 Strings, §7 Arrays, §4 Functions |
| JS #41–55 | §7 Arrays, §8 Objects, §9 Closures, §4 Functions |
| JS #56–65 | §7 Arrays, §10 Errors, §11 Regex, §13 Async, §16 Data Structures |
| JS #66–78 | §15 DOM, §13 Async, §14 ES6+, §18 Proxy |
| JS #79–85 | §13 Async, §14 ES6+, §15 DOM, §18 Proxy |
| JS #86–100 | §12 Classes, §13 Async, §17 Generators, §19 Metaprogramming |

---

*This document is a companion to [02-javascript-coding-problems.md](./02-javascript-coding-problems.md). For functional programming concepts and techniques, see [03-functional-programming-guide.md](./03-functional-programming-guide.md). For detailed solutions with FP analysis, see [05-problem-solving-with-fp.md](./05-problem-solving-with-fp.md).*
