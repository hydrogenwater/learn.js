# 🧠 Functional Programming in JavaScript — A Complete Guide

> **Purpose**: A comprehensive tutorial on functional programming (FP) in JavaScript, progressing from first principles to advanced theory. Each section includes clear explanations, runnable code examples, and links to the practice problems where you'll apply the concepts.
>
> **Cross-references**: Problems from [04-functional-programming-problems.md](./04-functional-programming-problems.md) are marked **FP #N**. Problems from [02-javascript-coding-problems.md](./02-javascript-coding-problems.md) are marked **JS #N**. For detailed problem solutions, see [05-problem-solving-with-fp.md](./05-problem-solving-with-fp.md).

---

## Table of Contents

### Part I — Foundations
1. [What Is Functional Programming?](#1-what-is-functional-programming)
2. [Pure Functions & Referential Transparency](#2-pure-functions--referential-transparency)
3. [Immutability](#3-immutability)
4. [First-Class & Higher-Order Functions](#4-first-class--higher-order-functions)
5. [Declarative vs. Imperative Style](#5-declarative-vs-imperative-style)

### Part II — Core Techniques
6. [map, filter, reduce — The FP Trinity](#6-map-filter-reduce--the-fp-trinity)
7. [Function Composition](#7-function-composition)
8. [Currying & Partial Application](#8-currying--partial-application)
9. [Closures in FP](#9-closures-in-fp)
10. [Recursion](#10-recursion)

### Part III — Advanced Patterns
11. [Functors](#11-functors)
12. [Monads: Maybe & Either](#12-monads-maybe--either)
13. [Lenses & Optics](#13-lenses--optics)
14. [Transducers](#14-transducers)
15. [Lazy Evaluation & Generators](#15-lazy-evaluation--generators)
16. [Trampolines & Stack Safety](#16-trampolines--stack-safety)
17. [Algebraic Data Types & Pattern Matching](#17-algebraic-data-types--pattern-matching)
18. [Persistent Data Structures](#18-persistent-data-structures)

### Part IV — FP in Practice
19. [State Management with Reducers](#19-state-management-with-reducers)
20. [Functional Reactive Programming](#20-functional-reactive-programming)
21. [Error Handling Without Exceptions](#21-error-handling-without-exceptions)
22. [FP Design Patterns](#22-fp-design-patterns)
23. [When NOT to Use FP](#23-when-not-to-use-fp)

---

# Part I — Foundations

---

## 1. What Is Functional Programming?

Functional programming is a **programming paradigm** where programs are built by composing **pure functions** and avoiding **shared mutable state** and **side effects**.

### Three Paradigms Compared

| Aspect | Imperative | Object-Oriented | Functional |
|:-------|:-----------|:----------------|:-----------|
| **Primary unit** | Statement | Object | Function |
| **State** | Mutable variables | Encapsulated in objects | Immutable values |
| **Control flow** | Loops, conditions | Method dispatch | Recursion, composition |
| **Key idea** | "Do this, then this" | "Objects send messages" | "Compute this value" |
| **Side effects** | Everywhere | Managed by objects | Isolated and controlled |

### The Five Pillars of FP

```
┌─────────────────────────────────────────────────────────────┐
│                   FUNCTIONAL PROGRAMMING                     │
├──────────┬──────────┬──────────┬──────────┬─────────────────┤
│  Pure    │ Immuta-  │ Higher-  │ Function │ Declarative     │
│ Functions│  bility  │  Order   │ Compo-   │ Style           │
│          │          │ Functions│  sition  │                 │
└──────────┴──────────┴──────────┴──────────┴─────────────────┘
```

### Why FP in JavaScript?

JavaScript is a multi-paradigm language that supports FP natively:
- Functions are **first-class values** (can be passed, returned, stored)
- Built-in FP methods: `map`, `filter`, `reduce`, `flatMap`
- Closures and lexical scoping
- Arrow functions for concise lambdas
- Spread/rest operators for immutable operations

JavaScript is NOT a "pure" FP language like Haskell — it allows mutation, side effects, and imperative code. The art is knowing when and how to apply FP principles pragmatically.

**Practice**: FP #1, #2

---

## 2. Pure Functions & Referential Transparency

> **🎰 Mental Model: The Vending Machine**
> A pure function is like a vending machine. If you put in $1 and press 'A1', you get a Snickers bar. You will *always* get a Snickers bar for that input. Furthermore, the vending machine doesn't change the weather outside, or steal your wallet — it has no side effects.

A **pure function** has two properties:
1. **Deterministic**: Given the same inputs, it always returns the same output
2. **No side effects**: It doesn't change anything outside itself

### What Makes a Function Pure?

```js
// ✅ PURE — depends only on its inputs, returns a value
const add = (a, b) => a + b;

// ✅ PURE — same input always gives the same output
const toUpperCase = (str) => str.toUpperCase();

// ✅ PURE — creates and returns a new object
const withAge = (person, age) => ({ ...person, age });
```

### What Makes a Function Impure?

```js
// ❌ IMPURE — reads external mutable state
let taxRate = 0.2;
const calculateTax = (price) => price * taxRate;  // result depends on taxRate

// ❌ IMPURE — writes to external state (side effect)
let total = 0;
const addToTotal = (n) => { total += n; };

// ❌ IMPURE — console.log is a side effect (I/O)
const greet = (name) => {
  console.log(`Hello, ${name}`);  // side effect!
  return `Hello, ${name}`;
};

// ❌ IMPURE — depends on current time (non-deterministic)
const getGreeting = () => {
  const hour = new Date().getHours();
  return hour < 12 ? "Good morning" : "Good evening";
};

// ❌ IMPURE — mutates its input
const sortArray = (arr) => {
  arr.sort();     // mutates the original array!
  return arr;
};
```

### The Side Effects Catalog

| Side Effect | Example | Why It's Impure |
|:-----------|:--------|:----------------|
| Console output | `console.log(x)` | I/O — observable effect |
| DOM manipulation | `element.textContent = x` | Mutates external state |
| HTTP requests | `fetch(url)` | I/O — network interaction |
| Database writes | `db.save(record)` | I/O — external state |
| Modifying arguments | `arr.push(x)` | Mutates input |
| Reading global state | `window.innerWidth` | Non-deterministic |
| Random values | `Math.random()` | Non-deterministic |
| Current time | `Date.now()` | Non-deterministic |
| Throwing exceptions | `throw new Error()` | Disrupts control flow |

### How to Make Impure Code Pure

**Strategy: Move the impurity to the edges of your program.**

```js
// ❌ Impure: fetches data AND processes it
async function getActiveUsers() {
  const response = await fetch("/api/users");  // impure: I/O
  const users = await response.json();
  return users.filter(u => u.active);          // pure part
}

// ✅ Better: separate the pure logic from the impure shell
const filterActive = (users) => users.filter(u => u.active);  // pure

async function getActiveUsers() {
  const response = await fetch("/api/users");  // impure — isolated at the edge
  const users = await response.json();
  return filterActive(users);                  // pure function called
}
```

### Referential Transparency

An expression is **referentially transparent** if it can be replaced by its value without changing the program:

```js
// Referentially transparent:
const x = add(3, 5);     // x = 8
const y = add(3, 5) * 2; // We can replace add(3, 5) with 8: y = 8 * 2 = 16

// NOT referentially transparent:
const x = Math.random();         // x = ???
const y = Math.random() * 2;     // NOT the same as x * 2!
```

**Why this matters**: Referentially transparent code is easier to reason about, test, debug, and optimize (memoization only works for pure functions).

**Practice**: FP #1, #2, #15, #19

---

## 3. Immutability

> **📸 Mental Model: The Photograph**
> Immutability means treating data like a photograph. Once taken, you cannot change the people in the picture. If someone blinks, you don't magically fix the current photo; you take a *new* photo where their eyes are open. In code, if a user changes their name, we don't modify the existing user object, we create a new user object with the updated name.

**Immutability** means never changing data after it's created. Instead of modifying existing values, you create new ones.

### Why Immutability?

| Benefit | Explanation |
|:--------|:-----------|
| **Predictability** | If data can't change, you can trust it at any point |
| **Debugging** | You can track what data looked like at every step |
| **Concurrency** | No race conditions when nothing can be mutated |
| **Undo/Redo** | Keep old versions around — they're never overwritten |
| **Memoization** | Safe to cache results when inputs can't change |

### Immutable Object Updates

```js
const user = { name: "Alice", age: 25, role: "admin" };

// ❌ MUTATING — changes the original object
user.age = 26;

// ✅ IMMUTABLE — creates a new object
const olderUser = { ...user, age: 26 };
// user is still { name: "Alice", age: 25, role: "admin" }

// Removing a property immutably
const { role, ...userWithoutRole } = user;
// userWithoutRole = { name: "Alice", age: 25 }
```

### Immutable Array Updates

```js
const nums = [1, 2, 3, 4, 5];

// ❌ MUTATING operations
nums.push(6);          // don't use push
nums.splice(1, 1);     // don't use splice
nums.sort();           // don't use sort (in-place)
nums[0] = 99;          // don't assign by index

// ✅ IMMUTABLE alternatives
const appended  = [...nums, 6];                         // [1, 2, 3, 4, 5, 6]
const prepended = [0, ...nums];                         // [0, 1, 2, 3, 4, 5]
const removed   = nums.filter((_, i) => i !== 1);       // [1, 3, 4, 5]
const updated   = nums.map((n, i) => i === 0 ? 99 : n); // [99, 2, 3, 4, 5]
const sorted    = [...nums].sort((a, b) => a - b);      // copy first, then sort
const reversed  = [...nums].reverse();                   // copy first, then reverse
const inserted  = [...nums.slice(0, 2), 99, ...nums.slice(2)]; // [1, 2, 99, 3, 4, 5]
```

### Immutable Nested Object Updates

This is where it gets tricky — you must spread at every level:

```js
const state = {
  user: {
    name: "Alice",
    address: {
      city: "NYC",
      zip: "10001"
    }
  },
  settings: { theme: "dark" }
};

// Update city immutably — spread at EVERY nested level
const newState = {
  ...state,
  user: {
    ...state.user,
    address: {
      ...state.user.address,
      city: "LA"
    }
  }
};
// state.user.address.city is still "NYC"
// newState.user.address.city is "LA"
// state.settings === newState.settings (shared reference — efficient!)
```

### Object.freeze — Enforcing Immutability

```js
const config = Object.freeze({
  apiUrl: "https://api.example.com",
  timeout: 5000
});

config.apiUrl = "hacked";     // silently fails (throws in strict mode)
config.apiUrl;                 // "https://api.example.com"

// ⚠️ Object.freeze is SHALLOW — nested objects are still mutable!
const deep = Object.freeze({ nested: { value: 1 } });
deep.nested.value = 2;         // THIS WORKS — freeze is shallow
deep.nested.value;             // 2 😱

// Solution: deep freeze (FP #36)
const deepFreeze = (obj) => {
  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === "object" && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return Object.freeze(obj);
};
```

### Structural Sharing

When you create a new object with spread, the unchanged parts share references with the original:

```js
const original = { a: 1, b: { c: 2 }, d: [3, 4] };
const updated = { ...original, a: 10 };

updated.b === original.b;  // true! Same reference — not copied
updated.d === original.d;  // true! Same reference — not copied
// Only the changed part (a) is new. This makes immutable updates efficient.
```

**Practice**: FP #3, #4, #5, #11, #16, #19, #36, #38; JS #41, #77

---

## 4. First-Class & Higher-Order Functions

### First-Class Functions

In JavaScript, functions are **first-class citizens** — they can be:

```js
// 1. Assigned to variables
const greet = (name) => `Hello, ${name}`;

// 2. Stored in data structures
const operations = [
  (x) => x + 1,
  (x) => x * 2,
  (x) => x ** 2
];

// 3. Passed as arguments
[1, 2, 3].map((x) => x * 2);

// 4. Returned from functions
const createGreeter = (greeting) => (name) => `${greeting}, ${name}!`;
const hola = createGreeter("Hola");
hola("Alice");  // "Hola, Alice!"
```

### Higher-Order Functions

A **higher-order function** (HOF) is a function that either:
- Takes a function as an argument, OR
- Returns a function as its result

```js
// Takes a function: map, filter, reduce, sort, setTimeout, addEventListener
[1, 2, 3].map(x => x * 2);       // map takes a callback
[1, 2, 3].filter(x => x > 1);    // filter takes a predicate
setTimeout(() => {}, 1000);       // setTimeout takes a callback

// Returns a function: factories, closures, decorators
const multiplier = (factor) => (n) => n * factor;
const triple = multiplier(3);
triple(7);  // 21

// Both: function that takes and returns a function
const not = (predicate) => (...args) => !predicate(...args);
const isOdd = (n) => n % 2 !== 0;
const isEven = not(isOdd);
isEven(4);  // true
```

### Common Higher-Order Patterns

```js
// Predicate builder
const isGreaterThan = (threshold) => (value) => value > threshold;
[5, 12, 8, 20].filter(isGreaterThan(10));  // [12, 20]

// Property accessor builder
const prop = (key) => (obj) => obj[key];
users.map(prop("name"));  // ["Alice", "Bob", "Charlie"]

// Function wrapper / decorator
const withLogging = (fn) => (...args) => {
  console.log(`Calling with args:`, args);
  const result = fn(...args);
  console.log(`Result:`, result);
  return result;
};
const loggedAdd = withLogging((a, b) => a + b);
loggedAdd(3, 5);  // logs: "Calling with args: [3, 5]", "Result: 8"
```

### Callback Functions

A callback is a function passed to another function to be called later:

```js
// Synchronous callback
[3, 1, 2].sort((a, b) => a - b);  // comparator is a callback

// Asynchronous callback
setTimeout(() => console.log("done"), 1000);

// Event handler callback
button.addEventListener("click", (event) => {
  console.log("clicked!");
});
```

**Practice**: FP #12, #13, #14, #31, #32; JS #42, #43, #44, #46, #47, #48, #49

---

## 5. Declarative vs. Imperative Style

### Imperative: HOW to Do It

Imperative code gives step-by-step instructions — you tell the computer *how* to achieve the goal:

```js
// Find all adults, get their names, sort alphabetically
const people = [
  { name: "Charlie", age: 15 },
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Diana", age: 12 }
];

// Imperative approach
const result = [];
for (let i = 0; i < people.length; i++) {
  if (people[i].age >= 18) {
    result.push(people[i].name);
  }
}
result.sort();
// result = ["Alice", "Bob"]
```

### Declarative: WHAT to Get

Declarative code describes the desired result — you tell the computer *what* you want:

```js
// Same task, declarative approach
const result = people
  .filter(p => p.age >= 18)
  .map(p => p.name)
  .sort();
// result = ["Alice", "Bob"]
```

### Side-by-Side Comparisons

| Task | Imperative | Declarative |
|:-----|:-----------|:------------|
| Sum | `let sum = 0; for(...) sum += n;` | `nums.reduce((a, b) => a + b, 0)` |
| Find max | `let max = -Infinity; for(...) if(n > max)...` | `nums.reduce((a, b) => a > b ? a : b)` |
| Flatten | `const flat = []; for(...) flat.push(...arr)` | `arrs.reduce((a, b) => a.concat(b), [])` |
| Count | `let count = 0; for(...) if(pred) count++` | `arr.filter(pred).length` |
| Transform | `const result = []; for(...) result.push(fn(x))` | `arr.map(fn)` |
| Group | `const groups = {}; for(...) { if(!groups[k])...}` | `arr.reduce((g, x) => ({...g, [key(x)]: [...(g[key(x)]??[]), x]}), {})` |

### Expressions vs. Statements

FP prefers **expressions** (produce a value) over **statements** (perform an action):

```js
// Statement — performs an action, no value
if (score >= 90) { grade = "A"; }
else { grade = "B"; }

// Expression — produces a value
const grade = score >= 90 ? "A" : "B";

// Statement — for loop
const doubled = [];
for (const n of nums) { doubled.push(n * 2); }

// Expression — map
const doubled = nums.map(n => n * 2);
```

### The Declarative Mindset

When you catch yourself writing a `for` loop, ask:
1. Am I **transforming** each element? → Use `map`
2. Am I **selecting** elements? → Use `filter`
3. Am I **combining** elements into one value? → Use `reduce`
4. Am I **checking** a condition for all/some? → Use `every`/`some`
5. Am I **searching** for an element? → Use `find`/`findIndex`
6. Am I **doing** something for each element? → Use `forEach` (last resort — side effects)

**Practice**: FP #6, #7, #8, #20, #39, #40

---

# Part II — Core Techniques

---

## 6. map, filter, reduce — The FP Trinity

> **🍲 Mental Model: Kitchen Ingredients**
> - `map`: You have a basket of raw potatoes. You slice every single potato. Now you have a basket of *sliced potatoes*. (Same amount, new form).
> - `filter`: You inspect the sliced potatoes and throw away the rotten ones. (Fewer items, same form).
> - `reduce`: You take the remaining good slices, put them in a pot, add milk and butter, and mash them down into a *single bowl of mashed potatoes*. (Many items become one new thing).

These three methods are the foundation of functional data processing in JavaScript. Master them, and you can express virtually any data transformation.

### `map` — Transform

**Contract**: Takes a function, applies it to every element, returns a new array of the same length.

```js
// Basic usage
[1, 2, 3].map(x => x * 2)           // [2, 4, 6]

// The callback receives: (element, index, array)
["a", "b", "c"].map((el, i) => `${i}: ${el}`)
// ["0: a", "1: b", "2: c"]

// Common patterns
users.map(u => u.name)               // extract a field
strings.map(s => s.trim())           // transform each
numbers.map(String)                  // convert types
```

**The Functor Law**: `map` should preserve structure. Mapping the identity function returns an equivalent array:
```js
arr.map(x => x)  // returns a copy of arr (same values, different reference)
```

### `filter` — Select

**Contract**: Takes a predicate (function returning boolean), returns a new array with only the elements where the predicate returned `true`.

```js
[1, 2, 3, 4, 5].filter(x => x > 3)     // [4, 5]
["", "hello", null, "world"].filter(Boolean)  // ["hello", "world"]

// Common patterns
users.filter(u => u.active)              // keep matching items
words.filter(w => w.length > 3)          // keep based on property
items.filter((item, i, arr) => arr.indexOf(item) === i)  // unique items
```

### `reduce` — Accumulate

**Contract**: Takes a reducer function and an initial value, applies the reducer across all elements to produce a single result.

```js
// Signature: arr.reduce((accumulator, current, index, array) => newAccumulator, initialValue)

// Sum
[1, 2, 3, 4].reduce((sum, n) => sum + n, 0)  // 10

// The accumulator can be ANY type:

// Build an object (frequency counter)
["a", "b", "a", "c"].reduce((counts, item) => ({
  ...counts,
  [item]: (counts[item] || 0) + 1
}), {})
// { a: 2, b: 1, c: 1 }

// Flatten arrays
[[1, 2], [3, 4], [5]].reduce((flat, arr) => [...flat, ...arr], [])
// [1, 2, 3, 4, 5]

// Group by
people.reduce((groups, person) => ({
  ...groups,
  [person.age]: [...(groups[person.age] || []), person]
}), {})

// Pipeline (apply a list of functions)
const pipeline = [add1, double, square];
pipeline.reduce((value, fn) => fn(value), 5)  // square(double(add1(5)))

// Maximum
[3, 7, 2, 9].reduce((max, n) => n > max ? n : max)
// 9 (no initial value — uses first element as seed)
```

### `reduceRight` — Right-to-Left Accumulation

```js
// Same as reduce, but processes from right to left
[[1], [2], [3]].reduceRight((acc, arr) => [...acc, ...arr], [])
// [3, 2, 1]

// Used for right-to-left function composition
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);
```

### `flatMap` — Map + Flatten

```js
// Equivalent to .map(fn).flat(1) but in one pass
[1, 2, 3].flatMap(x => [x, x * 10])
// [1, 10, 2, 20, 3, 30]

// Great for "expand" operations
["hello world", "foo bar"].flatMap(s => s.split(" "))
// ["hello", "world", "foo", "bar"]

// Filter + Map in one step (return empty array to skip)
[1, 2, 3, 4].flatMap(x => x > 2 ? [x * 10] : [])
// [30, 40]
```

### Chaining — Data Pipelines

```js
const result = orders
  .filter(o => o.status === "completed")      // select
  .map(o => ({                                 // transform
    ...o,
    total: o.items.reduce((sum, i) => sum + i.price, 0)
  }))
  .filter(o => o.total > 100)                 // select again
  .sort((a, b) => b.total - a.total)          // sort (returns new with toSorted)
  .slice(0, 10);                              // take top 10
```

### Implementing from Scratch

Understanding the internals is essential for JS #42–44:

```js
const myMap = (arr, fn) =>
  arr.reduce((result, item, i) => [...result, fn(item, i, arr)], []);

const myFilter = (arr, pred) =>
  arr.reduce((result, item, i) => pred(item, i, arr) ? [...result, item] : result, []);

const myReduce = (arr, fn, init) => {
  let acc = init !== undefined ? init : arr[0];
  const startIndex = init !== undefined ? 0 : 1;
  for (let i = startIndex; i < arr.length; i++) {
    acc = fn(acc, arr[i], i, arr);
  }
  return acc;
};
```

**Practice**: FP #6–10, #17–18, #20, #29, #40; JS #25, #26, #42–45, #51

---

## 7. Function Composition

**Function composition** is combining simple functions to build complex operations. It's how FP achieves modularity: small, focused functions snap together like building blocks.

### Two-Function Compose

```js
// compose(f, g) returns a function that applies g first, then f
const compose = (f, g) => (x) => f(g(x));

const add1 = x => x + 1;
const double = x => x * 2;

const doubleThenAdd1 = compose(add1, double);
doubleThenAdd1(5);  // add1(double(5)) = add1(10) = 11
```

### Variadic Compose (Right-to-Left)

```js
const compose = (...fns) => (x) =>
  fns.reduceRight((acc, fn) => fn(acc), x);

compose(add1, double, square)(3);
// add1(double(square(3))) = add1(double(9)) = add1(18) = 19
```

### Pipe (Left-to-Right)

Pipe is compose in reverse — the first function is applied first. Most developers find this more readable because it matches the data flow direction:

```js
const pipe = (...fns) => (x) =>
  fns.reduce((acc, fn) => fn(acc), x);

pipe(add1, double, square)(3);
// square(double(add1(3))) = square(double(4)) = square(8) = 64

// Read it like a pipeline: take 3 → add 1 → double → square
```

### Real-World Composition

```js
// Individual pure functions
const filterInStock = products => products.filter(p => p.inStock);
const applyDiscount = (percent) => (products) =>
  products.map(p => ({ ...p, price: p.price * (1 - percent / 100) }));
const sortByPrice = products => [...products].sort((a, b) => a.price - b.price);
const formatPrices = products =>
  products.map(p => ({ ...p, price: `$${p.price.toFixed(2)}` }));

// Compose them into a pipeline
const processProducts = pipe(
  filterInStock,
  applyDiscount(10),
  sortByPrice,
  formatPrices
);

// Use it
processProducts(products);
// Each function is small, testable, and reusable
```

### Point-Free Style

**Point-free** means defining functions without explicitly mentioning their arguments:

```js
// Pointed (names the argument)
const getNames = (people) => people.map(p => p.name);

// Point-free (no argument named)
const prop = (key) => (obj) => obj[key];
const getNames = map(prop("name"));

// Pointed
const isAdult = (person) => person.age >= 18;

// Point-free with a predicate builder
const propGte = (key, threshold) => (obj) => obj[key] >= threshold;
const isAdult = propGte("age", 18);
```

Point-free is elegant but can hurt readability when overdone. Use it when it makes intent clearer.

### Debugging Compositions

When a pipeline produces unexpected results, insert a `tap` function:

```js
const tap = (label) => (value) => {
  console.log(`[${label}]`, value);
  return value;  // pass through — doesn't affect the pipeline
};

pipe(
  filterInStock,
  tap("after filter"),      // peek at intermediate value
  applyDiscount(10),
  tap("after discount"),
  sortByPrice,
  formatPrices
)(products);
```

**Practice**: FP #21, #22, #23, #30; JS #53

---

## 8. Currying & Partial Application

> **🏭 Mental Model: The Assembly Line**
> Currying turns a function that takes all its arguments at once into an assembly line. At the first station, you add the chassis (first argument). It passes to the next station, waiting. When ready, the next station adds the engine (second argument), and so on, until the car is finished and rolls off the line.

### Currying

**Currying** transforms a function that takes multiple arguments into a chain of single-argument functions:

```js
// Uncurried
const add = (a, b, c) => a + b + c;
add(1, 2, 3);  // 6

// Manually curried
const addCurried = (a) => (b) => (c) => a + b + c;
addCurried(1)(2)(3);  // 6

// Partially applied — creates specialized functions
const add1 = addCurried(1);     // (b) => (c) => 1 + b + c
const add1and2 = add1(2);       // (c) => 1 + 2 + c = (c) => 3 + c
add1and2(3);                     // 6
```

### Auto-Curry (Generic Curry Function)

```js
const curry = (fn) => {
  const curried = (...args) =>
    args.length >= fn.length
      ? fn(...args)                                    // enough args — call fn
      : (...moreArgs) => curried(...args, ...moreArgs); // not enough — collect more
  return curried;
};

const add = curry((a, b, c) => a + b + c);

add(1)(2)(3);     // 6  — one at a time
add(1, 2)(3);     // 6  — two then one
add(1)(2, 3);     // 6  — one then two
add(1, 2, 3);     // 6  — all at once
```

### Partial Application

**Partial application** fixes some arguments of a function, returning a new function that takes the remaining ones:

```js
const partial = (fn, ...presetArgs) =>
  (...laterArgs) => fn(...presetArgs, ...laterArgs);

const multiply = (a, b, c) => a * b * c;
const double = partial(multiply, 2);     // fix first arg to 2
double(3, 4);   // 2 * 3 * 4 = 24

const sixTimes = partial(multiply, 2, 3); // fix first two args
sixTimes(4);    // 2 * 3 * 4 = 24
```

### Currying vs. Partial Application

| Feature | Currying | Partial Application |
|:--------|:---------|:-------------------|
| Returns | Chain of single-arg functions | A single function with some args fixed |
| Granularity | One argument at a time | Any number of arguments |
| Usage | `f(a)(b)(c)` | `partial(f, a)(b, c)` |
| Flexibility | Always consistent pattern | More flexible arg counts |

### Why Curry? Real Benefits

```js
// 1. Create specialized functions
const map = curry((fn, arr) => arr.map(fn));
const filter = curry((pred, arr) => arr.filter(pred));

const doubleAll = map(x => x * 2);       // specialized mapper
const getEvens = filter(x => x % 2 === 0); // specialized filter

doubleAll([1, 2, 3]);   // [2, 4, 6]
getEvens([1, 2, 3, 4]); // [2, 4]

// 2. Compose with curried functions (point-free)
const processNumbers = pipe(
  filter(x => x > 0),          // keep positive
  map(x => x * 10),            // scale up
  reduce((a, b) => a + b, 0)   // sum
);

// 3. Configuration
const log = curry((level, message) => console[level](message));
const warn = log("warn");
const error = log("error");
warn("Something fishy");  // console.warn("Something fishy")
```

**Practice**: FP #24, #25; JS #49

---

## 9. Closures in FP

Closures are the **mechanism** that makes most FP techniques work. A closure is a function bundled together with its surrounding state (the lexical environment).

### How Closures Enable FP

```js
// Closure for memoization — the cache persists across calls
const memoize = (fn) => {
  const cache = new Map();  // enclosed — persists across calls
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};

// Closure for factory functions
const createMultiplier = (factor) => {
  return (n) => n * factor;  // factor is enclosed
};
const triple = createMultiplier(3);
triple(10);  // 30

// Closure for once (call-at-most-once)
const once = (fn) => {
  let called = false;     // enclosed state
  let result;             // enclosed state
  return (...args) => {
    if (called) return result;
    called = true;
    result = fn(...args);
    return result;
  };
};
```

### Closures as Private State

In FP, closures replace classes as the mechanism for encapsulation:

```js
// OOP approach — class with private state
class Counter {
  #count = 0;
  increment() { return ++this.#count; }
  getCount() { return this.#count; }
}

// FP approach — closure with enclosed state
const createCounter = (initial = 0) => {
  let count = initial;
  return {
    increment: () => ++count,
    getCount: () => count
  };
};
// Note: this example uses mutation (count++) — a pragmatic trade-off
// A fully immutable version would return a new counter on each operation
```

### Closures in Currying

Every curried function is a closure:

```js
const add = (a) => {
  // This returned function is a closure over `a`
  return (b) => {
    // This returned function is a closure over both `a` and `b`
    return (c) => a + b + c;
  };
};

add(1)(2)(3);
// When add(1) is called, it returns a function that "remembers" a=1
// When that function(2) is called, it returns a function that "remembers" a=1, b=2
// When that function(3) is called, it computes 1 + 2 + 3 = 6
```

**Practice**: FP #13, #14, #31, #32; JS #46, #47, #48, #49

---

## 10. Recursion

In FP, **recursion** replaces loops. A recursive function calls itself to solve smaller subproblems.

### Anatomy of a Recursive Function

Every recursive function has two parts:
1. **Base case**: The condition where recursion stops
2. **Recursive case**: The function calls itself with a "smaller" problem

```js
// Sum an array recursively
const sum = (arr) => {
  if (arr.length === 0) return 0;            // base case
  return arr[0] + sum(arr.slice(1));          // recursive case
};

sum([1, 2, 3, 4]);
// 1 + sum([2, 3, 4])
// 1 + 2 + sum([3, 4])
// 1 + 2 + 3 + sum([4])
// 1 + 2 + 3 + 4 + sum([])
// 1 + 2 + 3 + 4 + 0 = 10
```

### The Accumulator Pattern

Regular recursion builds up a chain of deferred operations. The **accumulator pattern** passes the running result as a parameter:

```js
// Without accumulator — O(n) stack frames, deferred additions
const sum = (arr) => {
  if (arr.length === 0) return 0;
  return arr[0] + sum(arr.slice(1));
};

// With accumulator — result is computed as we go
const sum = (arr, acc = 0) => {
  if (arr.length === 0) return acc;
  return sum(arr.slice(1), acc + arr[0]);
};
// This is "tail-recursive" — the recursive call is the LAST operation
```

### Common Recursive Patterns

```js
// Recursive map
const map = (arr, fn) =>
  arr.length === 0 ? [] : [fn(arr[0]), ...map(arr.slice(1), fn)];

// Recursive filter
const filter = (arr, pred) =>
  arr.length === 0 ? []
  : pred(arr[0]) ? [arr[0], ...filter(arr.slice(1), pred)]
  : filter(arr.slice(1), pred);

// Recursive deep flatten
const deepFlatten = (arr) =>
  arr.reduce((flat, item) =>
    Array.isArray(item) ? [...flat, ...deepFlatten(item)] : [...flat, item],
  []);

// Recursive tree traversal
const sumTree = (tree) =>
  tree.tag === "Leaf" ? tree.value
  : sumTree(tree.left) + sumTree(tree.right);
```

### Mutual Recursion

Two or more functions that call each other:

```js
const isEven = (n) => n === 0 ? true : isOdd(n - 1);
const isOdd  = (n) => n === 0 ? false : isEven(n - 1);

isEven(4);  // true
isOdd(3);   // true
```

### Recursion vs. Loops — Trade-offs

| Factor | Recursion | Loops |
|:-------|:----------|:------|
| **Readability** | Natural for trees, nested structures | Better for simple iterations |
| **Stack usage** | O(n) stack frames (can overflow) | O(1) |
| **Immutability** | Naturally immutable | Requires discipline |
| **Performance** | Slower (function call overhead) | Faster |
| **FP purity** | Preferred | Discouraged |

### The Stack Overflow Problem

JavaScript doesn't optimize tail calls in practice (except Safari). Deep recursion can crash:

```js
const sum = (n, acc = 0) => n <= 0 ? acc : sum(n - 1, acc + n);
sum(100000);  // 💥 RangeError: Maximum call stack size exceeded
```

**Solutions**: Trampolines (→ §16), iteration for performance-critical code, or restructuring with `reduce`.

**Practice**: FP #26, #27, #28, #35, #37, #44; JS #19, #23, #39

---

# Part III — Advanced Patterns

---

## 11. Functors

A **functor** is any type that implements `map` and follows two laws.

### The Functor Interface

```js
// A functor is something you can map over:
// container.map(f) → new container with f applied to the inner value

// Arrays are functors:
[1, 2, 3].map(x => x + 1)   // [2, 3, 4]

// You can make your own:
const Box = (value) => ({
  map: (fn) => Box(fn(value)),
  value
});

Box(5).map(x => x + 1).map(x => x * 2);
// Box(12) — { map: ..., value: 12 }
```

### The Functor Laws

1. **Identity**: `functor.map(x => x)` returns an equivalent functor
2. **Composition**: `functor.map(f).map(g)` equals `functor.map(x => g(f(x)))`

```js
// Law 1: Identity
[1, 2, 3].map(x => x)        // [1, 2, 3] ✅

// Law 2: Composition
const f = x => x + 1;
const g = x => x * 2;

[1, 2, 3].map(f).map(g)      // [4, 6, 8]
[1, 2, 3].map(x => g(f(x)))  // [4, 6, 8] ✅ Same result!
```

Why do laws matter? They guarantee that refactoring `map(f).map(g)` into `map(compose(g, f))` always works — critical for optimization.

**Practice**: FP #41 (Maybe as functor)

---

## 12. Monads: Maybe & Either

A **monad** is a functor with an additional operation: `flatMap` (also called `chain` or `bind`). Monads solve the problem of composing operations that return wrapped values.

### The Maybe Monad — Safe Null Handling

```js
// The Problem: null checks everywhere
const getStreet = (user) => {
  if (user && user.address && user.address.street) {
    return user.address.street;
  }
  return "Unknown";
};

// The Solution: Maybe monad
const Maybe = {
  of: (value) =>
    value == null
      ? { map: () => Maybe.of(null), flatMap: () => Maybe.of(null), getOrElse: (def) => def }
      : {
          map: (fn) => Maybe.of(fn(value)),
          flatMap: (fn) => fn(value),
          getOrElse: () => value
        }
};

// Usage — clean, composable null handling
Maybe.of(user)
  .flatMap(u => Maybe.of(u.address))
  .flatMap(a => Maybe.of(a.street))
  .getOrElse("Unknown");

// If any step is null, the rest of the chain is skipped
Maybe.of(null).map(x => x * 2).getOrElse(0);  // 0
Maybe.of(5).map(x => x * 2).getOrElse(0);     // 10
```

### The Either Monad — Error Handling Without Exceptions

```js
// Left = error, Right = success
const Left = (value) => ({
  map: () => Left(value),            // skip transformation
  flatMap: () => Left(value),        // skip chaining
  fold: (leftFn, _rightFn) => leftFn(value)
});

const Right = (value) => ({
  map: (fn) => Right(fn(value)),     // apply transformation
  flatMap: (fn) => fn(value),        // chain operations
  fold: (_leftFn, rightFn) => rightFn(value)
});

const Either = {
  left: Left,
  right: Right
};

// Usage: safe division
const divide = (a, b) =>
  b === 0 ? Either.left("Division by zero") : Either.right(a / b);

divide(10, 2)
  .map(x => x + 1)
  .fold(
    err => `Error: ${err}`,     // handles the error path
    val => `Result: ${val}`     // handles the success path
  );
// "Result: 6"

divide(10, 0)
  .map(x => x + 1)
  .fold(
    err => `Error: ${err}`,
    val => `Result: ${val}`
  );
// "Error: Division by zero"
```

### The Monad Laws

1. **Left identity**: `Monad.of(a).flatMap(f)` equals `f(a)`
2. **Right identity**: `m.flatMap(Monad.of)` equals `m`
3. **Associativity**: `m.flatMap(f).flatMap(g)` equals `m.flatMap(x => f(x).flatMap(g))`

These laws guarantee that flatMap chaining behaves predictably, no matter how you group the operations.

### Promises Are (Almost) Monads

```js
// Promise.resolve is like Monad.of
// .then() is like .flatMap()

Promise.resolve(5)
  .then(x => x + 1)                    // like .map(x => x + 1)
  .then(x => Promise.resolve(x * 2))   // like .flatMap(x => Monad.of(x * 2))
  .then(console.log);                   // 12
```

Promises auto-flatten nested Promises in `.then()`, which is the flatMap behavior. However, Promises don't strictly follow all monad laws due to their eager execution.

**Practice**: FP #41, #42, #50, #58

---

## 13. Lenses & Optics

**Lenses** are composable pairs of getter/setter functions that focus on a specific part of a data structure, enabling elegant immutable updates.

### The Lens Interface

```js
// A lens is { getter, setter }
const lens = (getter, setter) => ({ getter, setter });

// Core operations
const view = (lens, obj) => lens.getter(obj);
const set  = (lens, val, obj) => lens.setter(obj, val);
const over = (lens, fn, obj) => set(lens, fn(view(lens, obj)), obj);
```

### Using Lenses

```js
const nameLens = lens(
  obj => obj.name,                         // getter
  (obj, value) => ({ ...obj, name: value }) // setter (immutable!)
);

const user = { name: "Alice", age: 25 };

view(nameLens, user);                        // "Alice"
set(nameLens, "Bob", user);                  // { name: "Bob", age: 25 }
over(nameLens, s => s.toUpperCase(), user);   // { name: "ALICE", age: 25 }
// user is still { name: "Alice", age: 25 }
```

### Composing Lenses for Deep Access

The real power of lenses is composition — you can compose them to focus deeper:

```js
const composeLens = (outer, inner) => lens(
  obj => view(inner, view(outer, obj)),
  (obj, val) => over(outer, innerObj => set(inner, val, innerObj), obj)
);

const addressLens = lens(
  obj => obj.address,
  (obj, val) => ({ ...obj, address: val })
);
const cityLens = lens(
  obj => obj.city,
  (obj, val) => ({ ...obj, city: val })
);

const addressCityLens = composeLens(addressLens, cityLens);

const person = { name: "Alice", address: { city: "NYC", zip: "10001" } };
view(addressCityLens, person);                 // "NYC"
set(addressCityLens, "LA", person);
// { name: "Alice", address: { city: "LA", zip: "10001" } }
```

Compare this to the manual spread approach:
```js
// Without lenses — verbose and error-prone
{ ...person, address: { ...person.address, city: "LA" } }
```

**Practice**: FP #43, #56

---

## 14. Transducers

**Transducers** are composable, collection-agnostic transformations. They solve the efficiency problem of chaining multiple `map`/`filter` calls (which create intermediate arrays).

### The Problem

```js
// This creates 3 intermediate arrays:
const result = bigArray
  .filter(x => x > 0)    // intermediate array 1
  .map(x => x * 2)       // intermediate array 2
  .filter(x => x < 100); // intermediate array 3
```

### Transducers: One Pass, No Intermediates

```js
// A transducer takes a reducer and returns a new reducer
const tMap = (fn) => (reducer) => (acc, item) => reducer(acc, fn(item));
const tFilter = (pred) => (reducer) => (acc, item) =>
  pred(item) ? reducer(acc, item) : acc;

// Compose transducers (left-to-right — unlike compose!)
const xform = compose(
  tFilter(x => x > 0),
  tMap(x => x * 2),
  tFilter(x => x < 100)
);

// Apply with different reducers and initial values
const arrayReducer = (acc, item) => [...acc, item];
const sumReducer = (acc, item) => acc + item;

// Same transformation, different outputs:
bigArray.reduce(xform(arrayReducer), []);  // array of results
bigArray.reduce(xform(sumReducer), 0);     // sum of results
```

The key insight: transducers separate **what** transformation to apply from **how** to collect results.

**Practice**: FP #40 (intuition), #48 (full implementation)

---

## 15. Lazy Evaluation & Generators

**Lazy evaluation** means computing values only when they're needed. JavaScript generators provide lazy evaluation.

### Eager vs. Lazy

```js
// EAGER — computes everything upfront
const result = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .filter(n => n % 2 === 0)   // processes ALL 10 elements → [2, 4, 6, 8, 10]
  .map(n => n * n)             // processes ALL 5 elements → [4, 16, 36, 64, 100]
  .slice(0, 3);                // takes 3 → [4, 16, 36]
// Total work: 15 operations (even though we only needed 3 results)

// LAZY — computes only what's needed
function* lazyFilter(gen, pred) {
  for (const x of gen) if (pred(x)) yield x;
}
function* lazyMap(gen, fn) {
  for (const x of gen) yield fn(x);
}
function* lazyTake(gen, n) {
  let count = 0;
  for (const x of gen) {
    if (count++ >= n) return;
    yield x;
  }
}

const result = [...lazyTake(
  lazyMap(
    lazyFilter([1,2,3,4,5,6,7,8,9,10].values(), n => n % 2 === 0),
    n => n * n
  ),
  3
)];
// Total work: 6 filter checks + 3 map transforms = 9 operations
```

### Infinite Sequences

Lazy evaluation enables infinite sequences — impossible with eager arrays:

```js
function* naturals() {
  let n = 1;
  while (true) yield n++;
}

function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

// Take the first 10 Fibonacci numbers
[...lazyTake(fibonacci(), 10)]
// [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

// Sum of first 100 even natural numbers
const lazyReduce = (gen, fn, init) => {
  let acc = init;
  for (const x of gen) acc = fn(acc, x);
  return acc;
};

lazyReduce(
  lazyTake(lazyFilter(naturals(), n => n % 2 === 0), 100),
  (a, b) => a + b,
  0
);
// 10100
```

**Practice**: FP #45; JS #70, #97

---

## 16. Trampolines & Stack Safety

JavaScript doesn't optimize tail calls (except Safari), so deep recursion causes stack overflow. **Trampolines** solve this.

### The Problem

```js
const sum = (n, acc = 0) => n <= 0 ? acc : sum(n - 1, acc + n);
sum(100000);  // 💥 RangeError: Maximum call stack size exceeded
```

### The Trampoline Solution

Instead of actually recursing, return a **thunk** (a function that will perform the next step). A loop then executes thunks until a final value is reached:

```js
const trampoline = (fn) => (...args) => {
  let result = fn(...args);
  while (typeof result === "function") {
    result = result();  // keep calling until we get a non-function value
  }
  return result;
};

const safeSum = trampoline(function sum(n, acc = 0) {
  if (n <= 0) return acc;           // final value — loop stops
  return () => sum(n - 1, acc + n); // thunk — loop continues
});

safeSum(100000);  // 5000050000 ✅ No stack overflow!
```

### How It Works

```
safeSum(3)
→ sum(3, 0) returns () => sum(2, 3)      ← thunk
→ sum(2, 3) returns () => sum(1, 5)      ← thunk
→ sum(1, 5) returns () => sum(0, 6)      ← thunk
→ sum(0, 6) returns 6                     ← final value
Result: 6
```

The key insight: we trade stack depth for a simple loop, turning O(n) stack frames into O(1).

**Practice**: FP #44

---

## 17. Algebraic Data Types & Pattern Matching

**Algebraic Data Types (ADTs)** are types composed of other types. The two fundamental forms are:
- **Product types**: Record/tuple — a value that has field A *and* field B (like objects)
- **Sum types**: Tagged union — a value that is variant A *or* variant B

### Sum Types in JavaScript

JavaScript doesn't have native sum types, but we can model them with tagged objects:

```js
// Define variants
const Leaf = (value) => ({ tag: "Leaf", value });
const Branch = (left, right) => ({ tag: "Branch", left, right });

// Pattern matching
const match = (value, patterns) => patterns[value.tag](value);

// Build a tree
const tree = Branch(
  Branch(Leaf(1), Leaf(2)),
  Leaf(3)
);

// Sum all leaves
const sumTree = (tree) => match(tree, {
  Leaf: ({ value }) => value,
  Branch: ({ left, right }) => sumTree(left) + sumTree(right)
});
sumTree(tree);  // 6

// Map over a tree (produces a new tree — immutable!)
const mapTree = (tree, fn) => match(tree, {
  Leaf: ({ value }) => Leaf(fn(value)),
  Branch: ({ left, right }) => Branch(mapTree(left, fn), mapTree(right, fn))
});

const doubled = mapTree(tree, x => x * 2);
sumTree(doubled);  // 12
sumTree(tree);      // 6 (original unchanged)
```

### Result Type (Like Rust's Result)

```js
const Ok = (value) => ({
  tag: "Ok", value,
  map: (fn) => Ok(fn(value)),
  flatMap: (fn) => fn(value),
  unwrapOr: (_) => value
});

const Err = (error) => ({
  tag: "Err", error,
  map: () => Err(error),
  flatMap: () => Err(error),
  unwrapOr: (def) => def
});

// Usage
const parseJSON = (str) => {
  try { return Ok(JSON.parse(str)); }
  catch (e) { return Err(e.message); }
};

parseJSON('{"a": 1}').map(obj => obj.a).unwrapOr(null);  // 1
parseJSON('invalid').map(obj => obj.a).unwrapOr(null);     // null
```

**Practice**: FP #51, #60

---

## 18. Persistent Data Structures

**Persistent data structures** preserve previous versions when modified. They use **structural sharing** to be memory-efficient.

### Immutable Linked List

```js
// A cons cell: { head, tail }
// Empty list: null

const cons = (head, tail) => ({ head, tail });
const head = (list) => list.head;
const tail = (list) => list.tail;
const isEmpty = (list) => list === null;

// Build: List.of(1, 2, 3)
const listOf = (...items) =>
  items.reduceRight((list, item) => cons(item, list), null);

// The magic of persistence:
const list = listOf(1, 2, 3);        // 1 → 2 → 3 → null
const list = cons(0, list);          // 0 → 1 → 2 → 3 → null
// list is still 1 → 2 → 3 → null
// list SHARES the tail with list — zero copying!

// Convert to array
const toArray = (list) => {
  const result = [];
  let current = list;
  while (current !== null) {
    result.push(current.head);
    current = current.tail;
  }
  return result;
};
```

### Why Structural Sharing Matters

```
list1:         [1] → [2] → [3] → null
                ↑
list2:   [0] ──┘

Both lists exist simultaneously.
list2 reuses list1's entire structure.
No copying needed for the prepend operation!
```

This principle scales to trees (like those used in React's virtual DOM and Redux's state management).

**Practice**: FP #46, #57

---

# Part IV — FP in Practice

---

## 19. State Management with Reducers

The Redux pattern is FP applied to state management:

### The Reducer Pattern

```js
// A reducer is a pure function: (state, action) → newState
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1, history: [...state.history, state.count + 1] };
    case "DECREMENT":
      return { ...state, count: state.count - 1, history: [...state.history, state.count - 1] };
    case "RESET":
      return { ...state, count: 0, history: [...state.history, 0] };
    default:
      return state;  // unknown action — return unchanged state
  }
};

const initialState = { count: 0, history: [] };

// Apply actions (each returns a new state — state is never mutated)
const s = reducer(initialState, { type: "INCREMENT" }); // { count: 1, history: [1] }
const s = reducer(s, { type: "INCREMENT" });            // { count: 2, history: [1, 2] }
const s = reducer(s, { type: "DECREMENT" });            // { count: 1, history: [1, 2, 1] }
const s = reducer(s, { type: "RESET" });                // { count: 0, history: [1, 2, 1, 0] }
```

### Why Reducers Are FP

1. **Pure function**: Same state + action always produces the same new state
2. **Immutable**: Original state is never modified
3. **Predictable**: You can replay actions to reconstruct any past state
4. **Testable**: Just call the function with inputs and check the output

### Building a Simple Store

```js
const createStore = (reducer, initialState) => {
  let state = initialState;
  const listeners = [];
  return {
    getState: () => state,
    dispatch: (action) => {
      state = reducer(state, action);
      listeners.forEach(fn => fn(state));
    },
    subscribe: (fn) => {
      listeners.push(fn);
      return () => listeners.splice(listeners.indexOf(fn), 1);
    }
  };
};
```

**Practice**: FP #38; JS #80, #88

---

## 20. Functional Reactive Programming

FRP treats events as **streams of values over time** and applies FP transformations to them.

### Observable — A Stream of Values

```js
const Observable = (subscribe) => ({
  subscribe,

  map: (fn) => Observable(observer =>
    subscribe({
      next: val => observer.next(fn(val)),
      error: err => observer.error(err),
      complete: () => observer.complete()
    })
  ),

  filter: (pred) => Observable(observer =>
    subscribe({
      next: val => pred(val) && observer.next(val),
      error: err => observer.error(err),
      complete: () => observer.complete()
    })
  ),

  take: (n) => Observable(observer => {
    let count = 0;
    subscribe({
      next: val => {
        if (count++ < n) {
          observer.next(val);
          if (count === n) observer.complete();
        }
      },
      error: err => observer.error(err),
      complete: () => observer.complete()
    });
  })
});
```

### Using Observables

```js
// Create a stream that emits numbers
const numbers = Observable(observer => {
  let i = 0;
  const id = setInterval(() => {
    if (i >= 10) { clearInterval(id); observer.complete(); }
    else observer.next(i++);
  }, 100);
});

// Process the stream with FP operations
numbers
  .filter(n => n % 2 === 0)   // keep even numbers
  .map(n => n * 10)            // multiply by 10
  .take(3)                     // only first 3
  .subscribe({
    next: val => console.log(val),     // 0, 20, 40
    complete: () => console.log("Done")
  });
```

**Practice**: FP #53; JS #71, #88

---

## 21. Error Handling Without Exceptions

FP avoids `throw`/`try`/`catch` because exceptions are side effects that disrupt control flow.

### The Either/Result Approach

```js
// Parse → Validate → Transform → Format
// Each step can fail, but errors flow through the pipeline automatically

const parseAge = (input) => {
  const n = Number(input);
  return isNaN(n) ? Either.left(`"${input}" is not a number`)
                  : Either.right(n);
};

const validateAge = (age) =>
  age < 0 ? Either.left("Age cannot be negative")
  : age > 150 ? Either.left("Age seems unrealistic")
  : Either.right(age);

const processAge = (input) =>
  parseAge(input)
    .flatMap(validateAge)
    .map(age => ({ age, category: age >= 18 ? "adult" : "minor" }));

processAge("25").fold(console.error, console.log);
// { age: 25, category: "adult" }

processAge("abc").fold(console.error, console.log);
// "abc" is not a number

processAge("-5").fold(console.error, console.log);
// Age cannot be negative
```

### Advantages Over try/catch

| Feature | try/catch | Either/Result |
|:--------|:----------|:-------------|
| Composable | ❌ Breaks the pipeline | ✅ Chains naturally |
| Type-safe | ❌ Any code can throw | ✅ Errors are explicit values |
| Short-circuits | ✅ Stops at throw | ✅ Left skips subsequent maps |
| Recoverable | ❌ Need separate catch blocks | ✅ fold/getOrElse at the end |

**Practice**: FP #42; JS #57

---

## 22. FP Design Patterns

### The Interpreter Pattern (Free Monad)

Separate what to do from how to do it:

```js
// Define instructions as data
const Get = (key, next) => ({ tag: "Get", key, next });
const Put = (key, value, next) => ({ tag: "Put", key, value, next });
const End = (value) => ({ tag: "End", value });

// Write a "program" as data
const program = Put("name", "Alice", Get("name", (name) => End(`Hello, ${name}`)));

// Interpret the program
const interpret = (prog, store = {}) => {
  switch (prog.tag) {
    case "Put": return interpret(prog.next, { ...store, [prog.key]: prog.value });
    case "Get": return interpret(prog.next(store[prog.key]), store);
    case "End": return prog.value;
  }
};

interpret(program);  // "Hello, Alice"
```

The same program can be interpreted differently — in-memory, with logging, with actual database calls. This is the essence of the Free Monad pattern.

### The Pipeline Pattern

```js
const processOrder = pipe(
  validateOrder,
  calculateTotals,
  applyDiscounts,
  addTax,
  formatInvoice
);
```

### The Middleware Pattern

```js
const applyMiddleware = (...middlewares) => (store) => (next) => (action) =>
  middlewares.reduceRight(
    (dispatch, middleware) => middleware(store)(dispatch),
    next
  )(action);
```

**Practice**: FP #52, #60

---

## 23. When NOT to Use FP

FP is powerful but not always the best tool. Be pragmatic:

### Performance-Critical Code

```js
// FP style — creates intermediate arrays
const sum = arr.filter(x => x > 0).map(x => x * 2).reduce((a, b) => a + b, 0);

// Imperative — single pass, no allocations
let sum = 0;
for (let i = 0; i < arr.length; i++) {
  if (arr[i] > 0) sum += arr[i] * 2;
}
// For hot loops processing millions of elements, the imperative version wins
```

### When Readability Suffers

```js
// Over-abstracted FP — hard to read
const process = pipe(
  map(compose(formatCurrency, applyTax(0.2), multiply(0.9))),
  filter(compose(not, propEq("status", "cancelled"))),
  sortBy(prop("date"))
);

// Clearer alternative
const process = (orders) =>
  orders
    .filter(o => o.status !== "cancelled")
    .map(o => ({ ...o, total: formatCurrency(o.price * 0.9 * 1.2) }))
    .sort((a, b) => a.date - b.date);
```

### Inherently Stateful Systems

- DOM manipulation
- Animation/game loops
- Connection/session management
- File I/O

For these, isolate the impure parts and keep your business logic pure:

```
┌──────────────────────────────────────────────────┐
│                 APPLICATION                       │
│                                                   │
│   ┌──────────────┐    ┌────────────────────────┐ │
│   │  IMPURE SHELL │    │     PURE CORE          │ │
│   │  (I/O, DOM,   │ →  │  (Business logic,      │ │
│   │   network)    │ ←  │   data transforms)     │ │
│   └──────────────┘    └────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

### The 80/20 Rule

In practice, aim for:
- **80% pure functions** for your business logic
- **20% controlled impurity** for I/O, DOM, and system boundaries

---

## Glossary

| Term | Definition |
|:-----|:-----------|
| **Arity** | Number of arguments a function takes |
| **Closure** | Function + its captured environment |
| **Combinator** | Higher-order function using only its arguments |
| **Currying** | Converting multi-arg function to chain of single-arg functions |
| **Functor** | Type with a lawful `map` |
| **Idempotent** | Applying an operation multiple times gives same result as once |
| **Lambda** | Anonymous function: `x => x + 1` |
| **Monad** | Functor with lawful `flatMap`/`chain` |
| **Morphism** | Transformation from one type to another |
| **Partial Application** | Fixing some arguments of a function |
| **Point-free** | Defining functions without naming their arguments |
| **Predicate** | Function that returns boolean |
| **Reducer** | Function `(state, action) → newState` |
| **Referential Transparency** | Expression replaceable by its value |
| **Side Effect** | Observable change outside the function |
| **Thunk** | Function with no arguments, delaying computation |
| **Transducer** | Composable reducer transformer |

---

*This document is a companion to [04-functional-programming-problems.md](./04-functional-programming-problems.md) and [02-javascript-coding-problems.md](./02-javascript-coding-problems.md). For detailed solutions, see [05-problem-solving-with-fp.md](./05-problem-solving-with-fp.md). For JavaScript syntax, see [01-javascript-foundations.md](./01-javascript-foundations.md). For a quick cheat sheet, see [06-quick-reference-card.md](./06-quick-reference-card.md).*
