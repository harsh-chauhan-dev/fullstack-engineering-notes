# Node.js Architecture

Node.js follows a **Single-Threaded, Event-Driven, Non-Blocking I/O Architecture**. It is designed to efficiently handle thousands of concurrent requests without creating a new thread for every client.

---

# Architecture Overview

```text
                    Client Requests
                           │
                           ▼
                  +-----------------+
                  |   Event Queue   |
                  +-----------------+
                           │
                           ▼
                  +-----------------+
                  |   Event Loop    |
                  +-----------------+
                    │            │
         Synchronous│            │Asynchronous
            Code    │            ▼
                    │      +-------------+
                    │      |    libuv    |
                    │      +-------------+
                    │            │
                    │            ▼
                    │    Thread Pool / OS
                    │            │
                    └────────────┤
                                 ▼
                        Callback Queue
                                 │
                                 ▼
                           Event Loop
                                 │
                                 ▼
                             Response
```

---

# Components of Node.js Architecture

## 1. Client

A client sends an HTTP request to the Node.js server.

Examples:

- Browser
- Mobile Application
- Postman
- Frontend Application

Example Request

```http
GET /users
```

---

## 2. Event Queue

Every incoming request first enters the Event Queue.

Example

```text
Request 1
Request 2
Request 3
Request 4
```

The Event Loop continuously checks this queue and processes incoming requests.

---

## 3. Event Loop

The **Event Loop** is the heart of Node.js.

Its responsibilities include:

- Receiving requests
- Executing synchronous code
- Delegating asynchronous tasks
- Running callback functions after tasks complete

Conceptually, it behaves like:

```text
while (true) {
    Check Call Stack
    Check Callback Queue
    Execute Pending Callback
}
```

> **Note:** This is only a conceptual representation. The actual Event Loop implementation is more complex.

---

## 4. Call Stack

The Call Stack stores the functions currently being executed.

Example

```js
function greet() {
    sayHello();
}

function sayHello() {
    console.log("Hello");
}

greet();
```

Call Stack

```text
sayHello()
greet()
Global()
```

Functions are removed from the stack once execution is complete.

---

## 5. libuv

Node.js uses **libuv**, a C library that powers asynchronous operations.

libuv is responsible for:

- Event Loop
- File System Operations
- DNS Lookup
- TCP Connections
- Timers
- Thread Pool

Without libuv, Node.js could not efficiently handle asynchronous tasks.

---

## 6. Thread Pool

Although JavaScript runs on a single thread, libuv maintains a **Thread Pool** (4 threads by default).

Tasks handled by the Thread Pool include:

- Reading files
- Writing files
- Password hashing (bcrypt)
- Compression
- DNS lookup

Example

```js
const fs = require("fs");

fs.readFile("notes.txt", () => {
    console.log("File Read Successfully");
});
```

The file is read by a worker thread while the main thread continues executing other code.

---

## 7. Operating System

The operating system performs low-level operations such as:

- File access
- Database communication
- Network communication

After completing the task, it notifies libuv.

---

## 8. Callback Queue

Once an asynchronous operation finishes, its callback function is placed inside the Callback Queue.

Example

```js
fs.readFile("data.txt", () => {
    console.log("Done");
});
```

The callback waits until the Event Loop moves it to the Call Stack.

---

# Complete Request Flow

```text
Client
   │
   ▼
Event Queue
   │
   ▼
Event Loop
   │
   ├──────────────► Synchronous Code
   │
   └──────────────► Asynchronous Task
                        │
                        ▼
                      libuv
                        │
                        ▼
             Thread Pool / Operating System
                        │
                        ▼
                 Callback Queue
                        │
                        ▼
                   Event Loop
                        │
                        ▼
                  Execute Callback
                        │
                        ▼
                  Send Response
```

---

# Example

```js
const fs = require("fs");

console.log("Start");

fs.readFile("file.txt", () => {
    console.log("Reading Completed");
});

console.log("End");
```

Output

```bash
Start
End
Reading Completed
```

### Execution Flow

1. `Start` is printed.
2. `readFile()` is delegated to libuv.
3. `End` is printed immediately.
4. File reading happens in the background.
5. Callback is added to the Callback Queue.
6. Event Loop executes the callback.
7. `Reading Completed` is printed.

---

# Why is Node.js Fast?

Node.js is fast because it combines:

- V8 Engine compiles JavaScript into machine code.
- Event Loop handles asynchronous tasks efficiently.
- Non-Blocking I/O prevents waiting.
- libuv manages background operations.
- Thread Pool executes expensive I/O tasks.

---

# Advantages of Node.js Architecture

- High Performance
- Handles thousands of concurrent requests
- Non-Blocking I/O
- Low Memory Usage
- Event-Driven Design
- Scalable Applications
- Lightweight Server

---

# Limitations

- Not suitable for CPU-intensive tasks.
- Heavy synchronous code blocks the Event Loop.
- Single JavaScript thread means long-running computations affect all requests.
- Complex asynchronous logic can become difficult to manage without Promises or async/await.

---

# Real-World Applications

Node.js architecture is ideal for:

- REST APIs
- Chat Applications
- Streaming Platforms
- Online Gaming Servers
- Real-Time Dashboards
- Microservices
- WebSocket Applications

---

# Interview Questions

## 1. What is Node.js architecture?

Node.js follows a **Single-Threaded, Event-Driven, Non-Blocking I/O Architecture** powered by the Event Loop and libuv.

---

## 2. Why is Node.js called single-threaded?

JavaScript executes on a single main thread, while asynchronous operations are delegated to libuv and the operating system.

---

## 3. What is the Event Loop?

The Event Loop continuously monitors the Call Stack and Callback Queue, executing callbacks when the Call Stack becomes empty.

---

## 4. What is libuv?

libuv is a C library that provides:

- Event Loop
- Asynchronous I/O
- Thread Pool
- Cross-platform support

---

## 5. What is the Thread Pool?

The Thread Pool is a group of worker threads managed by libuv that execute expensive asynchronous tasks like file operations and password hashing.

---

## 6. What is the difference between the Call Stack and Callback Queue?

| Call Stack | Callback Queue |
|------------|----------------|
| Executes current functions | Stores completed asynchronous callbacks |
| LIFO (Last In, First Out) | FIFO (First In, First Out) |
| Handles synchronous code | Handles asynchronous callbacks |

---

## 7. Does Node.js create a new thread for every request?

No. Node.js uses a single JavaScript thread and delegates asynchronous work to libuv, allowing it to efficiently serve many concurrent requests.

---

## 8. Why is Node.js suitable for I/O-intensive applications?

Because it uses non-blocking I/O and an Event Loop, enabling it to handle many file, network, and database operations concurrently without blocking the main thread.

---

# Summary

- Node.js uses a **Single-Threaded** execution model.
- The **Event Loop** manages request processing.
- **libuv** provides asynchronous I/O and the Thread Pool.
- The **Call Stack** executes synchronous code.
- The **Callback Queue** stores completed asynchronous callbacks.
- This architecture makes Node.js highly efficient for **I/O-bound and real-time applications**.