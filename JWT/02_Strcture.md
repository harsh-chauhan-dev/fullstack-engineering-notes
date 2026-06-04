# JWT (JSON Web Token)

Since you're building an E-commerce application using Node.js, Express, React, and PostgreSQL, JWT is one of the most important authentication concepts to understand.

## What Problem Does JWT Solve?

Imagine your E-commerce application has the following users:

* Harsh
* Vishal
* Akshay
* Riddhima
* Prerna

When Vishal logs in, the server must know:

* Who is making the request?
* Is the user authenticated?
* Can they access only their own orders, cart, and profile information?

JWT helps the server identify the user without requiring them to log in again on every request.

---

# Structure of JWT

A JWT consists of three parts:

```text
Header.Payload.Signature
```

Example:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJpZCI6MTAxLCJuYW1lIjoiSGFyc2gifQ
.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

* Part 1 → Header
* Part 2 → Payload
* Part 3 → Signature

---

## 1. Header

The Header contains metadata about the token.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Where:

* `alg` = Algorithm used for signing the token
* `typ` = Token type (JWT)

---

## 2. Payload

The Payload contains information about the user.

Example:

```json
{
  "id": 101,
  "name": "Harsh"
}
```

⚠️ Important:

The payload is encoded, not encrypted.

Anyone who has the token can decode and view the payload.

Never store:

* Passwords
* OTPs
* Bank details
* Sensitive personal information

inside a JWT payload.

---

## 3. Signature

The Signature is generated using:

* Header
* Payload
* Secret Key

Example:

```js
jwt.sign(payload, secretKey);
```

The Signature ensures that:

* The token has not been modified.
* The token was issued by a trusted server.

If someone changes the payload, the signature becomes invalid and the server rejects the token.

---

## Token Verification

When a request reaches the server, the JWT is verified.

Example:

```js
jwt.verify(token, secretKey);
```

The server checks:

1. Is the token valid?
2. Has the token been modified?
3. Has the token expired?

If all checks pass, the user is authenticated.

---

# JWT Authentication Flow

```text
User Login
    |
    v
Server verifies credentials
    |
    v
Server generates JWT
    |
    v
Token sent to user
    |
    v
User sends token with requests
    |
    v
Server verifies token
    |
    v
Access granted
```

---

# Real-Life Example

Imagine your college issues an ID card.

* Login = Identity verification
* JWT = College ID card
* Server = Security guard

When entering the library:

```text
Show ID Card
      ↓
Guard verifies it
      ↓
Entry Allowed
```

Similarly:

```text
Send JWT
    ↓
Server verifies it
    ↓
Access Granted
```

---

## Advantages of JWT

* Stateless authentication
* Fast verification
* Easy to use with APIs
* Works across different platforms
* Supports expiration time
* Scalable for large applications

---

## Disadvantages of JWT

* Difficult to revoke immediately after issuance
* Token size is larger than a session ID
* If stored insecurely (for example, in LocalStorage), it may be stolen through XSS attacks
* Payload data is visible after decoding

---

## Interview Definition

JWT (JSON Web Token) is a compact, URL-safe token used for authentication and authorization. It securely transfers information between a client and a server using a digitally signed token consisting of three parts: Header, Payload, and Signature. JWT enables stateless authentication and is widely used in modern web applications and REST APIs.
