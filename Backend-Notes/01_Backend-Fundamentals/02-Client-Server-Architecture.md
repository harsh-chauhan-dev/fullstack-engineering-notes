# Client-Server Architecture

![Client-Server Architecture](<assets/Client-Server-1.png>)

## Introduction

Client-Server Architecture is a computing model where multiple clients request services or resources from a central server. The server manages processing, data storage, and resources, while clients handle user interaction.

It is the foundation of:

* Websites
* Mobile Applications
* Desktop Applications
* Cloud Services
* Online Games
* Banking Systems

Almost every modern application follows the Client-Server model.

---

## What is Client-Server Architecture?

> Client-Server Architecture is a network-based architecture where clients send requests to a server, and the server processes those requests and returns appropriate responses.

### Client

A client is a device or application that requests services from a server.

**Examples:**

* Web Browsers (Chrome, Firefox)
* Mobile Applications
* Desktop Applications
* React Applications

### Server

A server is a computer or software that provides services to clients.

**Examples:**

* Node.js
* Express.js
* Django
* Apache
* Nginx

---

## Real-Life Example

Think of a restaurant:

* **Customer (Client)** places an order.
* **Kitchen (Server)** prepares the food.
* **Waiter (Network/API)** delivers the food back.

```text
+------------+        Request        +------------+
|   Client   | --------------------> |   Server   |
| (Browser)  |                       | (Backend)  |
+------------+ <-------------------- +------------+
                    Response
```

---

![Architecture Diagram](<assets/Client-Server-2.png>)

---

## Components of Client-Server Architecture

### 1. Client

The client is responsible for interacting with users and sending requests to the server.

**Responsibilities:**

* Collect user input
* Send requests
* Display responses

**Examples:**

* Chrome
* Firefox
* Mobile Apps
* React Frontend

---

### 2. Server

The server receives requests, processes them, and sends responses.

**Responsibilities:**

* Handle requests
* Execute business logic
* Communicate with databases
* Return responses

**Examples:**

* Node.js
* Express.js
* Django
* Apache

---

### 3. Database

A database stores application data permanently.

**Examples:**

* MySQL
* PostgreSQL
* MongoDB
* Oracle

**Database Operations:**

* Store Data
* Retrieve Data
* Update Data
* Delete Data

```text
Client → Server → Database
```

**Example:**

YouTube stores:

* Videos
* Users
* Comments
* Likes

inside databases.

---

### 4. Network

The network connects clients and servers.

**Examples:**

* Internet
* Wi-Fi
* LAN

### Protocols Used

* HTTP
* HTTPS
* TCP/IP

---

# How Client-Server Architecture Works

The Client-Server model follows a **Request-Response Cycle**.

## Step 1: User Action

The user performs an action such as clicking a button or submitting a form.

```text
User
 ↓
Client (Browser/App)
```

---

## Step 2: Client Sends Request

The client sends an HTTP request to the server.

```http
GET /profile
```

```text
Client → Server
```

---

## Step 3: Server Processes Request

The server receives the request and executes business logic.

```text
Validate Request
↓
Process Logic
↓
Generate Response
```

---

## Step 4: Database Interaction

If data is needed, the server queries the database.

```text
Server → Database
```

Example:

```sql
SELECT * FROM users;
```

---

## Step 5: Database Returns Data

```text
Database → Server
```

Example:

```json
{
  "name": "Harsh"
}
```

---

## Step 6: Server Sends Response

The server creates and returns a response.

```json
{
  "success": true,
  "message": "Data Retrieved Successfully"
}
```

---

## Step 7: Client Displays Result

The client receives the response and updates the user interface.

```text
Database
   ↓
Server
   ↓
Client
   ↓
User
```

---

## Request-Response Flow Diagram

```text
┌──────────┐
│  Client  │
└────┬─────┘
     │ Request
     ▼
┌──────────┐
│  Server  │
└────┬─────┘
     │ Query
     ▼
┌──────────┐
│ Database │
└────┬─────┘
     │ Data
     ▲
┌────┴─────┐
│  Server  │
└────┬─────┘
     │ Response
     ▲
┌────┴─────┐
│  Client  │
└──────────┘
```

---

## Types of Client-Server Architecture

### 1. Two-Tier Architecture

The client directly communicates with the database.

```text
Client
  ↕
Database
```

#### Advantages

* Simple
* Fast

#### Disadvantages

* Poor Security
* Difficult Maintenance
* Not Scalable

#### Example

Small Office Applications

---

### 2. Three-Tier Architecture

The most common architecture used in modern web applications.

```text
Client
 ↓
Application Server
 ↓
Database
```

#### Advantages

* Secure
* Easy Maintenance
* Scalable

#### Disadvantages

* More Complexity

#### Examples

* MERN Stack
* PERN Stack
* E-commerce Platforms

---

### 3. N-Tier Architecture

Used by large-scale enterprise systems.

```text
Client
 ↓
Load Balancer
 ↓
API Gateway
 ↓
Application Server
 ↓
Microservices
 ↓
Database
```

#### Examples

* Netflix
* Amazon
* Google

#### Advantages

* High Scalability
* Better Reliability
* High Performance

---

## Client-Server Architecture in PERN Stack

```text
React (Client)
      ↓
HTTP Request
      ↓
Node.js + Express (Server)
      ↓
PostgreSQL (Database)
```

### Flow

1. React sends a request.
2. Express receives the request.
3. Server processes the request.
4. PostgreSQL returns data.
5. Express sends a response.
6. React updates the UI.

---

## Characteristics

* Centralized Data Management
* Resource Sharing
* Scalability
* Security
* Reliability
* Maintainability

---

## Advantages

### 1. Better Security

Clients cannot directly access the database.

```text
Client → Server → Database
```

### 2. Easy Maintenance

Only server updates are needed.

### 3. Centralized Data

Data remains consistent.

### 4. Scalability

Supports thousands or millions of users.

### 5. Resource Sharing

Multiple clients share services.

### 6. Backup and Recovery

Data can be backed up centrally.

---

## Disadvantages

### 1. Single Point of Failure

If the server crashes, all clients may be affected.

### 2. Network Dependency

Requires a stable internet connection.

### 3. High Infrastructure Cost

Large systems require powerful servers.

### 4. Heavy Traffic Problems

Too many users can overload the server.

---

## Interview Questions

### What is Client-Server Architecture?

A network model where clients request services and servers process requests and return responses.

### What is a Client?

A software application or device that requests resources from a server.

### What is a Server?

A computer or software that provides services to clients.

### What is the role of a Database?

To store and manage application data.

### Which architecture is most commonly used today?

**Three-Tier Architecture**

```text
Client
 ↓
Server
 ↓
Database
```

---

## Summary

```text
CLIENT
(Browser/App)
      ↓ Request
SERVER
(Processes Request)
      ↓ Query
DATABASE
(Stores Data)
      ↑ Data
SERVER
      ↑ Response
CLIENT
      ↑
USER
```

### Key Takeaway

> **Client sends Request → Server processes Request → Database provides Data → Server sends Response → Client displays Result.**

This simple Request-Response cycle is the foundation of almost every modern web application.
