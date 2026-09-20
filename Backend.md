Absolutely. Since your interview focus is **Backend = Node.js + Express.js + REST APIs + Authentication + Role-Based Access Control (RBAC)**, you should prepare this as an **interview-oriented backend sheet**, not as a generic Node.js tutorial.

# Backend Interview Notes

## Node.js + Express.js + REST API + Authentication + RBAC

---

# 1. Backend Fundamentals

## What is Backend?

The **backend** is the server-side part of an application responsible for:

* Business logic
* Database operations
* Authentication and authorization
* API creation
* Data validation
* Security
* Request/response handling
* Communication with frontend

Typical flow:

```text
Frontend
   ↓
HTTP Request
   ↓
Backend Server
   ↓
Authentication
   ↓
Authorization
   ↓
Business Logic
   ↓
Database
   ↓
Backend
   ↓
HTTP Response
   ↓
Frontend
```

Example:

```text
React
  ↓
POST /api/tickets
  ↓
Express.js
  ↓
JWT Authentication
  ↓
RBAC
  ↓
Ticket Controller
  ↓
MongoDB
  ↓
Response
```

---

# 2. Node.js

## What is Node.js?

**Node.js is a JavaScript runtime environment that allows JavaScript to execute outside the browser.**

It is built on Google's **V8 JavaScript engine**.

Normally:

```text
JavaScript → Browser
```

With Node.js:

```text
JavaScript → Server
```

Therefore, you can use JavaScript for backend development.

---

# 3. Why Node.js?

Important interview points:

* JavaScript on server
* Fast execution through V8
* Non-blocking I/O
* Event-driven architecture
* Asynchronous programming
* Large npm ecosystem
* Good for I/O-heavy applications
* Easy integration with frontend JavaScript applications

---

# 4. Node.js Architecture

Node.js primarily uses:

```text
Single JavaScript Thread
        ↓
Event Loop
        ↓
Non-blocking I/O
        ↓
Callbacks / Promises
```

### Important:

**Node.js is single-threaded for JavaScript execution, but it can use background threads through its runtime/libuv for certain operations.**

---

# 5. What is the Event Loop?

The **event loop** allows Node.js to handle asynchronous operations without blocking the main JavaScript thread.

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Timer
```

Because `setTimeout()` is asynchronous.

---

# 6. Blocking vs Non-Blocking

### Blocking

```javascript
const data = fs.readFileSync("file.txt");
console.log(data);
```

The execution waits until the file is completely read.

### Non-blocking

```javascript
fs.readFile("file.txt", (err, data) => {
    console.log(data);
});
```

Node.js can continue doing other work.

### Interview answer

> Node.js uses non-blocking asynchronous I/O, allowing it to handle multiple concurrent requests efficiently without waiting for every I/O operation to finish.

---

# 7. Synchronous vs Asynchronous

### Synchronous

```text
Task 1
 ↓
wait
 ↓
Task 2
 ↓
wait
 ↓
Task 3
```

### Asynchronous

```text
Task 1 ──────────→ result
Task 2 ─────→ result
Task 3 ─────────────→ result
```

Node.js heavily uses asynchronous operations.

---

# 8. Callback

A callback is a function passed to another function to be executed later.

```javascript
function greet(name, callback) {
    console.log("Hello " + name);
    callback();
}

greet("Shreya", () => {
    console.log("Done");
});
```

---

# 9. Promise

A Promise represents the eventual result of an asynchronous operation.

States:

```text
Pending
   ↓
 ┌───────┐
 ↓       ↓
Fulfilled Rejected
```

Example:

```javascript
const promise = fetchData();

promise
    .then(data => console.log(data))
    .catch(error => console.log(error));
```

---

# 10. async/await

Modern Node.js applications commonly use:

```javascript
async function getUser() {
    try {
        const user = await User.findById(id);
        return user;
    } catch (error) {
        console.log(error);
    }
}
```

### Why use async/await?

It makes asynchronous code easier to read and maintain compared with deeply nested callbacks.

---

# 11. npm

**npm = Node Package Manager**

Used for:

* Installing packages
* Managing dependencies
* Running scripts
* Publishing packages

Example:

```bash
npm install express
```

---

# 12. package.json

Contains project metadata and dependencies.

Example:

```json
{
  "name": "backend",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^5.0.0"
  }
}
```

---

# 13. package-lock.json

`package-lock.json` records the exact dependency versions installed.

### package.json

Defines:

```text
What dependencies the project needs
```

### package-lock.json

Defines:

```text
Exactly which dependency versions were installed
```

---

# 14. Express.js

## What is Express.js?

**Express.js is a lightweight web framework for Node.js used to build APIs and web servers.**

It provides:

* Routing
* Middleware
* Request handling
* Response handling
* Error handling
* API development

---

# 15. Basic Express Server

```javascript
const express = require("express");

const app = express();

app.use(express.json());

app.get("/", (req, res) => {
    res.json({
        message: "Server running"
    });
});

app.listen(5000, () => {
    console.log("Server started");
});
```

---

# 16. Request and Response

Every HTTP request contains things such as:

```text
Method
URL
Headers
Body
Query parameters
Route parameters
```

Express provides:

```javascript
req
res
```

### Request

```javascript
req.body
req.params
req.query
req.headers
```

### Response

```javascript
res.json()
res.send()
res.status()
res.sendStatus()
```

---

# 17. HTTP Methods

Important REST methods:

| Method | Purpose                   |
| ------ | ------------------------- |
| GET    | Retrieve data             |
| POST   | Create data               |
| PUT    | Replace/update resource   |
| PATCH  | Partially update resource |
| DELETE | Delete resource           |

Example:

```text
GET    /users
POST   /users
GET    /users/10
PUT    /users/10
PATCH  /users/10
DELETE /users/10
```

---

# 18. REST API

## What is REST?

**REST = Representational State Transfer**

REST is an architectural style for designing APIs around resources.

Example resource:

```text
/users
/tickets
/products
/orders
```

---

# 19. REST Principles

Important principles:

### 1. Client-Server

Frontend and backend are separate.

### 2. Stateless

Every request should contain the information required to process it.

Server does not rely on previous request state.

### 3. Uniform Interface

Use consistent resource-based URLs.

Good:

```text
GET /users/10
```

Less RESTful:

```text
GET /getUserById?id=10
```

### 4. Cacheable

Responses can be cached when appropriate.

### 5. Layered System

Client doesn't necessarily know whether it communicates directly with the server or through intermediary systems.

---

# 20. REST API Example

For ticket management:

```text
POST   /api/tickets
GET    /api/tickets
GET    /api/tickets/:id
PATCH  /api/tickets/:id
DELETE /api/tickets/:id
```

---

# 21. Route Parameters

Example:

```javascript
app.get("/users/:id", (req, res) => {
    console.log(req.params.id);
});
```

Request:

```text
GET /users/123
```

Then:

```javascript
req.params.id
```

returns:

```text
123
```

---

# 22. Query Parameters

Request:

```text
GET /users?page=2&limit=10
```

Access:

```javascript
req.query.page
req.query.limit
```

Useful for:

* Searching
* Filtering
* Pagination
* Sorting

Example:

```text
GET /products?category=mobile&sort=price
```

---

# 23. Request Body

For POST:

```javascript
app.use(express.json());

app.post("/users", (req, res) => {
    const { name, email } = req.body;

    res.json({
        name,
        email
    });
});
```

Request:

```json
{
  "name": "Shreya",
  "email": "shreya@example.com"
}
```

---

# 24. HTTP Status Codes

## 2xx — Success

### 200

Request successful.

```text
GET successful
```

### 201

Resource created.

```text
POST successful
```

### 204

Success but no response body.

Commonly used with DELETE.

---

## 4xx — Client Error

### 400 Bad Request

Invalid request.

### 401 Unauthorized

Authentication missing/invalid.

Think:

> "Who are you?"

### 403 Forbidden

Authenticated but doesn't have permission.

Think:

> "I know who you are, but you aren't allowed."

### 404 Not Found

Resource doesn't exist.

### 409 Conflict

Conflict with existing state.

Example:

```text
Email already registered
```

---

## 5xx — Server Error

### 500 Internal Server Error

Unexpected server-side error.

### 503 Service Unavailable

Server/service temporarily unavailable.

---

# 25. 401 vs 403 ⭐

Very common interview question.

### 401

User is **not authenticated**.

```text
No token
Invalid token
Expired token
```

### 403

User is authenticated but **not authorized**.

```text
User = normal employee
Resource = admin-only
```

Therefore:

```text
401 → Authentication problem
403 → Authorization problem
```

---

# 26. Middleware

## What is Middleware?

Middleware is a function that executes **between the incoming request and the final response**.

```text
Request
   ↓
Middleware
   ↓
Route Handler
   ↓
Response
```

Example:

```javascript
app.use((req, res, next) => {
    console.log(req.method, req.url);
    next();
});
```

`next()` passes control to the next middleware.

---

# 27. Types of Middleware

### Application-level

```javascript
app.use(...)
```

### Router-level

```javascript
router.use(...)
```

### Built-in

```javascript
express.json()
express.urlencoded()
```

### Third-party

Examples:

```text
cors
helmet
morgan
```

### Error-handling middleware

```javascript
(err, req, res, next)
```

---

# 28. Middleware Execution

Example:

```javascript
app.use(logger);

app.use(authenticate);

app.get("/profile", getProfile);
```

Flow:

```text
Request
 ↓
logger
 ↓
authenticate
 ↓
getProfile
 ↓
Response
```

---

# 29. Router

Instead of putting everything in `server.js`, routes can be separated.

```javascript
const express = require("express");

const router = express.Router();

router.get("/", getUsers);
router.post("/", createUser);

module.exports = router;
```

Then:

```javascript
app.use("/api/users", userRouter);
```

Now:

```text
GET /api/users
POST /api/users
```

---

# 30. Controllers

Controllers contain request-handling logic.

Example:

```javascript
const getUsers = async (req, res) => {
    const users = await User.find();

    res.status(200).json(users);
};
```

---

# 31. Models

Models represent database data.

For MongoDB + Mongoose:

```javascript
const userSchema = new mongoose.Schema({
    name: String,
    email: String,
    password: String,
    role: String
});

const User = mongoose.model("User", userSchema);
```

---

# 32. Recommended Backend Structure

For your interviews, know this architecture:

```text
backend/
│
├── server.js
├── app.js
│
├── routes/
│   ├── auth.routes.js
│   ├── user.routes.js
│   └── ticket.routes.js
│
├── controllers/
│   ├── auth.controller.js
│   ├── user.controller.js
│   └── ticket.controller.js
│
├── models/
│   ├── User.js
│   └── Ticket.js
│
├── middleware/
│   ├── auth.middleware.js
│   ├── role.middleware.js
│   └── error.middleware.js
│
├── services/
│
├── utils/
│
└── config/
```

---

# 33. Authentication

## What is Authentication?

Authentication answers:

> **Who are you?**

Example:

```text
User enters:
email + password
       ↓
Backend verifies credentials
       ↓
User authenticated
```

---

# 34. Authorization

Authorization answers:

> **What are you allowed to do?**

Example:

```text
Authenticated User
        ↓
Role = Admin
        ↓
Can delete users
```

while:

```text
Role = User
        ↓
Cannot delete users
```

---

# 35. Authentication vs Authorization ⭐

| Authentication | Authorization                |
| -------------- | ---------------------------- |
| Who are you?   | What can you access?         |
| Login          | Permissions                  |
| Identity       | Access control               |
| JWT/session    | Role/permission              |
| Happens first  | Happens after authentication |

---

# 36. Password Storage

**Never store plain-text passwords.**

Bad:

```json
{
  "password": "mypassword123"
}
```

Instead use password hashing.

Common library:

```text
bcrypt
```

---

# 37. Password Hashing

```javascript
const bcrypt = require("bcrypt");

const hashedPassword = await bcrypt.hash(password, 10);
```

Store:

```text
hashedPassword
```

not:

```text
password
```

---

# 38. Password Verification

During login:

```javascript
const isMatch = await bcrypt.compare(
    password,
    user.password
);
```

If:

```javascript
isMatch === true
```

credentials are valid.

---

# 39. Why Hashing Instead of Encryption?

### Hashing

One-way.

```text
password
   ↓
hash
```

You don't decrypt the hash to retrieve the original password.

### Encryption

Two-way.

```text
plaintext
   ↓
encrypted
   ↓
decrypted
```

Passwords should generally be stored using **password hashing**, not reversible encryption.

---

# 40. JWT

## What is JWT?

**JWT = JSON Web Token**

It is commonly used for stateless authentication.

Flow:

```text
Login
 ↓
Backend verifies credentials
 ↓
JWT generated
 ↓
Client receives token
 ↓
Client sends token with future requests
 ↓
Backend verifies token
```

---

# 41. JWT Structure ⭐

JWT consists of three parts:

```text
Header.Payload.Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

### Header

Contains metadata such as algorithm.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Payload

Contains claims.

```json
{
  "userId": "123",
  "role": "admin"
}
```

### Signature

Used to verify token integrity.

---

# 42. JWT Example

Using `jsonwebtoken`:

```javascript
const jwt = require("jsonwebtoken");

const token = jwt.sign(
    {
        userId: user._id,
        role: user.role
    },
    process.env.JWT_SECRET,
    {
        expiresIn: "1h"
    }
);
```

---

# 43. JWT Verification

```javascript
const decoded = jwt.verify(
    token,
    process.env.JWT_SECRET
);
```

If valid:

```javascript
decoded.userId
decoded.role
```

can be accessed.

---

# 44. JWT Authentication Middleware

Typical implementation:

```javascript
const jwt = require("jsonwebtoken");

const authenticate = (req, res, next) => {
    const authHeader = req.headers.authorization;

    if (!authHeader) {
        return res.status(401).json({
            message: "Authentication required"
        });
    }

    const token = authHeader.split(" ")[1];

    try {
        const decoded = jwt.verify(
            token,
            process.env.JWT_SECRET
        );

        req.user = decoded;

        next();

    } catch (error) {
        return res.status(401).json({
            message: "Invalid token"
        });
    }
};
```

---

# 45. Authorization Header

Frontend sends:

```http
Authorization: Bearer <JWT_TOKEN>
```

Backend extracts:

```javascript
req.headers.authorization
```

Then:

```javascript
const token = authHeader.split(" ")[1];
```

---

# 46. Complete JWT Flow ⭐⭐⭐

```text
              LOGIN
                ↓
       email + password
                ↓
        Find user in DB
                ↓
      bcrypt.compare()
                ↓
        Credentials valid?
           /          \
         No            Yes
         ↓              ↓
       401          Generate JWT
                         ↓
                  Send JWT to client
                         ↓
                Client stores token
                         ↓
              Future API request
                         ↓
             Authorization header
                         ↓
              Authentication MW
                         ↓
                 jwt.verify()
                         ↓
                  req.user = decoded
                         ↓
                  Authorization
                         ↓
                    Controller
```

This entire flow is extremely important for your interview.

---

# 47. Role-Based Access Control

## What is RBAC?

**RBAC = Role-Based Access Control**

Access is granted based on the user's role.

Example:

```text
Admin
Moderator
User
```

Permissions:

```text
Admin → create/read/update/delete
Moderator → read/update
User → read
```

---

# 48. RBAC Flow

```text
Request
   ↓
JWT Authentication
   ↓
User identified
   ↓
Role extracted
   ↓
Role authorization
   ↓
Permission?
  / \
Yes  No
 ↓    ↓
API   403
```

---

# 49. Role Middleware

```javascript
const authorizeRoles = (...allowedRoles) => {
    return (req, res, next) => {

        if (!allowedRoles.includes(req.user.role)) {
            return res.status(403).json({
                message: "Access denied"
            });
        }

        next();
    };
};
```

Usage:

```javascript
router.delete(
    "/users/:id",
    authenticate,
    authorizeRoles("admin"),
    deleteUser
);
```

Flow:

```text
Request
 ↓
authenticate
 ↓
authorizeRoles("admin")
 ↓
deleteUser
```

---

# 50. Authentication + RBAC Example

```javascript
router.get(
    "/admin/dashboard",
    authenticate,
    authorizeRoles("admin"),
    getDashboard
);
```

### Step 1

Authentication:

```text
Is JWT valid?
```

### Step 2

Authorization:

```text
Does user have admin role?
```

### Step 3

Controller:

```text
Return dashboard
```

---

# 51. RBAC Database Design

User:

```json
{
  "name": "Shreya",
  "email": "shreya@example.com",
  "password": "hashedPassword",
  "role": "moderator"
}
```

Then:

```javascript
if (user.role === "admin")
```

can determine permissions.

---

# 52. RBAC vs Permission-Based Access

### RBAC

```text
User → Role → Permissions
```

Example:

```text
Shreya → Moderator → update tickets
```

### Permission-based

```text
User → Permissions
```

Example:

```text
Shreya → ["ticket:read", "ticket:update"]
```

RBAC is simpler for many applications.

---

# 53. REST API + Authentication Example

Suppose you have a ticket management system.

### Public

```text
POST /api/auth/register
POST /api/auth/login
```

### Authenticated

```text
GET /api/tickets
POST /api/tickets
```

### Moderator

```text
PATCH /api/tickets/:id
```

### Admin

```text
DELETE /api/tickets/:id
```

---

# 54. API Layer Architecture

A clean backend often follows:

```text
Route
  ↓
Middleware
  ↓
Controller
  ↓
Service
  ↓
Model
  ↓
Database
```

Example:

```text
POST /api/tickets
        ↓
ticket.routes.js
        ↓
authenticate
        ↓
ticketController.createTicket()
        ↓
ticketService.createTicket()
        ↓
Ticket model
        ↓
MongoDB
```

---

# 55. Controller vs Service

### Controller

Handles:

* HTTP request
* HTTP response
* Input extraction
* Status codes

### Service

Handles:

* Business logic
* Complex operations
* Reusable logic

Example:

```javascript
const createTicket = async (req, res, next) => {
    try {
        const ticket = await ticketService.createTicket(
            req.body,
            req.user
        );

        res.status(201).json(ticket);

    } catch (error) {
        next(error);
    }
};
```

---

# 56. Error Handling

Instead of repeating:

```javascript
try {
   ...
} catch(error) {
   ...
}
```

you can use centralized error middleware.

```javascript
app.use((err, req, res, next) => {
    console.error(err);

    res.status(err.statusCode || 500).json({
        message: err.message || "Internal Server Error"
    });
});
```

Important:

```text
(err, req, res, next)
```

The four parameters identify Express error-handling middleware.

---

# 57. `next()`

Middleware:

```javascript
app.use((req, res, next) => {
    console.log("Middleware");
    next();
});
```

`next()` means:

> Pass control to the next middleware or route handler.

---

# 58. CORS

## What is CORS?

**CORS = Cross-Origin Resource Sharing**

It controls whether a browser allows requests between different origins.

Example:

```text
Frontend:
http://localhost:3000

Backend:
http://localhost:5000
```

Different origins.

You may configure:

```javascript
const cors = require("cors");

app.use(cors());
```

Or restrict it:

```javascript
app.use(cors({
    origin: "http://localhost:3000"
}));
```

---

# 59. Environment Variables

Never hardcode sensitive configuration.

Bad:

```javascript
const JWT_SECRET = "my-secret-key";
```

Better:

```javascript
const JWT_SECRET = process.env.JWT_SECRET;
```

`.env`:

```text
PORT=5000
MONGO_URI=...
JWT_SECRET=...
```

Use:

```javascript
require("dotenv").config();
```

---

# 60. Why `.env`?

Store:

* Database credentials
* JWT secret
* API keys
* Port configuration
* External service credentials

And add:

```text
.env
```

to `.gitignore`.

---

# 61. Input Validation

Never blindly trust:

```javascript
req.body
```

Validate:

```text
Email
Password
Required fields
Data types
String lengths
Allowed values
```

Libraries:

```text
Joi
Zod
express-validator
```

Example:

```javascript
if (!email || !password) {
    return res.status(400).json({
        message: "Email and password required"
    });
}
```

---

# 62. Authentication Security

Important interview points:

### Passwords

Use:

```text
bcrypt
```

### Tokens

Use expiration:

```javascript
expiresIn: "1h"
```

### Secrets

Use environment variables.

### APIs

Use HTTPS in production.

### Input

Validate and sanitize.

### HTTP headers

Use security middleware such as Helmet where appropriate.

---

# 63. Helmet

Helmet helps set various HTTP security headers.

```javascript
const helmet = require("helmet");

app.use(helmet());
```

Interview answer:

> Helmet provides middleware that helps secure Express applications by setting various HTTP response headers.

---

# 64. Rate Limiting

Protect APIs against excessive requests.

Example:

```text
100 requests / 15 minutes
```

Useful for:

* Login
* OTP
* Password reset
* Public APIs

A common package is:

```text
express-rate-limit
```

---

# 65. SQL Injection / NoSQL Injection

Never directly trust user input.

For SQL:

```text
SELECT * FROM users WHERE email = '...'
```

Poorly handled input can manipulate queries.

For MongoDB, malicious query operators can also cause problems if unvalidated input is passed directly into database queries.

Use:

* Validation
* Sanitization
* Proper database APIs
* Least privilege

---

# 66. API Pagination

Instead of:

```text
GET /tickets
```

returning 1 million records:

```text
GET /tickets?page=2&limit=20
```

Backend:

```javascript
const page = Number(req.query.page) || 1;
const limit = Number(req.query.limit) || 20;

const skip = (page - 1) * limit;
```

Then query:

```javascript
Ticket.find()
    .skip(skip)
    .limit(limit);
```

---

# 67. API Filtering

```text
GET /tickets?status=open
```

Backend:

```javascript
const { status } = req.query;

const filter = {};

if (status) {
    filter.status = status;
}

const tickets = await Ticket.find(filter);
```

---

# 68. API Sorting

```text
GET /tickets?sort=createdAt
```

Database query can apply sorting.

For descending:

```javascript
.sort({ createdAt: -1 })
```

---

# 69. API Search

Example:

```text
GET /tickets?search=payment
```

Backend searches relevant fields.

For MongoDB, this might use regex or preferably an appropriate indexed/text-search strategy depending on requirements.

---

# 70. API Versioning

You may version APIs:

```text
/api/v1/users
/api/v2/users
```

Benefits:

* Backward compatibility
* Gradual API changes
* Easier client migration

---

# 71. PUT vs PATCH ⭐

### PUT

Generally represents replacement of the resource.

```http
PUT /users/123
```

Could send the complete representation.

### PATCH

Partial modification.

```http
PATCH /users/123
```

Example:

```json
{
  "name": "New Name"
}
```

---

# 72. Idempotency ⭐

An operation is idempotent if repeating it produces the same intended resource state.

Common examples:

```text
GET → idempotent
PUT → generally idempotent
DELETE → generally idempotent
```

POST is generally **not** idempotent.

---

# 73. Stateless API

Suppose:

```text
Request 1 → Server
Request 2 → Server
```

The server shouldn't need hidden session state from request 1 to understand request 2 when using a stateless JWT API.

JWT allows authentication information to be carried with each request.

---

# 74. JWT vs Session Authentication

| JWT                              | Session                                                 |
| -------------------------------- | ------------------------------------------------------- |
| Token-based                      | Session-based                                           |
| Often stateless                  | Server stores session state                             |
| Token contains claims            | Session ID references server state                      |
| Common for APIs                  | Common for traditional web apps                         |
| Easy across distributed services | Requires session storage/sharing in scaled environments |

Neither is universally "better"; choice depends on architecture and security requirements.

---

# 75. Access Token vs Refresh Token

### Access Token

Short-lived.

```text
15 min / 1 hour
```

Used for API requests.

### Refresh Token

Longer-lived.

Used to obtain a new access token.

Flow:

```text
Login
 ↓
Access Token + Refresh Token
 ↓
Access token expires
 ↓
Refresh token
 ↓
New Access Token
```

---

# 76. Token Expiration

JWT:

```javascript
jwt.sign(
    payload,
    secret,
    { expiresIn: "1h" }
);
```

Why?

If a token is stolen, its usefulness is limited by expiration.

---

# 77. Authentication Middleware vs Authorization Middleware

### Authentication

```javascript
authenticate(req, res, next)
```

Does:

```text
Extract token
 ↓
Verify token
 ↓
Identify user
 ↓
req.user
```

### Authorization

```javascript
authorizeRoles("admin")
```

Does:

```text
Read req.user.role
 ↓
Check permission
 ↓
Allow / deny
```

---

# 78. Complete Backend Request Flow ⭐⭐⭐

For your interviews, memorize this:

```text
CLIENT
  ↓
HTTP Request
  ↓
Express Router
  ↓
CORS / Security Middleware
  ↓
Authentication Middleware
  ↓
Authorization / RBAC Middleware
  ↓
Validation Middleware
  ↓
Controller
  ↓
Service / Business Logic
  ↓
Model / Database
  ↓
Controller
  ↓
HTTP Response
  ↓
CLIENT
```

Example:

```text
POST /api/tickets
        ↓
Express Router
        ↓
JWT verification
        ↓
User identified
        ↓
RBAC
        ↓
Input validation
        ↓
Ticket Controller
        ↓
Gemini/Business Logic
        ↓
MongoDB
        ↓
201 Created
```

---

# 79. Your AI Ticket Management Project Connection

This is particularly important because your project uses this backend architecture.

Your project flow can be explained as:

```text
Client
  ↓
REST API
  ↓
Express.js
  ↓
JWT Authentication
  ↓
RBAC
  ↓
Ticket Controller
  ↓
MongoDB
  ↓
Inngest
  ↓
Gemini API
  ↓
Ticket Classification
  ↓
Update MongoDB
  ↓
Moderator Assignment
  ↓
Email Notification
```

---

# 80. How to Explain Your Backend in Interview

### "Explain your backend architecture."

> I built the backend using Node.js and Express.js and exposed REST APIs for authentication and ticket management. I separated routes, controllers, middleware, and models to keep the application modular. Authentication was implemented using JWT, while bcrypt was used for password hashing. After authentication, I used role-based middleware to restrict endpoints based on the user's role. The controllers handled HTTP requests and responses, while database operations were performed through the model layer. For asynchronous ticket processing, I integrated Inngest and used the Gemini API for ticket categorization and priority analysis.

---

# 81. "How does login work in your project?"

> The user sends their email and password through the login API. The backend finds the user in MongoDB and uses bcrypt to compare the provided password with the stored hashed password. If the credentials are valid, the server generates a JWT containing relevant claims such as the user ID and role. The client then sends this token with subsequent API requests using the Authorization Bearer header. Authentication middleware verifies the token and attaches the decoded user information to `req.user`.

---

# 82. "How did you implement RBAC?"

> I stored the user's role in the user record and included the role in the authenticated user context. After JWT authentication, an authorization middleware checks whether the user's role is allowed to access the requested route. For example, an admin-only endpoint uses `authenticate` followed by `authorizeRoles("admin")`. If the token is valid but the role isn't permitted, the API returns 403 Forbidden.

---

# 83. "What happens when JWT is invalid?"

```text
Request
 ↓
Authorization Header
 ↓
Extract JWT
 ↓
jwt.verify()
 ↓
Invalid
 ↓
401 Unauthorized
```

---

# 84. "What happens if JWT is valid but role isn't allowed?"

```text
JWT valid
 ↓
User authenticated
 ↓
Role checked
 ↓
Role not permitted
 ↓
403 Forbidden
```

---

# 85. "Why Express.js?"

Good answer:

> I used Express because it provides a lightweight structure for building Node.js APIs, especially routing and middleware. Its middleware architecture made it straightforward to implement authentication, authorization, validation, and centralized error handling.

---

# 86. "Why Node.js?"

> Node.js works well for I/O-heavy applications because of its event-driven, non-blocking architecture. It also allowed me to use JavaScript across the application and has a large ecosystem through npm.

---

# 87. "Why REST API?"

> REST provides a simple resource-oriented interface between the frontend and backend. It uses standard HTTP methods and status codes, making the APIs predictable and easy to consume.

---

# 88. "Why bcrypt?"

> Passwords should not be stored in plain text. bcrypt is designed for password hashing and includes a salt, making password hashes resistant to common attacks such as precomputed rainbow-table attacks.

---

# 89. "Why JWT?"

> JWT provides a convenient token-based authentication mechanism for APIs. After login, the server issues a signed token and subsequent requests include it so the backend can verify the user's identity without relying on a server-side session for every request.

---

# 90. "Where should JWT be stored?"

This is a nuanced security question.

For browser applications, **HttpOnly, Secure cookies** are often preferred for refresh/session-like tokens because JavaScript cannot directly read an HttpOnly cookie, reducing exposure to token theft through certain XSS scenarios.

If tokens are stored in browser storage such as `localStorage`, JavaScript can access them, so an XSS vulnerability can expose them.

Interview answer:

> Token storage depends on the application architecture and threat model. For browser-based applications, HttpOnly Secure cookies are commonly used for sensitive session or refresh tokens because they reduce JavaScript access. If using cookies, CSRF protections also need to be considered.

---

# 91. Cookies

Cookie:

```text
Browser
  ↓
Cookie
  ↓
Server
```

Important flags:

```text
HttpOnly
Secure
SameSite
```

### HttpOnly

JavaScript cannot directly access it.

### Secure

Sent only over HTTPS.

### SameSite

Controls cross-site cookie behavior and helps mitigate CSRF.

---

# 92. CSRF

**CSRF = Cross-Site Request Forgery**

An attacker attempts to cause a user's browser to send an unwanted authenticated request.

Especially relevant when authentication relies on cookies.

Defenses can include:

```text
SameSite cookies
CSRF tokens
Origin/Referer checks where appropriate
```

---

# 93. XSS

**XSS = Cross-Site Scripting**

Attacker injects malicious JavaScript into a web application.

Potential consequences include:

```text
Stealing accessible tokens
Reading sensitive page data
Performing actions as the user
```

Defense:

```text
Input validation
Output encoding
Content Security Policy
Secure frameworks
Avoid unsafe HTML injection
```

---

# 94. Important Backend Security Checklist

For interviews:

```text
✓ Password hashing
✓ JWT expiration
✓ Secure secrets
✓ HTTPS
✓ Input validation
✓ Authorization
✓ RBAC
✓ CORS configuration
✓ Rate limiting
✓ Security headers
✓ Error handling
✓ Avoid sensitive error messages
✓ Database access controls
✓ Logging and monitoring
```

---

# 95. Common Interview Questions

Prepare these **very thoroughly**:

### Node.js

1. What is Node.js?
2. Why Node.js?
3. What is V8?
4. What is event loop?
5. What is non-blocking I/O?
6. Node.js is single-threaded—what does that mean?
7. Callback vs Promise?
8. Promise vs async/await?
9. What is npm?
10. What is package.json?

### Express

11. What is Express?
12. What is middleware?
13. What is `next()`?
14. What are different types of middleware?
15. How does routing work?
16. What is `express.Router()`?
17. How do you handle errors?
18. What is `req.params`?
19. What is `req.query`?
20. What is `req.body`?

### REST

21. What is REST?
22. What makes an API RESTful?
23. GET vs POST?
24. PUT vs PATCH?
25. DELETE?
26. What are HTTP status codes?
27. 401 vs 403?
28. What is idempotency?
29. What is statelessness?
30. What is API versioning?

### Authentication

31. Authentication vs authorization?
32. How does JWT work?
33. JWT structure?
34. How do you verify JWT?
35. What is bcrypt?
36. Why hash passwords?
37. Access token vs refresh token?
38. JWT vs session?
39. What happens when JWT expires?
40. Where should tokens be stored?

### RBAC

41. What is RBAC?
42. How did you implement RBAC?
43. Authentication vs authorization middleware?
44. Why return 403?
45. How would you implement multiple roles?
46. RBAC vs permission-based access control?

### Security

47. What is CORS?
48. What is CSRF?
49. What is XSS?
50. What is rate limiting?
51. What is Helmet?
52. Why use environment variables?
53. How do you validate API input?
54. How do you protect APIs?

---

# 96. 10 Questions You Absolutely Must Master

If the interviewer has limited time, prioritize these:

### ⭐ 1. Explain Node.js architecture.

```text
V8
 ↓
Event Loop
 ↓
Non-blocking I/O
 ↓
Callbacks / Promises
```

### ⭐ 2. Explain Express middleware.

```text
Request
 ↓
Middleware
 ↓
Middleware
 ↓
Controller
 ↓
Response
```

### ⭐ 3. Explain REST API.

```text
Resources + HTTP methods + stateless communication
```

### ⭐ 4. Explain JWT authentication.

```text
Login
 ↓
Verify credentials
 ↓
Generate JWT
 ↓
Client sends JWT
 ↓
Verify JWT
 ↓
Identify user
```

### ⭐ 5. Authentication vs authorization.

```text
Authentication → Who are you?
Authorization → What can you do?
```

### ⭐ 6. 401 vs 403.

```text
401 → Not authenticated
403 → Authenticated but not allowed
```

### ⭐ 7. Explain RBAC.

```text
User
 ↓
Role
 ↓
Permission
```

### ⭐ 8. Explain bcrypt.

```text
Password
 ↓
bcrypt hash
 ↓
Database
```

Login:

```text
Password
 ↓
bcrypt.compare()
 ↓
Match / reject
```

### ⭐ 9. Explain your backend architecture.

```text
Route
 ↓
Middleware
 ↓
Controller
 ↓
Service
 ↓
Model
 ↓
Database
```

### ⭐ 10. Explain your project end-to-end.

```text
Client
 ↓
Express REST API
 ↓
JWT
 ↓
RBAC
 ↓
Controller
 ↓
MongoDB
 ↓
Inngest
 ↓
Gemini
 ↓
Database update
 ↓
Email notification
```

---

# 97. One-Page Mental Model

Before your interview, remember this:

```text
                    BACKEND
                       │
        ┌──────────────┴──────────────┐
        │                             │
     NODE.JS                       EXPRESS
        │                             │
   Runtime                     Web Framework
        │                             │
   Event Loop                    Routing
   Async I/O                    Middleware
        │                             │
        └──────────────┬──────────────┘
                       ↓
                    REST API
                       ↓
                  HTTP Request
                       ↓
               Authentication
                       ↓
                     JWT
                       ↓
                Authorization
                       ↓
                     RBAC
                       ↓
                  Validation
                       ↓
                  Controller
                       ↓
                    Service
                       ↓
                    Model
                       ↓
                   Database
                       ↓
                  HTTP Response
```

For **your interview specifically**, I would learn these in this order:

**1. Node.js fundamentals → 2. Event Loop/async → 3. Express → 4. Middleware → 5. REST APIs → 6. HTTP/status codes → 7. MongoDB/Mongoose → 8. Authentication → 9. JWT → 10. bcrypt → 11. Authorization → 12. RBAC → 13. API security → 14. Your project's backend architecture.**

The **highest-value preparation** is not memorizing definitions—it is being able to take one API such as `POST /api/tickets` and explain **exactly what happens from the frontend request all the way to MongoDB and back**, including JWT authentication, RBAC, validation, controller logic, database operation, errors, and response.
