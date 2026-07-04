# 🧮 JavaScript Functional Programming Problems — Beginner to Advanced

A curated set of **60 coding challenges** designed to build your functional programming skills from the ground up. Every problem reinforces one or more FP principles: **pure functions, immutability, higher-order functions, function composition, recursion, currying, and declarative style**.

> **Rule of thumb for every problem below:** avoid mutation, avoid side effects, and prefer expressions over statements.

---

## Core Principles Reference

Before you begin, keep these principles in mind:

| Principle | Description |
|:----------|:------------|
| **Pure Function** | Same inputs always produce the same output; no side effects |
| **Immutability** | Never mutate data; always return new values |
| **Higher-Order Function** | A function that takes or returns another function |
| **Function Composition** | Combining simple functions to build complex operations |
| **Recursion** | A function that calls itself to solve smaller subproblems |
| **Declarative Style** | Describe *what* to do, not *how* to do it (avoid `for`/`while` loops) |
| **Currying** | Transforming a function of multiple arguments into a chain of single-argument functions |
| **Referential Transparency** | An expression can be replaced with its value without changing the program's behavior |

---

## 🌱 Level 1 — Beginner (Problems 1–20)

> **Focus**: Pure functions, basic immutability, introduction to `map`/`filter`/`reduce`, avoiding mutation

---

### Problem 1: Pure Addition

Write a pure function `add(a, b)` that returns the sum of two numbers. Verify its purity by calling it multiple times with the same arguments and confirming the result never changes.

```js
add(3, 5);  // 8
add(3, 5);  // 8 (always the same)
add(-1, 1); // 0
```

**🎯 FP Principle**: Pure functions — no side effects, deterministic output.

<details><summary>💡 Hint</summary>A pure function only depends on its arguments and produces no side effects (no <code>console.log</code>, no mutation, no I/O).</details>

---

### Problem 2: Impure to Pure

The function below is **impure**. Refactor it into a pure function.

```js
// Impure version
let taxRate = 0.2;
function calculateTotal(price) {
  return price + price * taxRate;
}
```

Your pure version should accept `taxRate` as a parameter and return the total without relying on any external variable.

```js
calculateTotal(100, 0.2);  // 120
calculateTotal(100, 0.1);  // 110
calculateTotal(50, 0.25);  // 62.5
```

**🎯 FP Principle**: Referential transparency — the result depends solely on the inputs.

<details><summary>💡 Hint</summary>Move every dependency into the function's parameter list. The function should rely on nothing outside its own scope.</details>

---

### Problem 3: Immutable Object Update

Write a function `updateAge(person, newAge)` that returns a **new** object with the updated age. The original object must remain unchanged.

```js
const alice = { name: "Alice", age: 25 };
const olderAlice = updateAge(alice, 30);

console.log(olderAlice); // { name: "Alice", age: 30 }
console.log(alice);      // { name: "Alice", age: 25 } — unchanged!
```

**🎯 FP Principle**: Immutability — never modify original data.

<details><summary>💡 Hint</summary>Use the spread operator: <code>{ ...person, age: newAge }</code></details>

---

### Problem 4: Immutable Array Push

Write a function `appendItem(arr, item)` that returns a **new** array with the item appended. The original array must not be mutated.

```js
const fruits = ["apple", "banana"];
const moreFruits = appendItem(fruits, "cherry");

console.log(moreFruits); // ["apple", "banana", "cherry"]
console.log(fruits);     // ["apple", "banana"] — unchanged!
```

**🎯 FP Principle**: Immutability with arrays.

<details><summary>💡 Hint</summary>Use <code>[...arr, item]</code> instead of <code>arr.push(item)</code>.</details>

---

### Problem 5: Immutable Array Remove

Write a function `removeItem(arr, index)` that returns a **new** array with the item at the given index removed.

```js
const nums = [10, 20, 30, 40];
removeItem(nums, 1);  // [10, 30, 40]
console.log(nums);    // [10, 20, 30, 40] — unchanged!
```

**🎯 FP Principle**: Immutability — use `filter` or `slice` instead of `splice`.

<details><summary>💡 Hint</summary>Use <code>arr.filter((_, i) => i !== index)</code>.</details>

---

### Problem 6: Transform with `map`

Write a function `doubleAll(numbers)` that returns a new array with every number doubled. Do **not** use a loop.

```js
doubleAll([1, 2, 3, 4]);   // [2, 4, 6, 8]
doubleAll([]);              // []
doubleAll([0, -1, 5]);      // [0, -2, 10]
```

**🎯 FP Principle**: Declarative transformation with `map`.

<details><summary>💡 Hint</summary><code>numbers.map(n => n * 2)</code></details>

---

### Problem 7: Filter with a Predicate

Write a function `getEvens(numbers)` that returns only the even numbers from the array. Do **not** use a loop.

```js
getEvens([1, 2, 3, 4, 5, 6]);  // [2, 4, 6]
getEvens([7, 9, 11]);           // []
getEvens([2, 4, 6]);            // [2, 4, 6]
```

**🎯 FP Principle**: Declarative filtering with `filter`.

<details><summary>💡 Hint</summary><code>numbers.filter(n => n % 2 === 0)</code></details>

---

### Problem 8: Reduce to a Single Value

Write a function `sum(numbers)` using `reduce`. No loops, no external variables.

```js
sum([1, 2, 3, 4, 5]);  // 15
sum([]);                // 0
sum([10]);              // 10
```

**🎯 FP Principle**: Reducing a collection to a single accumulated value.

<details><summary>💡 Hint</summary><code>numbers.reduce((acc, n) => acc + n, 0)</code></details>

---

### Problem 9: Transform Objects in an Array

Write a function `getNames(people)` that extracts just the `name` property from each object.

```js
const people = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 }
];

getNames(people); // ["Alice", "Bob", "Charlie"]
```

**🎯 FP Principle**: Using `map` to project a specific property.

<details><summary>💡 Hint</summary><code>people.map(person => person.name)</code></details>

---

### Problem 10: Chain `map` and `filter`

Write a function `getAdultNames(people)` that returns the names of people aged 18 or older. Use method chaining — no loops.

```js
const people = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 15 },
  { name: "Charlie", age: 18 },
  { name: "Diana", age: 12 }
];

getAdultNames(people); // ["Alice", "Charlie"]
```

**🎯 FP Principle**: Composing `filter` and `map` into a data pipeline.

<details><summary>💡 Hint</summary><code>people.filter(p => p.age >= 18).map(p => p.name)</code></details>

---

### Problem 11: Immutable Nested Object Update

Write a function `updateCity(user, newCity)` that returns a new user object with the city updated inside the nested `address` object. No mutation at any level.

```js
const user = {
  name: "Alice",
  address: { street: "123 Main St", city: "Springfield", zip: "62704" }
};

const updated = updateCity(user, "Shelbyville");
console.log(updated.address.city);  // "Shelbyville"
console.log(user.address.city);     // "Springfield" — unchanged!
```

**🎯 FP Principle**: Deep immutability — spread at every nested level.

<details><summary>💡 Hint</summary><code>{ ...user, address: { ...user.address, city: newCity } }</code></details>

---

### Problem 12: First-Class Functions

Write a function `applyOperation(a, b, operation)` where `operation` is a function passed as an argument.

```js
const multiply = (x, y) => x * y;
const subtract = (x, y) => x - y;

applyOperation(5, 3, multiply);  // 15
applyOperation(10, 4, subtract); // 6
applyOperation(2, 8, (a, b) => a + b); // 10
```

**🎯 FP Principle**: Functions as first-class citizens — passed as arguments.

<details><summary>💡 Hint</summary>Simply call <code>operation(a, b)</code> inside your function.</details>

---

### Problem 13: Returning Functions

Write a function `createMultiplier(factor)` that **returns** a new function. The returned function multiplies any number by the given factor.

```js
const double = createMultiplier(2);
const triple = createMultiplier(3);

double(5);  // 10
triple(5);  // 15
double(0);  // 0
```

**🎯 FP Principle**: Higher-order functions — returning a function.

<details><summary>💡 Hint</summary><code>return (n) => n * factor;</code></details>

---

### Problem 14: Simple Predicate Builder

Write a function `isGreaterThan(threshold)` that returns a predicate function. Use it with `filter`.

```js
const isAbove10 = isGreaterThan(10);

isAbove10(15);  // true
isAbove10(5);   // false

[5, 12, 8, 20, 3].filter(isGreaterThan(10)); // [12, 20]
```

**🎯 FP Principle**: Higher-order functions and partial application.

<details><summary>💡 Hint</summary><code>return (value) => value > threshold;</code></details>

---

### Problem 15: Pure String Transformer

Write a pure function `formatName(first, last)` that returns a formatted full name. Test it is pure: same inputs always yield the same output, and no external state is read or written.

```js
formatName("john", "doe");     // "John Doe"
formatName("ALICE", "SMITH");  // "Alice Smith"
formatName("bOb", "jOnEs");    // "Bob Jones"
```

**🎯 FP Principle**: Pure function — deterministic, side-effect-free.

<details><summary>💡 Hint</summary>Capitalize the first letter, lowercase the rest: <code>str[0].toUpperCase() + str.slice(1).toLowerCase()</code></details>

---

### Problem 16: Immutable Array Update

Write a function `updateAt(arr, index, value)` that returns a new array with the element at `index` replaced by `value`.

```js
const colors = ["red", "green", "blue"];
updateAt(colors, 1, "yellow");  // ["red", "yellow", "blue"]
console.log(colors);            // ["red", "green", "blue"] — unchanged!
```

**🎯 FP Principle**: Immutable updates without `splice`.

<details><summary>💡 Hint</summary><code>arr.map((item, i) => i === index ? value : item)</code></details>

---

### Problem 17: Count with `reduce`

Write a function `countOccurrences(arr, target)` that counts how many times `target` appears in `arr` using `reduce`.

```js
countOccurrences([1, 2, 3, 2, 1, 2], 2);            // 3
countOccurrences(["a", "b", "a", "c", "a"], "a");    // 3
countOccurrences([1, 2, 3], 5);                       // 0
```

**🎯 FP Principle**: Using `reduce` for aggregation instead of imperative counting.

<details><summary>💡 Hint</summary><code>arr.reduce((count, item) => item === target ? count + 1 : count, 0)</code></details>

---

### Problem 18: Build Object with `reduce`

Write a function `arrayToObject(pairs)` that converts an array of `[key, value]` pairs into an object using `reduce`.

```js
arrayToObject([["name", "Alice"], ["age", 25], ["city", "NYC"]]);
// { name: "Alice", age: 25, city: "NYC" }

arrayToObject([]);
// {}
```

**🎯 FP Principle**: Building data structures declaratively with `reduce`.

<details><summary>💡 Hint</summary><code>pairs.reduce((obj, [key, value]) => ({ ...obj, [key]: value }), {})</code></details>

---

### Problem 19: Avoid Mutation Trap

Identify what's wrong with this code and write a pure, immutable version of `addToCart`:

```js
// Buggy version — mutates the original cart!
function addToCart(cart, item) {
  cart.items.push(item);
  cart.total += item.price;
  return cart;
}
```

Your pure version should return a brand-new cart object without modifying the original.

```js
const cart = { items: [{ name: "Book", price: 10 }], total: 10 };
const newCart = addToCart(cart, { name: "Pen", price: 2 });

console.log(newCart.total);     // 12
console.log(newCart.items.length); // 2
console.log(cart.total);        // 10 — unchanged!
console.log(cart.items.length); // 1 — unchanged!
```

**🎯 FP Principle**: Avoiding mutation in real-world data structures.

<details><summary>💡 Hint</summary><code>return { items: [...cart.items, item], total: cart.total + item.price };</code></details>

---

### Problem 20: Declarative Max

Write a function `findMax(numbers)` that returns the largest number using `reduce`. No `Math.max`, no loops, no mutation.

```js
findMax([3, 7, 2, 9, 1]);  // 9
findMax([-5, -1, -8]);      // -1
findMax([42]);              // 42
```

**🎯 FP Principle**: Declarative style — express the *what*, not the *how*.

<details><summary>💡 Hint</summary><code>numbers.reduce((max, n) => n > max ? n : max)</code> (no initial value — uses first element as seed).</details>

---

## 🌿 Level 2 — Intermediate (Problems 21–40)

> **Focus**: Function composition, currying, closures in FP, advanced `reduce` patterns, recursion, point-free style

---

### Problem 21: Simple Compose

Write a function `compose(f, g)` that creates a new function applying `g` first, then `f`.

```js
const add1 = x => x + 1;
const double = x => x * 2;

const doubleThenAdd1 = compose(add1, double);
doubleThenAdd1(5);  // 11  — double(5) = 10, then add1(10) = 11

const add1ThenDouble = compose(double, add1);
add1ThenDouble(5);  // 12  — add1(5) = 6, then double(6) = 12
```

**🎯 FP Principle**: Function composition — building complex operations from simple parts.

<details><summary>💡 Hint</summary><code>return (x) => f(g(x));</code></details>

---

### Problem 22: Pipe (Left-to-Right Composition)

Write a function `pipe(...fns)` that composes functions left-to-right (the first function is applied first).

```js
const add1 = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

pipe(add1, double, square)(3);   // square(double(add1(3))) = square(8) = 64
pipe(double, add1)(5);           // add1(double(5)) = 11
```

**🎯 FP Principle**: Left-to-right composition is more readable for data pipelines.

<details><summary>💡 Hint</summary><code>fns.reduce((acc, fn) => fn(acc), x)</code> — return a function that threads the initial value through each function.</details>

---

### Problem 23: Compose with Multiple Functions

Write a function `compose(...fns)` that composes an arbitrary number of functions right-to-left.

```js
const add1 = x => x + 1;
const double = x => x * 2;
const negate = x => -x;

compose(negate, double, add1)(3);  // negate(double(add1(3))) = negate(8) = -8
compose(add1)(5);                  // 6
compose()(5);                      // 5 (identity)
```

**🎯 FP Principle**: Variadic composition with `reduceRight`.

<details><summary>💡 Hint</summary><code>fns.reduceRight((acc, fn) => fn(acc), x)</code>. Handle the empty case by returning the identity function.</details>

---

### Problem 24: Manual Currying

Write a function `curry(fn)` that transforms a function of multiple arguments into a sequence of single-argument functions.

```js
const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);

curriedAdd(1)(2)(3);    // 6
curriedAdd(1, 2)(3);    // 6
curriedAdd(1)(2, 3);    // 6
curriedAdd(1, 2, 3);    // 6
```

**🎯 FP Principle**: Currying enables partial application and point-free style.

<details><summary>💡 Hint</summary>Return a recursive function that collects arguments. When <code>args.length >= fn.length</code>, call <code>fn(...args)</code>. Otherwise, return a new function that appends more arguments.</details>

---

### Problem 25: Partial Application

Write a function `partial(fn, ...presetArgs)` that fixes the first N arguments of a function.

```js
const multiply = (a, b, c) => a * b * c;

const double = partial(multiply, 2);
double(3, 4);   // 24

const sixTimes = partial(multiply, 2, 3);
sixTimes(4);    // 24

const always24 = partial(multiply, 2, 3, 4);
always24();     // 24
```

**🎯 FP Principle**: Partial application — creating specialized functions from general ones.

<details><summary>💡 Hint</summary><code>return (...laterArgs) => fn(...presetArgs, ...laterArgs);</code></details>

---

### Problem 26: Recursive Sum

Write a function `recursiveSum(arr)` that sums an array using **recursion** with no loops and no `reduce`.

```js
recursiveSum([1, 2, 3, 4, 5]);  // 15
recursiveSum([]);                // 0
recursiveSum([42]);              // 42
```

**🎯 FP Principle**: Recursion as the FP alternative to loops.

<details><summary>💡 Hint</summary>Base case: empty array returns 0. Recursive case: <code>arr[0] + recursiveSum(arr.slice(1))</code>.</details>

---

### Problem 27: Recursive Map

Write a function `recursiveMap(arr, fn)` that works like `Array.prototype.map` but uses recursion instead of a loop.

```js
recursiveMap([1, 2, 3], x => x * 2);            // [2, 4, 6]
recursiveMap(["a", "b", "c"], s => s.toUpperCase()); // ["A", "B", "C"]
recursiveMap([], x => x);                        // []
```

**🎯 FP Principle**: Implementing standard FP operations recursively.

<details><summary>💡 Hint</summary>Base case: empty array returns <code>[]</code>. Recursive case: <code>[fn(arr[0]), ...recursiveMap(arr.slice(1), fn)]</code>.</details>

---

### Problem 28: Recursive Flatten

Write a function `deepFlatten(arr)` that recursively flattens a deeply nested array. No `.flat()` allowed.

```js
deepFlatten([1, [2, [3, [4]], 5]]);  // [1, 2, 3, 4, 5]
deepFlatten([[1, 2], [3, [4, [5]]]]);  // [1, 2, 3, 4, 5]
deepFlatten([]);                     // []
deepFlatten([1, 2, 3]);             // [1, 2, 3]
```

**🎯 FP Principle**: Recursion to solve variable-depth data structures.

<details><summary>💡 Hint</summary>Use <code>reduce</code> with recursion: if an element is an array, recursively flatten it; otherwise, wrap it in an array and concatenate.</details>

---

### Problem 29: Group By with `reduce`

Write a pure function `groupBy(arr, keyFn)` where `keyFn` is a function that extracts the grouping key from each element.

```js
groupBy([1, 2, 3, 4, 5, 6], n => n % 2 === 0 ? "even" : "odd");
// { odd: [1, 3, 5], even: [2, 4, 6] }

groupBy(["one", "two", "three", "four"], s => s.length);
// { 3: ["one", "two"], 5: ["three"], 4: ["four"] }
```

**🎯 FP Principle**: Using `reduce` to build complex data structures declaratively.

<details><summary>💡 Hint</summary>In your reducer, compute the key, then spread the existing group (or empty array) and append the current item.</details>

---

### Problem 30: Function Composition Pipeline for Data

Build a data processing pipeline using `pipe` to transform an array of raw product data.

```js
const products = [
  { name: "Laptop", price: 999, inStock: true },
  { name: "Phone", price: 699, inStock: false },
  { name: "Tablet", price: 499, inStock: true },
  { name: "Watch", price: 299, inStock: true },
  { name: "Headphones", price: 149, inStock: false }
];

// Build these as separate pure functions, then compose them:
// 1. filterInStock — keeps only in-stock products
// 2. applyDiscount(percent) — reduces each product's price (returns new array)
// 3. sortByPrice — sorts ascending by price (returns new array)
// 4. formatPrices — converts price to a "$X.XX" string

const processProducts = pipe(
  filterInStock,
  applyDiscount(10),
  sortByPrice,
  formatPrices
);

processProducts(products);
// [
//   { name: "Watch", price: "$269.10" },
//   { name: "Tablet", price: "$449.10" },
//   { name: "Laptop", price: "$899.10" }
// ]
```

**🎯 FP Principle**: Composing a pipeline of pure, single-responsibility functions.

<details><summary>💡 Hint</summary>Each function takes an array and returns a new array. <code>applyDiscount</code> is a curried function: <code>percent => products => products.map(...)</code>.</details>

---

### Problem 31: Memoize (Pure Caching)

Write a function `memoize(fn)` that caches the results of a **pure** function based on its arguments.

```js
let callCount = 0;
const expensiveSquare = (n) => { callCount++; return n * n; };

const memoSquare = memoize(expensiveSquare);
memoSquare(4);  // 16 (computed)
memoSquare(4);  // 16 (cached — callCount stays at 1)
memoSquare(5);  // 25 (computed — callCount is now 2)
memoSquare(4);  // 16 (still cached)
```

**🎯 FP Principle**: Memoization works because pure functions are referentially transparent.

<details><summary>💡 Hint</summary>Use a <code>Map</code> in a closure. Key on <code>JSON.stringify(args)</code> to handle multi-arg functions.</details>

---

### Problem 32: Once (Call-at-Most-Once Wrapper)

Write a function `once(fn)` that ensures `fn` is only called once. Subsequent calls return the first result.

```js
const initialize = once(() => {
  console.log("Initialized!");
  return { status: "ready" };
});

initialize(); // logs "Initialized!", returns { status: "ready" }
initialize(); // returns { status: "ready" } — no log, no re-execution
initialize(); // returns { status: "ready" }
```

**🎯 FP Principle**: Controlling side effects through function wrappers.

<details><summary>💡 Hint</summary>Use a closure to track whether <code>fn</code> has been called and to store the result.</details>

---

### Problem 33: Zip Two Arrays

Write a pure function `zip(arr1, arr2)` that pairs up elements from two arrays. The result length should match the shorter array.

```js
zip([1, 2, 3], ["a", "b", "c"]);     // [[1, "a"], [2, "b"], [3, "c"]]
zip([1, 2], ["a", "b", "c"]);         // [[1, "a"], [2, "b"]]
zip([], [1, 2, 3]);                    // []
```

**🎯 FP Principle**: Declarative combination of data structures.

<details><summary>💡 Hint</summary>Use <code>Array.from({ length: Math.min(...) }, (_, i) => [arr1[i], arr2[i]])</code>.</details>

---

### Problem 34: ZipWith (Generalized Zip)

Write a function `zipWith(fn, arr1, arr2)` that combines two arrays element-wise using a function.

```js
zipWith((a, b) => a + b, [1, 2, 3], [4, 5, 6]);       // [5, 7, 9]
zipWith((a, b) => `${a}:${b}`, ["a", "b"], [1, 2]);    // ["a:1", "b:2"]
zipWith((a, b) => a * b, [2, 3], [4, 5, 6]);            // [8, 15]
```

**🎯 FP Principle**: Abstracting a pattern (zip) with a higher-order function.

<details><summary>💡 Hint</summary>Like <code>zip</code>, but apply <code>fn(arr1[i], arr2[i])</code> at each index.</details>

---

### Problem 35: Unfold (Recursive Generation)

Write a function `unfold(fn, seed)` that builds an array by repeatedly calling `fn(seed)`. The function `fn` should return `[value, nextSeed]` to continue, or `null` to stop.

```js
// Generate [1, 2, 3, 4, 5]
unfold(n => n > 5 ? null : [n, n + 1], 1);  // [1, 2, 3, 4, 5]

// Generate [1, 2, 4, 8, 16]
unfold(n => n > 16 ? null : [n, n * 2], 1); // [1, 2, 4, 8, 16]

// Fibonacci first 8 terms
unfold(
  ([a, b, count]) => count <= 0 ? null : [a, [b, a + b, count - 1]],
  [0, 1, 8]
);
// [0, 1, 1, 2, 3, 5, 8, 13]
```

**🎯 FP Principle**: Unfold is the dual of `reduce` — it builds a list from a seed instead of collapsing a list to a value.

<details><summary>💡 Hint</summary>Recursively call <code>fn(seed)</code>. If it returns <code>null</code>, return <code>[]</code>. Otherwise, return <code>[value, ...unfold(fn, nextSeed)]</code>.</details>

---

### Problem 36: Recursive Object Freeze

Write a pure function `deepFreeze(obj)` that recursively freezes an object and all of its nested objects. Return the frozen object.

```js
const config = deepFreeze({
  db: { host: "localhost", port: 5432 },
  features: ["auth", "logging"]
});

config.db.host = "remote";  // silently fails (or throws in strict mode)
config.db.host;              // "localhost"
config.features.push("new"); // throws TypeError
```

**🎯 FP Principle**: Enforcing immutability at the language level.

<details><summary>💡 Hint</summary>Iterate over <code>Object.keys(obj)</code>, recursively freeze any values that are objects, then call <code>Object.freeze(obj)</code>.</details>

---

### Problem 37: Functional `every` and `some`

Implement `functionalEvery(arr, predicate)` and `functionalSome(arr, predicate)` using recursion. No loops, no built-in `every`/`some`.

```js
functionalEvery([2, 4, 6], n => n % 2 === 0);   // true
functionalEvery([2, 3, 6], n => n % 2 === 0);   // false
functionalEvery([], n => n > 0);                  // true (vacuously true)

functionalSome([1, 3, 5], n => n % 2 === 0);    // false
functionalSome([1, 2, 5], n => n % 2 === 0);    // true
functionalSome([], n => n > 0);                   // false
```

**🎯 FP Principle**: Implementing fundamental predicates recursively.

<details><summary>💡 Hint</summary>For <code>every</code>: base case is empty array → true. If <code>predicate(arr[0])</code> is false, return false. Otherwise, recurse on the rest. <code>some</code> is the dual.</details>

---

### Problem 38: Immutable State Reducer

Write a `reducer(state, action)` function (Redux-style) that handles three action types for a counter. It must be a **pure function** that never mutates state.

```js
const initialState = { count: 0, history: [] };

reducer(initialState, { type: "INCREMENT" });
// { count: 1, history: [1] }

reducer(initialState, { type: "DECREMENT" });
// { count: -1, history: [-1] }

reducer({ count: 5, history: [5] }, { type: "RESET" });
// { count: 0, history: [5, 0] }

reducer(initialState, { type: "UNKNOWN" });
// { count: 0, history: [] }  — returns state unchanged
```

**🎯 FP Principle**: Pure reducers for state management — the foundation of Redux.

<details><summary>💡 Hint</summary>Use a switch on <code>action.type</code>. For each case, return a new object using spread: <code>{ count: newCount, history: [...state.history, newCount] }</code>.</details>

---

### Problem 39: Functional Lookup Table (No `if`/`switch`)

Rewrite the following function to be fully declarative — **no `if`, `else`, `switch`, or ternary operators** allowed. Use an object as a lookup table.

```js
// Imperative version to replace:
function getStatusMessage(code) {
  if (code === 200) return "OK";
  else if (code === 404) return "Not Found";
  else if (code === 500) return "Internal Server Error";
  else return "Unknown Status";
}
```

```js
getStatusMessage(200);  // "OK"
getStatusMessage(404);  // "Not Found"
getStatusMessage(500);  // "Internal Server Error"
getStatusMessage(418);  // "Unknown Status"
```

**🎯 FP Principle**: Data-driven (declarative) branching instead of control-flow branching.

<details><summary>💡 Hint</summary>Define a <code>const statusMessages = { 200: "OK", 404: "Not Found", ... }</code> and return <code>statusMessages[code] ?? "Unknown Status"</code>.</details>

---

### Problem 40: Transducer Intuition — `mapFilter`

Write a function `mapFilter(arr, mapFn, filterFn)` that maps and filters in a **single pass** (one traversal of the array), using `reduce`. Compare its efficiency to chaining `.filter().map()`.

```js
mapFilter(
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
  n => n * n,        // square each number
  n => n % 2 !== 0   // keep only odd numbers
);
// [1, 9, 25, 49, 81]  — filter is applied to the ORIGINAL value, map to the kept values

mapFilter(
  ["hello", "", "world", "", "!"],
  s => s.toUpperCase(),
  s => s.length > 0
);
// ["HELLO", "WORLD", "!"]
```

**🎯 FP Principle**: Transducers reduce multiple passes into one — the idea behind efficient FP data processing.

<details><summary>💡 Hint</summary><code>arr.reduce((acc, item) => filterFn(item) ? [...acc, mapFn(item)] : acc, [])</code></details>

---

## 🌳 Level 3 — Advanced (Problems 41–60)

> **Focus**: Functors and monadic patterns, lenses, transducers, trampolines, algebraic data types, advanced recursion, FP system design

---

### Problem 41: Maybe — Safe Null Handling

Implement a `Maybe` type that wraps a value which may be `null` or `undefined`. It should support `map`, `flatMap`, and `getOrElse`.

```js
const Maybe = {
  of: (value) => { /* ... */ },
  // nothing: () => /* ... */
};

Maybe.of(5).map(x => x * 2).getOrElse(0);         // 10
Maybe.of(null).map(x => x * 2).getOrElse(0);       // 0
Maybe.of("hello").map(s => s.toUpperCase()).getOrElse(""); // "HELLO"

// Chaining through potentially null operations:
const getStreetName = user =>
  Maybe.of(user)
    .flatMap(u => Maybe.of(u.address))
    .flatMap(a => Maybe.of(a.street))
    .getOrElse("Unknown");

getStreetName({ address: { street: "123 Main" } });  // "123 Main"
getStreetName({ address: {} });                        // "Unknown"
getStreetName(null);                                   // "Unknown"
```

**🎯 FP Principle**: The Maybe functor/monad — replacing null checks with composable transformations.

<details><summary>💡 Hint</summary>Create two variants: <code>Just(value)</code> and <code>Nothing()</code>. <code>Just.map(fn)</code> applies <code>fn</code>; <code>Nothing.map(fn)</code> returns <code>Nothing</code>. <code>Maybe.of(x)</code> returns <code>Nothing</code> if x is null/undefined, otherwise <code>Just(x)</code>.</details>

---

### Problem 42: Either — Error Handling Without Exceptions

Implement an `Either` type with `Left` (error) and `Right` (success). It should support `map`, `flatMap`, and `fold`.

```js
const divide = (a, b) =>
  b === 0 ? Either.left("Division by zero") : Either.right(a / b);

divide(10, 2).map(x => x + 1).fold(
  err => `Error: ${err}`,
  val => `Result: ${val}`
);
// "Result: 6"

divide(10, 0).map(x => x + 1).fold(
  err => `Error: ${err}`,
  val => `Result: ${val}`
);
// "Error: Division by zero"

// Chaining Either operations:
const parseNumber = str => {
  const n = Number(str);
  return isNaN(n) ? Either.left(`"${str}" is not a number`) : Either.right(n);
};

parseNumber("42")
  .flatMap(n => divide(100, n))
  .map(result => result.toFixed(2))
  .fold(err => err, val => val);
// "2.38"

parseNumber("abc")
  .flatMap(n => divide(100, n))
  .fold(err => err, val => val);
// "\"abc\" is not a number"
```

**🎯 FP Principle**: The Either monad — composable error handling without `try`/`catch`.

<details><summary>💡 Hint</summary><code>Right.map(fn)</code> applies <code>fn</code> to the value; <code>Left.map(fn)</code> ignores <code>fn</code> and passes the error along. <code>fold(leftFn, rightFn)</code> unwraps the value.</details>

---

### Problem 43: Lenses — Focused Immutable Updates

Implement a `lens(getter, setter)` function and helpers `view`, `set`, and `over`. A lens focuses on a specific part of a data structure for reading and immutable updating.

```js
// Define a lens for the 'name' property
const nameLens = lens(
  obj => obj.name,
  (obj, value) => ({ ...obj, name: value })
);

const user = { name: "Alice", age: 25 };

view(nameLens, user);                      // "Alice"
set(nameLens, "Bob", user);                // { name: "Bob", age: 25 }
over(nameLens, s => s.toUpperCase(), user); // { name: "ALICE", age: 25 }

// Original unchanged:
console.log(user.name);  // "Alice"

// Composable lenses for nested data:
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
set(addressCityLens, "LA", person);            // { name: "Alice", address: { city: "LA", zip: "10001" } }
```

**🎯 FP Principle**: Lenses provide composable, reusable accessors for immutable data.

<details><summary>💡 Hint</summary><code>view(lens, obj) = lens.getter(obj)</code>. <code>set(lens, val, obj) = lens.setter(obj, val)</code>. <code>over(lens, fn, obj) = set(lens, fn(view(lens, obj)), obj)</code>. Compose by nesting: <code>view(composed, obj) = view(inner, view(outer, obj))</code>.</details>

---

### Problem 44: Trampoline — Stack-Safe Recursion

Write a `trampoline(fn)` function that makes recursive functions stack-safe by using thunks (lazy evaluation) instead of actual call stack frames.

```js
// This would blow the stack with normal recursion for large n:
const unsafeSum = (n, acc = 0) =>
  n <= 0 ? acc : unsafeSum(n - 1, acc + n);

// Trampolined version:
const safeSum = trampoline(function sum(n, acc = 0) {
  if (n <= 0) return acc;
  return () => sum(n - 1, acc + n);  // return a thunk instead of recursing
});

safeSum(100000);  // 5000050000 — no stack overflow!
safeSum(0);       // 0
safeSum(10);      // 55
```

**🎯 FP Principle**: Trampolining enables functional recursion at any depth without stack overflow.

<details><summary>💡 Hint</summary><code>trampoline</code> runs a loop: call the function, and if the result is a function (thunk), call it again. Repeat until the result is not a function.</details>

---

### Problem 45: Lazy Evaluation with Generators

Create a lazy evaluation system using generators. Implement `lazyMap`, `lazyFilter`, `lazyTake`, and `lazyReduce` that process elements one at a time.

```js
function* naturals() {
  let n = 1;
  while (true) yield n++;
}

// This would be impossible with eager arrays (infinite sequence!):
const result = lazyReduce(
  lazyTake(
    lazyMap(
      lazyFilter(naturals(), n => n % 2 === 0),  // even numbers
      n => n * n                                    // square them
    ),
    5                                              // take first 5
  ),
  (acc, n) => acc + n,
  0
);

console.log(result);  // 4 + 16 + 36 + 64 + 100 = 220

// Equivalently, compose them:
const first5EvenSquares = pipe(
  gen => lazyFilter(gen, n => n % 2 === 0),
  gen => lazyMap(gen, n => n * n),
  gen => lazyTake(gen, 5),
  gen => [...gen]
)(naturals());

console.log(first5EvenSquares);  // [4, 16, 36, 64, 100]
```

**🎯 FP Principle**: Lazy evaluation — compute values only when needed, enabling infinite data structures.

<details><summary>💡 Hint</summary>Each function is a generator function: <code>function* lazyMap(gen, fn) { for (const x of gen) yield fn(x); }</code></details>

---

### Problem 46: Immutable Linked List

Implement a persistent (immutable) linked list with `cons`, `head`, `tail`, `isEmpty`, `map`, `filter`, `reduce`, and `toArray`.

```js
const empty = List.empty();
const list = List.of(1, 2, 3, 4, 5);  // builds from an array

head(list);       // 1
toArray(tail(list)); // [2, 3, 4, 5]

const newList = cons(0, list);
toArray(newList); // [0, 1, 2, 3, 4, 5]
toArray(list);    // [1, 2, 3, 4, 5] — unchanged!

toArray(listMap(list, x => x * 2));         // [2, 4, 6, 8, 10]
toArray(listFilter(list, x => x > 2));      // [3, 4, 5]
listReduce(list, (acc, x) => acc + x, 0);   // 15
```

**🎯 FP Principle**: Persistent data structures — the old version is always preserved.

<details><summary>💡 Hint</summary>A list node is <code>{ value, next }</code> or <code>null</code> for empty. <code>cons(value, list)</code> returns <code>{ value, next: list }</code> — this is O(1) and shares the tail.</details>

---

### Problem 47: Church Encoding — Booleans and Numbers

Implement booleans and natural numbers using only **functions** (no JavaScript primitives). This is Church encoding.

```js
// Church Booleans
const TRUE = (a) => (_b) => a;
const FALSE = (_a) => (b) => b;
const IF = (condition) => (then) => (else_) => condition(then)(else_);
const AND = (a) => (b) => a(b)(FALSE);
const OR = (a) => (b) => a(TRUE)(b);
const NOT = (a) => a(FALSE)(TRUE);

// Convert back to JS for testing:
const toBool = (churchBool) => churchBool(true)(false);

toBool(TRUE);              // true
toBool(FALSE);             // false
toBool(AND(TRUE)(TRUE));   // true
toBool(AND(TRUE)(FALSE));  // false
toBool(OR(FALSE)(TRUE));   // true
toBool(NOT(TRUE));         // false

// Church Numerals
const ZERO = (f) => (x) => x;
const ONE = (f) => (x) => f(x);
const TWO = (f) => (x) => f(f(x));
const SUCC = (n) => (f) => (x) => f(n(f)(x));
const ADD = (m) => (n) => (f) => (x) => m(f)(n(f)(x));
const MULT = (m) => (n) => (f) => m(n(f));

const toNum = (churchNum) => churchNum(x => x + 1)(0);

toNum(ZERO);             // 0
toNum(ONE);              // 1
toNum(SUCC(TWO));        // 3
toNum(ADD(TWO)(TWO));    // 4
toNum(MULT(TWO)(SUCC(TWO))); // 6
```

**🎯 FP Principle**: Everything can be represented as functions — the theoretical foundation of lambda calculus.

<details><summary>💡 Hint</summary>A Church numeral <code>n</code> takes a function <code>f</code> and applies it <code>n</code> times. <code>SUCC</code> adds one more application. <code>ADD</code> chains applications.</details>

---

### Problem 48: Transducers

Implement a transducer system. A transducer is a composable, reusable transformation that is independent of the input/output collection type.

```js
// Transducer-creating functions:
const tMap = (fn) => (reducer) => (acc, item) => reducer(acc, fn(item));
const tFilter = (pred) => (reducer) => (acc, item) => pred(item) ? reducer(acc, item) : acc;
const tTake = (n) => { /* ... */ };

// Compose transducers (they compose left-to-right!):
const xform = compose(
  tFilter(n => n % 2 !== 0),  // keep odd
  tMap(n => n * 10),           // multiply by 10
  tTake(3)                     // take first 3
);

// Apply to different collection types with the SAME transducer:
const arrayReducer = (acc, item) => [...acc, item];
const sumReducer = (acc, item) => acc + item;

transduce(xform, arrayReducer, [], [1, 2, 3, 4, 5, 6, 7, 8, 9]);
// [10, 30, 50]

transduce(xform, sumReducer, 0, [1, 2, 3, 4, 5, 6, 7, 8, 9]);
// 90
```

**🎯 FP Principle**: Transducers decouple transformation logic from data structure traversal.

<details><summary>💡 Hint</summary>A transducer takes a reducer and returns a new reducer. <code>compose</code> chains them. <code>transduce(xform, reducer, init, coll)</code> calls <code>coll.reduce(xform(reducer), init)</code>.</details>

---

### Problem 49: Recursive Descent Expression Parser (Pure)

Write a pure expression parser that evaluates mathematical expressions. No mutation of external state — use an index passed through the recursive calls.

```js
evaluate("2 + 3");          // 5
evaluate("2 + 3 * 4");      // 14  (respects operator precedence)
evaluate("(2 + 3) * 4");    // 20
evaluate("10 - 2 * 3 + 1"); // 5
evaluate("((1 + 2) * (3 + 4))"); // 21
```

The parser should handle: `+`, `-`, `*`, `/`, parentheses, and integer/float literals.

**🎯 FP Principle**: Pure recursion for parsing — each function returns `[result, remainingIndex]` without mutating shared state.

<details><summary>💡 Hint</summary>Write three mutually recursive functions: <code>parseExpression</code> (handles +/-), <code>parseTerm</code> (handles *//), <code>parseFactor</code> (handles numbers and parentheses). Each returns <code>[value, newIndex]</code>.</details>

---

### Problem 50: Functional State — State Monad

Implement a `State` monad that threads state through a computation without explicit mutation.

```js
// State monad: a computation is a function (state) => [value, newState]
const State = {
  of: (value) => (state) => [value, state],
  get: () => (state) => [state, state],
  put: (newState) => (_state) => [undefined, newState],
  modify: (fn) => (state) => [undefined, fn(state)],
};

// Chain computations:
const chain = (stateM, fn) => (state) => {
  const [value, newState] = stateM(state);
  return fn(value)(newState);
};

// Example: Stack operations as pure state transformations
const push = (value) => State.modify(stack => [value, ...stack]);
const pop = () => chain(State.get(), (stack) =>
  chain(State.put(stack.slice(1)), () =>
    State.of(stack[0])
  )
);

const program = chain(push(1), () =>
  chain(push(2), () =>
    chain(push(3), () =>
      chain(pop(), (top) =>
        chain(pop(), (second) =>
          State.of({ popped: [top, second] })
        )
      )
    )
  )
);

program([]);
// [{ popped: [3, 2] }, [1]]
```

**🎯 FP Principle**: The State monad makes stateful computation pure by threading state explicitly.

<details><summary>💡 Hint</summary>A State value is a function <code>s => [a, s']</code>. <code>chain</code> runs the first computation, passes the result to a function that returns the next computation, and threads the state.</details>

---

### Problem 51: Algebraic Data Types — Pattern Matching

Implement a simple pattern matching system and use it to define a recursive `Tree` type with `Leaf` and `Branch` variants.

```js
// Define the Tree type
const Leaf = (value) => ({ tag: "Leaf", value });
const Branch = (left, right) => ({ tag: "Branch", left, right });

// Pattern matching helper
const match = (value, patterns) => patterns[value.tag](value);

// Build a tree
const tree = Branch(
  Branch(Leaf(1), Leaf(2)),
  Branch(Leaf(3), Leaf(4))
);

// Sum all leaf values using pattern matching
const sumTree = (tree) => match(tree, {
  Leaf: ({ value }) => value,
  Branch: ({ left, right }) => sumTree(left) + sumTree(right)
});

sumTree(tree);  // 10

// Map over a tree
const mapTree = (tree, fn) => match(tree, {
  Leaf: ({ value }) => Leaf(fn(value)),
  Branch: ({ left, right }) => Branch(mapTree(left, fn), mapTree(right, fn))
});

const doubled = mapTree(tree, x => x * 2);
sumTree(doubled);  // 20

// Count leaves
const countLeaves = (tree) => match(tree, {
  Leaf: () => 1,
  Branch: ({ left, right }) => countLeaves(left) + countLeaves(right)
});

countLeaves(tree);  // 4
```

**🎯 FP Principle**: Algebraic data types and pattern matching — the backbone of typed FP.

<details><summary>💡 Hint</summary>Each variant is a factory function that creates a tagged object. <code>match</code> dispatches on the tag.</details>

---

### Problem 52: Free Monad — Declarative Instruction Set

Build a "Free Monad" pattern that separates *describing* computations from *executing* them. Create a mini DSL for key-value store operations.

```js
// Define instructions (data, not behavior):
const Get = (key, next) => ({ tag: "Get", key, next });
const Put = (key, value, next) => ({ tag: "Put", key, value, next });
const End = (value) => ({ tag: "End", value });

// DSL helper: chain operations
const chain = (instruction, fn) => { /* ... */ };

// Write a program as data:
const program =
  Put("name", "Alice",
    Put("age", 25,
      Get("name", (name) =>
        Get("age", (age) =>
          End({ name, age })
        )
      )
    )
  );

// Interpreter 1: In-memory object store
const runInMemory = (program, store = {}) => {
  switch (program.tag) {
    case "Put": return runInMemory(program.next, { ...store, [program.key]: program.value });
    case "Get": return runInMemory(program.next(store[program.key]), store);
    case "End": return program.value;
  }
};

runInMemory(program);  // { name: "Alice", age: 25 }

// Interpreter 2: Logging interpreter (logs every operation)
const runWithLogging = (program, store = {}, log = []) => { /* ... */ };
```

**🎯 FP Principle**: Separating description from execution — programs as data structures that can be interpreted differently.

<details><summary>💡 Hint</summary>Each instruction is just data. The interpreter pattern-matches on the tag and decides what to do. Different interpreters can run the same program differently (in-memory, with logging, with actual I/O).</details>

---

### Problem 53: Functional Reactive Programming — Observable

Build a minimal FRP `Observable` with pure operators. Every operator returns a new Observable without mutating the original.

```js
const Observable = (subscribe) => ({
  subscribe,
  map: (fn) => Observable(observer =>
    subscribe({ next: val => observer.next(fn(val)), complete: () => observer.complete() })
  ),
  filter: (pred) => Observable(observer =>
    subscribe({ next: val => pred(val) && observer.next(val), complete: () => observer.complete() })
  ),
  scan: (reducer, seed) => { /* accumulate like reduce but emit each intermediate value */ },
  take: (n) => { /* emit only the first n values then complete */ },
  merge: (other) => /* merge two observables into one */
});

// Usage:
const clicks = Observable(observer => {
  let i = 0;
  const id = setInterval(() => {
    if (i >= 10) { clearInterval(id); observer.complete(); }
    else observer.next(i++);
  }, 100);
});

clicks
  .filter(n => n % 2 === 0)
  .map(n => n * 10)
  .take(3)
  .subscribe({
    next: val => console.log(val),     // 0, 20, 40
    complete: () => console.log("Done")
  });
```

**🎯 FP Principle**: FRP treats events as composable streams, applying FP transformations to values over time.

<details><summary>💡 Hint</summary>Each operator returns a new <code>Observable</code> whose subscribe wraps the original observer with transformation logic. <code>scan</code> maintains an accumulator in the closure. <code>take</code> counts emissions and calls <code>complete</code> after <code>n</code>.</details>

---

### Problem 54: Hindley-Milner Type Checker (Simplified)

Build a simplified type inference engine for a tiny expression language. Implement the core `unify` and `infer` functions.

```js
// Expression AST
const Num = (n) => ({ tag: "Num", n });
const Bool = (b) => ({ tag: "Bool", b });
const Var = (name) => ({ tag: "Var", name });
const App = (fn, arg) => ({ tag: "App", fn, arg });
const Lam = (param, body) => ({ tag: "Lam", param, body });
const Let = (name, value, body) => ({ tag: "Let", name, value, body });

// Type AST
const TNum = { tag: "TNum" };
const TBool = { tag: "TBool" };
const TVar = (name) => ({ tag: "TVar", name });
const TFun = (from, to) => ({ tag: "TFun", from, to });

// Infer types:
infer({}, Num(42));                     // TNum
infer({}, Bool(true));                  // TBool
infer({}, Lam("x", Var("x")));         // TFun(TVar("a"), TVar("a"))  — identity is polymorphic
infer({}, App(Lam("x", Var("x")), Num(5)));  // TNum
```

**🎯 FP Principle**: Type inference is a purely functional algorithm — it demonstrates unification, substitution, and recursive tree traversal.

<details><summary>💡 Hint</summary><code>infer</code> returns a type and a set of substitutions. <code>unify(t1, t2)</code> returns a substitution that makes the two types equal. Use a counter for generating fresh type variables.</details>

---

### Problem 55: Zipper — Navigable Immutable Trees

Implement a Zipper for a binary tree. A Zipper lets you "focus" on a node and navigate/update it efficiently while keeping everything immutable.

```js
const tree = Branch(
  Branch(Leaf(1), Leaf(2)),
  Branch(Leaf(3), Leaf(4))
);

const z = Zipper.fromTree(tree);

// Navigate
const z = z.goLeft();           // focus on left child
const z = z.goLeft();          // focus on Leaf(1)

// Read
z.value();                       // 1

// Update (immutably!)
const z = z.update(x => x * 10); // Leaf(1) → Leaf(10)

// Navigate back up and reconstruct
const newTree = z.goUp().goUp().toTree();
// Branch(Branch(Leaf(10), Leaf(2)), Branch(Leaf(3), Leaf(4)))

// Original tree is unchanged:
sumTree(tree);    // 10
sumTree(newTree);  // 19
```

**🎯 FP Principle**: Zippers provide efficient, focused navigation of immutable trees by remembering the "context" (the path taken).

<details><summary>💡 Hint</summary>A zipper is <code>{ focus, context }</code>. The context is a stack of "breadcrumbs" — each breadcrumb records which direction you went and the sibling subtree. <code>goUp</code> reconstructs the parent from the breadcrumb.</details>

---

### Problem 56: Functional Optics — Prisms

Extend the lens system from Problem 43 with **Prisms** — optics for working with sum types (values that might be one of several variants).

```js
// A Prism focuses on one variant of a sum type
const stringToNumber = prism(
  s => { const n = Number(s); return isNaN(n) ? null : n; },  // preview: String → Maybe<Number>
  n => String(n)                                                 // review: Number → String
);

preview(stringToNumber, "42");    // 42
preview(stringToNumber, "hello"); // null
review(stringToNumber, 42);       // "42"

// Composing Prism with Lens:
const data = { value: "100" };
const valueLens = lens(obj => obj.value, (obj, v) => ({ ...obj, value: v }));

// Compose lens + prism to reach into an object, parse the string as number,
// transform it, and put it back as a string:
const composed = composeLensPrism(valueLens, stringToNumber);

over(composed, n => n * 2, data);  // { value: "200" }
over(composed, n => n * 2, { value: "abc" });  // { value: "abc" } — no change, prism didn't match
```

**🎯 FP Principle**: Prisms are the dual of lenses — lenses focus on product types (records), prisms focus on sum types (variants/enums).

<details><summary>💡 Hint</summary>A prism has <code>preview: s → a | null</code> and <code>review: a → s</code>. <code>over</code> with a prism applies the function only when <code>preview</code> succeeds.</details>

---

### Problem 57: Purely Functional Red-Black Tree

Implement an immutable Red-Black Tree (self-balancing BST) with `insert`, `search`, `toSortedArray`, and `fromArray`.

```js
const tree = RBTree.fromArray([5, 3, 7, 1, 4, 6, 8, 2]);

RBTree.search(tree, 4);     // true
RBTree.search(tree, 9);     // false
RBTree.toSortedArray(tree);  // [1, 2, 3, 4, 5, 6, 7, 8]

// Insert returns a new tree — original is unchanged:
const tree2 = RBTree.insert(tree, 9);
RBTree.search(tree2, 9);    // true
RBTree.search(tree, 9);     // false — original unchanged
RBTree.toSortedArray(tree2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// The tree is always balanced:
RBTree.height(tree2);        // ≤ 2 * log2(9) ≈ 6
```

**🎯 FP Principle**: Persistent balanced trees — efficient immutable data structures shared via structural sharing.

<details><summary>💡 Hint</summary>Follow Chris Okasaki's functional Red-Black Tree algorithm: after inserting, pattern-match on four "unbalanced" cases and rotate. All operations return new nodes; untouched subtrees are shared.</details>

---

### Problem 58: Monad Transformers — MaybeT

Implement `MaybeT`, a monad transformer that adds "nullable" behavior on top of another monad (like `Promise`).

```js
// MaybeT wraps Promise<Maybe<A>>

const fetchUser = (id) =>
  MaybeT.of(
    id > 0
      ? Promise.resolve({ id, name: "Alice", addressId: 42 })
      : Promise.resolve(null)
  );

const fetchAddress = (addressId) =>
  MaybeT.of(
    addressId === 42
      ? Promise.resolve({ city: "NYC", zip: "10001" })
      : Promise.resolve(null)
  );

// Chain async + nullable operations cleanly:
const getCity = (userId) =>
  fetchUser(userId)
    .flatMap(user => fetchAddress(user.addressId))
    .map(address => address.city);

await getCity(1).run();   // "NYC"
await getCity(-1).run();  // null  (user not found, short-circuits)
await getCity(2).run();   // null  (address not found)
```

**🎯 FP Principle**: Monad transformers compose multiple effects (async + nullable) into a single, clean pipeline.

<details><summary>💡 Hint</summary><code>MaybeT.of(promise)</code> wraps a <code>Promise&lt;A | null&gt;</code>. <code>flatMap(fn)</code> does <code>this.promise.then(val => val == null ? null : fn(val).run())</code>.</details>

---

### Problem 59: CPS (Continuation-Passing Style) Transform

Convert these direct-style functions into Continuation-Passing Style (CPS), then implement a `callCC` (call-with-current-continuation) for early exit.

```js
// Direct style:
const factorial = (n) => n <= 1 ? 1 : n * factorial(n - 1);

// CPS version:
const factorialCPS = (n, k) => {
  // k is the continuation — "what to do next with the result"
};

factorialCPS(5, result => console.log(result));  // 120

// Direct style:
const fibonacci = (n) => n <= 1 ? n : fibonacci(n - 1) + fibonacci(n - 2);

// CPS version:
const fibonacciCPS = (n, k) => { /* ... */ };

fibonacciCPS(10, result => console.log(result));  // 55

// callCC — lets you "escape" from a computation early:
const findFirstNegative = (arr, k) =>
  callCC(exit => {
    arr.forEach(n => {
      if (n < 0) exit(n);  // immediately returns n, skipping the rest
    });
    return null;  // no negative found
  }, k);

findFirstNegative([1, 2, -3, 4], result => console.log(result));  // -3
findFirstNegative([1, 2, 3, 4], result => console.log(result));    // null
```

**🎯 FP Principle**: CPS makes control flow explicit — every function explicitly says what to do next, enabling advanced control flow like backtracking and coroutines.

<details><summary>💡 Hint</summary>In CPS, instead of returning a value, you call the continuation <code>k</code> with the result. For <code>factorialCPS</code>: base case calls <code>k(1)</code>; recursive case calls <code>factorialCPS(n-1, result => k(n * result))</code>.</details>

---

### Problem 60: Build a Functional Programming Language Interpreter

Build an interpreter for a small functional programming language that supports:
- Lambda expressions
- Let bindings
- Recursive definitions (`letrec`)
- Pattern matching
- Algebraic data types
- Closures with lexical scoping

```js
const program = `
  let compose = \\f -> \\g -> \\x -> f (g x) in
  let double = \\x -> x + x in
  let inc = \\x -> x + 1 in
  let doubleThenInc = compose inc double in
  doubleThenInc 5
`;

interpret(program);  // 11

const listProgram = `
  letrec map = \\f -> \\xs ->
    match xs with
    | Nil -> Nil
    | Cons x rest -> Cons (f x) (map f rest)
  in
  let double = \\x -> x + x in
  map double (Cons 1 (Cons 2 (Cons 3 Nil)))
`;

interpret(listProgram);  // Cons 2 (Cons 4 (Cons 6 Nil))
```

The interpreter should include:
1. **Lexer** — tokenizes the source string
2. **Parser** — builds an AST
3. **Evaluator** — walks the AST with an environment for variable bindings

**🎯 FP Principle**: A pure interpreter is itself a study in functional programming — environments as immutable maps, closures capturing environments, and recursion for evaluation.

<details><summary>💡 Hint</summary>Start small: first support integers and addition. Then add lambdas (closures = <code>{ params, body, env }</code>). Then add <code>let</code> (extend the environment). Then <code>letrec</code> (use a fixed-point combinator or mutable ref). Then ADTs and pattern matching (tagged objects + recursive destructuring).</details>

---

## 🗺️ FP Concept Reference Matrix

| FP Concept | Problems |
|:-----------|:---------|
| Pure Functions | 1, 2, 15, 19 |
| Immutability | 3, 4, 5, 11, 16, 19, 36, 38 |
| `map` / `filter` / `reduce` | 6, 7, 8, 9, 10, 17, 18, 20, 29, 40 |
| Higher-Order Functions | 12, 13, 14, 31, 32 |
| Function Composition | 21, 22, 23, 30 |
| Currying & Partial Application | 24, 25 |
| Recursion | 26, 27, 28, 35, 37, 44 |
| Closures in FP | 13, 14, 31, 32 |
| Declarative Style | 6, 7, 20, 39, 40 |
| Functor / Monad | 41, 42, 50, 58 |
| Lenses & Optics | 43, 56 |
| Lazy Evaluation | 45 |
| Persistent Data Structures | 46, 57 |
| Lambda Calculus | 47 |
| Transducers | 48 |
| Pattern Matching & ADTs | 51, 60 |
| CPS & Trampolines | 44, 59 |
| Parsing | 49 |
| FRP (Reactive) | 53 |
| Free Monad | 52 |
| Type Inference | 54 |
| Zippers | 55 |
| Interpreters | 60 |

---

## 📝 How to Use This Problem Set

1. **Start at Level 1** even if you know JavaScript well. FP thinking is different from imperative thinking, and the beginner problems rewire your instincts.
2. **Follow the constraint**: Every function you write should be **pure** unless the problem explicitly asks for side effects. If you find yourself using `let`, a `for` loop, or `push`, pause and rethink.
3. **Test immutability**: After every problem, verify that input data was **not mutated**. Add assertions like `console.assert(originalArray.length === 3)`.
4. **Compose solutions**: Once you build small functions, reuse them. Problems 21–23 and 30 are designed to make you combine earlier solutions.
5. **Read the FP Principle tag**: Every problem has a `🎯 FP Principle` tag telling you exactly which concept to focus on.
6. **Progressive difficulty**:
   - **Level 1** (Problems 1–20): Should take 5–10 minutes each
   - **Level 2** (Problems 21–40): Should take 15–30 minutes each
   - **Level 3** (Problems 41–60): Should take 30–90 minutes each (some may take a full afternoon)

> **Tip**: Create a separate `.js` file for each problem (e.g., `fp-01-pure-addition.js`) in this folder to keep your solutions organized.

Happy functional programming! 🎉
