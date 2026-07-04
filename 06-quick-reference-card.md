# ⚡ JavaScript & FP Quick Reference Card

> **Purpose**: A concise cheat sheet for daily use. Quickly look up array methods, FP patterns, terminology, and modern JavaScript syntax.
> 
> **Links**:
> - 📘 [JS Foundations](./01-javascript-foundations.md)
> - 🧠 [FP Guide](./03-functional-programming-guide.md)
> - 🔍 [FP Problem Solutions](./05-problem-solving-with-fp.md)

---

## 🧠 Memory Aids & Mental Models

### JavaScript Foundations
- **Variables (`var`, `let`, `const`)** → **📦 The Boxes and Labels**: `var` is a leaky box, `let` is a sturdy bin, `const` is a locked safe.
- **Hoisting** → **🛗 The Elevator**: Declarations go up to the top floor, but assignments stay on their original floor.
- **Closures** → **🎒 The Backpack**: A function carries a backpack of variables from its birthplace everywhere it goes.
- **Event Loop** → **🍳 The Restaurant Kitchen**: Call Stack (Chef), Web APIs (Appliances), Callback Queue (Order Tickets), Microtask Queue (VIP Tickets).
- **Promises** → **📟 The Restaurant Buzzer**: You get a buzzer (Pending). When food is ready, it buzzes (Resolved) or they run out of ingredients (Rejected).

### Functional Programming
- **Pure Functions** → **🎰 The Vending Machine**: Same money in, same snack out. No changing the weather (no side effects).
- **Immutability** → **📸 The Photograph**: You can't change the people in the picture once taken; you must take a new photo.
- **Array Methods (`map`, `filter`, `reduce`)** → **🍲 Kitchen Ingredients**: Map (slice potatoes), Filter (discard rotten), Reduce (mash into one bowl).
- **Currying** → **🏭 The Assembly Line**: A function passes through stations, getting one argument at a time until finished.

---

## 1. Array Methods Cheat Sheet

### 🔄 Transformation (Returns New Array)
| Method | Purpose | Signature | Example |
|:---|:---|:---|:---|
| `map` | Transform each element | `(val, i, arr) => new_val` | `[1, 2].map(x => x * 2)` → `[2, 4]` |
| `filter` | Select matching elements | `(val, i, arr) => boolean` | `[1, 2].filter(x => x > 1)` → `[2]` |
| `flatMap` | Map then flatten 1 level | `(val, i, arr) => [new_val]` | `[1, 2].flatMap(x => [x, x*2])` → `[1, 2, 2, 4]` |
| `concat` | Combine arrays | `...arrays` | `[1].concat([2])` → `[1, 2]` |
| `slice` | Extract a portion | `(start, end)` | `[1, 2, 3].slice(1)` → `[2, 3]` |
| `toSorted`* | Sort immutably | `(a, b) => number` | `[3, 1, 2].toSorted((a,b) => a-b)` → `[1, 2, 3]` |
| `toReversed`*| Reverse immutably | None | `[1, 2].toReversed()` → `[2, 1]` |
| `toSpliced`* | Splice immutably | `(start, delCount, ...items)`| `[1, 2, 3].toSpliced(1, 1, 9)` → `[1, 9, 3]` |
| `with`* | Replace at index immutably | `(index, value)` | `[1, 2, 3].with(1, 9)` → `[1, 9, 3]` |

*\* ES2023 methods. If unavailable, use `[...arr].sort(...)` etc.*

### 🔍 Searching & Checking (Returns Value/Boolean)
| Method | Purpose | Signature | Example |
|:---|:---|:---|:---|
| `find` | Get first matching element | `(val, i, arr) => boolean` | `[1, 2, 3].find(x => x > 1)` → `2` |
| `findIndex` | Get index of first match | `(val, i, arr) => boolean` | `[1, 2, 3].findIndex(x => x>1)` → `1` |
| `some` | True if ANY match | `(val, i, arr) => boolean` | `[1, 2, 3].some(x => x > 2)` → `true` |
| `every` | True if ALL match | `(val, i, arr) => boolean` | `[1, 2, 3].every(x => x > 0)` → `true` |
| `includes` | True if element exists | `(value, fromIndex?)` | `[1, 2, 3].includes(2)` → `true` |

### 🧮 Accumulation (Returns Any Type)
| Method | Purpose | Signature | Example |
|:---|:---|:---|:---|
| `reduce` | Reduce left-to-right | `(acc, val, i, arr) => acc, init?`| `[1, 2].reduce((a, b) => a+b, 0)` → `3` |
| `reduceRight`| Reduce right-to-left | `(acc, val, i, arr) => acc, init?`| `["a", "b"].reduceRight((a,b)=>a+b)` → `"ba"` |

### ⚠️ Mutating Methods (Avoid in FP)
- `push()`, `pop()`, `shift()`, `unshift()` — *Use spread syntax instead.*
- `splice()` — *Use `slice` or `toSpliced` instead.*
- `sort()`, `reverse()` — *Use `toSorted`/`toReversed` or spread first `[...arr].sort()`.*
- `fill()`, `copyWithin()`

---

## 2. Immutable Operations (The FP Way)

### 📦 Objects
```js
const obj = { a: 1, b: 2, c: { d: 3 } };

// Add/Update
const updated = { ...obj, a: 99 };
const deepUpdate = { ...obj, c: { ...obj.c, d: 99 } };

// Delete
const { b, ...withoutB } = obj;

// Merge
const merged = { ...obj1, ...obj2 }; // obj2 overwrites obj1
```

### 📋 Arrays
```js
const arr = [1, 2, 3];

// Add (push/unshift)
const appended = [...arr, 4];
const prepended = [0, ...arr];

// Insert at index
const inserted = [...arr.slice(0, 1), 99, ...arr.slice(1)];

// Remove at index
const removed = arr.filter((_, i) => i !== 1);

// Update at index
const updated = arr.map((val, i) => i === 1 ? 99 : val);
```

---

## 3. Core FP Patterns

### 🧩 Compose & Pipe
```js
// Pipe: Left-to-Right data flow (More readable)
const pipe = (...fns) => (x) => fns.reduce((acc, fn) => fn(acc), x);
const process = pipe(filterEvens, doubleAll, sum);

// Compose: Right-to-Left data flow (Mathematical f(g(x)))
const compose = (...fns) => (x) => fns.reduceRight((acc, fn) => fn(acc), x);
const processCompose = compose(sum, doubleAll, filterEvens);
```

### 🍛 Curry & Partial
```js
// Manual Curry
const add = a => b => a + b;
const add5 = add(5);

// Auto Curry
const curry = (fn) => {
  const curried = (...args) =>
    args.length >= fn.length
      ? fn(...args)
      : (...more) => curried(...args, ...more);
  return curried;
};

// Partial Application
const partial = (fn, ...presetArgs) =>
  (...laterArgs) => fn(...presetArgs, ...laterArgs);
```

### 🧱 Closures / Encapsulation
```js
const createCounter = (initial = 0) => {
  let count = initial; // Private state
  return {
    inc: () => ++count,
    get: () => count
  };
};
```

### 🚦 Memoization
```js
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (!cache.has(key)) cache.set(key, fn(...args));
    return cache.get(key);
  };
};
```

---

## 4. Modern JS Quick Ref

### 📦 Destructuring & Rest/Spread
```js
// Arrays
const [first, second, ...rest] = [1, 2, 3, 4]; // first=1, rest=[3,4]
const clone = [...rest];

// Objects
const { name: fullName, age = 18, ...other } = user; // Renaming & Defaults
const cloneObj = { ...other };

// Function Params
const greet = ({ name, role = "Guest" }) => `Hi ${name} (${role})`;
const sum = (...args) => args.reduce((a,b) => a+b, 0); // Variadic
```

### ❓ Optional Chaining & Nullish Coalescing
```js
// Optional Chaining (?.) - Safe deep access
const zip = user?.address?.zip; // undefined if address is null/undefined

// Nullish Coalescing (??) - Defaults for null/undefined ONLY (not 0, "", false)
const limit = config.limit ?? 10;
```

### 🔄 Object Iteration
```js
const obj = { a: 1, b: 2 };
Object.keys(obj);    // ["a", "b"]
Object.values(obj);  // [1, 2]
Object.entries(obj); // [["a", 1], ["b", 2]]

// Transform object via entries:
Object.fromEntries(
  Object.entries(obj).map(([k, v]) => [k, v * 2])
); // { a: 2, b: 4 }
```

### 🔤 Strings & Regex
```js
str.includes("foo");
str.startsWith("a");
str.endsWith("z");
str.padStart(3, "0");  // "5" -> "005"
str.replace(/foo/g, "bar");
str.replaceAll("foo", "bar"); // ES2021
[...str] // Safely split into characters (handles emojis)
```

---

## 5. FP Terminology Glossary

| Term | Meaning in simple terms |
|:---|:---|
| **Pure Function** | Returns same output for same input, causes no side effects. |
| **Side Effect** | Any interaction with the outside world (I/O, DOM, mutation). |
| **Referential Transparency** | An expression can be replaced by its evaluated value without changing program behavior. |
| **Immutability** | Data cannot be changed after creation. Make copies instead. |
| **Higher-Order Function (HOF)** | A function that takes a function as an argument or returns one. |
| **First-Class Function** | Functions can be treated like any other variable. |
| **Closure** | A function that "remembers" the variables in its lexical scope even when executed outside that scope. |
| **Declarative** | Programming by describing *what* you want, not *how* to do it (Imperative). |
| **Currying** | Breaking a function taking multiple args into a chain of functions taking 1 arg each. |
| **Partial Application** | Pre-filling some arguments of a function and returning a new function for the rest. |
| **Arity** | The number of arguments a function takes (unary=1, binary=2). |
| **Point-Free Style** | Defining functions without explicitly referencing their arguments (e.g. `const getNames = map(prop("name"))`). |
| **Functor** | An object/type that implements a `map` method obeying certain laws (e.g. Array). |
| **Monad** | A Functor that also implements a `flatMap`/`bind`/`chain` method (e.g. Promise, Maybe). |
| **Transducer** | A composable, efficient pipeline that transforms data without intermediate arrays. |
| **Thunk** | A delayed computation: a function that takes no arguments and returns a value. |
