# 🚀 JavaScript Coding Problems — Beginner to Advanced

A comprehensive collection of **100 coding challenges** organized into 5 difficulty levels. Each problem includes a clear description, expected behavior, and hints to guide your learning.

---

## 📗 Level 1 — Absolute Beginner (Problems 1–20)

> **Topics**: Variables, Data Types, Operators, Basic I/O, Type Conversion

---

### Problem 1: Hello, World!
Write a program that logs `"Hello, World!"` to the console.

```js
// Expected Output:
// Hello, World!
```

<details><summary>💡 Hint</summary>Use <code>console.log()</code></details>

---

### Problem 2: Variable Declarations
Declare three variables using `var`, `let`, and `const`. Assign your name, age, and a boolean `isStudent`. Log all three.

```js
// Expected Output:
// Name: Alice
// Age: 25
// Is Student: true
```

<details><summary>💡 Hint</summary><code>const</code> cannot be reassigned, <code>let</code> is block-scoped, <code>var</code> is function-scoped.</details>

---

### Problem 3: Swap Two Variables
Given two variables `a = 5` and `b = 10`, swap their values **without using a third variable**.

```js
// Before: a = 5, b = 10
// After:  a = 10, b = 5
```

<details><summary>💡 Hint</summary>Try destructuring assignment: <code>[a, b] = [b, a]</code></details>

---

### Problem 4: Data Type Checker
Write a function `checkType(value)` that returns the data type of the given value as a string.

```js
checkType(42);        // "number"
checkType("hello");   // "string"
checkType(true);      // "boolean"
checkType(null);      // "object"  (this is a known JS quirk!)
checkType(undefined); // "undefined"
```

<details><summary>💡 Hint</summary>Use the <code>typeof</code> operator.</details>

---

### Problem 5: Temperature Converter
Write two functions:
- `celsiusToFahrenheit(c)` → returns the Fahrenheit equivalent.
- `fahrenheitToCelsius(f)` → returns the Celsius equivalent.

```js
celsiusToFahrenheit(0);    // 32
celsiusToFahrenheit(100);  // 212
fahrenheitToCelsius(32);   // 0
fahrenheitToCelsius(212);  // 100
```

<details><summary>💡 Hint</summary>F = (C × 9/5) + 32 and C = (F − 32) × 5/9</details>

---

### Problem 6: Area Calculator
Write a function `calculateArea(shape, ...dimensions)` that calculates the area of:
- `"circle"` → given radius
- `"rectangle"` → given length and width
- `"triangle"` → given base and height

```js
calculateArea("circle", 5);        // 78.54 (approx)
calculateArea("rectangle", 4, 6);  // 24
calculateArea("triangle", 3, 8);   // 12
```

<details><summary>💡 Hint</summary>Circle area = π × r², Triangle area = ½ × base × height</details>

---

### Problem 7: String Length Without `.length`
Write a function `getStringLength(str)` that returns the length of a string **without** using the `.length` property.

```js
getStringLength("hello");  // 5
getStringLength("");        // 0
```

<details><summary>💡 Hint</summary>Convert the string to an array using spread syntax, then count elements.</details>

---

### Problem 8: Odd or Even
Write a function `oddOrEven(num)` that returns `"Odd"` or `"Even"`.

```js
oddOrEven(3);  // "Odd"
oddOrEven(4);  // "Even"
oddOrEven(0);  // "Even"
```

<details><summary>💡 Hint</summary>Use the modulo operator <code>%</code>.</details>

---

### Problem 9: Simple Interest Calculator
Write a function `simpleInterest(principal, rate, time)` that returns the simple interest.

```js
simpleInterest(1000, 5, 2);  // 100
simpleInterest(5000, 8, 3);  // 1200
```

<details><summary>💡 Hint</summary>SI = (P × R × T) / 100</details>

---

### Problem 10: Max of Three
Write a function `maxOfThree(a, b, c)` that returns the largest of three numbers **without** using `Math.max`.

```js
maxOfThree(10, 5, 8);   // 10
maxOfThree(3, 7, 2);    // 7
maxOfThree(-1, -5, -3); // -1
```

<details><summary>💡 Hint</summary>Use nested <code>if-else</code> or ternary operators to compare.</details>

---

### Problem 11: Type Coercion Explorer
Predict and verify the output of these expressions. Write a function `coercionQuiz()` that logs each result:

```js
"5" + 3       // ?
"5" - 3       // ?
"5" * "2"     // ?
true + true   // ?
false + "1"   // ?
null + 1      // ?
undefined + 1 // ?
"" + 0        // ?
```

<details><summary>💡 Hint</summary><code>+</code> with a string concatenates; <code>-</code>, <code>*</code>, <code>/</code> convert to numbers.</details>

---

### Problem 12: Leap Year Checker
Write a function `isLeapYear(year)` that returns `true` if the year is a leap year.

```js
isLeapYear(2024); // true
isLeapYear(1900); // false
isLeapYear(2000); // true
isLeapYear(2023); // false
```

<details><summary>💡 Hint</summary>Divisible by 4, but not by 100, unless also divisible by 400.</details>

---

### Problem 13: Reverse a Number
Write a function `reverseNumber(num)` that reverses the digits of a number. Handle negative numbers.

```js
reverseNumber(12345);  // 54321
reverseNumber(-678);   // -876
reverseNumber(1000);   // 1
```

<details><summary>💡 Hint</summary>Convert to string, reverse, convert back. Handle sign separately.</details>

---

### Problem 14: Percentage Calculator
Write a function `calculateGrade(marks, total)` that returns the percentage and letter grade.

```js
calculateGrade(85, 100);  // { percentage: 85, grade: "A" }
calculateGrade(62, 100);  // { percentage: 62, grade: "B" }
calculateGrade(45, 100);  // { percentage: 45, grade: "C" }
calculateGrade(30, 100);  // { percentage: 30, grade: "F" }
```

| Percentage | Grade |
|:----------:|:-----:|
| 80–100     | A     |
| 60–79      | B     |
| 40–59      | C     |
| Below 40   | F     |

<details><summary>💡 Hint</summary>Calculate percentage first, then use <code>if-else</code> to determine the grade.</details>

---

### Problem 15: Concatenate Without `+`
Write a function `concatStrings(str1, str2)` that concatenates two strings **without** using the `+` operator.

```js
concatStrings("Hello", " World"); // "Hello World"
concatStrings("Java", "Script");  // "JavaScript"
```

<details><summary>💡 Hint</summary>Use template literals <code>`${str1}${str2}`</code> or <code>String.prototype.concat()</code>.</details>

---

### Problem 16: BMI Calculator
Write a function `calculateBMI(weight, height)` where weight is in kg and height in meters. Return the BMI value and the category.

```js
calculateBMI(70, 1.75);
// { bmi: 22.86, category: "Normal weight" }
```

| BMI Range     | Category        |
|:-------------:|:---------------:|
| < 18.5        | Underweight     |
| 18.5–24.9     | Normal weight   |
| 25–29.9       | Overweight      |
| ≥ 30          | Obese           |

<details><summary>💡 Hint</summary>BMI = weight / (height × height). Round to 2 decimal places.</details>

---

### Problem 17: Character Counter
Write a function `countChar(str, char)` that counts how many times a character appears in a string.

```js
countChar("hello", "l");         // 2
countChar("javascript", "a");    // 2
countChar("banana", "z");        // 0
```

<details><summary>💡 Hint</summary>Use <code>split(char).length - 1</code> or loop through each character.</details>

---

### Problem 18: Power Calculator (No `Math.pow`)
Write a function `power(base, exponent)` that calculates base^exponent **without** using `Math.pow` or `**`.

```js
power(2, 3);  // 8
power(5, 0);  // 1
power(3, 4);  // 81
```

<details><summary>💡 Hint</summary>Use a loop to multiply the base by itself <code>exponent</code> times.</details>

---

### Problem 19: Digital Root
Write a function `digitalRoot(num)` that keeps summing the digits until a single digit is obtained.

```js
digitalRoot(493); // 4 + 9 + 3 = 16 → 1 + 6 = 7
digitalRoot(132189); // 6
digitalRoot(0);      // 0
```

<details><summary>💡 Hint</summary>Use a <code>while</code> loop or recursion. Also explore the mathematical formula: <code>1 + (n - 1) % 9</code>.</details>

---

### Problem 20: Tip Calculator
Write a function `splitBill(total, tipPercent, numPeople)` that calculates how much each person pays.

```js
splitBill(100, 15, 4);
// { totalWithTip: 115, tipAmount: 15, perPerson: 28.75 }
```

<details><summary>💡 Hint</summary>Calculate tip, add to total, divide by number of people.</details>

---

## 📘 Level 2 — Beginner (Problems 21–40)

> **Topics**: Control Flow, Loops, Functions, Basic Arrays & Strings

---

### Problem 21: FizzBuzz
Write a function `fizzBuzz(n)` that prints numbers from 1 to `n`:
- Print `"Fizz"` for multiples of 3
- Print `"Buzz"` for multiples of 5
- Print `"FizzBuzz"` for multiples of both
- Print the number otherwise

```js
fizzBuzz(15);
// 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz
```

---

### Problem 22: Palindrome Checker
Write a function `isPalindrome(str)` that checks if a string reads the same forwards and backwards (case-insensitive, ignore spaces).

```js
isPalindrome("racecar");          // true
isPalindrome("A man a plan a canal Panama"); // true
isPalindrome("hello");            // false
```

<details><summary>💡 Hint</summary>Normalize the string (lowercase, remove spaces), then compare with its reverse.</details>

---

### Problem 23: Fibonacci Sequence
Write a function `fibonacci(n)` that returns an array of the first `n` Fibonacci numbers.

```js
fibonacci(8); // [0, 1, 1, 2, 3, 5, 8, 13]
fibonacci(1); // [0]
fibonacci(2); // [0, 1]
```

<details><summary>💡 Hint</summary>Each number is the sum of the previous two. Seed with [0, 1].</details>

---

### Problem 24: Count Vowels and Consonants
Write a function `countVowelsConsonants(str)` that returns an object with counts of vowels and consonants.

```js
countVowelsConsonants("Hello World");
// { vowels: 3, consonants: 7 }
```

<details><summary>💡 Hint</summary>Define a set of vowels and check each alphabetic character.</details>

---

### Problem 25: Array Sum & Average
Write a function `sumAndAverage(arr)` that returns the sum and average of an array of numbers.

```js
sumAndAverage([1, 2, 3, 4, 5]); // { sum: 15, average: 3 }
sumAndAverage([10, 20, 30]);     // { sum: 60, average: 20 }
```

<details><summary>💡 Hint</summary>Use <code>reduce()</code> for sum, then divide by <code>arr.length</code>.</details>

---

### Problem 26: Remove Duplicates
Write a function `removeDuplicates(arr)` that returns a new array with duplicates removed, **without** using `Set`.

```js
removeDuplicates([1, 2, 2, 3, 4, 4, 5]); // [1, 2, 3, 4, 5]
removeDuplicates(["a", "b", "a", "c"]);   // ["a", "b", "c"]
```

<details><summary>💡 Hint</summary>Use <code>filter()</code> with <code>indexOf()</code>, or build a new array checking <code>includes()</code>.</details>

---

### Problem 27: Title Case a Sentence
Write a function `titleCase(str)` that capitalizes the first letter of every word.

```js
titleCase("hello world");                // "Hello World"
titleCase("the quick brown fox jumps");  // "The Quick Brown Fox Jumps"
```

<details><summary>💡 Hint</summary>Split by spaces, capitalize first char of each word, join back.</details>

---

### Problem 28: Find Second Largest
Write a function `secondLargest(arr)` that returns the second largest number in an array.

```js
secondLargest([5, 1, 8, 3, 9, 2]); // 8
secondLargest([10, 10, 9]);         // 9
secondLargest([1]);                 // null
```

<details><summary>💡 Hint</summary>Track both the largest and second largest while iterating.</details>

---

### Problem 29: Reverse Words in a Sentence
Write a function `reverseWords(str)` that reverses the order of words (not characters).

```js
reverseWords("Hello World");                    // "World Hello"
reverseWords("JavaScript is awesome");          // "awesome is JavaScript"
reverseWords("  spaces   should  be trimmed "); // "trimmed be should spaces"
```

<details><summary>💡 Hint</summary>Split, filter empty strings, reverse, join.</details>

---

### Problem 30: Multiplication Table
Write a function `multiplicationTable(n)` that prints the multiplication table for `n` (from 1 to 10).

```js
multiplicationTable(5);
// 5 x 1 = 5
// 5 x 2 = 10
// ...
// 5 x 10 = 50
```

---

### Problem 31: Array Rotation
Write a function `rotateArray(arr, k)` that rotates an array `k` positions to the right.

```js
rotateArray([1, 2, 3, 4, 5], 2);  // [4, 5, 1, 2, 3]
rotateArray([1, 2, 3], 1);         // [3, 1, 2]
rotateArray([1, 2, 3], 5);         // [2, 3, 1] (k > length)
```

<details><summary>💡 Hint</summary>Use <code>k % arr.length</code> to handle cases where k > length. Use <code>slice()</code> and <code>concat()</code>.</details>

---

### Problem 32: Password Validator
Write a function `validatePassword(password)` that checks if a password meets these rules:
- At least 8 characters
- Contains at least one uppercase letter
- Contains at least one lowercase letter
- Contains at least one digit
- Contains at least one special character (`!@#$%^&*`)

```js
validatePassword("Str0ng!Pass");  // { valid: true, errors: [] }
validatePassword("weak");
// { valid: false, errors: ["Too short", "No uppercase", "No digit", "No special char"] }
```

<details><summary>💡 Hint</summary>Check each condition independently and collect error messages.</details>

---

### Problem 33: Caesar Cipher
Write a function `caesarCipher(str, shift)` that shifts each letter by `shift` positions. Preserve case, ignore non-letters.

```js
caesarCipher("abc", 3);                // "def"
caesarCipher("Hello, World!", 5);      // "Mjqqt, Btwqi!"
caesarCipher("xyz", 3);                // "abc" (wraps around)
```

<details><summary>💡 Hint</summary>Use character codes (<code>charCodeAt</code>, <code>String.fromCharCode</code>) and modulo for wrapping.</details>

---

### Problem 34: Flatten a Nested Array (One Level)
Write a function `flattenOneLevel(arr)` that flattens an array one level deep, **without** using `.flat()`.

```js
flattenOneLevel([[1, 2], [3, 4], [5]]); // [1, 2, 3, 4, 5]
flattenOneLevel([1, [2, 3], 4]);         // [1, 2, 3, 4]
flattenOneLevel([[1, [2]], 3]);           // [1, [2], 3]
```

<details><summary>💡 Hint</summary>Use <code>reduce()</code> with <code>concat()</code>, or the spread operator.</details>

---

### Problem 35: Count Words in a String
Write a function `wordCount(str)` that returns the number of words, handling multiple spaces.

```js
wordCount("Hello World");           // 2
wordCount("  This   has   spaces"); // 3
wordCount("");                       // 0
```

<details><summary>💡 Hint</summary>Use <code>trim()</code> then <code>split(/\s+/)</code>. Handle empty string edge case.</details>

---

### Problem 36: Find Missing Number
Given an array containing `n-1` unique numbers from 1 to `n`, find the missing number.

```js
findMissing([1, 2, 4, 5, 6]); // 3
findMissing([3, 7, 1, 2, 8, 4, 5]); // 6
findMissing([1]);  // 2
```

<details><summary>💡 Hint</summary>Use the formula: sum of 1..n = n(n+1)/2. Subtract the array sum.</details>

---

### Problem 37: Chunk Array
Write a function `chunkArray(arr, size)` that splits an array into chunks of the given size.

```js
chunkArray([1, 2, 3, 4, 5], 2);    // [[1, 2], [3, 4], [5]]
chunkArray([1, 2, 3, 4, 5, 6], 3); // [[1, 2, 3], [4, 5, 6]]
chunkArray([1, 2, 3], 5);           // [[1, 2, 3]]
```

<details><summary>💡 Hint</summary>Use a <code>for</code> loop incrementing by <code>size</code>, slicing the array each iteration.</details>

---

### Problem 38: Longest Word in a Sentence
Write a function `longestWord(sentence)` that returns the longest word. If there's a tie, return the first one.

```js
longestWord("The quick brown fox jumped"); // "jumped"
longestWord("I love JavaScript");          // "JavaScript"
```

<details><summary>💡 Hint</summary>Split into words, then use <code>reduce()</code> to track the longest.</details>

---

### Problem 39: Recursive Countdown
Write a function `countdown(n)` using **recursion** (no loops) that logs numbers from `n` down to 1, then logs `"Go!"`.

```js
countdown(5);
// 5
// 4
// 3
// 2
// 1
// Go!
```

<details><summary>💡 Hint</summary>Base case: when <code>n <= 0</code>, log "Go!". Otherwise log <code>n</code> and call <code>countdown(n - 1)</code>.</details>

---

### Problem 40: Unique Characters
Write a function `hasUniqueChars(str)` that returns `true` if all characters in the string are unique.

```js
hasUniqueChars("abcdef");   // true
hasUniqueChars("aabcdef");  // false
hasUniqueChars("");          // true
```

<details><summary>💡 Hint</summary>Compare the length of the string with the size of a <code>Set</code> created from it.</details>

---

## 📙 Level 3 — Intermediate (Problems 41–65)

> **Topics**: Objects, Higher-Order Functions, Array Methods, Closures, Error Handling, JSON, Regular Expressions

---

### Problem 41: Deep Clone an Object
Write a function `deepClone(obj)` that creates a deep copy of an object (handle nested objects and arrays). Do **not** use `JSON.parse(JSON.stringify())`.

```js
const original = { a: 1, b: { c: 2, d: [3, 4] } };
const cloned = deepClone(original);
cloned.b.c = 99;
console.log(original.b.c); // 2 (unchanged)
```

<details><summary>💡 Hint</summary>Use recursion. Check <code>Array.isArray()</code> and <code>typeof === "object"</code> for each value.</details>

---

### Problem 42: Implement `Array.prototype.map` from Scratch
Write a function `myMap(arr, callback)` that mimics `.map()`.

```js
myMap([1, 2, 3], x => x * 2);         // [2, 4, 6]
myMap(["a", "b"], (el, i) => el + i);  // ["a0", "b1"]
```

<details><summary>💡 Hint</summary>Create a new array, loop through <code>arr</code>, push <code>callback(arr[i], i, arr)</code>.</details>

---

### Problem 43: Implement `Array.prototype.filter` from Scratch
Write a function `myFilter(arr, callback)` that mimics `.filter()`.

```js
myFilter([1, 2, 3, 4, 5], x => x > 3);      // [4, 5]
myFilter(["hi", "", "hello"], x => x.length); // ["hi", "hello"]
```

---

### Problem 44: Implement `Array.prototype.reduce` from Scratch
Write a function `myReduce(arr, callback, initialValue)` that mimics `.reduce()`.

```js
myReduce([1, 2, 3, 4], (acc, curr) => acc + curr, 0);     // 10
myReduce([1, 2, 3, 4], (acc, curr) => acc + curr);         // 10 (no initial value)
myReduce([[1], [2], [3]], (acc, curr) => acc.concat(curr)); // [1, 2, 3]
```

<details><summary>💡 Hint</summary>If no <code>initialValue</code>, use the first element and start iteration from index 1.</details>

---

### Problem 45: Group By Property
Write a function `groupBy(arr, key)` that groups an array of objects by a specified key.

```js
const people = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 25 },
  { name: "Diana", age: 30 }
];

groupBy(people, "age");
// {
//   25: [{ name: "Alice", age: 25 }, { name: "Charlie", age: 25 }],
//   30: [{ name: "Bob", age: 30 }, { name: "Diana", age: 30 }]
// }
```

<details><summary>💡 Hint</summary>Use <code>reduce()</code> with the key value as the accumulator's property name.</details>

---

### Problem 46: Debounce Function
Implement a `debounce(fn, delay)` function that ensures `fn` is only called after `delay` ms of inactivity.

```js
const log = debounce(() => console.log("Fired!"), 300);
log(); log(); log(); // Only logs "Fired!" once, 300ms after the last call
```

<details><summary>💡 Hint</summary>Use <code>setTimeout</code> and <code>clearTimeout</code>. Store the timer ID in a closure.</details>

---

### Problem 47: Throttle Function
Implement a `throttle(fn, limit)` function that ensures `fn` is called at most once every `limit` ms.

```js
const log = throttle(() => console.log("Called!"), 1000);
// Rapidly calling log() will only execute once per second
```

<details><summary>💡 Hint</summary>Track the last execution time. Only call <code>fn</code> if enough time has elapsed.</details>

---

### Problem 48: Memoization
Write a function `memoize(fn)` that caches results of expensive function calls.

```js
const slowSquare = (n) => {
  console.log("Computing...");
  return n * n;
};

const fastSquare = memoize(slowSquare);
fastSquare(5); // "Computing..." → 25
fastSquare(5); // 25 (no "Computing...", retrieved from cache)
fastSquare(6); // "Computing..." → 36
```

<details><summary>💡 Hint</summary>Use a closure to store a <code>Map</code> or object as cache. Use <code>JSON.stringify(args)</code> as the key.</details>

---

### Problem 49: Curry Function
Write a function `curry(fn)` that transforms a multi-argument function into a chain of single-argument functions.

```js
const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);

curriedAdd(1)(2)(3);   // 6
curriedAdd(1, 2)(3);   // 6
curriedAdd(1)(2, 3);   // 6
curriedAdd(1, 2, 3);   // 6
```

<details><summary>💡 Hint</summary>Return a function that collects arguments. When enough are collected (≥ <code>fn.length</code>), call <code>fn</code>.</details>

---

### Problem 50: Event Emitter
Implement a simple `EventEmitter` class with `on(event, listener)`, `emit(event, ...args)`, and `off(event, listener)`.

```js
const emitter = new EventEmitter();
const greet = (name) => console.log(`Hello, ${name}!`);

emitter.on("greet", greet);
emitter.emit("greet", "Alice");  // "Hello, Alice!"
emitter.off("greet", greet);
emitter.emit("greet", "Bob");    // (nothing happens)
```

<details><summary>💡 Hint</summary>Store listeners in a <code>Map</code> of event names → arrays of functions.</details>

---

### Problem 51: Frequency Counter
Write a function `frequencyCounter(arr)` that returns an object with the frequency of each element.

```js
frequencyCounter([1, 2, 2, 3, 3, 3]);
// { 1: 1, 2: 2, 3: 3 }

frequencyCounter(["apple", "banana", "apple", "cherry", "banana", "apple"]);
// { apple: 3, banana: 2, cherry: 1 }
```

---

### Problem 52: Object Deep Equality
Write a function `deepEqual(a, b)` that checks whether two values are deeply equal (handles nested objects, arrays, and primitives).

```js
deepEqual({ a: 1, b: { c: 2 } }, { a: 1, b: { c: 2 } }); // true
deepEqual([1, [2, 3]], [1, [2, 3]]);                         // true
deepEqual({ a: 1 }, { a: "1" });                             // false
```

<details><summary>💡 Hint</summary>Recursively compare. Check types, array lengths, and object keys.</details>

---

### Problem 53: Pipe and Compose
Implement `pipe(...fns)` and `compose(...fns)`:
- `pipe` applies functions left-to-right.
- `compose` applies functions right-to-left.

```js
const add1 = x => x + 1;
const double = x => x * 2;
const square = x => x * x;

pipe(add1, double, square)(2);     // square(double(add1(2))) = square(double(3)) = square(6) = 36
compose(add1, double, square)(2);  // add1(double(square(2))) = add1(double(4)) = add1(8) = 9
```

<details><summary>💡 Hint</summary>Use <code>reduce()</code> for pipe and <code>reduceRight()</code> for compose.</details>

---

### Problem 54: Flatten Object
Write a function `flattenObject(obj)` that flattens nested objects with dot notation.

```js
flattenObject({
  a: 1,
  b: { c: 2, d: { e: 3 } },
  f: [4, 5]
});
// { "a": 1, "b.c": 2, "b.d.e": 3, "f": [4, 5] }
```

<details><summary>💡 Hint</summary>Recurse through object properties, building up the key path with dots.</details>

---

### Problem 55: Range Function
Write a function `range(start, end, step)` that returns an array of numbers.

```js
range(1, 5);       // [1, 2, 3, 4, 5]
range(0, 10, 2);   // [0, 2, 4, 6, 8, 10]
range(5, 1, -1);   // [5, 4, 3, 2, 1]
range(1, 1);       // [1]
```

<details><summary>💡 Hint</summary>Default step to 1 or -1 based on <code>start</code> vs <code>end</code>. Loop and push.</details>

---

### Problem 56: Validate Brackets
Write a function `isValidBrackets(str)` that checks if brackets are properly matched and nested.

```js
isValidBrackets("(){}[]");       // true
isValidBrackets("({[]})");       // true
isValidBrackets("(]");           // false
isValidBrackets("([)]");         // false
isValidBrackets("");             // true
```

<details><summary>💡 Hint</summary>Use a stack. Push opening brackets, pop and match on closing brackets.</details>

---

### Problem 57: Retry with Exponential Backoff
Write a function `retry(fn, maxRetries, baseDelay)` that retries an async function with exponential backoff.

```js
let attempts = 0;
const flaky = async () => {
  attempts++;
  if (attempts < 3) throw new Error("Fail");
  return "Success!";
};

const result = await retry(flaky, 5, 100);
console.log(result); // "Success!" (after 2 failures)
```

<details><summary>💡 Hint</summary>Use a loop with <code>try/catch</code>. Delay = <code>baseDelay * 2^attempt</code>. Wrap delay in a Promise with setTimeout.</details>

---

### Problem 58: Parse Query String
Write a function `parseQueryString(url)` that extracts query parameters into an object.

```js
parseQueryString("https://example.com?name=Alice&age=25&active=true");
// { name: "Alice", age: "25", active: "true" }

parseQueryString("https://example.com?tags=js&tags=css");
// { tags: ["js", "css"] }
```

<details><summary>💡 Hint</summary>Split on <code>?</code>, then <code>&</code>, then <code>=</code>. Handle duplicate keys by converting to arrays.</details>

---

### Problem 59: Linked List Implementation
Implement a `LinkedList` class with methods: `append(value)`, `prepend(value)`, `delete(value)`, `find(value)`, `toArray()`.

```js
const list = new LinkedList();
list.append(1);
list.append(2);
list.append(3);
list.prepend(0);
list.toArray();    // [0, 1, 2, 3]
list.delete(2);
list.toArray();    // [0, 1, 3]
list.find(1);      // Node { value: 1, next: Node { value: 3 } }
```

<details><summary>💡 Hint</summary>Create a <code>Node</code> class with <code>value</code> and <code>next</code>. The list tracks <code>head</code>.</details>

---

### Problem 60: Stack and Queue
Implement `Stack` (LIFO) and `Queue` (FIFO) classes.

```js
// Stack
const stack = new Stack();
stack.push(1); stack.push(2); stack.push(3);
stack.pop();   // 3
stack.peek();  // 2
stack.size();  // 2

// Queue
const queue = new Queue();
queue.enqueue(1); queue.enqueue(2); queue.enqueue(3);
queue.dequeue();  // 1
queue.peek();     // 2
queue.size();     // 2
```

---

### Problem 61: Email Validator (Regex)
Write a function `isValidEmail(email)` that validates an email address using a regular expression.

```js
isValidEmail("user@example.com");       // true
isValidEmail("user.name+tag@sub.domain.com"); // true
isValidEmail("user@");                  // false
isValidEmail("@domain.com");            // false
isValidEmail("user@domain");            // false
```

<details><summary>💡 Hint</summary>A reasonable regex: <code>/^[^\s@]+@[^\s@]+\.[^\s@]+$/</code> (simplified).</details>

---

### Problem 62: Promise.all from Scratch
Implement `myPromiseAll(promises)` that works like `Promise.all`.

```js
const p1 = Promise.resolve(1);
const p2 = new Promise(resolve => setTimeout(() => resolve(2), 100));
const p3 = Promise.resolve(3);

myPromiseAll([p1, p2, p3]).then(console.log); // [1, 2, 3]

myPromiseAll([p1, Promise.reject("Error")])
  .catch(console.log); // "Error"
```

<details><summary>💡 Hint</summary>Return a new Promise. Track resolved count. Reject immediately on any rejection.</details>

---

### Problem 63: LRU Cache
Implement a `LRUCache` class with `get(key)` and `put(key, value)` methods, with a maximum capacity.

```js
const cache = new LRUCache(2);
cache.put("a", 1);
cache.put("b", 2);
cache.get("a");     // 1
cache.put("c", 3);  // Evicts "b" (least recently used)
cache.get("b");     // -1 (not found)
cache.get("c");     // 3
```

<details><summary>💡 Hint</summary>Use a <code>Map</code> (which preserves insertion order). On access, delete and re-insert to move to end.</details>

---

### Problem 64: Matrix Rotation (90°)
Write a function `rotateMatrix(matrix)` that rotates an N×N matrix 90 degrees clockwise.

```js
rotateMatrix([
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
]);
// [
//   [7, 4, 1],
//   [8, 5, 2],
//   [9, 6, 3]
// ]
```

<details><summary>💡 Hint</summary>Transpose the matrix (swap rows and columns), then reverse each row.</details>

---

### Problem 65: Longest Common Prefix
Write a function `longestCommonPrefix(strs)` that finds the longest common prefix among an array of strings.

```js
longestCommonPrefix(["flower", "flow", "flight"]);  // "fl"
longestCommonPrefix(["dog", "racecar", "car"]);     // ""
longestCommonPrefix(["interstellar", "internet", "internal"]); // "inter"
```

<details><summary>💡 Hint</summary>Start with the first string as the prefix. Shrink it until all strings match.</details>

---

## 📕 Level 4 — Upper Intermediate (Problems 66–85)

> **Topics**: DOM Manipulation, Async/Await, ES6+ Features, Generators, Proxies, Web APIs

---

### Problem 66: Dynamic Todo List (DOM)
Build a Todo List that supports:
- Adding tasks via an input field
- Marking tasks as complete (strikethrough)
- Deleting tasks
- Counter showing "X of Y tasks completed"

Create the HTML, CSS, and JS. All DOM manipulation must be in pure JavaScript (no frameworks).

<details><summary>💡 Hint</summary>Use <code>createElement</code>, event delegation on the list container, toggle a CSS class for completion.</details>

---

### Problem 67: Infinite Scroll (DOM + Fetch)
Create a page that fetches and displays items as the user scrolls near the bottom. Use `https://jsonplaceholder.typicode.com/posts?_page=N&_limit=10` as a mock API.

<details><summary>💡 Hint</summary>Listen to the <code>scroll</code> event. When <code>scrollHeight - scrollTop - clientHeight < threshold</code>, fetch the next page.</details>

---

### Problem 68: Async Task Queue
Implement an `AsyncTaskQueue` class that processes async tasks with a configurable concurrency limit.

```js
const queue = new AsyncTaskQueue(2); // max 2 concurrent tasks

queue.add(() => fetch("/api/1"));
queue.add(() => fetch("/api/2"));
queue.add(() => fetch("/api/3")); // waits for a slot

queue.onComplete(() => console.log("All done!"));
```

<details><summary>💡 Hint</summary>Maintain a queue of pending tasks and a count of active tasks. When one finishes, dequeue the next.</details>

---

### Problem 69: Implement `Promise.allSettled`
Write `myPromiseAllSettled(promises)` that returns results for **all** promises (both fulfilled and rejected).

```js
myPromiseAllSettled([
  Promise.resolve(1),
  Promise.reject("error"),
  Promise.resolve(3)
]).then(console.log);
// [
//   { status: "fulfilled", value: 1 },
//   { status: "rejected", reason: "error" },
//   { status: "fulfilled", value: 3 }
// ]
```

---

### Problem 70: Lazy Iterator with Generators
Write a generator function `lazyRange(start, end)` that yields numbers one at a time.

```js
const gen = lazyRange(1, 5);
gen.next(); // { value: 1, done: false }
gen.next(); // { value: 2, done: false }
// ...
gen.next(); // { value: 5, done: false }
gen.next(); // { value: undefined, done: true }
```

Then implement `lazyMap(generator, fn)` and `lazyFilter(generator, fn)` that return new generators.

```js
const evens = lazyFilter(lazyRange(1, 10), n => n % 2 === 0);
const doubled = lazyMap(evens, n => n * 2);
[...doubled]; // [4, 8, 12, 16, 20]
```

---

### Problem 71: Observable Pattern
Implement a simple `Observable` class:

```js
const obs = new Observable(subscriber => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  subscriber.complete();
});

obs.subscribe({
  next: val => console.log(val),       // 1, 2, 3
  complete: () => console.log("Done"), // "Done"
  error: err => console.error(err)
});
```

Add support for `.pipe(operator1, operator2, ...)` with operators like `map(fn)` and `filter(fn)`.

---

### Problem 72: Proxy-Based Validation
Use `Proxy` to create a schema-validated object that throws errors on invalid assignments.

```js
const schema = {
  name: { type: "string", required: true },
  age: { type: "number", min: 0, max: 150 },
  email: { type: "string", pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/ }
};

const user = createValidatedObject(schema);
user.name = "Alice";    // ✅ OK
user.age = 25;          // ✅ OK
user.age = -5;          // ❌ Throws: "age must be >= 0"
user.email = "invalid"; // ❌ Throws: "email does not match pattern"
delete user.name;       // ❌ Throws: "name is required"
```

<details><summary>💡 Hint</summary>Use <code>new Proxy({}, { set, deleteProperty })</code> and validate in the traps.</details>

---

### Problem 73: Implement Destructuring Helpers
Write these utility functions that mimic destructuring:

```js
// Pick specific keys
pick({ a: 1, b: 2, c: 3 }, ["a", "c"]); // { a: 1, c: 3 }

// Omit specific keys
omit({ a: 1, b: 2, c: 3 }, ["b"]); // { a: 1, c: 3 }

// Rename keys
renameKeys({ name: "Alice", age: 25 }, { name: "fullName" });
// { fullName: "Alice", age: 25 }
```

---

### Problem 74: Pub/Sub System
Build a `PubSub` system supporting:
- Namespaced events (e.g., `"user.login"`, `"user.*"`)
- Wildcard subscriptions
- One-time subscriptions (`once`)

```js
const ps = new PubSub();
ps.subscribe("user.*", data => console.log("User event:", data));
ps.subscribe("user.login", data => console.log("Login:", data));
ps.once("user.login", data => console.log("First login only:", data));

ps.publish("user.login", { id: 1 });
// "User event: { id: 1 }"
// "Login: { id: 1 }"
// "First login only: { id: 1 }"

ps.publish("user.login", { id: 2 });
// "User event: { id: 2 }"
// "Login: { id: 2 }"
// (no "First login only" — it was removed)
```

---

### Problem 75: Interactive Quiz App (DOM)
Build a timed quiz application that:
- Displays one question at a time with multiple-choice answers
- Has a countdown timer per question (10 seconds)
- Shows correct/incorrect feedback with animations
- Displays final score with a progress bar

Use this sample data:

```js
const questions = [
  { q: "What is typeof null?", options: ["null", "object", "undefined", "number"], answer: 1 },
  { q: "Which is not a JS data type?", options: ["string", "boolean", "float", "symbol"], answer: 2 },
  { q: "What does === check?", options: ["Value only", "Type only", "Value and type", "Reference"], answer: 2 }
];
```

---

### Problem 76: Custom `Array.from` and `Array.of`
Implement `myArrayFrom(iterable, mapFn)` and `myArrayOf(...items)`.

```js
myArrayFrom("hello");                      // ["h", "e", "l", "l", "o"]
myArrayFrom([1, 2, 3], x => x * 2);        // [2, 4, 6]
myArrayFrom({ length: 3 }, (_, i) => i);   // [0, 1, 2]

myArrayOf(1, 2, 3);     // [1, 2, 3]
myArrayOf(3);            // [3]  (unlike Array(3) which creates empty slots)
```

---

### Problem 77: Deep Freeze
Write a function `deepFreeze(obj)` that recursively freezes an object so no properties can be modified at any depth.

```js
const config = deepFreeze({
  db: { host: "localhost", port: 5432 },
  api: { key: "secret" }
});

config.db.host = "remote";      // Silently fails (or throws in strict mode)
console.log(config.db.host);    // "localhost"
```

<details><summary>💡 Hint</summary>Use <code>Object.freeze()</code> recursively on all object-type values.</details>

---

### Problem 78: Drag & Drop Sortable List (DOM)
Build a drag-and-drop sortable list using the HTML5 Drag and Drop API. Support:
- Visual feedback during drag (ghost element, drop zone highlighting)
- Reordering items
- An output showing the current order

---

### Problem 79: Cancellable Fetch
Write a `cancellableFetch(url, options)` that returns both the fetch promise and a `cancel()` function.

```js
const { promise, cancel } = cancellableFetch("https://api.example.com/data");

promise
  .then(data => console.log(data))
  .catch(err => {
    if (err.name === "AbortError") console.log("Request cancelled");
  });

// Cancel after 100ms
setTimeout(cancel, 100);
```

<details><summary>💡 Hint</summary>Use <code>AbortController</code> and pass its <code>signal</code> to <code>fetch</code>.</details>

---

### Problem 80: State Machine
Implement a generic `StateMachine` class:

```js
const trafficLight = new StateMachine({
  initial: "red",
  states: {
    red:    { on: { TIMER: "green" } },
    green:  { on: { TIMER: "yellow" } },
    yellow: { on: { TIMER: "red" } }
  }
});

trafficLight.state;             // "red"
trafficLight.transition("TIMER");
trafficLight.state;             // "green"
trafficLight.transition("TIMER");
trafficLight.state;             // "yellow"
trafficLight.transition("INVALID"); // throws or returns current state
```

Add support for `onEnter` and `onExit` hooks.

---

### Problem 81: Virtual DOM Differ
Write a `diff(oldTree, newTree)` function that computes the minimal set of patches to transform one virtual DOM tree into another.

```js
const oldTree = {
  type: "div",
  props: { id: "app" },
  children: [
    { type: "h1", props: {}, children: ["Hello"] },
    { type: "p", props: { class: "text" }, children: ["World"] }
  ]
};

const newTree = {
  type: "div",
  props: { id: "app" },
  children: [
    { type: "h1", props: {}, children: ["Hi"] },
    { type: "span", props: { class: "text" }, children: ["World"] }
  ]
};

diff(oldTree, newTree);
// [
//   { type: "TEXT_CHANGE", path: [0, 0], value: "Hi" },
//   { type: "NODE_REPLACE", path: [1], newNode: { type: "span", ... } }
// ]
```

---

### Problem 82: Dependency Injection Container
Build a DI container:

```js
const container = new DIContainer();

container.register("logger", () => ({
  log: (msg) => console.log(`[LOG] ${msg}`)
}));

container.register("userService", (deps) => ({
  getUser: (id) => {
    deps.logger.log(`Fetching user ${id}`);
    return { id, name: "Alice" };
  }
}), ["logger"]);

const userService = container.resolve("userService");
userService.getUser(1); // [LOG] Fetching user 1 → { id: 1, name: "Alice" }
```

Handle circular dependency detection.

---

### Problem 83: Implement `JSON.stringify` (Partial)
Write `myStringify(value)` supporting: strings, numbers, booleans, null, arrays, and plain objects. Handle circular references by throwing.

```js
myStringify(42);                  // "42"
myStringify("hello");             // '"hello"'
myStringify(true);                // "true"
myStringify(null);                // "null"
myStringify([1, "two", null]);    // '[1,"two",null]'
myStringify({ a: 1, b: "two" }); // '{"a":1,"b":"two"}'

const circular = {};
circular.self = circular;
myStringify(circular);            // Throws: "Circular reference detected"
```

---

### Problem 84: Intersection Observer Lazy Loader (DOM)
Create a page with 50+ image placeholders that lazy-load actual images when they enter the viewport, using the `IntersectionObserver` API. Include:
- Smooth fade-in animation
- Loading skeleton/placeholder
- Error handling for failed loads

---

### Problem 85: Web Worker Calculator
Create a calculator that offloads heavy computations to a Web Worker:
- Main thread sends expressions
- Worker thread evaluates and returns results
- Support cancelling long-running calculations

---

## 📓 Level 5 — Advanced (Problems 86–100)

> **Topics**: Design Patterns, Performance, Algorithms, Metaprogramming, System Design

---

### Problem 86: Promise from Scratch
Implement a `MyPromise` class that follows the Promises/A+ spec:
- Supports `then(onFulfilled, onRejected)` with chaining
- Supports `catch(onRejected)`
- Handles async resolution
- Supports `MyPromise.resolve()` and `MyPromise.reject()`

```js
const p = new MyPromise((resolve, reject) => {
  setTimeout(() => resolve(42), 100);
});

p.then(val => val * 2)
 .then(val => console.log(val))  // 84
 .catch(err => console.error(err));
```

<details><summary>💡 Hint</summary>Key: store callbacks, manage state transitions (pending → fulfilled/rejected), handle async resolution with microtask queuing (<code>queueMicrotask</code>).</details>

---

### Problem 87: Implement `async/await` with Generators
Write a function `asyncRunner(generatorFn)` that executes a generator function as if it were an async function.

```js
function* fetchData() {
  const response = yield fetch("https://jsonplaceholder.typicode.com/todos/1");
  const data = yield response.json();
  console.log(data.title);
  return data;
}

asyncRunner(fetchData); // Logs the todo title
```

<details><summary>💡 Hint</summary>Recursively call <code>gen.next(resolvedValue)</code> inside <code>.then()</code>. Handle errors with <code>gen.throw()</code>.</details>

---

### Problem 88: Reactive State Management
Build a reactive state system (inspired by Vue/MobX):

```js
const state = reactive({
  firstName: "John",
  lastName: "Doe",
  get fullName() { return `${this.firstName} ${this.lastName}`; }
});

effect(() => {
  console.log(`Name changed: ${state.fullName}`);
});
// Logs immediately: "Name changed: John Doe"

state.firstName = "Jane";
// Automatically logs: "Name changed: Jane Doe"
```

<details><summary>💡 Hint</summary>Use <code>Proxy</code> to intercept get/set. Track which effects access which properties (dependency tracking). Re-run effects when dependencies change.</details>

---

### Problem 89: Binary Search Tree
Implement a `BST` class with:
- `insert(value)`
- `search(value)` → returns the node or null
- `delete(value)`
- `inOrder()` → returns sorted array
- `findMin()`, `findMax()`
- `height()`
- `isBalanced()`

```js
const bst = new BST();
[5, 3, 7, 1, 4, 6, 8].forEach(v => bst.insert(v));
bst.inOrder();    // [1, 3, 4, 5, 6, 7, 8]
bst.search(4);    // Node { value: 4, left: null, right: null }
bst.height();     // 2
bst.isBalanced(); // true
```

---

### Problem 90: Graph Implementation (Adjacency List)
Implement a `Graph` class (undirected) with:
- `addVertex(v)`, `addEdge(v1, v2)`
- `bfs(start)` → returns visited nodes in BFS order
- `dfs(start)` → returns visited nodes in DFS order
- `shortestPath(start, end)` → returns the shortest path array
- `hasCycle()` → returns boolean

```js
const g = new Graph();
["A", "B", "C", "D", "E"].forEach(v => g.addVertex(v));
g.addEdge("A", "B"); g.addEdge("A", "C");
g.addEdge("B", "D"); g.addEdge("C", "E");

g.bfs("A");              // ["A", "B", "C", "D", "E"]
g.shortestPath("A", "E"); // ["A", "C", "E"]
```

---

### Problem 91: Template Engine
Build a simple template engine that supports:
- Variable interpolation: `{{ name }}`
- Conditionals: `{% if condition %} ... {% endif %}`
- Loops: `{% for item in items %} ... {% endfor %}`

```js
const template = `
<h1>Hello, {{ name }}!</h1>
{% if isAdmin %}
  <p>Welcome, admin.</p>
{% endif %}
<ul>
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
</ul>
`;

const result = render(template, {
  name: "Alice",
  isAdmin: true,
  items: ["Apple", "Banana", "Cherry"]
});
```

---

### Problem 92: Build a Module Bundler (Simplified)
Build a simple module bundler that:
1. Reads an entry file
2. Parses `import`/`require` statements to build a dependency graph
3. Bundles all modules into a single output string that can be evaluated

```js
// input: entry.js imports helper.js which imports utils.js
const bundle = bundler("./entry.js");
// Output: A self-contained function that runs the module tree
```

<details><summary>💡 Hint</summary>Use regex or a basic parser for imports. Map module IDs to their code. Wrap each in a function and use a runtime module resolver.</details>

---

### Problem 93: Concurrent Task Scheduler with Priority
Build a `PriorityTaskScheduler`:
- Tasks have priorities (1 = highest)
- Configurable concurrency limit
- Higher priority tasks preempt lower priority ones in the queue
- Support task cancellation and progress callbacks

```js
const scheduler = new PriorityTaskScheduler({ concurrency: 2 });

scheduler.add(() => heavyTask(), { priority: 3, id: "task" });
scheduler.add(() => heavyTask(), { priority: 1, id: "task" }); // runs first
scheduler.add(() => heavyTask(), { priority: 2, id: "task" });

scheduler.cancel("task");
scheduler.onProgress((id, progress) => console.log(`${id}: ${progress}%`));
```

---

### Problem 94: Implement `Object.create` and Prototypal Inheritance
Write `myObjectCreate(proto)` and demonstrate prototypal inheritance by building a shape hierarchy.

```js
const Shape = {
  init(type) { this.type = type; return this; },
  describe() { return `I am a ${this.type}`; }
};

const Circle = myObjectCreate(Shape);
Circle.init = function(radius) {
  Shape.init.call(this, "circle");
  this.radius = radius;
  return this;
};
Circle.area = function() {
  return Math.PI * this.radius ** 2;
};

const c = myObjectCreate(Circle).init(5);
c.describe(); // "I am a circle"
c.area();     // 78.54...
```

---

### Problem 95: Trie (Prefix Tree)
Implement a `Trie` class with:
- `insert(word)`
- `search(word)` → exact match
- `startsWith(prefix)` → returns boolean
- `autocomplete(prefix, limit)` → returns matching words
- `delete(word)`

```js
const trie = new Trie();
["apple", "app", "apricot", "banana", "band"].forEach(w => trie.insert(w));

trie.search("app");           // true
trie.search("ap");            // false
trie.startsWith("ap");        // true
trie.autocomplete("ap", 3);   // ["app", "apple", "apricot"]
trie.autocomplete("ban", 5);  // ["banana", "band"]
trie.delete("app");
trie.search("app");           // false
trie.search("apple");         // true
```

---

### Problem 96: Implement `JSON.parse` (Simplified)
Write `myJSONParse(str)` that parses a JSON string supporting: strings, numbers, booleans, null, arrays, and objects.

```js
myJSONParse('42');                    // 42
myJSONParse('"hello"');               // "hello"
myJSONParse('[1, "two", true, null]'); // [1, "two", true, null]
myJSONParse('{"a":1,"b":[2,3]}');     // { a: 1, b: [2, 3] }
```

<details><summary>💡 Hint</summary>Build a recursive descent parser. Track position with an index. Write functions for each type: <code>parseValue</code>, <code>parseString</code>, <code>parseNumber</code>, <code>parseArray</code>, <code>parseObject</code>.</details>

---

### Problem 97: Async Iterable Pipeline
Build an async pipeline that processes data streams:

```js
async function* numbersStream() {
  for (let i = 1; i <= 100; i++) {
    await new Promise(r => setTimeout(r, 10));
    yield i;
  }
}

const result = await pipeline(
  numbersStream(),
  filter(n => n % 2 === 0),    // even numbers
  map(n => n * n),              // square them
  take(5),                      // first 5
  collect()                     // gather into array
);

console.log(result); // [4, 16, 36, 64, 100]
```

Implement `pipeline`, `filter`, `map`, `take`, and `collect` for async iterables.

---

### Problem 98: JavaScript Interpreter (Mini)
Build a mini interpreter for a subset of JavaScript that supports:
- Variable declarations (`let x = 5;`)
- Arithmetic expressions (`+`, `-`, `*`, `/`)
- Print statements (`print(x);`)
- If/else conditionals
- While loops

```js
const code = `
  let x = 10;
  let y = 3;
  let result = 0;
  while (x > 0) {
    result = result + y;
    x = x - 1;
  }
  print(result);
`;

interpret(code); // 30
```

<details><summary>💡 Hint</summary>Build three phases: Tokenizer (lexer) → Parser (AST) → Evaluator (tree-walker). Start with the tokenizer.</details>

---

### Problem 99: Virtual File System
Implement an in-memory file system:

```js
const fs = new VirtualFileSystem();

fs.mkdir("/home");
fs.mkdir("/home/user");
fs.writeFile("/home/user/hello.txt", "Hello World");
fs.readFile("/home/user/hello.txt");  // "Hello World"
fs.ls("/home/user");                  // ["hello.txt"]
fs.ls("/home");                       // ["user/"]
fs.cp("/home/user/hello.txt", "/home/user/copy.txt");
fs.mv("/home/user/copy.txt", "/home/backup.txt");
fs.rm("/home/user/hello.txt");
fs.find("/home", "*.txt");            // ["/home/backup.txt"]
```

Support: `mkdir`, `writeFile`, `readFile`, `ls`, `rm`, `cp`, `mv`, `find` (with glob matching).

---

### Problem 100: Build a Reactive Spreadsheet Engine
Build a spreadsheet engine that supports:
- Cell references (`A1`, `B2`)
- Formulas (`=A1 + B2`, `=SUM(A1:A5)`, `=IF(A1 > 10, "yes", "no")`)
- Automatic recalculation when dependencies change
- Circular reference detection

```js
const sheet = new Spreadsheet();

sheet.set("A", 10);
sheet.set("A", 20);
sheet.set("A", "=A + A");
sheet.get("A");               // 30

sheet.set("B1", "=SUM(A:A)");
sheet.get("B1");               // 60

sheet.set("A", 50);           // triggers recalculation
sheet.get("A");               // 70
sheet.get("B1");               // 140

sheet.set("C1", "=D1");
sheet.set("D1", "=C1");       // Throws: "Circular reference detected"
```

---

## 🗺️ Topic Reference Matrix

| Topic | Problems |
|:------|:---------|
| Variables & Types | 1–4, 11 |
| Operators & Math | 5, 6, 8, 9, 10, 13, 18, 19 |
| Strings | 7, 15, 17, 22, 24, 27, 29, 33, 35, 65 |
| Control Flow | 12, 14, 16, 21, 32 |
| Loops | 20, 23, 30, 36, 39 |
| Arrays | 25, 26, 28, 31, 34, 37, 38, 40 |
| Objects | 41, 45, 51, 52, 54, 73, 77 |
| Higher-Order Functions | 42, 43, 44, 46, 47, 48, 49, 53 |
| Closures & Scope | 46, 47, 48, 49 |
| Classes & OOP | 50, 59, 60, 63, 80, 82 |
| Regular Expressions | 58, 61, 91 |
| DOM Manipulation | 66, 67, 75, 78, 84 |
| Async / Promises | 57, 62, 68, 69, 79, 86, 87, 97 |
| ES6+ Features | 3, 55, 70, 72, 76, 88 |
| Generators & Iterators | 70, 87, 97 |
| Proxy & Reflect | 72, 88 |
| Data Structures | 56, 59, 60, 63, 89, 90, 95 |
| Design Patterns | 50, 71, 74, 80, 82, 93 |
| Algorithms | 33, 36, 64, 65, 89, 90, 95 |
| System Design | 81, 85, 91, 92, 96, 98, 99, 100 |

---

## 📝 How to Use This Problem Set

1. **Start at your level** — Don't skip ahead; the early problems build foundational thinking.
2. **Solve before looking at hints** — Struggle is where learning happens.
3. **Write tests** — For each problem, write at least 3 test cases including edge cases.
4. **Refactor** — After solving, try to find a more elegant or efficient solution.
5. **Time yourself** — As you progress, try to solve Level 1–2 problems in under 10 minutes, Level 3 in under 20, Level 4 in under 30, and Level 5 in under 60.

> **Tip**: Create a separate `.js` file for each problem (e.g., `problem-01.js`, `problem-02.js`) in this folder to keep your solutions organized.

Happy coding! 🎉
