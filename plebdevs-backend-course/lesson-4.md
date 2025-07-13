## Lesson Slides
- [Lesson 4 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-4.pdf)
- [Lesson 4 link to slides](https://docs.google.com/presentation/d/1M3wmeBco_Z31ahhSbSSx0yyUUpqDytj_A62eZNan-h8/edit?usp=sharing)

# Lesson 4: Express Middleware - Securing & Enhancing Your Server

## Overview

Welcome to Lesson 4 of the PlebDevs Backend Course! Today we're diving deeper into Express middleware - a crucial concept for building robust, secure, and feature-rich backend applications.

### What You'll Learn

- **Understanding Middleware**: What it is and how it works in Express
- **Security Enhancement**: Protecting your server from common attacks
- **Logging & Monitoring**: Setting up request/response logging
- **Development Tools**: Using nodemon for automatic server restarts
- **Practical Implementation**: Building and testing middleware in our Pleb Wallet backend

### Prerequisites

- Completed Lessons 1-3 of the PlebDevs Backend Course
- Basic understanding of Express.js and Node.js
- Your pleb-wallet-backend project from previous lessons

---

## What is Middleware?

Middleware is a function that sits between the request and response objects and can modify or intercept either one before they reach their destination. Think of it as a series of checkpoints that every request passes through before reaching your endpoint logic.

### Key Characteristics

- **Sits in the middle**: Between request and response in the request-response cycle
- **Can modify data**: Transform requests or responses
- **Can terminate cycles**: Send responses or pass control to next middleware
- **Order matters**: Executed in the order they're defined

### Common Use Cases

- **Authentication**: Verify user identity and permissions
- **Logging**: Track incoming requests and outgoing responses
- **Error Handling**: Catch and handle errors gracefully
- **Compression**: Reduce response size for better performance
- **CORS**: Enable cross-origin requests
- **Rate Limiting**: Prevent abuse and DoS attacks
- **Caching**: Store frequently requested data
- **CSRF Protection**: Prevent cross-site request forgery
- **Custom Functionality**: Add any custom logic to your request pipeline

---

## How Express Middleware Works

### Middleware Function Structure

```javascript
app.use((req, res, next) => {
  // Your middleware logic here
  next(); // Pass control to next middleware
});
```

### Parameters

- **`req`**: The request object containing client data
- **`res`**: The response object for sending data back
- **`next`**: Function that passes control to the next middleware

### Example: Multiple Middleware Functions

```javascript
// Middleware function 1
app.use((req, res, next) => {
  console.log('Middleware 1');
  next(); // Pass control to next middleware
});

// Middleware function 2  
app.use((req, res, next) => {
  console.log('Middleware 2');
  res.send('Hello World!'); // Terminates the cycle
});

// Middleware function 3
app.use((req, res, next) => {
  console.log('Middleware 3');
  // This won't execute - previous middleware sent response
});
```

---

## Setting Up Middleware in Our Pleb Wallet Backend

### Step 1: Install Middleware Packages

First, let's install the essential middleware packages we'll be using:

```bash
npm install helmet morgan cors express-rate-limit
```

### The Middleware We're Adding

#### 🛡️ Helmet
- **Purpose**: Security enhancement through HTTP headers
- **Protection**: XSS attacks, clickjacking, and other common vulnerabilities
- **Implementation**: Sets various security-related HTTP headers

#### 📝 Morgan
- **Purpose**: HTTP request/response logging
- **Benefits**: Debugging, monitoring, and analytics
- **Formats**: Multiple logging formats available

#### 🌐 CORS
- **Purpose**: Cross-Origin Resource Sharing
- **Function**: Allows web pages from different domains to access your API
- **Security**: Configurable access controls

#### ⚡ Express-Rate-Limit
- **Purpose**: Rate limiting to prevent abuse
- **Protection**: DoS attacks, brute-force attempts
- **Configuration**: Customizable limits per IP address

### Step 2: Import Middleware

Update your `index.js` file with the new imports:

```javascript
const express = require("express");
const helmet = require("helmet");
const morgan = require("morgan");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
const usersRouter = require("./routers/usersRouter");
const lightningRouter = require("./routers/lightningRouter");
```

### Step 3: Initialize Basic Middleware

Add these middleware functions **before** `server.use(express.json())`:

```javascript
// Create Express server instance
const server = express();

// Security middleware - sets various HTTP headers
server.use(helmet());

// Logging middleware - logs all requests in 'common' format
server.use(morgan("common"));

// CORS middleware - enables cross-origin requests
server.use(cors());
```

### Step 4: Add Rate Limiting

```javascript
// Rate limiting middleware - prevents abuse
server.use(
  rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
  })
);
```

### Complete Updated index.js

```javascript
const express = require("express");
const helmet = require("helmet");
const morgan = require("morgan");
const cors = require("cors");
const rateLimit = require("express-rate-limit");
const usersRouter = require("./routers/usersRouter");
const lightningRouter = require("./routers/lightningRouter");

const server = express();

// Security middleware
server.use(helmet());

// Logging middleware
server.use(morgan("common"));

// CORS middleware
server.use(cors());

// Rate limiting middleware
server.use(
  rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100, // limit each IP to 100 requests per windowMs
  })
);

// JSON parsing middleware
server.use(express.json());

// Routes
server.get("/", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});

server.use("/users", usersRouter);
server.use("/lightning", lightningRouter);

const PORT = process.env.PORT || 5500;

server.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
});
```

---

## Development Tool: Nodemon

### What is Nodemon?

Nodemon is a utility that automatically restarts your Node.js server whenever file changes are detected. This saves you from manually stopping and starting your server during development.

### Installation

```bash
npm install nodemon
```

### Setup

Update your `package.json` scripts:

```json
{
  "name": "pleb-wallet-backend",
  "version": "1.0.0",
  "description": "Lightning wallet backend for PlebDevs course",
  "main": "index.js",
  "scripts": {
    "start": "nodemon index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2",
    "helmet": "^6.0.1",
    "morgan": "^1.10.0",
    "cors": "^2.8.5",
    "express-rate-limit": "^6.7.0"
  }
}
```

### Usage

Start your server with automatic restart capability:

```bash
npm run start
```

You should see output similar to:
```
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
Server listening on port 5500
```

---

## Testing Your Middleware

### 1. Testing Morgan (Logging)

Make a request to your server using any HTTP client (browser, Postman, etc.):

```
GET http://localhost:5500/
```

You should see log output in your terminal:
```
::1 - - [23/Mar/2024:10:30:45 +0000] "GET / HTTP/1.1" 200 24
```

This shows:
- IP address
- Timestamp
- HTTP method and path
- Status code
- Response size

### 2. Testing Helmet (Security Headers)

Check the response headers from your last request. You should see additional security headers like:
- `X-XSS-Protection`
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Strict-Transport-Security`

### 3. Testing CORS

CORS will allow cross-origin requests. You can test this by making requests from different domains or using fetch from a browser console on a different website.

---

## Understanding Denial of Service (DoS) Attacks

### What is a DoS Attack?

A Denial of Service attack attempts to overwhelm a server with traffic, making it unavailable to legitimate users. Common methods include:

- **Request Flooding**: Sending high volumes of requests
- **Resource Exhaustion**: Consuming server resources
- **Bandwidth Consumption**: Filling network capacity

### The Pattern

Many cyber attacks follow a simple pattern: **repetitive or looping behavior**. Attackers use automated tools to repeatedly perform actions, often just a simple for loop!

### Our Defense: Rate Limiting

Rate limiting protects against these attacks by:
- Limiting requests per IP address
- Setting time windows for request limits
- Automatically blocking excessive requests

---

## Testing Rate Limiting - Mini DoS Attack

Let's test our rate limiting by performing a controlled attack on our own server!

### The Attack Script

```javascript
const http = require('http');

function sendRequest() {
  const options = {
    host: 'localhost',
    port: 5500,
    path: '/',
    method: 'GET',
  };

  const req = http.request(options, (res) => {
    console.log(`Response status code: ${res.statusCode}`);
  });

  req.on('error', (e) => {
    console.error(`Request error: ${e.message}`);
  });

  req.end();
}

// Send a request every 100ms
setInterval(sendRequest, 100);
```

### Running the Attack

1. **Start your server** (if not already running):
   ```bash
   npm run start
   ```

2. **Open a new terminal** and enter Node.js REPL:
   ```bash
   node
   ```

3. **Paste the attack script** and press Enter

4. **Watch the magic happen**:
   - You'll see status codes start as `200` (success)
   - After 100 requests, they'll switch to `429` (rate limited)
   - Your server logs will show all the requests via Morgan

5. **Stop the attack** with `Ctrl+C`

### What You Should See

```
Response status code: 200
Response status code: 200
Response status code: 200
...
Response status code: 429
Response status code: 429
Response status code: 429
```

The `429` status code means "Too Many Requests" - your rate limiting is working!

---

## Key Takeaways

### Middleware Concepts
- **Functions between request and response**: Middleware sits in the request-response cycle
- **Order matters**: Middleware executes in the order it's defined
- **Can terminate or continue**: Either send a response or call `next()`
- **Modular functionality**: Each middleware handles a specific concern

### Security Benefits
- **Helmet**: Adds security headers to prevent common attacks
- **Rate Limiting**: Prevents abuse and DoS attacks
- **CORS**: Controls cross-origin access
- **Logging**: Provides visibility into server activity

### Development Improvements
- **Nodemon**: Automatic server restarts during development
- **Better debugging**: Comprehensive logging with Morgan
- **Security by default**: Protection against common vulnerabilities

---

## What's Next?

In the next lesson, we'll dive deeper into custom middleware by building our own authentication middleware. We'll learn how to:

- Create custom middleware functions
- Implement JWT-based authentication
- Protect routes with authentication middleware
- Handle authentication errors gracefully

---

## Resources

### Official Documentation
- [Express Middleware Guide](https://expressjs.com/en/guide/using-middleware.html)
- [Helmet.js Documentation](https://helmetjs.github.io/)
- [Morgan Documentation](https://github.com/expressjs/morgan)
- [CORS Documentation](https://github.com/expressjs/cors)

### Additional Learning
- [Express.js Middleware Tutorial](https://www.tutorialspoint.com/expressjs/expressjs_middleware.htm)
- [Express.js Fundamentals - Middleware Explained](https://www.youtube.com/watch?v=9HOem0amlyg)
- [Security Best Practices](https://expressjs.com/en/advanced/best-practice-security.html)

---

## Troubleshooting

### Common Issues

**Server won't start after adding middleware:**
- Check that all packages are installed: `npm install`
- Verify middleware is added before routes
- Ensure proper syntax in middleware setup

**Rate limiting not working:**
- Verify the rate limit configuration
- Check that requests are coming from the same IP
- Ensure rate limiting middleware is before other middleware

**Headers not showing security additions:**
- Confirm Helmet is installed and configured
- Check that Helmet middleware is being used
- Verify in browser dev tools or HTTP client

**Nodemon not restarting:**
- Check that the script is correctly configured in package.json
- Verify file changes are being saved
- Try manually restarting with `rs` in the terminal

---

*Great job completing Lesson 4! You've successfully enhanced your server with essential middleware for security, logging, and development efficiency. Your Pleb Wallet backend is becoming more robust and production-ready with each lesson.* 