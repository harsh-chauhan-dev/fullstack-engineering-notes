# Node.js

Node.js is a **JavaScript runtime environment** that allows you to run JavaScript outside the browser.

It is mainly used for building:

- Backend applications
- REST APIs
- Web servers
- Real-time applications
- CLI tools
- Microservices

---

## Why Node.js?

Before Node.js, JavaScript could only run inside browsers.

Node.js allows JavaScript to run on:

- Your computer
- Servers
- Cloud environments

This means you can build both the frontend and backend using JavaScript.

---

# How Node.js Works

## 1. Built on Google's V8 Engine

Node.js uses Google's **V8 JavaScript Engine**, which is also used in Chrome.

### What does V8 do?

- Converts JavaScript into machine code.
- Executes code very quickly.
- Improves application performance.

```
JavaScript
      │
      ▼
V8 Engine
      │
      ▼
Machine Code
      │
      ▼
CPU Executes
```

---

## 2. Event-Driven Architecture

Node.js follows an **Event-Driven Architecture**.

Instead of constantly checking for work, Node.js waits for events.

When an event occurs, it executes the corresponding callback function.

Example:

```js
const http = require("http");

const server = http.createServer((req, res) => {
    res.end("Hello from Node.js");
});

server.listen(3000);
```

Whenever a request arrives:

```
Client Request
       │
       ▼
Event Triggered
       │
       ▼
Callback Function Executes
       │
       ▼
Response Sent
```

---

## 3. Non-Blocking (Asynchronous) I/O

This is one of the biggest reasons Node.js is fast.

### Blocking

```
Task 1
   ↓
Wait
   ↓
Task 2
```

Every task waits for the previous task.

---

### Non-Blocking

```
Task 1 --------\
                \
Task 2 ----------> Continue Execution
                 /
Task 3 ---------/
```

Node.js starts long-running operations and continues executing other code without waiting.

Example:

```js
const fs = require("fs");

fs.readFile("file.txt", "utf8", () => {
    console.log("Reading file completed.");
});

console.log("Next Task");
```

Output

```bash
Next Task
Reading file completed.
```

Node.js doesn't wait for the file to finish reading.

---

## 4. Single Thread + Event Loop

Node.js uses a **single main thread**.

Instead of creating a new thread for every request, it uses an **Event Loop**.

The Event Loop manages asynchronous operations efficiently.

```
Client Request
        │
        ▼
    Event Loop
        │
        ▼
Asynchronous Task
        │
        ▼
Callback Queue
        │
        ▼
Execute Callback
        │
        ▼
Response
```

This allows Node.js to handle thousands of concurrent requests efficiently.

---

# Advantages of Node.js

- Fast execution using the V8 Engine
- Non-blocking architecture
- Event-driven programming model
- Lightweight
- Large NPM ecosystem
- JavaScript on both frontend and backend
- Great for APIs and real-time applications

---

# Limitations of Node.js

- Not ideal for CPU-intensive tasks
- Single thread can become blocked by heavy computations
- Callback nesting can make code harder to read (mitigated with Promises and async/await)

---

# Where is Node.js Used?

- REST APIs
- Chat applications
- Streaming services
- Real-time dashboards
- Online games
- CLI tools
- Microservices

---

# Interview Questions

### 1. What is Node.js?

Node.js is a JavaScript runtime environment built on Google's V8 engine that allows JavaScript to run outside the browser.

---

### 2. Is Node.js a programming language?

No.

It is a runtime environment.

---

### 3. Is Node.js single-threaded?

Yes.

JavaScript execution is single-threaded, but Node.js uses the Event Loop and worker threads (internally where applicable) to handle asynchronous operations efficiently.

---

### 4. Why is Node.js fast?

Because it uses:

- Google's V8 Engine
- Non-blocking I/O
- Event Loop
- Event-driven architecture

---

### 5. What is the Event Loop?

The Event Loop continuously checks whether asynchronous tasks have completed. When they do, it executes their callback functions.

---

### 6. What is Non-Blocking I/O?

Node.js starts an I/O operation and continues executing other code instead of waiting for it to finish.

---

### 7. What is the difference between Blocking and Non-Blocking?

| Blocking | Non-Blocking |
|----------|--------------|
| Waits for task completion | Doesn't wait |
| Slower | Faster |
| Sequential execution | Concurrent handling of I/O |

---

### 8. Can Node.js handle multiple users simultaneously?

Yes.

Using asynchronous I/O and the Event Loop, Node.js can efficiently handle many concurrent client connections.

---

### 9. What is V8?

V8 is Google's JavaScript engine that compiles JavaScript into machine code for fast execution.

---

### 10. When should you use Node.js?

Use Node.js for:

- REST APIs
- Real-time applications
- Streaming
- Chat applications
- Microservices

Avoid it for heavy CPU-bound tasks such as video rendering or large scientific computations.

---

# Key Takeaways

- Node.js is a JavaScript runtime.
- Built on Google's V8 Engine.
- Uses an Event-Driven Architecture.
- Uses Non-Blocking I/O.
- Uses a Single Thread with an Event Loop.
- Best suited for I/O-intensive applications.