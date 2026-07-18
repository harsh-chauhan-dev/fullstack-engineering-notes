# Modules in Node.js

A **Module** is a reusable piece of code that encapsulates related functionality.

Instead of writing all your code in a single file, you divide it into multiple files (modules). This makes your code easier to organize, maintain, and reuse.

> **Simple Definition**
>
> A module is simply a JavaScript file that contains code which can be exported and imported into other files.

---

# Why Do We Need Modules?

Imagine building an E-Commerce application.

Without modules:

```text
app.js

✔ Authentication
✔ Products
✔ Orders
✔ Payments
✔ Database
✔ Email
✔ Everything...
```

One huge file becomes difficult to maintain.

---

With modules:

```text
project/

app.js

auth.js
product.js
order.js
payment.js
database.js
email.js
```

Each file has a single responsibility.

Benefits:

- Better organization
- Easy maintenance
- Code reusability
- Team collaboration
- Easier debugging

---

# Types of Modules in Node.js

Node.js supports three types of modules.

## 1. Core Modules

These are built into Node.js.

No installation is required.

Examples

```text
fs
http
path
os
crypto
events
stream
url
```

Example

```js
const fs = require("fs");

fs.readFile("data.txt", "utf8", (err, data) => {
    console.log(data);
});
```

---

## 2. Local Modules

Modules created by the developer.

Project Structure

```text
project/

app.js
math.js
```

math.js

```js
function add(a, b) {
    return a + b;
}

module.exports = add;
```

app.js

```js
const add = require("./math");

console.log(add(5, 3));
```

Output

```text
8
```

---

## 3. Third-Party Modules

Modules installed using npm.

Example

```bash
npm install express
```

Usage

```js
const express = require("express");

const app = express();
```

Examples

- express
- mongoose
- axios
- dotenv
- bcrypt
- jsonwebtoken

---

# CommonJS Module System

Node.js originally uses the **CommonJS** module system.

Important keywords:

```text
require()

module.exports

exports
```

---

# require()

`require()` imports another module.

Syntax

```js
const moduleName = require("module-name");
```

Example

```js
const path = require("path");
```

Local Module

```js
const math = require("./math");
```

---

# module.exports

Used to export data from a module.

math.js

```js
function add(a, b) {
    return a + b;
}

module.exports = add;
```

Import

```js
const add = require("./math");

console.log(add(10, 20));
```

Output

```text
30
```

---

# Exporting Multiple Values

math.js

```js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

app.js

```js
const math = require("./math");

console.log(math.add(5, 2));

console.log(math.subtract(5, 2));
```

Output

```text
7
3
```

---

# exports Shortcut

Instead of

```js
module.exports.add = add;
```

You can write

```js
exports.add = add;
```

Example

```js
exports.add = (a, b) => a + b;

exports.multiply = (a, b) => a * b;
```

Import

```js
const math = require("./math");

console.log(math.add(2, 5));
```

---

# Difference Between module.exports and exports

Both are used for exporting.

Example

```js
exports.name = "Harsh";
```

works.

But

```js
exports = {};
```

does **not** work because it breaks the reference.

Correct

```js
module.exports = {};
```

### Rule

✅ Use `module.exports` when exporting a single value.

✅ Use `exports` when adding multiple properties.

---

# ES Modules (ESM)

Modern JavaScript uses **ES Modules**.

Keywords

```text
import

export
```

Example

math.js

```js
export function add(a, b) {
    return a + b;
}
```

app.js

```js
import { add } from "./math.js";

console.log(add(5, 3));
```

---

# Enabling ES Modules

Method 1

package.json

```json
{
  "type": "module"
}
```

Method 2

Use

```text
.mjs
```

extension.

---

# Default Export

math.js

```js
export default function add(a, b) {
    return a + b;
}
```

Import

```js
import add from "./math.js";
```

---

# Named Export

math.js

```js
export function add() {}

export function subtract() {}
```

Import

```js
import { add, subtract } from "./math.js";
```

---

# Module Resolution

When Node.js sees

```js
require("./math");
```

It searches in this order:

```text
math.js

math.json

math.node

math/index.js
```

For package imports:

```js
require("express")
```

Node.js searches:

```text
node_modules/

↓

package.json

↓

main

↓

index.js
```

---

# Module Caching

Node.js loads a module only **once**.

Future calls to `require()` return the cached version.

Example

math.js

```js
console.log("Loaded");

module.exports = {};
```

app.js

```js
require("./math");

require("./math");

require("./math");
```

Output

```text
Loaded
```

The module is executed only once.

---

# Built-in Module Examples

## File System

```js
const fs = require("fs");
```

---

## HTTP

```js
const http = require("http");
```

---

## Path

```js
const path = require("path");
```

---

## OS

```js
const os = require("os");
```

---

## Crypto

```js
const crypto = require("crypto");
```

---

# Common Mistakes

❌ Forgetting `./`

```js
require("math")
```

Node.js searches inside `node_modules`.

Correct

```js
require("./math")
```

---

❌ Using `exports = {}`

Wrong

```js
exports = {
    add
};
```

Correct

```js
module.exports = {
    add
};
```

---

❌ Mixing CommonJS and ES Modules incorrectly.

Choose one module system per project unless you understand interoperability.

---

# CommonJS vs ES Modules

| CommonJS | ES Modules |
|-----------|------------|
| `require()` | `import` |
| `module.exports` | `export` |
| Synchronous loading | Static imports |
| Default in older Node.js | Standard JavaScript module system |
| `.js` (default) | `.js` with `"type": "module"` or `.mjs` |

---

# Interview Questions

## 1. What is a module in Node.js?

A module is a reusable JavaScript file that encapsulates related functionality and can be imported into other files.

---

## 2. What are the different types of modules?

- Core Modules
- Local Modules
- Third-Party Modules

---

## 3. What is `require()`?

`require()` is a CommonJS function used to import modules.

---

## 4. What is `module.exports`?

It exports values from a module so they can be used in other files.

---

## 5. What is the difference between `exports` and `module.exports`?

`exports` is a shortcut to `module.exports`. Reassigning `exports` breaks that link, while assigning to `module.exports` changes what the module exports.

---

## 6. What is module caching?

When a module is first loaded, Node.js caches it. Subsequent `require()` calls return the cached module instead of reloading it.

---

## 7. What is the difference between CommonJS and ES Modules?

CommonJS uses `require()` and `module.exports`, while ES Modules use `import` and `export`.

---

## 8. Why do we use modules?

- Organize code
- Reuse functionality
- Improve maintainability
- Enable team collaboration
- Reduce code duplication

---

# Summary

- A module is a reusable JavaScript file.
- Node.js supports **Core**, **Local**, and **Third-Party** modules.
- CommonJS uses `require()` and `module.exports`.
- ES Modules use `import` and `export`.
- Node.js caches modules after the first load.
- Using modules makes applications scalable, maintainable, and easier to develop.