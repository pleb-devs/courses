## Lesson Slides
- [Lesson 3 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-3.pdf)
- [Lesson 3 link to slides](https://docs.google.com/presentation/d/1l3ii5VtdZcSTdFlGdczx3CD4Vg4qn_x2JfvcjFCt7JE/edit?usp=sharing)

# Lesson 3: Learn Express Router

## Overview
Welcome back to the PlebDevs Backend Course! In this lesson, we'll dive deep into Express Router and learn how to structure our API using RESTful principles. We'll build the foundation for our pleb-wallet-backend by creating organized, modular routes that will handle user authentication and Lightning Network operations.

## Learning Objectives
By the end of this lesson, you will be able to:
- Understand REST API principles and conventions
- Use Express Router to organize routes into separate files
- Create modular, maintainable API endpoints
- Handle request parameters and body data
- Test API endpoints using Insomnia
- Structure a professional backend application

## Key Concepts

### REST APIs
**REST** (Representational State Transfer) is a widely adopted philosophy for building APIs that can communicate in a structured way. Think of REST as a common language that developers use to build API routes and endpoints.

REST provides standard methods:
- **GET**: Retrieve data
- **POST**: Create new data
- **PUT**: Update existing data
- **DELETE**: Remove data

### Express Router
Express Router is a built-in middleware function that allows you to:
- Group and organize routes into separate files
- Keep code modular and organized
- Make applications easier to maintain and scale
- Define multiple routes with specific URL patterns and HTTP methods

## Planning Our API Structure

Before we start coding, let's plan what our pleb-wallet-backend needs:

### Data Categories
1. **Users**
   - Signup/authentication flows
   - Admin rights for wallet spending
   - Authorized users can create invoices
   - Non-logged-in users can view balance only

2. **Lightning**
   - Create invoices (authenticated users)
   - Pay invoices (admin only)
   - Get wallet balance (anyone)
   - Save/retrieve paid and received invoices

### API Routes Structure

#### Root Route
```
GET / - Welcome message
```

#### Users Routes (`/users`)
```
GET /users - Get all users
POST /users/register - Register a new user
POST /users/login - Login existing user
PUT /users/:id - Update user by ID
DELETE /users/:id - Delete user by ID
GET /users/user - Get user by username
```

#### Lightning Routes (`/lightning`)
```
GET /lightning/invoices - Get all invoices
POST /lightning/invoice - Create an invoice
POST /lightning/pay - Pay an invoice
GET /lightning/balance - Get wallet balance
```

## Implementation

### Step 1: Create Router Files

First, create the routers folder and files:

```bash
mkdir routers
touch routers/usersRouter.js
touch routers/lightningRouter.js
```

Your project structure should now look like this:
```
pleb-wallet-backend/
├── routers/
│   ├── usersRouter.js
│   └── lightningRouter.js
├── index.js
├── package.json
└── ...
```

### Step 2: Build Users Router

Create `routers/usersRouter.js`:

```javascript
const router = require("express").Router();

// GET all users
router.get("/", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});

// GET user by their username
router.get("/user", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});

// POST a user to register
router.post("/register", (req, res) => {
  const user = req.body;
  
  console.log(user);
  
  res.status(201).json({ message: "I'm alive!" });
});

// POST a user to login
router.post("/login", (req, res) => {
  const user = req.body;
  
  console.log(user);
  
  res.status(200).json({ message: "I'm alive!" });
});

// PUT a user to update them by their id
router.put("/:id", (req, res) => {
  const id = req.params.id;
  const user = req.body;
  
  console.log(id, user);
  
  res.status(200).json({ message: "I'm alive!" });
});

// DELETE a user by their id
router.delete("/:id", (req, res) => {
  const id = req.params.id;
  
  console.log(id);
  
  res.status(200).json({ message: "I'm alive!" });
});

// Export our router so we can use it in index.js
module.exports = router;
```

### Step 3: Build Lightning Router

Create `routers/lightningRouter.js`:

```javascript
const router = require("express").Router();

// GET lightning wallet balance
router.get("/balance", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});

// GET all invoices from the database
router.get("/invoices", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});

// POST required info to create an invoice
router.post("/invoice", (req, res) => {
  const { value, memo } = req.body;
  
  console.log(value, memo);
  
  res.status(200).json({ message: "I'm alive!" });
});

// POST an invoice to pay
router.post("/pay", (req, res) => {
  const { payment_request } = req.body;
  
  console.log(payment_request);
  
  res.status(200).json({ message: "I'm alive!" });
});

// Export our router so we can use it in index.js
module.exports = router;
```

### Step 4: Update index.js

Add the router imports and middleware to your `index.js`:

```javascript
const express = require("express");
const usersRouter = require("./routers/usersRouter");
const lightningRouter = require("./routers/lightningRouter");

// Create a new instance of the Express server
const server = express();

// Use the built-in JSON middleware to parse incoming JSON requests
server.use(express.json());

// Set up a route to handle GET requests to the root path
server.get("/", (req, res) => {
  // Send a JSON response with a "message" property set to "I'm alive!"
  res.status(200).json({ message: "I'm alive!" });
});

// Add our routers before server.listen()
server.use("/users", usersRouter);
server.use("/lightning", lightningRouter);

// Set the server to listen on the provided port, or 5500 if no port is specified
const PORT = process.env.PORT || 5500;

server.listen(PORT, () => {
  // Log a message to the console when the server starts listening
  console.log(`Server listening on port ${PORT}`);
});
```

## Understanding Request Parameters

Express allows you to define routes with parameters using a colon (`:`) followed by the parameter name.

```javascript
// Route with parameter
router.get("/:id", (req, res) => {
  const id = req.params.id; // Extract the id from the URL
  // Handle the request...
});
```

Examples:
- `/users/123` - id would be "123"
- `/users/john-doe` - id would be "john-doe"

## Testing Your API

### Start Your Server
```bash
node index.js
```

### Test Endpoints with Insomnia

Set up the following requests in Insomnia:

#### GET Requests
- `GET http://localhost:5500/` - Root endpoint
- `GET http://localhost:5500/users` - Get all users
- `GET http://localhost:5500/users/user` - Get user by username
- `GET http://localhost:5500/lightning/invoices` - Get all invoices
- `GET http://localhost:5500/lightning/balance` - Get wallet balance

#### POST Requests with JSON Body

**Register User:**
```
POST http://localhost:5500/users/register
Content-Type: application/json

{
  "username": "testuser",
  "password": "testpass"
}
```

**Login User:**
```
POST http://localhost:5500/users/login
Content-Type: application/json

{
  "username": "testuser",
  "password": "testpass"
}
```

**Create Invoice:**
```
POST http://localhost:5500/lightning/invoice
Content-Type: application/json

{
  "value": 1000,
  "memo": "Test invoice"
}
```

**Pay Invoice:**
```
POST http://localhost:5500/lightning/pay
Content-Type: application/json

{
  "payment_request": "lnbc1000n1..."
}
```

#### PUT/DELETE Requests with Parameters

**Update User:**
```
PUT http://localhost:5500/users/3
Content-Type: application/json

{
  "username": "updateduser"
}
```

**Delete User:**
```
DELETE http://localhost:5500/users/1
```

## Key Takeaways

### Express Router Benefits
- **Modular Organization**: Keep related routes together
- **Maintainability**: Easy to find and update specific functionality
- **Scalability**: Add new features without cluttering main file
- **Reusability**: Share middleware and handlers across routes

### RESTful Design Principles
- Use appropriate HTTP methods for different operations
- Structure URLs to represent resources logically
- Maintain consistent naming conventions
- Keep endpoints predictable and intuitive

### DRY Principle
**Don't Repeat Yourself** - By organizing code into routers, we avoid duplicating logic and create reusable components.

## What's Next?

In the next lesson, we'll:
- Connect our API to a real database
- Implement proper user authentication
- Add data validation and error handling
- Begin integrating Lightning Network functionality

## Resources

- [Express Routing Official Documentation](https://expressjs.com/en/guide/routing.html)
- [Express Router Tutorial](https://scotch.io/tutorials/learn-to-use-the-new-router-in-expressjs-4)
- [RESTful Routing in Express](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes#RESTful_routing_in_Express)
- [REST APIs: How They Work](https://blog.hubspot.com/website/what-is-rest-api)
- [Express Request Parameters](https://www.educative.io/answers/what-is-reqparams-in-expressjs)

## Review

In this lesson, we covered:

1. **Express Routers**: How to create and use routers to organize routes for specific parts of our application
2. **RESTful APIs**: Creating a standardized interface for interacting with application resources using HTTP methods
3. **Request Parameters**: Handling dynamic parameters in Express routes for flexible endpoint design
4. **API Testing**: Using Insomnia to test our endpoints and verify functionality

We now have a solid foundation for our pleb-wallet-backend with organized, testable endpoints ready for the next phase of development! 