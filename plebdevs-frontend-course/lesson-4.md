[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-4.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-4.mp4)

## Additional Resources
- **Slides:** [Development Environments Setup Presentation](https://docs.google.com/presentation/d/1mXp7dFQzVGE4sV9o8xYUW3woFWS5NzOUCbRyNrMWKl4/edit?usp=sharing)

# Learn Development Environments: Setting Up Your Professional Workflow

## Lesson Overview
Welcome to lesson 4 of the PlebDevs Frontend Course! After learning HTML, CSS, and JavaScript, it's time to level up your development workflow. In this lesson, we'll set up a complete professional development environment that mirrors what real developers use in the industry. You'll learn to work with modern tools, manage code with Git and GitHub, build React applications, and deploy them to the internet.

This lesson is a game-changer - it transforms you from someone who writes code in isolation to a developer who can collaborate, version control, and ship applications to real users.

## Prerequisites
- Completion of Lessons 1-3 (HTML, CSS, JavaScript fundamentals)
- A computer running Mac, Windows, or Linux
- Internet connection for downloading tools and accessing cloud services
- Basic familiarity with file systems and command line (helpful but not required)
- The Bitcoin wallet project from lesson 3

## Key Learning Objectives
- **Set up a professional local development environment** with Visual Studio Code and essential extensions
- **Master Git and GitHub** for version control and code collaboration
- **Understand development environments** - local, remote, and production
- **Create your first React application** and understand component-based architecture
- **Learn package management** with Node.js and npm
- **Deploy applications to production** using Vercel for real-world accessibility
- **Establish a professional workflow** that scales from personal projects to team collaboration
- **Build your developer portfolio** with GitHub as proof of work

## Why Development Environments Matter

### The Professional Developer Workflow
Real developers don't just write code in isolation. They work within ecosystems of tools, services, and processes that enable:

- **Collaboration** - Multiple developers working on the same codebase
- **Version control** - Tracking changes, rolling back mistakes, branching features
- **Quality assurance** - Linting, testing, code review processes
- **Deployment** - Getting code from development to production seamlessly
- **Maintenance** - Updating dependencies, fixing bugs, adding features

### Three Essential Environments
Modern development typically involves three key environments:

1. **Local Environment** - Your computer where you write and test code
2. **Remote Repository** - GitHub where code is stored and shared
3. **Production Environment** - Live servers where users access your application

Each serves a specific purpose and they work together to create a professional development workflow. 

## Setting Up Your Local Development Environment

### Installing Essential Tools

#### Node.js: The JavaScript Runtime
Node.js allows JavaScript to run outside of browsers and provides the foundation for modern web development tools.

**Installation:**
1. Visit [nodejs.org](https://nodejs.org/en/download/)
2. Download the LTS (Long Term Support) version for your operating system
3. Run the installer with default settings
4. Verify installation by opening terminal/command prompt and running:
   ```bash
   node --version
   npm --version
   ```

**What Node.js Provides:**
- **Runtime environment** - Execute JavaScript outside browsers
- **npm (Node Package Manager)** - Install and manage code libraries
- **Build tools** - Compile, bundle, and optimize your code
- **Development servers** - Run your applications locally

#### Visual Studio Code: Your Code Editor
Visual Studio Code is a free, powerful code editor that's become the industry standard for web development.

**Installation:**
1. Visit [code.visualstudio.com](https://code.visualstudio.com/download)
2. Download for your operating system (Mac, Windows, Linux)
3. Install with default settings
4. Launch Visual Studio Code

**Adding VS Code to PATH (Command Line Access):**
This allows you to open VS Code from the terminal with the `code` command.

1. Open Visual Studio Code
2. Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows/Linux)
3. Type "shell command" and select "Shell Command: Install 'code' command in PATH"
4. Follow the prompts to install

**Test it works:**
```bash
code --version
```

### Essential Visual Studio Code Extensions

Extensions supercharge VS Code with additional functionality. Install these essential extensions:

#### ESLint - JavaScript Code Quality
**What it does:** Analyzes your JavaScript code and highlights errors, warnings, and style issues in real-time.

**Why you need it:**
- Catches syntax errors before you run your code
- Enforces consistent code style
- Provides live documentation for functions and methods
- Helps you learn JavaScript best practices

**Installation:**
1. Open VS Code Extensions panel (Ctrl+Shift+X)
2. Search for "ESLint"
3. Click Install on the official ESLint extension

#### Prettier - Code Formatter
**What it does:** Automatically formats your code to follow consistent style rules.

**Why you need it:**
- Consistent indentation and spacing
- Automatic formatting on save
- Reduces time spent on code formatting
- Makes code more readable and professional

**Installation:**
1. Search for "Prettier - Code formatter" in Extensions
2. Install the official Prettier extension
3. Configure to format on save:
   - Go to Settings (Cmd+, or Ctrl+,)
   - Search for "format on save"
   - Check the "Editor: Format On Save" option

### Visual Studio Code Interface Overview

#### Key Areas
- **Explorer** (left sidebar) - File and folder navigation
- **Search** - Find and replace across your project
- **Source Control** - Git integration for version control
- **Extensions** - Install and manage extensions
- **Editor** - Where you write your code
- **Terminal** - Integrated command line
- **Problems** - View errors and warnings
- **Output** - See build results and logs

#### Essential Shortcuts
```
Cmd+P / Ctrl+P     - Quick file open
Cmd+Shift+P        - Command palette
Cmd+`             - Toggle terminal
Cmd+B             - Toggle sidebar
Cmd+S             - Save file
Cmd+Z             - Undo
Cmd+Shift+Z       - Redo
```

#### Integrated Terminal
The integrated terminal is one of VS Code's most powerful features:
- Access command line without leaving your editor
- Run build commands, start development servers
- Execute Git commands
- Install packages with npm

**Opening Terminal:**
- View → Terminal
- Or use shortcut: Ctrl+` (backtick)

### Code Intelligence Features

#### IntelliSense
VS Code provides intelligent code completion:
- **Auto-completion** - Suggests functions, variables, and methods
- **Parameter hints** - Shows function parameters as you type
- **Type information** - Displays data types and documentation
- **Error highlighting** - Red squiggles under syntax errors

#### Live Documentation
Hover over functions to see:
- Function signatures
- Parameter descriptions
- Return types
- Usage examples

Example with console.log:
```javascript
console.log() // Hover to see: "(method) Console.log(...data: any[]): void"
```

### Working with Projects

#### Opening Projects
Three ways to open projects in VS Code:
1. **File → Open Folder** - Browse and select your project folder
2. **Command line** - `code /path/to/your/project`
3. **Drag and drop** - Drag project folder onto VS Code icon

#### Project Structure
VS Code automatically recognizes project types:
- **package.json** - Identifies Node.js projects
- **.git folder** - Shows Git repository status
- **Language-specific files** - Enables appropriate extensions and features

### Customizing Your Environment

#### Themes and Appearance
1. **Command Palette** → "Preferences: Color Theme"
2. Popular themes: Dark+, One Dark Pro, Dracula
3. Install additional themes from Extensions marketplace

#### Settings Sync
Sync your VS Code settings across devices:
1. Sign in with GitHub account
2. Enable Settings Sync
3. Your extensions, settings, and themes sync automatically

#### Workspace Settings
Configure settings per project by creating `.vscode/settings.json`:
```json
{
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.formatOnSave": true,
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  }
}
``` 

## Git and GitHub: Version Control and Collaboration

### Understanding Git vs GitHub

#### Git: The Version Control System
Git is a **protocol** - a set of rules for tracking changes in code. Think of it as a sophisticated "Save" system that:
- **Tracks every change** made to your code
- **Enables collaboration** without conflicts
- **Allows experimentation** with branches
- **Provides backup and recovery** for your work
- **Shows detailed history** of who changed what when

Git was created in the early 2000s to solve the problem of multiple developers working on the same codebase. Without version control, this was chaos:
- Files getting overwritten
- No way to roll back changes
- Impossible to track who made what changes
- No way to experiment safely

#### GitHub: The Platform
GitHub is a **website and cloud service** built on top of Git. It provides:
- **Remote repositories** - Store your Git repositories in the cloud
- **Visual interface** - Web-based UI for Git operations
- **Collaboration tools** - Issues, pull requests, project management
- **Backup and sharing** - Your code is safe and accessible anywhere
- **Portfolio building** - Showcase your work and contributions

**Analogy:** If Git is the engine, GitHub is the car that makes it accessible and user-friendly.

### Why Every Developer Uses GitHub

#### 1. Proof of Work
GitHub acts as your developer portfolio:
- **Green squares** show daily coding activity
- **Repositories** showcase your projects
- **Contribution history** proves consistent work
- **Employers look at GitHub** profiles when hiring

#### 2. Collaboration and Open Source
- **99% of professional developers** use Git/GitHub
- **All major projects** are on GitHub (Bitcoin, Linux, React, etc.)
- **Contributing to open source** builds reputation and skills
- **Learning from others** by reading real codebases

#### 3. Backup and Accessibility
- **Never lose your work** - everything is backed up in the cloud
- **Access from anywhere** - work on any computer
- **Share easily** - send links to your projects
- **Version history** - see how your code evolved

### Essential Git Terminology

Understanding these terms is crucial for working with Git and GitHub:

#### Repository (Repo)
A **folder that contains your code** and is tracked by Git. Think of it as a project folder with superpowers.
```bash
my-bitcoin-wallet/     # This is a repository
├── index.html
├── style.css
├── script.js
└── .git/             # Hidden folder that makes it a Git repo
```

#### Commit
A **saved snapshot** of your code at a specific point in time. Each commit has:
- **Unique ID** (hash) - like a fingerprint
- **Message** - description of what changed
- **Timestamp** - when it was made
- **Author** - who made the changes

```bash
# Example commit history
commit a1b2c3d "Add Bitcoin price display feature"
commit e4f5g6h "Fix wallet balance calculation bug"  
commit i7j8k9l "Initial project setup"
```

#### Staging
**Preparing commits** before they're saved. It's like putting items in a shopping cart before checkout:
- **Working directory** - files you're editing
- **Staging area** - files ready to be committed  
- **Repository** - committed files in Git history

#### Push
**Uploading your local commits** to GitHub (remote repository). Your changes go from your computer to the cloud.

#### Pull
**Downloading changes** from GitHub to your local machine. Get updates from the remote repository.

#### Branch
A **separate version** of your code where you can experiment without affecting the main version.
- **main/master branch** - the primary, stable version
- **feature branches** - experimental work
- **merge** - combining branches back together

#### Pull Request (PR)
A **request to merge** your branch into the main branch. It includes:
- **Description** of changes made
- **Code review** process
- **Discussion** between team members
- **Approval** before merging

#### Clone
**Downloading a complete copy** of a repository from GitHub to your computer.

#### Fork
**Creating your own copy** of someone else's repository. Useful for:
- Contributing to open source projects
- Experimenting with existing code
- Creating your own version of a project

### Installing and Configuring Git

#### Installation
**Mac:** Git comes pre-installed, but you can update it:
```bash
# Check if Git is installed
git --version

# Install via Homebrew (if you have it)
brew install git
```

**Windows:** Download from [git-scm.com](https://git-scm.com/download/win)

**Linux:** 
```bash
# Ubuntu/Debian
sudo apt install git

# CentOS/RHEL
sudo yum install git
```

#### First-Time Setup
Configure Git with your identity (use the same email as your GitHub account):

```bash
# Set your name (appears in commit history)
git config --global user.name "Your Name"

# Set your email (must match GitHub account)
git config --global user.email "your.email@example.com"

# Verify configuration
git config --global --list
```

### Creating Your GitHub Account

1. **Visit [github.com](https://github.com)**
2. **Sign up** with a professional username (employers will see this)
3. **Choose a plan** - Free plan is perfect for learning
4. **Verify your email** - important for security
5. **Complete your profile** - add a photo and bio

#### Profile Tips
- **Use your real name** or professional username
- **Add a profile picture** - makes you more memorable
- **Write a bio** - mention you're learning Bitcoin development
- **Pin your best repositories** - showcase your top projects

### Your First Repository

#### Creating a Repository on GitHub
1. **Click "New repository"** (green button on GitHub)
2. **Name your repository** - use descriptive names like "bitcoin-price-tracker"
3. **Add description** - explain what the project does
4. **Make it public** - show off your work
5. **Initialize with README** - creates a starting file
6. **Click "Create repository"**

#### Cloning to Your Computer
```bash
# Navigate to where you want the project
cd ~/Desktop

# Clone the repository (replace with your URL)
git clone https://github.com/yourusername/your-repo-name.git

# Enter the project directory
cd your-repo-name

# Open in VS Code
code .
```

### Basic Git Workflow

#### The Daily Git Workflow
This is what you'll do every time you make changes:

```bash
# 1. Check what files have changed
git status

# 2. Add files to staging area
git add .                 # Add all changes
git add filename.js       # Add specific file

# 3. Commit your changes with a message
git commit -m "Add Bitcoin price fetching feature"

# 4. Push to GitHub
git push origin main
```

#### Understanding Git Status
Git status shows three types of files:
- **Red files** - modified but not staged
- **Green files** - staged and ready to commit
- **Clean working tree** - no changes to commit

```bash
# Example git status output
On branch main
Changes not staged for commit:
  modified:   index.html        # Red - needs staging
  modified:   script.js         # Red - needs staging

Changes to be committed:
  modified:   style.css         # Green - staged and ready
```

#### Writing Good Commit Messages
Commit messages should be:
- **Clear and descriptive** - explain what changed
- **Present tense** - "Add feature" not "Added feature"  
- **Specific** - "Fix wallet balance display bug" not "Fix stuff"

```bash
# Good commit messages
git commit -m "Add real-time Bitcoin price updates"
git commit -m "Fix wallet balance calculation error"
git commit -m "Improve mobile responsive design"

# Poor commit messages
git commit -m "stuff"
git commit -m "changes"
git commit -m "idk"
```

### Working with Remotes

#### Understanding Remote Repositories
- **Local repository** - Git repo on your computer
- **Remote repository** - Git repo on GitHub
- **Origin** - default name for your main remote repository

#### Remote Commands
```bash
# View your remotes
git remote -v

# Add a remote (usually done automatically when cloning)
git remote add origin https://github.com/username/repo.git

# Push to remote
git push origin main       # Push main branch
git push                   # Push current branch (if upstream is set)

# Pull from remote
git pull origin main       # Pull main branch
git pull                   # Pull current branch
```

### Branching and Merging

#### Why Use Branches?
Branches let you:
- **Experiment safely** - try new features without breaking main code
- **Work on multiple features** simultaneously
- **Collaborate effectively** - different people work on different branches
- **Review code** before it goes to production

#### Basic Branching
```bash
# Create and switch to a new branch
git checkout -b feature/bitcoin-price-display

# Switch between branches
git checkout main                    # Switch to main
git checkout feature/bitcoin-price-display  # Switch to feature branch

# See all branches
git branch

# Delete a branch (after merging)
git branch -d feature/bitcoin-price-display
```

#### Merging Branches
```bash
# Switch to main branch
git checkout main

# Merge feature branch into main
git merge feature/bitcoin-price-display

# Push merged changes
git push origin main
```

### GitHub Web Interface

#### Repository Navigation
- **Code tab** - browse files and folders
- **Issues tab** - track bugs and feature requests
- **Pull requests tab** - code review and merging
- **Actions tab** - automated workflows (CI/CD)
- **Settings tab** - repository configuration

#### Viewing Commit History
Click on "commits" to see:
- **Commit messages** and timestamps
- **File changes** (green additions, red deletions)
- **Author information**
- **Commit hashes** for referencing specific versions

#### Understanding Pull Requests
Pull requests are where professional development happens:
1. **Create feature branch** and make changes
2. **Push branch to GitHub**
3. **Open pull request** to merge into main
4. **Code review** - team members review changes
5. **Discussion** - suggest improvements
6. **Approve and merge** - changes go to main branch

### GitHub as Your Portfolio

#### Contribution Graph
The green squares on your profile show:
- **Daily activity** - commits, pull requests, issues
- **Consistency** - regular contributions over time
- **Intensity** - darker green = more activity

**Goal:** Get into the habit of making at least one commit per day when working on projects.

#### Showcasing Your Work
- **Pin repositories** - highlight your best 6 projects
- **Write good READMEs** - explain what your project does
- **Use descriptive names** - "bitcoin-lightning-wallet" not "project1"
- **Add screenshots** - show your projects in action
- **Include live demos** - link to deployed versions

#### README Best Practices
A good README includes:
```markdown
# Project Name
Brief description of what it does

## Features
- List key features
- What makes it special

## Demo
[Live Demo](https://your-app.vercel.app)

## Screenshots
![App Screenshot](screenshot.png)

## Installation
Steps to run locally

## Technologies Used
- HTML, CSS, JavaScript
- React, Node.js
- LNbits, Bitcoin APIs

## What I Learned
- New skills gained
- Challenges overcome
``` 

## Introduction to React

### What is React?

React is a **JavaScript library for building user interfaces**, particularly web applications. It was created by Facebook (now Meta) and has become the most popular way to build modern web applications.

#### Why React Matters
- **Industry standard** - Most companies use React for frontend development
- **Component-based** - Build UIs as reusable pieces
- **Easier to manage** - Organizes complex applications
- **Better developer experience** - Powerful tools and debugging
- **Job market** - High demand for React developers

#### React vs Vanilla JavaScript
In previous lessons, we built applications with HTML, CSS, and JavaScript separately. This works for small projects but becomes difficult as applications grow:

**Vanilla JavaScript challenges:**
- **Code scattered** across multiple files
- **Hard to reuse** UI components
- **Manual DOM manipulation** is error-prone
- **State management** becomes complex
- **Testing and debugging** is difficult

**React solutions:**
- **Everything in one place** - HTML, CSS, and JavaScript together
- **Reusable components** - Write once, use everywhere
- **Automatic updates** - React handles DOM changes
- **Predictable state** - Easier to understand and debug
- **Rich ecosystem** - Tools, libraries, and community support

### React Fundamentals

#### Components: The Building Blocks
React applications are built from **components** - reusable pieces of UI that combine HTML, CSS, and JavaScript:

```jsx
// A simple React component
function BitcoinPrice() {
    return (
        <div className="price-display">
            <h2>Bitcoin Price</h2>
            <p>$45,000</p>
        </div>
    );
}
```

**Key concepts:**
- **Functions that return HTML** - Components are JavaScript functions
- **JSX syntax** - Write HTML-like code in JavaScript
- **Reusable** - Use the same component multiple times
- **Composable** - Combine components to build complex UIs

#### JSX: HTML in JavaScript
JSX lets you write HTML-like syntax directly in JavaScript:

```jsx
// JSX (what you write)
const greeting = <h1>Hello, Bitcoin developers!</h1>;

// What it becomes (JavaScript)
const greeting = React.createElement('h1', null, 'Hello, Bitcoin developers!');
```

**JSX differences from HTML:**
- `className` instead of `class` (class is a JavaScript keyword)
- `onClick` instead of `onclick` (camelCase event handlers)
- Curly braces `{}` for JavaScript expressions
- Self-closing tags must have `/` (like `<img />`)

#### State: Making Components Dynamic
State allows components to remember and change data:

```jsx
import { useState } from 'react';

function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    
    // This function updates the price
    const updatePrice = (newPrice) => {
        setPrice(newPrice);
    };
    
    return (
        <div>
            <h2>Bitcoin Price: ${price}</h2>
            <button onClick={() => updatePrice(50000)}>
                Update Price
            </button>
        </div>
    );
}
```

**State concepts:**
- **useState hook** - React's way to add state to components
- **Current value** - `price` holds the current state
- **Setter function** - `setPrice` updates the state
- **Re-rendering** - React automatically updates the UI when state changes

#### Effects: Handling Side Effects
Effects let you perform operations like API calls, timers, and cleanup:

```jsx
import { useState, useEffect } from 'react';

function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    
    useEffect(() => {
        // This runs after the component mounts
        fetchBitcoinPrice().then(setPrice);
        
        // Set up interval to update price every 10 seconds
        const interval = setInterval(() => {
            fetchBitcoinPrice().then(setPrice);
        }, 10000);
        
        // Cleanup function (runs when component unmounts)
        return () => clearInterval(interval);
    }, []); // Empty dependency array = run once on mount
    
    return <div>Bitcoin Price: ${price}</div>;
}
```

**Effect concepts:**
- **useEffect hook** - Perform side effects in components
- **Dependencies array** - Control when effects run
- **Cleanup function** - Prevent memory leaks
- **Lifecycle events** - React automatically manages when effects run

### Creating Your First React App

#### Using Create React App
Create React App is a tool that sets up a complete React development environment with one command:

```bash
# Navigate to your projects folder
cd ~/Desktop

# Create a new React app
npx create-react-app pleb-wallet-react

# Enter the project directory
cd pleb-wallet-react

# Open in VS Code
code .
```

**What this creates:**
- **Complete project structure** - All necessary files and folders
- **Development server** - Live reload as you make changes
- **Build system** - Compiles and optimizes your code
- **Testing setup** - Framework for writing tests
- **Git repository** - Version control ready to go

#### Project Structure Overview
```
pleb-wallet-react/
├── public/
│   ├── index.html          # Main HTML file
│   └── favicon.ico         # Website icon
├── src/
│   ├── App.js              # Main App component
│   ├── App.css             # App styles
│   ├── index.js            # Entry point
│   └── index.css           # Global styles
├── package.json            # Project configuration
├── package-lock.json       # Dependency lock file
└── node_modules/           # Installed packages
```

#### Understanding package.json
The `package.json` file is your project's configuration center:

```json
{
  "name": "pleb-wallet-react",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
```

**Key sections:**
- **dependencies** - External packages your project uses
- **scripts** - Commands you can run (`npm start`, `npm build`, etc.)
- **name/version** - Project identification

#### Understanding node_modules
The `node_modules` folder contains all the code libraries your project depends on:
- **Massive folder** - Can contain thousands of files
- **Don't edit manually** - Managed by npm
- **Can be regenerated** - Delete and run `npm install` to recreate
- **Not in Git** - Too large for version control

#### Starting Your Development Server
```bash
# Start the development server
npm start
```

This opens your React app in the browser at `http://localhost:3000` with:
- **Live reload** - Changes appear instantly
- **Error display** - Syntax errors shown in browser
- **Hot module replacement** - Updates without full page refresh

### React App Architecture

#### How React Apps Work
React apps follow a specific architecture:

1. **index.html** - Contains a single `<div id="root"></div>`
2. **index.js** - JavaScript entry point that injects React into the HTML
3. **App.js** - Main component that contains your entire application
4. **Components** - Smaller pieces that make up your app

#### Entry Point: index.js
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

**What this does:**
- **Finds the root div** in index.html
- **Creates React root** - Tells React where to inject components
- **Renders App component** - Your main application component

#### Main Component: App.js
```jsx
import React from 'react';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Pleb Wallet</h1>
        <p>Your Bitcoin Lightning Wallet</p>
      </header>
    </div>
  );
}

export default App;
```

**Component structure:**
- **Import React** - Required for JSX
- **Import CSS** - Component-specific styles
- **Function component** - Returns JSX
- **Export default** - Makes component available to other files

### Building a Bitcoin Price Component

Let's build a real component that fetches Bitcoin prices:

#### Step 1: Create the Component
```jsx
// BitcoinPrice.js
import React, { useState, useEffect } from 'react';

function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        fetchBitcoinPrice();
        const interval = setInterval(fetchBitcoinPrice, 10000);
        return () => clearInterval(interval);
    }, []);

    const fetchBitcoinPrice = async () => {
        try {
            const response = await fetch('https://api.coinbase.com/v2/prices/BTC-USD/spot');
            const data = await response.json();
            setPrice(Number(data.data.amount));
            setLoading(false);
        } catch (error) {
            console.error('Failed to fetch Bitcoin price:', error);
            setLoading(false);
        }
    };

    if (loading) {
        return <div>Loading Bitcoin price...</div>;
    }

    return (
        <div className="bitcoin-price">
            <h2>Bitcoin Price</h2>
            <p>${price.toLocaleString()}</p>
        </div>
    );
}

export default BitcoinPrice;
```

#### Step 2: Use the Component in App.js
```jsx
import React from 'react';
import BitcoinPrice from './BitcoinPrice';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Pleb Wallet</h1>
        <BitcoinPrice />
      </header>
    </div>
  );
}

export default App;
```

#### Step 3: Add Styling
```css
/* App.css */
.bitcoin-price {
    background: #f7931a;
    color: white;
    padding: 20px;
    border-radius: 10px;
    margin: 20px 0;
    text-align: center;
}

.bitcoin-price h2 {
    margin: 0 0 10px 0;
}

.bitcoin-price p {
    font-size: 2em;
    font-weight: bold;
    margin: 0;
}
```

### Package Management with NPM

#### What is NPM?
NPM (Node Package Manager) is the world's largest software registry containing over 2 million packages:
- **Free and open source** - Anyone can publish packages
- **Quality packages** - Battle-tested by millions of developers
- **Easy installation** - One command to add functionality
- **Dependency management** - Automatically handles package dependencies

#### Installing Packages
```bash
# Install a package for your project
npm install axios                  # API client
npm install react-qr-code          # QR code generator

# Install development-only packages
npm install --save-dev eslint      # Code linting (dev only)

# Install packages globally (available system-wide)
npm install -g create-react-app    # Global CLI tools
```

#### Package.json Updates
When you install packages, `package.json` automatically updates:

```json
{
  "dependencies": {
    "axios": "^1.6.0",
    "react": "^18.2.0",
    "react-qr-code": "^2.0.11"
  }
}
```

#### Using Installed Packages
```jsx
// Import and use installed packages
import axios from 'axios';
import QRCode from 'react-qr-code';

function PaymentRequest() {
    const [invoice, setInvoice] = useState('');

    const fetchInvoice = async () => {
        const response = await axios.get('/api/invoice');
        setInvoice(response.data.payment_request);
    };

    return (
        <div>
            <button onClick={fetchInvoice}>Generate Invoice</button>
            {invoice && <QRCode value={invoice} />}
        </div>
    );
}
```

#### Package Security and Quality
When choosing packages, consider:
- **Download count** - More downloads usually mean better quality
- **Recent updates** - Active maintenance is important
- **GitHub stars** - Community approval indicator
- **Documentation** - Good docs mean easier integration
- **Bundle size** - Smaller packages = faster loading

#### Common Useful Packages
```bash
# API and HTTP requests
npm install axios

# Date manipulation
npm install date-fns

# QR code generation
npm install react-qr-code

# Bitcoin utilities
npm install bitcoin-price-api

# UI component libraries
npm install @mui/material           # Material-UI
npm install react-bootstrap         # Bootstrap components

# State management
npm install zustand                 # Simple state management
```

### React Development Workflow

#### Daily Development Process
1. **Start development server** - `npm start`
2. **Edit components** - Make changes in VS Code
3. **See changes live** - Browser updates automatically
4. **Test functionality** - Click around, test features
5. **Commit changes** - `git add`, `git commit`, `git push`

#### Hot Reloading
React's development server provides hot reloading:
- **Save file** → Browser updates instantly
- **Syntax errors** → Show in browser overlay
- **State preserved** → No need to recreate app state
- **Fast iteration** → See changes immediately

#### Debugging React Apps

#### Browser Developer Tools
React provides additional debugging tools:
- **Install React Developer Tools** browser extension
- **Components tab** - Inspect React component tree
- **Profiler tab** - Analyze performance
- **State inspection** - See component state and props

#### Common Debugging Techniques
```jsx
function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    
    // Console logging for debugging
    console.log('Current price:', price);
    
    useEffect(() => {
        console.log('Component mounted');
        fetchPrice();
        
        return () => {
            console.log('Component unmounting');
        };
    }, []);
    
    const fetchPrice = async () => {
        try {
            const data = await fetch('/api/price');
            console.log('API response:', data);
            setPrice(data.price);
        } catch (error) {
            console.error('API error:', error);
        }
    };
    
    return <div>Price: ${price}</div>;
}
``` 

## Deploying to Production with Vercel

### What is Production Deployment?

**Production deployment** means making your application available to real users on the internet. Up until now, your app only runs on your computer (`localhost:3000`). Deployment puts it on a real URL that anyone can visit.

### Why Vercel?

Vercel is a deployment platform specifically designed for frontend applications:
- **Free tier** - Perfect for learning and small projects
- **Automatic deployments** - Connects directly to GitHub
- **Global CDN** - Fast loading worldwide
- **HTTPS by default** - Secure connections
- **Custom domains** - Use your own domain name
- **Zero configuration** - Works out of the box with React

### Setting Up Vercel

#### Creating Your Account
1. **Visit [vercel.com](https://vercel.com)**
2. **Sign up with GitHub** - This connects your repositories automatically
3. **Complete profile** - Add your name and details
4. **Choose hobby plan** - Free tier is perfect for learning

#### Connecting Your Repository
Once your React app is pushed to GitHub:

1. **Click "New Project"** on Vercel dashboard
2. **Import from GitHub** - Vercel lists your repositories
3. **Select your React app** repository
4. **Configure project** - Vercel automatically detects it's a React app
5. **Click "Deploy"** - Your app starts building

#### Automatic Configuration
Vercel automatically detects:
- **Framework** - Identifies Create React App
- **Build command** - `npm run build`
- **Output directory** - `build/`
- **Node.js version** - Uses latest stable version

### Understanding the Deployment Process

#### What Happens During Deployment
1. **Code checkout** - Vercel downloads your GitHub repository
2. **Install dependencies** - Runs `npm install`
3. **Build application** - Runs `npm run build`
4. **Optimize assets** - Compresses images, minifies code
5. **Deploy to CDN** - Distributes globally for fast access
6. **Generate URL** - Creates your live application URL

#### Build Process Details
```bash
# What `npm run build` does:
✅ Compiles JSX to JavaScript
✅ Bundles all components into optimized files
✅ Minifies CSS and JavaScript
✅ Optimizes images and assets
✅ Generates service worker for caching
✅ Creates production-ready files in build/ folder
```

### Live Deployment Workflow

#### The Complete Workflow
This is the professional development cycle you'll use:

```bash
# 1. Make changes locally
code .                          # Edit in VS Code
npm start                       # Test locally

# 2. Commit changes
git add .
git commit -m "Add new feature"
git push origin main

# 3. Automatic deployment
# Vercel detects the GitHub push
# Automatically builds and deploys
# New version is live in ~30 seconds
```

#### Watching Deployments
In your Vercel dashboard, you can:
- **See build progress** - Real-time deployment logs
- **View deployment history** - Every version ever deployed
- **Rollback if needed** - Instantly revert to previous version
- **Preview branches** - Test feature branches before merging

#### Environment Variables
For sensitive data like API keys:

```bash
# In Vercel dashboard → Settings → Environment Variables
REACT_APP_API_KEY=your_secret_key_here
REACT_APP_LNBITS_URL=https://demo.lnbits.com
```

**Using environment variables in React:**
```jsx
const apiKey = process.env.REACT_APP_API_KEY;
const lnbitsUrl = process.env.REACT_APP_LNBITS_URL;
```

**Important:** Only variables starting with `REACT_APP_` are included in the build.

### Custom Domains

#### Adding Your Own Domain
Once you're ready for a professional presence:

1. **Buy a domain** - From providers like Namecheap, GoDaddy
2. **Add to Vercel** - Project Settings → Domains
3. **Configure DNS** - Point your domain to Vercel
4. **Automatic HTTPS** - Vercel provides SSL certificates

Example: `your-bitcoin-wallet.com` instead of `random-name.vercel.app`

### Project: Complete Development to Deployment

Let's build and deploy a complete Bitcoin price tracker:

#### Step 1: Create React App
```bash
# Create new React app
npx create-react-app bitcoin-price-tracker
cd bitcoin-price-tracker

# Install additional packages
npm install axios

# Open in VS Code
code .
```

#### Step 2: Build Bitcoin Price Component
```jsx
// src/BitcoinPrice.js
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import './BitcoinPrice.css';

function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    const [previousPrice, setPreviousPrice] = useState(0);
    const [loading, setLoading] = useState(true);
    const [lastUpdated, setLastUpdated] = useState(null);

    useEffect(() => {
        fetchPrice();
        const interval = setInterval(fetchPrice, 30000); // Update every 30 seconds
        return () => clearInterval(interval);
    }, []);

    const fetchPrice = async () => {
        try {
            const response = await axios.get('https://api.coinbase.com/v2/prices/BTC-USD/spot');
            const newPrice = Number(response.data.data.amount);
            
            setPreviousPrice(price);
            setPrice(newPrice);
            setLastUpdated(new Date());
            setLoading(false);
        } catch (error) {
            console.error('Error fetching price:', error);
            setLoading(false);
        }
    };

    const getPriceDirection = () => {
        if (price > previousPrice) return 'up';
        if (price < previousPrice) return 'down';
        return 'same';
    };

    const formatPrice = (price) => {
        return new Intl.NumberFormat('en-US', {
            style: 'currency',
            currency: 'USD',
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
        }).format(price);
    };

    if (loading) {
        return (
            <div className="bitcoin-price loading">
                <div className="spinner"></div>
                <p>Loading Bitcoin price...</p>
            </div>
        );
    }

    return (
        <div className={`bitcoin-price ${getPriceDirection()}`}>
            <h1>₿ Bitcoin Price</h1>
            <div className="price-display">
                <span className="price">{formatPrice(price)}</span>
                <span className="direction">
                    {getPriceDirection() === 'up' && '↗'}
                    {getPriceDirection() === 'down' && '↘'}
                    {getPriceDirection() === 'same' && '→'}
                </span>
            </div>
            {lastUpdated && (
                <p className="last-updated">
                    Last updated: {lastUpdated.toLocaleTimeString()}
                </p>
            )}
            <button onClick={fetchPrice} className="refresh-btn">
                Refresh Price
            </button>
        </div>
    );
}

export default BitcoinPrice;
```

#### Step 3: Add Styling
```css
/* src/BitcoinPrice.css */
.bitcoin-price {
    max-width: 600px;
    margin: 2rem auto;
    padding: 2rem;
    border-radius: 12px;
    background: linear-gradient(135deg, #f7931a, #ff6b35);
    color: white;
    text-align: center;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
}

.bitcoin-price.up {
    background: linear-gradient(135deg, #00d4aa, #00a085);
}

.bitcoin-price.down {
    background: linear-gradient(135deg, #ff4757, #ff3742);
}

.bitcoin-price h1 {
    margin: 0 0 1rem 0;
    font-size: 2rem;
    font-weight: 300;
}

.price-display {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin: 1.5rem 0;
}

.price {
    font-size: 3rem;
    font-weight: bold;
    letter-spacing: -2px;
}

.direction {
    font-size: 2rem;
    animation: pulse 2s infinite;
}

.last-updated {
    margin: 1rem 0;
    opacity: 0.8;
    font-size: 0.9rem;
}

.refresh-btn {
    background: rgba(255, 255, 255, 0.2);
    border: 2px solid rgba(255, 255, 255, 0.3);
    color: white;
    padding: 0.75rem 1.5rem;
    border-radius: 25px;
    cursor: pointer;
    font-size: 1rem;
    transition: all 0.3s ease;
}

.refresh-btn:hover {
    background: rgba(255, 255, 255, 0.3);
    transform: translateY(-2px);
}

.loading {
    background: linear-gradient(135deg, #667eea, #764ba2);
}

.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid rgba(255, 255, 255, 0.3);
    border-top: 4px solid white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 1rem auto;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

@keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.1); }
}

@media (max-width: 768px) {
    .bitcoin-price {
        margin: 1rem;
        padding: 1.5rem;
    }
    
    .price {
        font-size: 2.5rem;
    }
    
    .bitcoin-price h1 {
        font-size: 1.5rem;
    }
}
```

#### Step 4: Update App.js
```jsx
// src/App.js
import React from 'react';
import BitcoinPrice from './BitcoinPrice';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <BitcoinPrice />
        <footer>
          <p>Made with ❤️ by a Bitcoin pleb</p>
          <p>Data from Coinbase API</p>
        </footer>
      </header>
    </div>
  );
}

export default App;
```

#### Step 5: Push to GitHub
```bash
# Initialize Git (if not already done)
git init
git add .
git commit -m "Initial Bitcoin price tracker"

# Create repository on GitHub and push
git remote add origin https://github.com/yourusername/bitcoin-price-tracker.git
git branch -M main
git push -u origin main
```

#### Step 6: Deploy to Vercel
1. **Go to Vercel dashboard**
2. **Click "New Project"**
3. **Import your GitHub repository**
4. **Click "Deploy"**
5. **Wait for deployment to complete**
6. **Visit your live URL!**

### Continuous Deployment

#### What is Continuous Deployment?
Once connected to GitHub, every push to your main branch automatically triggers a new deployment:

```bash
# Make a change
echo "Added new feature" >> README.md

# Commit and push
git add .
git commit -m "Update documentation"
git push origin main

# Vercel automatically:
# 1. Detects the push
# 2. Starts building
# 3. Deploys new version
# 4. Updates live URL
```

#### Branch Previews
Vercel also creates preview deployments for feature branches:
```bash
# Create feature branch
git checkout -b feature/add-charts

# Make changes and push
git add .
git commit -m "Add price charts"
git push origin feature/add-charts

# Vercel creates a preview URL just for this branch
# Perfect for testing before merging to main
```

## Hands-on Exercises

### Exercise 1: Complete Development Environment Setup
Set up your professional development environment:

1. **Install all required tools:**
   - Node.js and npm
   - Visual Studio Code
   - Git
   - Essential VS Code extensions (ESLint, Prettier)

2. **Create GitHub account and profile:**
   - Professional username
   - Profile picture and bio
   - Pin your best repositories

3. **Test your setup:**
   - Create a new React app
   - Open in VS Code
   - Start development server
   - Make changes and see live updates

### Exercise 2: Git Workflow Practice
Master the basic Git workflow:

1. **Create a new repository:**
   - Initialize locally with `git init`
   - Create on GitHub
   - Connect local to remote
   - Push initial commit

2. **Practice daily workflow:**
   - Make changes to files
   - Use `git status` to check changes
   - Stage with `git add`
   - Commit with descriptive messages
   - Push to GitHub

3. **Experiment with branches:**
   - Create feature branch
   - Make changes on branch
   - Switch between branches
   - Merge feature into main

### Exercise 3: React Component Building
Build a collection of Bitcoin-related React components:

1. **Bitcoin News Component:**
   ```jsx
   // Fetch from a crypto news API
   // Display latest Bitcoin headlines
   // Add refresh functionality
   // Include loading states
   ```

2. **Satoshi Quote Generator:**
   ```jsx
   // Array of famous Satoshi quotes
   // Random quote on button click
   // Tweet quote functionality
   // Copy to clipboard feature
   ```

3. **Block Height Tracker:**
   ```jsx
   // Fetch current Bitcoin block height
   // Show time since last block
   // Display mining difficulty
   // Real-time updates
   ```

### Exercise 4: Full-Stack Bitcoin App
Create a comprehensive Bitcoin dashboard:

1. **Plan your application:**
   - Sketch UI layout
   - List required components
   - Identify data sources
   - Plan user interactions

2. **Build components:**
   - Price display with charts
   - News feed
   - Calculator (USD ↔ BTC)
   - Lightning network stats

3. **Add advanced features:**
   - Local storage for favorites
   - Dark/light theme toggle
   - Responsive design
   - Progressive Web App features

4. **Deploy and share:**
   - Push to GitHub
   - Deploy to Vercel
   - Add custom domain
   - Share with the Bitcoin community

### Exercise 5: Open Source Contribution
Contribute to the Bitcoin development community:

1. **Find a project:**
   - Browse Bitcoin-related repositories
   - Look for "good first issue" labels
   - Read contribution guidelines
   - Fork the repository

2. **Make your contribution:**
   - Fix a bug or add a feature
   - Write good commit messages
   - Test your changes thoroughly
   - Submit a pull request

3. **Engage with maintainers:**
   - Respond to feedback professionally
   - Make requested changes
   - Learn from code review
   - Celebrate when merged!

## Learning Resources

### Official Documentation
- **[React Documentation](https://react.dev/)** - Official React docs with interactive examples
- **[Create React App](https://create-react-app.dev/)** - Complete setup and configuration guide
- **[Node.js Documentation](https://nodejs.org/docs/)** - Node.js and npm reference
- **[Git Documentation](https://git-scm.com/doc)** - Complete Git command reference
- **[GitHub Guides](https://guides.github.com/)** - Step-by-step GitHub tutorials
- **[Vercel Documentation](https://vercel.com/docs)** - Deployment and hosting guide

### Development Tools
- **[Visual Studio Code](https://code.visualstudio.com/docs)** - Editor documentation and tutorials
- **[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/)** - Browser extension for debugging
- **[Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)** - Quick Git command reference
- **[npm Documentation](https://docs.npmjs.com/)** - Package management guide

### Bitcoin Development Resources
- **[Bitcoin Developer Documentation](https://developer.bitcoin.org/)** - Official Bitcoin development guide
- **[Lightning Network Resources](https://lightning.network/lightning-network-paper.pdf)** - Lightning Network whitepaper
- **[LNbits Documentation](https://lnbits.com/)** - Lightning wallet and account system
- **[Bitcoin APIs](https://bitcoinbook.info/wp-content/themes/bitcoinbook/assets/appendix-bitcoin-apis.html)** - List of Bitcoin APIs

### React Learning Path
- **[React Tutorial](https://react.dev/learn)** - Interactive tutorial from React team
- **[React Hooks Guide](https://react.dev/reference/react)** - Complete hooks reference
- **[React Patterns](https://reactpatterns.com/)** - Common React patterns and best practices
- **[React Performance](https://kentcdodds.com/blog/fix-the-slow-render-before-you-fix-the-re-render)** - Optimization techniques

### Project Ideas and Inspiration
- **[GitHub Explore](https://github.com/explore)** - Discover trending repositories
- **[Awesome Bitcoin](https://github.com/igorbarinov/awesome-bitcoin)** - Curated Bitcoin resources
- **[React Projects](https://github.com/topics/react-projects)** - Example React applications
- **[Bitcoin Development Examples](https://github.com/bitcoinbook/bitcoinbook)** - Mastering Bitcoin code examples

## Best Practices and Tips

### Development Environment
1. **Keep tools updated** - Regularly update Node.js, VS Code, and extensions
2. **Use consistent formatting** - Let Prettier handle code formatting
3. **Learn keyboard shortcuts** - Speed up your development workflow
4. **Organize your workspace** - Keep projects organized in folders
5. **Use meaningful names** - For files, functions, and variables

### Git and GitHub
1. **Commit frequently** - Small, focused commits are easier to understand
2. **Write clear commit messages** - Explain what and why, not how
3. **Use branches for features** - Keep main branch stable
4. **Review before pushing** - Use `git diff` to check changes
5. **Keep README updated** - Document your project clearly

### React Development
1. **Component naming** - Use PascalCase for component names
2. **File organization** - One component per file, group related files
3. **State management** - Keep state as local as possible
4. **Effect cleanup** - Always clean up intervals, subscriptions
5. **Error boundaries** - Handle errors gracefully
6. **Performance** - Use React DevTools to identify bottlenecks

### Code Quality
1. **Use TypeScript** - Add type safety as you advance
2. **Write tests** - Start with simple unit tests
3. **Handle errors** - Always include try/catch for async operations
4. **Validate inputs** - Check user inputs and API responses
5. **Document complex logic** - Add comments for future self

### Security Considerations
1. **Never commit secrets** - Use environment variables
2. **Validate API responses** - Don't trust external data
3. **Use HTTPS** - Always use secure connections
4. **Keep dependencies updated** - Regularly update packages
5. **Follow security advisories** - Subscribe to security notifications

## Next Steps and Career Development

### Immediate Next Steps
1. **Complete the course project** - Build and deploy your Bitcoin wallet
2. **Practice daily** - Code a little bit every day
3. **Join communities** - Bitcoin development Discord/Telegram groups
4. **Read code** - Study open source Bitcoin projects
5. **Document learning** - Write about what you build

### Advanced Topics to Explore
- **TypeScript** - Add static typing to JavaScript
- **Testing** - Jest, React Testing Library
- **State Management** - Redux, Zustand, Context API
- **Styling Solutions** - Styled Components, Tailwind CSS
- **Build Tools** - Webpack, Vite, custom configurations
- **Backend Development** - Node.js, Express, databases
- **Blockchain Integration** - Web3, Bitcoin libraries

### Building Your Portfolio
1. **GitHub presence** - Maintain consistent green squares
2. **Project documentation** - Write excellent READMEs
3. **Live demos** - Deploy projects for others to see
4. **Blog about learning** - Share your journey
5. **Contribute to open source** - Build reputation in community

### Career Opportunities
- **Frontend Developer** - Focus on user interfaces
- **Full-Stack Developer** - Frontend + backend skills
- **Bitcoin Developer** - Specialize in Bitcoin/Lightning
- **DevOps Engineer** - Focus on deployment and infrastructure
- **Technical Writer** - Document Bitcoin technology
- **Open Source Maintainer** - Lead Bitcoin projects

### Continuing Education
- **Advanced React patterns** - Compound components, render props
- **Performance optimization** - Code splitting, lazy loading
- **Accessibility** - Making apps usable for everyone
- **Progressive Web Apps** - Native app-like experiences
- **Server-side rendering** - Next.js, Gatsby
- **Mobile development** - React Native

## Key Takeaways

1. **Professional development requires proper tooling** - VS Code, Git, Node.js are industry standards
2. **Version control is essential** - Git and GitHub are used by every professional developer
3. **React simplifies complex applications** - Component-based architecture scales better than vanilla JavaScript
4. **Deployment should be automatic** - Continuous deployment from GitHub to production
5. **Package management saves time** - npm ecosystem provides solutions for common problems
6. **Practice builds understanding** - Set up your environment and build projects daily
7. **Community involvement accelerates learning** - Contribute to open source and engage with developers
8. **Portfolio building starts now** - Every project you build showcases your skills

Congratulations! You've learned to set up a professional development environment and understand the modern web development workflow. You can now build React applications, manage code with Git, and deploy to production. These are the exact same tools and processes used by developers at major tech companies.

The next lesson will dive deeper into React and build a complete Lightning wallet application. Keep practicing, keep building, and welcome to the professional development community! 🚀

---

## What's Next?

In future lessons, you'll learn:
- **Advanced React patterns** - Custom hooks, context, performance optimization
- **Lightning Network integration** - Building real Bitcoin applications
- **Backend development** - APIs, databases, authentication
- **Production best practices** - Testing, monitoring, scaling
- **Career preparation** - Portfolio building, interview skills, job hunting

The journey continues, and you're well-equipped for what comes next! ⚡️🧡 