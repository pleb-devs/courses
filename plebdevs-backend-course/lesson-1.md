[![Watch the course intro video](https://img.shields.io/badge/Watch-Lesson%201%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-1.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-1.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-2-lesson-1.webm)

## Lesson Slides
- [Lesson 1 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-1.pdf)
- [Lesson 1 link to slides](https://docs.google.com/presentation/d/1eOUZ86R34oNiWEHRW70N5d-2qDuSQVkHmG6xi6ajBHc/edit?usp=sharing)

# Lesson 1: Learn Node.js - Introduction to Node.js and the Server Environment

## Course Overview
Welcome to PlebDevs Backend Course! In this comprehensive 13-lesson journey, we'll transform you from a JavaScript developer into a proficient backend developer with a special focus on Lightning App Development. This course builds on frontend fundamentals to create robust server-side applications.

## What You'll Build
Throughout this course, you'll build a complete **Lightning Wallet Backend** that includes:
- Node.js server with Express
- User authentication & authorization with JWTs
- Local Lightning development environment
- Lightning integration using LND
- SQL database with Knex
- Full deployment pipeline

## Lesson Overview
This lesson provides a soft introduction to Node.js and the server environment. While Node.js might seem like a big, complicated concept, it's really just JavaScript running in a different environment with some powerful new capabilities.

## Prerequisites
- Basic JavaScript knowledge
- Understanding of programming fundamentals
- Text editor (VS Code recommended)
- Terminal/command line familiarity

## Key Learning Objectives
By the end of this lesson, you'll understand:
- What Node.js is and why it's important
- The difference between server and client environments
- How to work with modules and NPM
- Basic Node.js concepts and terminology
- How to create and run your first Node.js application

## What is Node.js?

### Definition
Node.js is a **JavaScript runtime** built on the V8 engine that allows you to run JavaScript code outside of a web browser.

### Key Components

#### The V8 Engine
- Open-source JavaScript engine developed by Google
- Designed to compile and execute JavaScript code at lightning-fast speeds
- The same engine that powers Google Chrome

#### Why Node.js Matters
Before Node.js, JavaScript was confined to the browser. Node.js breaks JavaScript out of its cage, enabling:
- **Server-side applications** - Build web servers and APIs
- **Command-line tools** - Create utilities and scripts
- **Desktop applications** - Build cross-platform apps
- **System-level access** - Interact with file systems, networks, and hardware

### System-Level Resources
Running JavaScript outside the browser means developers can access:
- **File system** - Read and write files
- **Network sockets** - Handle real-time communication
- **Memory management** - Direct system resource access
- **Operating system APIs** - Interface with the underlying OS

## Server Environment vs Web Environment

### Understanding the Difference

| Server Environment | Web Environment |
|-------------------|-----------------|
| Executes server-side code | Executes client-side code |
| Runs on secure, controlled servers | Runs in user's browser |
| Has full system access | Sandboxed for security |
| Serves data to clients | Consumes data from servers |
| Can be cloud-hosted or self-hosted | Depends on browser capabilities |

### Client-Server Model
The **client-server model** is a fundamental concept in web development:
- **Server**: Holds and serves data, handles business logic, maintains security
- **Client**: Requests and displays data, handles user interactions
- **Communication**: Clients request resources from servers via APIs

### Why Backend Development?
- **Data Security**: Sensitive operations happen on secure servers
- **Scalability**: Handle thousands of concurrent users
- **Business Logic**: Implement complex algorithms and data processing
- **API Creation**: Build interfaces for multiple client applications

## Node.js Capabilities

### Popular Use Cases
- **Data streaming** - Real-time data processing
- **Server-side proxy** - Route and manage requests
- **Big data and analytics** - Process large datasets
- **Wireless connectivity** - IoT device communication
- **System monitoring** - Track resource usage
- **Real-time applications** - Chat apps, gaming, live updates
- **Web scraping** - Automated data collection
- **REST APIs** - The foundation of modern web services

## Essential Node.js Concepts

### 1. Modules and the CommonJS System

#### What Are Modules?
Modules are self-contained units of code that:
- Keep code organized and manageable
- Have their own variables and functions
- Can be imported and exported between files
- Enable code reuse and sharing

#### CommonJS Module System
Node.js uses the CommonJS module system for importing and exporting code:

```javascript
// greeting.js - Creating a module
const greeting = "Hello, World!";
module.exports = greeting;

// index.js - Using a module
const greeting = require('./greeting');
console.log(greeting); // "Hello, World!"
```

#### Exporting Multiple Items
```javascript
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

module.exports = {
  add,
  subtract
};

// app.js
const { add, subtract } = require('./math');
console.log(add(5, 3)); // 8
```

### 2. Node Package Manager (NPM)

#### What is NPM?
NPM is a package manager that:
- Provides access to over 1 million open-source packages
- Manages project dependencies
- Includes a command-line interface for easy package management
- Enables sharing your own packages with the community

#### Basic NPM Commands
```bash
npm --version          # Check NPM version
npm init -y           # Initialize a new project
npm install axios     # Install a package
npm install           # Install all dependencies
```

#### Working with Built-in Modules
Node.js provides many built-in modules for common tasks:

```javascript
// File system operations
const fs = require('fs');
fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Operating system information
const os = require('os');
console.log(os.platform());
console.log(os.architecture());
```

## Setting Up Your First Node.js Project

### Step 1: Installation
1. Visit [nodejs.org](https://nodejs.org)
2. Download the LTS version for your operating system
3. Run the installer and follow the prompts
4. Verify installation:
```bash
node --version
npm --version
```

### Step 2: Create Your Project
```bash
# Create project directory
mkdir hello-node
cd hello-node

# Initialize Node.js project
npm init -y

# Create main file
touch index.js  # or create in your code editor
```

### Step 3: Your First Node.js Code
```javascript
// index.js
console.log("Hello, Node.js!");
```

### Step 4: Run Your Code
```bash
node index.js
```

## Hands-on Mini Project: Command-Line Greeting App

Let's build a command-line application that greets users and displays the current date and time.

### Project Setup
```bash
mkdir hello-node
cd hello-node
npm init -y
```

### Complete Code
```javascript
// index.js
const readline = require('readline');

// Create an interface for reading input from the console
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// Get the current date and time
const now = new Date();

// Ask the user for their name
rl.question('What is your name? ', (name) => {
  // Display the greeting and current date and time
  console.log(`Hello, ${name}! The current date and time is ${now.toString()}.`);
  
  // Close the readline interface
  rl.close();
});
```

### Running the Project
```bash
node index.js
```

### Code Breakdown
1. **Import readline module**: Enables console input/output
2. **Create interface**: Sets up stdin/stdout communication
3. **Get current date**: Creates a Date object for timestamp
4. **Ask question**: Prompts user for input with callback
5. **Display result**: Shows greeting with user's name and timestamp
6. **Close interface**: Properly terminates the program

## Understanding Node.js Under the Hood

### The Event Loop
The event loop is Node.js's secret weapon for handling multiple operations efficiently:

#### Key Concepts
- **Single-threaded**: Node.js executes one task at a time
- **Non-blocking**: Doesn't get stuck waiting for slow operations
- **Event-driven**: Responds to events as they occur
- **Callback queue**: Manages tasks waiting to be executed

#### How It Works
1. Node.js receives a task
2. If the task is quick, it executes immediately
3. If the task is slow (file reading, network request), it:
   - Starts the task
   - Registers a callback function
   - Moves to the next task
   - Executes the callback when the slow task completes

### Asynchronous Operations
**Asynchronous** means "not at the same time":
- **Synchronous**: Like a phone call - both parties must be present
- **Asynchronous**: Like text messaging - responses come when convenient

#### Callbacks
Callbacks are functions that run after another function completes:
```javascript
// Reading a file asynchronously
fs.readFile('data.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data); // This runs AFTER the file is read
});

console.log('This runs BEFORE the file is read');
```

## Best Practices for Node.js Development

### 1. Project Organization
- Use descriptive file and folder names
- Keep related files together
- Separate concerns into different modules
- Create a logical project structure

### 2. Error Handling
- Always handle errors in callbacks
- Use try-catch blocks for synchronous code
- Provide meaningful error messages

### 3. Code Quality
- Write clear, readable code
- Use consistent naming conventions
- Comment complex logic
- Keep functions small and focused

## Common Issues and Solutions

### 1. "Module not found" Error
**Problem**: `Error: Cannot find module 'module-name'`
**Solution**: 
```bash
npm install module-name
```

### 2. "node: command not found"
**Problem**: Node.js not installed properly
**Solution**: 
- Reinstall Node.js from official website
- Check PATH environment variable
- Restart terminal

### 3. Permission Errors
**Problem**: Cannot install packages globally
**Solution**: 
```bash
# Use npx for one-time use
npx package-name

# Or configure npm properly
npm config set prefix '~/.npm-global'
```

## Exercise: Build Your First Node.js App

### Challenge
Create a simple calculator app that:
1. Asks the user for two numbers
2. Asks for an operation (+, -, *, /)
3. Displays the result

### Starter Code
```javascript
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// Your code here
```

### Solution Approach
1. Use nested readline questions
2. Convert strings to numbers
3. Use a switch statement for operations
4. Handle division by zero
5. Close the interface properly

## Additional Resources

### Documentation
- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [NPM Documentation](https://docs.npmjs.com/)
- [CommonJS Modules](https://nodejs.org/api/modules.html)

### Learning Resources
- [Node.js Crash Course](https://www.youtube.com/watch?v=fBNz5xF-Kx4) - 1-hour video tutorial
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices) - GitHub repository
- [Node.js Basics Cheat Sheet](https://overapi.com/nodejs) - Quick reference

### Development Tools
- [Nodemon](https://nodemon.io/) - Auto-restart server during development
- [Node.js Debugger](https://nodejs.org/en/docs/guides/debugging-getting-started/) - Built-in debugging tools

## Next Steps

### Immediate Actions
1. **Set up Node.js** on your development machine
2. **Complete the mini project** from this lesson
3. **Experiment** with different built-in modules
4. **Practice** the require/module.exports pattern

### Prepare for Lesson 2
In our next lesson, we'll dive into building our first server using Express.js. We'll cover:
- Setting up an Express server
- Creating routes and endpoints
- Handling HTTP requests and responses
- Building the foundation for our Lightning Wallet backend

### Key Takeaways
- Node.js is just JavaScript in a different environment
- The event loop makes Node.js fast and efficient
- Modules help organize and share code
- NPM provides access to millions of packages
- Asynchronous programming is fundamental to Node.js

## Remember
Every expert was once a beginner. Node.js might seem overwhelming at first, but it's built on JavaScript fundamentals you already know. Focus on understanding the concepts, practice regularly, and don't be afraid to experiment.

The journey from frontend to full-stack developer starts with this single step. You're not just learning Node.js – you're building the foundation for creating powerful, scalable applications that can handle real-world traffic and solve real problems.

Ready to build your first server? Let's continue to Lesson 2! 🚀 