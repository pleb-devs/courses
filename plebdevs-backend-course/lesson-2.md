[![Watch the course intro video](https://img.shields.io/badge/Watch-Lesson%202%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-2.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-2.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-2.webm)

## Lesson Slides
- [Lesson 2 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-2.pdf)
- [Lesson 2 link to slides](https://docs.google.com/presentation/d/1xIkS_Rb8cIrR4eEUT04LJdfdapl1GLCuWWqYVAyG2pc/edit?usp=sharing)

# Lesson 2: Learn Express Basics - Building Your First Server

## Course Overview
Welcome to Lesson 2 of the PlebDevs Backend Course! Now that you understand Node.js fundamentals, we're going to build on that knowledge with **Express.js** - the most popular and powerful web application framework for Node.js. This lesson transforms you from knowing JavaScript to building real web servers that can handle API requests and serve data.

## What You'll Build
In this lesson, you'll create the foundation for our **Lightning Wallet Backend** by building:
- Your first Express.js server
- HTTP API endpoints using REST principles
- Request/response handling system
- Status code management
- The `pleb-wallet-backend` project structure

## Lesson Overview
This lesson provides a comprehensive introduction to Express.js and HTTP APIs. We'll move beyond basic Node.js to create a functioning web server that can communicate with client applications. By the end, you'll have a solid understanding of server-side development and be ready to build complex backend applications.

## Prerequisites
- Basic JavaScript knowledge
- Node.js installed (from Lesson 1)
- Understanding of modules and NPM
- Text editor (VS Code recommended)
- Basic terminal/command line familiarity

## Key Learning Objectives
By the end of this lesson, you'll understand:
- What HTTP APIs are and why they're essential
- How to set up and configure Express.js
- The fundamentals of CRUD operations
- How to create API endpoints and routes
- Request and response object handling
- HTTP status codes and their meanings
- How to test APIs using Insomnia or Postman

## What are HTTP APIs?

### Definition
An **HTTP API** (Application Programming Interface) is a web-based interface that allows communication between different systems or applications over the internet using the HTTP protocol.

### Breaking Down the Acronym
- **HTTP**: Hypertext Transfer Protocol - the fundamental protocol of the internet
- **API**: Application Programming Interface - the "interface" is the key word here

Think of an API as a **contract** or **gateway** between your secure server environment and the outside world. It defines exactly:
- What data clients can request
- What data clients can send
- How that communication happens
- What responses they'll receive

### Why HTTP APIs Matter

#### Security and Control
- **Server-side protection**: Your business logic and sensitive data stay secure
- **Controlled access**: You define exactly what operations are allowed
- **Data validation**: You can validate and sanitize all incoming data
- **Authentication**: You control who can access what resources

#### Standardization
- **Universal protocol**: HTTP works across all platforms and devices
- **Consistent patterns**: RESTful APIs follow established conventions
- **Interoperability**: Different systems can communicate seamlessly

### The Client-Server Model
The API facilitates communication in the **client-server model**:

| Client Side | Server Side |
|-------------|-------------|
| Makes requests | Handles requests |
| Displays data | Stores and processes data |
| User interactions | Business logic |
| Potentially insecure | Secure environment |

## HTTP Methods and CRUD Operations

### Understanding CRUD
**CRUD** represents the four basic operations you can perform on data:

- **C**reate - Add new data
- **R**ead - Retrieve existing data  
- **U**pdate - Modify existing data
- **D**elete - Remove data

### HTTP Methods Map to CRUD

| HTTP Method | CRUD Operation | Purpose | Example |
|-------------|----------------|---------|---------|
| `POST` | Create | Add new resource | Create new user |
| `GET` | Read | Retrieve data | Get user profile |
| `PUT` | Update | Replace entire resource | Update user info |
| `DELETE` | Delete | Remove resource | Delete user account |

### RESTful API Design
**REST** (Representational State Transfer) is a set of architectural principles:

```
GET    /users          # Get all users
GET    /users/123      # Get user with ID 123
POST   /users          # Create new user
PUT    /users/123      # Update user with ID 123
DELETE /users/123      # Delete user with ID 123
```

## Setting Up Your Express.js Environment

### Step 1: Project Initialization
```bash
# Create project directory
mkdir pleb-wallet-backend
cd pleb-wallet-backend

# Initialize Node.js project
npm init -y

# Create main server file
touch index.js

# Install Express
npm install express
```

### Step 2: Project Structure
After setup, your project should look like this:
```
pleb-wallet-backend/
├── index.js
├── package.json
├── package-lock.json
└── node_modules/
```

### Understanding package.json
```json
{
  "name": "pleb-wallet-backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

## Building Your First Express Server

### Complete Server Code
```javascript
// index.js

// Import the Express library
const express = require("express");

// Create a new instance of the Express server
const server = express();

// Use the built-in JSON middleware to parse incoming JSON requests
server.use(express.json());

// Set up a route to handle GET requests to the root path
server.get("/", (req, res) => {
  // Send a JSON response with a "message" property set to "I'm alive!"
  res.status(200).json({ message: "I'm alive!" });
});

// Set the server to listen on the provided port, or 5500 if no port is specified
const PORT = process.env.PORT || 5500;

server.listen(PORT, () => {
  // Log a message to the console when the server starts listening
  console.log(`Server listening on port ${PORT}`);
});
```

### Code Breakdown

#### 1. Import Express
```javascript
const express = require("express");
```
- Uses CommonJS module system to import Express
- Express is now available as a function

#### 2. Create Server Instance
```javascript
const server = express();
```
- Calls `express()` to create a new application instance
- This instance will handle all HTTP requests

#### 3. Configure Middleware
```javascript
server.use(express.json());
```
- **Middleware** runs between receiving a request and sending a response
- `express.json()` parses incoming JSON data from request bodies
- Essential for handling POST/PUT requests with JSON data

#### 4. Define Route/Endpoint
```javascript
server.get("/", (req, res) => {
  res.status(200).json({ message: "I'm alive!" });
});
```
- `server.get()` defines a route that responds to GET requests
- `"/"` is the **endpoint** or **route path**
- `(req, res)` are the request and response objects
- `res.status(200)` sets HTTP status code to 200 (OK)
- `res.json()` sends a JSON response

#### 5. Start Server
```javascript
const PORT = process.env.PORT || 5500;
server.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
});
```
- `process.env.PORT` checks for environment variable (useful for deployment)
- Falls back to port 5500 for local development
- `server.listen()` starts the server
- Callback function runs when server is ready

## Understanding Request and Response Objects

### The Request Object (req)
The `req` object contains information about the incoming HTTP request:

```javascript
server.get("/users", (req, res) => {
  // Request properties you can access:
  console.log(req.method);     // HTTP method (GET, POST, etc.)
  console.log(req.url);        // Full URL path
  console.log(req.headers);    // HTTP headers
  console.log(req.query);      // Query string parameters
  console.log(req.params);     // URL parameters
  console.log(req.body);       // Request body (for POST/PUT)
});
```

### The Response Object (res)
The `res` object is used to send responses back to the client:

```javascript
server.get("/users", (req, res) => {
  // Different ways to send responses:
  res.status(200).json({ users: [] });           // JSON response
  res.status(201).send("User created");          // Text response
  res.status(404).json({ error: "Not found" });  // Error response
  res.redirect("/login");                         // Redirect
});
```

## HTTP Status Codes

### Common Status Codes
Understanding status codes is crucial for API development:

| Code | Status | When to Use |
|------|--------|-------------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST (resource created) |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Resource doesn't exist |
| 500 | Internal Server Error | Server-side error |

### Status Code Examples
```javascript
// Success responses
res.status(200).json({ message: "Success" });
res.status(201).json({ id: 123, message: "User created" });

// Error responses  
res.status(400).json({ error: "Invalid input" });
res.status(404).json({ error: "User not found" });
res.status(500).json({ error: "Server error" });
```

## Testing Your API

### Using Insomnia or Postman

#### Installation
1. Download [Insomnia](https://insomnia.rest/) (recommended) or [Postman](https://www.postman.com/)
2. Install and create a new project
3. Both are free and provide excellent API testing capabilities

#### Making Your First API Request
1. **Start your server**: `node index.js`
2. **Open Insomnia**
3. **Create new request**:
   - Method: GET
   - URL: `http://localhost:5500`
   - Click Send

#### Expected Response
```json
{
  "message": "I'm alive!"
}
```

### Testing Different Endpoints
```javascript
// Add more endpoints to test
server.get("/users", (req, res) => {
  res.status(200).json({ 
    users: [
      { id: 1, name: "Alice" },
      { id: 2, name: "Bob" }
    ]
  });
});

server.post("/users", (req, res) => {
  const { name } = req.body;
  res.status(201).json({ 
    id: Date.now(), 
    name: name,
    message: "User created successfully" 
  });
});
```

## Building Multiple Endpoints

### Expanding Your API
Let's create a more comprehensive API structure:

```javascript
// index.js - Enhanced version
const express = require("express");
const server = express();

// Middleware
server.use(express.json());

// Root endpoint
server.get("/", (req, res) => {
  res.status(200).json({ message: "Lightning Wallet API is alive!" });
});

// Users endpoints
server.get("/users", (req, res) => {
  // In a real app, this would come from a database
  const users = [
    { id: 1, username: "alice", email: "alice@example.com" },
    { id: 2, username: "bob", email: "bob@example.com" }
  ];
  res.status(200).json({ users });
});

server.get("/users/:id", (req, res) => {
  const { id } = req.params;
  // In a real app, query the database
  const user = { id: parseInt(id), username: "alice", email: "alice@example.com" };
  res.status(200).json({ user });
});

server.post("/users", (req, res) => {
  const { username, email } = req.body;
  
  // Validate input
  if (!username || !email) {
    return res.status(400).json({ error: "Username and email are required" });
  }
  
  // In a real app, save to database
  const newUser = {
    id: Date.now(),
    username,
    email,
    createdAt: new Date().toISOString()
  };
  
  res.status(201).json({ user: newUser, message: "User created successfully" });
});

// Lightning endpoints (placeholder for future lessons)
server.get("/lightning/info", (req, res) => {
  res.status(200).json({ 
    message: "Lightning integration coming soon!",
    nodeId: "placeholder"
  });
});

// Error handling middleware
server.use((req, res) => {
  res.status(404).json({ error: "Endpoint not found" });
});

const PORT = process.env.PORT || 5500;
server.listen(PORT, () => {
  console.log(`🚀 Lightning Wallet Backend listening on port ${PORT}`);
});
```

## Best Practices for Express Development

### 1. Project Organization
```
pleb-wallet-backend/
├── index.js              # Main server file
├── routes/               # Route definitions
│   ├── userRoutes.js
│   └── lightningRoutes.js
├── middleware/           # Custom middleware
│   └── auth.js
├── models/              # Data models
│   └── User.js
├── utils/               # Utility functions
│   └── validation.js
├── package.json
└── .env                 # Environment variables
```

### 2. Error Handling
```javascript
// Always handle errors properly
server.get("/users/:id", (req, res) => {
  try {
    const { id } = req.params;
    
    // Validate input
    if (!id || isNaN(id)) {
      return res.status(400).json({ error: "Invalid user ID" });
    }
    
    // Process request
    const user = getUserById(id);
    
    if (!user) {
      return res.status(404).json({ error: "User not found" });
    }
    
    res.status(200).json({ user });
  } catch (error) {
    console.error("Error getting user:", error);
    res.status(500).json({ error: "Internal server error" });
  }
});
```

### 3. Input Validation
```javascript
const validateUser = (userData) => {
  const errors = [];
  
  if (!userData.username || userData.username.length < 3) {
    errors.push("Username must be at least 3 characters");
  }
  
  if (!userData.email || !userData.email.includes("@")) {
    errors.push("Valid email is required");
  }
  
  return errors;
};

server.post("/users", (req, res) => {
  const errors = validateUser(req.body);
  
  if (errors.length > 0) {
    return res.status(400).json({ errors });
  }
  
  // Process valid data
});
```

## Common Issues and Solutions

### 1. Port Already in Use
**Problem**: `Error: listen EADDRINUSE :::5500`
**Solution**: 
```bash
# Kill process using the port
lsof -ti:5500 | xargs kill -9

# Or use a different port
const PORT = process.env.PORT || 5501;
```

### 2. Cannot POST/PUT Data
**Problem**: Request body is undefined
**Solution**: 
```javascript
// Make sure you have JSON middleware
server.use(express.json());

// For form data, also add:
server.use(express.urlencoded({ extended: true }));
```

### 3. CORS Issues
**Problem**: Frontend can't access your API
**Solution**: 
```bash
npm install cors
```
```javascript
const cors = require('cors');
server.use(cors());
```

## Hands-on Exercise: Build a Task API

### Challenge
Create a simple task management API with the following endpoints:
- `GET /tasks` - Get all tasks
- `POST /tasks` - Create a new task
- `GET /tasks/:id` - Get a specific task
- `PUT /tasks/:id` - Update a task
- `DELETE /tasks/:id` - Delete a task

### Starter Code
```javascript
const express = require("express");
const server = express();

server.use(express.json());

// In-memory storage (replace with database later)
let tasks = [
  { id: 1, title: "Learn Node.js", completed: false },
  { id: 2, title: "Build Express API", completed: false }
];

// Your endpoints here

const PORT = process.env.PORT || 5500;
server.listen(PORT, () => {
  console.log(`Task API listening on port ${PORT}`);
});
```

### Solution Framework
```javascript
// GET all tasks
server.get("/tasks", (req, res) => {
  res.status(200).json({ tasks });
});

// POST new task
server.post("/tasks", (req, res) => {
  const { title } = req.body;
  // Validate and create task
  // Return 201 status with new task
});

// GET specific task
server.get("/tasks/:id", (req, res) => {
  const { id } = req.params;
  // Find task by ID
  // Return 404 if not found
});

// PUT update task
server.put("/tasks/:id", (req, res) => {
  const { id } = req.params;
  // Update task
  // Return updated task
});

// DELETE task
server.delete("/tasks/:id", (req, res) => {
  const { id } = req.params;
  // Remove task from array
  // Return 204 status
});
```

## Understanding Middleware

### What is Middleware?
Middleware functions are functions that have access to the request object, response object, and the next middleware function in the application's request-response cycle.

### Built-in Middleware
```javascript
// JSON parsing middleware
server.use(express.json());

// URL-encoded form data
server.use(express.urlencoded({ extended: true }));

// Serve static files
server.use(express.static('public'));
```

### Custom Middleware
```javascript
// Logging middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next(); // Pass control to next middleware
};

server.use(logger);

// Authentication middleware (coming in later lessons)
const authenticate = (req, res, next) => {
  // Check for valid token
  const token = req.headers.authorization;
  
  if (!token) {
    return res.status(401).json({ error: "No token provided" });
  }
  
  // Verify token logic here
  next();
};

// Use on specific routes
server.get("/protected", authenticate, (req, res) => {
  res.json({ message: "You are authenticated!" });
});
```

## Environment Variables and Configuration

### Using .env Files
```bash
# Install dotenv
npm install dotenv
```

Create `.env` file:
```
PORT=5500
NODE_ENV=development
DATABASE_URL=your_database_url
```

Load in your application:
```javascript
require('dotenv').config();

const PORT = process.env.PORT || 5500;
const NODE_ENV = process.env.NODE_ENV || 'development';
```

## Development Workflow Improvements

### Auto-restart with Nodemon
```bash
# Install nodemon for development
npm install -g nodemon

# Or install locally
npm install --save-dev nodemon
```

Add to `package.json`:
```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  }
}
```

Run with auto-restart:
```bash
npm run dev
```

## API Documentation Best Practices

### Document Your Endpoints
```javascript
/**
 * @route GET /users
 * @desc Get all users
 * @access Public
 * @returns {Array} Array of user objects
 */
server.get("/users", (req, res) => {
  // Implementation
});

/**
 * @route POST /users
 * @desc Create a new user
 * @access Public
 * @param {String} username - User's username
 * @param {String} email - User's email
 * @returns {Object} Created user object
 */
server.post("/users", (req, res) => {
  // Implementation
});
```

## Next Steps and Advanced Concepts

### What's Coming Next
In Lesson 3, we'll dive deeper into:
- Advanced routing with Express Router
- Database integration with SQL
- User authentication and authorization
- Middleware patterns and security
- Error handling strategies

### Immediate Actions
1. **Complete the hands-on exercise** - Build the task API
2. **Experiment with different endpoints** - Try POST, PUT, DELETE
3. **Test with Insomnia/Postman** - Get comfortable with API testing
4. **Set up nodemon** - Improve your development workflow

### Key Takeaways
- Express.js makes server development straightforward and powerful
- HTTP APIs follow RESTful conventions for consistency
- Request/response objects provide all the data you need
- Status codes communicate the outcome of operations
- Middleware provides a powerful way to extend functionality
- Proper error handling is essential for robust APIs

## Additional Resources

### Official Documentation
- [Express.js Official Documentation](https://expressjs.com/)
- [Node.js HTTP Module](https://nodejs.org/api/http.html)
- [MDN HTTP Response Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

### Learning Resources
- [Express.js Crash Course](https://www.youtube.com/watch?v=L72fhGm1tfE) - YouTube tutorial
- [REST API Design Best Practices](https://restfulapi.net/) - Comprehensive guide
- [Express.js Cheatsheet](https://devhints.io/express) - Quick reference

### Development Tools
- [Insomnia](https://insomnia.rest/) - API testing tool
- [Postman](https://www.postman.com/) - Alternative API testing
- [Nodemon](https://nodemon.io/) - Development server auto-restart

## Summary

Congratulations! You've just built your first Express.js server and learned the fundamentals of HTTP API development. You now understand:

- How to create and configure Express servers
- The principles of RESTful API design
- HTTP methods and status codes
- Request/response handling
- API testing with dedicated tools
- Best practices for server development

This foundation is crucial for everything we'll build in the rest of the course. In our next lesson, we'll expand on these concepts by adding database integration and user authentication to create a more sophisticated backend system.

The journey from understanding Node.js to building production-ready APIs is well underway. You're not just learning Express.js - you're mastering the patterns and practices that power modern web applications.

Ready to add a database to your server? Let's continue to Lesson 3! 🚀 