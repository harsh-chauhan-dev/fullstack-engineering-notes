# 🌐 REST Architecture

## 📖 Overview

**REST (Representational State Transfer)** is an architectural style used to design scalable and maintainable web services. It defines a set of principles for communication between clients and servers using the HTTP protocol.

Most modern applications built with Node.js, Express.js, React, Django, Spring Boot, Laravel, and ASP.NET use REST APIs to exchange data.

---

## 🎯 Learning Objectives

After completing this topic, you should understand:

* What REST is
* Why REST is used
* REST Constraints
* Resources and Endpoints
* HTTP Methods in REST
* CRUD Operations
* RESTful API Design
* HTTP Status Codes
* REST in Express.js

---

# What is REST?

REST stands for **Representational State Transfer**.

It is an architectural style introduced by **Roy Fielding** in his doctoral dissertation in 2000.

REST treats everything as a **Resource** and uses standard HTTP methods to perform operations on those resources.

### Example Resources

```text
/users
/products
/orders
/notes
```

Each URL represents a resource that can be accessed or modified through HTTP requests.

---

# Why REST?

Without REST, every developer could design APIs differently.

### Non-RESTful Example

```text
/getUsers
/createUser
/updateUser
/deleteUser
```

### RESTful Example

```text
GET    /users
POST   /users
PATCH  /users/:id
DELETE /users/:id
```

REST provides a standard structure that improves consistency and maintainability.

---

# REST Architecture

```text
Client
   ↓
HTTP Request
   ↓
REST API
   ↓
Database
   ↓
HTTP Response
   ↓
Client
```

### Example

```text
React Application
        ↓
GET /users
        ↓
Express API
        ↓
PostgreSQL
        ↓
JSON Response
```

---

# REST Constraints

A RESTful system follows six architectural constraints.

---

## 1. Client-Server Architecture

The client and server should be independent.

### Client

* React
* Angular
* Mobile Apps
* Web Browsers

### Server

* Node.js
* Express.js
* Django
* Spring Boot

### Benefits

* Independent development
* Better scalability
* Easier maintenance

---

## 2. Stateless

Every request must contain all information needed for processing.

The server should not store information about previous requests.

### Example

```http
GET /profile
Authorization: Bearer JWT_TOKEN
```

Each request contains authentication information.

### Benefits

* Easier scaling
* Better performance
* Simplified load balancing

---

## 3. Cacheable

Responses should indicate whether they can be cached.

### Example

```http
Cache-Control: max-age=3600
```

The client can reuse cached responses instead of repeatedly contacting the server.

### Benefits

* Faster responses
* Reduced server load
* Improved performance

---

## 4. Uniform Interface

REST APIs should follow a consistent structure.

### Resource-Based URLs

Good:

```text
/users
/users/1
/products
/products/5
```

Bad:

```text
/getUser
/createProduct
/updateUser
```

---

### Use HTTP Methods

#### GET

Retrieve data.

```http
GET /users
```

#### POST

Create data.

```http
POST /users
```

#### PUT

Replace an entire resource.

```http
PUT /users/1
```

#### PATCH

Update specific fields.

```http
PATCH /users/1
```

#### DELETE

Remove data.

```http
DELETE /users/1
```

---

## 5. Layered System

The client should not know the internal architecture of the application.

### Example

```text
Client
  ↓
Load Balancer
  ↓
API Gateway
  ↓
Application Server
  ↓
Database
```

The client only interacts with the API endpoint.

### Benefits

* Better security
* Improved scalability
* Flexible architecture

---

## 6. Code on Demand (Optional)

The server may send executable code to the client.

Example:

```javascript
console.log("Hello World");
```

This constraint is optional and rarely used in modern APIs.

---

# Resources in REST

A resource represents any data managed by the application.

### Examples

```text
/users
/products
/orders
/notes
```

### Single Resource

```text
/users/5
```

Represents a specific user.

### Collection Resource

```text
/users
```

Represents all users.

---

# CRUD Operations in REST

CRUD stands for:

* Create
* Read
* Update
* Delete

| Operation | HTTP Method | Endpoint   |
| --------- | ----------- | ---------- |
| Create    | POST        | /users     |
| Read All  | GET         | /users     |
| Read One  | GET         | /users/:id |
| Update    | PATCH / PUT | /users/:id |
| Delete    | DELETE      | /users/:id |

---

# Example: Notes API

## Create Note

```http
POST /notes
```

Request Body:

```json
{
  "title": "REST Notes",
  "content": "Learning REST Architecture"
}
```

---

## Get All Notes

```http
GET /notes
```

---

## Get Single Note

```http
GET /notes/10
```

---

## Update Note

```http
PATCH /notes/10
```

Request Body:

```json
{
  "title": "Updated Title"
}
```

---

## Delete Note

```http
DELETE /notes/10
```

---

# HTTP Status Codes

REST APIs use HTTP status codes to indicate the result of a request.

## Success Responses

| Code | Meaning    |
| ---- | ---------- |
| 200  | OK         |
| 201  | Created    |
| 204  | No Content |

---

## Client Errors

| Code | Meaning      |
| ---- | ------------ |
| 400  | Bad Request  |
| 401  | Unauthorized |
| 403  | Forbidden    |
| 404  | Not Found    |

---

## Server Errors

| Code | Meaning               |
| ---- | --------------------- |
| 500  | Internal Server Error |

---

# REST Response Example

Request:

```http
GET /users/1
```

Response:

```json
{
  "id": 1,
  "name": "Harsh",
  "email": "harsh@example.com"
}
```

---

# REST in Express.js

Example REST API Routes:

```javascript
const express = require("express");
const app = express();

app.get("/users", getUsers);

app.get("/users/:id", getUserById);

app.post("/users", createUser);

app.patch("/users/:id", updateUser);

app.delete("/users/:id", deleteUser);
```

This follows RESTful conventions and is commonly used in production applications.

---

# REST vs Non-REST API Design

## Non-RESTful

```text
/getUsers
/createUser
/updateUser
/deleteUser
```

## RESTful

```text
GET    /users
POST   /users
PATCH  /users/:id
DELETE /users/:id
```

RESTful APIs are easier to understand, maintain, and scale.

---

# REST in the PERN Stack

```text
React Frontend
      ↓
REST API (Express.js)
      ↓
PostgreSQL Database
```

Example:

```javascript
await fetch("/api/notes");
```

↓

```http
GET /api/notes
```

↓

```json
[
  {
    "id": 1,
    "title": "REST Notes"
  }
]
```

---

# Key Takeaways

* REST stands for Representational State Transfer.
* REST is an architectural style for designing web APIs.
* Everything in REST is treated as a resource.
* Resources are identified using URLs.
* HTTP methods define actions on resources.
* REST APIs should be stateless.
* CRUD operations map directly to HTTP methods.
* REST improves scalability, maintainability, and consistency.
* REST is the foundation of modern backend development.

---

## 📌 Summary

REST Architecture provides a standardized way for clients and servers to communicate over HTTP. By following REST principles and constraints, developers can build scalable, maintainable, and efficient APIs used in modern web and mobile applications.