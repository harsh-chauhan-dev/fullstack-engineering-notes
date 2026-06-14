# Internet Basics & HTTP Fundamentals

## Context

The Internet is the global infrastructure that allows computers to communicate.
HTTP is the application-layer protocol that standardizes how web clients and servers exchange data.

Understanding this layer is critical before building backend systems.

---

# 1. What is the Internet?

The Internet is a global network of interconnected computers that communicate using standardized protocols (TCP/IP).

It is infrastructure — not a website, not an app.

It includes:
- Routers
- Switches
- Cables (fiber, ethernet)
- Wireless signals
- ISPs (Internet Service Providers)

---

# 2. Client–Server Model

Web communication follows a Client–Server architecture.

Client:
- Browser
- Mobile App
- Frontend Application

Server:
- Backend application (Node.js, Express)
- Database server
- Cloud service

Flow:

Client → Request → Server  
Server → Response → Client

---

# 3. IP Address

An IP address is a unique identifier for a device on a network.

Example:
192.168.1.1

Types:
- Public IP → Visible on the Internet
- Private IP → Used inside local networks

Without IP addresses, devices cannot communicate.

---

# 4. DNS (Domain Name System)

Humans remember domain names.
Computers communicate using IP addresses.

DNS translates:

example.com → 93.184.216.34

Steps:
1. Browser checks cache
2. OS checks cache
3. DNS server resolves domain
4. IP address returned

No DNS → No website access.

---

# 5. What is HTTP?

HTTP = HyperText Transfer Protocol

It is the protocol that defines how clients and servers communicate on the Web.

It is:
- Stateless
- Request–Response based
- Application-layer protocol

HTTP does not handle routing or physical transmission.
It only defines communication format.

---

# 6. HTTP Request Structure

Example:

GET /users HTTP/1.1  
Host: example.com  
User-Agent: Chrome  
Accept: application/json  

Components:
- Method
- Path
- Version
- Headers
- Optional Body

---

# 7. HTTP Response Structure

Example:

HTTP/1.1 200 OK  
Content-Type: application/json  

{
  "users": []
}

Components:
- Status Line
- Headers
- Body

---

# 8. HTTP Methods

GET → Read data  
POST → Create data  
PUT → Replace data  
PATCH → Update partially  
DELETE → Remove data  

Correct REST-style usage:

GET /users  
POST /users  
DELETE /users/10  

---

# 9. HTTP Status Codes

1xx → Informational  
2xx → Success  
3xx → Redirection  
4xx → Client error  
5xx → Server error  

Common codes:

200 → OK  
201 → Created  
400 → Bad Request  
401 → Unauthorized  
403 → Forbidden  
404 → Not Found  
500 → Internal Server Error  

---

# 10. HTTP vs HTTPS

HTTP:
- Port 80
- No encryption

HTTPS:
- Port 443
- Uses TLS encryption
- Secure communication

HTTPS protects sensitive data like passwords and tokens.

---

# 11. Stateless Nature of HTTP

Each request is independent.

Server does not remember previous requests unless:
- Cookies
- Sessions
- JWT tokens

This is why authentication systems exist.

---

# 12. Practical Observation Task

Open Chrome → F12 → Network tab → Refresh page.

Inspect:
- Request method
- Status code
- Headers
- Response body

This builds real-world understanding.

---

# Interview Questions

1. What happens when you type a URL in the browser?
2. What is DNS?
3. Difference between HTTP and HTTPS?
4. What does stateless mean?
5. Difference between public and private IP?
6. Explain HTTP request structure.
7. What are common status codes?

---

# Summary

The Internet provides infrastructure.
DNS resolves domain names.
HTTP standardizes communication.
Client–Server model enables web interaction.

Every backend system you build will rely on these fundamentals.