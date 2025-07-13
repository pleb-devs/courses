## Lesson Slides
- [Lesson 11 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-11.pdf)
- [Lesson 11 link to slides](https://docs.google.com/presentation/d/1x1oJMGHM_4nWfm4RGJVgnEbUW6QSvuok2ND1ToV0ppo/edit?usp=sharing)

# Lesson 11: Connecting the API and Database

## Overview

Welcome to Lesson 11! This is where everything comes together. We've built our server, created our API endpoints, set up our database with migrations and seeds, but they haven't been talking to each other yet. Today we're going to bridge that gap and create a fully functional backend system.

**What We'll Cover:**
- Understanding database models in Knex.js
- Creating user models for database interaction
- Updating user API endpoints to use the database
- Creating invoice models for Lightning operations
- Updating invoice endpoints with database integration
- Testing the complete system with Postman and Polar
- Debugging common database integration issues

**Note:** This lesson represents the culmination of our backend development! By the end, you'll have a fully functional Lightning wallet backend that connects your API to your database seamlessly.

## The Big Picture: Our Complete Setup

Before we dive into the code, let's understand where we are in our development journey. We've completed all the foundational pieces:

- ✅ **Database Setup:** SQLite with proper schema
- ✅ **Knex Configuration:** Migrations and seeds running
- ✅ **API Endpoints:** Express routes with middleware
- ✅ **Lightning Integration:** LND connection established
- ✅ **Authentication:** JWT-based user auth

Now we need to connect these systems together with **database models** - the bridge between our API and our database.

## What Are Database Models in Knex.js?

**Database models are the defined structures and helper functions that provide an interface for querying and manipulating data stored in your database tables.**

Think of models as translators between your JavaScript code and your SQL database. They provide a clean, consistent way to:

### Core Responsibilities

**Data Representation:**
- Each model typically represents one table in your database
- A `User` model represents the `users` table
- An `Invoice` model represents the `invoices` table

**Query Interface:**
- Models provide methods for common database operations
- `findAll()` - retrieve all records
- `findByUsername()` - find specific records
- `create()` - insert new records
- `update()` - modify existing records
- `delete()` - remove records

**Abstraction Layer:**
- Hide complex SQL queries behind simple JavaScript methods
- Provide consistent error handling
- Enable code reuse across different endpoints

### Why Use Models?

**Separation of Concerns:**
```javascript
// Without models (bad):
router.get('/users', (req, res) => {
  db('users').select('*').then(users => {
    // Database logic mixed with route logic
  });
});

// With models (good):
router.get('/users', (req, res) => {
  User.findAll().then(users => {
    // Clean separation of concerns
  });
});
```

**Reusability:**
Models can be used across multiple endpoints, reducing code duplication.

**Maintainability:**
Changes to database structure only require updates in one place.

## Setting Up the Database Models Directory

Let's start by organizing our code properly:

### Creating the Models Directory

```bash
mkdir db/models
```

Your `db` folder should now look like this:
```
db/
├── dbConfig.js
├── dev.sqlite3
├── migrations/
├── seeds/
└── models/        # <- New directory
```

### Understanding dbConfig.js

Before we create models, let's remember what `dbConfig.js` does:

```javascript
const knex = require("knex");
const config = require("../knexfile");

// Determine environment (development or production)
const env = process.env.NODE_ENV || "development";

// Initialize Knex with the appropriate configuration
const db = knex(config[env]);

module.exports = db;
```

This file:
1. Imports the Knex library
2. Loads our configuration from `knexfile.js`
3. Determines the current environment
4. Creates a database connection
5. Exports the connection for use in models

## Creating the User Model

Let's create our first model for user operations.

### Create `db/models/user.js`

```javascript
// First, we require our configured instance of knex from the dbConfig.js file.
const db = require("../dbConfig");

// We export an object with several methods, each representing a different database operation
module.exports = {
  // The findAll method retrieves all records from the 'users' table
  findAll: () => {
    return db("users");
  },
  
  // The findByUsername method retrieves the first record where username matches
  findByUsername: (username) => {
    return db("users").where({ username }).first();
  },
  
  // The create method inserts a new record into the 'users' table
  create: (user) => {
    return db("users").insert(user).returning("*");
  },
  
  // The update method finds a user by id and updates their record
  update: (id, user) => {
    return db("users").where({ id }).update(user).returning("*");
  },
  
  // The delete method removes a user record by id
  delete: (id) => {
    return db("users").where({ id }).del();
  },
};
```

### Breaking Down the User Model

**Method Patterns:**
Each method returns a Knex query, which returns a Promise. This allows us to use `.then()` and `.catch()` in our endpoints.

**findAll():**
- Simple query to get all users
- Returns: `Promise<User[]>`

**findByUsername(username):**
- Uses `.where()` to filter by username
- Uses `.first()` to get only one result
- Returns: `Promise<User | undefined>`

**create(user):**
- Uses `.insert()` to add a new record
- Uses `.returning("*")` to get the created user back
- Returns: `Promise<User>`

**update(id, user):**
- Uses `.where({ id })` to find the specific user
- Uses `.update(user)` to apply changes
- Returns: `Promise<User>`

**delete(id):**
- Uses `.where({ id })` to find the user
- Uses `.del()` to remove the record
- Returns: `Promise<number>` (number of deleted rows)

## Updating User Endpoints

Now let's update our user endpoints to use the database models.

### Adding Required Imports

First, update `routers/usersRouter.js` with the necessary imports:

```javascript
const User = require("../db/models/user");
const authenticate = require("./middleware/authenticate.js");
const authenticateAdmin = require("./middleware/authenticateAdmin.js");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");
```

### Updating GET All Users Endpoint

Replace the existing GET `/` endpoint:

```javascript
// GET all users
// Before processing the request, we apply the 'authenticateAdmin' middleware
router.get("/", authenticateAdmin, (req, res) => {
  // Call the 'findAll' method from our User model
  User.findAll()
    .then((users) => {
      // If successful, send back status 200 with the users
      res.status(200).json(users);
    })
    .catch((err) => {
      // If error occurs, send back status 500 with the error
      res.status(500).json(err);
    });
});
```

**Key Changes:**
- Added `authenticateAdmin` middleware (only admins can see all users)
- Replaced mock data with `User.findAll()`
- Added proper error handling with `.catch()`

### Updating GET User by Username Endpoint

Replace the existing GET `/user` endpoint:

```javascript
// GET user by their username
// Using 'authenticate' middleware to verify the client's JWT token
router.get("/user", authenticate, async (req, res) => {
  // Get the JWT from the 'authorization' header
  const token = req.headers.authorization;
  // Retrieve the secret key for JWT verification from environment variables
  const secret = process.env.JWT_SECRET;

  // Use the 'verify' method to decode the token
  jwt.verify(token, secret, (err, decodedToken) => {
    // If error occurred during token decoding, respond with 401
    if (err) {
      res.status(401).json({ message: "Error decoding token", Error: err });
    }
    
    // If token was successfully decoded, find user by username
    User.findByUsername(decodedToken.username)
      .then((user) => {
        // If user found, respond with status 200 and user data
        res.status(200).json(user);
      })
      .catch((err) => {
        // If error occurred, respond with status 500 and error
        res.status(500).json(err);
      });
  });
});
```

**Key Changes:**
- Extract username from JWT token
- Use `User.findByUsername()` to get user from database
- Handle both JWT errors and database errors

### Updating Register Endpoint

Replace the existing POST `/register` endpoint:

```javascript
// POST a user to register
router.post("/register", (req, res) => {
  // Use bcrypt to hash the password before storing in database
  // The '14' is the cost factor for hashing complexity
  const hash = bcrypt.hashSync(req.body.password, 14);

  // Replace the plain text password with the hashed version
  req.body.password = hash;

  // Create new user record in database
  User.create(req.body)
    .then((user) => {
      // If successful, respond with status 201 (Created) and user data
      res.status(201).json({ data: user });
    })
    .catch((err) => {
      // If error occurs, respond with status 500 and error
      res.status(500).json({ error: err });
    });
});
```

**Key Changes:**
- Hash password with bcrypt before saving
- Use `User.create()` to insert into database
- Changed status code to 201 (Created) for proper REST semantics

### Updating Login Endpoint

Replace the existing POST `/login` endpoint:

```javascript
// POST a user to login
router.post("/login", (req, res) => {
  // Extract username and password from request body
  const { username, password } = req.body;

  // Find user by username in database
  User.findByUsername(username)
    .then((user) => {
      // Check if user exists and password matches
      if (user && bcrypt.compareSync(password, user.password)) {
        // If valid, generate JWT token
        const token = generateToken(user);
        // Respond with success message, token, and user data
        res.status(200).json({ 
          message: `Welcome ${user.username}!`, 
          token, 
          user 
        });
      } else {
        // If invalid credentials, respond with 401
        res.status(401).json({ message: "Invalid credentials" });
      }
    })
    .catch((err) => {
      // If database error, respond with 500
      res.status(500).json({ error: err });
    });
});
```

**Key Changes:**
- Removed mock user data
- Use `User.findByUsername()` to get real user from database
- Compare provided password with stored hash using bcrypt
- Generate JWT token with actual user data

### Updating Update User Endpoint

Replace the existing PUT `/:id` endpoint:

```javascript
// PUT a user to update them
// Using 'authenticateAdmin' middleware for admin-only access
router.put("/:id", authenticateAdmin, (req, res) => {
  // Call the 'update' method with user ID and request body
  User.update(req.params.id, req.body)
    .then((user) => {
      // If successful, respond with status 200 and updated user
      res.status(200).json(user);
    })
    .catch((err) => {
      // If error occurs, respond with status 500 and error
      res.status(500).json(err);
    });
});
```

**Key Changes:**
- Use `req.params.id` to get user ID from URL
- Use `User.update()` to modify database record
- Only update fields provided in request body

### Updating Delete User Endpoint

Replace the existing DELETE `/:id` endpoint:

```javascript
// DELETE a user
router.delete("/:id", authenticateAdmin, (req, res) => {
  // Call the 'delete' method with user ID from URL parameters
  User.delete(req.params.id)
    .then((result) => {
      // If successful, respond with status 200 and success message
      res.status(200).json({ message: "User deleted successfully" });
    })
    .catch((err) => {
      // If error occurs, respond with status 500 and error
      res.status(500).json(err);
    });
});
```

**Key Changes:**
- Use `User.delete()` to remove user from database
- Return success message instead of user data

## Updating Authentication Middleware

Our middleware also needs to be updated to use the database instead of mock data.

### Update `routers/middleware/authenticateAdmin.js`

```javascript
const jwt = require("jsonwebtoken");
const User = require("../../db/models/user");

module.exports = (req, res, next) => {
  // Extract token from request header
  const token = req.headers.authorization;
  
  // Get JWT secret and admin key from environment variables
  const secret = process.env.JWT_SECRET || "Satoshi Nakamoto";
  const key = process.env.ADMIN_KEY || "1234";

  // If token is present, verify it
  if (token) {
    jwt.verify(token, secret, async (err, decodedToken) => {
      // If token verification fails, return 401
      if (err || !decodedToken) {
        res.status(401).json({ message: "Error with your verification" });
      } else {
        // If token is valid, find user in database
        const user = await User.findByUsername(decodedToken.username);
        
        // Extract admin key from user object
        const adminKey = user?.adminKey?.toString() ?? "";
        
        // Check if user's admin key matches environment admin key
        if (adminKey !== key) {
          // If admin key doesn't match, return 401
          res.status(401).json({ message: "Must be an admin" });
        } else {
          // If admin key matches, continue to next middleware/endpoint
          next();
        }
      }
    });
  } else {
    // If no token present, return 401
    res.status(401).json({ message: "No token!" });
  }
};
```

**Key Changes:**
- Removed mock user data
- Use `User.findByUsername()` to get real user from database
- Check actual admin key from user record
- Added proper error handling for database operations

## Testing User Endpoints

Let's test our updated user endpoints to ensure everything works correctly.

### Testing Flow with Postman

We'll go through a complete user lifecycle to test all endpoints:

1. **Register a new user** with admin privileges
2. **Login** to get a JWT token
3. **Get user by username** using the token
4. **Update the user** (requires re-login after username change)
5. **Get all users** (admin endpoint)
6. **Delete the user** (admin endpoint)

### Step 1: Register a New User

**POST** `http://localhost:3000/api/users/register`

```json
{
  "username": "testuser",
  "password": "testpass",
  "adminKey": "1234"
}
```

**Expected Response:**
```json
{
  "data": {
    "id": 3,
    "username": "testuser",
    "password": "$2a$14$...",
    "adminKey": "1234"
  }
}
```

### Step 2: Login

**POST** `http://localhost:3000/api/users/login`

```json
{
  "username": "testuser",
  "password": "testpass"
}
```

**Expected Response:**
```json
{
  "message": "Welcome testuser!",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 3,
    "username": "testuser",
    "password": "$2a$14$...",
    "adminKey": "1234"
  }
}
```

Copy the token for subsequent requests.

### Step 3: Get User by Username

**GET** `http://localhost:3000/api/users/user`

**Headers:**
```
Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Expected Response:**
```json
{
  "id": 3,
  "username": "testuser",
  "password": "$2a$14$...",
  "adminKey": "1234"
}
```

### Step 4: Update User

**PUT** `http://localhost:3000/api/users/3`

**Headers:**
```
Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Body:**
```json
{
  "username": "updateduser"
}
```

**Expected Response:**
```json
{
  "id": 3,
  "username": "updateduser",
  "password": "$2a$14$...",
  "adminKey": "1234"
}
```

### Step 5: Login Again (Username Changed)

Since we updated the username, we need a new token:

**POST** `http://localhost:3000/api/users/login`

```json
{
  "username": "updateduser",
  "password": "testpass"
}
```

### Step 6: Get All Users (Admin Endpoint)

**GET** `http://localhost:3000/api/users/`

**Headers:**
```
Authorization: [new-token-from-step-5]
```

**Expected Response:**
```json
[
  {
    "id": 1,
    "username": "Alice",
    "password": "$2a$14$...",
    "adminKey": "1234"
  },
  {
    "id": 2,
    "username": "Bob",
    "password": "$2a$14$...",
    "adminKey": null
  },
  {
    "id": 3,
    "username": "updateduser",
    "password": "$2a$14$...",
    "adminKey": "1234"
  }
]
```

### Step 7: Delete User

**DELETE** `http://localhost:3000/api/users/3`

**Headers:**
```
Authorization: [admin-token]
```

**Expected Response:**
```json
{
  "message": "User deleted successfully"
}
```

## Creating the Invoice Model

Now let's create the model for our Lightning invoice operations.

### Create `db/models/invoice.js`

```javascript
// First, we require our configured instance of knex from the dbConfig.js file.
const db = require("../dbConfig");

module.exports = {
  // The findAll method retrieves all records from the 'invoices' table
  findAll: () => {
    return db("invoices");
  },
  
  // The findOne method retrieves an invoice by payment_request
  findOne: (payment_request) => {
    return db("invoices").where({ payment_request }).first();
  },
  
  // The create method inserts a new invoice record
  create: (invoice) => {
    return db("invoices").insert(invoice).returning("*");
  },
  
  // The update method finds an invoice by payment_request and updates it
  update: (payment_request, invoice) => {
    return db("invoices")
      .where({ payment_request })
      .update(invoice)
      .returning("*");
  },
  
  // The delete method removes an invoice by id
  delete: (id) => {
    return db("invoices").where({ id }).del();
  },
};
```

### Invoice Model Specifics

**Why use `payment_request` instead of `id`?**
- Payment requests are unique identifiers in Lightning
- They're used to identify invoices across different systems
- More natural for Lightning operations than database IDs

**findOne(payment_request):**
- Lightning-specific lookup method
- Used to check if an invoice exists before updating
- Critical for the invoice event stream

## Updating Invoice Endpoints

Let's update our Lightning router to use the invoice model.

### Adding Invoice Import

Add to `routers/lightningRouter.js`:

```javascript
const Invoice = require("../db/models/invoice");
```

### Updating GET All Invoices Endpoint

Replace the existing GET `/invoices` endpoint:

```javascript
// GET all invoices from the database
router.get("/invoices", (req, res) => {
  // Call the 'findAll' method from our Invoice model
  Invoice.findAll()
    .then((invoices) => {
      // If successful, send back status 200 with invoices
      res.status(200).json(invoices);
    })
    .catch((err) => {
      // If error occurs, send back status 500 with error
      res.status(500).json(err);
    });
});
```

**Key Changes:**
- No authentication required (transaction history is public in our app)
- Use `Invoice.findAll()` to get all invoices from database
- Return all invoice data including payment status

### Updating Create Invoice Endpoint

Replace the existing POST `/invoice` endpoint:

```javascript
// POST required info to create an invoice
router.post("/invoice", authenticate, (req, res) => {
  // Extract value, memo, and user_id from request body
  const { value, memo, user_id } = req.body;
  
  // Call the createInvoice function with the extracted data
  createInvoice({ value, memo, user_id })
    .then((invoice) => {
      // If successful, respond with status 200 and invoice data
      res.status(200).json(invoice);
    })
    .catch((err) => {
      // If error occurs, respond with status 500 and error
      res.status(500).json(err);
    });
});
```

**Key Changes:**
- Extract `user_id` from request body
- Pass all required data to `createInvoice` function
- The function will handle both LND and database operations

## Updating Lightning Functions

Our Lightning functions in `lnd.js` need to be updated to work with the database.

### Updating createInvoice Function

Replace the existing `createInvoice` function in `lnd.js`:

```javascript
const createInvoice = async ({ value, memo, user_id }) => {
  // Use LND's addInvoice method to create a Lightning invoice
  const invoice = await lnd.services.Lightning.addInvoice({
    value: value,
    memo: memo,
  });

  // After creating the Lightning invoice, save it to our database
  await Invoice.create({
    payment_request: invoice.payment_request,
    value: value,
    memo: memo,
    settled: false,  // New invoice starts as unpaid
    send: false,     // This is an incoming invoice
    user_id: user_id,
  });

  // Return the Lightning invoice data
  return invoice;
};
```

**Key Changes:**
- Added database import: `const Invoice = require("./db/models/invoice");`
- Save invoice to database immediately after creating with LND
- Set appropriate initial values for database fields
- Link invoice to specific user with `user_id`

### Updating Invoice Event Stream

The invoice event stream listens for payment notifications from LND and updates our database accordingly.

Replace the existing `invoiceEventStream` function:

```javascript
const invoiceEventStream = async () => {
  await lnd.services.Lightning.subscribeInvoices({
    add_index: 0,
    settle_index: 0,
  })
    .on("data", async (data) => {
      // Only process settled (paid) invoices
      if (data.settled) {
        // Check if the invoice exists in our database
        const existingInvoice = await Invoice.findOne(data.payment_request);

        // If invoice exists, update it to reflect payment
        if (existingInvoice) {
          await Invoice.update(data.payment_request, {
            settled: data.settled,           // Mark as paid
            settle_date: data.settle_date,   // Record payment timestamp
          });
          console.log("Invoice updated in database:", data.payment_request);
        } else {
          console.log("Invoice not found in database:", data.payment_request);
        }
      }
    })
    .on("error", (err) => {
      console.log("Invoice event stream error:", err);
    });
};
```

**Key Changes:**
- Use `Invoice.findOne()` to check if invoice exists
- Use `Invoice.update()` to mark invoice as paid
- Handle case where invoice doesn't exist in database
- Added logging for debugging

### Updating Pay Invoice Endpoint

Replace the existing POST `/pay` endpoint:

```javascript
router.post("/pay", authenticateAdmin, async (req, res) => {
  // Extract payment_request and user_id from request body
  const { payment_request, user_id } = req.body;

  // Attempt to pay the invoice using LND
  const pay = await payInvoice({ payment_request });

  // If payment error occurred, return error
  if (pay.payment_error) {
    res.status(500).json(pay.payment_error);
  }

  // If payment was successful, save to database
  if (pay?.payment_route) {
    const payment = await Invoice.create({
      payment_request: payment_request,
      send: true,                                    // This is an outgoing payment
      value: pay.payment_route.total_amt,           // Total amount paid
      fees: pay.payment_route.total_fees,           // Routing fees
      settled: true,                                // Payment completed instantly
      settle_date: Date.now(),                      // Payment timestamp
      user_id: user_id                              // User who made the payment
    });

    // Return the payment record
    res.status(200).json(payment);
  }
});
```

**Key Changes:**
- Added `user_id` parameter to track who made the payment
- Save outgoing payment to database immediately
- Record all payment details including fees
- Set `send: true` to indicate outgoing payment

## Testing Invoice Endpoints

Let's test our updated invoice endpoints with both Postman and Polar.

### Prerequisites

1. **Start your server**: `npm run start`
2. **Start Polar**: Open Docker Desktop, then open Polar
3. **Start your network**: Click on your network and start it
4. **Verify connection**: Check that your `.env` file has correct LND credentials

### Testing Flow

1. **Login as Alice** to get admin token
2. **Create an invoice** through the API
3. **Check database** to see the unpaid invoice
4. **Pay the invoice** from Bob's node in Polar
5. **Check database** to see the invoice marked as paid
6. **Create an invoice** from Bob in Polar
7. **Pay the invoice** through our API
8. **Verify the payment** in the database

### Step 1: Login as Alice

**POST** `http://localhost:3000/api/users/login`

```json
{
  "username": "Alice",
  "password": "pass1"
}
```

Copy the token for subsequent requests.

### Step 2: Create Invoice Through API

**POST** `http://localhost:3000/api/invoices/invoice`

**Headers:**
```
Authorization: [alice-token]
```

**Body:**
```json
{
  "value": 100,
  "memo": "Test invoice from API",
  "user_id": 1
}
```

**Expected Response:**
```json
{
  "payment_request": "lnbcrt1u1p...",
  "r_hash": "...",
  "add_index": "1"
}
```

### Step 3: Check All Invoices

**GET** `http://localhost:3000/api/invoices/invoices`

You should see your new invoice with `settled: false`.

### Step 4: Pay Invoice from Bob

1. In Polar, click on Bob's node
2. Click "Pay Invoice"
3. Paste the payment request from Step 2
4. Click "Pay Invoice"

### Step 5: Check Invoice Status

**GET** `http://localhost:3000/api/invoices/invoices`

You should now see the invoice with `settled: true` and a `settle_date`.

### Step 6: Create Invoice from Bob

1. In Polar, click on Bob's node
2. Click "Create Invoice"
3. Set amount to 1000 sats
4. Set memo to "Test payment to Bob"
5. Click "Create Invoice"
6. Copy the payment request

### Step 7: Pay Invoice Through API

**POST** `http://localhost:3000/api/invoices/pay`

**Headers:**
```
Authorization: [alice-token]
```

**Body:**
```json
{
  "payment_request": "[bob-payment-request]",
  "user_id": 1
}
```

**Expected Response:**
```json
{
  "payment_request": "lnbcrt10u1p...",
  "send": true,
  "value": 1000,
  "fees": 0,
  "settled": true,
  "settle_date": "2024-01-01T12:00:00.000Z",
  "user_id": 1
}
```

### Step 8: Verify Final State

**GET** `http://localhost:3000/api/invoices/invoices`

You should see both invoices:
- One incoming (paid by Bob): `send: false`, `settled: true`
- One outgoing (paid to Bob): `send: true`, `settled: true`

## Common Issues and Debugging

### SQLite Constraint Errors

**Error:** `SQLite constraint: NOT NULL constraint failed: invoices.user_id`

**Solution:** Ensure all required fields are provided in your request body:
```json
{
  "payment_request": "lnbc...",
  "user_id": 1  // Don't forget this!
}
```

### Token Errors

**Error:** `Must be an admin`

**Solution:** 
1. Verify your admin key in `.env` matches the user's `adminKey`
2. Check that you're using the correct token
3. Ensure the token hasn't expired

### Database Connection Issues

**Error:** `Database is locked`

**Solution:**
1. Make sure only one instance of your server is running
2. Close any SQLite browser connections
3. Restart your server

### LND Connection Issues

**Error:** `No connection to LND`

**Solution:**
1. Verify Polar is running
2. Check your `.env` file has correct paths
3. Ensure the LND node is started in Polar

## The Complete Data Flow

Understanding how data flows through our system:

```
1. User Request → Express Route → Middleware → Controller
2. Controller → Model → Database Query → Results
3. Results → Controller → Response → User

Lightning Flow:
1. Create Invoice → LND → Database → Response
2. Invoice Paid → LND Event → Database Update
3. Pay Invoice → LND → Database → Response
```

### Example: Creating an Invoice

1. **POST** `/invoice` with `{ value: 100, memo: "test", user_id: 1 }`
2. **Middleware** validates JWT token
3. **Controller** calls `createInvoice()` function
4. **createInvoice()** calls LND to create Lightning invoice
5. **createInvoice()** calls `Invoice.create()` to save to database
6. **Database** stores invoice with `settled: false`
7. **Response** returns Lightning invoice data
8. **Event Stream** listens for payment
9. **Payment Occurs** → Event Stream updates database
10. **Database** now shows `settled: true`

## Key Takeaways

1. **Models Provide Abstraction:** Clean interface between API and database
2. **Promises Enable Async Operations:** Use `.then()` and `.catch()` for database operations
3. **Error Handling is Critical:** Always handle both success and failure cases
4. **JWT Integration:** Extract user data from tokens for database operations
5. **Lightning Integration:** Combine LND operations with database persistence
6. **Event Streams:** Real-time updates when Lightning payments occur
7. **Testing is Essential:** Use Postman and Polar to verify complete functionality

## What's Next?

In the next lesson, we'll:
- Connect our backend to the frontend application
- Update the frontend to use our API instead of LNBits
- Test the complete full-stack application
- Prepare for deployment to production

## Practice Exercises

1. **Add New Model Methods:**
   - Create a `findByUserId()` method in the Invoice model
   - Add a `findByDateRange()` method for invoice history

2. **Enhanced Error Handling:**
   - Add custom error messages for different failure scenarios
   - Implement retry logic for failed database operations

3. **Database Optimization:**
   - Add indexes to frequently queried fields
   - Create compound queries for complex operations

4. **Security Enhancements:**
   - Add input validation to all model methods
   - Implement rate limiting for API endpoints

5. **Lightning Features:**
   - Add support for invoice expiration
   - Implement webhook notifications for payments

## Resources

### Essential Documentation
- [Knex.js Query Builder](https://knexjs.org/guide/query-builder.html) - Complete query reference
- [Express.js Error Handling](https://expressjs.com/en/guide/error-handling.html) - Proper error handling patterns
- [JWT Best Practices](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/) - Security considerations

### Debugging Tools
- [SQLite Browser](https://sqlitebrowser.org/) - View database contents
- [Postman](https://www.postman.com/) - API testing
- [Polar](https://lightningpolar.com/) - Lightning network testing

### Lightning Development
- [LND API Documentation](https://lightning.engineering/api-docs/) - Complete LND reference
- [Lightning Network Specifications](https://github.com/lightning/bolts) - Protocol details

Remember: Database integration is where your application becomes truly functional. Take time to understand each piece and test thoroughly. The systematic approach we've learned here will serve you well as your Lightning applications grow in complexity!

The next lesson will bring everything together by connecting our backend to a frontend application, creating a complete full-stack Lightning wallet experience. 