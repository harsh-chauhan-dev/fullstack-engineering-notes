# JavaScript Advanced Developer Handbook
## Modules 14 — 20

> **A comprehensive reference for senior JavaScript engineers.**
> Covers the `this` keyword, functional programming, advanced async, engine internals, memory optimization, design patterns, and browser APIs.

**Difficulty:** 🟢 Beginner · 🟡 Intermediate · 🔴 Advanced

---

## 📚 Table of Contents

| Module | Topic | Sections |
|--------|-------|----------|
| [14 — `this` Keyword](#module-14--the-this-keyword-deep-dive) | What is `this` · 4 Binding Rules · Default · Implicit · Explicit · `new` · Arrow Functions · Classes · Precedence · Edge Cases · Reference Table · Real-World · Interview Q&A · Common Mistakes | 14 |
| [15 — Functional Programming](#module-15--functional-programming-in-javascript) | What is FP · Pure Functions · Immutability · Higher-Order Functions · map/filter/reduce · Composition · Currying · Closures as State · Monads · Transducers · FP Patterns · Interview Q&A | 12 |
| [16 — Advanced Async](#module-16--advanced-asynchronous-javascript) | Async Model · Promise Internals · Combinators · for-await-of · Async Generators · Concurrency · AbortController · Error Handling · Streams · Scheduling · Production Patterns · Interview Q&A | 12 |
| [17 — JS Engine Internals](#module-17--javascript-engine-internals) | Engine Architecture · AST · Ignition · TurboFan · Hidden Classes · Garbage Collection · Optimization Killers · Shapes · Typed Arrays · DevTools Profiling · Interview Q&A | 11 |
| [18 — Memory Optimization](#module-18--memory-optimization) | Memory Model · GC Decisions · 6 Memory Leaks · WeakRef · WeakMap/WeakSet · Object Pooling · Data Structures · Large Data · Profiling · Interview Q&A | 10 |
| [19 — Design Patterns](#module-19--design-patterns-in-javascript) | Why Patterns · Creational (Singleton · Factory · Builder · Prototype) · Structural (Decorator · Proxy · Facade) · Behavioral (Observer · Strategy · Command · Chain · State) · Concurrency · Reference · Interview Q&A | 7 |
| [20 — Browser APIs](#module-20--browser-apis) | API Landscape · Storage · Fetch/Network · Web Workers · Service Workers/PWA · Observers · Web Animations · WebSockets/SSE · File/Clipboard/Media · Performance · Permissions/Sensors · Interview Q&A | 12 |

---

# Module 14 — The `this` Keyword Deep Dive

> **"The most misunderstood concept in JavaScript"**
> `this` is not about **where** a function is written — it's about **how** it is called.

---


## 14.1 What is `this`?

**Definition:** `this` is a special keyword that refers to the **execution context** of a function — the object that is currently "owning" or "calling" it at runtime.

Four things `this` is **not**:

- ❌ Not the function itself
- ❌ Not the function's lexical scope
- ❌ Not fixed at write time (except in arrow functions)
- ❌ Not always the global object

```javascript
// `this` is dynamic — same function, entirely different results
function showThis() {
  console.log(this);
}

showThis();            // window | undefined (strict)  ← default binding
obj.showThis();        // obj                          ← implicit binding
showThis.call(other);  // other                        ← explicit binding
new showThis();        // brand new object             ← new binding
```

> **The golden rule:** `this` is determined at **call time**, not at definition time (with one exception: arrow functions, which capture `this` at definition time from the enclosing scope).

---

## 14.2 The 4 Binding Rules — Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                     THIS BINDING RULES                            │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Rule 1: DEFAULT BINDING                                │    │
│  │  Pattern:  fn()                                         │    │
│  │  Result:   this = global object (window / globalThis)   │    │
│  │            this = undefined in strict mode              │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Rule 2: IMPLICIT BINDING                               │    │
│  │  Pattern:  obj.fn()                                     │    │
│  │  Result:   this = obj  (object left of the dot)         │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Rule 3: EXPLICIT BINDING                               │    │
│  │  Pattern:  fn.call(x) / fn.apply(x) / fn.bind(x)()     │    │
│  │  Result:   this = x  (whatever you specify)             │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  Rule 4: NEW BINDING                                    │    │
│  │  Pattern:  new fn()                                     │    │
│  │  Result:   this = freshly created object                │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  SPECIAL CASE: ARROW FUNCTION                           │    │
│  │  Pattern:  () => { ... }                                │    │
│  │  Result:   this = lexical scope at definition site      │    │
│  │            Cannot be overridden by any rule above       │    │
│  └─────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────┘
```

---

## 14.3 Rule 1 — Default Binding

When a function is called **standalone** (no object, no `new`, no explicit binding), `this` falls back to the global object — or `undefined` in strict mode.

```javascript
// ── Non-strict mode ──────────────────────────────────────────────
var appName = "MyApp";  // var attaches to window

function showName() {
  console.log(this.appName);  // this = window
}
showName();  // "MyApp"

// ── Strict mode ──────────────────────────────────────────────────
"use strict";

function strictFn() {
  console.log(this);  // undefined ← no global fallback!
}
strictFn();

// ── ES Modules (always strict) ───────────────────────────────────
// In any .mjs file or <script type="module">:
console.log(this);  // undefined at top level
```

```
DEFAULT BINDING DIAGRAM:
────────────────────────────────────────────────────────────────

  showName()             ← standalone call — no owner object
      │
      ▼
  ┌───────────────────────────────────────┐
  │  SLOPPY MODE → this = window/global   │
  │  STRICT MODE → this = undefined       │
  └───────────────────────────────────────┘
```

> **Interview Insight:** Classes and ES modules are **always in strict mode**, even without writing `"use strict"`. This is why standalone calls inside a class method crash with `TypeError: Cannot read properties of undefined` instead of silently reading from `window`.

---

## 14.4 Rule 2 — Implicit Binding

When a function is called **as a method of an object** (`obj.fn()`), `this` is set to the object immediately to the left of the dot.

```javascript
const user = {
  name: "Alice",
  age: 25,
  greet() {
    return `Hello, I'm ${this.name}`;  // this = user
  },
};

user.greet();  // "Hello, I'm Alice"  ✅
```

**The "last object before the dot" rule:**

```javascript
const company = {
  name: "Acme Corp",
  ceo: {
    name: "Bob",
    introduce() {
      return `I'm ${this.name}`;  // this = ceo, not company
    },
  },
};

company.ceo.introduce();  // "I'm Bob"  ← ceo is last before the dot
```

---

### ⚠️ Implicit Binding Loss — The #1 `this` Bug

The binding is **silently dropped** when a method is detached from its object. This is the most common `this` mistake in JavaScript.

```javascript
const user = {
  name: "Alice",
  greet() { return `Hello, I'm ${this.name}`; },
};

// ❌ Three ways binding is lost:

const fn = user.greet;         // 1. assigned to variable
fn();                          // "Hello, I'm undefined"

setTimeout(user.greet, 1000);  // 2. passed as callback — this = window
[1].forEach(user.greet);       // 3. passed to HOF     — this = window
```

```
BINDING LOSS DIAGRAM:
────────────────────────────────────────────────────────────────

  user.greet()
  │
  │  "user" is to the left of the dot
  └─► this = user  ✅

  const fn = user.greet   ← reference copied, context stripped
  fn()
  │
  │  nothing to the left of ()
  └─► this = window / undefined  ❌
```

**The three reliable fixes:**

```javascript
// Fix 1: bind() — permanently lock the context
const boundGreet = user.greet.bind(user);
setTimeout(boundGreet, 1000);          // this = user  ✅

// Fix 2: arrow wrapper — calls it as a method
setTimeout(() => user.greet(), 1000);  // this = user  ✅

// Fix 3: arrow class field — bound at construction (modern, preferred)
class Component {
  handleClick = () => {  // arrow captures `this` at construction time
    console.log(this);   // always the instance  ✅
  };
}
```

---

## 14.5 Rule 3 — Explicit Binding

You can **force** `this` to be any value using three built-in methods available on every function.

### `call()` — invoke immediately, pass args individually

```javascript
function introduce(greeting, punctuation) {
  return `${greeting}, I'm ${this.name}${punctuation}`;
}

const alice = { name: "Alice" };
const bob   = { name: "Bob"   };

introduce.call(alice, "Hello", "!");  // "Hello, I'm Alice!"
introduce.call(bob,   "Hey",   ".");  // "Hey, I'm Bob."

// Signature: fn.call(thisArg, arg1, arg2, ...)
```

### `apply()` — invoke immediately, pass args as an array

```javascript
introduce.apply(alice, ["Hello", "!"]);  // "Hello, I'm Alice!"
introduce.apply(bob,   ["Hey",   "."]);  // "Hey, I'm Bob."

// Signature: fn.apply(thisArg, [arg1, arg2, ...])

// Classic use: spread array into a function expecting individual args
const nums = [3, 1, 4, 1, 5, 9];
Math.max.apply(null, nums);  // 9  (modern: Math.max(...nums))
```

### `bind()` — return a new permanently bound function

```javascript
// Signature: fn.bind(thisArg, arg1?, arg2?, ...)
// Does NOT call the function — returns a new one

const greetAlice = introduce.bind(alice, "Hello");
greetAlice("!");  // "Hello, I'm Alice!"
greetAlice("?");  // "Hello, I'm Alice?"

// bind() is permanent — even call/apply cannot override it
const greetBob = introduce.bind(bob);
greetBob.call(alice, "Hi", "!");  // "Hi, I'm Bob!"  ← bob wins
```

**Side-by-side comparison:**

| Method    | Invokes Immediately | Arg Format          | Returns         | Rebindable? |
| --------- | :-----------------: | ------------------- | --------------- | :---------: |
| `call()`  |       ✅ Yes        | individual args     | function result |     N/A     |
| `apply()` |       ✅ Yes        | array               | function result |     N/A     |
| `bind()`  |       ❌ No         | individual (preset) | new function    |   ❌ Never  |

```javascript
// Real-world: borrow Array methods for array-like objects
const nodeList = document.querySelectorAll("li");
Array.prototype.forEach.call(nodeList, (el) => console.log(el.textContent));

// Real-world: partial application with bind
function multiply(a, b) { return a * b; }

const double = multiply.bind(null, 2);  // null = don't care about this
const triple = multiply.bind(null, 3);

double(5);  // 10
triple(5);  // 15
```

---

## 14.6 Rule 4 — `new` Binding

When a function is called with `new`, JavaScript creates a **brand new empty object**, sets `this` to it, runs the function body, and returns the object.

```javascript
function Person(name, age) {
  this.name = name;  // `this` = the new empty object
  this.age  = age;
  // implicit: return this
}

const alice = new Person("Alice", 25);
alice.name;  // "Alice"
alice.age;   // 25
```

**What `new` does — all four steps:**

```
new Person("Alice", 25)
────────────────────────────────────────────────────────────────

  Step 1 │ Create empty object
         │   obj = {}
         │
  Step 2 │ Set its prototype
         │   Object.setPrototypeOf(obj, Person.prototype)
         │
  Step 3 │ Run constructor with this = obj
         │   Person.call(obj, "Alice", 25)
         │   → obj.name = "Alice"
         │   → obj.age  = 25
         │
  Step 4 │ Return obj
         │   (unless constructor returns a different plain object)
         ▼
  Result: { name: "Alice", age: 25 }
```

```javascript
// Edge case: constructor returns a plain object — overrides new
function WeirdFn() {
  this.x = 1;
  return { y: 2 };  // explicit object return replaces `this`
}
new WeirdFn();  // { y: 2 }  ← NOT { x: 1 }

// Returning a primitive is ignored — new still returns `this`
function NormalFn() {
  this.x = 1;
  return 42;  // primitive ignored
}
new NormalFn();  // { x: 1 }  ✅
```

> **Interview Insight:** `new` has the **highest precedence** of all four binding rules. Even a `bind()`-ed function gets a fresh `this` when called with `new` — the bound context is overridden.

---

## 14.7 Arrow Functions — Lexical `this`

Arrow functions have **no `this` of their own**. They capture `this` from the **enclosing lexical scope at the time they are written**. This value is locked in permanently — no rule can change it.

```javascript
// ── The problem arrow functions were designed to solve ───────────
const timer = {
  name: "My Timer",

  // ❌ Regular callback — `this` is lost inside setTimeout
  startBroken() {
    setTimeout(function () {
      console.log(this.name);  // undefined — this = window
    }, 100);
  },

  // ✅ Arrow callback — `this` inherited from startFixed's scope
  startFixed() {
    setTimeout(() => {
      console.log(this.name);  // "My Timer" ✅
    }, 100);
  },
};
```

```
LEXICAL THIS DIAGRAM:
────────────────────────────────────────────────────────────────

  const obj = {
    name: "Alice",

    outer() {                     ← called as obj.outer() → this = obj
      const inner = () => {       ← arrow: no own `this`
        console.log(this.name);   ← looks up to outer's scope
      };
      inner();                    ← call-site doesn't matter for arrows
    }
  };

  obj.outer();  // "Alice" ✅

  Arrow's this lookup chain:
  inner() → ❌ no own this
          → ✅ inherit from outer() → this = obj
          → console.log("Alice")
```

**Arrow functions CANNOT be rebound — ever:**

```javascript
const fn = () => console.log(this);
const obj = { name: "Alice" };

fn.call(obj);    // outer `this` — call() has zero effect on arrows
fn.apply(obj);   // same
fn.bind(obj)();  // same — bind returns the original fn unchanged

new fn();        // TypeError: fn is not a constructor
```

**When NOT to use arrow functions:**

```javascript
// ❌ Arrow as object method — this = outer scope, not the object
const user = {
  name: "Alice",
  greet: () => `Hello, I'm ${this.name}`,  // this = window!
};
user.greet();  // "Hello, I'm undefined"

// ✅ Method shorthand — this = user
const user = {
  name: "Alice",
  greet() { return `Hello, I'm ${this.name}`; },
};
user.greet();  // "Hello, I'm Alice"

// ❌ Event handler needing `this` = DOM element
button.addEventListener("click", () => {
  this.classList.add("active");  // this = window, not button!
});

// ✅ Regular function for DOM handlers
button.addEventListener("click", function () {
  this.classList.add("active");  // this = button ✅
});
```

| Use Case                        | Best Choice         | Why                                 |
| ------------------------------- | ------------------- | ----------------------------------- |
| Object method                   | Regular / shorthand | Needs `this` = the object           |
| Callback inside a method        | Arrow function      | Needs `this` from enclosing method  |
| Event handler (needs element)   | Regular function    | DOM sets `this` = the element       |
| Event handler (needs instance)  | Arrow class field   | `this` locked to class instance     |
| Constructor                     | Regular / class     | Arrows cannot be constructed        |
| HOF callback (map/filter)       | Arrow function      | Shorter, inherits outer `this`      |

---

## 14.8 `this` in Classes

Classes run in strict mode automatically. `this` in a class always refers to the instance — but binding loss still applies when methods are detached.

```javascript
class Counter {
  #count = 0;

  constructor(name) {
    this.name = name;  // this = new instance
  }

  increment() {
    this.#count++;
    return this;  // ← return this enables method chaining
  }

  decrement() {
    this.#count--;
    return this;
  }

  value() { return this.#count; }

  // ⚠️ Binding lost if this method is detached from the instance
  logValue() {
    console.log(`${this.name}: ${this.#count}`);
  }
}

// Method chaining — works because increment/decrement return `this`
const c = new Counter("score");
c.increment().increment().increment().decrement();
c.value();  // 2

// Binding loss in strict mode → undefined, not window
const { logValue } = c;
logValue();  // TypeError: Cannot read properties of undefined

// Fix: arrow class field — bound at construction, impossible to lose
class SafeCounter {
  #count = 0;
  logValue = () => {
    console.log(this.#count);  // always the instance ✅
  };
}
```

**Inheritance and `this`:**

```javascript
class Animal {
  constructor(name) { this.name = name; }
  speak() { return `${this.name} makes a sound`; }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);        // MUST call super() before touching `this`
    this.breed = breed; // this = new Dog instance
  }
  bark() { return `${this.name} barks!`; }
}

const rex = new Dog("Rex", "Lab");
rex.speak();  // "Rex makes a sound" — this = rex inside Animal.speak
rex.bark();   // "Rex barks!"        — this = rex inside Dog.bark
```

> **Interview Insight:** Forgetting `super()` before using `this` in a derived class constructor throws a `ReferenceError`. JavaScript requires the parent to initialize the object before the child can access it.

---

## 14.9 `this` Precedence Rules

When multiple binding rules could apply, a strict priority order decides the winner.

```
PRECEDENCE ORDER (1 = highest):
────────────────────────────────────────────────────────────────

  Priority 1 │ new binding
             │   new fn()  →  this = new object
             │
  Priority 2 │ Explicit binding
             │   fn.call(x) / fn.apply(x) / fn.bind(x)()
             │   →  this = x
             │
  Priority 3 │ Implicit binding
             │   obj.fn()  →  this = obj
             │
  Priority 4 │ Default binding (lowest)
             │   fn()  →  this = window (sloppy) | undefined (strict)
             │
  Exception  │ Arrow function
             │   Ignores ALL rules — always uses lexical this
```

```javascript
// Demonstrating precedence:
function show() { console.log(this?.label ?? String(this)); }

const a = { label: "A", show };
const b = { label: "B" };

// Priority 4 — Default
show();               // window or undefined

// Priority 3 — Implicit
a.show();             // "A"

// Priority 2 — Explicit overrides Implicit
a.show.call(b);       // "B"  ← b wins over a

// Priority 1 — new overrides bind
const bound = show.bind(a);
new bound();          // {}   ← new object wins over bound a
```

```
DECISION FLOWCHART — "What is `this`?"
────────────────────────────────────────────────────────────────

  Is the function an arrow function?
  │
  ├─ YES → this = enclosing lexical scope at write time. STOP.
  │
  └─ NO ──► Was it called with `new`?
            │
            ├─ YES → this = newly created object. STOP.
            │
            └─ NO ──► Was it called with call / apply / bind?
                      │
                      ├─ YES → this = explicitly provided value. STOP.
                      │
                      └─ NO ──► Was it called as obj.fn()?
                                │
                                ├─ YES → this = obj. STOP.
                                │
                                └─ NO ──► Strict mode?
                                          │
                                          ├─ YES → this = undefined
                                          └─ NO  → this = global object
```

---

## 14.10 Edge Cases

### `this` in Array Method Callbacks

```javascript
const obj = {
  name: "Alice",
  items: [1, 2, 3],

  // ❌ Regular callback — this is lost inside forEach
  processBroken() {
    this.items.forEach(function (item) {
      console.log(this.name, item);  // undefined — this is not obj
    });
  },

  // ✅ Fix 1: arrow callback inherits this from processGood
  processGood() {
    this.items.forEach((item) => {
      console.log(this.name, item);  // "Alice" ✅
    });
  },

  // ✅ Fix 2: thisArg parameter (2nd argument of forEach/map/filter)
  processAlt() {
    this.items.forEach(function (item) {
      console.log(this.name, item);  // "Alice" ✅
    }, this);  // ← pass this explicitly as the callback context
  },
};
```

### `this` in Timers

```javascript
const controller = {
  count: 0,

  // ❌ Binding lost — callback is a standalone call
  startBroken() {
    setInterval(function () {
      this.count++;  // this = window → NaN
    }, 1000);
  },

  // ✅ Arrow preserves binding
  start() {
    this.timer = setInterval(() => {
      this.count++;             // this = controller ✅
      console.log(this.count);
    }, 1000);
  },

  stop() { clearInterval(this.timer); },
};
```

### `this` in Event Handlers

```javascript
class Toggle {
  constructor(el) {
    this.el   = el;
    this.isOn = false;

    // ❌ this inside handler = the DOM element, not the class instance
    el.addEventListener("click", this.handleClick);

    // ✅ Option A: bind
    el.addEventListener("click", this.handleClick.bind(this));

    // ✅ Option B: arrow wrapper
    el.addEventListener("click", (e) => this.handleClick(e));
  }

  handleClick() {
    // With plain assignment above — this = <button> element
    // With bind or arrow wrapper — this = Toggle instance  ✅
    this.isOn = !this.isOn;
    this.el.classList.toggle("active", this.isOn);
  }

  // ✅ Option C: arrow class field — cleanest modern approach
  handleClickField = () => {
    this.isOn = !this.isOn;
    this.el.classList.toggle("active", this.isOn);
  };
}
```

### `this` Lost via Destructuring

```javascript
const obj = {
  value: 42,
  getValue() { return this.value; },
};

// ❌ Destructuring strips the object context
const { getValue } = obj;
getValue();  // undefined (or TypeError in strict)

// ✅ Fix: bind before destructuring
const { getValue } = { getValue: obj.getValue.bind(obj) };
getValue();  // 42 ✅
```

### Returning `this` for Method Chaining (Fluent Interface)

```javascript
class QueryBuilder {
  #table      = "";
  #conditions = [];
  #limit      = null;

  from(table)  { this.#table = table;         return this; }
  where(cond)  { this.#conditions.push(cond); return this; }
  limitTo(n)   { this.#limit = n;             return this; }

  build() {
    let q = `SELECT * FROM ${this.#table}`;
    if (this.#conditions.length)
      q += ` WHERE ${this.#conditions.join(" AND ")}`;
    if (this.#limit)
      q += ` LIMIT ${this.#limit}`;
    return q;
  }
}

// Each method returns `this` → fluent chaining
const sql = new QueryBuilder()
  .from("users")
  .where("active = true")
  .where("age > 18")
  .limitTo(10)
  .build();
// "SELECT * FROM users WHERE active = true AND age > 18 LIMIT 10"
```

---

## 14.11 `this` Binding — Full Reference Table

| Call Pattern                          | `this` Value                      | Notes                              |
| ------------------------------------- | --------------------------------- | ---------------------------------- |
| `fn()`                                | `window` / `globalThis`           | Sloppy mode only                   |
| `fn()` in strict mode                 | `undefined`                       | Classes + modules always strict    |
| `obj.fn()`                            | `obj`                             | Implicit binding                   |
| `obj.inner.fn()`                      | `obj.inner`                       | Last object before the dot         |
| `fn.call(ctx, a, b)`                  | `ctx`                             | Explicit, immediate                |
| `fn.apply(ctx, [a, b])`               | `ctx`                             | Explicit, immediate                |
| `fn.bind(ctx)()`                      | `ctx`                             | Explicit, permanent, not immediate |
| `new fn()`                            | New empty object                  | Highest priority rule              |
| `const f = () => {}; f()`             | Enclosing scope's `this`          | Cannot be overridden               |
| `class` method call                   | Instance (strict)                 | `undefined` if detached            |
| `class` arrow field                   | Always the instance               | Bound at construction              |
| `setTimeout(fn, ms)`                  | `window` / `undefined`            | fn runs as standalone call         |
| `el.addEventListener("click", fn)`    | `el` (the DOM element)            | Implicit binding by the DOM        |
| `el.addEventListener("click", () =>)` | Outer `this`, **not** `el`        | Arrow ignores DOM binding          |

---

## 14.12 Real-World Examples

### Example 1 — React Class Component (Historic Pattern)

```javascript
// ❌ Before fix: handlers lose `this` when called by React's event system
class SearchBar extends React.Component {
  constructor(props) {
    super(props);
    this.state = { query: "" };

    // Binding in the constructor was the standard fix pre-2019
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(e) {
    this.setState({ query: e.target.value });  // this = component ✅
  }

  handleSubmit(e) {
    e.preventDefault();
    this.props.onSearch(this.state.query);     // this = component ✅
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input onChange={this.handleChange} value={this.state.query} />
      </form>
    );
  }
}

// ✅ Modern: arrow class fields — no bind() needed anywhere
class SearchBarModern extends React.Component {
  state = { query: "" };

  handleChange = (e) => this.setState({ query: e.target.value });
  handleSubmit = (e) => {
    e.preventDefault();
    this.props.onSearch(this.state.query);
  };
}
```

### Example 2 — Explicit Binding for Method Borrowing

```javascript
// Different data structures, one shared formatting method
const csvFormatter = {
  delimiter: ",",
  format(items) {
    return items.join(this.delimiter);
  },
};

const tsvFormatter  = { delimiter: "\t" };
const pipeFormatter = { delimiter: " | " };

// Borrow csvFormatter.format, inject a different `this` each time
csvFormatter.format.call(tsvFormatter,  ["a", "b", "c"]);  // "a\tb\tc"
csvFormatter.format.call(pipeFormatter, ["a", "b", "c"]);  // "a | b | c"

// Permanent borrow with bind
const toTSV  = csvFormatter.format.bind(tsvFormatter);
const toPipe = csvFormatter.format.bind(pipeFormatter);

toTSV(["x", "y", "z"]);   // "x\ty\tz"
toPipe(["x", "y", "z"]);  // "x | y | z"
```

### Example 3 — Event System with Correct `this` Binding

```javascript
class EventEmitter {
  #events = new Map();

  on(event, listener) {
    if (!this.#events.has(event)) this.#events.set(event, []);
    this.#events.get(event).push(listener);
    return this;  // chainable
  }

  emit(event, ...args) {
    (this.#events.get(event) ?? []).forEach((fn) => fn(...args));
    return this;
  }
}

class Store extends EventEmitter {
  #state;

  constructor(initial) {
    super();
    this.#state = initial;
  }

  // Arrow class field — `this` always refers to the Store instance
  // Safe to destructure and pass as a callback anywhere
  dispatch = (action) => {
    this.#state = this.#reduce(this.#state, action);
    this.emit("change", this.#state);  // this = Store ✅
  };

  #reduce(state, action) {
    switch (action.type) {
      case "INCREMENT": return { ...state, count: state.count + 1 };
      case "RESET":     return { ...state, count: 0 };
      default:          return state;
    }
  }

  getState() { return { ...this.#state }; }
}

const store = new Store({ count: 0 });
store.on("change", (state) => console.log("State:", state));

// dispatch can be passed anywhere — binding is locked by arrow field
const { dispatch } = store;
dispatch({ type: "INCREMENT" });  // State: { count: 1 }
dispatch({ type: "INCREMENT" });  // State: { count: 2 }
dispatch({ type: "RESET" });      // State: { count: 0 }
```

### Example 4 — Mixin Pattern Using `call()`

```javascript
// Mixins inject behavior by explicitly setting `this` at call time
const Serializable = {
  serialize()       { return JSON.stringify(this); },
  deserialize(json) { return Object.assign(this, JSON.parse(json)); },
};

const Validatable = {
  validate() {
    return Object.keys(this.rules || {}).every(
      (field) => this.rules[field](this[field])
    );
  },
};

class User {
  constructor(name, email) {
    this.name  = name;
    this.email = email;
    this.rules = {
      name:  (v) => typeof v === "string" && v.length > 0,
      email: (v) => /\S+@\S+\.\S+/.test(v),
    };
  }
}

Object.assign(User.prototype, Serializable, Validatable);

const user = new User("Alice", "alice@example.com");
user.serialize();  // '{"name":"Alice","email":"alice@example.com",...}'
user.validate();   // true
```

---

## 14.13 Interview Questions

### Q1 — Predict the output

```javascript
const obj = {
  name: "Alice",
  outer() {
    const inner = function () {
      return this.name;
    };
    return inner();
  },
};

console.log(obj.outer()); // ?
```

<details>
<summary>Answer + Explanation</summary>

```
"" or undefined  (sloppy mode — this = window, window.name = "" by default)
TypeError        (strict mode — this = undefined, can't read .name)

Why: inner() is called as a standalone function — no object to its left.
Default binding applies: this = window (sloppy) or undefined (strict).

Fix: change inner to an arrow function to inherit outer()'s this = obj.
const inner = () => this.name;  // "Alice" ✅
```

</details>

---

### Q2 — Fix the broken timer

```javascript
const stopwatch = {
  elapsed: 0,
  start() {
    setInterval(function () {
      this.elapsed++;
      console.log(this.elapsed);
    }, 1000);
  },
};

stopwatch.start();  // NaN, NaN, NaN...
```

<details>
<summary>Answer + Explanation</summary>

```javascript
// Root cause: setInterval calls its callback as a standalone function.
// this = window. window.elapsed = undefined. undefined++ = NaN.

// Fix 1 — arrow function (recommended)
start() {
  setInterval(() => {
    this.elapsed++;         // this = stopwatch ✅
    console.log(this.elapsed);
  }, 1000);
}

// Fix 2 — .bind(this)
start() {
  setInterval(function () {
    this.elapsed++;
    console.log(this.elapsed);
  }.bind(this), 1000);
}
```

</details>

---

### Q3 — Trace the binding

```javascript
function show() { console.log(this.label); }

const x = { label: "X", show };
const y = { label: "Y" };

x.show();              // ?
x.show.call(y);        // ?
const fn = x.show.bind(y);
fn.call(x);            // ?
new fn();              // ?
```

<details>
<summary>Answer</summary>

```
"X"       — implicit binding: x is to the left of the dot
"Y"       — explicit: call(y) overrides implicit
"Y"       — bind is permanent; call(x) cannot override it
undefined — new creates a fresh object with no `label` property
```

</details>

---

### Q4 — `new` vs returning an object

```javascript
function Tag(value) {
  this.value = value;
  return { value: "override" };  // returns a plain object
}

const t = new Tag("original");
console.log(t.value);  // ?
```

<details>
<summary>Answer</summary>

```
"override"

When a constructor explicitly returns a plain object,
`new` returns THAT object — not `this`.

Returning a primitive (42, "str", true) is ignored,
and `new` returns `this` as normal.
```

</details>

---

### Q5 — Arrow class field vs prototype method

```javascript
class Greeter {
  name = "Alice";
  regularGreet() { return this?.name; }
  arrowGreet = () => this?.name;
}

const g = new Greeter();
const { regularGreet, arrowGreet } = g;

regularGreet();  // ?
arrowGreet();    // ?
```

<details>
<summary>Answer</summary>

```
undefined — regularGreet is detached; this = undefined in strict class scope
"Alice"   — arrowGreet is an arrow class field; this is permanently bound
            to the instance at construction time; destructuring can't break it
```

</details>

---

### Q6 — Explain the four rules and their precedence

> **Model answer for interviews:**
>
> There are four binding rules in descending priority order. **`new` binding** (highest) creates a fresh object and assigns it to `this`. **Explicit binding** via `call()`, `apply()`, or `bind()` lets you specify `this` directly — `bind()` creates a permanent version. **Implicit binding** sets `this` to the object left of the dot in a method call. **Default binding** (lowest) falls back to the global object in sloppy mode or `undefined` in strict mode.
>
> Arrow functions are a special case that bypass all four rules. They capture `this` lexically from the enclosing scope at the time they are written, and this value can never be changed by any of the four rules.

---

## 14.14 Common Mistakes Cheat Sheet

```javascript
// ❌ MISTAKE 1: Arrow function as an object method
const counter = {
  count: 0,
  increment: () => this.count++,  // this = outer scope, NOT counter
};

// ✅ FIX: use method shorthand
const counter = {
  count: 0,
  increment() { this.count++; },  // this = counter ✅
};

// ─────────────────────────────────────────────────────────────────

// ❌ MISTAKE 2: Forgetting that callbacks lose implicit binding
class Logger {
  prefix = "[LOG]";
  logAll(items) {
    items.forEach(function (item) {
      console.log(this.prefix, item);  // this = undefined!
    });
  }
}

// ✅ FIX: arrow callback
logAll(items) {
  items.forEach((item) => console.log(this.prefix, item));  // ✅
}

// ─────────────────────────────────────────────────────────────────

// ❌ MISTAKE 3: Passing class methods as callbacks without binding
class Form {
  handleSubmit() { console.log(this.data); }
  mount(el) {
    el.addEventListener("submit", this.handleSubmit);  // this = el, not Form!
  }
}

// ✅ FIX: arrow class field
class Form {
  handleSubmit = () => { console.log(this.data); };  // always bound ✅
}

// ─────────────────────────────────────────────────────────────────

// ❌ MISTAKE 4: Arrow function as prototype method
Animal.prototype.speak = () => this.name;  // this = window forever

// ✅ FIX: regular function
Animal.prototype.speak = function () { return this.name; };  // ✅

// ─────────────────────────────────────────────────────────────────

// ❌ MISTAKE 5: Assuming nested regular function inherits method's this
const obj = {
  value: 10,
  compute() {
    function helper() { return this.value * 2; }  // this = undefined!
    return helper();
  },
};

// ✅ FIX: arrow function for helper
const obj = {
  value: 10,
  compute() {
    const helper = () => this.value * 2;  // inherits compute's this ✅
    return helper();
  },
};
```

**Quick decision guide:**

```
Writing an object method?            → method shorthand    fn() { }
Writing a callback inside a method?  → arrow function      () => { }
Writing a constructor?               → class or function
Event handler needing the element?   → regular function
Event handler needing the instance?  → arrow class field
Need to borrow a method once?        → .call() / .apply()
Need to lock a context permanently?  → .bind()
```

---

> **Module 14 Complete.**
> Master the four rules, understand arrow function lexical scoping, and you will diagnose any `this` bug on sight — in interviews and in production.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 14 of 14 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 15 — Functional Programming in JavaScript

> **"Write programs by composing pure functions, avoiding shared state and mutable data."**
> Functional Programming (FP) is a paradigm, not a library. JavaScript supports it natively.

---


## 15.1 What is Functional Programming?

**Definition:** A programming paradigm that treats computation as the evaluation of mathematical functions, avoiding **side effects** and **mutable state**.

```
FP CORE PILLARS:
────────────────────────────────────────────────────────────────
  ┌─────────────────┐   ┌──────────────────┐   ┌────────────────┐
  │  Pure Functions  │   │  Immutability    │   │  Composition   │
  │  same in → same  │   │  never mutate,   │   │  small fns →   │
  │  out, no effects │   │  return new data │   │  bigger fns    │
  └─────────────────┘   └──────────────────┘   └────────────────┘
        ┌──────────────────────┐   ┌──────────────────────┐
        │  Higher-Order Fns    │   │  Declarative Style   │
        │  fns as values,      │   │  describe WHAT,      │
        │  fns returning fns   │   │  not HOW             │
        └──────────────────────┘   └──────────────────────┘
```

**OOP vs FP mindset:**

| Concern        | OOP                      | FP                         |
| -------------- | ------------------------ | -------------------------- |
| Unit of code   | Object / Class           | Function                   |
| State          | Mutated inside objects   | Transformed, never mutated |
| Code reuse     | Inheritance              | Composition                |
| Side effects   | Expected, managed        | Avoided, isolated          |
| Data flow      | Hidden inside objects    | Explicit through functions |

---

## 15.2 Pure Functions

**Definition:** A function is **pure** if it always returns the same output for the same input, and produces no side effects.

```javascript
// ── Pure functions ───────────────────────────────────────────────
const add       = (a, b) => a + b;          // same in → always same out
const double    = (x) => x * 2;
const greet     = (name) => `Hello, ${name}`;
const getLength = (arr) => arr.length;      // reads arr, does not change it

// ── Impure functions (side effects) ─────────────────────────────
let total = 0;
const addToTotal = (n) => { total += n; };  // ❌ mutates external state

const logAndReturn = (x) => {
  console.log(x);                           // ❌ I/O is a side effect
  return x;
};

const getRandom = () => Math.random();      // ❌ different output each call
const getDate   = () => new Date();         // ❌ depends on external state

// ── Why it matters ───────────────────────────────────────────────
// Pure functions are:
// • Testable  — no setup/teardown, just assert(fn(input) === output)
// • Cacheable — same input = same output, safe to memoize
// • Parallelizable — no shared state to corrupt
// • Predictable — no hidden dependencies

// Side effects are NECESSARY — but isolate them at the edges
// Pure core → impure shell (I/O, DB, UI updates)
```

**Identifying side effects:**

```
SIDE EFFECTS include:
  • Modifying variables outside the function scope
  • Mutating function arguments (objects/arrays)
  • Reading/writing to DOM, localStorage, files, DB
  • HTTP requests, console.log, Date.now(), Math.random()
  • Throwing exceptions (observable state change)
```

---

## 15.3 Immutability

**Definition:** Never modify existing data — return new data instead.

```javascript
// ── Arrays — immutable patterns ──────────────────────────────────
const original = [1, 2, 3, 4, 5];

// ❌ Mutating originals
original.push(6);
original.splice(1, 1);
original.sort();

// ✅ Return new arrays
const withSix   = [...original, 6];
const without2  = original.filter((_, i) => i !== 1);
const sorted    = [...original].sort((a, b) => a - b);
const updated   = original.map((x, i) => (i === 2 ? 99 : x));  // change index 2

// ── Objects — immutable patterns ─────────────────────────────────
const user = { name: "Alice", age: 25, role: "user" };

// ❌ Mutating
user.age  = 26;
user.role = "admin";
delete user.name;

// ✅ Return new objects
const olderUser    = { ...user, age: 26 };
const promotedUser = { ...user, role: "admin" };
const noName       = (({ name, ...rest }) => rest)(user);

// ── Nested immutability ───────────────────────────────────────────
const state = {
  user: { name: "Alice", prefs: { theme: "light", lang: "en" } },
  posts: [{ id: 1, title: "Hello" }],
};

// ✅ Update nested field immutably
const newState = {
  ...state,
  user: {
    ...state.user,
    prefs: { ...state.user.prefs, theme: "dark" },
  },
};

// ── Object.freeze for enforcement ────────────────────────────────
const config = Object.freeze({
  API_URL: "https://api.example.com",
  TIMEOUT: 5000,
});

config.API_URL = "other";  // silently ignored (strict: TypeError)
config.API_URL;            // "https://api.example.com" — unchanged

// Note: freeze is shallow — nested objects still mutable
const deepFreeze = (obj) => {
  Object.keys(obj).forEach((key) => {
    if (typeof obj[key] === "object" && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
  return Object.freeze(obj);
};
```

---

## 15.4 First-Class & Higher-Order Functions

**First-class functions:** Functions are values — they can be assigned, passed, and returned just like numbers or strings.

**Higher-order functions (HOF):** Functions that take other functions as arguments, or return functions as results.

```javascript
// ── Functions as values ───────────────────────────────────────────
const sayHi   = () => "Hi!";             // assigned to variable
const greetFn = sayHi;                   // copied like any value
const fns     = [Math.sqrt, Math.abs];   // stored in array
const ops     = { add: (a, b) => a + b, mul: (a, b) => a * b };

// ── HOF: accepts function ─────────────────────────────────────────
function applyTwice(fn, value) {
  return fn(fn(value));
}
applyTwice((x) => x + 3, 7);  // 13  ← (7+3)+3

function applyAll(fns, value) {
  return fns.reduce((v, fn) => fn(v), value);
}
applyAll([Math.abs, Math.sqrt, Math.floor], -16);  // 4

// ── HOF: returns function (function factory) ─────────────────────
function multiplier(factor) {
  return (n) => n * factor;  // closes over factor
}
const double  = multiplier(2);
const triple  = multiplier(3);
const times10 = multiplier(10);

[1, 2, 3].map(double);   // [2, 4, 6]
[1, 2, 3].map(triple);   // [3, 6, 9]
[1, 2, 3].map(times10);  // [10, 20, 30]

// ── HOF: both ─────────────────────────────────────────────────────
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

const expensiveFib = memoize(function fib(n) {
  if (n <= 1) return n;
  return expensiveFib(n - 1) + expensiveFib(n - 2);
});
expensiveFib(40);  // fast — cached
```

---

## 15.5 map / filter / reduce — Deep Dive

The holy trinity of functional array transformation.

```
DATA PIPELINE MENTAL MODEL:
────────────────────────────────────────────────────────────────

  Raw Data
     │
     ▼ filter()  — keep what passes the test
     │
     ▼ map()     — transform each element
     │
     ▼ reduce()  — accumulate into final shape
     │
     ▼
  Result
```

```javascript
// ── map: transform each element → new array of same length ───────
// map(fn) → fn(value, index, array)

[1, 2, 3].map((n) => n ** 2);                       // [1, 4, 9]
["alice", "bob"].map((s) => s.toUpperCase());        // ["ALICE", "BOB"]
[{x:1},{x:2}].map((o) => o.x);                      // [1, 2]  — pluck
[1, 2, 3].map((n, i) => `${i}:${n}`);               // ["0:1","1:2","2:3"]

// ── filter: keep elements where fn returns true ───────────────────
// filter(fn) → fn(value, index, array)

[1,2,3,4,5,6].filter((n) => n % 2 === 0);           // [2, 4, 6]
["", "a", null, "b", undefined].filter(Boolean);    // ["a", "b"]  ← clean!
users.filter((u) => u.active && u.age >= 18);

// ── reduce: accumulate all elements into one value ────────────────
// reduce(fn, initial) → fn(accumulator, value, index, array)

[1,2,3,4,5].reduce((sum, n) => sum + n, 0);         // 15
[1,2,3,4,5].reduce((max, n) => n > max ? n : max, -Infinity); // 5

// Build object from array
const votes = ["Alice","Bob","Alice","Carol","Bob","Alice"];
votes.reduce((tally, name) => {
  tally[name] = (tally[name] ?? 0) + 1;
  return tally;
}, {});
// { Alice: 3, Bob: 2, Carol: 1 }

// Group by property
const people = [
  { name: "Alice", dept: "Eng" },
  { name: "Bob",   dept: "UX"  },
  { name: "Carol", dept: "Eng" },
];
people.reduce((groups, person) => {
  (groups[person.dept] ??= []).push(person);
  return groups;
}, {});
// { Eng: [Alice, Carol], UX: [Bob] }

// Flatten with reduce
[[1,2],[3,4],[5,6]].reduce((flat, arr) => [...flat, ...arr], []);
// [1,2,3,4,5,6]

// ── Chaining pipelines ────────────────────────────────────────────
const orders = [
  { product: "Laptop", price: 999,  qty: 2, active: true  },
  { product: "Mouse",  price: 29,   qty: 5, active: true  },
  { product: "Desk",   price: 450,  qty: 1, active: false },
  { product: "Chair",  price: 350,  qty: 3, active: true  },
];

const totalRevenue = orders
  .filter((o) => o.active)
  .map((o) => o.price * o.qty)
  .reduce((sum, revenue) => sum + revenue, 0);
// (999*2) + (29*5) + (350*3) = 1998 + 145 + 1050 = 3193
```

---

## 15.6 Function Composition

**Definition:** Combine simple functions to build complex ones. Output of one function becomes input of the next.

```
COMPOSITION DIAGRAM:
────────────────────────────────────────────────────────────────

  compose(f, g)(x)  =  f(g(x))   ← right to left
  pipe(f, g)(x)     =  g(f(x))   ← left to right (more natural)

  pipe(trim, toLower, exclaim)("  HELLO  ")
       │        │        │
       ▼        ▼        ▼
  "  HELLO  " → "hello" → "hello" → "hello!"
```

```javascript
// ── compose: right-to-left ────────────────────────────────────────
const compose = (...fns) => (x) => fns.reduceRight((v, fn) => fn(v), x);

// ── pipe: left-to-right (more readable) ──────────────────────────
const pipe = (...fns) => (x) => fns.reduce((v, fn) => fn(v), x);

// ── Building blocks ───────────────────────────────────────────────
const trim    = (s) => s.trim();
const toLower = (s) => s.toLowerCase();
const toUpper = (s) => s.toUpperCase();
const exclaim = (s) => `${s}!`;
const words   = (s) => s.split(" ");
const join    = (sep) => (arr) => arr.join(sep);
const replace = (from, to) => (s) => s.replace(new RegExp(from, "g"), to);

// ── Compose pipelines ─────────────────────────────────────────────
const normalize  = pipe(trim, toLower);
const shout      = pipe(trim, toUpper, exclaim);
const slug       = pipe(trim, toLower, replace(" ", "-"));
const titleCase  = pipe(
  trim,
  words,
  (ws) => ws.map((w) => w[0].toUpperCase() + w.slice(1)),
  join(" ")
);

normalize("  Hello World  ");  // "hello world"
shout("  hello  ");            // "HELLO!"
slug(" My Blog Post ");        // "my-blog-post"
titleCase("the quick brown fox");  // "The Quick Brown Fox"

// ── Compose with multiple-arg functions ───────────────────────────
// Use currying to make functions pipeline-compatible (single arg)
const map    = (fn) => (arr) => arr.map(fn);
const filter = (fn) => (arr) => arr.filter(fn);
const reduce = (fn, init) => (arr) => arr.reduce(fn, init);

const sumOfDoubledEvens = pipe(
  filter((n) => n % 2 === 0),
  map((n) => n * 2),
  reduce((sum, n) => sum + n, 0)
);

sumOfDoubledEvens([1, 2, 3, 4, 5, 6]);  // (2+4+6)*2 = 24
```

---

## 15.7 Currying & Partial Application

**Currying:** Transform a function of N args into N functions of 1 arg each.
**Partial Application:** Pre-fill some arguments, return a function for the rest.

```javascript
// ── Manual currying ───────────────────────────────────────────────
// Uncurried: add(a, b, c)
const addUncurried = (a, b, c) => a + b + c;

// Curried: add(a)(b)(c)
const add = (a) => (b) => (c) => a + b + c;

add(1)(2)(3);   // 6
const add1   = add(1);       // (b) => (c) => 1 + b + c
const add1_2 = add(1)(2);    // (c) => 1 + 2 + c
add1_2(3);      // 6

// ── Auto-curry utility ────────────────────────────────────────────
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function (...moreArgs) {
      return curried.apply(this, [...args, ...moreArgs]);
    };
  };
}

const curriedAdd = curry((a, b, c) => a + b + c);
curriedAdd(1)(2)(3);   // 6
curriedAdd(1, 2)(3);   // 6
curriedAdd(1)(2, 3);   // 6
curriedAdd(1, 2, 3);   // 6  ← all styles work

// ── Partial application ───────────────────────────────────────────
function partial(fn, ...presetArgs) {
  return function (...laterArgs) {
    return fn(...presetArgs, ...laterArgs);
  };
}

const multiply  = (a, b) => a * b;
const double    = partial(multiply, 2);
const triple    = partial(multiply, 3);

double(5);   // 10
triple(4);   // 12

// ── Real-world currying ───────────────────────────────────────────
// Curried functions are perfect for pipelines
const prop       = (key)       => (obj)    => obj[key];
const propEq     = (key, val)  => (obj)    => obj[key] === val;
const sortBy     = (key)       => (arr)    => [...arr].sort((a,b) => a[key] > b[key] ? 1 : -1);
const mapProp    = (key)       => (arr)    => arr.map(prop(key));
const filterProp = (key, val)  => (arr)    => arr.filter(propEq(key, val));

const users = [
  { name: "Charlie", age: 30, active: true  },
  { name: "Alice",   age: 25, active: true  },
  { name: "Bob",     age: 35, active: false },
];

const getActiveNames = pipe(
  filterProp("active", true),
  sortBy("name"),
  mapProp("name")
);

getActiveNames(users);  // ["Alice", "Charlie"]
```

```
CURRYING vs PARTIAL APPLICATION:
────────────────────────────────────────────────────────────────
  Currying:            f(a, b, c)  →  f(a)(b)(c)
  — always one arg at a time
  — arity is fixed (one argument per call)

  Partial Application: f(a, b, c)  →  f(a)(b, c)
  — any number of args per call
  — fills args from left, returns fn for remaining args
```

---

## 15.8 Closures as State

In FP, closures replace mutable objects as containers for encapsulated state.

```javascript
// ── Counter using closure (no class needed) ───────────────────────
function createCounter(initial = 0, step = 1) {
  let count = initial;

  return {
    increment: ()  => (count += step),
    decrement: ()  => (count -= step),
    reset:     ()  => (count = initial),
    value:     ()  => count,
    toString:  ()  => `Counter(${count})`,
  };
}

const c = createCounter(10, 2);
c.increment();  // 12
c.increment();  // 14
c.decrement();  // 12
c.value();      // 12
// count is completely private — no way to access it directly

// ── Once — run a function exactly once ───────────────────────────
const once = (fn) => {
  let called = false, result;
  return (...args) => {
    if (!called) { called = true; result = fn(...args); }
    return result;
  };
};

const initApp = once(() => {
  console.log("Initialized!");
  return { ready: true };
});
initApp();  // logs "Initialized!" → { ready: true }
initApp();  // silent           → { ready: true }  (cached)

// ── Accumulator ───────────────────────────────────────────────────
const makeAccumulator = (initial = 0) => {
  let total = initial;
  return (n) => (total += n);
};

const bank = makeAccumulator(1000);
bank(500);   // 1500
bank(-200);  // 1300
bank(0);     // 1300

// ── Lazy evaluation ───────────────────────────────────────────────
const lazy = (fn) => {
  let computed = false, value;
  return () => {
    if (!computed) { value = fn(); computed = true; }
    return value;
  };
};

const expensiveConfig = lazy(() => {
  console.log("Computing config...");
  return { db: "localhost", port: 5432 };
});

expensiveConfig();  // logs "Computing config..." → config
expensiveConfig();  // silent → same config (cached)
```

---

## 15.9 Functors & Monads (Practical)

**Functor:** Any container that implements `.map()` — transforms the value inside without changing the container.
**Monad:** A functor that also implements `.chain()` (flatMap) — handles nested containers and chaining operations that return containers.

```javascript
// ── Arrays are functors ───────────────────────────────────────────
[1, 2, 3].map((x) => x * 2);   // [2, 4, 6]  — same container, new values

// ── Maybe Monad — safe handling of null/undefined ─────────────────
class Maybe {
  #value;

  constructor(value) { this.#value = value; }

  static of(value)    { return new Maybe(value); }
  static empty()      { return new Maybe(null);  }

  isNothing()         { return this.#value == null; }

  // Functor — map: transform if value exists
  map(fn) {
    return this.isNothing() ? Maybe.empty() : Maybe.of(fn(this.#value));
  }

  // Monad — chain: fn returns a Maybe (avoids Maybe(Maybe(...)))
  chain(fn) {
    return this.isNothing() ? Maybe.empty() : fn(this.#value);
  }

  // Extract with default
  getOrElse(defaultValue) {
    return this.isNothing() ? defaultValue : this.#value;
  }

  toString() {
    return this.isNothing() ? "Maybe(Nothing)" : `Maybe(${this.#value})`;
  }
}

// ── Safe property access — no optional chaining needed ───────────
const getUser    = (id) => Maybe.of(users.find((u) => u.id === id));
const getAddress = (user) => Maybe.of(user.address);
const getCity    = (addr) => Maybe.of(addr.city);

const cityName = getUser(1)
  .chain(getAddress)
  .chain(getCity)
  .map((city) => city.toUpperCase())
  .getOrElse("Unknown City");
// No null errors at any step — Maybe handles it

// ── Result / Either Monad — explicit error handling ───────────────
class Result {
  #value; #error; #isOk;

  constructor(isOk, value, error) {
    this.#isOk  = isOk;
    this.#value = value;
    this.#error = error;
  }

  static ok(value)    { return new Result(true,  value, null);  }
  static err(error)   { return new Result(false, null,  error); }

  isOk()  { return this.#isOk; }
  isErr() { return !this.#isOk; }

  map(fn) {
    return this.#isOk ? Result.ok(fn(this.#value)) : this;
  }

  chain(fn) {
    return this.#isOk ? fn(this.#value) : this;
  }

  match({ ok, err }) {
    return this.#isOk ? ok(this.#value) : err(this.#error);
  }
}

// Usage — railway-oriented programming
const parseJSON = (str) => {
  try { return Result.ok(JSON.parse(str)); }
  catch(e) { return Result.err(`Invalid JSON: ${e.message}`); }
};

const validateUser = (data) =>
  data.name ? Result.ok(data) : Result.err("Name is required");

const saveUser = (user) => Result.ok({ ...user, id: Date.now() });

const result = parseJSON('{"name":"Alice","age":25}')
  .chain(validateUser)
  .chain(saveUser)
  .map((user) => ({ ...user, createdAt: new Date() }));

result.match({
  ok:  (user)  => console.log("Saved:", user),
  err: (error) => console.error("Failed:", error),
});
```

---

## 15.10 Transducers

**Definition:** Composable, high-performance algorithmic transformations. Combine map/filter/reduce without creating intermediate arrays.

```javascript
// ── The performance problem with chaining ─────────────────────────
// Each step creates a new intermediate array
[1,2,3,4,5,6,7,8,9,10]
  .filter((n) => n % 2 === 0)  // → [2,4,6,8,10]  intermediate!
  .map((n) => n * 3)           // → [6,12,18,24,30] intermediate!
  .reduce((s, n) => s + n, 0); // → 90

// ── Transducer approach — one pass, no intermediates ─────────────
const mapT = (fn) => (reducer) => (acc, val) => reducer(acc, fn(val));
const filterT = (pred) => (reducer) => (acc, val) =>
  pred(val) ? reducer(acc, val) : acc;

const compose = (...fns) => (x) => fns.reduceRight((v, fn) => fn(v), x);

const xform = compose(
  filterT((n) => n % 2 === 0),  // composed transforms
  mapT((n) => n * 3)
);

const sum = (acc, n) => acc + n;

[1,2,3,4,5,6,7,8,9,10].reduce(xform(sum), 0);  // 90 — single pass!

// ── transduce utility ─────────────────────────────────────────────
function transduce(xform, reducer, initial, collection) {
  return collection.reduce(xform(reducer), initial);
}

// Building a result array instead of sum
const append = (arr, val) => [...arr, val];

transduce(xform, append, [], [1,2,3,4,5,6,7,8,9,10]);
// [6, 12, 18, 24, 30] — filtered evens, tripled, in one pass
```

---

## 15.11 FP Patterns in Real Codebases

```javascript
// ── 1. Data transformation pipeline (API response → UI state) ────
const transformOrders = pipe(
  filter((o) => o.status === "completed"),
  map(({ id, items, total, createdAt }) => ({
    id,
    total:    `$${total.toFixed(2)}`,
    itemCount: items.length,
    date:      new Date(createdAt).toLocaleDateString(),
  })),
  (orders) => orders.sort((a, b) => b.id - a.id)
);

// ── 2. Redux-style reducer (pure function) ────────────────────────
const initialState = { count: 0, loading: false, error: null };

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + (action.payload ?? 1) };
    case "DECREMENT":
      return { ...state, count: state.count - 1 };
    case "RESET":
      return initialState;
    default:
      return state;  // always return state — never mutate!
  }
};

// ── 3. Validator composition ──────────────────────────────────────
const required     = (val) => (val != null && val !== "") || "Required";
const minLength    = (n) => (val) => val.length >= n || `Min ${n} chars`;
const maxLength    = (n) => (val) => val.length <= n || `Max ${n} chars`;
const isEmail      = (val) => /\S+@\S+\.\S+/.test(val) || "Invalid email";
const isNumeric    = (val) => !isNaN(val) || "Must be a number";

const validate = (...rules) => (value) =>
  rules.reduce((errors, rule) => {
    const result = rule(value);
    return result === true ? errors : [...errors, result];
  }, []);

const validateEmail    = validate(required, isEmail);
const validatePassword = validate(required, minLength(8), maxLength(64));
const validateAge      = validate(required, isNumeric);

validateEmail("alice@example.com");  // []
validateEmail("not-an-email");       // ["Invalid email"]
validateEmail("");                   // ["Required"]

// ── 4. Event handler pipeline ─────────────────────────────────────
const handleSearch = pipe(
  (e) => e.target.value,           // extract value
  (s) => s.trim(),                 // clean
  (s) => s.toLowerCase(),          // normalize
  debounce(fetchResults, 300)      // rate-limit side effect
);

searchInput.addEventListener("input", handleSearch);
```

---

## 15.12 Interview Questions

### Q1 — What makes a function pure? Give an impure example and fix it.

```javascript
// ❌ Impure — depends on external state and mutates input
const TAX_RATE = 0.1;
function addTax(prices) {
  for (let i = 0; i < prices.length; i++) {
    prices[i] *= (1 + TAX_RATE);  // mutates the input array!
  }
  return prices;
}
```

<details>
<summary>Answer</summary>

```javascript
// ✅ Pure — same input always returns same output, no mutations
function addTax(prices, taxRate) {  // taxRate passed in, not closed over
  return prices.map((price) => price * (1 + taxRate));  // new array
}

addTax([10, 20, 30], 0.1);  // [11, 22, 33]
// original prices array unchanged
```

</details>

---

### Q2 — Implement `pipe` and explain how it differs from `compose`.

<details>
<summary>Answer</summary>

```javascript
// pipe: left-to-right execution (more readable)
const pipe = (...fns) => (x) => fns.reduce((v, fn) => fn(v), x);

// compose: right-to-left (mathematical notation f∘g)
const compose = (...fns) => (x) => fns.reduceRight((v, fn) => fn(v), x);

const double  = (x) => x * 2;
const addOne  = (x) => x + 1;
const square  = (x) => x * x;

pipe(double, addOne, square)(3);     // square(addOne(double(3))) = square(7) = 49
compose(square, addOne, double)(3);  // same result — functions in reverse order
```

</details>

---

### Q3 — What is currying and why is it useful?

<details>
<summary>Answer</summary>

```javascript
// Currying transforms f(a,b,c) into f(a)(b)(c)
// Useful because:
// 1. Creates specialized functions from general ones
// 2. Makes functions compatible with pipe/compose (single arg)
// 3. Enables point-free style

const curry = (fn) => {
  const arity = fn.length;
  return function curried(...args) {
    return args.length >= arity
      ? fn(...args)
      : (...more) => curried(...args, ...more);
  };
};

const add  = curry((a, b) => a + b);
const add5 = add(5);  // specialized function
[1,2,3].map(add5);    // [6, 7, 8]

// Point-free: describe what to do without mentioning data
const isEven   = (n) => n % 2 === 0;
const getEvens = (arr) => arr.filter(isEven);  // no `arr` argument mentioned
```

</details>

---

> **Module 15 Complete** — FP in JavaScript is about writing predictable, composable, testable code. Start with pure functions and immutability — the rest follows naturally.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 15 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 16 — Advanced Asynchronous JavaScript

> **"Async is not about doing things faster — it's about not waiting when you don't have to."**
> This module goes beyond basic Promises and async/await into production-grade patterns.

---


## 16.1 Async Mental Model Recap

```
THE JAVASCRIPT ASYNC PIPELINE:
────────────────────────────────────────────────────────────────
  Sync Code          Microtask Queue      Macrotask Queue
  ─────────          ───────────────      ───────────────
  Runs first         Promise callbacks    setTimeout
  Fills call stack   queueMicrotask()     setInterval
  Blocks nothing     MutationObserver     I/O callbacks
                     ← HIGH PRIORITY →    ← LOWER PRIORITY →

  Rule: Call stack empties → drain ALL microtasks → one macrotask
        → drain ALL microtasks → next macrotask → repeat
```

```javascript
// Execution order — predict this before reading the output
console.log("1");                           // sync

setTimeout(() => console.log("2"), 0);     // macrotask

Promise.resolve()
  .then(() => console.log("3"))             // microtask
  .then(() => console.log("4"));            // microtask (chained)

queueMicrotask(() => console.log("5"));    // microtask

console.log("6");                           // sync

// Output: 1, 6, 3, 5, 4, 2
// Explanation:
// Sync:       1, 6
// Microtasks: 3 (first .then) → 5 (queueMicrotask) → 4 (second .then)
// Macrotask:  2 (setTimeout)
```

---

## 16.2 Promise Internals

Understanding how Promises work under the hood prevents subtle bugs.

```javascript
// ── Building a minimal Promise from scratch ───────────────────────
class MiniPromise {
  #state    = "pending";   // "pending" | "fulfilled" | "rejected"
  #value    = undefined;
  #handlers = [];

  constructor(executor) {
    const resolve = (value) => {
      if (this.#state !== "pending") return;
      this.#state = "fulfilled";
      this.#value = value;
      this.#handlers.forEach((h) => h.onFulfilled(value));
    };

    const reject = (reason) => {
      if (this.#state !== "pending") return;
      this.#state = "rejected";
      this.#value = reason;
      this.#handlers.forEach((h) => h.onRejected(reason));
    };

    try { executor(resolve, reject); }
    catch (e) { reject(e); }
  }

  then(onFulfilled, onRejected) {
    return new MiniPromise((resolve, reject) => {
      const handle = { onFulfilled, onRejected };

      if (this.#state === "fulfilled") {
        queueMicrotask(() => {
          try { resolve(onFulfilled?.(this.#value) ?? this.#value); }
          catch (e) { reject(e); }
        });
      } else if (this.#state === "rejected") {
        queueMicrotask(() => {
          try {
            if (onRejected) resolve(onRejected(this.#value));
            else reject(this.#value);
          } catch (e) { reject(e); }
        });
      } else {
        this.#handlers.push(handle);
      }
    });
  }

  catch(onRejected) { return this.then(undefined, onRejected); }
  finally(fn) { return this.then((v) => (fn(), v), (e) => { fn(); throw e; }); }
}
```

```
PROMISE STATE MACHINE:
────────────────────────────────────────────────────────────────

  ┌─────────────┐   resolve(value)   ┌───────────────┐
  │   PENDING   │──────────────────►│  FULFILLED    │
  │             │                   │  value = x    │
  │  (initial)  │   reject(reason)  └───────┬───────┘
  │             │──────────────────►        │ .then(onFulfilled)
  └─────────────┘                  ┌────────▼──────┐
                                   │   REJECTED    │
                                   │  reason = e   │
                                   └───────┬───────┘
                                           │ .catch(onRejected)
                                    Both states → .finally()

  Key properties:
  • State transitions are one-way and irreversible
  • Callbacks always run asynchronously (via microtask queue)
  • .then() always returns a NEW Promise — enables chaining
  • Unhandled rejections trigger `unhandledrejection` event
```

---

## 16.3 Promise Combinators — All Patterns

```javascript
const p1 = fetch("/api/users");
const p2 = fetch("/api/posts");
const p3 = fetch("/api/comments");

// ── Promise.all ───────────────────────────────────────────────────
// All must succeed → [results]. First rejection → rejects immediately.
const [users, posts, comments] = await Promise.all([p1, p2, p3]);
// Total time = max(t1, t2, t3) — true parallelism
// ⚠️ If any one fails, you lose all results

// ── Promise.allSettled ────────────────────────────────────────────
// All run regardless → [{status, value/reason}]. Never rejects.
const results = await Promise.allSettled([p1, p2, p3]);
results.forEach(({ status, value, reason }) => {
  if (status === "fulfilled") process(value);
  else                        logError(reason);
});
// ✅ Use when partial success is acceptable

// ── Promise.race ──────────────────────────────────────────────────
// First to settle (fulfill OR reject) wins.
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timeout after ${ms}ms`)), ms)
  );
  return Promise.race([promise, timeout]);
}
const data = await withTimeout(fetch("/api/slow"), 5000);

// ── Promise.any ───────────────────────────────────────────────────
// First to FULFILL wins. Rejects only if ALL reject (AggregateError).
const fastest = await Promise.any([
  fetch("https://mirror1.example.com/data"),
  fetch("https://mirror2.example.com/data"),
  fetch("https://mirror3.example.com/data"),
]);
// ✅ Fastest server wins — great for redundancy

// ── Comparison table ──────────────────────────────────────────────
```

| Combinator             | Resolves When       | Rejects When          | Returns           | Use Case                  |
| ---------------------- | ------------------- | --------------------- | ----------------- | ------------------------- |
| `Promise.all`          | ALL fulfill         | ANY rejects           | Array of results  | All required, fast-fail   |
| `Promise.allSettled`   | ALL settle          | Never                 | Array of outcomes | Partial success OK        |
| `Promise.race`         | FIRST settles       | FIRST settles         | Single result     | Timeout, first-wins       |
| `Promise.any`          | FIRST fulfills      | ALL reject            | Single result     | Redundancy, fastest-wins  |

---

## 16.4 Async Iteration & `for-await-of`

```javascript
// ── Async iterable protocol ───────────────────────────────────────
// Object is async iterable if it has [Symbol.asyncIterator]()
// which returns an object with async next() method

const asyncRange = {
  from: 1,
  to: 5,
  [Symbol.asyncIterator]() {
    let current = this.from;
    const last  = this.to;
    return {
      async next() {
        await new Promise((r) => setTimeout(r, 100));  // simulate delay
        return current <= last
          ? { value: current++, done: false }
          : { value: undefined, done: true };
      },
    };
  },
};

for await (const num of asyncRange) {
  console.log(num);  // 1, 2, 3, 4, 5  (100ms apart)
}

// ── Paginated API with for-await-of ──────────────────────────────
async function* paginate(url) {
  let nextUrl = url;
  while (nextUrl) {
    const res  = await fetch(nextUrl);
    const data = await res.json();
    yield data.items;                         // yield each page
    nextUrl = data.nextPageUrl ?? null;       // null stops the loop
  }
}

for await (const page of paginate("/api/users?page=1")) {
  page.forEach((user) => processUser(user));
}

// ── Reading a stream with for-await-of ───────────────────────────
async function streamLines(url) {
  const res    = await fetch(url);
  const reader = res.body.getReader();
  const decoder = new TextDecoder();
  let buffer = "";

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    buffer += decoder.decode(value, { stream: true });
    const lines = buffer.split("\n");
    buffer = lines.pop();               // keep incomplete last line
    for (const line of lines) yield line;
  }
  if (buffer) yield buffer;
}
```

---

## 16.5 Async Generators

**Definition:** Generators that can `await` inside them and `yield` async values. The perfect tool for producing streams of async data.

```javascript
// ── Basic async generator ─────────────────────────────────────────
async function* countdown(from) {
  for (let i = from; i >= 0; i--) {
    await new Promise((r) => setTimeout(r, 1000));
    yield i;
  }
}

for await (const n of countdown(5)) {
  console.log(n);  // 5, 4, 3, 2, 1, 0  (1 second apart)
}

// ── Async generator pipeline ──────────────────────────────────────
async function* fetchUsers(ids) {
  for (const id of ids) {
    const res  = await fetch(`/api/users/${id}`);
    const user = await res.json();
    yield user;
  }
}

async function* filterActive(users) {
  for await (const user of users) {
    if (user.active) yield user;
  }
}

async function* mapToName(users) {
  for await (const user of users) {
    yield user.name;
  }
}

// Composable pipeline — lazy, processes one at a time
const names = mapToName(filterActive(fetchUsers([1, 2, 3, 4, 5])));
for await (const name of names) {
  console.log(name);
}

// ── Infinite event stream ─────────────────────────────────────────
async function* webSocketMessages(url) {
  const ws = new WebSocket(url);
  const queue = [];
  let resolve;

  ws.onmessage = (e) => {
    if (resolve) { resolve(e.data); resolve = null; }
    else queue.push(e.data);
  };

  try {
    while (true) {
      if (queue.length > 0) {
        yield queue.shift();
      } else {
        yield new Promise((r) => { resolve = r; });
      }
    }
  } finally {
    ws.close();
  }
}

for await (const msg of webSocketMessages("wss://example.com")) {
  handleMessage(msg);
}
```

---

## 16.6 Concurrency Patterns

```javascript
// ── Sequential vs Parallel — the classic mistake ─────────────────
async function sequential(ids) {
  const results = [];
  for (const id of ids) {
    results.push(await fetchUser(id));  // waits for each before next
  }
  return results;
  // Time: sum(t1 + t2 + t3 + ...)
}

async function parallel(ids) {
  return Promise.all(ids.map((id) => fetchUser(id)));  // all at once
  // Time: max(t1, t2, t3, ...)
}

// ── Controlled concurrency — limit parallel requests ─────────────
async function pooled(tasks, limit = 5) {
  const results = new Array(tasks.length);
  const executing = new Set();

  for (const [i, task] of tasks.entries()) {
    const p = Promise.resolve().then(() => task()).then((res) => {
      executing.delete(p);
      results[i] = res;
    });
    executing.add(p);

    if (executing.size >= limit) {
      await Promise.race(executing);  // wait for one to finish
    }
  }

  await Promise.all(executing);  // drain remaining
  return results;
}

// Example: process 100 items, max 5 at a time
const tasks = urls.map((url) => () => fetch(url).then((r) => r.json()));
const results = await pooled(tasks, 5);

// ── Semaphore — general concurrency control ───────────────────────
class Semaphore {
  #permits;
  #queue = [];

  constructor(permits) { this.#permits = permits; }

  async acquire() {
    if (this.#permits > 0) {
      this.#permits--;
      return;
    }
    await new Promise((resolve) => this.#queue.push(resolve));
  }

  release() {
    if (this.#queue.length > 0) {
      this.#queue.shift()();   // wake next waiter
    } else {
      this.#permits++;
    }
  }

  async use(fn) {
    await this.acquire();
    try     { return await fn(); }
    finally { this.release();   }
  }
}

const sem = new Semaphore(3);

await Promise.all(
  urls.map((url) => sem.use(() => fetch(url).then((r) => r.json())))
);

// ── Request deduplication ─────────────────────────────────────────
class RequestCache {
  #pending = new Map();

  async fetch(key, fn) {
    if (this.#pending.has(key)) {
      return this.#pending.get(key);  // reuse in-flight request
    }
    const promise = fn().finally(() => this.#pending.delete(key));
    this.#pending.set(key, promise);
    return promise;
  }
}

const cache = new RequestCache();
// Two simultaneous calls for the same user → only one network request
const [u1, u2] = await Promise.all([
  cache.fetch("user:1", () => fetchUser(1)),
  cache.fetch("user:1", () => fetchUser(1)),
]);
```

---

## 16.7 Cancellation: AbortController

```javascript
// ── Basic fetch cancellation ──────────────────────────────────────
const controller = new AbortController();
const { signal } = controller;

// Cancel after 5 seconds
const timeoutId = setTimeout(() => controller.abort(), 5000);

try {
  const res = await fetch("/api/data", { signal });
  clearTimeout(timeoutId);
  return await res.json();
} catch (err) {
  if (err.name === "AbortError") {
    console.log("Request cancelled");
  } else {
    throw err;
  }
}

// ── React-style: cancel on component unmount ──────────────────────
function useData(url) {
  const [data, setData] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    fetch(url, { signal: controller.signal })
      .then((r) => r.json())
      .then((d) => setData(d))
      .catch((err) => {
        if (err.name !== "AbortError") setError(err);
      });

    return () => controller.abort();  // cleanup = cancel
  }, [url]);

  return data;
}

// ── Cancellable async function wrapper ────────────────────────────
function cancellable(asyncFn) {
  let controller;

  const wrapped = async (...args) => {
    controller?.abort();
    controller = new AbortController();
    return asyncFn(...args, controller.signal);
  };

  wrapped.cancel = () => controller?.abort();
  return wrapped;
}

const searchUsers = cancellable(async (query, signal) => {
  const res = await fetch(`/api/search?q=${query}`, { signal });
  return res.json();
});

// Typing fast — previous requests are auto-cancelled
searchInput.addEventListener("input", (e) => searchUsers(e.target.value));

// ── Propagating cancellation ──────────────────────────────────────
async function complexOperation(signal) {
  signal.throwIfAborted();  // check before starting

  const step1 = await fetchStep1(signal);

  signal.throwIfAborted();  // check between steps

  const step2 = await fetchStep2(step1.id, signal);
  return step2;
}
```

---

## 16.8 Advanced Error Handling

```javascript
// ── Custom error hierarchy ────────────────────────────────────────
class AppError extends Error {
  constructor(message, options = {}) {
    super(message, { cause: options.cause });
    this.name       = this.constructor.name;
    this.statusCode = options.statusCode ?? 500;
    this.code       = options.code       ?? "UNKNOWN_ERROR";
    this.retryable  = options.retryable  ?? false;
  }
}

class NetworkError   extends AppError {
  constructor(msg, opts) { super(msg, { ...opts, code: "NETWORK_ERROR",    retryable: true  }); }
}
class NotFoundError  extends AppError {
  constructor(msg, opts) { super(msg, { ...opts, code: "NOT_FOUND",        statusCode: 404  }); }
}
class AuthError      extends AppError {
  constructor(msg, opts) { super(msg, { ...opts, code: "UNAUTHORIZED",     statusCode: 401  }); }
}
class ValidationError extends AppError {
  constructor(msg, fields) {
    super(msg, { code: "VALIDATION_ERROR", statusCode: 422 });
    this.fields = fields;
  }
}

// ── Retry with exponential backoff ────────────────────────────────
async function retry(fn, {
  attempts   = 3,
  delay      = 1000,
  factor     = 2,
  maxDelay   = 30000,
  retryWhen  = (e) => e.retryable ?? false,
  onRetry    = () => {},
} = {}) {
  let lastError;
  let currentDelay = delay;

  for (let attempt = 1; attempt <= attempts; attempt++) {
    try {
      return await fn();
    } catch (err) {
      lastError = err;
      if (attempt === attempts || !retryWhen(err)) throw err;

      onRetry({ attempt, error: err, nextDelay: currentDelay });
      await new Promise((r) => setTimeout(r, currentDelay));
      currentDelay = Math.min(currentDelay * factor, maxDelay);
    }
  }
  throw lastError;
}

// Usage
const user = await retry(
  () => fetchUser(userId),
  {
    attempts: 5,
    delay: 500,
    retryWhen: (e) => e instanceof NetworkError,
    onRetry: ({ attempt, nextDelay }) =>
      console.warn(`Retry ${attempt} in ${nextDelay}ms`),
  }
);

// ── Result pattern — no try/catch pollution ───────────────────────
async function safeRun(fn) {
  try {
    return { ok: true,  value: await fn() };
  } catch (error) {
    return { ok: false, error };
  }
}

const { ok, value: user, error } = await safeRun(() => fetchUser(1));
if (!ok) { handleError(error); return; }
render(user);

// ── Global async error boundary ───────────────────────────────────
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled rejection:", reason);
  // Send to error tracking service
  errorTracker.captureException(reason);
  // Graceful shutdown if critical
});

window.addEventListener("unhandledrejection", (event) => {
  event.preventDefault();
  errorTracker.captureException(event.reason);
});
```

---

## 16.9 Streams

```javascript
// ── ReadableStream — produce data lazily ─────────────────────────
const readable = new ReadableStream({
  start(controller) {
    // Enqueue initial data
    controller.enqueue("chunk 1");
    controller.enqueue("chunk 2");
  },
  pull(controller) {
    // Called when consumer wants more data
    if (done) controller.close();
    else      controller.enqueue(nextChunk());
  },
  cancel(reason) {
    // Consumer stopped reading
    cleanup(reason);
  },
});

// ── TransformStream — process data mid-pipe ───────────────────────
const uppercase = new TransformStream({
  transform(chunk, controller) {
    controller.enqueue(chunk.toUpperCase());
  },
});

const lineBreaker = new TransformStream({
  transform(chunk, controller) {
    controller.enqueue(chunk + "\n");
  },
});

// ── Piping streams ────────────────────────────────────────────────
await readable
  .pipeThrough(uppercase)
  .pipeThrough(lineBreaker)
  .pipeTo(new WritableStream({
    write(chunk) { process.stdout.write(chunk); },
  }));

// ── Fetch response is a ReadableStream ───────────────────────────
async function streamJSON(url) {
  const res     = await fetch(url);
  const reader  = res.body.getReader();
  const decoder = new TextDecoder();
  let body = "";

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    body += decoder.decode(value, { stream: true });

    // Progress tracking
    const received = +res.headers.get("content-length");
    onProgress(body.length / received);
  }

  return JSON.parse(body);
}
```

---

## 16.10 Scheduling: `queueMicrotask` & `scheduler`

```javascript
// ── queueMicrotask — run at end of current microtask checkpoint ───
function batchUpdates(fn) {
  let scheduled = false;
  const queue   = [];

  return function (...args) {
    queue.push(args);
    if (!scheduled) {
      scheduled = true;
      queueMicrotask(() => {
        scheduled = false;
        const batch = queue.splice(0);
        fn(batch);
      });
    }
  };
}

const batchedUpdate = batchUpdates((items) => {
  console.log("Batch:", items);  // all items queued before next microtask
});

batchedUpdate(1);
batchedUpdate(2);
batchedUpdate(3);
// One batch call: [[1], [2], [3]]

// ── scheduler.postTask — priority-based scheduling (modern) ───────
if ("scheduler" in globalThis) {
  // background: lowest priority, runs when idle
  scheduler.postTask(() => prefetchData(),      { priority: "background" });

  // user-visible: normal priority
  scheduler.postTask(() => renderChart(),       { priority: "user-visible" });

  // user-blocking: highest priority (same as microtask)
  scheduler.postTask(() => handleKeypress(),    { priority: "user-blocking" });
}

// ── requestIdleCallback — run during browser idle time ────────────
function scheduleIdleWork(fn) {
  if ("requestIdleCallback" in window) {
    requestIdleCallback((deadline) => {
      while (deadline.timeRemaining() > 0) {
        fn();
      }
    }, { timeout: 2000 });
  } else {
    setTimeout(fn, 0);  // fallback
  }
}

scheduleIdleWork(() => sendAnalytics(events));
```

---

## 16.11 Production Patterns

```javascript
// ── 1. Circuit breaker — stop hammering failing services ─────────
class CircuitBreaker {
  #state      = "closed";   // closed | open | half-open
  #failures   = 0;
  #lastFail   = null;
  #threshold;
  #resetTime;

  constructor({ threshold = 5, resetTime = 60000 } = {}) {
    this.#threshold  = threshold;
    this.#resetTime  = resetTime;
  }

  async call(fn) {
    if (this.#state === "open") {
      if (Date.now() - this.#lastFail > this.#resetTime) {
        this.#state = "half-open";
      } else {
        throw new Error("Circuit breaker OPEN — service unavailable");
      }
    }

    try {
      const result = await fn();
      if (this.#state === "half-open") {
        this.#state    = "closed";
        this.#failures = 0;
      }
      return result;
    } catch (err) {
      this.#failures++;
      this.#lastFail = Date.now();
      if (this.#failures >= this.#threshold) this.#state = "open";
      throw err;
    }
  }
}

const breaker = new CircuitBreaker({ threshold: 3, resetTime: 30000 });
const data    = await breaker.call(() => fetch("/api/fragile"));

// ── 2. Request queue with priority ────────────────────────────────
class PriorityQueue {
  #high   = [];
  #normal = [];
  #low    = [];
  #running = 0;
  #concurrency;

  constructor(concurrency = 3) { this.#concurrency = concurrency; }

  enqueue(fn, priority = "normal") {
    return new Promise((resolve, reject) => {
      const task = async () => {
        try { resolve(await fn()); }
        catch (e) { reject(e); }
        finally { this.#running--; this.#next(); }
      };
      this[`#${priority}`].push(task);
      this.#next();
    });
  }

  #next() {
    while (this.#running < this.#concurrency) {
      const task = this.#high.shift() ?? this.#normal.shift() ?? this.#low.shift();
      if (!task) break;
      this.#running++;
      task();
    }
  }
}

const queue = new PriorityQueue(3);
queue.enqueue(() => fetchCriticalData(), "high");
queue.enqueue(() => prefetchData(),      "low");

// ── 3. Cache with TTL and stale-while-revalidate ──────────────────
class AsyncCache {
  #store = new Map();

  async get(key, fetcher, { ttl = 60000, staleWhileRevalidate = false } = {}) {
    const cached = this.#store.get(key);
    const now    = Date.now();

    if (cached) {
      const isStale = now - cached.timestamp > ttl;
      if (!isStale) return cached.value;

      if (staleWhileRevalidate) {
        // Return stale value immediately, refresh in background
        this.#refresh(key, fetcher);
        return cached.value;
      }
    }

    return this.#refresh(key, fetcher);
  }

  async #refresh(key, fetcher) {
    const value = await fetcher();
    this.#store.set(key, { value, timestamp: Date.now() });
    return value;
  }
}
```

---

## 16.12 Interview Questions

### Q1 — What is the output and why?

```javascript
async function main() {
  console.log("A");
  await Promise.resolve();
  console.log("B");
  await new Promise((r) => setTimeout(r, 0));
  console.log("C");
}

main();
console.log("D");
```

<details>
<summary>Answer</summary>

```
A, D, B, C

A — sync, inside main before first await
D — sync, after main() call (main pauses at await)
B — microtask: Promise.resolve() resolves immediately, resumes after D
C — macrotask: setTimeout(r, 0) is a timer, runs after all microtasks
```

</details>

---

### Q2 — What's wrong with this code?

```javascript
async function loadAll(ids) {
  const results = [];
  for (const id of ids) {
    const user = await fetchUser(id);
    results.push(user);
  }
  return results;
}
```

<details>
<summary>Answer</summary>

```javascript
// Problem: sequential — each request waits for the previous one.
// With 10 IDs each taking 500ms → 5 seconds total.

// Fix: parallel with Promise.all
async function loadAll(ids) {
  return Promise.all(ids.map((id) => fetchUser(id)));
  // ~500ms total regardless of array size
}

// Fix with concurrency limit (avoid overwhelming the server)
async function loadAll(ids, limit = 5) {
  return pooled(ids.map((id) => () => fetchUser(id)), limit);
}
```

</details>

---

### Q3 — Implement `Promise.all` from scratch

<details>
<summary>Answer</summary>

```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    if (promises.length === 0) { resolve([]); return; }

    const results  = new Array(promises.length);
    let   resolved = 0;

    promises.forEach((p, i) => {
      Promise.resolve(p).then((value) => {
        results[i] = value;
        if (++resolved === promises.length) resolve(results);
      }).catch(reject);  // first rejection immediately rejects
    });
  });
}
```

</details>

---

> **Module 16 Complete** — Advanced async mastery means knowing not just the APIs, but the concurrency models, failure modes, and production patterns that make async code reliable at scale.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 16 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 17 — JavaScript Engine Internals

> **"You don't need to know how the engine works — until performance matters."**
> Understanding V8 internals lets you write code that the JIT compiler loves.

---


## 17.1 Engine Architecture Overview

```
V8 ENGINE PIPELINE:
────────────────────────────────────────────────────────────────

  JavaScript Source Code
          │
          ▼
  ┌───────────────────┐
  │   Scanner/Lexer   │  Tokenizes characters → tokens
  └────────┬──────────┘
           │
           ▼
  ┌───────────────────┐
  │      Parser       │  Tokens → Abstract Syntax Tree (AST)
  │   (Full / Lazy)   │  Lazy: skip function bodies until called
  └────────┬──────────┘
           │ AST
           ▼
  ┌───────────────────────────────────────────────────────┐
  │                     IGNITION                          │
  │               (Interpreter / Bytecode)                │
  │                                                       │
  │  AST → Bytecode  (compact, portable instructions)    │
  │  Executes bytecode directly                           │
  │  Collects feedback: which types, which paths are hot │
  └────────┬──────────────────────────────────────────────┘
           │ "Hot" functions (called frequently)
           │ Type feedback collected
           ▼
  ┌───────────────────────────────────────────────────────┐
  │                     TURBOFAN                          │
  │               (Optimizing JIT Compiler)               │
  │                                                       │
  │  Bytecode + type feedback → Optimized machine code   │
  │  Inlines functions, eliminates type checks            │
  │  Deoptimizes if types change ("bailout")              │
  └────────┬──────────────────────────────────────────────┘
           │
           ▼
  Native Machine Code Execution

  Key insight: V8 is a feedback-driven JIT compiler.
  It optimizes based on how your code actually runs.
```

**Major JS engines:**

| Engine               | Used By                        | Key Feature                      |
| -------------------- | ------------------------------ | -------------------------------- |
| **V8**               | Chrome, Node.js, Edge, Deno    | Ignition + TurboFan              |
| **SpiderMonkey**     | Firefox                        | Warp JIT, first JS engine        |
| **JavaScriptCore**   | Safari, React Native           | LLInt → Baseline → DFG → FTL    |
| **Hermes**           | React Native (mobile)          | AOT bytecode, optimized for RN   |

---

## 17.2 Parsing & the AST

```javascript
// This source code:
const add = (a, b) => a + b;

// Becomes this AST (simplified):
{
  type: "VariableDeclaration",
  kind: "const",
  declarations: [{
    type: "VariableDeclarator",
    id: { type: "Identifier", name: "add" },
    init: {
      type: "ArrowFunctionExpression",
      params: [
        { type: "Identifier", name: "a" },
        { type: "Identifier", name: "b" }
      ],
      body: {
        type: "BinaryExpression",
        operator: "+",
        left:  { type: "Identifier", name: "a" },
        right: { type: "Identifier", name: "b" }
      }
    }
  }]
}
```

**Lazy parsing — V8's performance trick:**

```
EAGER vs LAZY PARSING:
────────────────────────────────────────────────────────────────

  // Eager: parse entire file upfront
  // Problem: large apps have thousands of functions not called on load

  // V8's lazy parsing:
  // 1. Scan and pre-parse all function bodies (just syntax check)
  // 2. Only fully parse + compile functions when first called
  // 3. Result: faster startup time

  // You can hint V8 to eager-parse (IIFE pattern):
  (function eagerly() { /* ... */ })();  // ← IIFE = eager parse
  // V8 knows this runs immediately, so it's worth parsing now

  // Chrome DevTools → Coverage tab shows unused JS (lazy wins here)
```

---

## 17.3 Ignition: The Interpreter

Ignition produces **bytecode** — a compact, platform-independent instruction set that Ignition executes directly.

```
IGNITION PIPELINE:
────────────────────────────────────────────────────────────────
  AST
   │
   ▼ BytecodeGenerator
  Bytecode (register-based virtual machine instructions)
   │
   ▼ Interpreter
  Execution (with type feedback collection)

  Benefits of bytecode vs direct machine code:
  • Compact: bytecode is ~3-4x smaller than equivalent machine code
  • Portable: same bytecode on x64, ARM, MIPS
  • Fast startup: cheaper to generate than machine code
  • Feedback: Ignition collects type info for TurboFan
```

```javascript
// Example bytecode for: function add(a, b) { return a + b; }
// (conceptual — actual bytecode differs)

// LdaNamedProperty a0, [0]   ← load `a` into accumulator
// Star r0                     ← store in register 0
// LdaNamedProperty a1, [0]   ← load `b` into accumulator
// Add r0, [1]                 ← add r0 + accumulator
// Return                      ← return accumulator

// Ignition profile example — visualize with:
// node --print-bytecode --print-bytecode-filter=add script.js
```

---

## 17.4 TurboFan: The JIT Compiler

TurboFan takes **hot bytecode + type feedback** and compiles to optimized native machine code.

```
TURBOFAN OPTIMIZATION PIPELINE:
────────────────────────────────────────────────────────────────

  Bytecode + Type Feedback
          │
          ▼
  ┌──────────────────┐
  │  Sea of Nodes    │  Graph-based IR (nodes = ops, edges = data flow)
  │  (Intermediate   │
  │   Representation)│
  └────────┬─────────┘
           │
           ▼ Optimization passes:
           │ • Inlining: replace fn calls with fn body
           │ • Type specialization: int+int skips type checks
           │ • Escape analysis: stack-allocate non-escaping objects
           │ • Dead code elimination
           │ • Loop optimizations
           ▼
  ┌──────────────────┐
  │  Machine Code    │  x64 / ARM native instructions
  └──────────────────┘

  When types change → DEOPTIMIZATION (bailout back to Ignition)
  Too many deoptimizations → function marked as "megamorphic", never
  optimized again.
```

```javascript
// ── Why type consistency matters ──────────────────────────────────
function add(a, b) { return a + b; }

// ✅ V8 optimizes for int + int
for (let i = 0; i < 1e6; i++) add(1, 2);

// ❌ Suddenly called with strings → DEOPTIMIZE (throw away machine code)
add("hello", " world");  // type changed → bailout → back to bytecode

// ── Speculative optimization ──────────────────────────────────────
// V8 "speculates" that types won't change.
// It bets on the most common case and generates optimized code for it.
// If the bet is wrong → deoptimization penalty.
```

---

## 17.5 Hidden Classes & Inline Caches

**Hidden Classes (Shapes/Maps):** V8 internally creates hidden classes to make property access as fast as C++ struct access.

```
HIDDEN CLASS CREATION:
────────────────────────────────────────────────────────────────

  const p1 = {};        // Hidden class C0: {}
  p1.x = 1;            // Transition → Hidden class C1: {x}
  p1.y = 2;            // Transition → Hidden class C2: {x, y}

  const p2 = {};        // Hidden class C0: {}
  p2.x = 10;           // Transition → Hidden class C1: {x}  ← SHARES C1!
  p2.y = 20;           // Transition → Hidden class C2: {x, y} ← SHARES C2!

  // p1 and p2 share the same hidden class C2
  // V8 can use a single fast property lookup path for both

  ┌──────────────────────────────────────────────────────┐
  │  C0 {}                                               │
  │   │ add x → C1 {x}                                  │
  │              │ add y → C2 {x, y}   ← p1 and p2 here │
  └──────────────────────────────────────────────────────┘
```

```javascript
// ✅ Same property order = shared hidden class = fast
function createPoint(x, y) {
  return { x, y };  // always same shape → shared hidden class
}
const points = Array.from({ length: 1000 }, (_, i) => createPoint(i, i));

// ❌ Different property order = different hidden classes = slow
const p1 = { x: 1, y: 2 };  // hidden class: {x, y}
const p2 = { y: 2, x: 1 };  // hidden class: {y, x} — DIFFERENT!

// ❌ Adding properties dynamically = transitions = slower
function createUser(name) {
  const u = {};
  u.name = name;   // C0 → C1
  u.age  = 0;      // C1 → C2
  u.role = "user"; // C2 → C3  (3 transitions per object)
  return u;
}

// ✅ Assign all properties at construction
function createUser(name) {
  return { name, age: 0, role: "user" };  // one shape, no transitions
}
```

**Inline Caches (ICs):**

```
INLINE CACHE TYPES:
────────────────────────────────────────────────────────────────
  Monomorphic  — one hidden class seen  →  FAST (direct slot access)
  Polymorphic  — 2-4 hidden classes     →  OK   (small lookup table)
  Megamorphic  — 5+ hidden classes      →  SLOW (generic hash lookup)

  function getX(obj) { return obj.x; }

  getX({ x: 1, y: 2 });          // monomorphic — C0
  getX({ x: 1, y: 2 });          // monomorphic — still C0  ✅ FAST
  getX({ x: 1, y: 2, z: 3 });   // polymorphic — C0 and C1  OK
  getX({ a: 1, x: 2 });          // megamorphic — too many   SLOW
```

---

## 17.6 Garbage Collection Deep Dive

```
V8 GENERATIONAL GC:
────────────────────────────────────────────────────────────────

  ┌──────────────────────────────────────────────────────────┐
  │                    HEAP MEMORY                            │
  │                                                          │
  │  ┌─────────────────────────┐                            │
  │  │   NEW SPACE (1-8 MB)    │  ← Young generation        │
  │  │                         │                            │
  │  │  ┌──────────┐           │  Semi-space copying GC:    │
  │  │  │  From    │           │  • Allocate in "from"      │
  │  │  │  Space   │           │  • GC: copy live objects   │
  │  │  └──────────┘           │    to "to" space           │
  │  │  ┌──────────┐           │  • Swap from/to            │
  │  │  │   To     │           │  • Dead objects freed      │
  │  │  │  Space   │           │  Runs frequently (fast)    │
  │  │  └──────────┘           │                            │
  │  └─────────────────────────┘                            │
  │                                                          │
  │  ┌─────────────────────────┐                            │
  │  │   OLD SPACE (~1.5 GB)   │  ← Old generation          │
  │  │                         │                            │
  │  │  Long-lived objects     │  Mark-Sweep-Compact GC:    │
  │  │  (survived 2+ minor GCs)│  • Mark all live objects   │
  │  │                         │  • Sweep dead objects      │
  │  └─────────────────────────┘  • Compact fragmented mem  │
  │                                  Runs rarely (slow)      │
  │  ┌────────┐  ┌────────────┐                             │
  │  │  Code  │  │  Large     │                             │
  │  │  Space │  │  Object    │                             │
  │  │(bytecode│  │  Space     │                             │
  │  │machine │  │(>256KB obj)│                             │
  │  │  code) │  └────────────┘                             │
  │  └────────┘                                             │
  └──────────────────────────────────────────────────────────┘
```

**Orinoco: Concurrent & Incremental GC:**

```
TRADITIONAL GC:           ORINOCO (V8's modern GC):
──────────────────────    ──────────────────────────────────────
Main thread:              Main thread:
  ██ RUN ██ STOP █ GC █     ██ RUN ██ RUN ██ RUN ██
              │                        │
         STW: full stop         Concurrent marking:
         = jank/freeze          GC thread works alongside JS
                                Short incremental pauses only

  Techniques:
  • Concurrent marking: GC marks while JS runs
  • Incremental marking: split GC work into small slices
  • Parallel scavenging: multiple GC threads for minor GC
  • Idle-time GC: run GC during browser idle
```

---

## 17.7 V8 Optimization Killers

These patterns prevent V8 from generating optimized code.

```javascript
// ── 1. Inconsistent argument types ───────────────────────────────
function process(value) { return value * 2; }

process(5);         // V8 optimizes for Number
process("hello");   // DEOPTIMIZE — type changed

// ✅ Fix: keep types consistent
const processNum = (n) => n * 2;
const processStr = (s) => s.repeat(2);

// ── 2. Changing object shape after creation ───────────────────────
function createUser() {
  const user = {};
  user.name = "Alice";  // C0 → C1
  user.age  = 25;       // C1 → C2
  return user;
}
// ✅ Fix: all properties at once
function createUser() { return { name: "Alice", age: 25 }; }

// ── 3. Deleting properties ────────────────────────────────────────
const obj = { x: 1, y: 2 };
delete obj.x;  // ❌ changes hidden class → possible megamorphic path

// ✅ Fix: set to null/undefined instead
obj.x = undefined;  // same hidden class, property still "exists"

// ── 4. Sparse arrays ─────────────────────────────────────────────
const arr = [1, 2, 3];
arr[100] = 4;   // ❌ sparse — V8 switches from fast packed to slow

// ✅ Fix: always use contiguous arrays
const arr = new Array(100).fill(0);  // dense, all elements initialized

// ── 5. Mixed element kinds ────────────────────────────────────────
// V8 tracks array "element kinds" for optimization
// PACKED_SMI_ELEMENTS  → all small integers   (fastest)
// PACKED_DOUBLE_ELEMENTS → floats added
// PACKED_ELEMENTS       → objects/mixed added (slowest)

const ints = [1, 2, 3];        // PACKED_SMI_ELEMENTS ✅
ints.push(1.5);                 // → PACKED_DOUBLE_ELEMENTS (transition)
ints.push("hello");             // → PACKED_ELEMENTS (slowest, irreversible)

// ✅ Keep arrays homogeneous

// ── 6. arguments object ──────────────────────────────────────────
// ❌ Old pattern — arguments is a special object, hard to optimize
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) total += arguments[i];
  return total;
}

// ✅ Rest params — regular array, fully optimizable
const sum = (...nums) => nums.reduce((a, b) => a + b, 0);

// ── 7. try/catch in hot paths ────────────────────────────────────
// try/catch blocks can prevent inlining in some V8 versions
// ✅ Move try/catch outside the hot loop
function processItems(items) {
  try {
    for (const item of items) hotLoop(item);  // ❌ try wraps hot code
  } catch (e) { handle(e); }
}

function processItemsSafe(items) {
  for (const item of items) {
    try { hotLoop(item); }  // ✅ only wrap what can throw
    catch (e) { handle(e); }
  }
}
```

---

## 17.8 Shapes & Property Access

```javascript
// ── SMI: Small Integer optimization ──────────────────────────────
// V8 represents small integers (-2^30 to 2^30-1) as SMI
// SMIs have no heap allocation — they ARE the value in the pointer

let x = 42;          // SMI — no heap object!
let y = 2147483648;  // HeapNumber — too big for SMI

// ── Property access speed tiers ──────────────────────────────────
/*
  FASTEST: In-object properties (set in constructor)
  ┌──────────────┐
  │   Object     │  Direct memory offset — like C struct field access
  │  .x = 1     │  No lookup needed
  │  .y = 2     │
  └──────────────┘

  FAST: Properties in descriptor array (added dynamically, known shape)
  ┌──────────────┐   ┌──────────────────┐
  │   Object     │──►│ Properties Array  │  One extra pointer hop
  └──────────────┘   └──────────────────┘

  SLOW: Dictionary mode (too many properties, or delete used)
  ┌──────────────┐   ┌──────────────────┐
  │   Object     │──►│  Hash Map        │  Full hash lookup
  └──────────────┘   └──────────────────┘
*/

// ── In-object property slots ──────────────────────────────────────
// V8 reserves "in-object" slots based on constructor analysis
class Point {
  constructor(x, y) {
    this.x = x;  // slot 0 — in-object, fastest
    this.y = y;  // slot 1 — in-object, fastest
  }
}
// V8 pre-allocates 2 in-object slots for all Point instances

function createPoint(x, y) { return { x, y }; }  // same optimization
```

---

## 17.9 Numbers & Typed Arrays

```javascript
// ── V8 Number representation ──────────────────────────────────────
// SMI:        31-bit integer in pointer (no heap)  ← fastest
// HeapNumber: 64-bit IEEE 754 double in heap object
// BigInt:     arbitrary precision, always heap

// ── TypedArrays: bypasses JS overhead ────────────────────────────
// For number-crunching, TypedArrays are dramatically faster

// Regular array (boxing/unboxing overhead)
const arr = new Array(1e6).fill(0.5);  // array of HeapNumbers

// TypedArray (contiguous memory, no boxing)
const floats = new Float64Array(1e6).fill(0.5);  // raw C doubles!
const ints   = new Int32Array(1e6).fill(0);
const bytes  = new Uint8Array(1e6).fill(0);

// ── Performance comparison ────────────────────────────────────────
// TypedArray can be 10-100x faster for numeric work

function sumFloat64Array(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) sum += arr[i];  // no unboxing!
  return sum;
}

// ── SharedArrayBuffer + Atomics: multi-thread shared memory ───────
const sharedBuffer = new SharedArrayBuffer(4);
const sharedView   = new Int32Array(sharedBuffer);

// Worker thread can write, main thread can read
// Must use Atomics for synchronization
Atomics.store(sharedView, 0, 42);
const value = Atomics.load(sharedView, 0);  // 42

// Wait/notify for synchronization
Atomics.wait(sharedView, 0, 42);   // block until value ≠ 42
Atomics.notify(sharedView, 0, 1);  // wake one waiting thread
```

---

## 17.10 Profiling with DevTools

```
PERFORMANCE PROFILING WORKFLOW:
────────────────────────────────────────────────────────────────

  1. FLAME CHART (Performance tab)
     ─────────────────────────────
     • Record → perform actions → stop
     • Yellow = scripting, Purple = rendering, Green = painting
     • Wide bars = slow functions (self time)
     • Deep stacks = call chains

  2. CPU PROFILER (JavaScript Profiler tab)
     ────────────────────────────────────────
     • Profile → record → stop
     • "Heavy (Bottom Up)" shows hottest functions
     • Self time vs total time
     • Look for unexpected entries (type guards, bailouts)

  3. MEMORY PROFILER (Memory tab)
     ─────────────────────────────
     • Heap Snapshot: captures object graph
     • Allocation Timeline: shows allocations over time
     • Compare snapshots to find leaks
     • "Retainers" shows what's keeping objects alive

  4. NODE.JS PROFILING
     ──────────────────
     node --prof script.js        # generate V8 log
     node --prof-process isolate-*.log  # human-readable output

     # Or use clinic.js
     clinic flame -- node server.js
```

```javascript
// ── In-code performance measurement ──────────────────────────────
// High-resolution timing
const start = performance.now();
expensiveOperation();
console.log(`Took: ${performance.now() - start}ms`);

// Mark and measure
performance.mark("op-start");
expensiveOperation();
performance.mark("op-end");
performance.measure("operation", "op-start", "op-end");
console.log(performance.getEntriesByName("operation"));

// ── Detecting deoptimizations ─────────────────────────────────────
// node --trace-deopt script.js
// node --trace-opt --trace-deopt script.js

// ── Checking if function is optimized (debug builds only) ─────────
// %OptimizeFunctionOnNextCall(fn);  // force optimize
// %GetOptimizationStatus(fn);       // check status
```

---

## 17.11 Interview Questions

### Q1 — What is JIT compilation and how does V8 use it?

<details>
<summary>Answer</summary>

```
JIT (Just-In-Time) compiles code at runtime, combining benefits of:
• Interpretation: fast startup, no ahead-of-time compilation
• Compilation: fast execution via native machine code

V8's two-tier JIT:
1. Ignition (interpreter): Converts AST → bytecode, runs immediately.
   Collects type feedback (what types are actually used).

2. TurboFan (optimizing compiler): Takes hot bytecode + type feedback,
   generates optimized native machine code.
   If types change → deoptimization → back to Ignition.

Key insight: V8 bets that types stay consistent. When they do →
fast machine code. When they don't → penalty (bailout + recompile).
```

</details>

---

### Q2 — Why should you initialize all object properties in the constructor?

<details>
<summary>Answer</summary>

```
V8 assigns a "hidden class" (shape) to each object based on its
property layout. Objects with the same layout share the same hidden class,
enabling fast "inline cache" property access.

Adding properties in different orders or at different times creates
different hidden classes:
• Different shapes → separate IC entries → slower access
• Too many shapes → "megamorphic" → falls back to hash map lookup

Initializing all properties in the constructor (or object literal) means:
1. All instances get the same hidden class
2. V8 can directly compute property offsets (like C struct)
3. Property access is effectively O(1) via pointer arithmetic
```

</details>

---

### Q3 — What are the generational GC spaces and why does the split exist?

<details>
<summary>Answer</summary>

```
V8 uses generational GC based on the "generational hypothesis":
most objects die young (temporary allocations, closures, etc.).

New Space (Young Generation, 1-8MB):
• Small, collected frequently (Minor GC / Scavenge)
• Uses copying: live objects copied to "to" space, dead ones abandoned
• Fast because most objects are dead — little to copy

Old Space (Old Generation, ~1.5GB):
• Objects that survived 2+ Minor GCs are "promoted" here
• Collected infrequently (Major GC / Mark-Sweep-Compact)
• More expensive — many live objects to trace

The split means:
• Most allocations never leave New Space (cheap)
• Major GC on small set of long-lived objects
• Result: less total GC time, shorter pauses
```

</details>

---

> **Module 17 Complete** — Understanding engine internals turns performance from guesswork into engineering. Write code that gives V8 stable types and predictable shapes, and it will reward you with machine-code speed.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 17 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 18 — Memory Optimization

> **"Memory leaks don't crash your app immediately — they slowly strangle it."**
> This module covers how to find, fix, and prevent memory problems in production JavaScript.

---


## 18.1 Memory Model Recap

```
JAVASCRIPT MEMORY LAYOUT:
────────────────────────────────────────────────────────────────

  ┌─────────────────────────────────────────────────────┐
  │                    STACK                             │
  │  Fast, fixed-size, LIFO                              │
  │  Stores: primitives, references, call frames         │
  │  Managed: automatically (function returns = freed)   │
  │  Size: ~1-8 MB                                       │
  │                                                      │
  │  ┌─────────────────────────────────────────────┐    │
  │  │  fn2 frame: let y = 20                      │    │
  │  ├─────────────────────────────────────────────┤    │
  │  │  fn1 frame: let x = 10, ref → 0x001         │    │
  │  ├─────────────────────────────────────────────┤    │
  │  │  Global frame                               │    │
  │  └─────────────────────────────────────────────┘    │
  └─────────────────────────────────────────────────────┘

  ┌─────────────────────────────────────────────────────┐
  │                     HEAP                             │
  │  Flexible, large, dynamic                            │
  │  Stores: objects, arrays, functions, closures        │
  │  Managed: Garbage Collector                          │
  │  Size: up to several GB                              │
  │                                                      │
  │  0x001: { name: "Alice", age: 25 }                  │
  │  0x042: [1, 2, 3, 4, 5]                             │
  │  0x087: function greet() { ... }                    │
  │  0x0C1: closure environment { count: 5 }            │
  └─────────────────────────────────────────────────────┘
```

---

## 18.2 How Garbage Collection Decides

V8 uses **reachability** — an object is kept if it can be reached from a "root".

```
REACHABILITY ALGORITHM (Mark & Sweep):
────────────────────────────────────────────────────────────────

  ROOTS (always alive):
  • Global object (window/global)
  • Currently executing function's local variables
  • Call stack variables
  • CPU registers

  MARKING PHASE:
  Start from roots → follow all references → mark reachable objects

        Global
          │
          ├── user ──────────► { name: "Alice" }  ← MARKED (reachable)
          │                        │
          │                        └── address ──► { city: "NYC" }  ← MARKED
          │
          └── cache ──────────► { data: [...] }   ← MARKED

  Orphaned objects (no path from roots):
          ┌──► { temp: "data" }  ← NOT MARKED (unreachable) → FREED

  SWEEP PHASE:
  Free all unmarked objects, add memory to free list

  COMPACT PHASE (sometimes):
  Move live objects together → reduce fragmentation
```

---

## 18.3 The 6 Common Memory Leaks

### Leak 1: Forgotten Event Listeners

```javascript
// ❌ Listener holds reference to bigData forever
function setup() {
  const bigData = new Float64Array(1_000_000);

  document.addEventListener("scroll", function handler() {
    process(bigData);  // bigData cannot be GC'd while handler lives
  });
  // handler is on `document` — lives as long as the page!
}
setup();  // call this 10 times = 10MB leaked

// ✅ Fix: remove listener when done
function setup() {
  const bigData = new Float64Array(1_000_000);

  const handler = () => process(bigData);
  document.addEventListener("scroll", handler);

  return () => document.removeEventListener("scroll", handler);  // cleanup
}

const cleanup = setup();
// Later, when component is destroyed:
cleanup();

// ✅ Fix 2: AbortSignal for grouped cleanup
const controller = new AbortController();
document.addEventListener("scroll",  handler1, { signal: controller.signal });
document.addEventListener("resize",  handler2, { signal: controller.signal });
document.addEventListener("keydown", handler3, { signal: controller.signal });

controller.abort();  // removes ALL three at once
```

### Leak 2: Uncleaned Timers

```javascript
// ❌ Interval keeps running after component is gone
class DataPoller {
  constructor(url) {
    this.data = null;
    // ❌ Stores reference to `this` in closure — prevents GC
    setInterval(() => {
      fetch(url).then((r) => r.json()).then((d) => (this.data = d));
    }, 5000);
  }
  // No cleanup method — interval runs forever
}

// ✅ Fix: always return or expose a cleanup method
class DataPoller {
  #intervalId = null;
  data = null;

  start(url) {
    this.#intervalId = setInterval(async () => {
      this.data = await fetch(url).then((r) => r.json());
    }, 5000);
    return this;
  }

  stop() {
    clearInterval(this.#intervalId);
    this.#intervalId = null;
  }
}

// React equivalent
useEffect(() => {
  const id = setInterval(fetchData, 5000);
  return () => clearInterval(id);  // ← cleanup function
}, []);
```

### Leak 3: Detached DOM Nodes

```javascript
// ❌ Node removed from DOM but still referenced in JS
const cache = new Map();

function cacheNode(key) {
  const div = document.createElement("div");
  document.body.appendChild(div);
  cache.set(key, div);           // strong reference kept!
  document.body.removeChild(div);// removed from DOM...
  // div still in cache → NOT garbage collected
}

// ✅ Fix: use WeakMap — key can be GC'd if no other references
const cache = new WeakMap();

function cacheNode(key, node) {
  cache.set(node, key);  // node is the WeakMap key — GC-able
}
```

### Leak 4: Closures Over Large Objects

```javascript
// ❌ Handler closes over the entire large response
async function loadAndProcess(url) {
  const response = await fetch(url).then((r) => r.json());
  const { importantField, anotherField } = response;
  const bigPayload = response.data;  // large array

  return function handler() {
    // handler only uses importantField — but closes over ALL of response
    return importantField.toUpperCase();
  };
  // bigPayload held in memory as long as handler is referenced!
}

// ✅ Fix: extract only what you need before returning
async function loadAndProcess(url) {
  const response = await fetch(url).then((r) => r.json());
  const important = response.importantField;  // extract
  // response and bigPayload not captured in closure
  return () => important.toUpperCase();
}
```

### Leak 5: Accidental Globals

```javascript
// ❌ Missing declaration → attaches to window
function calculate() {
  result = 42;           // ← no let/const/var! Becomes window.result
  total  = result * 2;  // ← same
}

// ✅ Always declare variables
function calculate() {
  const result = 42;
  const total  = result * 2;
  return total;
}

// ✅ "use strict" catches this
"use strict";
function calculate() {
  result = 42;  // ReferenceError: result is not defined  ← caught!
}
```

### Leak 6: Caches Without Eviction

```javascript
// ❌ Cache grows unbounded
const cache = new Map();

function expensiveCompute(input) {
  if (cache.has(input)) return cache.get(input);
  const result = doWork(input);
  cache.set(input, result);  // never evicted!
  return result;
}
// After millions of calls: OOM

// ✅ Fix 1: LRU Cache with size limit
class LRUCache {
  #map    = new Map();
  #maxSize;

  constructor(maxSize = 100) { this.#maxSize = maxSize; }

  get(key) {
    if (!this.#map.has(key)) return undefined;
    const value = this.#map.get(key);
    this.#map.delete(key);
    this.#map.set(key, value);  // move to end (most recent)
    return value;
  }

  set(key, value) {
    if (this.#map.has(key)) this.#map.delete(key);
    else if (this.#map.size >= this.#maxSize) {
      // Delete least recently used (first entry)
      this.#map.delete(this.#map.keys().next().value);
    }
    this.#map.set(key, value);
  }

  has(key)    { return this.#map.has(key); }
  get size()  { return this.#map.size;     }
}

// ✅ Fix 2: TTL cache
class TTLCache {
  #store = new Map();
  #ttl;

  constructor(ttlMs = 60_000) { this.#ttl = ttlMs; }

  set(key, value) {
    this.#store.set(key, { value, expires: Date.now() + this.#ttl });
  }

  get(key) {
    const entry = this.#store.get(key);
    if (!entry) return undefined;
    if (Date.now() > entry.expires) { this.#store.delete(key); return undefined; }
    return entry.value;
  }
}
```

---

## 18.4 WeakRef & FinalizationRegistry

```javascript
// ── WeakRef: hold reference without preventing GC ─────────────────
class ImageCache {
  #cache = new Map();

  set(id, image) {
    this.#cache.set(id, new WeakRef(image));  // won't prevent GC
  }

  get(id) {
    const ref = this.#cache.get(id);
    if (!ref) return null;

    const image = ref.deref();  // returns object or undefined (if GC'd)
    if (!image) {
      this.#cache.delete(id);   // clean up stale entry
      return null;
    }
    return image;
  }
}

// ── FinalizationRegistry: run callback when object is GC'd ────────
const registry = new FinalizationRegistry((heldValue) => {
  // Runs after the target object is garbage collected
  console.log(`${heldValue} was collected`);
  cleanupResources(heldValue);
});

function createResource(name) {
  const resource = { data: new Float64Array(1_000_000) };
  registry.register(resource, name);  // watch `resource`, pass `name` to callback
  return resource;
}

let res = createResource("bigData");
res = null;  // eligible for GC → FinalizationRegistry callback will fire (eventually)

// ── WeakRef cache with auto-cleanup ──────────────────────────────
class WeakCache {
  #refs    = new Map();
  #registry;

  constructor() {
    this.#registry = new FinalizationRegistry((key) => {
      this.#refs.delete(key);   // auto-remove when target is GC'd
    });
  }

  set(key, value) {
    const ref = new WeakRef(value);
    this.#refs.set(key, ref);
    this.#registry.register(value, key);
  }

  get(key) {
    return this.#refs.get(key)?.deref();
  }
}
```

---

## 18.5 WeakMap & WeakSet — When to Use

```javascript
// ── WeakMap: private data per object instance ─────────────────────
// Keys must be objects. Keys are weakly held — GC can collect key+value.

const privateData = new WeakMap();

class SecureUser {
  constructor(name, password) {
    privateData.set(this, { password, loginAttempts: 0 });
    this.name = name;
  }

  login(attempt) {
    const priv = privateData.get(this);
    if (priv.loginAttempts >= 3) throw new Error("Account locked");
    if (attempt !== priv.password) {
      priv.loginAttempts++;
      return false;
    }
    priv.loginAttempts = 0;
    return true;
  }
}

const u = new SecureUser("Alice", "secret123");
// u.password → undefined  (not on instance)
// privateData.get(u).password → "secret123"  (only internal access)

// When `u` is GC'd, privateData entry is automatically removed ✅

// ── WeakMap: metadata without affecting GC ────────────────────────
const elementMeta = new WeakMap();

function attachMeta(el, data) {
  elementMeta.set(el, data);  // el removed from DOM → both el and data GC'd
}

// vs Map: attachMeta keeps el in memory even after DOM removal

// ── WeakSet: track objects without preventing GC ──────────────────
const processed = new WeakSet();

function processOnce(item) {
  if (processed.has(item)) return;   // already done
  doWork(item);
  processed.add(item);
  // When `item` is GC'd, processed entry is removed automatically
}
```

**Map vs WeakMap decision:**

| Need                              | Use         |
| --------------------------------- | ----------- |
| Iterate keys/values               | `Map`       |
| Know the size                     | `Map`       |
| Keys are primitive strings/numbers| `Map`       |
| Keys are DOM elements or objects  | `WeakMap`   |
| Data should live only while key lives | `WeakMap` |
| Private instance data             | `WeakMap`   |

---

## 18.6 Object Pooling

**Definition:** Pre-allocate and reuse objects instead of creating/destroying them in hot paths. Reduces GC pressure.

```javascript
class ObjectPool {
  #pool    = [];
  #create;
  #reset;
  #maxSize;

  constructor({ create, reset = () => {}, maxSize = 100 }) {
    this.#create  = create;
    this.#reset   = reset;
    this.#maxSize = maxSize;
  }

  acquire() {
    return this.#pool.length > 0
      ? this.#pool.pop()       // reuse pooled object
      : this.#create();        // create new if pool empty
  }

  release(obj) {
    if (this.#pool.length < this.#maxSize) {
      this.#reset(obj);        // clean it up
      this.#pool.push(obj);    // return to pool
    }
    // else: discard — pool is full
  }

  get size() { return this.#pool.length; }
}

// ── Vector3 pool for physics engine ──────────────────────────────
const vec3Pool = new ObjectPool({
  create: () => ({ x: 0, y: 0, z: 0 }),
  reset:  (v) => { v.x = 0; v.y = 0; v.z = 0; },
  maxSize: 1000,
});

function computeForce(pos1, pos2) {
  const diff = vec3Pool.acquire();   // no GC allocation!
  diff.x = pos2.x - pos1.x;
  diff.y = pos2.y - pos1.y;
  diff.z = pos2.z - pos1.z;

  const force = magnitude(diff);
  vec3Pool.release(diff);            // return to pool, not GC
  return force;
}

// ── Promise pool for async operations ────────────────────────────
// Pre-allocate resolve/reject wrappers to avoid function allocations
// in tight async loops (advanced pattern for high-throughput services)
```

---

## 18.7 Efficient Data Structures

```javascript
// ── TypedArray vs regular Array ───────────────────────────────────
// Regular array of numbers: each element is a HeapNumber (~32 bytes)
const arr = new Array(1_000_000).fill(3.14);  // ~32 MB

// Float64Array: each element is 8 raw bytes
const typed = new Float64Array(1_000_000).fill(3.14);  // 8 MB

// Operations on TypedArray: no boxing/unboxing → faster

// ── Map vs Object for large keyed data ───────────────────────────
// Object: string keys, prototype overhead, re-hashed on additions
// Map: optimized hash table, any key type, predictable O(1) ops

const map = new Map();
for (let i = 0; i < 1_000_000; i++) map.set(`key_${i}`, i);
// Map is faster for frequent insertions/deletions than plain objects

// ── Set for uniqueness checks ─────────────────────────────────────
// Array.includes = O(n), Set.has = O(1) amortized
const ids = new Set(existingIds);

function isDuplicate(id) { return ids.has(id); }  // O(1)

// ── Compact representations ───────────────────────────────────────
// Store flags in bitmask instead of object of booleans
const FLAGS = {
  ACTIVE:   1 << 0,  // 0b00001 = 1
  ADMIN:    1 << 1,  // 0b00010 = 2
  VERIFIED: 1 << 2,  // 0b00100 = 4
  PREMIUM:  1 << 3,  // 0b01000 = 8
  BANNED:   1 << 4,  // 0b10000 = 16
};

// Instead of { active: true, admin: false, verified: true, ... }
// Use a single integer:
let userFlags = FLAGS.ACTIVE | FLAGS.VERIFIED;  // 0b00101 = 5

const isActive   = (f) => (f & FLAGS.ACTIVE)   !== 0;
const isAdmin    = (f) => (f & FLAGS.ADMIN)    !== 0;
const addFlag    = (f, flag) => f | flag;
const removeFlag = (f, flag) => f & ~flag;
const toggleFlag = (f, flag) => f ^ flag;

isActive(userFlags);   // true
isAdmin(userFlags);    // false
userFlags = addFlag(userFlags, FLAGS.ADMIN);  // grant admin
```

---

## 18.8 Large Data Optimization

```javascript
// ── Virtualization: render only visible items ─────────────────────
// Instead of DOM nodes for 100,000 items, render ~20 (visible window)

class VirtualList {
  #itemHeight;
  #items;
  #visibleCount;
  #container;

  constructor(container, items, itemHeight) {
    this.#container   = container;
    this.#items       = items;
    this.#itemHeight  = itemHeight;
    this.#visibleCount = Math.ceil(container.clientHeight / itemHeight) + 2;

    container.style.position = "relative";
    container.style.height   = `${items.length * itemHeight}px`;
    container.style.overflow = "auto";

    container.addEventListener("scroll", () => this.#render());
    this.#render();
  }

  #render() {
    const scrollTop  = this.#container.scrollTop;
    const startIndex = Math.floor(scrollTop / this.#itemHeight);
    const endIndex   = Math.min(startIndex + this.#visibleCount, this.#items.length);

    this.#container.innerHTML = this.#items
      .slice(startIndex, endIndex)
      .map((item, i) => `
        <div style="position:absolute;top:${(startIndex + i) * this.#itemHeight}px;
                    height:${this.#itemHeight}px">
          ${item.name}
        </div>
      `)
      .join("");
  }
}

// ── Lazy loading / pagination ─────────────────────────────────────
class InfiniteScroll {
  #page  = 1;
  #loading = false;

  constructor(container, loadFn) {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting && !this.#loading) this.#loadNext(loadFn);
    }, { threshold: 0.1 });

    observer.observe(container.querySelector(".sentinel"));
  }

  async #loadNext(loadFn) {
    this.#loading = true;
    const items = await loadFn(this.#page++);
    this.#append(items);
    this.#loading = false;
  }
}

// ── Streaming large JSON ──────────────────────────────────────────
// Don't parse 50MB JSON at once — stream it
async function* streamLargeJSON(url) {
  const res     = await fetch(url);
  const reader  = res.body.getReader();
  const decoder = new TextDecoder();
  let buffer = "";

  for (;;) {
    const { done, value } = await reader.read();
    if (done) break;
    buffer += decoder.decode(value, { stream: true });

    // Find complete JSON objects in the buffer
    let boundary;
    while ((boundary = buffer.indexOf("\n")) !== -1) {
      yield JSON.parse(buffer.slice(0, boundary));
      buffer = buffer.slice(boundary + 1);
    }
  }
}

for await (const record of streamLargeJSON("/api/large-export")) {
  await processRecord(record);  // process one at a time, not all at once
}
```

---

## 18.9 Detecting & Profiling Memory Issues

```
DEVTOOLS MEMORY WORKFLOW:
────────────────────────────────────────────────────────────────

  Step 1: BASELINE SNAPSHOT
  ──────────────────────────
  DevTools → Memory → Heap Snapshot → Take Snapshot
  Note the total heap size

  Step 2: REPRODUCE THE SUSPECTED LEAK
  ──────────────────────────────────────
  Navigate to feature, trigger the suspected leak
  (open/close modal, load/unload component, run operation N times)

  Step 3: FORCE GC + SECOND SNAPSHOT
  ────────────────────────────────────
  Click the trash icon (Collect Garbage) in Memory tab
  Take another Heap Snapshot

  Step 4: COMPARE SNAPSHOTS
  ──────────────────────────
  In Snapshot 2 dropdown → select "Comparison" view
  Sort by "# New" or "Size Delta"
  Look for objects that keep growing

  Step 5: FIND RETAINERS
  ───────────────────────
  Click a suspicious object → expand "Retainers" section
  This shows the reference chain keeping it alive
  Follow the chain to find the leak source

  Red flags:
  • (closure) retainers
  • Detached DOM subtree entries
  • event_listener entries
  • timer/interval entries you don't expect
```

```javascript
// ── In-code memory monitoring ─────────────────────────────────────
// Node.js
const { heapUsed, heapTotal, rss } = process.memoryUsage();
console.log(`Heap: ${(heapUsed / 1024 / 1024).toFixed(2)} MB`);

// Browser
if ("memory" in performance) {
  const { usedJSHeapSize, totalJSHeapSize } = performance.memory;
  console.log(`JS Heap: ${(usedJSHeapSize / 1024 / 1024).toFixed(2)} MB`);
}

// ── Automated leak detection ──────────────────────────────────────
async function detectLeak(action, iterations = 10) {
  // Force baseline GC
  if (global.gc) global.gc();  // requires --expose-gc flag
  const before = process.memoryUsage().heapUsed;

  for (let i = 0; i < iterations; i++) await action();

  if (global.gc) global.gc();
  const after = process.memoryUsage().heapUsed;
  const delta = ((after - before) / 1024 / 1024).toFixed(2);

  if (after > before * 1.1) {
    console.warn(`Possible leak: +${delta} MB after ${iterations} iterations`);
  } else {
    console.log(`OK: ${delta} MB delta`);
  }
}

await detectLeak(async () => {
  const component = createComponent();
  await component.mount();
  await component.destroy();
});
```

---

## 18.10 Interview Questions

### Q1 — Name the 4 most common JavaScript memory leaks

<details>
<summary>Answer</summary>

```
1. Forgotten event listeners
   document.addEventListener() without a corresponding removeEventListener.
   The handler closes over data which cannot be GC'd.

2. Uncleaned timers
   setInterval() without clearInterval() — runs forever, holding references.

3. Detached DOM nodes
   Elements removed from the DOM but still referenced in JS variables or
   Maps/arrays. The GC sees them as reachable — never freed.

4. Closures over large objects
   A returned function closes over a large outer variable unnecessarily.
   The variable lives as long as the function is referenced.

Additional common ones: accidental globals, unbounded caches.
```

</details>

---

### Q2 — When would you use WeakMap over Map?

<details>
<summary>Answer</summary>

```
Use WeakMap when:
1. Keys are objects and you want data to be GC'd when the key object dies
   Example: DOM node → metadata. When node is removed from DOM and
   dereferenced, both the node and metadata are GC'd.

2. Storing private data per instance
   class MyClass {
     constructor() { privateData.set(this, { secret: 42 }); }
   }
   When the instance is GC'd, the private data is too.

3. You don't need to iterate or get the size
   WeakMap has no .size, .keys(), .values(), .forEach()

Use Map when:
• You need to iterate over entries
• You need to know the count
• Keys are primitives (numbers, strings)
• You want the cache to persist regardless of key lifecycle
```

</details>

---

### Q3 — How does the generational GC help performance?

<details>
<summary>Answer</summary>

```
The generational hypothesis: most objects die young.
Temporary allocations (loop variables, intermediate results, closures from
single async operations) rarely survive beyond the function that created them.

V8 splits memory into:
• New Space (small, ~1-8MB): frequent minor GC using copying
• Old Space (large, ~1.5GB): infrequent major GC using mark-sweep

Minor GC is fast because:
• Most objects in New Space are dead — very little to copy
• Only live survivors are copied (work proportional to live objects, not dead)
• Dead objects are "freed" instantly by abandoning the old space

This means the majority of your temporary allocations are reclaimed quickly
and cheaply, with only long-lived objects incurring expensive major GC cost.
```

</details>

---

> **Module 18 Complete** — Memory mastery is the difference between an app that runs for hours and one that runs for days. Profile first, optimize second, prevent leaks always.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 18 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 19 — Design Patterns in JavaScript

> **"Design patterns are solutions to recurring problems — not templates to blindly copy."**
> Every pattern here is shown with a real-world JavaScript use case.

---


## 19.1 Why Design Patterns?

```
PATTERN TAXONOMY (Gang of Four + JS-specific):
────────────────────────────────────────────────────────────────

  ┌─────────────────────────────────────────────────────────┐
  │  CREATIONAL — how objects are created                   │
  │  Singleton, Factory, Abstract Factory, Builder,         │
  │  Prototype                                              │
  └─────────────────────────────────────────────────────────┘

  ┌─────────────────────────────────────────────────────────┐
  │  STRUCTURAL — how objects are composed                  │
  │  Decorator, Proxy, Facade, Adapter, Composite,          │
  │  Flyweight, Bridge                                      │
  └─────────────────────────────────────────────────────────┘

  ┌─────────────────────────────────────────────────────────┐
  │  BEHAVIORAL — how objects communicate                   │
  │  Observer, Strategy, Command, Chain of Responsibility,  │
  │  Iterator, Mediator, State, Template Method             │
  └─────────────────────────────────────────────────────────┘
```

---

## 19.2 Creational Patterns

### Singleton

**Intent:** Ensure a class has only one instance and provide global access to it.

```javascript
// ── ES6 Module Singleton (simplest, most idiomatic JS) ────────────
// Modules are cached — import always returns the same object

// config.js
const config = {
  apiUrl:  "https://api.example.com",
  timeout: 5000,
  debug:   false,
};
export default config;

// Anywhere in the app:
import config from "./config.js";  // always the same object

// ── Class-based Singleton ─────────────────────────────────────────
class Database {
  static #instance = null;
  #connection;

  constructor(connectionString) {
    if (Database.#instance) return Database.#instance;
    this.#connection = connect(connectionString);
    Database.#instance = this;
  }

  static getInstance(cs) { return new Database(cs); }
  query(sql)             { return this.#connection.execute(sql); }
}

const db1 = Database.getInstance("postgres://localhost/mydb");
const db2 = Database.getInstance("postgres://localhost/mydb");
db1 === db2;  // true — same instance

// ── When to use Singleton ─────────────────────────────────────────
// ✅ Database connection pool, logger, config, event bus
// ❌ Anything that needs testing isolation (hard to mock!)
// ✅ Alternative: dependency injection over global singletons
```

---

### Factory Method

**Intent:** Define an interface for creating objects, letting subclasses decide which class to instantiate.

```javascript
// ── Simple Factory (most common in JS) ────────────────────────────
class NotificationFactory {
  static create(type, options) {
    switch (type) {
      case "email":   return new EmailNotification(options);
      case "sms":     return new SMSNotification(options);
      case "push":    return new PushNotification(options);
      case "webhook": return new WebhookNotification(options);
      default:        throw new Error(`Unknown notification type: ${type}`);
    }
  }
}

const notif = NotificationFactory.create("email", {
  to:      "alice@example.com",
  subject: "Welcome!",
  body:    "Hello, Alice.",
});
await notif.send();

// ── Polymorphic classes ────────────────────────────────────────────
class Notification {
  constructor({ to, body }) {
    this.to   = to;
    this.body = body;
  }
  async send() { throw new Error("send() must be implemented"); }
}

class EmailNotification extends Notification {
  constructor({ to, subject, body }) {
    super({ to, body });
    this.subject = subject;
  }
  async send() {
    return emailService.send({ to: this.to, subject: this.subject, body: this.body });
  }
}

class SMSNotification extends Notification {
  async send() {
    return smsService.send({ phone: this.to, message: this.body });
  }
}

// ── Registry-based Factory (extensible) ───────────────────────────
class WidgetFactory {
  static #registry = new Map();

  static register(type, WidgetClass) {
    this.#registry.set(type, WidgetClass);
  }

  static create(type, options) {
    const WidgetClass = this.#registry.get(type);
    if (!WidgetClass) throw new Error(`Widget "${type}" not registered`);
    return new WidgetClass(options);
  }
}

WidgetFactory.register("button",   ButtonWidget);
WidgetFactory.register("dropdown", DropdownWidget);
WidgetFactory.register("chart",    ChartWidget);
// Third parties can register their own:
WidgetFactory.register("calendar", ThirdPartyCalendar);
```

---

### Builder

**Intent:** Separate the construction of a complex object from its representation.

```javascript
class QueryBuilder {
  #query = {
    table:      "",
    conditions: [],
    orderBy:    null,
    limit:      null,
    offset:     null,
    joins:      [],
    select:     ["*"],
  };

  from(table)             { this.#query.table = table;             return this; }
  select(...cols)         { this.#query.select = cols;             return this; }
  where(condition)        { this.#query.conditions.push(condition); return this; }
  join(table, on)         { this.#query.joins.push({ table, on }); return this; }
  orderBy(col, dir="ASC") { this.#query.orderBy = `${col} ${dir}`; return this; }
  limit(n)                { this.#query.limit = n;                 return this; }
  offset(n)               { this.#query.offset = n;               return this; }

  build() {
    if (!this.#query.table) throw new Error("Table is required");
    let sql = `SELECT ${this.#query.select.join(", ")} FROM ${this.#query.table}`;
    for (const { table, on } of this.#query.joins)
      sql += ` JOIN ${table} ON ${on}`;
    if (this.#query.conditions.length)
      sql += ` WHERE ${this.#query.conditions.join(" AND ")}`;
    if (this.#query.orderBy) sql += ` ORDER BY ${this.#query.orderBy}`;
    if (this.#query.limit)   sql += ` LIMIT ${this.#query.limit}`;
    if (this.#query.offset)  sql += ` OFFSET ${this.#query.offset}`;
    return sql;
  }
}

const sql = new QueryBuilder()
  .from("users")
  .select("id", "name", "email")
  .join("orders", "users.id = orders.user_id")
  .where("users.active = true")
  .where("users.age >= 18")
  .orderBy("users.name")
  .limit(20)
  .offset(40)
  .build();
```

---

### Prototype Pattern

**Intent:** Create new objects by cloning an existing object (the prototype).

```javascript
// JavaScript's prototype chain IS the prototype pattern
// Object.create() = explicit prototype pattern

const userPrototype = {
  greet()    { return `Hello, I'm ${this.name}`; },
  serialize(){ return JSON.stringify({ name: this.name, role: this.role }); },
};

function createUser(name, role) {
  const user = Object.create(userPrototype);  // user inherits from prototype
  user.name  = name;
  user.role  = role;
  return user;
}

const alice = createUser("Alice", "admin");
alice.greet();  // "Hello, I'm Alice"

// Deep clone pattern (modern)
const original = { name: "Alice", config: { theme: "dark", lang: "en" } };
const clone     = structuredClone(original);  // deep clone, no shared refs
```

---

## 19.3 Structural Patterns

### Decorator

**Intent:** Add behavior to objects dynamically without altering their class.

```javascript
// ── Function decorator (most Pythonic JS) ─────────────────────────
function logged(fn) {
  return function (...args) {
    console.log(`→ ${fn.name}(${args.join(", ")})`);
    const result = fn.apply(this, args);
    console.log(`← ${fn.name} returned: ${result}`);
    return result;
  };
}

function timed(fn) {
  return function (...args) {
    const start = performance.now();
    const result = fn.apply(this, args);
    console.log(`${fn.name}: ${(performance.now() - start).toFixed(2)}ms`);
    return result;
  };
}

function memoized(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Stack decorators (compose them)
const processData = logged(timed(memoized(expensiveCompute)));

// ── Class-based Decorator ─────────────────────────────────────────
class UserService {
  async getUser(id) { return db.find("users", id); }
}

class CachedUserService {
  #service;
  #cache = new Map();

  constructor(service) { this.#service = service; }

  async getUser(id) {
    if (this.#cache.has(id)) return this.#cache.get(id);
    const user = await this.#service.getUser(id);
    this.#cache.set(id, user);
    return user;
  }
}

class LoggedUserService {
  #service;
  constructor(service) { this.#service = service; }

  async getUser(id) {
    console.log(`Fetching user ${id}`);
    const user = await this.#service.getUser(id);
    console.log(`Got user: ${user.name}`);
    return user;
  }
}

// Layer decorators
const service = new LoggedUserService(new CachedUserService(new UserService()));
```

---

### Proxy Pattern

**Intent:** Provide a surrogate or placeholder for another object to control access.

```javascript
// ── Validation proxy ──────────────────────────────────────────────
function createValidatedUser(data) {
  const validators = {
    name:  (v) => typeof v === "string" && v.length > 0,
    age:   (v) => typeof v === "number" && v >= 0 && v <= 150,
    email: (v) => /\S+@\S+\.\S+/.test(v),
  };

  return new Proxy(data, {
    set(target, key, value) {
      if (key in validators && !validators[key](value)) {
        throw new TypeError(`Invalid value for ${key}: ${value}`);
      }
      target[key] = value;
      return true;
    },
  });
}

const user = createValidatedUser({ name: "Alice", age: 25 });
user.name = "Bob";   // ✅
user.age  = -5;      // TypeError: Invalid value for age: -5

// ── Reactive proxy (Vue 3's core mechanism) ───────────────────────
function reactive(target, onChange) {
  return new Proxy(target, {
    set(obj, key, value) {
      const old = obj[key];
      obj[key] = value;
      if (old !== value) onChange(key, value, old);
      return true;
    },
    get(obj, key) {
      const val = obj[key];
      // Auto-wrap nested objects
      return val && typeof val === "object" ? reactive(val, onChange) : val;
    },
  });
}

const state = reactive({ count: 0, name: "Alice" }, (key, newVal, oldVal) => {
  console.log(`${key}: ${oldVal} → ${newVal}`);
  rerender();
});

state.count++;  // "count: 0 → 1" + rerender

// ── API rate-limit proxy ──────────────────────────────────────────
function rateLimited(api, maxPerSecond = 10) {
  const queue = [];
  let   count = 0;
  const interval = setInterval(() => { count = 0; }, 1000);

  return new Proxy(api, {
    get(target, method) {
      if (typeof target[method] !== "function") return target[method];
      return async (...args) => {
        if (count >= maxPerSecond) {
          await new Promise((r) => queue.push(r));
        }
        count++;
        return target[method](...args);
      };
    },
  });
}
```

---

### Facade

**Intent:** Provide a simplified interface to a complex subsystem.

```javascript
// ── Complex subsystem ─────────────────────────────────────────────
// AuthService, UserService, ProfileService, CacheService, EventBus
// all need to coordinate for a "login" operation

class AuthFacade {
  #auth;
  #users;
  #profiles;
  #cache;
  #events;

  constructor({ auth, users, profiles, cache, events }) {
    this.#auth     = auth;
    this.#users    = users;
    this.#profiles = profiles;
    this.#cache    = cache;
    this.#events   = events;
  }

  async login(email, password) {
    // Coordinates multiple services — caller sees simple interface
    const token   = await this.#auth.authenticate(email, password);
    const user    = await this.#users.findByEmail(email);
    const profile = await this.#profiles.load(user.id);

    this.#cache.set(`user:${user.id}`, { user, profile });
    this.#events.emit("user:login", { userId: user.id });

    return { token, user, profile };
  }

  async logout(userId) {
    await this.#auth.invalidateToken(userId);
    this.#cache.delete(`user:${userId}`);
    this.#events.emit("user:logout", { userId });
  }
}

// Caller only knows about facade — not the 5 services it uses
const auth = new AuthFacade({ auth, users, profiles, cache, events });
const { token, user } = await auth.login("alice@example.com", "password");
```

---

## 19.4 Behavioral Patterns

### Observer / Event Emitter

**Intent:** Define a one-to-many dependency — when one object changes state, all dependents are notified automatically.

```javascript
class EventEmitter {
  #events = new Map();

  on(event, listener) {
    if (!this.#events.has(event)) this.#events.set(event, new Set());
    this.#events.get(event).add(listener);
    // Return unsubscribe function
    return () => this.off(event, listener);
  }

  once(event, listener) {
    const wrapper = (...args) => {
      listener(...args);
      this.off(event, wrapper);
    };
    return this.on(event, wrapper);
  }

  off(event, listener) {
    this.#events.get(event)?.delete(listener);
    return this;
  }

  emit(event, ...args) {
    this.#events.get(event)?.forEach((fn) => fn(...args));
    return this;
  }

  removeAllListeners(event) {
    if (event) this.#events.delete(event);
    else       this.#events.clear();
    return this;
  }
}

// ── Real-world: Store with Observer ──────────────────────────────
class Store extends EventEmitter {
  #state;

  constructor(initial) {
    super();
    this.#state = initial;
  }

  getState()  { return { ...this.#state }; }

  setState(updates) {
    const prev = this.#state;
    this.#state = { ...this.#state, ...updates };
    this.emit("change", this.#state, prev);

    // Granular events per changed key
    Object.keys(updates).forEach((key) => {
      if (updates[key] !== prev[key]) {
        this.emit(`change:${key}`, updates[key], prev[key]);
      }
    });
  }
}

const store = new Store({ count: 0, theme: "light" });

store.on("change:count", (newVal) => updateCounter(newVal));
store.on("change:theme", (theme)  => applyTheme(theme));

store.setState({ count: 5 });   // fires change + change:count
store.setState({ theme: "dark" }); // fires change + change:theme
```

---

### Strategy

**Intent:** Define a family of algorithms, encapsulate each one, and make them interchangeable.

```javascript
// ── Sorting strategies ────────────────────────────────────────────
const sortStrategies = {
  bubble: (arr) => {
    const a = [...arr];
    for (let i = 0; i < a.length; i++)
      for (let j = 0; j < a.length - i - 1; j++)
        if (a[j] > a[j+1]) [a[j], a[j+1]] = [a[j+1], a[j]];
    return a;
  },
  quick: (arr) => {
    if (arr.length <= 1) return arr;
    const [pivot, ...rest] = arr;
    return [
      ...sortStrategies.quick(rest.filter((x) => x <= pivot)),
      pivot,
      ...sortStrategies.quick(rest.filter((x) => x > pivot)),
    ];
  },
  builtin: (arr) => [...arr].sort((a, b) => a - b),
};

class Sorter {
  #strategy;
  constructor(strategy = "builtin") { this.#strategy = sortStrategies[strategy]; }
  setStrategy(name)   { this.#strategy = sortStrategies[name]; return this; }
  sort(data)          { return this.#strategy(data); }
}

// ── Payment strategy ──────────────────────────────────────────────
const paymentStrategies = {
  creditCard: async ({ amount, cardNumber, cvv }) => {
    return stripeAPI.charge({ amount, card: cardNumber, cvv });
  },
  paypal: async ({ amount, email }) => {
    return paypalAPI.pay({ amount, payerEmail: email });
  },
  crypto: async ({ amount, walletAddress }) => {
    return cryptoAPI.transfer({ amount, to: walletAddress });
  },
};

class Checkout {
  #strategy;
  setPaymentMethod(method) { this.#strategy = paymentStrategies[method]; }
  async pay(details) { return this.#strategy(details); }
}
```

---

### Command

**Intent:** Encapsulate a request as an object — supports undo/redo, queuing, and logging.

```javascript
class CommandHistory {
  #history = [];
  #redoStack = [];

  execute(command) {
    command.execute();
    this.#history.push(command);
    this.#redoStack = [];  // redo stack cleared on new action
    return this;
  }

  undo() {
    const command = this.#history.pop();
    if (!command) return this;
    command.undo();
    this.#redoStack.push(command);
    return this;
  }

  redo() {
    const command = this.#redoStack.pop();
    if (!command) return this;
    command.execute();
    this.#history.push(command);
    return this;
  }
}

// ── Text editor commands ──────────────────────────────────────────
class InsertTextCommand {
  #doc; #position; #text;

  constructor(doc, position, text) {
    this.#doc      = doc;
    this.#position = position;
    this.#text     = text;
  }

  execute() { this.#doc.insert(this.#position, this.#text); }
  undo()    { this.#doc.delete(this.#position, this.#text.length); }
}

class DeleteTextCommand {
  #doc; #position; #length; #deleted;

  constructor(doc, position, length) {
    this.#doc      = doc;
    this.#position = position;
    this.#length   = length;
  }

  execute() {
    this.#deleted = this.#doc.read(this.#position, this.#length);
    this.#doc.delete(this.#position, this.#length);
  }
  undo() { this.#doc.insert(this.#position, this.#deleted); }
}

const history = new CommandHistory();
history
  .execute(new InsertTextCommand(doc, 0, "Hello"))
  .execute(new InsertTextCommand(doc, 5, " World"))
  .execute(new DeleteTextCommand(doc, 5, 6));

history.undo();  // restore " World"
history.undo();  // restore "Hello"
history.redo();  // re-insert "Hello"
```

---

### Chain of Responsibility

**Intent:** Pass a request along a chain of handlers — each decides to process it or pass it on.

```javascript
// ── Express-style middleware chain ────────────────────────────────
class Pipeline {
  #handlers = [];

  use(handler) {
    this.#handlers.push(handler);
    return this;
  }

  async execute(ctx) {
    let index = -1;

    const next = async () => {
      index++;
      const handler = this.#handlers[index];
      if (handler) await handler(ctx, next);
    };

    await next();
    return ctx;
  }
}

// Auth middleware
const authenticate = async (ctx, next) => {
  const token = ctx.headers.authorization?.split(" ")[1];
  if (!token) { ctx.status = 401; return; }
  ctx.user = await verifyToken(token);
  await next();
};

// Rate limit middleware
const rateLimit = (max) => async (ctx, next) => {
  const count = await redis.incr(`rl:${ctx.ip}`);
  if (count === 1) await redis.expire(`rl:${ctx.ip}`, 60);
  if (count > max) { ctx.status = 429; return; }
  await next();
};

// Logging middleware
const logger = async (ctx, next) => {
  const start = Date.now();
  await next();
  console.log(`${ctx.method} ${ctx.path} ${ctx.status} ${Date.now()-start}ms`);
};

const pipeline = new Pipeline()
  .use(logger)
  .use(authenticate)
  .use(rateLimit(100))
  .use(handleRequest);

await pipeline.execute(ctx);
```

---

### State Pattern

**Intent:** Allow an object to alter its behavior when its internal state changes.

```javascript
// ── Traffic Light State Machine ───────────────────────────────────
class TrafficLight {
  #state;

  #states = {
    red: {
      color:    "🔴 Red",
      duration: 3000,
      next:     () => this.#transition("green"),
    },
    green: {
      color:    "🟢 Green",
      duration: 2500,
      next:     () => this.#transition("yellow"),
    },
    yellow: {
      color:    "🟡 Yellow",
      duration: 500,
      next:     () => this.#transition("red"),
    },
  };

  constructor() { this.#state = this.#states.red; }

  #transition(name) {
    this.#state = this.#states[name];
    console.log(this.#state.color);
    setTimeout(() => this.#state.next(), this.#state.duration);
  }

  start() {
    console.log(this.#state.color);
    setTimeout(() => this.#state.next(), this.#state.duration);
  }
}

// ── Async state machine (fetch states) ───────────────────────────
class DataFetcher {
  #state = "idle";

  #transitions = {
    idle:    { fetch: "loading"                                 },
    loading: { success: "success", error: "error"              },
    success: { fetch: "loading", reset: "idle"                 },
    error:   { retry: "loading",  reset: "idle"                },
  };

  transition(event) {
    const next = this.#transitions[this.#state]?.[event];
    if (!next) throw new Error(`Invalid: ${event} in state ${this.#state}`);
    this.#state = next;
    return this;
  }

  get state() { return this.#state; }

  async fetch(url) {
    this.transition("fetch");
    try {
      const data = await fetch(url).then((r) => r.json());
      this.transition("success");
      return data;
    } catch (err) {
      this.transition("error");
      throw err;
    }
  }
}
```

---

## 19.5 Concurrency Patterns

### Publisher-Subscriber (Pub/Sub)

```javascript
// Decoupled from EventEmitter — publisher and subscriber don't know each other

class PubSub {
  #channels = new Map();

  subscribe(channel, subscriber) {
    if (!this.#channels.has(channel)) this.#channels.set(channel, new Set());
    this.#channels.get(channel).add(subscriber);
    return () => this.#channels.get(channel)?.delete(subscriber);
  }

  publish(channel, data) {
    this.#channels.get(channel)?.forEach((fn) => fn(data));
  }
}

const bus = new PubSub();

// Subscribers (don't know who publishes)
bus.subscribe("order:created",  (order) => sendConfirmationEmail(order));
bus.subscribe("order:created",  (order) => updateInventory(order));
bus.subscribe("order:created",  (order) => notifyWarehouse(order));

// Publisher (doesn't know who subscribes)
async function createOrder(items) {
  const order = await db.orders.create(items);
  bus.publish("order:created", order);
  return order;
}
```

---

## 19.6 Patterns Quick Reference

| Pattern                  | Category    | JS Use Case                             | Key Mechanism              |
| ------------------------ | ----------- | --------------------------------------- | -------------------------- |
| Singleton                | Creational  | Config, logger, DB pool                 | Static instance, ES module |
| Factory Method           | Creational  | Notification types, widget library      | `switch` / registry map    |
| Builder                  | Creational  | Query builder, test fixtures            | Fluent interface, `this`   |
| Prototype                | Creational  | Object cloning, instance templates      | `Object.create`, spread    |
| Decorator                | Structural  | Logging, caching, validation wrappers   | HOF wrapping, Proxy        |
| Proxy                    | Structural  | Validation, reactivity, rate limiting   | `new Proxy(target, handler)`|
| Facade                   | Structural  | SDK simplification, multi-service ops   | Wrapper class              |
| Observer / EventEmitter  | Behavioral  | DOM events, store subscriptions         | Listener arrays            |
| Strategy                 | Behavioral  | Sorting, payment methods, auth methods  | Swappable function/object  |
| Command                  | Behavioral  | Undo/redo, task queue, audit log        | Encapsulated execute/undo  |
| Chain of Responsibility  | Behavioral  | Middleware (Express/Koa), validators    | Linked handler chain       |
| State                    | Behavioral  | UI states, async lifecycle, game logic  | State object map           |
| Pub/Sub                  | Concurrency | Cross-module events, microservices      | Decoupled channels         |

---

## 19.7 Interview Questions

### Q1 — What's the difference between Observer and Pub/Sub?

<details>
<summary>Answer</summary>

```
Observer (EventEmitter):
• Subject and observers know each other (direct reference)
• Subject calls observer methods directly
• Tightly coupled — useful when you want tight coordination
• Example: DOM element emitting events to registered handlers

Pub/Sub (Message Bus):
• Publishers and subscribers are completely decoupled
• Communication through a shared channel/topic name
• Publisher doesn't know subscribers exist
• Subscriber doesn't know who publishes
• Useful for cross-module communication, microservices
• Example: Analytics events, domain events across bounded contexts

Key question: "Does the publisher need to know about subscribers?"
• Yes → Observer
• No  → Pub/Sub
```

</details>

---

### Q2 — When would you choose Decorator over inheritance?

<details>
<summary>Answer</summary>

```
Use Decorator when:
1. You need to add behavior at runtime, not compile time
2. You need multiple independent behaviors that can be combined
3. Subclassing would create an explosion of classes

Example: Logging + Caching + Validation
• Inheritance: LoggingCachingValidatingService (one class per combo)
• Decorator: LoggingService(CachingService(ValidationService(base)))
             — mix and match at runtime

Use Inheritance when:
1. The subclass IS-A more specific version of the parent
2. The relationship is permanent and well-defined
3. You need polymorphic substitution (Liskov substitution principle)

JavaScript's prototype chain means all objects already USE the
prototype pattern — decorators are often more pragmatic.
```

</details>

---

### Q3 — Implement a simple Command pattern with undo

<details>
<summary>Answer</summary>

```javascript
const history = [];

function executeCommand(command) {
  command.execute();
  history.push(command);
}

function undo() {
  history.pop()?.undo();
}

// Usage
executeCommand({
  execute: () => console.log("do A"),
  undo:    () => console.log("undo A"),
});
executeCommand({
  execute: () => console.log("do B"),
  undo:    () => console.log("undo B"),
});
undo();  // "undo B"
undo();  // "undo A"
```

</details>

---

> **Module 19 Complete** — Design patterns are a shared vocabulary. The goal is not to apply every pattern, but to know which problem each pattern solves and reach for the right tool at the right time.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 19 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*

---

# Module 20 — Browser APIs

> **"JavaScript's power in the browser comes from the APIs around it, not the language itself."**
> This module covers the modern Browser API surface — from storage to sensors to workers.

---


## 20.1 Browser API Landscape

```
BROWSER API CATEGORIES:
────────────────────────────────────────────────────────────────
  ┌────────────────────────────────────────────────────────────┐
  │  STORAGE                                                   │
  │  localStorage · sessionStorage · IndexedDB · CacheAPI     │
  │  Cookies · sessionStorage · Storage Buckets (Origin)      │
  └────────────────────────────────────────────────────────────┘
  ┌────────────────────────────────────────────────────────────┐
  │  NETWORK                                                   │
  │  Fetch · WebSocket · SSE · WebRTC · Beacon · Background   │
  │  Sync · WebTransport                                       │
  └────────────────────────────────────────────────────────────┘
  ┌────────────────────────────────────────────────────────────┐
  │  THREADING                                                 │
  │  Web Workers · Service Workers · Worklets · SharedWorker  │
  └────────────────────────────────────────────────────────────┘
  ┌────────────────────────────────────────────────────────────┐
  │  OBSERVATION                                               │
  │  IntersectionObserver · MutationObserver                  │
  │  ResizeObserver · PerformanceObserver · ReportingObserver  │
  └────────────────────────────────────────────────────────────┘
  ┌────────────────────────────────────────────────────────────┐
  │  MEDIA & FILES                                             │
  │  File API · FileReader · MediaDevices · Canvas · WebGL    │
  │  MediaStream · WebAudio · Clipboard · Drag & Drop         │
  └────────────────────────────────────────────────────────────┘
  ┌────────────────────────────────────────────────────────────┐
  │  SENSORS & DEVICE                                          │
  │  Geolocation · DeviceOrientation · Battery · Vibration    │
  │  Bluetooth (Web Bluetooth) · USB (WebUSB)                 │
  └────────────────────────────────────────────────────────────┘
```

---

## 20.2 Storage APIs

### localStorage & sessionStorage

```javascript
// ── localStorage (persists across sessions) ───────────────────────
localStorage.setItem("theme", "dark");
localStorage.getItem("theme");           // "dark"
localStorage.removeItem("theme");
localStorage.clear();

// ── sessionStorage (cleared when tab closes) ──────────────────────
sessionStorage.setItem("tempData", "...");

// ── Storing objects (must serialize) ─────────────────────────────
const user = { name: "Alice", prefs: { theme: "dark" } };
localStorage.setItem("user", JSON.stringify(user));
const saved = JSON.parse(localStorage.getItem("user") ?? "null");

// ── Type-safe localStorage wrapper ───────────────────────────────
class TypedStorage {
  constructor(prefix = "") { this.prefix = prefix; }

  #key(k) { return `${this.prefix}${k}`; }

  get(key, defaultValue = null) {
    try {
      const raw = localStorage.getItem(this.#key(key));
      return raw !== null ? JSON.parse(raw) : defaultValue;
    } catch { return defaultValue; }
  }

  set(key, value) {
    localStorage.setItem(this.#key(key), JSON.stringify(value));
  }

  remove(key) { localStorage.removeItem(this.#key(key)); }
  has(key)    { return localStorage.getItem(this.#key(key)) !== null; }
}

const store = new TypedStorage("app:");
store.set("user", { name: "Alice", age: 25 });
store.get("user");                         // { name: "Alice", age: 25 }
store.get("missing", { default: true });   // { default: true }

// ── Storage event (cross-tab sync) ───────────────────────────────
window.addEventListener("storage", (event) => {
  console.log(`Key "${event.key}" changed:`);
  console.log("  old:", event.oldValue);
  console.log("  new:", event.newValue);
  console.log("  url:", event.url);
});
```

### IndexedDB

```javascript
// ── Modern IndexedDB with async/await wrapper ─────────────────────
class IDBStore {
  #dbPromise;

  constructor(dbName, version, onUpgrade) {
    this.#dbPromise = new Promise((resolve, reject) => {
      const req = indexedDB.open(dbName, version);
      req.onupgradeneeded = (e) => onUpgrade(e.target.result, e);
      req.onsuccess       = (e) => resolve(e.target.result);
      req.onerror         = (e) => reject(e.target.error);
    });
  }

  async #tx(storeName, mode, fn) {
    const db   = await this.#dbPromise;
    return new Promise((resolve, reject) => {
      const tx    = db.transaction(storeName, mode);
      const store = tx.objectStore(storeName);
      const req   = fn(store);
      req.onsuccess = (e) => resolve(e.target.result);
      req.onerror   = (e) => reject(e.target.error);
    });
  }

  get(storeName, key) {
    return this.#tx(storeName, "readonly", (s) => s.get(key));
  }

  put(storeName, value) {
    return this.#tx(storeName, "readwrite", (s) => s.put(value));
  }

  delete(storeName, key) {
    return this.#tx(storeName, "readwrite", (s) => s.delete(key));
  }

  async getAll(storeName) {
    return this.#tx(storeName, "readonly", (s) => s.getAll());
  }
}

// Usage
const db = new IDBStore("MyApp", 1, (db) => {
  db.createObjectStore("users", { keyPath: "id", autoIncrement: true });
  db.createObjectStore("posts", { keyPath: "id" });
});

await db.put("users", { id: 1, name: "Alice", age: 25 });
const user = await db.get("users", 1);
```

---

## 20.3 Fetch & Network APIs

```javascript
// ── Full-featured fetch wrapper ───────────────────────────────────
async function request(url, options = {}) {
  const {
    method   = "GET",
    body     = null,
    headers  = {},
    timeout  = 10_000,
    signal   = null,
  } = options;

  const controller = new AbortController();
  const timeoutId  = setTimeout(() => controller.abort(), timeout);

  // Merge external signal with timeout signal
  const combinedSignal = signal
    ? AbortSignal.any([signal, controller.signal])
    : controller.signal;

  try {
    const res = await fetch(url, {
      method,
      headers: { "Content-Type": "application/json", ...headers },
      body:    body ? JSON.stringify(body) : null,
      signal:  combinedSignal,
    });

    clearTimeout(timeoutId);

    if (!res.ok) {
      const error = await res.json().catch(() => ({ message: res.statusText }));
      const err   = new Error(error.message ?? "Request failed");
      err.status  = res.status;
      throw err;
    }

    return res.status === 204 ? null : res.json();
  } catch (err) {
    clearTimeout(timeoutId);
    if (err.name === "AbortError") throw new Error("Request timed out");
    throw err;
  }
}

// ── Beacon API: fire-and-forget analytics ─────────────────────────
// Sends data even when page is unloading (no response)
document.addEventListener("visibilitychange", () => {
  if (document.visibilityState === "hidden") {
    navigator.sendBeacon("/analytics", JSON.stringify({
      event:     "page_exit",
      duration:  Date.now() - pageLoadTime,
      scrolled:  window.scrollY,
    }));
  }
});

// ── Server-Sent Events ────────────────────────────────────────────
// One-way real-time updates from server → client (no WebSocket needed)
const sse = new EventSource("/api/events");

sse.addEventListener("message", (e) => {
  const data = JSON.parse(e.data);
  console.log("Received:", data);
});

sse.addEventListener("notification", (e) => {
  showNotification(JSON.parse(e.data));
});

sse.addEventListener("error", (e) => {
  if (sse.readyState === EventSource.CLOSED) {
    console.log("Connection closed");
  }
});
```

---

## 20.4 Web Workers

**Definition:** Run JavaScript on a background thread — no UI blocking for CPU-intensive work.

```javascript
// ── worker.js (separate file) ─────────────────────────────────────
self.onmessage = function ({ data: { type, payload } }) {
  switch (type) {
    case "SORT":
      const sorted = payload.slice().sort((a, b) => a - b);
      self.postMessage({ type: "SORT_DONE", result: sorted });
      break;

    case "HASH":
      // CPU-intensive without blocking UI
      const hash = computeHash(payload);
      self.postMessage({ type: "HASH_DONE", result: hash });
      break;
  }
};

// ── main.js: communicating with the worker ────────────────────────
const worker = new Worker("worker.js");

// Promise wrapper for cleaner async usage
function workerRequest(worker, type, payload) {
  return new Promise((resolve, reject) => {
    const responseType = `${type}_DONE`;

    function handler({ data }) {
      if (data.type === responseType) {
        worker.removeEventListener("message", handler);
        resolve(data.result);
      } else if (data.type === "ERROR") {
        worker.removeEventListener("message", handler);
        reject(new Error(data.error));
      }
    }

    worker.addEventListener("message", handler);
    worker.postMessage({ type, payload });
  });
}

const sorted = await workerRequest(worker, "SORT", [5, 2, 8, 1, 9]);

// ── Inline Worker (no separate file) ─────────────────────────────
function createInlineWorker(fn) {
  const blob   = new Blob([`(${fn.toString()})()`], { type: "application/javascript" });
  const url    = URL.createObjectURL(blob);
  const worker = new Worker(url);
  URL.revokeObjectURL(url);  // cleanup
  return worker;
}

const worker = createInlineWorker(function () {
  self.onmessage = ({ data }) => {
    const result = data.reduce((sum, n) => sum + n, 0);
    self.postMessage(result);
  };
});

// ── SharedArrayBuffer: zero-copy data sharing ─────────────────────
const sharedBuffer = new SharedArrayBuffer(4 * 1000);  // 1000 floats
const sharedView   = new Float32Array(sharedBuffer);

// Fill in main thread
sharedView.fill(Math.PI);

// Worker reads without copying
worker.postMessage({ buffer: sharedBuffer });  // transferring SAB reference
```

```
WEB WORKER LIMITS:
────────────────────────────────────────────────────────────────
  ✅ Available:  setTimeout, setInterval, fetch, XMLHttpRequest,
                 WebSockets, IndexedDB, Crypto, Console,
                 TypedArrays, Canvas (OffscreenCanvas)

  ❌ NOT available: window, document, DOM, alert/confirm/prompt,
                    localStorage, sessionStorage (use IndexedDB)
```

---

## 20.5 Service Workers & PWA

```javascript
// ── Registering a service worker ─────────────────────────────────
if ("serviceWorker" in navigator) {
  window.addEventListener("load", async () => {
    try {
      const reg = await navigator.serviceWorker.register("/sw.js", {
        scope: "/",
      });
      console.log("SW registered:", reg.scope);
    } catch (err) {
      console.error("SW failed:", err);
    }
  });
}

// ── sw.js: the service worker ─────────────────────────────────────
const CACHE_NAME  = "my-app-v1";
const STATIC_URLS = ["/", "/index.html", "/app.js", "/styles.css"];

// Install: cache static assets
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(STATIC_URLS))
      .then(() => self.skipWaiting())
  );
});

// Activate: clean up old caches
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches.keys().then((names) =>
      Promise.all(
        names.filter((n) => n !== CACHE_NAME).map((n) => caches.delete(n))
      )
    ).then(() => self.clients.claim())
  );
});

// Fetch: intercept network requests
self.addEventListener("fetch", (event) => {
  const { request } = event;

  // API calls: network-first (fresh data, cache fallback)
  if (request.url.includes("/api/")) {
    event.respondWith(
      fetch(request)
        .then((res) => {
          const clone = res.clone();
          caches.open(CACHE_NAME).then((cache) => cache.put(request, clone));
          return res;
        })
        .catch(() => caches.match(request))
    );
    return;
  }

  // Static assets: cache-first
  event.respondWith(
    caches.match(request).then((cached) => cached ?? fetch(request))
  );
});

// ── Push notifications ────────────────────────────────────────────
self.addEventListener("push", (event) => {
  const data = event.data?.json() ?? { title: "New update", body: "App updated" };
  event.waitUntil(
    self.registration.showNotification(data.title, {
      body:  data.body,
      icon:  "/icon-192.png",
      badge: "/badge-72.png",
      data:  { url: data.url },
    })
  );
});

self.addEventListener("notificationclick", (event) => {
  event.notification.close();
  event.waitUntil(clients.openWindow(event.notification.data.url));
});
```

---

## 20.6 Intersection & Mutation Observers

### IntersectionObserver

```javascript
// ── Lazy loading images ───────────────────────────────────────────
const lazyImages = document.querySelectorAll("img[data-src]");

const imageObserver = new IntersectionObserver((entries, observer) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.removeAttribute("data-src");
      observer.unobserve(img);  // stop watching once loaded
    }
  });
}, {
  rootMargin: "200px",    // trigger 200px before entering viewport
  threshold:  0,          // any intersection
});

lazyImages.forEach((img) => imageObserver.observe(img));

// ── Infinite scroll ───────────────────────────────────────────────
const sentinel = document.querySelector(".scroll-sentinel");

const scrollObserver = new IntersectionObserver(async ([entry]) => {
  if (entry.isIntersecting) {
    const nextPage = await loadMoreItems(currentPage++);
    appendItems(nextPage);
  }
}, { threshold: 0.1 });

scrollObserver.observe(sentinel);

// ── Animate on scroll ─────────────────────────────────────────────
const animObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    entry.target.classList.toggle("visible", entry.isIntersecting);
    if (entry.isIntersecting) animObserver.unobserve(entry.target);
  });
}, { threshold: 0.2 });

document.querySelectorAll(".animate-in").forEach((el) => animObserver.observe(el));
```

### MutationObserver & ResizeObserver

```javascript
// ── MutationObserver: watch DOM changes ──────────────────────────
const mutObs = new MutationObserver((mutations) => {
  mutations.forEach(({ type, target, addedNodes, removedNodes, attributeName }) => {
    if (type === "childList") {
      addedNodes.forEach((n) => n.nodeType === 1 && initComponent(n));
      removedNodes.forEach((n) => n.nodeType === 1 && destroyComponent(n));
    }
    if (type === "attributes") {
      console.log(`${target.tagName}.${attributeName} changed`);
    }
  });
});

mutObs.observe(document.body, {
  childList:  true,    // watch for added/removed children
  subtree:    true,    // watch all descendants
  attributes: true,   // watch attribute changes
  attributeFilter: ["data-theme", "aria-expanded"],  // only these attrs
});

mutObs.disconnect();  // stop observing

// ── ResizeObserver: watch element size changes ────────────────────
const resObs = new ResizeObserver((entries) => {
  entries.forEach(({ target, contentRect, borderBoxSize }) => {
    const { width, height } = contentRect;
    console.log(`${target.id}: ${width}×${height}`);

    // Responsive component behavior
    target.classList.toggle("compact", width < 400);
    target.classList.toggle("wide",    width > 800);
  });
});

resObs.observe(document.querySelector(".resizable"));
resObs.unobserve(el);
resObs.disconnect();
```

---

## 20.7 Web Animations API

```javascript
// ── Basic animation ───────────────────────────────────────────────
const el = document.querySelector(".box");

const animation = el.animate(
  [
    { transform: "translateX(0px)",   opacity: 1   },
    { transform: "translateX(200px)", opacity: 0.5, offset: 0.7 },
    { transform: "translateX(300px)", opacity: 0   },
  ],
  {
    duration:   1000,       // ms
    easing:     "ease-out",
    delay:      200,
    iterations: 2,          // Infinity for loop
    direction:  "alternate",
    fill:       "forwards", // keep final state
  }
);

// Control
animation.pause();
animation.play();
animation.reverse();
animation.finish();
animation.cancel();

// Events
animation.addEventListener("finish", () => el.remove());

// ── Await completion ──────────────────────────────────────────────
await animation.finished;
console.log("Animation complete");

// ── Sequence animations ───────────────────────────────────────────
async function fadeInSlideUp(element) {
  // Parallel
  const [fade, slide] = [
    element.animate([{ opacity: 0 }, { opacity: 1 }],        { duration: 400 }),
    element.animate([{ transform: "translateY(20px)" }, { transform: "translateY(0)" }], { duration: 400 }),
  ];
  await Promise.all([fade.finished, slide.finished]);

  // Sequential: bounce after
  await element.animate(
    [{ transform: "scale(1)" }, { transform: "scale(1.05)" }, { transform: "scale(1)" }],
    { duration: 300, easing: "ease-in-out" }
  ).finished;
}

// ── getAnimations: inspect running animations ─────────────────────
document.getAnimations();                    // all animations on page
element.getAnimations({ subtree: true });    // animations on element and descendants
```

---

## 20.8 WebSockets & Server-Sent Events

```javascript
// ── WebSocket client with reconnect ──────────────────────────────
class ReliableWebSocket extends EventTarget {
  #ws;
  #url;
  #reconnectDelay;
  #maxDelay;
  #currentDelay;

  constructor(url, { reconnectDelay = 1000, maxDelay = 30_000 } = {}) {
    super();
    this.#url             = url;
    this.#reconnectDelay  = reconnectDelay;
    this.#maxDelay        = maxDelay;
    this.#currentDelay    = reconnectDelay;
    this.#connect();
  }

  #connect() {
    this.#ws = new WebSocket(this.#url);

    this.#ws.onopen = () => {
      this.#currentDelay = this.#reconnectDelay;  // reset backoff
      this.dispatchEvent(new CustomEvent("open"));
    };

    this.#ws.onmessage = (e) => {
      const data = JSON.parse(e.data);
      this.dispatchEvent(new CustomEvent("message", { detail: data }));
    };

    this.#ws.onclose = (e) => {
      if (!e.wasClean) {
        console.log(`Reconnecting in ${this.#currentDelay}ms...`);
        setTimeout(() => this.#connect(), this.#currentDelay);
        this.#currentDelay = Math.min(this.#currentDelay * 2, this.#maxDelay);
      }
    };

    this.#ws.onerror = (e) => this.dispatchEvent(new CustomEvent("error", { detail: e }));
  }

  send(data)    { this.#ws.send(JSON.stringify(data)); }
  close(code)   { this.#ws.close(code); }
  get readyState() { return this.#ws.readyState; }
}

const ws = new ReliableWebSocket("wss://api.example.com/ws");
ws.addEventListener("message", ({ detail }) => handleMessage(detail));
ws.send({ type: "subscribe", channel: "prices" });

// ── SSE: server-sent events ───────────────────────────────────────
// Server sends:
//   data: {"price": 150.25}\n\n
//   event: alert\ndata: {"msg": "High volume"}\n\n

const sse = new EventSource("/api/prices", { withCredentials: true });

sse.onmessage = (e) => updatePrice(JSON.parse(e.data));
sse.addEventListener("alert", (e) => showAlert(JSON.parse(e.data)));
sse.onerror = () => console.error("SSE error, will auto-reconnect");
```

---

## 20.9 File, Clipboard & Media APIs

```javascript
// ── File API ─────────────────────────────────────────────────────
const input = document.querySelector("input[type=file]");

input.addEventListener("change", async ({ target }) => {
  for (const file of target.files) {
    console.log(`Name: ${file.name}, Size: ${file.size}, Type: ${file.type}`);

    // Read as text
    const text = await file.text();

    // Read as ArrayBuffer (binary)
    const buffer = await file.arrayBuffer();

    // Create object URL for preview
    const url = URL.createObjectURL(file);
    document.querySelector("img").src = url;
    // Clean up when done:
    URL.revokeObjectURL(url);
  }
});

// ── Drag and drop ─────────────────────────────────────────────────
const dropZone = document.querySelector(".drop-zone");

dropZone.addEventListener("dragover",  (e) => { e.preventDefault(); dropZone.classList.add("over"); });
dropZone.addEventListener("dragleave", ()  => dropZone.classList.remove("over"));
dropZone.addEventListener("drop",      async (e) => {
  e.preventDefault();
  dropZone.classList.remove("over");

  const files = [...e.dataTransfer.files];
  for (const file of files) await processFile(file);
});

// ── Clipboard API ─────────────────────────────────────────────────
// Read
const text = await navigator.clipboard.readText();

// Write
await navigator.clipboard.writeText("Hello, clipboard!");

// Read image
const [item] = await navigator.clipboard.read();
if (item.types.includes("image/png")) {
  const blob = await item.getType("image/png");
  const img  = new Image();
  img.src = URL.createObjectURL(blob);
}

// ── MediaDevices: camera & microphone ─────────────────────────────
async function startCamera() {
  const stream = await navigator.mediaDevices.getUserMedia({
    video: { width: 1920, height: 1080, facingMode: "environment" },
    audio: true,
  });

  const video = document.querySelector("video");
  video.srcObject = stream;
  await video.play();

  return stream;  // keep reference to stop later
}

function stopCamera(stream) {
  stream.getTracks().forEach((t) => t.stop());
}

// Capture photo from video stream
function capturePhoto(video) {
  const canvas  = document.createElement("canvas");
  canvas.width  = video.videoWidth;
  canvas.height = video.videoHeight;
  canvas.getContext("2d").drawImage(video, 0, 0);
  return canvas.toDataURL("image/jpeg", 0.9);
}
```

---

## 20.10 Performance APIs

```javascript
// ── Navigation Timing ─────────────────────────────────────────────
const [navEntry] = performance.getEntriesByType("navigation");
console.log({
  DNS:        navEntry.domainLookupEnd - navEntry.domainLookupStart,
  TCP:        navEntry.connectEnd - navEntry.connectStart,
  TTFB:       navEntry.responseStart - navEntry.requestStart,
  download:   navEntry.responseEnd - navEntry.responseStart,
  DOMParsing: navEntry.domContentLoadedEventEnd - navEntry.responseEnd,
  loadEvent:  navEntry.loadEventEnd - navEntry.loadEventStart,
  total:      navEntry.loadEventEnd - navEntry.startTime,
});

// ── Core Web Vitals ───────────────────────────────────────────────
// LCP, FID, CLS — measure with PerformanceObserver

// Largest Contentful Paint (LCP)
new PerformanceObserver((list) => {
  const entries = list.getEntries();
  const lcp     = entries[entries.length - 1];  // last = latest LCP candidate
  console.log("LCP:", lcp.startTime.toFixed(0), "ms");
}).observe({ type: "largest-contentful-paint", buffered: true });

// Cumulative Layout Shift (CLS)
let cls = 0;
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    if (!entry.hadRecentInput) cls += entry.value;
  }
  console.log("CLS:", cls.toFixed(4));
}).observe({ type: "layout-shift", buffered: true });

// First Input Delay / Interaction to Next Paint (INP)
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log("Interaction:", entry.name, entry.duration.toFixed(0), "ms");
  }
}).observe({ type: "event", durationThreshold: 16, buffered: true });

// ── Custom marks & measures ───────────────────────────────────────
performance.mark("render-start");
renderHeavyComponent();
performance.mark("render-end");
performance.measure("render-time", "render-start", "render-end");

const [measure] = performance.getEntriesByName("render-time");
console.log(`Render took: ${measure.duration.toFixed(2)}ms`);

// ── Resource Timing ───────────────────────────────────────────────
performance.getEntriesByType("resource").forEach(({ name, duration, transferSize }) => {
  console.log(`${name}: ${duration.toFixed(0)}ms, ${(transferSize / 1024).toFixed(1)}KB`);
});
```

---

## 20.11 Permissions & Sensors

```javascript
// ── Permissions API ───────────────────────────────────────────────
async function checkPermission(name) {
  const status = await navigator.permissions.query({ name });
  console.log(`${name}: ${status.state}`);  // "granted" | "denied" | "prompt"

  status.addEventListener("change", () => {
    console.log(`${name} changed to: ${status.state}`);
  });

  return status.state;
}

await checkPermission("geolocation");
await checkPermission("notifications");
await checkPermission("clipboard-read");

// ── Geolocation ───────────────────────────────────────────────────
function getPosition() {
  return new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(resolve, reject, {
      enableHighAccuracy: true,
      timeout:            5000,
      maximumAge:         0,
    });
  });
}

const { coords: { latitude, longitude, accuracy } } = await getPosition();

// Watch for changes
const watchId = navigator.geolocation.watchPosition(
  ({ coords }) => updateMap(coords),
  (err) => console.error(err),
  { enableHighAccuracy: true }
);
navigator.geolocation.clearWatch(watchId);  // stop watching

// ── Battery API ───────────────────────────────────────────────────
const battery = await navigator.getBattery?.();
if (battery) {
  console.log(`Battery: ${(battery.level * 100).toFixed(0)}%`);
  console.log(`Charging: ${battery.charging}`);
  console.log(`Time to full: ${battery.chargingTime}s`);

  battery.addEventListener("levelchange",   () => adjustPerformance(battery.level));
  battery.addEventListener("chargingchange", () => updateUI(battery.charging));
}

// ── Device orientation ────────────────────────────────────────────
window.addEventListener("deviceorientation", ({ alpha, beta, gamma }) => {
  // alpha: rotation around z (compass)  0-360
  // beta:  rotation around x (tilt)    -180 to 180
  // gamma: rotation around y (tilt)     -90 to 90
  rotateCube(alpha, beta, gamma);
});

// ── Vibration ─────────────────────────────────────────────────────
navigator.vibrate?.(200);              // 200ms
navigator.vibrate?.([200, 100, 200]);  // on/off/on pattern
navigator.vibrate?.(0);                // stop
```

---

## 20.12 Interview Questions

### Q1 — When would you use Web Workers?

<details>
<summary>Answer</summary>

```
Use Web Workers when an operation:
1. Takes > 16ms (would block a 60fps frame)
2. Is CPU-intensive: sorting large datasets, image processing,
   cryptography, parsing large JSON, complex calculations

Examples:
• Sorting/searching 100k+ items
• Real-time audio processing
• Video frame analysis
• Compressing/decompressing data
• Generating PDF/Excel files client-side
• Complex physics simulations

Don't use for:
• Simple async operations (use fetch/Promises)
• DOM manipulation (workers can't touch the DOM)
• Operations < 1ms (worker overhead outweighs benefit)

Communication: postMessage() / onmessage (structured clone algorithm)
Shared memory: SharedArrayBuffer + Atomics (careful with synchronization)
```

</details>

---

### Q2 — What is a Service Worker and how does it enable offline functionality?

<details>
<summary>Answer</summary>

```
A Service Worker is a JavaScript file that runs in a background thread,
separate from the page, acting as a programmable network proxy.

It intercepts fetch() requests and can:
1. Serve cached responses (offline support)
2. Cache new responses for future offline use
3. Implement caching strategies:
   • Cache-first:   serve cache, fall back to network
   • Network-first: try network, fall back to cache
   • Stale-while-revalidate: serve cache immediately, refresh in background

Lifecycle:
1. Register: navigator.serviceWorker.register("/sw.js")
2. Install:  cache static assets
3. Activate: clean old caches, take control of clients
4. Fetch:    intercept all network requests

PWA requirements:
• HTTPS (or localhost)
• Service worker
• Web App Manifest (name, icons, theme_color, display: "standalone")
• Start URL must be cacheable
```

</details>

---

### Q3 — What's the difference between localStorage, sessionStorage, and IndexedDB?

<details>
<summary>Answer</summary>

```
localStorage:
• Persists across sessions (until cleared)
• Scope: origin (protocol + domain + port)
• Size: ~5-10 MB
• Sync API (blocks main thread)
• Strings only (must JSON.stringify objects)
• Use for: preferences, tokens, small settings

sessionStorage:
• Cleared when tab closes
• Scope: per tab (even same origin)
• Same size/API as localStorage
• Use for: temporary form data, wizard state, tab-specific data

IndexedDB:
• Persists across sessions
• Size: up to ~50% of disk space
• Async API (non-blocking)
• Stores structured data (objects, ArrayBuffers, Blobs)
• Indexes for fast queries
• Transactions for data integrity
• Use for: offline app data, large datasets, caching API responses,
           files/images, anything > 5MB

Cookies:
• Sent with every HTTP request (unlike the others)
• Small size (~4KB)
• Can be HTTP-only (inaccessible to JS — secure for tokens)
• Use for: session tokens, server-shared state
```

</details>

---

> **Module 20 Complete** — Browser APIs are what transform JavaScript from a language into a platform. The web has sensors, workers, storage, streams, animations, and push notifications — all accessible without any install.

---

*JavaScript Developer Handbook — Core to Advanced*
*Module 20 of 20 · Difficulty: 🔴 Advanced*
*GitHub-ready · Last updated 2025*