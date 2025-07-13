## Lesson Slides
- [Lesson 5 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-5.pdf)
- [Lesson 5 link to slides](https://docs.google.com/presentation/d/190OsLT3zosjGmjJt8pBA9PmVdfFTPLF4EtuC_CMMc_k/edit?usp=sharing)

# Lesson 5: Learn Express Authentication

**PlebDevs Backend Course - Building Lightning Apps with Node.js**

---

## Overview

In this lesson, we'll dive deep into authentication for our Express server. This is arguably one of the most important and complex lessons in the course, as authentication is critical for securing any backend application.

### What You'll Learn

- **JSON Web Tokens (JWT)** - Industry standard for stateless authentication
- **Environment Variables** - Securely storing sensitive configuration
- **Password Hashing** - Using bcryptjs to secure user passwords
- **Custom Middleware** - Building authentication and authorization layers
- **Multi-level Access Control** - Regular users vs Admin users

### Prerequisites

- Completed Lessons 1-4
- Basic understanding of Express.js and middleware
- Node.js and npm installed

---

## What is Authentication?

Authentication is the process of verifying the identity (or pseudo-identity) of a user or system accessing your server. In web applications, authentication is crucial for:

- **Protecting sensitive resources** - Database, Lightning node, user data
- **Controlling access** - Who can do what in your application
- **Maintaining security** - Preventing unauthorized actions

> 🔒 **Security First**: Without proper authentication, anyone could potentially access your sensitive information or perform actions on your behalf.

---

## JSON Web Tokens (JWT)

### What are JWTs?

JSON Web Tokens are an open standard (RFC 7519) for securely transmitting information between parties as a JSON object. They're perfect for authentication because they are:

- **Stateless** - No server-side storage required
- **Scalable** - Easy to distribute across multiple servers
- **Self-contained** - Carry all necessary information

### JWT Structure

A JWT consists of three parts separated by dots (`.`):

```
header.payload.signature
```

1. **Header** - Specifies the algorithm used (e.g., HS256)
2. **Payload** - Contains user data and claims
3. **Signature** - Verifies the token's authenticity

### JWT Debugger

Visit [jwt.io](https://jwt.io) to visualize and debug JWT tokens. This tool is invaluable for understanding JWT structure and troubleshooting authentication issues.

---

## Setting Up JWT Authentication

### Step 1: Install Required Packages

```bash
npm install jsonwebtoken bcryptjs dotenv
```

- **jsonwebtoken** - JWT creation and verification
- **bcryptjs** - Password hashing
- **dotenv** - Environment variable management

### Step 2: Import Dependencies

Add these imports to your `usersRouter.js`:

```javascript
const jwt = require("jsonwebtoken");
const bcrypt = require("bcryptjs");
```

### Step 3: Create Token Generation Function

Add this function to the bottom of your `usersRouter.js`:

```javascript
/**
 * Generate a JSON Web Token (JWT) for a given user
 * @param {Object} user - User object containing id, username, admin status
 * @returns {string} - Signed JWT token
 */
function generateToken(user) {
  // Define the payload to be included in the token
  const payload = {
    id: user.id,
    username: user.username,
    admin: user.admin,
  };
  
  // Get the JWT secret from environment variables
  const secret = process.env.JWT_SECRET || "Satoshi Nakamoto";
  
  // Define token options
  const options = {
    expiresIn: "1d", // Token expires in 1 day
  };
  
  // Generate and return the JWT
  return jwt.sign(payload, secret, options);
}
```

---

## Environment Variables

### Why Use Environment Variables?

Environment variables allow you to:
- **Store sensitive data** separate from your code
- **Enable different configurations** for development/production
- **Prevent secrets** from being committed to version control

### Setting Up Environment Variables

1. **Create a `.env` file** in your project root:

```bash
# Server configuration
PORT=5500

# JWT Secret (use a strong, random string in production)
JWT_SECRET=keepitsecretkeepitsafe

# Admin key for elevated permissions
ADMIN_KEY=1234
```

2. **Create/Update `.gitignore`**:

```
node_modules/
.env
```

> ⚠️ **Important**: Never commit your `.env` file to version control!

3. **Install and configure dotenv** in `index.js`:

```javascript
const dotenv = require("dotenv");

// Load environment variables
dotenv.config();

// Use environment variables
const PORT = process.env.PORT || 5500;
```

---

## Password Hashing with bcryptjs

### Why Hash Passwords?

- **Security** - Plain text passwords are vulnerable if your database is compromised
- **One-way function** - Hashes cannot be reversed to reveal the original password
- **Comparison** - bcrypt can compare plain text passwords to hashed versions

### Hashing Process

```javascript
// Hash a password
const hashedPassword = bcrypt.hashSync(password, 14);

// Compare passwords
const isValid = bcrypt.compareSync(plainTextPassword, hashedPassword);
```

### Updated Login Endpoint

Replace your existing login endpoint in `usersRouter.js`:

```javascript
router.post("/login", (req, res) => {
  // Extract credentials from request body
  const { username, password } = req.body;
  
  // Placeholder user object (will be replaced with database query)
  const DBuser = {
    username: "test",
    password: "pass1",
  };
  
  // Hash the stored password for comparison
  const hashedPassword = bcrypt.hashSync(DBuser.password, 14);
  
  // Verify user credentials
  if (DBuser && bcrypt.compareSync(password, hashedPassword)) {
    // Generate JWT token
    const token = generateToken(DBuser);
    
    // Send success response
    res.status(200).json({ 
      message: `Welcome ${DBuser.username}!`, 
      token, 
      DBuser 
    });
  } else {
    // Send error response
    res.status(401).json({ message: "Invalid credentials" });
  }
});
```

---

## Custom Authentication Middleware

### Planning Our Access Levels

Our application will have three levels of access:

1. **Public** - Anyone can view wallet balance and transactions
2. **Authenticated Users** - Can create invoices
3. **Admin Users** - Can pay invoices (spend money!)

### Creating the Middleware Directory

Create the following structure:
```
routers/
  middleware/
    authenticate.js
    authenticateAdmin.js
```

### Basic Authentication Middleware

Create `routers/middleware/authenticate.js`:

```javascript
const jwt = require("jsonwebtoken");

/**
 * Middleware to authenticate users via JWT
 * Checks for valid JWT token in Authorization header
 */
module.exports = (req, res, next) => {
  // Extract token from Authorization header
  const token = req.headers.authorization;
  
  // Get JWT secret from environment variables
  const secret = process.env.JWT_SECRET || "Satoshi Nakamoto";
  
  if (token) {
    // Verify the token
    jwt.verify(token, secret, (err, decodedToken) => {
      if (err) {
        // Token is invalid
        res.status(401).json({ 
          message: "Not Allowed", 
          Error: err.message 
        });
      } else {
        // Token is valid, continue to next middleware/endpoint
        console.log("Decoded token:", decodedToken);
        next();
      }
    });
  } else {
    // No token provided
    res.status(401).json({ message: "No token!" });
  }
};
```

### Admin Authentication Middleware

Create `routers/middleware/authenticateAdmin.js`:

```javascript
const jwt = require("jsonwebtoken");

/**
 * Middleware to authenticate admin users
 * Checks for valid JWT token AND admin key
 */
module.exports = (req, res, next) => {
  // Extract token from Authorization header
  const token = req.headers.authorization;
  
  // Get JWT secret from environment variables
  const secret = process.env.JWT_SECRET || "Satoshi Nakamoto";
  
  if (token) {
    jwt.verify(token, secret, async (err, decodedToken) => {
      if (err || !decodedToken) {
        // Token verification failed
        res.status(401).json({ 
          message: "Error with your verification" 
        });
      } else {
        // Token is valid, now check admin privileges
        
        // Placeholder user object (will be replaced with database query)
        const user = {
          username: "test",
          password: "pass1",
          adminKey: 1234,
        };
        
        // Extract admin key from user object
        const adminKey = user?.adminKey?.toString() ?? "";
        
        // Check if user has valid admin key
        if (adminKey !== process.env.ADMIN_KEY) {
          // User is not an admin
          res.status(401).json({ message: "Must be an admin" });
        } else {
          // User is admin, continue to endpoint
          next();
        }
      }
    });
  } else {
    // No token provided
    res.status(401).json({ message: "No token!" });
  }
};
```

---

## Implementing Middleware on Endpoints

### Protecting the Create Invoice Endpoint

Update your `lightningRouter.js`:

```javascript
const authenticate = require("./middleware/authenticate");

// POST endpoint to create an invoice (requires authentication)
router.post("/invoice", authenticate, (req, res) => {
  const { value, memo } = req.body;
  
  console.log("Creating invoice:", { value, memo });
  
  res.status(200).json({ message: "I'm alive!" });
});
```

### Protecting the Pay Invoice Endpoint

```javascript
const authenticateAdmin = require("./middleware/authenticateAdmin");

// POST endpoint to pay an invoice (requires admin privileges)
router.post("/pay", authenticateAdmin, (req, res) => {
  const { payment_request } = req.body;
  
  console.log("Paying invoice:", payment_request);
  
  res.status(200).json({ message: "I'm alive!" });
});
```

---

## Testing Your Authentication System

### Test 1: Unauthenticated Request

Try accessing the protected endpoint without a token:

```bash
# Should return 401 - No token!
curl -X POST http://localhost:5500/lightning/invoice
```

### Test 2: Get Authentication Token

Login to get a JWT token:

```bash
curl -X POST http://localhost:5500/users/login \
  -H "Content-Type: application/json" \
  -d '{"username": "test", "password": "pass1"}'
```

### Test 3: Authenticated Request

Use the token from step 2 in the Authorization header:

```bash
curl -X POST http://localhost:5500/lightning/invoice \
  -H "Authorization: YOUR_JWT_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"value": 1000, "memo": "Test invoice"}'
```

### Test 4: Admin Request

Test the admin endpoint with the same token:

```bash
curl -X POST http://localhost:5500/lightning/pay \
  -H "Authorization: YOUR_JWT_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"payment_request": "lnbc..."}'
```

---

## Common HTTP Status Codes

Understanding these status codes is crucial for authentication:

- **200 OK** - Request successful
- **401 Unauthorized** - Authentication failed
- **403 Forbidden** - Authenticated but not authorized
- **500 Internal Server Error** - Server-side error

---

## Security Best Practices

### 1. Strong JWT Secrets
```javascript
// BAD - Predictable secret
JWT_SECRET=password123

// GOOD - Random, long secret
JWT_SECRET=h8f9d7s6f5g4h3j2k1l0m9n8b7v6c5x4z3a2s1d0f9g8h7j6k5l4m3n2b1v0c9x8z7
```

### 2. Token Expiration
```javascript
const options = {
  expiresIn: "1d", // Tokens expire in 1 day
};
```

### 3. Environment Variables
- Never commit `.env` files to version control
- Use different secrets for different environments
- Rotate secrets regularly in production

### 4. Password Hashing
```javascript
// Use appropriate salt rounds (12-15 for production)
const hashedPassword = bcrypt.hashSync(password, 14);
```

---

## Troubleshooting Common Issues

### Issue: "Invalid signature" error
**Cause**: Token was signed with a different secret
**Solution**: Ensure the same JWT_SECRET is used for signing and verification

### Issue: "Token expired" error
**Cause**: JWT has passed its expiration time
**Solution**: User needs to login again to get a new token

### Issue: "No token" error
**Cause**: Authorization header is missing or incorrectly formatted
**Solution**: Ensure the header is set as `Authorization: your-jwt-token`

### Issue: "Must be an admin" error
**Cause**: User doesn't have admin privileges
**Solution**: Verify the user has the correct adminKey in their profile

---

## Next Steps

In the next lesson, we'll:
- Set up a real database to store users
- Implement proper user registration
- Connect our authentication system to the database
- Begin integrating with the Lightning Network

---

## Key Takeaways

1. **Authentication is Critical** - Never skip security in your applications
2. **JWTs are Stateless** - Perfect for scalable applications
3. **Environment Variables** - Keep secrets separate from code
4. **Middleware is Powerful** - Use it to implement cross-cutting concerns
5. **Defense in Depth** - Use multiple layers of security

---

## Additional Resources

- [JWT.io](https://jwt.io/) - JWT debugger and documentation
- [bcryptjs npm package](https://www.npmjs.com/package/bcryptjs) - Password hashing library
- [dotenv npm package](https://www.npmjs.com/package/dotenv) - Environment variable management
- [Express Middleware Guide](https://expressjs.com/en/guide/using-middleware.html) - Official Express middleware documentation

---

## Practice Exercises

1. **Create a logout endpoint** that invalidates tokens (hint: you'll need to maintain a blacklist)
2. **Add token refresh functionality** to extend user sessions
3. **Implement role-based access control** with different user roles
4. **Add rate limiting** to prevent brute force attacks on login

> 💡 **Remember**: This is one of the most complex lessons in the course. Take your time, experiment with the code, and don't hesitate to review the concepts multiple times. Authentication is a fundamental skill that you'll use in every backend application you build.

---

*This lesson is part of the PlebDevs Backend Course - Building Lightning Apps with Node.js* 