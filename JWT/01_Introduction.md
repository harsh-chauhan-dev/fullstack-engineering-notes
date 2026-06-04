# JWT (JSON Web Token)

JWT (JSON Web Token) is a compact and secure way to transfer information between two parties as a JSON object. It is commonly used for:

* **Authentication** – Verifying who a user is.
* **Authorization** – Determining what a user is allowed to do.

Think of JWT as a digital identity card. After a user successfully logs in, the server generates a JWT and sends it to the client. The client then includes this token in future requests, allowing the server to identify and verify the user.

---

## Features of JWT

### 1. Stateless Authentication

The server does not need to store session data.

Instead:

* The user stores the token.
* The server verifies the token whenever a request arrives.

**Benefit:** Better scalability because the server does not need to maintain session information for every user.

---

### 2. Compact Size

JWT is a small text string that is easy to transmit over the internet.

Example:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6MTUxNjIzOTAyMn0.
KMUFsIDTnFmyG3nMiGM6H9FNFUROf3wh7SmqJp-QV30
```

JWT can be easily sent through:

* HTTP Headers
* Cookies
* APIs

---

### 3. Secure Verification

JWT is digitally signed using a secret key.

Example:

```javascript
jwt.sign(payload, secretKey);
```

If someone modifies the token:

```text
userId = 1
↓
userId = 999
```

The signature becomes invalid, and the server rejects the token.

---

### 4. Cross-Platform Support

JWT works with almost every modern programming language and framework, including:

* Node.js
* Java
* Python
* PHP
* C#
* Go

This makes JWT a popular choice for modern web applications and APIs.

---

### 5. Contains User Information

JWT can carry user-related information inside its payload.

Example:

```json
{
  "id": 101,
  "email": "harsh@gmail.com",
  "role": "user"
}
```

The server can quickly identify the user from the token without querying the database on every request.

---

### 6. Expiration Time

JWT can automatically expire after a specified period.

Example:

```javascript
jwt.sign(payload, secret, {
  expiresIn: "1d"
});
```

This token remains valid for one day.

After expiration:

* The user must log in again, or
* The application can issue a new token using a refresh token.

---

### 7. URL Safe

JWT uses Base64URL encoding, making it safe to send through:

* URLs
* HTTP Headers
* Cookies

Example:

```text
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
```

---

## When Should We Use JWT?

JWT is commonly used in:

* Authentication Systems
* REST APIs
* React Applications
* Angular Applications
* Vue Applications
* Mobile Applications
* Microservices Architecture

---

## Summary

JWT is a lightweight, secure, and scalable authentication mechanism widely used in modern web development. It allows servers to authenticate users without storing session data, making it ideal for APIs, single-page applications, and distributed systems.
