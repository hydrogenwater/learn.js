# 🔍 Problem-Solving with Functional Programming — Solutions Guide

> **Purpose**: Annotated solutions for all 160 problems (100 JS + 60 FP), organized by theme. Each solution shows the **functional approach**, explains which FP principles apply, discusses trade-offs, and cross-references the learning guides.
>
> **How to use**: Find the problem by theme or use Ctrl+F to search by problem number (e.g., "JS #42" or "FP #24"). Try solving each problem yourself first — then compare your approach with these notes.
>
> **Legend**:
> - 🟢 = Beginner | 🟡 = Intermediate | 🔴 = Advanced
> - **JS #N** = [02-javascript-coding-problems.md](./02-javascript-coding-problems.md)
> - **FP #N** = [04-functional-programming-problems.md](./04-functional-programming-problems.md)
> - **→ JS Found §N** = [01-javascript-foundations.md](./01-javascript-foundations.md)
> - **→ FP Guide §N** = [03-functional-programming-guide.md](./03-functional-programming-guide.md)

---

## Table of Contents

1. [Pure Functions & Side Effects](#1-pure-functions--side-effects)
2. [Immutability](#2-immutability)
3. [Transformation with map/filter/reduce](#3-transformation-with-mapfilterreduce)
4. [Strings & Characters](#4-strings--characters)
5. [Higher-Order Functions & Closures](#5-higher-order-functions--closures)
6. [Composition & Pipelines](#6-composition--pipelines)
7. [Currying & Partial Application](#7-currying--partial-application)
8. [Recursion](#8-recursion)
9. [Objects & Data Modeling](#9-objects--data-modeling)
10. [Control Flow & Logic](#10-control-flow--logic)
11. [Functional Data Structures](#11-functional-data-structures)
12. [Monads & Algebraic Types](#12-monads--algebraic-types)
13. [Advanced FP Techniques](#13-advanced-fp-techniques)
14. [Async & Promises Through an FP Lens](#14-async--promises-through-an-fp-lens)
15. [System Design with FP Influence](#15-system-design-with-fp-influence)

---

## 1. Pure Functions & Side Effects

**→ FP Guide §2 (Pure Functions)**

### 🟢 FP #1: Pure Addition

```js
const add = (a, b) => a + b;
```

**FP Alignment**: This is the simplest possible pure function — deterministic, no side effects. Note: `console.log` inside would make it impure. The function relies on nothing outside its parameters.

---

### 🟢 FP #2: Impure to Pure

```js
// ❌ Impure — depends on external `taxRate`
let taxRate = 0.2;
function calculateTotal(price) {
  return price + price * taxRate;
}

// ✅ Pure — all dependencies are parameters
const calculateTotal = (price, taxRate) => price + price * taxRate;
// OR more declaratively:
const calculateTotal = (price, taxRate) => price * (1 + taxRate);
```

**FP Alignment**: The key insight is **referential transparency** — the result depends solely on the inputs. You can call `calculateTotal(100, 0.2)` anywhere, anytime, and always get `120`.

---

### 🟢 FP #15: Pure String Transformer

```js
const capitalize = (str) =>
  str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();

const formatName = (first, last) =>
  `${capitalize(first)} ${capitalize(last)}`;
```

**FP Alignment**: Pure function with a helper extracted as a reusable pure function. Notice `capitalize` is also pure — string methods in JS always return new strings.

---

### 🟢 JS #1: Hello, World!

```js
console.log("Hello, World!");
```

**FP Perspective**: `console.log` is a side effect (I/O). In a pure FP program, you'd return the string and let the "impure shell" handle output. But this is the very first problem — pragmatism wins.

---

### 🟢 JS #4: Data Type Checker

```js
const checkType = (value) => typeof value;
```

**FP Alignment**: A pure function — `typeof` is a deterministic operator with no side effects. Note the `null` quirk: `typeof null === "object"` is a historic JS bug.

---

### 🟢 JS #5: Temperature Converter

```js
const celsiusToFahrenheit = (c) => (c * 9/5) + 32;
const fahrenheitToCelsius = (f) => (f - 32) * 5/9;
```

**FP Alignment**: Pure mathematical functions. These are also **inverses** of each other — a concept from algebra that maps nicely to FP's emphasis on mathematical properties: `fahrenheitToCelsius(celsiusToFahrenheit(x)) === x`.

---

### 🟢 JS #8: Odd or Even

```js
// Imperative
function oddOrEven(num) {
  if (num % 2 === 0) return "Even";
  else return "Odd";
}

// FP — expression-based, no statements
const oddOrEven = (num) => num % 2 === 0 ? "Even" : "Odd";
```

**FP Alignment**: The ternary version is an expression (returns a value) rather than a statement (performs an action). FP favors expressions.

---

### 🟢 JS #9: Simple Interest Calculator

```js
const simpleInterest = (principal, rate, time) => (principal * rate * time) / 100;
```

**FP Alignment**: Pure mathematical function — all inputs are parameters, output is deterministic.

---

### 🟢 JS #10: Max of Three

```js
// Imperative
function maxOfThree(a, b, c) {
  if (a >= b && a >= c) return a;
  if (b >= a && b >= c) return b;
  return c;
}

// FP — reduce over the arguments
const maxOfThree = (a, b, c) => [a, b, c].reduce((max, n) => n > max ? n : max);

// FP — composable approach (generalizes to any number)
const max = (...nums) => nums.reduce((a, b) => a > b ? a : b);
```

**FP Alignment**: The `reduce` version is declarative and generalizes naturally. The variadic `max` function is reusable for any number of arguments.

---

## 2. Immutability

**→ FP Guide §3 (Immutability)**

### 🟢 FP #3: Immutable Object Update

```js
const updateAge = (person, newAge) => ({ ...person, age: newAge });
```

**FP Alignment**: Spread creates a new object. The original is never touched. This is the foundational pattern for all immutable updates.

---

### 🟢 FP #4: Immutable Array Push

```js
const appendItem = (arr, item) => [...arr, item];
```

**FP Alignment**: `[...arr, item]` creates a new array. Never use `.push()` in FP code — it mutates.

---

### 🟢 FP #5: Immutable Array Remove

```js
const removeItem = (arr, index) => arr.filter((_, i) => i !== index);
// OR
const removeItem = (arr, index) => [...arr.slice(0, index), ...arr.slice(index + 1)];
```

**FP Alignment**: Both approaches create new arrays. The `filter` version is more declarative; the `slice` version is more explicit about what's happening.

---

### 🟢 FP #11: Immutable Nested Object Update

```js
const updateCity = (user, newCity) => ({
  ...user,
  address: {
    ...user.address,
    city: newCity
  }
});
```

**FP Alignment**: Deep immutability requires spreading at every level. This is verbose — it's exactly the problem that **lenses** solve (→ FP Guide §13).

---

### 🟢 FP #16: Immutable Array Update at Index

```js
const updateAt = (arr, index, value) =>
  arr.map((item, i) => i === index ? value : item);
```

**FP Alignment**: `map` is the FP way to "update" an array — it transforms every element, replacing only the one at the target index.

---

### 🟢 FP #19: Avoid Mutation Trap

```js
// ❌ Buggy — mutates cart
function addToCart(cart, item) {
  cart.items.push(item);
  cart.total += item.price;
  return cart;
}

// ✅ Pure — returns new cart
const addToCart = (cart, item) => ({
  items: [...cart.items, item],
  total: cart.total + item.price
});
```

**FP Alignment**: Real-world data structures like shopping carts are where immutability matters most. The buggy version has a hidden side effect — callers may not realize their cart was mutated.

---

### 🟡 FP #36: Recursive Object Freeze

```js
const deepFreeze = (obj) => {
  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === "object" && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return Object.freeze(obj);
};
```

**FP Alignment**: Enforces immutability at the language level. Note that `Object.freeze` is shallow — this recursive version handles nested objects. Arrays are objects too, so this freezes them as well.

**Trade-off**: `deepFreeze` uses `forEach` (a side-effectful iteration), which is pragmatic here since the purpose is a one-time setup, not a data transformation pipeline.

---

### 🟡 JS #41: Deep Clone an Object

```js
// FP approach — recursive pure function
const deepClone = (obj) => {
  if (obj === null || typeof obj !== "object") return obj;
  if (Array.isArray(obj)) return obj.map(deepClone);
  return Object.fromEntries(
    Object.entries(obj).map(([key, val]) => [key, deepClone(val)])
  );
};
```

**FP Alignment**: Uses `map` and `Object.entries`/`fromEntries` for declarative transformation. Each level is recursively cloned — a pure function that returns a completely new structure.

**Alternative**: `structuredClone(obj)` is the modern built-in (Node 17+, all modern browsers).

---

### 🟡 JS #77: Deep Freeze

```js
const deepFreeze = (obj) => {
  if (obj === null || typeof obj !== "object") return obj;
  Object.values(obj).forEach(val => deepFreeze(val));
  return Object.freeze(obj);
};
```

Same pattern as FP #36. → FP Guide §3.

---

## 3. Transformation with map/filter/reduce

**→ FP Guide §6 (The FP Trinity)**

### 🟢 FP #6: Transform with `map`

```js
const doubleAll = (numbers) => numbers.map(n => n * 2);
```

---

### 🟢 FP #7: Filter with a Predicate

```js
const getEvens = (numbers) => numbers.filter(n => n % 2 === 0);
```

---

### 🟢 FP #8: Reduce to a Single Value

```js
const sum = (numbers) => numbers.reduce((acc, n) => acc + n, 0);
```

---

### 🟢 FP #9: Transform Objects in an Array

```js
const getNames = (people) => people.map(person => person.name);
// Point-free alternative with a property accessor:
// const prop = (key) => (obj) => obj[key];
// const getNames = (people) => people.map(prop("name"));
```

---

### 🟢 FP #10: Chain `map` and `filter`

```js
const getAdultNames = (people) =>
  people
    .filter(p => p.age >= 18)
    .map(p => p.name);
```

**FP Alignment**: This is a **data pipeline** — filter first (select), then map (transform). Order matters: filtering before mapping is more efficient (fewer elements to transform).

---

### 🟢 FP #17: Count with `reduce`

```js
const countOccurrences = (arr, target) =>
  arr.reduce((count, item) => item === target ? count + 1 : count, 0);
```

---

### 🟢 FP #18: Build Object with `reduce`

```js
const arrayToObject = (pairs) =>
  pairs.reduce((obj, [key, value]) => ({ ...obj, [key]: value }), {});

// More performant for large arrays (avoids spreading on every iteration):
const arrayToObject = (pairs) => Object.fromEntries(pairs);
```

**Trade-off**: The `reduce` version with spread creates a new object on each iteration (O(n²) work). `Object.fromEntries` is O(n). For learning, the `reduce` version illustrates the concept; for production, use `Object.fromEntries`.

---

### 🟢 FP #20: Declarative Max

```js
const findMax = (numbers) => numbers.reduce((max, n) => n > max ? n : max);
```

**Note**: No initial value — `reduce` uses the first element as the seed. This throws on empty arrays, which is correct behavior (there's no max of nothing).

---

### 🟡 FP #29: Group By with `reduce`

```js
const groupBy = (arr, keyFn) =>
  arr.reduce((groups, item) => {
    const key = keyFn(item);
    return {
      ...groups,
      [key]: [...(groups[key] || []), item]
    };
  }, {});
```

**FP Alignment**: `reduce` is the most versatile array method — it can build any data structure from an array. This pattern (accumulating into an object) is extremely common.

---

### 🟡 FP #40: Transducer Intuition — `mapFilter`

```js
const mapFilter = (arr, mapFn, filterFn) =>
  arr.reduce((acc, item) =>
    filterFn(item) ? [...acc, mapFn(item)] : acc, []);
```

**FP Alignment**: Combines filter and map in a single pass — no intermediate array. This is the intuition behind **transducers** (→ FP Guide §14). The `reduce` is doing both jobs at once.

---

### 🟢 JS #25: Array Sum & Average

```js
const sumAndAverage = (arr) => {
  const total = arr.reduce((sum, n) => sum + n, 0);
  return { sum: total, average: total / arr.length };
};
```

---

### 🟢 JS #26: Remove Duplicates (without Set)

```js
// FP approach — filter with indexOf
const removeDuplicates = (arr) =>
  arr.filter((item, index) => arr.indexOf(item) === index);
```

**FP Alignment**: Uses `filter` declaratively — keep an element only if its first occurrence index matches the current index. No mutation, no external state.

---

### 🟡 JS #42: Implement `map` from Scratch

```js
const myMap = (arr, callback) =>
  arr.reduce((result, item, index, array) =>
    [...result, callback(item, index, array)], []);
```

**FP Alignment**: `map` is implemented as a special case of `reduce`. This demonstrates that `reduce` is the most fundamental array operation — `map`, `filter`, `find`, `every`, `some` can all be built from it.

---

### 🟡 JS #43: Implement `filter` from Scratch

```js
const myFilter = (arr, callback) =>
  arr.reduce((result, item, index, array) =>
    callback(item, index, array) ? [...result, item] : result, []);
```

---

### 🟡 JS #44: Implement `reduce` from Scratch

```js
const myReduce = (arr, callback, initialValue) => {
  let acc = initialValue !== undefined ? initialValue : arr[0];
  const startIndex = initialValue !== undefined ? 0 : 1;
  for (let i = startIndex; i < arr.length; i++) {
    acc = callback(acc, arr[i], i, arr);
  }
  return acc;
};
```

**FP Alignment**: This is one case where a loop is justified — we're implementing the primitive that everything else builds on. The loop is internal and the public interface is still functional.

---

### 🟡 JS #45: Group By Property

```js
const groupBy = (arr, key) =>
  arr.reduce((groups, item) => ({
    ...groups,
    [item[key]]: [...(groups[item[key]] || []), item]
  }), {});
```

---

### 🟡 JS #51: Frequency Counter

```js
const frequencyCounter = (arr) =>
  arr.reduce((freq, item) => ({
    ...freq,
    [item]: (freq[item] || 0) + 1
  }), {});
```

**FP Alignment**: Classic `reduce` pattern for building an object. Identical structure to groupBy — the accumulator is an object, each iteration spreads the previous state and updates one key.

---

## 4. Strings & Characters

**→ JS Found §6 (Strings)**

### 🟢 JS #7: String Length Without `.length`

```js
const getStringLength = (str) => [...str].reduce((count) => count + 1, 0);
// OR simply:
const getStringLength = (str) => [...str].length;
```

**FP Alignment**: Spread + reduce is the FP way. The spread handles Unicode correctly (unlike `str.split("")` which breaks on emoji).

---

### 🟢 JS #15: Concatenate Without `+`

```js
const concatStrings = (str, str) => `${str}${str}`;
```

---

### 🟢 JS #17: Character Counter

```js
const countChar = (str, char) =>
  [...str].filter(c => c === char).length;
// OR with reduce:
const countChar = (str, char) =>
  [...str].reduce((count, c) => c === char ? count + 1 : count, 0);
```

---

### 🟢 JS #22: Palindrome Checker

```js
const isPalindrome = (str) => {
  const normalized = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  return normalized === [...normalized].reverse().join("");
};
```

**FP Alignment**: A pipeline: normalize → spread → reverse → join → compare. Pure function, no mutation.

---

### 🟢 JS #24: Count Vowels and Consonants

```js
const countVowelsConsonants = (str) => {
  const vowels = new Set("aeiouAEIOU");
  return [...str.replace(/[^a-zA-Z]/g, "")].reduce(
    (counts, char) => vowels.has(char)
      ? { ...counts, vowels: counts.vowels + 1 }
      : { ...counts, consonants: counts.consonants + 1 },
    { vowels: 0, consonants: 0 }
  );
};
```

**FP Alignment**: Single-pass `reduce` that builds up an immutable accumulator. Each iteration returns a new object rather than mutating a counter.

---

### 🟢 JS #27: Title Case a Sentence

```js
const titleCase = (str) =>
  str.split(" ")
    .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
    .join(" ");
```

**FP Alignment**: Classic split → map → join pipeline. Each step is a pure transformation.

---

### 🟢 JS #29: Reverse Words in a Sentence

```js
const reverseWords = (str) =>
  str.trim().split(/\s+/).reverse().join(" ");
```

---

### 🟢 JS #33: Caesar Cipher

```js
const caesarCipher = (str, shift) =>
  [...str].map(char => {
    if (/[a-z]/.test(char))
      return String.fromCharCode(((char.charCodeAt(0) - 97 + shift) % 26 + 26) % 26 + 97);
    if (/[A-Z]/.test(char))
      return String.fromCharCode(((char.charCodeAt(0) - 65 + shift) % 26 + 26) % 26 + 65);
    return char;
  }).join("");
```

**FP Alignment**: Spread → map (transform each character) → join. Each character is independently transformed — a perfect fit for `map`. The `(% 26 + 26) % 26` pattern handles negative shifts correctly.

---

### 🟢 JS #35: Count Words

```js
const wordCount = (str) => {
  const trimmed = str.trim();
  return trimmed === "" ? 0 : trimmed.split(/\s+/).length;
};
```

---

### 🟡 JS #65: Longest Common Prefix

```js
const longestCommonPrefix = (strs) => {
  if (strs.length === 0) return "";
  return strs.reduce((prefix, str) => {
    while (!str.startsWith(prefix)) {
      prefix = prefix.slice(0, -1);
    }
    return prefix;
  }, strs[0]);
};
```

**FP Alignment**: Uses `reduce` to shrink the prefix across all strings. The `while` loop inside is a pragmatic choice — a purely recursive version would be less clear.

---

## 5. Higher-Order Functions & Closures

**→ FP Guide §4 (Higher-Order Functions), §9 (Closures)**

### 🟢 FP #12: First-Class Functions

```js
const applyOperation = (a, b, operation) => operation(a, b);
```

**FP Alignment**: Functions as values — passed as an argument and invoked. This is the foundation of all higher-order patterns.

---

### 🟢 FP #13: Returning Functions

```js
const createMultiplier = (factor) => (n) => n * factor;
```

**FP Alignment**: A factory that returns a closure. The returned function "remembers" `factor` from its enclosing scope.

---

### 🟢 FP #14: Simple Predicate Builder

```js
const isGreaterThan = (threshold) => (value) => value > threshold;

// Usage with filter:
[5, 12, 8, 20, 3].filter(isGreaterThan(10));  // [12, 20]
```

**FP Alignment**: This is **partial application** in disguise — we fix the `threshold` and return a single-argument function that can be passed directly to `filter`.

---

### 🟡 FP #31: Memoize

```js
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};
```

**FP Alignment**: Memoization only works for **pure functions** (referential transparency). If the function has side effects, caching would hide them. The cache itself uses mutation (Map.set), but it's hidden inside the closure — the external behavior is still pure.

---

### 🟡 FP #32: Once (Call-at-Most-Once)

```js
const once = (fn) => {
  let called = false;
  let result;
  return (...args) => {
    if (called) return result;
    called = true;
    result = fn(...args);
    return result;
  };
};
```

**FP Alignment**: Controls side effects by limiting execution. Uses closure mutation internally but presents a predictable external interface.

---

### 🟡 JS #46: Debounce

```js
const debounce = (fn, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
};
```

**FP Alignment**: A higher-order function that wraps another function with timing behavior. Uses closure to track the timer. This is inherently impure (timing is a side effect), but the pattern is functionally structured.

---

### 🟡 JS #47: Throttle

```js
const throttle = (fn, limit) => {
  let lastCall = 0;
  return (...args) => {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      return fn(...args);
    }
  };
};
```

---

### 🟡 JS #48: Memoization

Same as FP #31 — see above.

---

## 6. Composition & Pipelines

**→ FP Guide §7 (Function Composition)**

### 🟡 FP #21: Simple Compose

```js
const compose = (f, g) => (x) => f(g(x));
```

---

### 🟡 FP #22: Pipe (Left-to-Right)

```js
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);
```

---

### 🟡 FP #23: Compose with Multiple Functions

```js
const compose = (...fns) =>
  fns.length === 0
    ? (x) => x                                          // identity
    : (x) => fns.reduceRight((acc, fn) => fn(acc), x);
```

**FP Alignment**: Empty compose returns the identity function — a mathematically correct design choice.

---

### 🟡 FP #30: Function Composition Pipeline for Data

```js
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);

const filterInStock = (products) => products.filter(p => p.inStock);
const applyDiscount = (percent) => (products) =>
  products.map(p => ({ ...p, price: p.price * (1 - percent / 100) }));
const sortByPrice = (products) =>
  [...products].sort((a, b) => a.price - b.price);
const formatPrices = (products) =>
  products.map(p => ({ ...p, price: `$${p.price.toFixed(2)}` }));

const processProducts = pipe(
  filterInStock,
  applyDiscount(10),
  sortByPrice,
  formatPrices
);
```

**FP Alignment**: Each function is pure, single-purpose, and independently testable. `applyDiscount` is curried — `applyDiscount(10)` returns a function that takes products. The pipeline reads top-to-bottom like a recipe.

---

### 🟡 JS #53: Pipe and Compose

```js
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);
```

---

## 7. Currying & Partial Application

**→ FP Guide §8 (Currying)**

### 🟡 FP #24: Manual Currying

```js
const curry = (fn) => {
  const curried = (...args) =>
    args.length >= fn.length
      ? fn(...args)
      : (...more) => curried(...args, ...more);
  return curried;
};
```

**FP Alignment**: Auto-curry detects when enough arguments are collected (via `fn.length`) and either calls the original function or returns a partially applied version. This enables both `f(1)(2)(3)` and `f(1, 2, 3)`.

---

### 🟡 FP #25: Partial Application

```js
const partial = (fn, ...presetArgs) =>
  (...laterArgs) => fn(...presetArgs, ...laterArgs);
```

**FP Alignment**: Simpler than currying — just prepends some arguments. Unlike curry, partial doesn't chain.

---

### 🟡 JS #49: Curry Function

Same as FP #24. The JS problem version emphasizes that all calling patterns should work: `f(1)(2)(3)`, `f(1, 2)(3)`, etc.

---

## 8. Recursion

**→ FP Guide §10 (Recursion)**

### 🟡 FP #26: Recursive Sum

```js
const recursiveSum = (arr) =>
  arr.length === 0 ? 0 : arr[0] + recursiveSum(arr.slice(1));

// With accumulator (tail-recursive style):
const recursiveSum = (arr, acc = 0) =>
  arr.length === 0 ? acc : recursiveSum(arr.slice(1), acc + arr[0]);
```

---

### 🟡 FP #27: Recursive Map

```js
const recursiveMap = (arr, fn) =>
  arr.length === 0 ? [] : [fn(arr[0]), ...recursiveMap(arr.slice(1), fn)];
```

---

### 🟡 FP #28: Recursive Flatten

```js
const deepFlatten = (arr) =>
  arr.reduce((flat, item) =>
    Array.isArray(item)
      ? [...flat, ...deepFlatten(item)]
      : [...flat, item],
  []);
```

**FP Alignment**: Combines `reduce` with recursion. For each element: if it's an array, recursively flatten it; otherwise, keep it. This handles arbitrary nesting depth.

---

### 🟡 FP #35: Unfold (Recursive Generation)

```js
const unfold = (fn, seed) => {
  const result = fn(seed);
  return result === null ? [] : [result[0], ...unfold(fn, result[1])];
};
```

**FP Alignment**: Unfold is the **dual of reduce** — while reduce collapses a list to a value, unfold builds a list from a seed. This duality is a core FP concept.

---

### 🟡 FP #37: Functional `every` and `some`

```js
const functionalEvery = (arr, pred) =>
  arr.length === 0 ? true : pred(arr[0]) && functionalEvery(arr.slice(1), pred);

const functionalSome = (arr, pred) =>
  arr.length === 0 ? false : pred(arr[0]) || functionalSome(arr.slice(1), pred);
```

**FP Alignment**: Uses short-circuit evaluation (`&&` and `||`) — if `every` finds a false, it stops; if `some` finds a true, it stops. These are fundamental logical predicates implemented recursively.

---

### 🟢 JS #19: Digital Root

```js
// Recursive approach
const digitalRoot = (num) =>
  num < 10 ? num : digitalRoot([...String(num)].reduce((s, d) => s + Number(d), 0));

// Mathematical approach (pure, O(1))
const digitalRoot = (num) => num === 0 ? 0 : 1 + ((num - 1) % 9);
```

**FP Alignment**: Both are pure. The mathematical version is elegant — it replaces recursion with a formula. The recursive version demonstrates the pattern: convert → split → reduce → recurse.

---

### 🟢 JS #23: Fibonacci Sequence

```js
// FP approach — unfold pattern
const fibonacci = (n) => {
  const unfold = (fn, seed, count) =>
    count <= 0 ? [] : [seed[0], ...unfold(fn, fn(seed), count - 1)];

  return unfold(([a, b]) => [b, a + b], [0, 1], n);
};

// Or with reduce (iterative but functional)
const fibonacci = (n) =>
  Array.from({ length: n }).reduce(
    (acc) => [...acc, (acc.at(-1) ?? 0) + (acc.at(-2) ?? 1)],
    []
  ).map((_, i, arr) => i < 2 ? i : arr[i]);

// Simplest FP approach:
const fibonacci = (n) =>
  Array.from({ length: n }, (_, i) => i).reduce(
    (fibs, i) => [...fibs, i < 2 ? i : fibs[i - 1] + fibs[i - 2]],
    []
  );
```

---

### 🟢 JS #39: Recursive Countdown

```js
const countdown = (n) => {
  if (n <= 0) { console.log("Go!"); return; }
  console.log(n);
  countdown(n - 1);
};
```

**FP Perspective**: This is inherently impure (`console.log` is a side effect). A pure version would return an array: `const countdown = (n) => n <= 0 ? ["Go!"] : [n, ...countdown(n - 1)]`.

---

### 🟢 JS #13: Reverse a Number

```js
const reverseNumber = (num) => {
  const sign = Math.sign(num);
  const reversed = Number([...String(Math.abs(num))].reverse().join(""));
  return sign * reversed;
};
```

**FP Alignment**: Pipeline: absolute value → string → spread → reverse → join → number → restore sign. Each step is a pure transformation.

---

### 🟡 JS #34: Flatten One Level

```js
const flattenOneLevel = (arr) =>
  arr.reduce((flat, item) => flat.concat(item), []);
// OR:
const flattenOneLevel = (arr) => [].concat(...arr);
```

---

## 9. Objects & Data Modeling

**→ JS Found §8 (Objects), → FP Guide §3 (Immutability)**

### 🟢 JS #14: Percentage Calculator

```js
const calculateGrade = (marks, total) => {
  const percentage = (marks / total) * 100;
  const gradeScale = [
    [80, "A"], [60, "B"], [40, "C"]
  ];
  const grade = gradeScale.reduce(
    (found, [threshold, letter]) => found || (percentage >= threshold ? letter : null),
    null
  ) || "F";
  return { percentage, grade };
};
```

**FP Alignment**: Uses a data-driven lookup instead of a chain of `if/else`. The `gradeScale` array makes it easy to add/change grade boundaries without touching the logic.

---

### 🟡 FP #38: Immutable State Reducer

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT": {
      const count = state.count + 1;
      return { count, history: [...state.history, count] };
    }
    case "DECREMENT": {
      const count = state.count - 1;
      return { count, history: [...state.history, count] };
    }
    case "RESET":
      return { count: 0, history: [...state.history, 0] };
    default:
      return state;
  }
};
```

**FP Alignment**: This is the Redux pattern — a pure function that takes the current state and an action, returning a new state without mutation. It's testable, predictable, and supports time-travel debugging.

---

### 🟡 FP #39: Functional Lookup Table

```js
const getStatusMessage = (code) => {
  const messages = {
    200: "OK",
    404: "Not Found",
    500: "Internal Server Error"
  };
  return messages[code] ?? "Unknown Status";
};
```

**FP Alignment**: Data-driven branching — no `if`/`switch`/ternary. The object IS the logic. Adding new status codes means adding data, not code.

---

### 🟡 JS #52: Object Deep Equality

```js
const deepEqual = (a, b) => {
  if (a === b) return true;
  if (a == null || b == null) return false;
  if (typeof a !== typeof b) return false;
  if (typeof a !== "object") return false;

  const keysA = Object.keys(a);
  const keysB = Object.keys(b);
  if (keysA.length !== keysB.length) return false;

  return keysA.every(key => deepEqual(a[key], b[key]));
};
```

**FP Alignment**: A recursive pure function. Uses `every` (declarative) instead of a loop with early return. The function is referentially transparent — same inputs always produce the same result.

---

### 🟡 JS #54: Flatten Object

```js
const flattenObject = (obj, prefix = "") =>
  Object.entries(obj).reduce((flat, [key, value]) => {
    const newKey = prefix ? `${prefix}.${key}` : key;
    if (typeof value === "object" && value !== null && !Array.isArray(value)) {
      return { ...flat, ...flattenObject(value, newKey) };
    }
    return { ...flat, [newKey]: value };
  }, {});
```

**FP Alignment**: Recursive `reduce` over object entries. Each level contributes to the flat result through spread — immutable throughout.

---

### 🟢 JS #55: Range Function

```js
const range = (start, end, step) => {
  const s = step ?? (start <= end ? 1 : -1);
  return Array.from(
    { length: Math.floor(Math.abs(end - start) / Math.abs(s)) + 1 },
    (_, i) => start + i * s
  );
};
```

**FP Alignment**: `Array.from` with a mapping function is the declarative way to generate sequences. No loops, no mutation.

---

### 🟡 JS #73: Pick, Omit, RenameKeys

```js
const pick = (obj, keys) =>
  keys.reduce((result, key) =>
    key in obj ? { ...result, [key]: obj[key] } : result, {});

const omit = (obj, keys) =>
  Object.fromEntries(
    Object.entries(obj).filter(([key]) => !keys.includes(key))
  );

const renameKeys = (obj, keyMap) =>
  Object.fromEntries(
    Object.entries(obj).map(([key, val]) => [keyMap[key] || key, val])
  );
```

**FP Alignment**: All three are pure transformations using `entries` → transform → `fromEntries`. This is the FP pattern for object manipulation.

---

## 10. Control Flow & Logic

### 🟢 JS #12: Leap Year Checker

```js
const isLeapYear = (year) =>
  (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
```

**FP Alignment**: A pure predicate — a single boolean expression. No `if/else` needed when the logic is a simple boolean formula.

---

### 🟢 JS #21: FizzBuzz

```js
// Imperative with side effects
const fizzBuzz = (n) => {
  for (let i = 1; i <= n; i++) {
    if (i % 15 === 0) console.log("FizzBuzz");
    else if (i % 3 === 0) console.log("Fizz");
    else if (i % 5 === 0) console.log("Buzz");
    else console.log(i);
  }
};

// FP — returns an array, no side effects
const fizzBuzz = (n) =>
  Array.from({ length: n }, (_, i) => i + 1)
    .map(i =>
      (i % 3 === 0 ? "Fizz" : "") + (i % 5 === 0 ? "Buzz" : "") || i
    );
```

**FP Alignment**: The FP version returns data instead of performing I/O. The string concatenation trick (`"Fizz" + "Buzz" || i`) eliminates the `if/else if` chain — more declarative.

---

### 🟡 JS #56: Validate Brackets

```js
const isValidBrackets = (str) => {
  const pairs = { ")": "(", "]": "[", "}": "{" };
  const result = [...str].reduce((stack, char) => {
    if (stack === null) return null; // already failed
    if ("([{".includes(char)) return [...stack, char];
    if (")]}".includes(char)) {
      return stack.length > 0 && stack[stack.length - 1] === pairs[char]
        ? stack.slice(0, -1)
        : null;
    }
    return stack;
  }, []);
  return result !== null && result.length === 0;
};
```

**FP Alignment**: Uses `reduce` with an immutable stack (array). The stack grows with `[...stack, char]` and shrinks with `stack.slice(0, -1)` — no mutation. Returns `null` to signal failure (short-circuit pattern).

---

### 🟢 JS #40: Unique Characters

```js
const hasUniqueChars = (str) => new Set(str).size === str.length;
```

---

### 🟢 JS #28: Find Second Largest

```js
const secondLargest = (arr) => {
  if (arr.length < 2) return null;
  const sorted = [...new Set(arr)].sort((a, b) => b - a);
  return sorted.length < 2 ? null : sorted[1];
};
// Or with reduce (single pass, no sort):
const secondLargest = (arr) => {
  const [first, second] = arr.reduce(
    ([f, s], n) => n > f ? [n, f] : n > s && n < f ? [f, n] : [f, s],
    [-Infinity, -Infinity]
  );
  return second === -Infinity ? null : second;
};
```

---

### 🟢 JS #31: Array Rotation

```js
const rotateArray = (arr, k) => {
  const n = k % arr.length;
  return [...arr.slice(-n), ...arr.slice(0, -n)];
};
```

**FP Alignment**: Pure slice + spread — no mutation. Elegant and concise.

---

### 🟡 JS #37: Chunk Array

```js
const chunkArray = (arr, size) =>
  Array.from(
    { length: Math.ceil(arr.length / size) },
    (_, i) => arr.slice(i * size, (i + 1) * size)
  );
// Or with reduce:
const chunkArray = (arr, size) =>
  arr.reduce((chunks, item, i) =>
    i % size === 0
      ? [...chunks, [item]]
      : [...chunks.slice(0, -1), [...chunks[chunks.length - 1], item]],
  []);
```

---

### 🟢 JS #38: Longest Word

```js
const longestWord = (sentence) =>
  sentence.split(" ").reduce((longest, word) =>
    word.length > longest.length ? word : longest
  );
```

---

### 🟢 JS #36: Find Missing Number

```js
const findMissing = (arr) => {
  const n = arr.length + 1;
  const expectedSum = n * (n + 1) / 2;
  return expectedSum - arr.reduce((sum, num) => sum + num, 0);
};
```

**FP Alignment**: Mathematical approach + `reduce` — no loops, no mutation. The gauss formula `n(n+1)/2` combined with a sum reduction is elegant and O(n).

---

## 11. Functional Data Structures

**→ FP Guide §18 (Persistent Data Structures)**

### 🟡 JS #59: Linked List

```js
// OOP approach (as the problem asks):
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
    let curr = this.head;
    while (curr.next) curr = curr.next;
    curr.next = new Node(value);
  }

  prepend(value) { this.head = new Node(value, this.head); }

  delete(value) {
    if (!this.head) return;
    if (this.head.value === value) { this.head = this.head.next; return; }
    let curr = this.head;
    while (curr.next && curr.next.value !== value) curr = curr.next;
    if (curr.next) curr.next = curr.next.next;
  }

  find(value) {
    let curr = this.head;
    while (curr) {
      if (curr.value === value) return curr;
      curr = curr.next;
    }
    return null;
  }

  toArray() {
    const result = [];
    let curr = this.head;
    while (curr) { result.push(curr.value); curr = curr.next; }
    return result;
  }
}
```

**FP Perspective**: This uses mutation (`curr.next = ...`). The FP alternative is an immutable linked list (FP #46) where `cons` creates new nodes and old lists are preserved via structural sharing.

---

### 🔴 FP #46: Immutable Linked List

```js
const cons = (head, tail) => ({ head, tail });
const head = (list) => list.head;
const tail = (list) => list.tail;
const isEmpty = (list) => list === null;

const listOf = (...items) =>
  items.reduceRight((list, item) => cons(item, list), null);

const toArray = (list) =>
  isEmpty(list) ? [] : [head(list), ...toArray(tail(list))];

const listMap = (list, fn) =>
  isEmpty(list) ? null : cons(fn(head(list)), listMap(tail(list), fn));

const listFilter = (list, pred) =>
  isEmpty(list) ? null
  : pred(head(list)) ? cons(head(list), listFilter(tail(list), pred))
  : listFilter(tail(list), pred);

const listReduce = (list, fn, acc) =>
  isEmpty(list) ? acc : listReduce(tail(list), fn, fn(acc, head(list)));
```

**FP Alignment**: Fully immutable. `cons` prepends in O(1) and shares the entire tail. Both old and new lists coexist without copying.

---

### 🟡 JS #60: Stack and Queue

```js
// Functional Stack (immutable)
const createStack = () => ({
  items: [],
  push: function(item) { return { ...this, items: [...this.items, item] }; },
  pop: function() {
    return {
      value: this.items[this.items.length - 1],
      stack: { ...this, items: this.items.slice(0, -1) }
    };
  },
  peek: function() { return this.items[this.items.length - 1]; },
  size: function() { return this.items.length; }
});

// Class-based (as problem specifies)
class Stack {
  #items = [];
  push(item) { this.#items.push(item); }
  pop() { return this.#items.pop(); }
  peek() { return this.#items[this.#items.length - 1]; }
  size() { return this.#items.length; }
  isEmpty() { return this.#items.length === 0; }
}

class Queue {
  #items = [];
  enqueue(item) { this.#items.push(item); }
  dequeue() { return this.#items.shift(); }
  peek() { return this.#items[0]; }
  size() { return this.#items.length; }
  isEmpty() { return this.#items.length === 0; }
}
```

---

### 🟡 JS #63: LRU Cache

```js
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }

  get(key) {
    if (!this.cache.has(key)) return -1;
    const value = this.cache.get(key);
    this.cache.delete(key);     // remove
    this.cache.set(key, value); // re-insert at end
    return value;
  }

  put(key, value) {
    if (this.cache.has(key)) this.cache.delete(key);
    else if (this.cache.size >= this.capacity) {
      this.cache.delete(this.cache.keys().next().value); // evict oldest
    }
    this.cache.set(key, value);
  }
}
```

**FP Perspective**: LRU caches are inherently stateful. The FP approach would return a new cache on each operation (like the immutable stack above), but the performance trade-off makes mutation pragmatic here.

---

## 12. Monads & Algebraic Types

**→ FP Guide §11–12 (Functors, Monads), §17 (ADTs)**

### 🔴 FP #41: Maybe Monad

```js
const Maybe = {
  of: (value) =>
    value == null
      ? {
          map: () => Maybe.of(null),
          flatMap: () => Maybe.of(null),
          getOrElse: (def) => def,
          isNothing: true
        }
      : {
          map: (fn) => Maybe.of(fn(value)),
          flatMap: (fn) => fn(value),
          getOrElse: () => value,
          isNothing: false
        }
};
```

---

### 🔴 FP #42: Either Monad

```js
const Left = (value) => ({
  map: () => Left(value),
  flatMap: () => Left(value),
  fold: (leftFn, _) => leftFn(value),
  isLeft: true
});

const Right = (value) => ({
  map: (fn) => Right(fn(value)),
  flatMap: (fn) => fn(value),
  fold: (_, rightFn) => rightFn(value),
  isLeft: false
});

const Either = { left: Left, right: Right };
```

---

### 🔴 FP #51: Algebraic Data Types — Pattern Matching

```js
const Leaf = (value) => ({ tag: "Leaf", value });
const Branch = (left, right) => ({ tag: "Branch", left, right });
const match = (value, patterns) => patterns[value.tag](value);

const sumTree = (tree) => match(tree, {
  Leaf: ({ value }) => value,
  Branch: ({ left, right }) => sumTree(left) + sumTree(right)
});

const mapTree = (tree, fn) => match(tree, {
  Leaf: ({ value }) => Leaf(fn(value)),
  Branch: ({ left, right }) => Branch(mapTree(left, fn), mapTree(right, fn))
});
```

---

## 13. Advanced FP Techniques

**→ FP Guide §13–18**

### 🔴 FP #43: Lenses

```js
const lens = (getter, setter) => ({ getter, setter });
const view = (l, obj) => l.getter(obj);
const set = (l, val, obj) => l.setter(obj, val);
const over = (l, fn, obj) => set(l, fn(view(l, obj)), obj);

const composeLens = (outer, inner) => lens(
  (obj) => view(inner, view(outer, obj)),
  (obj, val) => over(outer, (innerObj) => set(inner, val, innerObj), obj)
);
```

---

### 🔴 FP #44: Trampoline

```js
const trampoline = (fn) => (...args) => {
  let result = fn(...args);
  while (typeof result === "function") result = result();
  return result;
};

const safeSum = trampoline(function sum(n, acc = 0) {
  if (n <= 0) return acc;
  return () => sum(n - 1, acc + n);
});
```

---

### 🔴 FP #45: Lazy Evaluation with Generators

```js
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
const lazyReduce = (gen, fn, init) => {
  let acc = init;
  for (const x of gen) acc = fn(acc, x);
  return acc;
};
```

---

### 🔴 FP #47: Church Encoding

```js
// Booleans
const TRUE  = (a) => (_) => a;
const FALSE = (_) => (b) => b;
const IF    = (c) => (t) => (e) => c(t)(e);
const AND   = (a) => (b) => a(b)(FALSE);
const OR    = (a) => (b) => a(TRUE)(b);
const NOT   = (a) => a(FALSE)(TRUE);

// Numerals
const ZERO = (f) => (x) => x;
const SUCC = (n) => (f) => (x) => f(n(f)(x));
const ADD  = (m) => (n) => (f) => (x) => m(f)(n(f)(x));
const MULT = (m) => (n) => (f) => m(n(f));

const toNum  = (n) => n(x => x + 1)(0);
const toBool = (b) => b(true)(false);
```

---

### 🔴 FP #48: Transducers

```js
const tMap = (fn) => (reducer) => (acc, item) => reducer(acc, fn(item));
const tFilter = (pred) => (reducer) => (acc, item) =>
  pred(item) ? reducer(acc, item) : acc;

const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);

const transduce = (xform, reducer, init, coll) =>
  coll.reduce(xform(reducer), init);
```

---

### 🔴 FP #33: Zip Two Arrays

```js
const zip = (arr1, arr2) =>
  Array.from(
    { length: Math.min(arr1.length, arr2.length) },
    (_, i) => [arr1[i], arr2[i]]
  );
```

---

### 🔴 FP #34: ZipWith

```js
const zipWith = (fn, arr1, arr2) =>
  Array.from(
    { length: Math.min(arr1.length, arr2.length) },
    (_, i) => fn(arr1[i], arr2[i])
  );
```

---

## 14. Async & Promises Through an FP Lens

**→ JS Found §13 (Async), → FP Guide §12 (Monads)**

### 🟡 JS #62: Promise.all from Scratch

```js
const myPromiseAll = (promises) =>
  new Promise((resolve, reject) => {
    const results = [];
    let completed = 0;
    if (promises.length === 0) return resolve([]);

    promises.forEach((p, i) => {
      Promise.resolve(p)
        .then(value => {
          results[i] = value;
          if (++completed === promises.length) resolve(results);
        })
        .catch(reject);
    });
  });
```

**FP Perspective**: `Promise.all` is like a monad "sequence" operation — it takes an array of monadic values and returns a monad of an array. The functional insight is that we're transforming `[Promise<A>]` into `Promise<[A]>`.

---

### 🟡 JS #69: Promise.allSettled

```js
const myPromiseAllSettled = (promises) =>
  new Promise((resolve) => {
    const results = [];
    let completed = 0;
    if (promises.length === 0) return resolve([]);

    promises.forEach((p, i) => {
      Promise.resolve(p)
        .then(value => { results[i] = { status: "fulfilled", value }; })
        .catch(reason => { results[i] = { status: "rejected", reason }; })
        .finally(() => { if (++completed === promises.length) resolve(results); });
    });
  });
```

---

### 🟡 JS #57: Retry with Exponential Backoff

```js
const retry = async (fn, maxRetries, baseDelay) => {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries) throw error;
      await new Promise(r => setTimeout(r, baseDelay * 2 ** attempt));
    }
  }
};
```

**FP Perspective**: A higher-order async function that wraps a fallible operation with retry logic. The exponential backoff formula `baseDelay * 2^attempt` is a pure computation.

---

### 🟡 JS #79: Cancellable Fetch

```js
const cancellableFetch = (url, options = {}) => {
  const controller = new AbortController();
  const promise = fetch(url, { ...options, signal: controller.signal })
    .then(res => res.json());
  return { promise, cancel: () => controller.abort() };
};
```

---

### 🔴 JS #68: Async Task Queue

```js
class AsyncTaskQueue {
  constructor(concurrency) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
    this.onCompleteCallback = null;
  }

  add(taskFn) {
    this.queue.push(taskFn);
    this.#run();
  }

  #run() {
    while (this.running < this.concurrency && this.queue.length > 0) {
      const task = this.queue.shift();
      this.running++;
      task().finally(() => {
        this.running--;
        this.#run();
        if (this.running === 0 && this.queue.length === 0 && this.onCompleteCallback) {
          this.onCompleteCallback();
        }
      });
    }
  }

  onComplete(callback) { this.onCompleteCallback = callback; }
}
```

---

### 🔴 JS #97: Async Iterable Pipeline

```js
const pipeline = async (source, ...transforms) => {
  let current = source;
  for (const transform of transforms) {
    current = transform(current);
  }
  return current;
};

const filter = (pred) => async function*(source) {
  for await (const item of source) if (pred(item)) yield item;
};

const map = (fn) => async function*(source) {
  for await (const item of source) yield fn(item);
};

const take = (n) => async function*(source) {
  let count = 0;
  for await (const item of source) {
    if (count++ >= n) return;
    yield item;
  }
};

const collect = () => async (source) => {
  const result = [];
  for await (const item of source) result.push(item);
  return result;
};
```

**FP Alignment**: This mirrors the lazy generator pattern (FP #45) but for async streams. Each operator is a higher-order function that takes a source and returns a new async generator — composable and lazy.

---

## 15. System Design with FP Influence

### 🟡 JS #50: Event Emitter

```js
class EventEmitter {
  constructor() { this.events = new Map(); }

  on(event, listener) {
    if (!this.events.has(event)) this.events.set(event, []);
    this.events.get(event).push(listener);
  }

  off(event, listener) {
    if (!this.events.has(event)) return;
    this.events.set(event, this.events.get(event).filter(l => l !== listener));
  }

  emit(event, ...args) {
    if (!this.events.has(event)) return;
    this.events.get(event).forEach(listener => listener(...args));
  }
}
```

**FP Perspective**: The Observer pattern is inherently imperative (state + side effects). The FP alternative is Observables/streams (→ FP #53, FP Guide §20) where events are treated as composable data streams.

---

### 🟡 JS #58: Parse Query String

```js
const parseQueryString = (url) => {
  const query = url.split("?")[1];
  if (!query) return {};
  return query.split("&").reduce((params, pair) => {
    const [key, value] = pair.split("=").map(decodeURIComponent);
    if (params[key] !== undefined) {
      params[key] = Array.isArray(params[key])
        ? [...params[key], value]
        : [params[key], value];
    } else {
      params[key] = value;
    }
    return params;
  }, {});
};
```

---

### 🟡 JS #64: Matrix Rotation (90°)

```js
const rotateMatrix = (matrix) =>
  matrix[0].map((_, colIndex) =>
    matrix.map(row => row[colIndex]).reverse()
  );
```

**FP Alignment**: Transpose + reverse done declaratively with `map`. No nested loops, no index juggling. The outer `map` iterates over column indices; the inner `map` extracts column values.

---

### 🔴 JS #80: State Machine

```js
class StateMachine {
  constructor({ initial, states }) {
    this.state = initial;
    this.states = states;
  }

  transition(event) {
    const currentConfig = this.states[this.state];
    if (!currentConfig?.on?.[event]) {
      throw new Error(`No transition '${event}' from state '${this.state}'`);
    }
    const oldState = this.state;
    this.state = currentConfig.on[event];
    currentConfig.onExit?.(oldState);
    this.states[this.state].onEnter?.(this.state);
    return this.state;
  }
}
```

**FP Perspective**: A pure FP state machine would be a function `(state, event) → state` — essentially a reducer! The class version wraps this in mutable state for convenience.

---

### 🔴 JS #83: Implement `JSON.stringify`

```js
const myStringify = (value, seen = new Set()) => {
  if (value === null) return "null";
  if (typeof value === "boolean" || typeof value === "number") return String(value);
  if (typeof value === "string") return `"${value}"`;
  if (typeof value === "object") {
    if (seen.has(value)) throw new Error("Circular reference detected");
    seen.add(value);
    if (Array.isArray(value)) {
      return `[${value.map(v => myStringify(v, seen)).join(",")}]`;
    }
    const entries = Object.entries(value)
      .map(([k, v]) => `"${k}":${myStringify(v, seen)}`)
      .join(",");
    return `{${entries}}`;
  }
  return undefined;
};
```

**FP Alignment**: Recursive, pattern-matching-style dispatch on the type of value. Uses `map` and `join` for arrays and objects. The `seen` Set for circular detection is the one impure element — a necessary pragmatism.

---

### 🔴 JS #86: Promise from Scratch

```js
class MyPromise {
  #state = "pending";
  #value = undefined;
  #callbacks = [];

  constructor(executor) {
    const resolve = (value) => {
      if (this.#state !== "pending") return;
      this.#state = "fulfilled";
      this.#value = value;
      this.#callbacks.forEach(cb => cb.onFulfilled(value));
    };
    const reject = (reason) => {
      if (this.#state !== "pending") return;
      this.#state = "rejected";
      this.#value = reason;
      this.#callbacks.forEach(cb => cb.onRejected(reason));
    };
    try { executor(resolve, reject); }
    catch (e) { reject(e); }
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      const handle = () => {
        queueMicrotask(() => {
          try {
            if (this.#state === "fulfilled") {
              const result = onFulfilled ? onFulfilled(this.#value) : this.#value;
              resolve(result);
            } else if (this.#state === "rejected") {
              if (onRejected) resolve(onRejected(this.#value));
              else reject(this.#value);
            }
          } catch (e) { reject(e); }
        });
      };
      if (this.#state === "pending") {
        this.#callbacks.push({ onFulfilled: () => handle(), onRejected: () => handle() });
      } else {
        handle();
      }
    });
  }

  catch(onRejected) { return this.then(null, onRejected); }

  static resolve(value) { return new MyPromise(r => r(value)); }
  static reject(reason) { return new MyPromise((_, r) => r(reason)); }
}
```

**FP Perspective**: A Promise is essentially a Monad where `.then()` acts as `flatMap`. The state machine inside (pending → fulfilled/rejected) follows the same pattern as a state reducer.

---

### 🔴 JS #88: Reactive State Management

```js
let activeEffect = null;

const reactive = (obj) => {
  const deps = new Map();
  return new Proxy(obj, {
    get(target, key) {
      if (activeEffect) {
        if (!deps.has(key)) deps.set(key, new Set());
        deps.get(key).add(activeEffect);
      }
      return target[key];
    },
    set(target, key, value) {
      target[key] = value;
      if (deps.has(key)) deps.get(key).forEach(fn => fn());
      return true;
    }
  });
};

const effect = (fn) => {
  activeEffect = fn;
  fn();
  activeEffect = null;
};
```

**FP Perspective**: This is Functional Reactive Programming (→ FP Guide §20) implemented with Proxies. The `effect` function creates a dependency-tracked side effect — the system automatically re-runs effects when their dependencies change.

---

### 🔴 JS #100: Reactive Spreadsheet Engine

```js
class Spreadsheet {
  constructor() {
    this.cells = {};
    this.deps = {};
  }

  set(cell, value) {
    this.cells[cell] = value;
    if (typeof value === "string" && value.startsWith("=")) {
      this.deps[cell] = this.#extractRefs(value);
      this.#checkCircular(cell, new Set());
    }
    this.#recalcDependents(cell);
  }

  get(cell) {
    const value = this.cells[cell];
    if (typeof value === "string" && value.startsWith("=")) {
      return this.#evaluate(value.slice(1));
    }
    return value;
  }

  #extractRefs(formula) {
    return [...formula.matchAll(/[A-Z]\d+/g)].map(m => m[0]);
  }

  #checkCircular(cell, visited) {
    if (visited.has(cell)) throw new Error("Circular reference detected");
    visited.add(cell);
    Object.entries(this.deps)
      .filter(([_, refs]) => refs.includes(cell))
      .forEach(([dependent]) => this.#checkCircular(dependent, new Set(visited)));
  }

  #evaluate(expr) {
    // Replace cell references with their values
    const resolved = expr.replace(/[A-Z]\d+/g, (ref) => this.get(ref));
    // Handle SUM(range)
    const withSums = resolved.replace(/SUM\(([A-Z])(\d+):([A-Z])(\d+)\)/g,
      (_, c, r1, c, r2) => {
        let sum = 0;
        for (let r = +r1; r <= +r2; r++) sum += this.get(`${c}${r}`) || 0;
        return sum;
      });
    return Function(`"use strict"; return (${withSums})`)();
  }

  #recalcDependents(cell) {
    Object.entries(this.deps)
      .filter(([_, refs]) => refs.includes(cell))
      .forEach(([dependent]) => this.#recalcDependents(dependent));
  }
}
```

**FP Perspective**: The spreadsheet is a dependency graph where cells are computed values. This is FRP in disguise — each cell is a reactive computation that depends on other cells and auto-updates when dependencies change.

---

## 📎 Index: Find Any Problem Quickly

| Problem | Section | Problem | Section |
|:--------|:--------|:--------|:--------|
| **JS #1–5** | §1 Pure Functions | **FP #1–2** | §1 Pure Functions |
| **JS #7–10** | §1, §4 Strings | **FP #3–5** | §2 Immutability |
| **JS #11–16** | §10 Control Flow | **FP #6–10** | §3 map/filter/reduce |
| **JS #17–20** | §4 Strings, §1 | **FP #11–14** | §2, §5 HOFs |
| **JS #21–29** | §4, §10 | **FP #15–20** | §1, §2, §3 |
| **JS #31–40** | §8, §10 | **FP #21–25** | §6, §7 Compose/Curry |
| **JS #41–45** | §2, §3, §9 | **FP #26–30** | §8 Recursion, §6 |
| **JS #46–53** | §5 HOFs, §3 | **FP #31–40** | §5, §3, §9, §13 |
| **JS #54–65** | §9, §10 | **FP #41–42** | §12 Monads |
| **JS #66–78** | §15 (DOM) | **FP #43–48** | §13 Advanced FP |
| **JS #79–88** | §14, §15 | **FP #49–54** | §13 Advanced FP |
| **JS #89–100** | §11, §15 | **FP #55–60** | §13 Advanced FP |

---

*This document is a companion to [02-javascript-coding-problems.md](./02-javascript-coding-problems.md) and [04-functional-programming-problems.md](./04-functional-programming-problems.md). For concept explanations, see [01-javascript-foundations.md](./01-javascript-foundations.md) and [03-functional-programming-guide.md](./03-functional-programming-guide.md).*
