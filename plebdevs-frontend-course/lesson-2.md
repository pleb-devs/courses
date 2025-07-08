[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-2.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-2.mp4)

# Learn CSS: Styling Your Bitcoin Wallet

## Lesson Overview
Welcome to lesson 2 of the PlebDevs Frontend Course! In this lesson, we'll dive deep into CSS (Cascading Style Sheets) and learn how to transform our plain HTML into beautiful, responsive web applications. We'll build a complete Bitcoin wallet interface called "Pleb Wallet" that looks professional and works on both desktop and mobile devices.

## Prerequisites
- Completion of Lesson 1 (HTML fundamentals)
- Text editor (VS Code, Sublime Text, or similar)
- Web browser with developer tools
- Basic understanding of HTML elements and structure

## Key Learning Objectives
- Understand what CSS is and how it works with HTML
- Master CSS properties, units, and selectors
- Learn CSS specificity and the cascade
- Implement responsive design with media queries
- Use Flexbox for modern layouts
- Build a complete Bitcoin wallet interface
- Apply CSS best practices and organization

## What is CSS?

### Definition
CSS (Cascading Style Sheets) is a rule-based language that describes how HTML elements should be displayed. While HTML provides the structure and content, CSS handles the presentation - colors, fonts, layout, spacing, and visual effects.

### Why CSS Matters
- **Separates content from presentation** - Keep HTML clean and semantic
- **Enables responsive design** - Adapt to different screen sizes
- **Provides consistency** - Maintain unified styling across pages
- **Enhances user experience** - Create engaging, professional interfaces

### CSS Rule Structure
```css
selector {
    property: value;
    property: value;
}
```

## CSS Integration Methods

### 1. Inline Styles (Avoid)
```html
<p style="color: blue; font-size: 16px;">This is inline styling</p>
```
**Why avoid:** Hard to maintain, violates separation of concerns, poor reusability.

### 2. Internal Styles (Limited Use)
```html
<head>
    <style>
        p {
            color: blue;
            font-size: 16px;
        }
    </style>
</head>
```

### 3. External Stylesheets (Recommended)
```html
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

**Benefits:**
- Clean separation of concerns
- Reusable across multiple pages
- Better organization and maintainability
- Improved performance (caching)

## CSS Properties Fundamentals

### Understanding Properties
CSS properties are the styles you apply to elements. Each property has a specific purpose and accepts certain values.

### Property Syntax
```css
element {
    property-name: value;
    another-property: another-value;
}
```

### The Cascade
CSS properties are read **top to bottom** and can be overwritten:

```css
p {
    color: pink;
    color: yellow; /* This wins - yellow text */
}
```

You can use this strategically:
```css
.footer {
    margin: 1% auto; /* General margin */
    margin-bottom: 0;  /* Specific override */
}
```

### Essential CSS Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `color` | Text color | `color: #8a4fff;` |
| `font-size` | Text size | `font-size: 1.2rem;` |
| `font-family` | Font type | `font-family: monospace;` |
| `font-weight` | Text boldness | `font-weight: bold;` |
| `background-color` | Background color | `background-color: #192734;` |
| `margin` | Outer spacing | `margin: 10px;` |
| `padding` | Inner spacing | `padding: 1%;` |
| `border` | Element border | `border: 2px solid #ffbf46;` |
| `border-radius` | Rounded corners | `border-radius: 5px;` |
| `width` | Element width | `width: 100%;` |
| `height` | Element height | `height: 300px;` |

## CSS Units

### Understanding Units
CSS units define measurements for properties like width, height, margin, and font-size.

### Absolute Units
Fixed measurements that remain the same across devices:
- **px (pixels)** - `1px = 1/96th of 1 inch`
- Use for: borders, small fixed elements

### Relative Units
Measurements relative to other elements or viewport:

| Unit | Description | Use Case |
|------|-------------|----------|
| `%` | Relative to parent element | Responsive widths |
| `rem` | Relative to root font size | Scalable typography |
| `em` | Relative to current font size | Component-based sizing |
| `vw` | 1% of viewport width | Full-width elements |
| `vh` | 1% of viewport height | Full-height elements |

### Unit Examples
```css
.container {
    width: 80%;        /* 80% of parent width */
    padding: 2rem;     /* 2x root font size */
    font-size: 1.2rem; /* 1.2x root font size */
    border: 1px solid black; /* 1 pixel border */
}

.hero {
    height: 100vh;     /* Full viewport height */
    width: 100vw;      /* Full viewport width */
}
```

## CSS Selectors

### What Are Selectors?
Selectors determine which HTML elements receive your CSS styles. They're patterns that match elements in your HTML.

### Basic Selector Types

#### 1. Universal Selector
```css
* {
    box-sizing: border-box; /* Applies to all elements */
}
```

#### 2. Type Selector (Element Selector)
```css
p {
    color: blue; /* All paragraphs */
}

h1 {
    font-size: 2rem; /* All h1 elements */
}
```

#### 3. Class Selector (Most Important)
```css
.button {
    background-color: #ffbf46;
    padding: 10px;
}
```

HTML usage:
```html
<button class="button">Click me</button>
<div class="button">Styled div</div>
```

#### 4. ID Selector (Avoid for Styling)
```css
#header {
    background-color: red;
}
```

**Why avoid IDs:** Too specific, hard to override, poor reusability.

### Combining Selectors

#### Descendant Selectors
```css
/* Any p inside a footer */
footer p {
    color: #8a4fff;
}

/* Any p inside an element with class balance-card */
.balance-card p {
    font-weight: bold;
}
```

#### Multiple Classes
```css
/* Element must have both classes */
.button.primary {
    background-color: blue;
}
```

### CSS Specificity

#### How Specificity Works
When multiple rules target the same element, specificity determines which rule wins:

1. **Inline styles** (avoid) - Highest specificity
2. **IDs** (avoid for styling) - High specificity  
3. **Classes** (use these) - Medium specificity
4. **Elements** (use for general styling) - Low specificity

#### Specificity Example
```css
p {
    color: yellow; /* Specificity: 1 */
}

.text {
    color: blue; /* Specificity: 10 - This wins */
}

div p {
    color: red; /* Specificity: 2 */
}
```

### Pseudo-Classes

#### Hover Effects
```css
.button:hover {
    cursor: pointer;
    opacity: 0.8;
}
```

#### Common Pseudo-Classes
- `:hover` - Mouse over element
- `:focus` - Element has focus
- `:active` - Element is being clicked
- `:first-child` - First child element
- `:last-child` - Last child element

## CSS Flexbox

### What is Flexbox?
Flexbox is a modern layout method that makes it easy to align and distribute space among items in a container, even when their size is unknown or dynamic.

### Basic Flexbox Setup
```css
.container {
    display: flex; /* Activates flexbox */
    flex-direction: row; /* Default: horizontal */
    justify-content: space-between; /* Distribute space */
    align-items: center; /* Vertical alignment */
}
```

### Key Flexbox Properties

#### Container Properties
```css
.flex-container {
    display: flex;
    flex-direction: row | column;
    justify-content: flex-start | center | flex-end | space-between | space-around | space-evenly;
    align-items: stretch | flex-start | center | flex-end;
    flex-wrap: nowrap | wrap;
}
```

#### Item Properties
```css
.flex-item {
    flex-grow: 1; /* Grow to fill space */
    flex-shrink: 1; /* Shrink if needed */
    flex-basis: auto; /* Initial size */
}
```

### Flexbox Examples
```css
/* Horizontal button row */
.buttons {
    display: flex;
    justify-content: space-around;
    gap: 10px;
}

/* Centered content */
.hero {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

/* Responsive cards */
.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}
```

## Responsive Design with Media Queries

### Understanding Media Queries
Media queries allow you to apply different styles based on device characteristics like screen width, height, or orientation.

### Basic Media Query Syntax
```css
@media (max-width: 768px) {
    /* Styles for screens 768px and smaller */
    .container {
        width: 100%;
        padding: 10px;
    }
}
```

### Common Breakpoints
```css
/* Mobile first approach */
.container {
    width: 100%; /* Mobile default */
}

/* Tablet and up */
@media (min-width: 768px) {
    .container {
        width: 80%;
    }
}

/* Desktop and up */
@media (min-width: 1024px) {
    .container {
        width: 60%;
    }
}
```

### Media Query Types
```css
/* Width-based */
@media (max-width: 600px) { }
@media (min-width: 600px) { }

/* Height-based */
@media (max-height: 400px) { }

/* Orientation */
@media (orientation: landscape) { }
@media (orientation: portrait) { }
```

## Building the Pleb Wallet Project

### Project Overview
We'll build a complete Bitcoin wallet interface featuring:
- Responsive design (desktop and mobile)
- Modern CSS techniques
- Professional styling
- Interactive hover effects

### Step 1: Project Setup

#### Create Project Structure
```
pleb-wallet/
├── index.html
├── index.css
└── BTCUSD.png
```

#### HTML Boilerplate
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Pleb Wallet</title>
    <link rel="stylesheet" href="index.css">
</head>
<body>
    <p>Hello world!</p>
</body>
</html>
```

#### Test CSS Connection
```css
/* index.css */
p {
    color: green;
}
```

### Step 2: HTML Structure

```html
<body>
    <header>
        <h1>Pleb Wallet</h1>
    </header>
    
    <main>
        <div class="buttons">
            <button>Send</button>
            <button>Receive</button>
        </div>
        
        <div class="row">
            <div class="balance-card">
                <h2>Balance</h2>
                <p>897900 sats</p>
            </div>
            <div class="balance-card">
                <h2>Price</h2>
                <p>$19,364</p>
            </div>
        </div>
        
        <div class="row">
            <div class="row-item">
                <h3>Transactions</h3>
                <p class="transaction">Sent to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">-2200 sats</p>
                <p class="transaction">Received from bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+100 sats</p>
                <p class="transaction">Received to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+900000 sats</p>
                <p class="transaction">Sent to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">-2200 sats</p>
                <p class="transaction">Received from bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+100 sats</p>
                <p class="transaction">Received to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+900000 sats</p>
            </div>
            <div class="row-item">
                <img src="./BTCUSD.png" alt="Bitcoin Price Chart" />
            </div>
        </div>
    </main>
    
    <footer>
        <p>Made by plebs, for plebs.</p>
    </footer>
</body>
```

### Step 3: Base Styles (Type Selectors)

```css
/* Global styles */
body {
    background-color: #192734;
    font-family: monospace;
    margin: 0;
    padding: 0;
}

/* Header */
header {
    border-bottom: 2px solid #ffbf46;
}

h1 {
    text-align: center;
    color: #8a4fff;
    margin: 20px 0;
}

/* Images */
img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

/* Footer */
footer {
    border-top: 2px solid #ffbf46;
    padding: 1%;
    text-align: center;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: #192734;
}

footer p {
    color: #8a4fff;
    margin: 0;
}
```

### Step 4: Component Styles (Class Selectors)

```css
/* Button container */
.buttons {
    width: 50%;
    margin: 0 auto;
    margin-top: 3%;
    display: flex;
    justify-content: space-around;
}

/* Button styles */
.buttons button {
    background-color: #ffbf46;
    border: 2px solid #8a4fff;
    border-radius: 5px;
    padding: 10px 15px;
    font-size: 1.2rem;
    width: 100px;
    cursor: pointer;
    transition: opacity 0.3s ease;
}

.buttons button:hover {
    opacity: 0.6;
}

/* Balance cards */
.balance-card {
    background-color: #ffbf46;
    border: 2px solid #8a4fff;
    padding: 1%;
    width: 25%;
    margin-top: 3%;
    margin-bottom: 1%;
    border-radius: 5px;
}

.balance-card h2 {
    margin: 0 0 10px 0;
    color: #192734;
}

.balance-card p {
    font-size: 1.2rem;
    font-weight: bold;
    margin: 0;
    color: #192734;
}

/* Layout rows */
.row {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 0 20px;
}

/* Row items */
.row-item {
    background-color: ghostwhite;
    border: 2px solid #8a4fff;
    border-radius: 5px;
    height: 300px;
    width: 40%;
    overflow: scroll;
    padding: 10px;
}

.row-item h3 {
    margin: 0 0 15px 0;
    color: #192734;
}

.row-item p {
    margin: 5px 0;
    color: #192734;
}

/* Transaction styles */
.transaction-amount {
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
    margin-bottom: 10px;
    font-weight: bold;
}
```

### Step 5: Responsive Design

```css
/* Tablet breakpoint */
@media (max-width: 876px) {
    .row-item {
        width: 100%;
        height: 200px;
        margin-bottom: 20px;
    }
    
    .balance-card {
        width: 100%;
        text-align: center;
        margin-right: 2%;
        margin-left: 2%;
    }
    
    .buttons {
        width: 80%;
    }
}

/* Mobile breakpoint */
@media (max-width: 615px) {
    .row {
        flex-direction: column;
        padding: 0 10px;
    }
    
    .row-item {
        width: 100%;
        margin-top: 15px;
    }
    
    .balance-card {
        width: 80%;
        margin: 15px auto;
    }
    
    .buttons {
        width: 90%;
        margin-top: 20px;
    }
    
    .buttons button {
        width: 80px;
        font-size: 1rem;
    }
    
    /* Add padding to prevent footer overlap */
    main {
        padding-bottom: 80px;
    }
}
```

## Advanced CSS Techniques

### Box Model Understanding
```css
/* Better box model */
* {
    box-sizing: border-box;
}
```

This ensures padding and borders are included in element width calculations.

### CSS Custom Properties (Variables)
```css
:root {
    --primary-color: #8a4fff;
    --secondary-color: #ffbf46;
    --background-color: #192734;
    --text-color: white;
}

.button {
    background-color: var(--secondary-color);
    color: var(--background-color);
}
```

### Transitions and Animations
```css
.button {
    transition: all 0.3s ease;
}

.button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}
```

### CSS Grid (Alternative to Flexbox)
```css
.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
}
```

## CSS Organization Best Practices

### 1. File Structure
```
styles/
├── base/
│   ├── reset.css
│   └── typography.css
├── components/
│   ├── buttons.css
│   └── cards.css
├── layout/
│   ├── header.css
│   └── footer.css
└── main.css
```

### 2. Naming Conventions (BEM)
```css
/* Block Element Modifier */
.wallet-card { }              /* Block */
.wallet-card__title { }       /* Element */
.wallet-card--highlighted { } /* Modifier */
```

### 3. CSS Organization Order
```css
.component {
    /* 1. Display & Layout */
    display: flex;
    position: relative;
    
    /* 2. Dimensions */
    width: 100%;
    height: 50px;
    
    /* 3. Spacing */
    margin: 10px;
    padding: 15px;
    
    /* 4. Colors & Typography */
    color: #333;
    font-size: 16px;
    
    /* 5. Borders & Shadows */
    border: 1px solid #ccc;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    
    /* 6. Transitions */
    transition: all 0.3s ease;
}
```

## Testing and Debugging

### Browser Developer Tools
1. **Right-click → Inspect** to open DevTools
2. **Elements tab** - View and edit HTML/CSS live
3. **Console tab** - See errors and warnings
4. **Device toolbar** - Test responsive design

### CSS Debugging Tips
```css
/* Temporary border to see element boundaries */
.debug {
    border: 1px solid red !important;
}

/* Use background colors to understand layout */
.container {
    background-color: rgba(255, 0, 0, 0.1);
}
```

### Common CSS Issues
1. **Specificity conflicts** - Use more specific selectors
2. **Box model confusion** - Add `box-sizing: border-box`
3. **Flexbox alignment** - Check `justify-content` vs `align-items`
4. **Mobile responsiveness** - Test on actual devices

## Performance Optimization

### CSS Performance Tips
1. **Minimize CSS file size**
2. **Use efficient selectors**
3. **Avoid deep nesting**
4. **Use CSS compression**
5. **Optimize images**

### Efficient Selectors
```css
/* Good - specific and fast */
.button { }
.card__title { }

/* Avoid - too generic */
* { }
div div div p { }
```

## Hands-on Exercises

### Exercise 1: Customize the Pleb Wallet
1. Change the color scheme to your preference
2. Add a new section for "Recent Activity"
3. Implement a dark/light mode toggle
4. Add animations to button hovers

### Exercise 2: Build a Bitcoin Price Dashboard
1. Create a grid layout for multiple cryptocurrencies
2. Add price change indicators (green/red arrows)
3. Make it fully responsive
4. Add a search filter

### Exercise 3: Create a Lightning Invoice Generator
1. Build a form with proper styling
2. Add validation styling (error states)
3. Create a QR code display area
4. Style for mobile-first design

## Learning Resources

### Essential References
- **[CSS-Tricks](https://css-tricks.com/)** - Comprehensive CSS guide
- **[MDN CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS)** - Official documentation
- **[Can I Use](https://caniuse.com/)** - Browser compatibility checker
- **[Flexbox Froggy](https://flexboxfroggy.com/)** - Interactive Flexbox game
- **[CSS Grid Garden](https://cssgridgarden.com/)** - CSS Grid learning game

### Advanced Learning
- **[CSS Animations](https://robots.thoughtbot.com/transitions-and-transforms)** - Transitions and transforms
- **[CSS Architecture](https://sass-lang.com/)** - SASS and CSS preprocessing
- **[CSS Frameworks](https://tailwindcss.com/)** - Tailwind CSS
- **[CSS-in-JS](https://styled-components.com/)** - Modern CSS solutions

### Design Inspiration
- **[Dribbble](https://dribbble.com/)** - Design inspiration
- **[Behance](https://behance.net/)** - Creative portfolios
- **[Awwwards](https://awwwards.com/)** - Award-winning web design
- **[CodePen](https://codepen.io/)** - CSS experiments and demos

## Next Steps

### Immediate Actions
1. **Complete the Pleb Wallet project** with all responsive features
2. **Experiment with different layouts** using Flexbox and Grid
3. **Practice media queries** on various screen sizes
4. **Build your own Bitcoin-themed components**

### Prepare for Next Lesson
In the next lesson, we'll dive into JavaScript and learn how to:
- Add interactivity to our wallet
- Make API calls to get real Bitcoin prices
- Handle user input and form validation
- Create dynamic content updates
- Connect to Lightning Network services

### Building Your Portfolio
- Create multiple CSS projects
- Experiment with different design patterns
- Build responsive layouts
- Share your work on CodePen or GitHub

## Key Takeaways

1. **CSS is about presentation** - Separate styling from content
2. **Selectors are powerful** - Use classes for styling, avoid IDs
3. **Flexbox simplifies layouts** - Modern solution for alignment
4. **Mobile-first is essential** - Design for smallest screens first
5. **Practice makes perfect** - Build projects to solidify knowledge
6. **Tools are your friend** - Use browser DevTools for debugging
7. **Performance matters** - Write efficient, maintainable CSS

## Troubleshooting Common Issues

### CSS Not Loading
```html
<!-- Check file path -->
<link rel="stylesheet" href="./styles.css">
<!-- vs -->
<link rel="stylesheet" href="styles.css">
```

### Styles Not Applying
1. Check selector specificity
2. Verify HTML class names match CSS
3. Look for typos in property names
4. Ensure proper syntax (semicolons, brackets)

### Responsive Issues
1. Add viewport meta tag
2. Test on actual devices
3. Use relative units (%, rem, em)
4. Check media query syntax

### Layout Problems
1. Use browser DevTools to inspect
2. Add temporary borders to see boundaries
3. Check box model with `box-sizing`
4. Verify Flexbox container/item properties

Remember: Every expert was once a beginner. The key is consistent practice and building real projects. Your Pleb Wallet is a solid foundation - now make it your own!

Happy styling! 🎨⚡️

---

## Complete Pleb Wallet Code

### HTML (index.html)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Pleb Wallet</title>
    <link rel="stylesheet" href="index.css">
</head>
<body>
    <header>
        <h1>Pleb Wallet</h1>
    </header>
    
    <main>
        <div class="buttons">
            <button onclick="alert('Send!')">Send</button>
            <button onclick="alert('Receive!')">Receive</button>
        </div>
        
        <div class="row">
            <div class="balance-card">
                <h2>Balance</h2>
                <p>897900 sats</p>
            </div>
            <div class="balance-card">
                <h2>Price</h2>
                <p>$19,364</p>
            </div>
        </div>
        
        <div class="row">
            <div class="row-item">
                <h3>Transactions</h3>
                <p class="transaction">Sent to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">-2200 sats</p>
                <p class="transaction">Received from bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+100 sats</p>
                <p class="transaction">Received to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+900000 sats</p>
                <p class="transaction">Sent to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">-2200 sats</p>
                <p class="transaction">Received from bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+100 sats</p>
                <p class="transaction">Received to bc1xxxxxxxxxxxxxxxx</p>
                <p class="transaction-amount">+900000 sats</p>
            </div>
            <div class="row-item">
                <img src="./BTCUSD.png" alt="Bitcoin Price Chart" />
            </div>
        </div>
    </main>
    
    <footer>
        <p>Made by plebs, for plebs.</p>
    </footer>
</body>
</html>
```

### CSS (index.css)
```css
/* Global Styles */
body {
    background-color: #192734;
    font-family: monospace;
    margin: 0;
    padding: 0;
}

/* Header */
header {
    border-bottom: 2px solid #ffbf46;
}

h1 {
    text-align: center;
    color: #8a4fff;
    margin: 20px 0;
}

/* Images */
img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

/* Footer */
footer {
    border-top: 2px solid #ffbf46;
    padding: 1%;
    text-align: center;
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: #192734;
}

footer p {
    color: #8a4fff;
    margin: 0;
}

/* Button Container */
.buttons {
    width: 50%;
    margin: 0 auto;
    margin-top: 3%;
    display: flex;
    justify-content: space-around;
}

/* Button Styles */
.buttons button {
    background-color: #ffbf46;
    border: 2px solid #8a4fff;
    border-radius: 5px;
    padding: 10px 15px;
    font-size: 1.2rem;
    width: 100px;
    cursor: pointer;
    transition: opacity 0.3s ease;
}

.buttons button:hover {
    opacity: 0.6;
}

/* Balance Cards */
.balance-card {
    background-color: #ffbf46;
    border: 2px solid #8a4fff;
    padding: 1%;
    width: 25%;
    margin-top: 3%;
    margin-bottom: 1%;
    border-radius: 5px;
}

.balance-card h2 {
    margin: 0 0 10px 0;
    color: #192734;
}

.balance-card p {
    font-size: 1.2rem;
    font-weight: bold;
    margin: 0;
    color: #192734;
}

/* Layout Rows */
.row {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 0 20px;
}

/* Row Items */
.row-item {
    background-color: ghostwhite;
    border: 2px solid #8a4fff;
    border-radius: 5px;
    height: 300px;
    width: 40%;
    overflow: scroll;
    padding: 10px;
}

.row-item h3 {
    margin: 0 0 15px 0;
    color: #192734;
}

.row-item p {
    margin: 5px 0;
    color: #192734;
}

/* Transaction Styles */
.transaction-amount {
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
    margin-bottom: 10px;
    font-weight: bold;
}

/* Responsive Design */
@media (max-width: 876px) {
    .row-item {
        width: 100%;
        height: 200px;
        margin-bottom: 20px;
    }
    
    .balance-card {
        width: 100%;
        text-align: center;
        margin-right: 2%;
        margin-left: 2%;
    }
    
    .buttons {
        width: 80%;
    }
}

@media (max-width: 615px) {
    .row {
        flex-direction: column;
        padding: 0 10px;
    }
    
    .row-item {
        width: 100%;
        margin-top: 15px;
    }
    
    .balance-card {
        width: 80%;
        margin: 15px auto;
    }
    
    .buttons {
        width: 90%;
        margin-top: 20px;
    }
    
    .buttons button {
        width: 80px;
        font-size: 1rem;
    }
    
    /* Add padding to prevent footer overlap */
    main {
        padding-bottom: 80px;
    }
}
```

Save these files in your project folder and open `index.html` in your browser to see your complete Bitcoin wallet interface! 