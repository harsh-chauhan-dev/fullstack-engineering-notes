# JavaScript Developer Handbook

### Core to Advanced — Full-Stack Edition

> **Purpose:** Complete reference for modern JavaScript development  
> **Audience:** Full-stack developers, interview prep, personal notes  
> **Format:** GitHub-ready documentation  
> **Standard:** ES2015+ (ES6 and beyond)

---

## 📋 Table of Contents

| #   | Module                                                             | Topics                                     | Level |
| --- | ------------------------------------------------------------------ | ------------------------------------------ | ----- |
| 1   | [JavaScript Introduction](#module-1--javascript-introduction)      | What is JS, engines, browser execution     | 🟢    |
| 2   | [Variables & Data Types](#module-2--variables-and-data-types)      | var/let/const, primitives, reference types | 🟢    |
| 3   | [Operators](#module-3--operators)                                  | Arithmetic, comparison, logical, coercion  | 🟢    |
| 4   | [Control Flow](#module-4--control-flow)                            | Conditionals, loops, truthy/falsy          | 🟢    |
| 5   | [Functions](#module-5--functions)                                  | All function types, HOF, IIFE, generators  | 🟡    |
| 6   | [Arrays & Objects](#module-6--arrays-and-objects)                  | Methods, destructuring, spread/rest        | 🟡    |
| 7   | [Engine Core Concepts](#module-7--javascript-engine-core-concepts) | Execution context, event loop, closures    | 🟡    |
| 8   | [Prototype & Inheritance](#module-8--prototype-and-inheritance)    | Proto chain, classes, inheritance          | 🟡    |
| 9   | [Asynchronous JavaScript](#module-9--asynchronous-javascript)      | Callbacks, Promises, async/await           | 🔴    |
| 10  | [DOM Manipulation](#module-10--dom-manipulation)                   | Selectors, events, delegation              | 🟡    |
| 11  | [Modern JavaScript ES6+](#module-11--modern-javascript-es6)        | All ES6+ features                          | 🟡    |
| 12  | [Memory Model](#module-12--javascript-memory-model)                | Stack, heap, GC, leaks                     | 🔴    |
| 13  | [Real World JavaScript](#module-13--real-world-javascript)         | React, Node.js, APIs                       | 🔴    |

**Difficulty:** 🟢 Beginner · 🟡 Intermediate · 🔴 Advanced

---

## 🗺️ Learning Path

```
┌─────────────────────────────────────────────────────────────────┐
│                    JAVASCRIPT LEARNING PATH                      │
│                                                                  │
│  WEEK 1          WEEK 2          WEEK 3          WEEK 4+        │
│  ─────────       ─────────       ─────────       ─────────      │
│  M1: Intro   →  M5: Functions → M7: Engine  →  M9: Async       │
│  M2: Types   →  M6: Arrays    → M8: Proto   →  M12: Memory     │
│  M3: Ops     →     & Objects  → M10: DOM    →  M13: Real World  │
│  M4: Control →  M11: ES6+     →             →                   │
└─────────────────────────────────────────────────────────────────┘
```

---

# Module 1 — JavaScript Introduction

## 1.1 What is JavaScript?

JavaScript is a **high-level**, **interpreted**, **dynamically typed**, **single-threaded** programming language. It was originally designed to make web pages interactive, but today it runs everywhere — browsers, servers, mobile apps, desktop apps, IoT devices, and more.

| Property      | JavaScript                                    |
| ------------- | --------------------------------------------- |
| **Paradigm**  | Multi-paradigm: OOP, functional, event-driven |
| **Typing**    | Dynamic + weak (loosely typed)                |
| **Execution** | Interpreted / JIT compiled                    |
| **Threading** | Single-threaded with async via event loop     |
| **Standard**  | ECMAScript (ECMA-262)                         |
| **Runtime**   | Browser, Node.js, Deno, Bun                   |

```javascript
// JavaScript in one snippet — the language in a nutshell
const greet = (name = "World") => `Hello, ${name}!`;

console.log(greet("Developer")); // Hello, Developer!
console.log(typeof greet); // "function"
console.log(1 + "1"); // "11"  ← dynamic typing at work
```

---

## 1.2 History and Evolution

```
YEAR   EVENT
────────────────────────────────────────────────────────────
1995   Brendan Eich creates "Mocha" in 10 days at Netscape
1995   Renamed "LiveScript", then "JavaScript" (marketing)
1996   Microsoft creates JScript for Internet Explorer
1997   ECMAScript 1 — first standardized version
1999   ES3 — regex, try/catch, do-while
2009   ES5 — strict mode, JSON, Array methods (forEach, map...)
2015   ES6 / ES2015 — THE BIG ONE ★
         let/const, arrow functions, classes, Promises,
         modules, template literals, destructuring, symbols
2016   ES7 — Array.includes(), exponent operator (**)
2017   ES8 — async/await, Object.entries/values
2018   ES9 — spread/rest in objects, Promise.finally
2019   ES10 — flat(), flatMap(), Object.fromEntries()
2020   ES11 — optional chaining (?.), nullish coalescing (??)
2021   ES12 — Promise.any(), logical assignment (??=)
2022   ES13 — Array.at(), Object.hasOwn(), top-level await
2023   ES14 — Array.toSorted(), toReversed(), findLast()
2024+  Ongoing improvements every year
```

> **Interview Insight:** ES6 (2015) is the most important version. It transformed JavaScript from a scripting language into a mature programming language. Every modern JS codebase uses ES6+ syntax.

---

## 1.3 JavaScript vs Other Languages

| Feature               | JavaScript     | Python      | Java     | C++       |
| --------------------- | -------------- | ----------- | -------- | --------- |
| **Typing**            | Dynamic        | Dynamic     | Static   | Static    |
| **Compiled?**         | JIT            | Interpreted | Compiled | Compiled  |
| **Threading**         | Single (async) | Multi       | Multi    | Multi     |
| **Runs in browser**   | ✅ Native      | ❌          | ❌       | ❌        |
| **Full-stack**        | ✅ (Node.js)   | ✅          | ✅       | Limited   |
| **Learning curve**    | Medium         | Easy        | Hard     | Very Hard |
| **Garbage collected** | ✅             | ✅          | ✅       | ❌ Manual |

**What makes JavaScript unique:**

- Only language that runs natively in browsers
- Same language for frontend AND backend (Node.js)
- Non-blocking I/O via event loop — handles thousands of concurrent requests
- Prototype-based inheritance (unlike classical OOP)

---

## 1.4 How JavaScript Works in the Browser

```
┌─────────────────────────────────────────────────────────────┐
│                        BROWSER                               │
│                                                             │
│  ┌─────────────┐    ┌──────────────────────────────────┐   │
│  │ HTML Parser  │    │         JavaScript Engine         │   │
│  │             │    │   (V8 in Chrome, SpiderMonkey     │   │
│  │ Builds DOM  │    │    in Firefox)                    │   │
│  └──────┬──────┘    └─────────────┬────────────────────┘   │
│         │                         │                         │
│         ▼                         ▼                         │
│  ┌─────────────┐    ┌──────────────────────────────────┐   │
│  │     DOM     │◄───│  Call Stack + Memory Heap         │   │
│  │   (Tree)    │    │                                   │   │
│  └─────────────┘    └─────────────┬────────────────────┘   │
│                                   │                         │
│                     ┌─────────────▼────────────────────┐   │
│                     │           Web APIs                │   │
│                     │  setTimeout · fetch · DOM events  │   │
│                     │  localStorage · geolocation       │   │
│                     └──────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

**Step-by-step page load:**

1. Browser downloads HTML → HTML Parser creates the DOM tree
2. Parser encounters `<script>` tag → pauses HTML parsing
3. Downloads and executes JavaScript
4. JS engine reads source → tokenizes → parses to AST → executes
5. JS can now read/modify the DOM
6. HTML parsing resumes

---

## 1.5 JavaScript Engines

**Definition:** A JavaScript engine reads JS source code, compiles it (JIT), and executes it.

| Engine                     | Browser/Runtime             | Creator |
| -------------------------- | --------------------------- | ------- |
| **V8**                     | Chrome, Node.js, Edge, Deno | Google  |
| **SpiderMonkey**           | Firefox                     | Mozilla |
| **JavaScriptCore (Nitro)** | Safari                      | Apple   |
| **Hermes**                 | React Native                | Meta    |

**How V8 Works (JIT Pipeline):**

```
JavaScript Source Code
        │
        ▼
   ┌─────────┐
   │  Parser  │  → Tokenizes source, checks syntax
   └────┬────┘
        │ Abstract Syntax Tree (AST)
        ▼
   ┌──────────┐
   │ Ignition │  → Interpreter → Bytecode (fast start)
   └────┬─────┘
        │ Profiling: finds "hot" code
        ▼
   ┌──────────────┐
   │  TurboFan    │  → JIT Compiler → Optimized Machine Code
   └──────────────┘
        │
        ▼
   Machine Code Execution
```

> **Key insight:** JavaScript is NOT purely interpreted. V8 uses JIT (Just-In-Time) compilation — hot code paths are compiled to native machine code for maximum performance.

---

## 1.6 Adding JavaScript to HTML

```html
<!-- Method 1: Inline — avoid for anything non-trivial -->
<button onclick="alert('Hello!')">Click</button>

<!-- Method 2: Internal <script> tag -->
<head>
  <script>
    // Runs immediately — DOM not ready yet!
    console.log("Internal script");
  </script>
</head>

<!-- Method 3: External file — BEST PRACTICE -->

<!-- Blocks HTML parsing until downloaded + executed (legacy) -->
<script src="app.js"></script>

<!-- defer: download in parallel, execute after HTML parsed ✅ -->
<script src="app.js" defer></script>

<!-- async: download in parallel, execute immediately when ready -->
<script src="analytics.js" async></script>
```

**Script loading comparison:**

```
Normal:   HTML ──────────────[BLOCK]──────────────────► DOM
          JS:          [download + execute]

defer:    HTML ──────────────────────────────────────► DOM
          JS:   [download         ]          [execute]

async:    HTML ──────────────────[BLOCK]─────────────► DOM
          JS:   [download  ][execute]
```

> **Best Practice:** Use `defer` for all application scripts. Use `async` only for independent scripts (analytics, ads) where execution order doesn't matter.

---

# Module 2 — Variables and Data Types

## 2.1 var vs let vs const

**Definition:** Variables are named containers that hold values. JavaScript has three keywords to declare them.

```javascript
// var — the old way (avoid in modern code)
var name = "Alice";
var name = "Bob"; // re-declare? No error. Dangerous.
console.log(name); // "Bob"

// let — block-scoped, reassignable
let age = 25;
age = 26; // OK — reassign
// let age = 30;    // SyntaxError — cannot re-declare

// const — block-scoped, not reassignable
const PI = 3.14159;
// PI = 3;          // TypeError — cannot reassign

// const with objects — the REFERENCE is const, not the content
const user = { name: "Alice" };
user.name = "Bob"; // OK — modifying content is fine
// user = {};       // TypeError — cannot reassign the reference
```

**Full Comparison Table:**

| Feature                | `var`             | `let`               | `const`           |
| ---------------------- | ----------------- | ------------------- | ----------------- |
| Scope                  | Function          | Block `{}`          | Block `{}`        |
| Hoisting               | Yes → `undefined` | Yes → TDZ ⚠️        | Yes → TDZ ⚠️      |
| Re-declare same scope  | ✅                | ❌                  | ❌                |
| Reassign               | ✅                | ✅                  | ❌                |
| Global object property | ✅ (`window.x`)   | ❌                  | ❌                |
| Use in modern code     | ❌ Avoid          | ✅ When reassigning | ✅ Default choice |

> **Best Practice:** Default to `const`. Switch to `let` only when you need to reassign. Never use `var` in new code.

---

## 2.2 Primitive Data Types

**Definition:** Primitive values are immutable and stored directly in the variable (on the stack). There are **7 primitive types** in JavaScript.

### Number

```javascript
// JavaScript has ONE number type for both integers and floats
let integer = 42;
let float = 3.14;
let negative = -100;
let sci = 1.5e6; // 1,500,000
let hex = 0xff; // 255
let binary = 0b1010; // 10
let octal = 0o17; // 15

// Special number values
Infinity - // 1/0
  Infinity; // -1/0
NaN; // "Not a Number" — result of invalid math
typeof NaN; // "number" ← yes, NaN is a number type!
isNaN("hello"); // true
Number.isNaN(NaN); // true (stricter version)
Number.isFinite(Infinity); // false
Number.MAX_SAFE_INTEGER; // 9007199254740991

// Floating point gotcha
0.1 + 0.2 === 0.3; // FALSE!  ← classic JS interview trap
0.1 +
  0.2 + // 0.30000000000000004
  // Fix:
  (0.1 + 0.2).toFixed(1) ===
  0.3; // true
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON; // true
```

### String

```javascript
let single = "Hello";
let double = "World";
let template = `Hello, ${single}!`; // Template literal (ES6)

// String methods
"hello".length; // 5
"hello".toUpperCase(); // "HELLO"
"WORLD".toLowerCase(); // "world"
"hello world".includes("world"); // true
"hello".startsWith("hel"); // true
"hello".endsWith("lo"); // true
"hello world".indexOf("world"); // 6
"hello".replace("l", "r"); // "herlo" (first only)
"hello".replaceAll("l", "r"); // "herro"
"a,b,c".split(","); // ["a","b","c"]
"  hello  ".trim(); // "hello"
"ha".repeat(3); // "hahaha"
"5".padStart(3, "0"); // "005"
"hello".slice(1, 3); // "el"

// Strings are immutable
let str = "hello";
str[0] = "H"; // silently fails
console.log(str); // "hello" — unchanged
```

### Boolean

```javascript
let isActive = true;
let isDeleted = false;

// Boolean() conversion
Boolean(1); // true
Boolean(0); // false
Boolean("hello"); // true
Boolean(""); // false
Boolean(null); // false
Boolean(undefined); // false
Boolean([]); // true  ← surprising! empty array is truthy
Boolean({}); // true  ← empty object is truthy
Boolean(NaN); // false
```

### Null

```javascript
// null = intentional absence of value (set by programmer)
let user = null; // "there is no user"

typeof null; // "object" ← HISTORIC BUG in JavaScript
null == undefined; // true  (loose equality)
null === undefined; // false (strict equality)
```

### Undefined

```javascript
// undefined = variable declared but not assigned
let x;
console.log(x); // undefined

function greet(name) {
  console.log(name); // undefined if no arg passed
}

// typeof undefined
typeof undefined; // "undefined"
typeof x; // "undefined" — even for undeclared vars!
```

### Symbol (ES6)

```javascript
// Symbol = unique, immutable primitive — no two symbols are equal
const id1 = Symbol("id");
const id2 = Symbol("id");
id1 === id2; // false — always unique!

// Primary use: unique object property keys (no conflicts)
const KEY = Symbol("secretKey");
const obj = {
  [KEY]: "secret value",
  name: "Alice",
};
obj[KEY]; // "secret value"
Object.keys(obj); // ["name"] — Symbol not visible here

// Well-known Symbols
Symbol.iterator; // make objects iterable
Symbol.toPrimitive; // customize type conversion
```

### BigInt (ES2020)

```javascript
// BigInt = integers larger than Number.MAX_SAFE_INTEGER
const big = 9007199254740991n; // append 'n'
const huge = BigInt("12345678901234567890");

// Cannot mix BigInt and Number without explicit conversion
// 10n + 5    // TypeError
10n + BigInt(5); // 15n

typeof 42n; // "bigint"
```

---

## 2.3 Reference Types

**Definition:** Reference types are stored in the heap. The variable holds a _reference (pointer)_ to the memory location, not the value itself.

### Object

```javascript
// Objects store key-value pairs
const person = {
  name: "Alice",
  age: 25,
  address: {
    // nested object
    city: "NYC",
    zip: "10001",
  },
  greet() {
    // method shorthand (ES6)
    return `Hi, I'm ${this.name}`;
  },
};

// Property access
person.name; // dot notation
person["name"]; // bracket notation
const key = "age";
person[key]; // 25 — dynamic key access
```

### Array

```javascript
// Arrays are ordered, indexed collections
const fruits = ["apple", "banana", "cherry"];
fruits[0]; // "apple"
fruits.length; // 3
typeof fruits; // "object" ← arrays ARE objects!
Array.isArray(fruits); // true ← proper check
```

### Function

```javascript
// Functions are first-class objects in JavaScript
function add(a, b) {
  return a + b;
}
typeof add; // "function"
add.name; // "add"  — functions have properties!
add.length; // 2      — number of expected params
```

---

## 2.4 Value Types vs Reference Types — Memory Diagram

This is the most important concept for understanding unexpected bugs.

```
PRIMITIVES — stored by value (STACK)
─────────────────────────────────────────────────────

let a = 5;
let b = a;    // b gets a COPY of the value
b = 10;

STACK:
┌─────────┬───────┐
│ a       │   5   │  ← unchanged
├─────────┼───────┤
│ b       │  10   │  ← independent copy
└─────────┴───────┘

console.log(a)  // 5 — not affected


REFERENCE TYPES — stored by reference (HEAP)
─────────────────────────────────────────────────────

let obj1 = { name: "Alice" };
let obj2 = obj1;   // obj2 gets a REFERENCE (pointer)
obj2.name = "Bob";

STACK:                 HEAP:
┌─────────┬────────┐  ┌─────────────────────┐
│ obj1    │ 0x001 ─┼─►│  { name: "Bob" }    │
├─────────┼────────┤  │  (same object!)     │
│ obj2    │ 0x001 ─┼─►│                     │
└─────────┴────────┘  └─────────────────────┘

console.log(obj1.name)  // "Bob" ← MUTATED via obj2!


FIXING IT — creating a real copy:
─────────────────────────────────────────────────────

// Shallow copy
let obj3 = { ...obj1 };           // spread operator
let obj4 = Object.assign({}, obj1);

// Deep copy (nested objects)
let obj5 = structuredClone(obj1); // ES2022 ✅
let obj6 = JSON.parse(JSON.stringify(obj1)); // old way (limitations)
```

> **Interview Insight:** Understanding value vs reference types is critical. It explains why `===` comparing two objects with same content returns `false`, and why passing objects to functions can mutate the original.

```javascript
// Equality trap with reference types
const a = { x: 1 };
const b = { x: 1 };
a === b; // false — different references in memory!

const c = a;
a === c; // true — same reference
```

### Exercise 2

```javascript
// Predict the output of each:
let x = 10;
let y = x;
y++;
console.log(x, y); // ?

const arr1 = [1, 2, 3];
const arr2 = arr1;
arr2.push(4);
console.log(arr1); // ?

const obj = { a: 1 };
const copy = { ...obj };
copy.a = 99;
console.log(obj.a); // ?
```

<details>
<summary>Answers</summary>

```
x=10, y=11   — primitive copy, independent
[1,2,3,4]    — same reference, both mutated
1            — spread creates shallow copy, original unchanged
```

</details>

---

# Module 3 — Operators

## 3.1 Arithmetic Operators

```javascript
5 + 3; // 8    — addition
5 - 3; // 2    — subtraction
5 * 3; // 15   — multiplication
10 / 3; // 3.333...  — division (always float)
10 % 3; // 1    — modulus (remainder)
2 ** 3; // 8    — exponentiation (ES7)

// Increment / Decrement
let a = 5;
a++; // returns 5, THEN increments (postfix)
++a; // increments FIRST, then returns (prefix)
a--; // returns current, then decrements
--a; // decrements first, then returns

// Practical distinction
let x = 5;
console.log(x++); // 5 — returns value before increment
console.log(x); // 6 — now incremented

let y = 5;
console.log(++y); // 6 — incremented first, then returned
```

---

## 3.2 Comparison Operators

```javascript
5 > 3      // true
5 < 3      // false
5 >= 5     // true
5 <= 4     // false

// Equality — the most important pair to understand
5 == "5"   // true  — loose: type coercion applied
5 === "5"  // false — strict: NO type coercion
5 != "5"   // false
5 !== "5"  // true  ← use this

// Object comparison — always by reference
{} === {}  // false
[] === []  // false
null == undefined   // true  (one exception where == is useful)
null === undefined  // false
NaN === NaN        // false  (NaN is never equal to itself!)
```

---

## 3.3 Logical Operators

```javascript
// AND — returns first falsy, or last value
true && true; // true
true && false; // false
"Alice" && 25; // 25 (both truthy → returns last)
null && "hi"; // null (first falsy → returns it)

// OR — returns first truthy, or last value
false || true; // true
"" || "guest"; // "guest" (first falsy, returns second)
"Alice" || "Bob"; // "Alice" (first truthy → returns it)

// NOT
!true; // false
!false; // true
!!0; // false (double-not converts to boolean)
!!"hello"; // true

// Nullish Coalescing ?? (ES2020) — only checks null/undefined
null ?? "default"; // "default"
undefined ?? "default"; // "default"
0 ?? "default"; // 0 ← IMPORTANT difference from ||
"" ?? "default"; // "" ← IMPORTANT difference from ||
false ?? "default"; // false

// || vs ?? — when to use which:
const count = userCount || 10; // BAD: 0 becomes 10
const count = userCount ?? 10; // GOOD: 0 stays 0

// Logical Assignment (ES2021)
x ||= "default"; // x = x || "default"
x &&= newValue; // x = x && newValue
x ??= "fallback"; // x = x ?? "fallback"
```

---

## 3.4 Assignment Operators

```javascript
let x = 10;
x += 5; // x = x + 5 → 15
x -= 3; // x = x - 3 → 12
x *= 2; // x = x * 2 → 24
x /= 4; // x = x / 4 → 6
x %= 4; // x = x % 4 → 2
x **= 3; // x = x ** 3 → 8
```

---

## 3.5 Type Coercion — Deep Dive

Type coercion is JavaScript automatically converting one type to another. It's the source of most JS quirks.

```javascript
// String coercion (+ with string)
"5" + 3; // "53"  — number → string
"5" + true; // "5true"
"5" + null; // "5null"
"5" + undefined; // "5undefined"

// Numeric coercion (-, *, /, %)
"5" - 3; // 2    — string → number
"5" * "2"; // 10
"5" - true; // 4    — true → 1
"5" - null; // 5    — null → 0
"5" - undefined; // NaN — undefined → NaN
"abc" - 1; // NaN  — invalid string → NaN

// Boolean coercion (in conditionals)
if (1) {
  /* runs  */
}
if (0) {
  /* skips */
}
if ("") {
  /* skips */
}
if ("0") {
  /* runs — non-empty string! */
}
if ([]) {
  /* runs — arrays are truthy! */
}
if ({}) {
  /* runs — objects are truthy! */
}
```

**Falsy Values — memorize all 8:**

```
false    0    -0    0n    ""    ''    ``    null    undefined    NaN
```

**Everything else is truthy** — including `"0"`, `[]`, `{}`, `function(){}`, `-1`.

```javascript
// Explicit conversion (always prefer this)
Number("42"); // 42
Number(true); // 1
Number(false); // 0
Number(null); // 0
Number(undefined); // NaN
Number(""); // 0
Number("  "); // 0
Number("abc"); // NaN

String(42); // "42"
String(null); // "null"
String(undefined); // "undefined"

Boolean(0); // false
Boolean(""); // false
Boolean("false"); // true   ← common mistake!
```

---

## 3.6 Strict vs Loose Equality

```javascript
// Loose equality (==) coercion rules:
// 1. null == undefined → true (only case)
// 2. If types differ, convert to number then compare
// 3. NaN never equals anything

0 == false; // true  (false → 0)
"" == false; // true  (both → 0)
"1" == true; // true  (both → 1)
null == undefined; // true  (special rule)
null == false; // false (null only == undefined)
NaN == NaN; // false (NaN ≠ NaN always)

// Strict equality (===) — same type AND same value
0 === false; // false (different types)
"" === false; // false
null === undefined; // false
1 === 1; // true
"hi" === "hi"; // true
```

**Equality Decision Table:**

| Comparison          | ==       | ===      |
| ------------------- | -------- | -------- |
| `1 == "1"`          | ✅ true  | ❌ false |
| `0 == false`        | ✅ true  | ❌ false |
| `null == undefined` | ✅ true  | ❌ false |
| `NaN == NaN`        | ❌ false | ❌ false |
| `[] == false`       | ✅ true  | ❌ false |
| `"" == false`       | ✅ true  | ❌ false |

> **Rule:** Always use `===`. The only acceptable use of `==` is `value == null` which checks for both `null` AND `undefined` simultaneously.

### Exercise 3

```javascript
// What does each expression evaluate to?
console.log(1 + "2" + 3);
console.log(1 + 2 + "3");
console.log(true + true);
console.log([] + []);
console.log([] + {});
console.log({} + []);
```

<details>
<summary>Answers</summary>

```
"123"           — "2" triggers string concat left-to-right
"33"            — 1+2=3 first, then "3" triggers concat
2               — true(1) + true(1)
""              — [].toString() + [].toString() = "" + ""
"[object Object]" — [] → "", {} → "[object Object]"
"[object Object]" — same as above (in expression context)
```

</details>

---

# Module 4 — Control Flow

## 4.1 if / else

```javascript
const score = 82;

// Full if/else if/else chain
if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B"); // ← this runs
} else if (score >= 70) {
  console.log("C");
} else {
  console.log("Fail");
}

// Guard clause pattern — preferred for cleaner code
function processPayment(amount) {
  if (!amount) return "Amount required";
  if (amount < 0) return "Amount must be positive";
  if (amount > 10000) return "Amount exceeds limit";

  // Core logic — no nesting needed
  return chargeCard(amount);
}

// Ternary operator — for simple assignments
const status = score >= 60 ? "Pass" : "Fail";
const label = isLoggedIn ? "Log Out" : "Log In";

// Nested ternary — avoid, use if/else instead
// const grade = s>=90 ? "A" : s>=80 ? "B" : s>=70 ? "C" : "F"; // BAD
```

---

## 4.2 switch Statement

```javascript
const day = "Wednesday";

switch (day) {
  case "Monday":
  case "Tuesday":
  case "Wednesday":
  case "Thursday":
  case "Friday":
    console.log("Weekday — work time");
    break; // ← CRITICAL — prevent fall-through
  case "Saturday":
  case "Sunday":
    console.log("Weekend — rest time");
    break;
  default:
    console.log("Invalid day");
}

// switch uses strict equality (===) internally
switch (
  true // clever pattern for ranges
) {
  case score >= 90:
    grade = "A";
    break;
  case score >= 80:
    grade = "B";
    break;
  default:
    grade = "C";
}
```

> **Gotcha:** Forgetting `break` causes fall-through — execution continues into the next case. This is occasionally intentional but usually a bug.

---

## 4.3 Truthy and Falsy Values

```javascript
// All 8 falsy values:
if (false) console.log("falsy");
if (0) console.log("falsy");
if (-0) console.log("falsy");
if (0n) console.log("falsy"); // BigInt zero
if ("") console.log("falsy");
if (null) console.log("falsy");
if (undefined) console.log("falsy");
if (NaN) console.log("falsy");

// Everything else = truthy, including:
if ("0") console.log("truthy"); // non-empty string
if ([]) console.log("truthy"); // empty array
if ({}) console.log("truthy"); // empty object
if (-1) console.log("truthy"); // non-zero number
if (function () {}) console.log("truthy"); // any function

// Practical pattern
function greet(name) {
  if (!name) return "Hello, Guest!"; // catches null, undefined, ""
  return `Hello, ${name}!`;
}
```

---

## 4.4 Loops

### for Loop

```javascript
// Classic for — use when you know the count
for (let i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// Loop through array by index
const fruits = ["apple", "banana", "cherry"];
for (let i = 0; i < fruits.length; i++) {
  console.log(`${i}: ${fruits[i]}`);
}

// for...of — iterate VALUES of any iterable (ES6) ✅
for (const fruit of fruits) {
  console.log(fruit); // apple, banana, cherry
}

// for...in — iterate KEYS of an object
const person = { name: "Alice", age: 25 };
for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}
// ⚠️ Avoid for...in on arrays — also iterates inherited properties
```

### while Loop

```javascript
// while — condition checked BEFORE each iteration
let count = 0;
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}

// Typical use: unknown number of iterations
let attempts = 0;
while (!isConnected() && attempts < 5) {
  connect();
  attempts++;
}
```

### do...while Loop

```javascript
// do...while — executes ONCE minimum, checks after
let num;
do {
  num = Math.floor(Math.random() * 6) + 1;
  console.log(`Rolled: ${num}`);
} while (num !== 6);
// Keeps rolling until we get a 6

// Common use: menu/prompt that must show at least once
let choice;
do {
  choice = getUserInput("Enter 1-3:");
} while (!["1", "2", "3"].includes(choice));
```

---

## 4.5 break and continue

```javascript
// break — exits loop entirely
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0 1 2 3 4
}

// continue — skips to next iteration
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue; // skip even
  console.log(i); // 1 3 5 7 9
}

// Labeled break — for nested loops
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer; // breaks BOTH loops
    console.log(i, j);
  }
}
// 0,0  0,1  0,2  1,0
```

### Exercise 4 — FizzBuzz

```javascript
// Classic interview question:
// Numbers 1-100: "Fizz" for multiples of 3,
// "Buzz" for multiples of 5, "FizzBuzz" for both
for (let i = 1; i <= 100; i++) {
  // YOUR CODE HERE
}
```

<details>
<summary>Answer</summary>

```javascript
for (let i = 1; i <= 100; i++) {
  if (i % 15 === 0) console.log("FizzBuzz");
  else if (i % 3 === 0) console.log("Fizz");
  else if (i % 5 === 0) console.log("Buzz");
  else console.log(i);
}
// Check 15 first — order matters!
```

</details>

---

# Module 5 — Functions

## 5.1 Function Declaration

**Definition:** Named function using the `function` keyword. Fully **hoisted** — can be called before its definition in code.

```javascript
// Call before declaration — works due to hoisting
console.log(add(2, 3)); // 5 ✅

function add(a, b) {
  return a + b;
}

// Default parameters (ES6)
function greet(name = "World", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}
greet(); // "Hello, World!"
greet("Alice"); // "Hello, Alice!"
greet("Alice", "Hi"); // "Hi, Alice!"

// Rest parameters — collects remaining args into array
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4, 5); // 15
```

---

## 5.2 Function Expression

**Definition:** A function assigned to a variable. **Not hoisted** — must be defined before calling.

```javascript
// Cannot call before definition
// sayHi();  // TypeError: sayHi is not a function

const sayHi = function () {
  return "Hi!";
};

sayHi(); // "Hi!"

// Named function expression — better stack traces
const calculate = function calcSum(a, b) {
  return a + b;
};
// calcSum is only accessible inside the function itself
```

---

## 5.3 Arrow Functions (ES6)

**Definition:** Concise function syntax. **Does NOT** have its own `this`, `arguments`, `super`, or `prototype`.

```javascript
// Syntax variations
const double = (x) => x * 2; // single param (parens optional)
const double2 = (x) => x * 2; // no parens for single param
const add = (a, b) => a + b; // multiple params
const noParams = () => "hello"; // no params — parens required
const withBody = (x) => {
  // with block body
  const result = x * 2;
  return result; // explicit return needed
};
const makeObj = (name) => ({ name }); // returning object — wrap in ()

// Arrow functions shine in callbacks
const nums = [1, 2, 3, 4, 5];
const doubled = nums.map((n) => n * 2);
const evens = nums.filter((n) => n % 2 === 0);
const total = nums.reduce((sum, n) => sum + n, 0);
```

**Arrow vs Regular — `this` binding:**

```javascript
// Regular function — `this` is determined by HOW it's called
const timer1 = {
  name: "Regular Timer",
  start: function () {
    setTimeout(function () {
      console.log(this.name); // undefined — `this` is window/global
    }, 100);
  },
};

// Arrow function — `this` inherited from enclosing scope (lexical)
const timer2 = {
  name: "Arrow Timer",
  start: function () {
    setTimeout(() => {
      console.log(this.name); // "Arrow Timer" ✅ — lexical this
    }, 100);
  },
};

// When NOT to use arrow functions:
const obj = {
  name: "Alice",
  // ❌ Wrong — arrow's `this` is outer scope (window), not obj
  greet: () => `Hello, I'm ${this.name}`,
  // ✅ Correct — regular method
  greet2: function () {
    return `Hello, I'm ${this.name}`;
  },
};
```

---

## 5.4 Anonymous Functions

```javascript
// Anonymous — no name, used inline
setTimeout(function () {
  console.log("anonymous timeout");
}, 1000);

[1, 2, 3].forEach(function (n) {
  console.log(n);
});

// Named vs anonymous — matters for debugging
const fn1 = function () {
  throw new Error();
};
// Stack trace: "fn1" — name inferred from variable

const fn2 = function namedFn() {
  throw new Error();
};
// Stack trace: "namedFn" — explicit name, clearer
```

---

## 5.5 Callback Functions

**Definition:** A function passed as an argument to another function, called at a later point.

```javascript
// Basic callback pattern
function processTask(data, onSuccess, onError) {
  try {
    const result = doWork(data);
    onSuccess(result);
  } catch (err) {
    onError(err);
  }
}

processTask(
  myData,
  (result) => console.log("Success:", result),
  (err) => console.error("Failed:", err),
);

// Node.js style callbacks (error-first)
fs.readFile("file.txt", "utf8", function (err, data) {
  if (err) return console.error(err);
  console.log(data);
});

// Callbacks are synchronous or asynchronous
[1, 2, 3].map((n) => n * 2); // sync callback — runs immediately
setTimeout(() => {}, 1000); // async callback — runs after delay
```

---

## 5.6 Higher Order Functions (HOF)

**Definition:** A function that **takes a function as an argument** OR **returns a function** (or both).

```javascript
// HOF that accepts a function
function applyToAll(arr, fn) {
  return arr.map(fn);
}
applyToAll([1, 2, 3], (x) => x * 2); // [2,4,6]

// HOF that returns a function — "function factory"
function createMultiplier(factor) {
  return function (number) {
    // closes over `factor`
    return number * factor;
  };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);
const times10 = createMultiplier(10);

double(5); // 10
triple(4); // 12
times10(7); // 70

// HOF that does both
function compose(f, g) {
  return (x) => f(g(x));
}

const addOne = (x) => x + 1;
const doubleIt = (x) => x * 2;
const addOneThenDouble = compose(doubleIt, addOne);
addOneThenDouble(3) // doubleIt(addOne(3)) = doubleIt(4) = 8
  [
    // Built-in HOFs
    (1, 2, 3)
  ].map(fn) // HOF — takes fn
  [(1, 2, 3)].filter(fn) // HOF — takes fn
  [(1, 2, 3)].reduce(fn, 0); // HOF — takes fn
```

---

## 5.7 IIFE — Immediately Invoked Function Expression

**Definition:** A function that defines and executes itself immediately. Creates private scope.

```javascript
// Classic IIFE syntax
(function () {
  const privateVar = "I'm private!";
  console.log("Runs immediately");
})();

// Arrow IIFE
(() => {
  console.log("Arrow IIFE");
})();

// IIFE with parameters
(function (global, doc) {
  // use `global` instead of `window` — works in any env
  global.myLibrary = {};
})(window, document);

// IIFE with return value
const result = (function () {
  const config = loadConfig();
  return processConfig(config);
})();
```

**Why use IIFE:**

```
Before ES6 modules:  Used to create private scope, avoid global pollution
After ES6 modules:   Less common, but still useful for initialization code
```

---

## 5.8 Generator Functions

**Definition:** Functions that can **pause and resume** execution. Use `yield` to pause, return an iterator.

```javascript
function* sequence() {
  console.log("Start");
  yield 1; // pause here, return 1
  console.log("After 1");
  yield 2; // pause here, return 2
  console.log("After 2");
  yield 3;
  console.log("End");
}

const gen = sequence();

gen.next(); // logs "Start", returns { value: 1, done: false }
gen.next(); // logs "After 1", returns { value: 2, done: false }
gen.next(); // logs "After 2", returns { value: 3, done: false }
gen.next(); // logs "End", returns { value: undefined, done: true }

// Infinite sequence — safe because it pauses
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;
  }
}

const nextId = idGenerator();
nextId.next().value; // 1
nextId.next().value; // 2
nextId.next().value; // 3 — on demand, no infinite loop

// for...of works with generators
function* range(start, end) {
  for (let i = start; i <= end; i++) yield i;
}

for (const n of range(1, 5)) console.log(n); // 1 2 3 4 5
```

---

## 5.9 Async Functions

**Definition:** Functions declared with `async`. Always return a Promise. Enable `await` syntax inside.

```javascript
// Async function declaration
async function fetchUser(id) {
  const res = await fetch(`/api/users/${id}`);
  return res.json(); // automatically wrapped in Promise.resolve()
}

// Async arrow function
const getUser = async (id) => {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
};

// Error handling
async function loadData() {
  try {
    const data = await fetchUser(1);
    return data;
  } catch (err) {
    console.error("Failed:", err.message);
    return null;
  }
}
```

---

## 5.10 Functions — Complete Comparison Table

| Type        | Syntax                    | Hoisted | `this`  | Use Case                   |
| ----------- | ------------------------- | ------- | ------- | -------------------------- |
| Declaration | `function fn(){}`         | ✅ Full | Dynamic | General purpose            |
| Expression  | `const fn = function(){}` | ❌      | Dynamic | Assignment, conditionals   |
| Arrow       | `const fn = () => {}`     | ❌      | Lexical | Callbacks, short functions |
| Anonymous   | `function() {}`           | N/A     | Dynamic | Inline callbacks           |
| IIFE        | `(function(){})()`        | N/A     | Dynamic | Immediate execution        |
| Generator   | `function* fn(){}`        | ✅      | Dynamic | Lazy sequences             |
| Async       | `async function fn(){}`   | ✅      | Dynamic | Async operations           |

### Exercise 5

```javascript
// 1. Create a function factory `power(exponent)` that returns
//    a function which raises its argument to that exponent
const square = power(2);
const cube = power(3);
square(4); // 16
cube(2); // 8

// 2. Create a memoize HOF that caches results
const memoizedFib = memoize(function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
});
```

<details>
<summary>Answers</summary>

```javascript
// power factory
function power(exponent) {
  return (base) => base ** exponent;
}

// memoize HOF
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}
```

</details>

---

# Module 6 — Arrays and Objects

## 6.1 Why Arrays Are Objects in JavaScript

```javascript
typeof []; // "object" ← arrays ARE objects
typeof {}; // "object"
Array.isArray([]); // true ← proper check for arrays

// Arrays are objects with numeric keys + special length property
const arr = ["a", "b", "c"];

// Under the hood it's essentially:
// { 0: "a", 1: "b", 2: "c", length: 3 }

arr[0]; // "a"
arr.length; // 3
arr.hasOwnProperty(0); // true

// You can even add named properties (don't do this)
arr.name = "myArray";
arr.name; // "myArray"
arr.length; // still 3 — named props don't affect length
```

---

## 6.2 Array Creation and Basics

```javascript
// Creation methods
const a1 = [1, 2, 3]; // literal (preferred)
const a2 = new Array(3); // [empty × 3]
const a3 = new Array(1, 2, 3); // [1, 2, 3]
const a4 = Array.from("hello"); // ["h","e","l","l","o"]
const a5 = Array.from({ length: 5 }, (_, i) => i); // [0,1,2,3,4]
const a6 = Array.of(1, 2, 3); // [1, 2, 3]

// Access
arr[0]; // first
arr[arr.length - 1]; // last
arr.at(-1); // last (ES2022) ✅
arr.at(-2); // second to last

// Mutation methods (change original)
arr.push("d"); // add to end → returns new length
arr.pop(); // remove from end → returns removed item
arr.unshift("x"); // add to start → returns new length
arr.shift(); // remove from start → returns removed item
arr.splice(1, 2); // remove 2 items from index 1
arr.splice(1, 0, "x"); // insert "x" at index 1
arr.reverse(); // reverses in-place
arr.sort(); // sorts in-place (alphabetically by default!)
arr.sort((a, b) => a - b); // numeric sort
arr.fill(0, 1, 3); // fill with 0 from index 1 to 3
```

---

## 6.3 Array Methods — The Core Four

### map — Transform each element

```javascript
// map(callback) → NEW array of transformed values
// Signature: callback(currentValue, index, array)

const nums = [1, 2, 3, 4, 5];

const doubled = nums.map((n) => n * 2);
// [2, 4, 6, 8, 10]  — original unchanged

// Real-world: shape data for UI
const users = [
  { id: 1, firstName: "Alice", lastName: "Smith" },
  { id: 2, firstName: "Bob", lastName: "Jones" },
];

const displayNames = users.map((user) => ({
  id: user.id,
  name: `${user.firstName} ${user.lastName}`,
}));
// [{ id:1, name:"Alice Smith" }, { id:2, name:"Bob Jones" }]
```

### filter — Keep elements that pass a test

```javascript
// filter(callback) → NEW array of elements where callback returns true

const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const evens = nums.filter((n) => n % 2 === 0);
// [2, 4, 6, 8, 10]

// Real-world: filter products
const products = [
  { name: "Laptop", price: 999, inStock: true, category: "tech" },
  { name: "Phone", price: 599, inStock: false, category: "tech" },
  { name: "Shirt", price: 39, inStock: true, category: "clothing" },
  { name: "Tablet", price: 449, inStock: true, category: "tech" },
];

const availableTech = products.filter(
  (p) => p.inStock && p.category === "tech",
);
// Laptop, Tablet
```

### reduce — Accumulate to a single value

```javascript
// reduce(callback, initialValue) → single value
// Signature: callback(accumulator, currentValue, index, array)

const nums = [1, 2, 3, 4, 5];

// Sum
const sum = nums.reduce((acc, n) => acc + n, 0);
// Step: 0+1=1, 1+2=3, 3+3=6, 6+4=10, 10+5=15

// Max value
const max = nums.reduce((acc, n) => (n > acc ? n : acc), -Infinity);

// Count occurrences
const words = ["apple", "banana", "apple", "cherry", "banana", "apple"];
const count = words.reduce((acc, word) => {
  acc[word] = (acc[word] || 0) + 1;
  return acc;
}, {});
// { apple: 3, banana: 2, cherry: 1 }

// Group by
const people = [
  { name: "Alice", dept: "Engineering" },
  { name: "Bob", dept: "Design" },
  { name: "Carol", dept: "Engineering" },
];

const byDept = people.reduce((acc, person) => {
  const dept = person.dept;
  if (!acc[dept]) acc[dept] = [];
  acc[dept].push(person);
  return acc;
}, {});
// { Engineering: [Alice, Carol], Design: [Bob] }
```

### forEach — Execute for side effects

```javascript
// forEach(callback) → undefined (NOT for building arrays)
// Use for side effects: logging, updating DOM, etc.

const cart = [
  { name: "Apple", price: 1.5 },
  { name: "Bread", price: 2.99 },
  { name: "Milk", price: 3.49 },
];

cart.forEach((item, index) => {
  console.log(`${index + 1}. ${item.name}: $${item.price}`);
});

// ⚠️ forEach cannot be stopped with break
// ⚠️ forEach ignores return values
// Use for...of if you need break/continue
```

---

## 6.4 More Array Methods

```javascript
const arr = [1, 2, 3, 4, 5];

// Search
arr.find(n => n > 3)          // 4 (first match or undefined)
arr.findIndex(n => n > 3)     // 3 (index of first match or -1)
arr.findLast(n => n < 4)      // 3 (ES2023)
arr.includes(3)               // true
arr.indexOf(3)                // 2 (or -1)
arr.lastIndexOf(3)            // 2

// Test
arr.some(n => n > 4)          // true (at least one)
arr.every(n => n > 0)         // true (all match)

// Transform
arr.flat()                    // flatten one level
[[1,2],[3,4]].flat()          // [1,2,3,4]
[1,[2,[3,[4]]]].flat(Infinity) // [1,2,3,4]
arr.flatMap(n => [n, n*2])    // [1,2,2,4,3,6,4,8,5,10]

// Copy (non-mutating versions — ES2023)
arr.toSorted((a,b) => b-a)    // [5,4,3,2,1] — new array
arr.toReversed()              // [5,4,3,2,1] — new array
arr.toSpliced(1, 2)           // removes 2 at index 1 — new array
arr.with(2, 99)               // replace index 2 with 99 — new array

// Combine
[...arr1, ...arr2]            // spread merge
arr1.concat(arr2)             // same result

// Convert
arr.join(" - ")               // "1 - 2 - 3 - 4 - 5"
Array.from(new Set(arr))      // remove duplicates
```

---

## 6.5 Objects — Creation and Access

```javascript
// Object literal (most common)
const user = {
  id: 1,
  name: "Alice",
  age: 25,
  isActive: true,

  // ES6 shorthand — when key and variable name match
  // { name } instead of { name: name }

  address: {
    city: "NYC",
    country: "USA",
  },

  // Method shorthand (ES6)
  greet() {
    return `Hi, I'm ${this.name}`;
  },

  // Computed property keys
  [`prop_${Date.now()}`]: "dynamic key",
};

// Access
user.name; // "Alice" — dot notation
user["name"]; // "Alice" — bracket notation
user.address.city; // "NYC"   — chained
user?.profile?.avatar; // safe — optional chaining

// Modify
user.name = "Bob";
user["age"] = 26;
delete user.isActive;

// Check existence
"name" in user; // true (includes prototype)
user.hasOwnProperty("name"); // true (own only)
Object.hasOwn(user, "name"); // true (ES2022, preferred)
```

---

## 6.6 Object Methods

```javascript
const obj = { a: 1, b: 2, c: 3 };

// Enumerate
Object.keys(obj); // ["a", "b", "c"]
Object.values(obj); // [1, 2, 3]
Object.entries(obj); // [["a",1], ["b",2], ["c",3]]

// Reconstruct
Object.fromEntries([
  ["a", 1],
  ["b", 2],
]); // { a:1, b:2 }
Object.fromEntries(Object.entries(obj).map(([k, v]) => [k, v * 2]));

// Copy and merge
Object.assign({}, obj); // shallow copy
Object.assign({}, obj1, obj2); // merge (obj2 wins conflicts)
const copy = { ...obj }; // spread (preferred)

// Control
Object.freeze(obj); // immutable — no add/modify/delete
Object.seal(obj); // can modify existing, no add/delete
Object.isFrozen(obj); // check if frozen

// Define property with descriptor
Object.defineProperty(obj, "id", {
  value: 42,
  writable: false,
  enumerable: true,
  configurable: false,
});

// Iterate
for (const [key, value] of Object.entries(obj)) {
  console.log(`${key}: ${value}`);
}
```

---

## 6.7 Destructuring

### Array Destructuring

```javascript
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3

// Skip elements
const [, , third] = [10, 20, 30]; // 30

// Default values
const [x = 0, y = 0] = [5]; // x=5, y=0

// Rest
const [head, ...tail] = [1, 2, 3, 4];
// head=1, tail=[2,3,4]

// Swap variables (elegant!)
let m = 1,
  n = 2;
[m, n] = [n, m];
// m=2, n=1

// From function return
function getCoords() {
  return [40.7128, -74.006];
}
const [lat, lng] = getCoords();
```

### Object Destructuring

```javascript
const user = { name: "Alice", age: 25, role: "admin", city: "NYC" };

// Basic
const { name, age } = user;

// Rename
const { name: userName, age: userAge } = user;

// Default values
const { name, role = "user", status = "active" } = user;

// Nested
const {
  address: { city, zip = "00000" },
} = { address: { city: "NYC" } };

// Rest
const { name, ...rest } = user;
// rest = { age:25, role:"admin", city:"NYC" }

// In function parameters — very common pattern in React/Node
function createUser({ name, age, role = "user" }) {
  return { name, age, role, createdAt: new Date() };
}
createUser({ name: "Bob", age: 30 });

// Rename + default in parameters
function render({ title = "Untitled", theme: colorTheme = "light" } = {}) {
  console.log(title, colorTheme);
}
```

---

## 6.8 Spread and Rest Operators

```javascript
// ─── SPREAD (...) ───────────────────────────────────────────
// Expands an iterable/object into individual elements

// Arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const merged = [...arr1, ...arr2]; // [1,2,3,4,5,6]
const copy = [...arr1]; // shallow copy
const withExtra = [...arr1, 0, ...arr2]; // [1,2,3,0,4,5,6]

// Objects
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const merged2 = { ...obj1, ...obj2 }; // { a:1, b:2, c:3, d:4 }
const updated = { ...user, age: 26 }; // override age

// Function calls
Math.max(...[3, 1, 4, 1, 5, 9]); // 9
console.log(...["a", "b", "c"]); // a b c

// ─── REST (...) ──────────────────────────────────────────────
// Collects remaining elements into an array

// In function parameters
function log(first, second, ...rest) {
  console.log(first, second, rest);
}
log(1, 2, 3, 4, 5); // 1, 2, [3,4,5]

// In destructuring
const [h, ...t] = [1, 2, 3, 4]; // h=1, t=[2,3,4]
const { a, ...others } = { a: 1, b: 2, c: 3 }; // others={b:2,c:3}
```

**Spread vs Rest — same syntax, opposite purpose:**

```
SPREAD: takes array/object → expands into individual parts
REST:   takes individual parts → collects into array/object
```

### Exercise 6

```javascript
// 1. Given this data, get an array of full names of active users over 18
const users = [
  { first: "Alice", last: "Smith", age: 25, active: true },
  { first: "Bob", last: "Jones", age: 16, active: true },
  { first: "Carol", last: "White", age: 30, active: false },
  { first: "Dave", last: "Brown", age: 22, active: true },
];

// 2. Using reduce, count how many users are in each age group:
// under18, adult (18-64), senior (65+)

// 3. Deep clone this object WITHOUT using JSON.parse/stringify
const config = { db: { host: "localhost", port: 5432 }, debug: true };
```

<details>
<summary>Answers</summary>

```javascript
// 1.
users.filter((u) => u.active && u.age >= 18).map((u) => `${u.first} ${u.last}`);
// ["Alice Smith", "Dave Brown"]

// 2.
users.reduce((acc, u) => {
  const group = u.age < 18 ? "under18" : u.age < 65 ? "adult" : "senior";
  acc[group] = (acc[group] || 0) + 1;
  return acc;
}, {});

// 3.
const clone = structuredClone(config); // ES2022
// or: { ...config, db: { ...config.db } } — manual shallow
```

</details>

---

# Module 7 — JavaScript Engine Core Concepts

## 7.1 Execution Context

**Definition:** The environment in which JavaScript code is evaluated and executed. Every time code runs, it runs inside an execution context.

```
Three types:
┌─────────────────────────────────────────────────────────┐
│ 1. GLOBAL EXECUTION CONTEXT (GEC)                       │
│    Created once when script starts                      │
│    • Creates global object (window/global)              │
│    • Sets `this` = global object                        │
│    • Hoists var declarations and function declarations  │
├─────────────────────────────────────────────────────────┤
│ 2. FUNCTION EXECUTION CONTEXT (FEC)                     │
│    Created each time a function is CALLED               │
│    • Has its own variable environment                   │
│    • Has reference to outer scope                       │
│    • Has its own `this` value                           │
├─────────────────────────────────────────────────────────┤
│ 3. EVAL EXECUTION CONTEXT                               │
│    Created by eval() — avoid!                           │
└─────────────────────────────────────────────────────────┘
```

**Two phases of every execution context:**

```
PHASE 1 — CREATION (Memory allocation)
─────────────────────────────────────────────────────────
• var variables → hoisted, set to undefined
• function declarations → hoisted completely
• let/const → hoisted but in TDZ (not accessible yet)
• arguments object created (in functions)
• this is determined

PHASE 2 — EXECUTION (Code runs line by line)
─────────────────────────────────────────────────────────
• Variables assigned their real values
• Functions execute
• Expressions evaluated
```

```javascript
// Visualizing the execution context:
var name = "Alice"; // Phase 1: name = undefined
// Phase 2: name = "Alice"

function greet() {
  var msg = "Hello"; // new FEC created
  return msg + " " + name; // name from outer scope
}

greet(); // new FEC pushed to call stack
```

---

## 7.2 Call Stack

**Definition:** A LIFO (Last In, First Out) data structure that tracks which function is currently executing.

```
Example code:
─────────────
function c() { return "done"; }
function b() { return c(); }
function a() { return b(); }
a();

Execution flow (call stack):
────────────────────────────────────────────────────────
STEP 1: script starts
┌─────────────────────┐
│  Global Context     │ ← always at bottom
└─────────────────────┘

STEP 2: a() is called
┌─────────────────────┐
│  a()                │ ← top of stack = currently executing
├─────────────────────┤
│  Global Context     │
└─────────────────────┘

STEP 3: a() calls b()
┌─────────────────────┐
│  b()                │ ← now executing
├─────────────────────┤
│  a()                │ ← waiting
├─────────────────────┤
│  Global Context     │
└─────────────────────┘

STEP 4: b() calls c()
┌─────────────────────┐
│  c()                │ ← now executing
├─────────────────────┤
│  b()                │ ← waiting
├─────────────────────┤
│  a()                │ ← waiting
├─────────────────────┤
│  Global Context     │
└─────────────────────┘

STEP 5: c() returns → popped off stack
STEP 6: b() returns → popped off stack
STEP 7: a() returns → popped off stack
```

**Stack Overflow:**

```javascript
function infinite() {
  return infinite(); // never terminates
}
infinite();
// RangeError: Maximum call stack size exceeded
```

---

## 7.3 Event Loop — The Full Picture

**Definition:** The mechanism that allows JavaScript (single-threaded) to handle asynchronous operations without blocking. It continuously checks: "Is the call stack empty? Is there anything in the queue?"

```
┌─────────────────────────────────────────────────────────────────┐
│                      JAVASCRIPT RUNTIME                          │
│                                                                  │
│  ┌───────────────────┐        ┌──────────────────────────────┐  │
│  │    CALL STACK      │        │          WEB APIs             │  │
│  │                   │        │  (Browser / Node environment) │  │
│  │   [              ]│        │                               │  │
│  │   [  fn()        ]│──────►│  setTimeout, setInterval      │  │
│  │   [  main()      ]│        │  fetch / XMLHttpRequest       │  │
│  │                   │        │  DOM events (click, etc.)     │  │
│  └────────┬──────────┘        │  fs.readFile (Node.js)        │  │
│           │                   └────────────────┬─────────────┘  │
│           │ empty?                              │ callback ready  │
│           │                                     ▼                │
│  ┌────────▼──────────────────────────────────────────────────┐  │
│  │                      EVENT LOOP                            │  │
│  │                                                            │  │
│  │  while(true) {                                             │  │
│  │    if (callStack.isEmpty()) {                              │  │
│  │      if (microtaskQueue.hasItems()) {                      │  │
│  │        callStack.push(microtaskQueue.dequeue()); // first! │  │
│  │      } else if (macrotaskQueue.hasItems()) {               │  │
│  │        callStack.push(macrotaskQueue.dequeue());           │  │
│  │      }                                                     │  │
│  │    }                                                       │  │
│  │  }                                                         │  │
│  └────────────────────────────────────────────────────────┬──┘  │
│                                                            │     │
│  ┌─────────────────────────┐    ┌────────────────────────┐│     │
│  │   MICROTASK QUEUE        │    │  MACROTASK (CALLBACK)  ││     │
│  │  (HIGH PRIORITY)         │    │  QUEUE                 ││     │
│  │                          │    │  (LOWER PRIORITY)      ││     │
│  │  Promise .then()         │    │  setTimeout callbacks  ││     │
│  │  Promise .catch()        │    │  setInterval callbacks ││     │
│  │  queueMicrotask()        │    │  I/O callbacks         ││     │
│  │  MutationObserver        │    │  UI render events      ││     │
│  └─────────────────────────┘    └────────────────────────┘│     │
└─────────────────────────────────────────────────────────────────┘
```

**Step-by-step execution trace:**

```javascript
console.log("1 — sync");

setTimeout(() => console.log("2 — macrotask"), 0);

Promise.resolve().then(() => console.log("3 — microtask"));

queueMicrotask(() => console.log("4 — microtask"));

console.log("5 — sync");

// OUTPUT:
// 1 — sync
// 5 — sync
// 3 — microtask     ← microtasks BEFORE macrotasks
// 4 — microtask
// 2 — macrotask
```

**Why this order?**

1. Sync code runs first (fills and empties call stack)
2. ALL microtasks run before any macrotask (Promises drain completely)
3. ONE macrotask runs, then ALL resulting microtasks, then next macrotask

---

## 7.4 Callback Queue and Microtask Queue

| Queue               | Contains                          | Priority                         | API                                 |
| ------------------- | --------------------------------- | -------------------------------- | ----------------------------------- |
| **Microtask Queue** | Promise callbacks, queueMicrotask | 🔴 HIGH — runs first             | `.then()`, `.catch()`, `.finally()` |
| **Macrotask Queue** | Timer callbacks, I/O callbacks    | 🟢 LOWER — runs after microtasks | `setTimeout`, `setInterval`, I/O    |

```javascript
// Starvation example — microtasks can block macrotasks!
function infiniteMicrotasks() {
  Promise.resolve().then(infiniteMicrotasks); // keeps queuing!
}
infiniteMicrotasks();
// setTimeout callback NEVER runs — macrotask queue starved
```

---

## 7.5 Hoisting

**Definition:** During the creation phase, declarations are moved ("hoisted") to the top of their scope. Only declarations are hoisted, not initializations.

```javascript
// ── var hoisting ────────────────────────────────────────────
console.log(x); // undefined — not ReferenceError!
var x = 5;
console.log(x); // 5

// What actually happens:
var x; // hoisted to top with value undefined
console.log(x); // undefined
x = 5; // assignment stays in place
console.log(x); // 5

// ── function declaration hoisting ───────────────────────────
greet(); // "Hello!" — works before declaration!
function greet() {
  console.log("Hello!");
}

// ── let/const — Temporal Dead Zone (TDZ) ────────────────────
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;

// let and const ARE hoisted, but not initialized → TDZ
//
// TDZ visualization:
//
// ┌── start of block scope ─────────────────────────────────┐
// │ [TDZ for y — exists in memory but inaccessible]         │
// │ console.log(y);  // ← ReferenceError! In TDZ            │
// │                                                         │
// │ let y = 10;      // ← TDZ ends here                     │
// │ console.log(y);  // 10 — now accessible                 │
// └─────────────────────────────────────────────────────────┘

// ── function expression NOT hoisted ─────────────────────────
sayHi(); // TypeError: sayHi is not a function
var sayHi = function () {
  console.log("Hi!");
};
// var sayHi is hoisted to undefined — calling undefined() = TypeError
```

---

## 7.6 Scope and Scope Chain

**Definition:** Scope determines where variables are accessible. The scope chain is how JavaScript looks up variables by traversing parent scopes.

```
Types of Scope:
┌─────────────────────────────────────────────────────────┐
│  GLOBAL SCOPE        → accessible everywhere            │
│  FUNCTION SCOPE      → accessible inside function       │
│  BLOCK SCOPE         → accessible inside {} (let/const) │
│  MODULE SCOPE        → accessible within module file    │
└─────────────────────────────────────────────────────────┘
```

```javascript
const globalVar = "global"; // Global scope

function outer() {
  const outerVar = "outer"; // Function scope

  function inner() {
    const innerVar = "inner"; // Function scope

    // Scope chain lookup:
    console.log(innerVar); // 1. own scope ✅
    console.log(outerVar); // 2. outer scope ✅
    console.log(globalVar); // 3. global scope ✅
    console.log(noSuchVar); // 4. not found → ReferenceError
  }
  inner();
}

/*
SCOPE CHAIN DIAGRAM:
─────────────────────────────────────────────────────────
inner scope
  │ innerVar = "inner"
  │ [not found] → look in parent scope
  ▼
outer scope
  │ outerVar = "outer"
  │ [not found] → look in parent scope
  ▼
global scope
  │ globalVar = "global"
  │ [not found] → ReferenceError
  ▼
null
*/
```

**Block Scope with let/const:**

```javascript
let x = "global";

{
  let x = "block"; // separate variable!
  console.log(x); // "block"
}

console.log(x); // "global" — outer unchanged

// var ignores block scope
{
  var y = "leaked!";
}
console.log(y); // "leaked!" — var escapes block!
```

---

## 7.7 Closures

**Definition:** A closure is a function that **remembers and accesses its lexical scope** even when executing outside of that scope. The inner function "closes over" the variables of its outer function.

```javascript
// Basic closure
function makeCounter(start = 0) {
  let count = start; // closed-over variable

  return {
    increment() {
      return ++count;
    },
    decrement() {
      return --count;
    },
    reset() {
      count = start;
      return count;
    },
    value() {
      return count;
    },
  };
}

const counter = makeCounter(10);
counter.increment(); // 11
counter.increment(); // 12
counter.decrement(); // 11
counter.value(); // 11
// count is NOT accessible from outside — true privacy!
```

```
CLOSURE MEMORY DIAGRAM:
──────────────────────────────────────────────────────────
When makeCounter() executes and returns:

STACK:            HEAP:
makeCounter()     ┌────────────────────────────────┐
  popped off  →   │ Closure environment             │
                  │  count = 11                     │
                  │  start = 10                     │
                  └──────┬─────────────────────────┘
                         │ referenced by
                         ▼
                  ┌────────────────────────────────┐
                  │ { increment, decrement,         │
                  │   reset, value }                │
                  │  (each fn has [[Environment]]   │
                  │   pointing to closure env)      │
                  └────────────────────────────────┘
```

**Real-world closure patterns:**

```javascript
// 1. Data Privacy / Module pattern
function createBankAccount(initialBalance) {
  let balance = initialBalance; // private state

  return {
    deposit(amount) {
      if (amount <= 0) throw new Error("Invalid amount");
      balance += amount;
      return balance;
    },
    withdraw(amount) {
      if (amount > balance) throw new Error("Insufficient funds");
      balance -= amount;
      return balance;
    },
    getBalance: () => balance,
  };
}

const acct = createBankAccount(1000);
acct.deposit(500); // 1500
acct.withdraw(200); // 1300
// balance is NOT accessible directly — private!

// 2. Memoization (cache expensive results)
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      console.log("Cache hit!");
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// 3. Partial application
function multiply(a, b) {
  return a * b;
}
function partial(fn, ...presetArgs) {
  return function (...laterArgs) {
    return fn(...presetArgs, ...laterArgs);
  };
}
const double = partial(multiply, 2);
double(5); // 10
double(7); // 14
```

> **Interview Insight — The Classic Loop Bug:**

```javascript
// ❌ BROKEN — all print 3
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Why? var is function-scoped, there's only ONE `i`,
// all callbacks close over the SAME `i`, which is 3 by then.

// ✅ FIX 1 — use let (block-scoped, new `i` each iteration)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // 0, 1, 2
}

// ✅ FIX 2 — IIFE to capture value
for (var i = 0; i < 3; i++) {
  ((j) => setTimeout(() => console.log(j), 100))(i);
}
```

### Exercise 7

```javascript
// 1. Create a `once` function — only executes the first time,
//    returns cached result on subsequent calls
const initApp = once(() => {
  console.log("App initialized!");
  return { ready: true };
});
initApp(); // logs "App initialized!" → { ready: true }
initApp(); // silent → { ready: true }

// 2. Explain the output:
function makeAdders() {
  const adders = [];
  for (let i = 0; i < 3; i++) {
    adders.push((n) => n + i);
  }
  return adders;
}
const [add0, add1, add2] = makeAdders();
console.log(add0(10), add1(10), add2(10)); // ?

// 3. Create a rate limiter — function that can only be called
//    once every `delay` milliseconds
```

<details>
<summary>Answers</summary>

```javascript
// 1.
function once(fn) {
  let called = false;
  let result;
  return function (...args) {
    if (!called) {
      called = true;
      result = fn.apply(this, args);
    }
    return result;
  };
}

// 2. Output: 10, 11, 12
// let creates new binding per iteration, so each closure has independent i

// 3.
function rateLimit(fn, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      return fn.apply(this, args);
    }
  };
}
```

</details>

---

# Module 8 — Prototype and Inheritance

## 8.1 Prototypes

**Definition:** Every JavaScript object has an internal hidden property `[[Prototype]]` that points to another object (its prototype). When you access a property, JS looks it up the prototype chain.

```javascript
// All objects inherit from Object.prototype
const obj = { name: "Alice" };

// The prototype chain:
Object.getPrototypeOf(obj) === Object.prototype; // true
Object.getPrototypeOf(Object.prototype) === null; // true (end of chain)

// Array prototype chain:
const arr = [];
Object.getPrototypeOf(arr) === Array.prototype; // true
Object.getPrototypeOf(Array.prototype) === Object.prototype; // true
// arr → Array.prototype → Object.prototype → null
```

---

## 8.2 Prototype Chain — Diagram

```
PROTOTYPE CHAIN LOOKUP
──────────────────────────────────────────────────────────────

const dog = { name: "Rex", breed: "Lab" };

// Access dog.toString() — not defined on dog itself
//
// Step 1: Look in dog's own properties
//   dog: { name: "Rex", breed: "Lab" }    ← toString? NO
//         │
//         │ [[Prototype]]
//         ▼
// Step 2: Look in Object.prototype
//   Object.prototype: { toString, hasOwnProperty, ... }  ← FOUND!
//         │
//         │ [[Prototype]]
//         ▼
//        null  ← end of chain

// Complete example:
const animal = {
  breathes: true,
  describe() { return `I breathe: ${this.breathes}`; }
};

const mammal = Object.create(animal);  // mammal's proto = animal
mammal.warm_blooded = true;

const human = Object.create(mammal);   // human's proto = mammal
human.name = "Alice";

human.name          // "Alice" — own property
human.warm_blooded  // true — from mammal prototype
human.breathes      // true — from animal prototype
human.toString()    // from Object.prototype

//  human → mammal → animal → Object.prototype → null
```

---

## 8.3 Constructor Functions

**Definition:** Regular functions used with `new` to create objects. The `new` keyword performs four operations.

```javascript
function Person(name, age) {
  // `this` is the new empty object
  this.name = name;
  this.age = age;
  // `this` is automatically returned
}

// Add methods to prototype — shared across all instances!
// (not duplicated in each object = memory efficient)
Person.prototype.greet = function () {
  return `Hello, I'm ${this.name}`;
};

Person.prototype.birthday = function () {
  this.age++;
  return this.age;
};

const alice = new Person("Alice", 25);
const bob = new Person("Bob", 30);

alice.greet(); // "Hello, I'm Alice"
bob.greet(); // "Hello, I'm Bob"
// both use the SAME greet function from prototype

alice.hasOwnProperty("name"); // true — own property
alice.hasOwnProperty("greet"); // false — inherited from prototype
```

**What `new` does under the hood:**

```javascript
// new Person("Alice", 25) does this:
function newOperator(Constructor, ...args) {
  // 1. Create empty object
  const obj = {};

  // 2. Set prototype
  Object.setPrototypeOf(obj, Constructor.prototype);

  // 3. Call constructor with new object as `this`
  const result = Constructor.apply(obj, args);

  // 4. Return the object (or result if constructor returned an object)
  return result instanceof Object ? result : obj;
}
```

---

## 8.4 ES6 Classes

**Definition:** Classes are **syntactic sugar** over constructor functions and prototypes. Same prototype-based mechanics, cleaner syntax.

```javascript
class Animal {
  // Class field (ES2022) — instance property
  #sound = "..."; // private field — # prefix

  constructor(name, species) {
    this.name = name;
    this.species = species;
  }

  // Instance method → Animal.prototype.speak
  speak() {
    return `${this.name} says ${this.#sound}`;
  }

  // Getter
  get info() {
    return `${this.name} (${this.species})`;
  }

  // Setter
  set nickname(value) {
    this._nickname = value.trim();
  }

  // Static method — called on the CLASS, not instances
  static create(name, species) {
    return new Animal(name, species);
  }

  // Static property
  static kingdom = "Animalia";
}

const lion = new Animal("Simba", "Lion");
lion.speak(); // "Simba says ..."
lion.info; // "Simba (Lion)"
Animal.create("Nala", "Lion"); // static call
Animal.kingdom; // "Animalia"
// lion.#sound      // SyntaxError — private!
```

**Class vs Constructor Function:**

```javascript
// These are equivalent!

// Constructor function style
function Car(make, model) {
  this.make = make;
  this.model = model;
}
Car.prototype.describe = function () {
  return `${this.make} ${this.model}`;
};

// Class style (preferred)
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
  describe() {
    return `${this.make} ${this.model}`;
  }
}
// typeof Car === "function"  — classes are functions!
```

---

## 8.5 Inheritance with extends

```javascript
class Animal {
  constructor(name) {
    this.name = name;
    this.alive = true;
  }
  breathe() {
    return `${this.name} breathes`;
  }
  toString() {
    return `[Animal: ${this.name}]`;
  }
}

class Dog extends Animal {
  #tricks = [];

  constructor(name, breed) {
    super(name); // MUST call super() before `this`!
    this.breed = breed;
  }

  bark() {
    return `${this.name}: Woof!`;
  }
  learn(trick) {
    this.#tricks.push(trick);
  }
  perform() {
    return this.#tricks.join(", ") || "No tricks yet";
  }

  // Override parent method
  toString() {
    return `[Dog: ${this.name}, ${this.breed}]`;
  }
}

class GuideDog extends Dog {
  constructor(name, breed, owner) {
    super(name, breed);
    this.owner = owner;
  }

  guide() {
    return `${this.name} guides ${this.owner}`;
  }
}

const rex = new GuideDog("Rex", "Lab", "Alice");
rex.breathe(); // inherited from Animal
rex.bark(); // inherited from Dog
rex.guide(); // own method
rex instanceof GuideDog; // true
rex instanceof Dog; // true
rex instanceof Animal; // true
```

**Prototype chain for classes:**

```
rex instance
  │ [[Prototype]]
  ▼
GuideDog.prototype  { guide }
  │ [[Prototype]]
  ▼
Dog.prototype       { bark, learn, perform, toString }
  │ [[Prototype]]
  ▼
Animal.prototype    { breathe, toString }
  │ [[Prototype]]
  ▼
Object.prototype    { hasOwnProperty, ... }
  │ [[Prototype]]
  ▼
null
```

### Exercise 8

```javascript
// Create a Shape class hierarchy:
// Shape (base): area(), perimeter(), toString()
// Circle extends Shape: radius
// Rectangle extends Shape: width, height
// Triangle extends Shape: a, b, c (sides)

// Each subclass should correctly implement area() and perimeter()
```

---

# Module 9 — Asynchronous JavaScript

## 9.1 Why Async JavaScript?

JavaScript is **single-threaded** — it can only do one thing at a time. Without async handling, network requests, file reads, and timers would freeze the browser.

```
WITHOUT ASYNC (blocking):
────────────────────────────────────────────────────────
Fetch user data... [===== 2 seconds =====]
                                           Fetch posts...
                                                         [=== 1 sec ===]
                                                                         Done
Total: 3 seconds, UI frozen entire time

WITH ASYNC (non-blocking):
────────────────────────────────────────────────────────
Fetch user data... [===== 2 seconds =====]
Fetch posts...     [=== 1 sec ===]
                                  |
                                  Both complete after ~2 seconds
                                  UI responsive the whole time
```

---

## 9.2 Callbacks

```javascript
// Basic async callback — Node.js style
const fs = require("fs");
fs.readFile("data.txt", "utf8", function (err, data) {
  if (err) {
    console.error("Error:", err);
    return;
  }
  console.log("Data:", data);
});

// setTimeout — most common async callback
console.log("before");
setTimeout(function () {
  console.log("async"); // runs after all sync code
}, 1000);
console.log("after");
// Output: before → after → async (after 1 second)
```

---

## 9.3 Callback Hell — The Problem

```javascript
// Real scenario: get user → get their posts → get first post comments
// With callbacks — "Pyramid of Doom"
getUser(userId, function (err, user) {
  if (err) return handleError(err);

  getPosts(user.id, function (err, posts) {
    if (err) return handleError(err);

    getComments(posts[0].id, function (err, comments) {
      if (err) return handleError(err);

      getAuthor(comments[0].authorId, function (err, author) {
        if (err) return handleError(err);

        // Finally doing actual work — 4 levels deep!
        render({ user, posts, comments, author });
      });
    });
  });
});

// Problems:
// 1. Hard to read — deeply nested
// 2. Error handling duplicated at every level
// 3. Hard to debug
// 4. Difficult to add parallel operations
```

---

## 9.4 Promises — The Solution

**Definition:** A Promise is an object representing the eventual completion or failure of an asynchronous operation. It has three states: pending, fulfilled, or rejected.

```
Promise State Machine:
──────────────────────────────────────────────────────────
                    ┌─────────────┐
                    │   PENDING   │ ← initial state
                    └──────┬──────┘
                           │
             ┌─────────────┴──────────────┐
             │                            │
             ▼                            ▼
      ┌────────────┐              ┌──────────────┐
      │  FULFILLED │              │   REJECTED   │
      │  (success) │              │   (failure)  │
      └────────────┘              └──────────────┘
             │                            │
             ▼                            ▼
         .then()                       .catch()
             │                            │
             └────────────┬───────────────┘
                          ▼
                      .finally()
```

```javascript
// Creating a Promise
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    // async operation
    fetch(`https://api.example.com/users/${id}`)
      .then((res) => {
        if (!res.ok) reject(new Error(`HTTP ${res.status}`));
        else resolve(res.json());
      })
      .catch(reject); // network errors
  });
}

// Consuming a Promise
fetchUser(1)
  .then((user) => {
    console.log("Got user:", user.name);
    return user; // pass to next .then()
  })
  .then((user) => user.email) // chain transforms
  .catch((err) => {
    console.error("Failed:", err.message);
  })
  .finally(() => {
    hideLoadingSpinner(); // always runs
  });
```

---

## 9.5 Promise Methods

```javascript
// Promise.all — all must succeed, returns array of results
// If ANY rejects → immediately rejects
const [user, posts, settings] = await Promise.all([
  fetchUser(1),
  fetchPosts(1),
  fetchSettings(1),
]);
// All run IN PARALLEL — much faster than sequential!

// Promise.allSettled — all run, get each result (ES2020)
// Never rejects — returns status for each
const results = await Promise.allSettled([
  fetchUser(1), // might fail
  fetchPosts(1), // might fail
  fetchSettings(1), // might fail
]);
results.forEach((result) => {
  if (result.status === "fulfilled") console.log(result.value);
  else console.log("Failed:", result.reason);
});

// Promise.race — first to settle wins
// Useful for timeouts
function timeout(ms) {
  return new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Timeout!")), ms),
  );
}
const data = await Promise.race([
  fetchUser(1),
  timeout(5000), // fail if takes more than 5 seconds
]);

// Promise.any — first to SUCCEED wins (ES2021)
// Only rejects if ALL reject
const fastestSource = await Promise.any([
  fetchFromServer1(data),
  fetchFromServer2(data),
  fetchFromServer3(data),
]);
```

---

## 9.6 async / await

**Definition:** `async/await` is syntactic sugar over Promises. Makes async code look and behave like synchronous code.

```javascript
// Solving "callback hell" with async/await — clean and readable!
async function loadPageData(userId) {
  try {
    // Sequential — each awaits the previous
    const user = await getUser(userId);
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    const author = await getAuthor(comments[0].authorId);

    render({ user, posts, comments, author });
    return { user, posts };
  } catch (err) {
    // ONE catch handles ALL errors above
    handleError(err);
    throw err; // re-throw if needed
  } finally {
    hideLoadingSpinner();
  }
}

// Parallel async — don't await one at a time if independent!
async function loadDashboard(userId) {
  // ❌ Slow — sequential (each waits for previous)
  const user = await fetchUser(userId);
  const posts = await fetchPosts(userId);
  const follows = await fetchFollows(userId);
  // Total: t(user) + t(posts) + t(follows)

  // ✅ Fast — all start simultaneously
  const [user, posts, follows] = await Promise.all([
    fetchUser(userId),
    fetchPosts(userId),
    fetchFollows(userId),
  ]);
  // Total: max(t(user), t(posts), t(follows))
}
```

---

## 9.7 Fetch API — Real Examples

```javascript
// Complete API service class
class ApiService {
  constructor(baseURL, token = null) {
    this.baseURL = baseURL;
    this.token = token;
  }

  get headers() {
    const h = { "Content-Type": "application/json" };
    if (this.token) h["Authorization"] = `Bearer ${this.token}`;
    return h;
  }

  async request(method, endpoint, body = null) {
    const options = { method, headers: this.headers };
    if (body) options.body = JSON.stringify(body);

    const res = await fetch(`${this.baseURL}${endpoint}`, options);

    if (!res.ok) {
      const error = await res.json().catch(() => ({}));
      throw new ApiError(error.message || res.statusText, res.status);
    }

    return res.status !== 204 ? res.json() : null;
  }

  get(endpoint) {
    return this.request("GET", endpoint);
  }
  post(endpoint, data) {
    return this.request("POST", endpoint, data);
  }
  put(endpoint, data) {
    return this.request("PUT", endpoint, data);
  }
  patch(endpoint, data) {
    return this.request("PATCH", endpoint, data);
  }
  delete(endpoint) {
    return this.request("DELETE", endpoint);
  }
}

// Custom error class
class ApiError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = "ApiError";
    this.statusCode = statusCode;
  }
}

// Usage
const api = new ApiService("https://jsonplaceholder.typicode.com");

async function example() {
  try {
    const users = await api.get("/users");
    const newUser = await api.post("/users", {
      name: "Alice",
      email: "alice@example.com",
    });
    const updated = await api.put(`/users/${newUser.id}`, {
      name: "Alice Updated",
    });
    await api.delete(`/users/${newUser.id}`);
  } catch (err) {
    if (err instanceof ApiError) {
      console.error(`API ${err.statusCode}: ${err.message}`);
    } else {
      console.error("Network error:", err.message);
    }
  }
}
```

---

## 9.8 Error Handling Patterns

```javascript
// 1. try/catch/finally
async function fetchData(url) {
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error(`HTTP Error: ${res.status}`);
    return await res.json();
  } catch (err) {
    if (err.name === "TypeError") {
      console.error("Network/CORS error:", err);
    } else {
      console.error("Request failed:", err.message);
    }
    return null;
  } finally {
    // cleanup — always runs
  }
}

// 2. Promise .catch()
fetchData(url).then(process).catch(console.error);

// 3. Utility: wrap async in safe result pattern
async function safeAsync(fn) {
  try {
    const data = await fn();
    return [null, data];
  } catch (err) {
    return [err, null];
  }
}

// Usage — Go-style error handling
const [err, user] = await safeAsync(() => fetchUser(1));
if (err) {
  /* handle error */
} else {
  /* use user */
}

// 4. Global unhandled rejections
window.addEventListener("unhandledrejection", (event) => {
  console.error("Unhandled rejection:", event.reason);
  event.preventDefault(); // stop console error in some environments
});
```

### Exercise 9

```javascript
// Build a retry wrapper for async functions
async function retry(fn, { attempts = 3, delay = 1000 } = {}) {
  // Try calling fn up to `attempts` times
  // Wait `delay` ms between attempts
  // Throw the last error if all attempts fail
}

// Usage:
const data = await retry(() => fetchFlakeyApi(), { attempts: 3, delay: 500 });
```

<details>
<summary>Answer</summary>

```javascript
async function retry(fn, { attempts = 3, delay = 1000 } = {}) {
  let lastError;
  for (let i = 0; i < attempts; i++) {
    try {
      return await fn();
    } catch (err) {
      lastError = err;
      if (i < attempts - 1) {
        await new Promise((resolve) => setTimeout(resolve, delay));
      }
    }
  }
  throw lastError;
}
```

</details>

---

# Module 10 — DOM Manipulation

## 10.1 What is the DOM?

```
HTML source:                    DOM Tree (in memory):
─────────────────               ──────────────────────────────
<!DOCTYPE html>                 Document
<html>                          └── html
  <head>                            ├── head
    <title>App</title>              │   └── title → "App"
  </head>                          └── body
  <body>                                ├── header
    <header>                            │   └── h1 → "Hello"
      <h1>Hello</h1>                    └── main
    </header>                               ├── p → "World"
    <main>                                  └── button → "Click"
      <p>World</p>
      <button>Click</button>
    </main>
  </body>
</html>
```

The DOM is a **live, tree-structured object representation** of the HTML document. JavaScript can read and modify it — changes reflect instantly in the browser.

---

## 10.2 Selecting Elements

```javascript
// Single element (returns first match or null)
document.getElementById("app"); // fastest — by ID
document.querySelector(".btn"); // CSS selector, first match
document.querySelector("nav > ul > li:first-child"); // complex selector

// Multiple elements
document.querySelectorAll(".card"); // NodeList (static snapshot)
document.getElementsByClassName("item"); // HTMLCollection (live)
document.getElementsByTagName("p"); // HTMLCollection (live)

// Working with NodeList
const items = document.querySelectorAll("li");
items.forEach((item) => console.log(item.textContent));
[...items].filter((item) => item.classList.contains("active"));

// Null safety
const btn = document.querySelector("#submit");
btn?.addEventListener("click", handler); // optional chaining
```

---

## 10.3 DOM Traversal

```javascript
const el = document.querySelector(".container");

// Children
el.children; // HTMLCollection of child elements
el.firstElementChild; // first child element
el.lastElementChild; // last child element
el.childNodes; // NodeList including text nodes

// Parent
el.parentElement; // immediate parent element
el.closest(".wrapper"); // nearest ancestor matching selector

// Siblings
el.nextElementSibling; // next sibling element
el.previousElementSibling; // previous sibling element

// Find within element
el.querySelector(".btn"); // search within `el` only
el.querySelectorAll("input"); // all inputs within `el`
```

---

## 10.4 Creating and Modifying Elements

```javascript
// Create elements
const card = document.createElement("div");
card.className = "card";
card.id = "user-card";
card.dataset.userId = "123"; // data-user-id="123"

// Content — choose carefully
card.textContent = "Safe text — no HTML parsing";
card.innerHTML = "<strong>Bold</strong>"; // ⚠️ XSS risk with user input!

// Attributes
card.setAttribute("aria-label", "User card");
card.getAttribute("class");
card.removeAttribute("hidden");
card.hasAttribute("disabled");

// Classes
card.classList.add("active", "visible");
card.classList.remove("hidden");
card.classList.toggle("selected");
card.classList.contains("active"); // true/false
card.classList.replace("old", "new");

// Styles
card.style.color = "red";
card.style.backgroundColor = "#f0f0f0";
card.style.display = "none"; // hide
card.style.cssText = "color: red; font-size: 16px;"; // bulk

// Insert into DOM
document.body.appendChild(card); // at end of body
document.body.prepend(card); // at start of body
container.insertBefore(card, referenceEl); // before reference
container.insertAdjacentHTML("beforeend", html); // HTML string
container.append(card, otherCard); // multiple at once

// insertAdjacentHTML positions:
// "beforebegin" — before the element itself
// "afterbegin"  — inside, before first child
// "beforeend"   — inside, after last child
// "afterend"    — after the element itself

// Remove and replace
card.remove();
container.replaceChild(newEl, oldEl);
oldEl.replaceWith(newEl);
container.innerHTML = ""; // clear all children
```

---

## 10.5 Events

```javascript
const btn = document.querySelector("#myBtn");

// Add event listener — preferred method
function handleClick(event) {
  console.log("clicked!");
  console.log(event.type); // "click"
  console.log(event.target); // element clicked
  console.log(event.currentTarget); // element with listener
  console.log(event.clientX); // mouse X position
}

btn.addEventListener("click", handleClick);
btn.removeEventListener("click", handleClick); // must be same reference

// One-time event
btn.addEventListener("click", handler, { once: true });

// Capture phase (default is bubble)
btn.addEventListener("click", handler, { capture: true });

// Common events
// Mouse:    click dblclick mouseenter mouseleave mousemove mousedown mouseup
// Keyboard: keydown keyup keypress
// Form:     submit change input focus blur
// Window:   load DOMContentLoaded resize scroll
// Touch:    touchstart touchend touchmove

// Prevent default
form.addEventListener("submit", (e) => {
  e.preventDefault(); // stop form refresh
  validateAndSubmit();
});

link.addEventListener("click", (e) => {
  e.preventDefault(); // stop navigation
});

// Stop propagation
child.addEventListener("click", (e) => {
  e.stopPropagation(); // don't bubble to parent
});
```

---

## 10.6 Event Delegation

**Definition:** Instead of attaching listeners to each element, attach ONE listener to a parent and check `event.target`. Handles dynamically-added elements automatically.

```javascript
// ❌ Inefficient — listener per button, dynamic buttons not handled
document.querySelectorAll(".action-btn").forEach((btn) => {
  btn.addEventListener("click", handleAction);
});

// ✅ Efficient — one listener, handles all current + future buttons
document.querySelector("#actions-container").addEventListener("click", (e) => {
  // Check what was actually clicked
  const btn = e.target.closest("[data-action]");
  if (!btn) return; // clicked something else

  const action = btn.dataset.action;
  const id = btn.dataset.id;

  switch (action) {
    case "edit":
      openEditor(id);
      break;
    case "delete":
      deleteItem(id);
      break;
    case "view":
      viewDetails(id);
      break;
  }
});

// Works for dynamically-added buttons too!
function addButton(id) {
  container.innerHTML += `
    <button data-action="delete" data-id="${id}">Delete ${id}</button>
  `; // ← automatically handled by existing listener
}
```

---

## 10.7 Practical DOM Example — Dynamic List

```javascript
class DynamicList {
  constructor(containerId) {
    this.container = document.querySelector(`#${containerId}`);
    this.items = [];
    this.render();
    this.bindEvents();
  }

  add(text) {
    this.items.push({ id: Date.now(), text, done: false });
    this.render();
  }

  toggle(id) {
    const item = this.items.find((i) => i.id === id);
    if (item) {
      item.done = !item.done;
      this.render();
    }
  }

  remove(id) {
    this.items = this.items.filter((i) => i.id !== id);
    this.render();
  }

  render() {
    this.container.innerHTML = `
      <ul class="list">
        ${this.items
          .map(
            (item) => `
          <li class="${item.done ? "done" : ""}" data-id="${item.id}">
            <span class="text">${item.text}</span>
            <button data-action="toggle">✓</button>
            <button data-action="remove">✕</button>
          </li>
        `,
          )
          .join("")}
      </ul>
    `;
  }

  bindEvents() {
    // Event delegation — one listener for all items
    this.container.addEventListener("click", (e) => {
      const li = e.target.closest("li");
      if (!li) return;
      const id = Number(li.dataset.id);
      const action = e.target.dataset.action;
      if (action === "toggle") this.toggle(id);
      if (action === "remove") this.remove(id);
    });
  }
}

const list = new DynamicList("app");
list.add("Learn JavaScript");
list.add("Build a project");
```

---

# Module 11 — Modern JavaScript (ES6+)

## 11.1 Key ES6+ Features Summary

```javascript
// ─── Arrow Functions ─────────────────────────────────────
const fn = (a, b) => a + b;

// ─── Template Literals ───────────────────────────────────
const msg = `Hello, ${name}! You are ${age} years old.`;
const multiline = `
  Line 1
  Line 2
`;
const tagged = html`<div>${userInput}</div>`;  // tagged templates

// ─── Destructuring ───────────────────────────────────────
const { name, age = 0 } = user;
const [first, ...rest] = arr;
function fn({ name, role = "user" }) { ... }  // in params

// ─── Spread / Rest ───────────────────────────────────────
const merged  = { ...obj1, ...obj2 };
const clone   = [...arr];
function sum(...args) { return args.reduce((a,b) => a+b); }

// ─── Optional Chaining (?.) ──────────────────────────────
const city    = user?.address?.city;           // no TypeError
const len     = user?.friends?.length ?? 0;
const name    = users?.[0]?.name;
const result  = fn?.();                        // call if fn exists

// ─── Nullish Coalescing (??) ─────────────────────────────
const count   = value ?? 0;      // 0 only for null/undefined
const label   = name ?? "Guest"; // NOT for falsy values

// ─── Logical Assignment ──────────────────────────────────
x ??= defaultValue;    // assign if null/undefined
x ||= fallback;        // assign if falsy
x &&= transform(x);   // assign if truthy

// ─── Short property / method names (ES6) ─────────────────
const x = 1, y = 2;
const point = { x, y };           // { x: 1, y: 2 }

// ─── Computed property keys ──────────────────────────────
const key = "name";
const obj = { [key]: "Alice" };   // { name: "Alice" }
const dynamic = { [`${prefix}_id`]: 42 };

// ─── String methods ──────────────────────────────────────
"hello".padStart(8, "0")     // "000hello"
"hello".padEnd(8, ".")       // "hello..."
"  hi  ".trimStart()         // "hi  "
"  hi  ".trimEnd()           // "  hi"
str.replaceAll("a", "b")     // replace all occurrences

// ─── Array methods ───────────────────────────────────────
arr.at(-1)                   // last element (ES2022)
arr.flat(depth)              // flatten nested
arr.flatMap(fn)              // map + flatten
arr.findLast(fn)             // find from end (ES2023)
Object.fromEntries(entries)  // entries → object (ES2019)

// ─── Object methods ──────────────────────────────────────
Object.hasOwn(obj, key)      // preferred over hasOwnProperty
structuredClone(obj)         // deep clone (ES2022)
```

---

## 11.2 Modules (import / export)

```javascript
// ─── math.js — Named exports ─────────────────────────────
export const PI = 3.14159265;

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

// ─── User.js — Default export ────────────────────────────
export default class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
  toString() {
    return `${this.name} <${this.email}>`;
  }
}

// ─── app.js — Importing ──────────────────────────────────
// Default import — name it anything
import User from "./User.js";

// Named imports — must match export names
import { add, multiply, PI } from "./math.js";

// Rename on import
import { add as sum } from "./math.js";

// Import all named exports as namespace
import * as Math from "./math.js";
Math.add(1, 2);
Math.PI;

// Import default + named
import User, { add, PI } from "./module.js";

// ─── Re-exporting (barrel files) ─────────────────────────
// index.js — consolidate exports
export { add, multiply } from "./math.js";
export { default as User } from "./User.js";
export * from "./utils.js";

// ─── Dynamic import — lazy loading ───────────────────────
// Only load when needed — great for performance
async function loadChart() {
  const { default: Chart } = await import("./chart.js");
  return new Chart(data);
}

// Route-based code splitting (React/webpack pattern)
const Dashboard = lazy(() => import("./Dashboard.js"));
```

```html
<!-- HTML: must declare type="module" -->
<script type="module" src="app.js"></script>
```

**Module scope rules:**

- Each module has its own scope (no global pollution)
- Modules are executed once (cached)
- Always in strict mode
- `import`/`export` only at top level (static analysis)

---

# Module 12 — JavaScript Memory Model

## 12.1 Stack Memory

**Definition:** Fast, fixed-size memory for primitive values and function call frames. Automatically managed — freed when function returns.

```
STACK MEMORY:
──────────────────────────────────────────────────────────
Properties: ordered, LIFO, fast access, limited size (~1-8MB)
Stores:     primitives, references (pointers), call frames

function outer() {
  let x = 10;           // pushed to stack
  let name = "Alice";   // pushed to stack

  function inner() {
    let y = 20;         // pushed to stack
  }                     // y popped when inner() returns
  inner();
}                       // x, name popped when outer() returns

STACK at peak:
┌─────────────────────────┐
│ inner frame: y = 20     │ ← top (most recent)
├─────────────────────────┤
│ outer frame: x=10,      │
│             name="Alice"│
├─────────────────────────┤
│ Global frame            │ ← bottom
└─────────────────────────┘
```

---

## 12.2 Heap Memory

**Definition:** Large, dynamic memory pool for objects, arrays, and functions. Managed by the garbage collector.

```
HEAP MEMORY:
──────────────────────────────────────────────────────────
Properties: large, flexible, slower access, unordered
Stores:     objects, arrays, functions, closures

const user = { name: "Alice" };   // object stored in heap
const arr  = [1, 2, 3];           // array stored in heap

STACK          HEAP
──────────      ─────────────────────────────────────────
│ user │ 0x001──► { name: "Alice", id: 1 } at 0x001
│ arr  │ 0x042──► [1, 2, 3]              at 0x042
──────────      ─────────────────────────────────────────

When you do: let user2 = user;
STACK          HEAP
──────────      ─────────────────────────────────────────
│ user  │ 0x001──► { name: "Alice" }
│ user2 │ 0x001──►  (same object!)
──────────
```

---

## 12.3 Garbage Collection

**Definition:** Automatic memory management. JavaScript's GC identifies objects that are no longer reachable and frees their memory.

**V8 Garbage Collection strategy:**

```
GENERATIONAL GC:
──────────────────────────────────────────────────────────
New Space (Young Generation)   Old Space (Old Generation)
──────────────────────────────  ──────────────────────────
• Small, frequently collected  • Larger, rarely collected
• Most objects die here         • Long-lived objects
• Minor GC (fast)               • Major GC (stop-the-world)

New objects → New Space → survive multiple GCs → Old Space

Reachability algorithm:
• Start from "roots" (global, stack, CPU registers)
• Mark all reachable objects
• Sweep (free) all unmarked objects
```

```javascript
// Object becomes garbage collectible when:
let obj = { data: "big data" }; // referenced

obj = null; // no more references → eligible for GC

// Reference keeping object alive:
function createTimer() {
  const bigData = new Array(1000000).fill("data");

  const interval = setInterval(() => {
    // bigData is captured in closure
    // → NOT garbage collected while interval runs!
    process(bigData);
  }, 1000);

  return () => clearInterval(interval); // cleanup function
}
const stop = createTimer();
// Later:
stop(); // clear interval → bigData can be GC'd
```

---

## 12.4 Memory Leaks

**Definition:** Memory that is no longer needed but NOT released because references to it still exist.

```javascript
// ──────────────────────────────────────────────────────────
// LEAK 1: Forgotten event listeners
// ──────────────────────────────────────────────────────────
function setupListener() {
  const bigData = new Array(10000).fill("leak");

  document.addEventListener("click", function handler() {
    console.log(bigData); // handler holds reference to bigData
  });
  // handler (and bigData) live forever on the document!
}

// Fix: store reference and remove when done
function setupListener() {
  const bigData = new Array(10000).fill("data");
  const handler = () => console.log(bigData.length);

  document.addEventListener("click", handler);

  return () => document.removeEventListener("click", handler);
}

// ──────────────────────────────────────────────────────────
// LEAK 2: Timers not cleared
// ──────────────────────────────────────────────────────────
class Component {
  constructor() {
    // ❌ Interval keeps running even after component is removed!
    this.interval = setInterval(() => this.update(), 1000);
  }

  destroy() {
    clearInterval(this.interval); // ✅ always clean up!
  }
}

// ──────────────────────────────────────────────────────────
// LEAK 3: Detached DOM nodes
// ──────────────────────────────────────────────────────────
let detachedNode;

function createDetached() {
  const div = document.createElement("div");
  document.body.appendChild(div);
  document.body.removeChild(div);
  detachedNode = div; // ← still referenced! Can't be GC'd
}

// Fix:
detachedNode = null; // release reference

// ──────────────────────────────────────────────────────────
// LEAK 4: Global variables
// ──────────────────────────────────────────────────────────
// ❌ Accidental global
function fn() {
  leakedVar = "I'm global!"; // missing let/const/var!
}

// ✅ Always declare variables
function fn() {
  const localVar = "I'm local";
}

// ──────────────────────────────────────────────────────────
// LEAK 5: Closures holding unnecessary data
// ──────────────────────────────────────────────────────────
function processData() {
  const hugeArray = new Array(1000000).fill("data");

  return function () {
    // hugeArray never used here but closure keeps it alive!
    return "done";
  };
}

// Fix: don't close over what you don't need
function processData() {
  const hugeArray = new Array(1000000).fill("data");
  const result = compute(hugeArray);
  // hugeArray not captured in closure → can be GC'd
  return function () {
    return result;
  };
}
```

**Detecting leaks with Chrome DevTools:**

```
1. Open DevTools → Memory tab
2. Take Heap Snapshot
3. Perform actions (navigate, use features)
4. Take another Heap Snapshot
5. Compare — look for objects that keep growing
6. Check "Retainers" to find what's keeping objects alive
```

---

# Module 13 — Real World JavaScript

## 13.1 JavaScript + React (Frontend)

```
JavaScript → DOM manipulation manually
React      → Handles DOM for you via Virtual DOM

React component = JavaScript function that returns UI
State changes   → React re-renders efficiently
```

```javascript
// React is JavaScript — all JS concepts apply directly

import { useState, useEffect, useCallback, useMemo } from "react";

// Closures → useState setter captures state
function Counter() {
  const [count, setCount] = useState(0);

  // Closures + HOFs → useCallback
  const increment = useCallback(() => {
    setCount((prev) => prev + 1); // closure over setCount
  }, []); // empty deps = stable reference

  return <button onClick={increment}>{count}</button>;
}

// Async/await → useEffect for data fetching
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false; // cleanup for race conditions

    async function loadUser() {
      try {
        setLoading(true);
        const res = await fetch(`/api/users/${userId}`);
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        const data = await res.json();
        if (!cancelled) setUser(data); // avoid setting state after unmount
      } catch (err) {
        if (!cancelled) setError(err.message);
      } finally {
        if (!cancelled) setLoading(false);
      }
    }

    loadUser();
    return () => {
      cancelled = true;
    }; // cleanup on unmount
  }, [userId]); // re-run when userId changes

  // Destructuring in JSX
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  const { name, email, address: { city } = {} } = user ?? {};
  return (
    <div>
      <h1>{name}</h1>
      <p>{email}</p>
      <p>{city}</p>
    </div>
  );
}

// Array methods → rendering lists
function UserList({ users }) {
  return (
    <ul>
      {users
        .filter((u) => u.active) // filter
        .sort((a, b) => a.name.localeCompare(b.name)) // sort
        .map(
          (
            user, // map to JSX
          ) => (
            <li key={user.id}>{user.name}</li>
          ),
        )}
    </ul>
  );
}
```

---

## 13.2 JavaScript + Node.js (Backend)

```
Browser JS:   window, document, DOM APIs
Node.js JS:   same language, different APIs
              → fs (file system), http, path, process
              → No window/document
              → CommonJS (require) or ES Modules (import)
```

```javascript
// Express REST API — Node.js

const express = require("express");
const app = express();
app.use(express.json());

// In-memory store (use a real DB in production)
const db = { users: [], nextId: 1 };

// Middleware — closures + HOFs in action
const auth = (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  req.user = verifyToken(token);
  next();
};

const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// CRUD routes
app.get(
  "/api/users",
  auth,
  asyncHandler(async (req, res) => {
    const { active, search } = req.query;
    let users = db.users;

    if (active !== undefined) {
      users = users.filter((u) => u.active === (active === "true"));
    }
    if (search) {
      users = users.filter((u) =>
        u.name.toLowerCase().includes(search.toLowerCase()),
      );
    }

    res.json({ data: users, total: users.length });
  }),
);

app.get(
  "/api/users/:id",
  auth,
  asyncHandler(async (req, res) => {
    const user = db.users.find((u) => u.id === Number(req.params.id));
    if (!user) return res.status(404).json({ error: "User not found" });
    res.json(user);
  }),
);

app.post(
  "/api/users",
  asyncHandler(async (req, res) => {
    const { name, email } = req.body;
    if (!name || !email) {
      return res.status(400).json({ error: "name and email required" });
    }

    const user = { id: db.nextId++, name, email, active: true };
    db.users.push(user);
    res.status(201).json(user);
  }),
);

app.put(
  "/api/users/:id",
  auth,
  asyncHandler(async (req, res) => {
    const index = db.users.findIndex((u) => u.id === Number(req.params.id));
    if (index === -1) return res.status(404).json({ error: "Not found" });
    db.users[index] = { ...db.users[index], ...req.body };
    res.json(db.users[index]);
  }),
);

app.delete(
  "/api/users/:id",
  auth,
  asyncHandler(async (req, res) => {
    const index = db.users.findIndex((u) => u.id === Number(req.params.id));
    if (index === -1) return res.status(404).json({ error: "Not found" });
    db.users.splice(index, 1);
    res.status(204).send();
  }),
);

// Global error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode ?? 500).json({ error: err.message });
});

app.listen(3000, () => console.log("Server running on :3000"));
```

---

## 13.3 APIs and Networking Patterns

```javascript
// Complete API client with interceptors, retry, caching
class HttpClient {
  #baseURL;
  #headers;
  #interceptors = { request: [], response: [] };
  #cache = new Map();

  constructor(baseURL, options = {}) {
    this.#baseURL = baseURL;
    this.#headers = {
      "Content-Type": "application/json",
      ...options.headers,
    };
  }

  use(type, fn) {
    this.#interceptors[type].push(fn);
    return this;
  }

  async #applyInterceptors(type, data) {
    let result = data;
    for (const fn of this.#interceptors[type]) {
      result = await fn(result);
    }
    return result;
  }

  async request(config) {
    config = await this.#applyInterceptors("request", {
      method: "GET",
      headers: { ...this.#headers },
      ...config,
      url: `${this.#baseURL}${config.url}`,
    });

    // Check cache for GET requests
    if (config.method === "GET" && this.#cache.has(config.url)) {
      return this.#cache.get(config.url);
    }

    const res = await fetch(config.url, config);
    let response = {
      status: res.status,
      ok: res.ok,
      data: res.status !== 204 ? await res.json() : null,
    };

    response = await this.#applyInterceptors("response", response);

    if (!response.ok) {
      const err = new Error(response.data?.message || "Request failed");
      err.status = response.status;
      throw err;
    }

    // Cache successful GET
    if (config.method === "GET") {
      this.#cache.set(config.url, response);
      setTimeout(() => this.#cache.delete(config.url), 60000); // 1 min TTL
    }

    return response;
  }

  get(url, params) {
    return this.request({
      url: url + (params ? "?" + new URLSearchParams(params) : ""),
      method: "GET",
    });
  }
  post(url, data) {
    return this.request({ url, method: "POST", body: JSON.stringify(data) });
  }
  put(url, data) {
    return this.request({ url, method: "PUT", body: JSON.stringify(data) });
  }
  patch(url, data) {
    return this.request({ url, method: "PATCH", body: JSON.stringify(data) });
  }
  delete(url) {
    return this.request({ url, method: "DELETE" });
  }
}

// Usage
const api = new HttpClient("https://api.example.com");

// Add auth interceptor
api.use("request", (config) => {
  const token = localStorage.getItem("token");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Add logging interceptor
api.use("response", (response) => {
  console.log(`Response: ${response.status}`);
  return response;
});
```

---

## 13.4 Full-Stack Pattern Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    FULL-STACK JS APP                         │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              FRONTEND (React / Vue / Angular)        │   │
│  │  Components · State Management · Routing             │   │
│  │  fetch/axios → REST API or GraphQL                   │   │
│  └─────────────────────────────┬───────────────────────┘   │
│                                │ HTTP / WebSocket            │
│  ┌─────────────────────────────▼───────────────────────┐   │
│  │              BACKEND (Node.js + Express)             │   │
│  │  Routes · Middleware · Business Logic                │   │
│  │  Auth (JWT/sessions) · Validation                    │   │
│  └─────────────────────────────┬───────────────────────┘   │
│                                │                            │
│  ┌─────────────────────────────▼───────────────────────┐   │
│  │              DATABASE LAYER                          │   │
│  │  MongoDB (Mongoose) · PostgreSQL (Prisma/Knex)       │   │
│  │  Redis (caching) · S3 (file storage)                 │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

# 📚 Interview Quick Reference

## Top 25 JavaScript Interview Questions

```
1.  What is the difference between var, let, and const?
    var: function-scoped, hoisted to undefined, re-declarable
    let: block-scoped, TDZ, not re-declarable
    const: block-scoped, TDZ, not re-declarable or reassignable

2.  Explain hoisting
    Declarations (var, function) moved to top of scope in creation phase
    var → undefined; let/const → TDZ; function declarations → complete

3.  What is the Temporal Dead Zone?
    Period from start of block scope until let/const declaration
    Variable exists in memory but accessing it throws ReferenceError

4.  What is a closure?
    Inner function retaining access to outer function's scope after it returns
    Uses: data privacy, memoization, function factories, module pattern

5.  Explain the event loop
    Single-threaded JS uses event loop for async: call stack empties →
    drain ALL microtasks (Promises) → one macrotask (setTimeout) → repeat

6.  Microtask vs Macrotask queue?
    Microtask: Promises (.then, .catch), queueMicrotask — HIGHER priority
    Macrotask: setTimeout, setInterval, I/O — LOWER priority

7.  What is the prototype chain?
    Objects have [[Prototype]] linking to another object
    Property lookup traverses this chain until found or null reached

8.  == vs ===
    == performs type coercion; === compares value AND type. Always use ===
    Exception: value == null checks for both null and undefined

9.  What is null vs undefined?
    null: intentional empty value (assigned by programmer)
    undefined: variable declared but not yet assigned a value

10. typeof null === "object" — why?
    Historic bug from JavaScript's first version (1995). Can't be fixed.

11. What is 'this' in JavaScript?
    Regular function: determined by how called (caller's context)
    Arrow function: lexically inherited from enclosing scope
    new: the newly created object

12. Explain call, apply, bind
    fn.call(ctx, a, b)   — call with context + individual args
    fn.apply(ctx, [a,b]) — call with context + array of args
    fn.bind(ctx, a, b)   — returns new function bound to context

13. What is event delegation?
    Attach one listener to parent, use event.target to identify child
    Benefits: fewer listeners, handles dynamic elements

14. Explain Promises
    Object representing eventual success/failure of async operation
    States: pending → fulfilled or rejected
    Methods: .then(), .catch(), .finally(), Promise.all/race/allSettled

15. async/await vs Promises
    async/await is syntactic sugar over Promises
    Makes code more readable, easier error handling with try/catch

16. What are higher-order functions?
    Functions that take functions as args or return functions
    Examples: map, filter, reduce, setTimeout

17. Explain the difference between deep and shallow copy
    Shallow: only top-level properties copied (nested refs shared)
    Deep: all levels copied (structuredClone, JSON.parse/stringify)

18. What is a Symbol?
    Unique primitive type — no two Symbols are equal
    Used for: unique object keys, well-known symbols (Symbol.iterator)

19. What is destructuring?
    Syntax to extract values from arrays/objects into variables
    Works in assignments, function parameters, loops

20. Explain optional chaining (?.)
    Access nested properties without TypeError if intermediate is null/undefined
    Returns undefined instead of throwing

21. Nullish coalescing (??) vs OR (||)?
    ||  returns right operand if left is FALSY (0, "", false, null, undefined)
    ??  returns right operand ONLY if left is null or undefined

22. What causes memory leaks in JS?
    Forgotten event listeners, uncleaned timers, detached DOM nodes,
    closures holding unnecessary large objects, accidental globals

23. Explain Stack vs Heap memory
    Stack: fixed size, fast, stores primitives + references
    Heap: large, dynamic, stores objects/arrays/functions

24. What is IIFE and why use it?
    Immediately Invoked Function Expression — runs and creates private scope
    Avoids global pollution; was module pattern before ES6 modules

25. Explain generator functions
    Pause/resume with yield; return iterator object
    Use for lazy sequences, infinite series, async control flow
```

---

## Essential Code Patterns

```javascript
// Debounce — delay execution until user stops
const debounce = (fn, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
};

// Throttle — execute at most once per interval
const throttle = (fn, interval) => {
  let last = 0;
  return (...args) => {
    const now = Date.now();
    if (now - last >= interval) {
      last = now;
      fn(...args);
    }
  };
};

// Deep clone
const deepClone = (obj) => structuredClone(obj); // ES2022

// Flatten deeply nested array
const deepFlat = (arr) => arr.flat(Infinity);

// Group array by key
const groupBy = (arr, key) =>
  arr.reduce((acc, item) => {
    (acc[item[key]] ??= []).push(item);
    return acc;
  }, {});

// Unique values
const unique = (arr) => [...new Set(arr)];

// Pipe functions (left to right composition)
const pipe =
  (...fns) =>
  (x) =>
    fns.reduce((v, fn) => fn(v), x);

// Chunk array
const chunk = (arr, size) =>
  arr.reduce(
    (acc, _, i) => (i % size ? acc : [...acc, arr.slice(i, i + size)]),
    [],
  );

// Safe deep get with default
const get = (obj, path, def) =>
  path.split(".").reduce((o, k) => o?.[k], obj) ?? def;

// Retry async
const retry = async (fn, n = 3) => {
  try {
    return await fn();
  } catch (e) {
    if (n <= 1) throw e;
    return retry(fn, n - 1);
  }
};
```

---

_JavaScript Developer Handbook — Core to Advanced_  
_Structured for learning, reference, and interview preparation_  
_GitHub-ready · Last updated 2025_
