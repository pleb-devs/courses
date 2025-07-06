[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.webm)

# Lesson 1: Bitcoin Price Display

## Welcome to Your First JavaScript Project!

Hey PlebDev! 👋

Welcome to your first hands-on JavaScript lesson. I'm excited to guide you through building your very first Bitcoin application - a live price tracker that will display the current Bitcoin price in a beautiful, dynamic format.

This lesson introduces you to the fundamental building blocks of JavaScript through a project that every Bitcoin developer makes at some point. By the end, you'll have a working Bitcoin price display and understand core JavaScript concepts you'll use in every project moving forward.

## What You'll Learn

### JavaScript Concepts
- **Variables**: How to store and manage data
- **Data Types**: Numbers, strings, and when to use each
- **DOM Manipulation**: How to make your web page interactive
- **Template Literals**: Modern string formatting for dynamic content

### Bitcoin Development Skills
- Working with Bitcoin price data
- Creating user-friendly Bitcoin interfaces
- Building your first portfolio project

## Prerequisites
- Visual Studio Code (or your favorite code editor) installed
- Basic HTML knowledge (don't worry, we'll walk through everything)
- A desire to build something cool! 🚀

## Project Overview: Bitcoin Price Tracker

We're building a simple but elegant Bitcoin price display that shows:
- Current Bitcoin price in USD
- Clean, professional formatting
- Last update timestamp
- Foundation for more advanced features

This might seem simple, but you're learning concepts that power every Bitcoin web application!

## Key Concepts Explained

Before we dive into building, let's thoroughly understand the essential JavaScript concepts you'll use:

### Variables: Your Data Storage System
Variables are like labeled containers that hold information. Think of them as boxes where you store different types of data:

```javascript
// let - for values that can change (mutable)
let bitcoinPrice = 95000;
bitcoinPrice = 96000; // ✅ This works - price updates
bitcoinPrice = 94500; // ✅ Can change again

// const - for values that don't change (immutable)
const currency = "USD";
const maxSupply = 21000000;
currency = "EUR"; // ❌ This causes an error - can't reassign

// var - the old way (avoid using this)
var oldWay = "don't use this"; // Has confusing scope rules
```

**When to use which:**
- `let` for dynamic data (prices, counters, user input, calculations)
- `const` for fixed values (API URLs, configuration, mathematical constants)
- Never use `var` in modern JavaScript

**Examples:**
```javascript
let currentBlock = 825000;        // Changes every ~10 minutes
let userBalance = 2.5;            // Changes with transactions
const satoshiPerBTC = 100000000;  // Never changes
const genesisDate = "2009-01-03"; // Historical constant
```

### Data Types: Different Kinds of Information
JavaScript automatically detects and handles different types of data:

```javascript
// Numbers (integers and decimals)
let wholeNumber = 95000;      // Integer
let decimal = 2.5;            // Float/decimal
let scientific = 1e8;         // Scientific notation (100,000,000)

// Strings (text)
let currency = "USD";         // Double quotes
let address = 'bc1q...';      // Single quotes
let invoice = `lnbc...`;      // Template literals (backticks)

// Booleans (true/false)
let isExpensive = true;       // Boolean true
let isAffordable = false;     // Boolean false
let hasEnoughSats = !isExpensive; // Boolean with negation

// Special values
let unknownPrice = null;      // Intentionally empty
let unsetValue;               // undefined (not set yet)

// You can check types
console.log(typeof 95000);        // "number"
console.log(typeof "Bitcoin");    // "string"
console.log(typeof true);         // "boolean"
```

**examples:**
```javascript
let btcAmount = 2.5;              // Number: Bitcoin amount
let satAmount = 250000000;        // Number: Satoshi equivalent
let walletAddress = "bc1q...";    // String: Bitcoin address
let isConfirmed = false;          // Boolean: Transaction status
let blockHash = "000000000019d6889c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f"; // String
```

### DOM Manipulation: Connecting JavaScript to HTML
The DOM (Document Object Model) is how JavaScript talks to your webpage. Think of it as the bridge between your JavaScript code and what users see:

```javascript
// Finding elements (like finding a specific box on a shelf)
const priceDisplay = document.getElementById("price-display");
const updateTime = document.getElementById("update-time");

// Getting element content
let currentText = priceDisplay.textContent; // Gets plain text
let currentHTML = priceDisplay.innerHTML;   // Gets HTML content

// Changing what users see
priceDisplay.textContent = "Bitcoin: $95,000"; // Plain text
priceDisplay.innerHTML = "<strong>Bitcoin: $95,000</strong>"; // HTML formatting

// Changing styles
priceDisplay.style.color = "#ff9500";        // Orange text
priceDisplay.style.fontSize = "2em";         // Bigger text
priceDisplay.style.backgroundColor = "#f5f5f5"; // Background color

// Adding/removing CSS classes
priceDisplay.classList.add("price-highlight");    // Add class
priceDisplay.classList.remove("price-dim");       // Remove class
priceDisplay.classList.toggle("price-active");    // Toggle on/off
```

**Real-world Bitcoin app examples:**
```javascript
// Update Bitcoin price display
const priceElement = document.getElementById("btc-price");
priceElement.textContent = `$${bitcoinPrice.toLocaleString()}`;
priceElement.style.color = bitcoinPrice > 100000 ? "green" : "orange";

// Update transaction status
const statusElement = document.getElementById("tx-status");
statusElement.textContent = isConfirmed ? "✅ Confirmed" : "⏳ Pending";
statusElement.className = isConfirmed ? "status-confirmed" : "status-pending";
```

### Template Literals: Modern String Building
Template literals revolutionize how we build strings with dynamic content:

```javascript
// Old way - string concatenation (messy and error-prone)
let name = "Satoshi";
let amount = 2.5;
let oldMessage = "Hello " + name + ", you have " + amount + " BTC worth $" + (amount * 95000);

// New way - template literals (clean and readable)
let newMessage = `Hello ${name}, you have ${amount} BTC worth $${(amount * 95000).toLocaleString()}`;

// Multi-line strings (impossible with quotes)
let bitcoinInfo = `
Bitcoin Information:
• Current Price: $${bitcoinPrice}
• Your Balance: ${amount} BTC
• Portfolio Value: $${(amount * bitcoinPrice).toLocaleString()}
• Last Update: ${new Date().toLocaleTimeString()}
`;

// Advanced formatting
let transactionSummary = `
Transaction Details:
From: ${senderAddress.substring(0, 10)}...
To: ${receiverAddress.substring(0, 10)}...
Amount: ${btcAmount.toFixed(8)} BTC (${(btcAmount * 100000000).toLocaleString()} sats)
Fee: ${feeAmount.toFixed(8)} BTC
Status: ${isConfirmed ? 'Confirmed ✅' : 'Pending ⏳'}
`;

// Calculations inside template literals
let portfolioReport = `
Portfolio Summary:
Holdings: ${btcAmount} BTC
Current Value: $${(btcAmount * currentPrice).toLocaleString()}
24h Change: ${change > 0 ? '+' : ''}${change.toFixed(2)}%
Profit/Loss: $${((btcAmount * currentPrice) - (btcAmount * buyPrice)).toLocaleString()}
`;
```

**Why template literals are essential for Bitcoin apps:**
- **Readability**: Easy to see the final string structure
- **Calculations**: Perform math directly in the string
- **Formatting**: Apply number formatting, dates, conditions
- **Multi-line**: Create complex displays without concatenation
- **Maintainability**: Much easier to modify and debug

**Common patterns you'll use:**
```javascript
// Price formatting
`$${price.toLocaleString()}`                    // $95,000
`${sats.toLocaleString()} sats`                 // 250,000,000 sats
`${btc.toFixed(8)} BTC`                         // 2.50000000 BTC

// Conditional display
`Status: ${isOnline ? 'Connected' : 'Offline'}` // Status: Connected
`${balance > 0 ? 'Balance' : 'No funds'}: ${balance}` // Balance: 2.5

// Address formatting (show first/last characters)
`${address.slice(0, 6)}...${address.slice(-6)}` // bc1q4d...h7x2n8
```

## Step-by-Step Build

### Step 1: Create Your Project
1. Create a new folder called `bitcoin-price-tracker`
2. Open it in Code Editor
3. Create a new file called `index.html`

### Step 2: Basic HTML Structure
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Price Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .price-container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .price {
            font-size: 3em;
            color: #ff9500;
            font-weight: bold;
            margin: 20px 0;
        }
        .update-time {
            color: #666;
            font-style: italic;
        }
    </style>
</head>
<body>
    <div class="price-container">
        <h1>🟠 Bitcoin Price Tracker</h1>
        <div id="price-display" class="price">Loading...</div>
        <div id="update-time" class="update-time">Initializing...</div>
    </div>
    
    <script>
        // Your JavaScript code will go here!
    </script>
</body>
</html>
```

### Step 3: Understanding Variables
Let's start with the foundation - variables:

```html
<script>
    // Variables store information we need
    // let - for values that can change
    let bitcoinPrice = 95000;
    let lastUpdate = "2025-01-15";
    
    // const - for values that stay the same
    const currency = "USD";
    const appName = "Bitcoin Price Tracker";
    
    console.log("Bitcoin price:", bitcoinPrice);
    console.log("Currency:", currency);
</script>
```

**Why This Matters**: In real Bitcoin apps, prices change constantly. We use `let` for dynamic data and `const` for configuration that doesn't change.

### Step 4: Working with the DOM
The DOM (Document Object Model) lets us interact with HTML elements:

```html
<script>
    // Variables and data types
    let bitcoinPrice = 95000;
    const currency = "USD";
    let lastUpdate = "2025-01-15";
    
    // DOM manipulation - finding elements
    const priceDisplay = document.getElementById("price-display");
    const updateTime = document.getElementById("update-time");
    
    // Update the content
    priceDisplay.textContent = `$${bitcoinPrice.toLocaleString()} ${currency}`;
    updateTime.textContent = `Last updated: ${lastUpdate}`;
</script>
```

### Step 5: Template Literals - Modern String Magic
Template literals make creating dynamic text much easier:

```html
<script>
    // Variables and data types
    let bitcoinPrice = 95000;
    const currency = "USD";
    let lastUpdate = "2025-01-15";
    
    // DOM manipulation
    const priceDisplay = document.getElementById("price-display");
    const updateTime = document.getElementById("update-time");
    
    // Template literals with backticks - this is the modern way!
    priceDisplay.innerHTML = `$${bitcoinPrice.toLocaleString()} ${currency}`;
    updateTime.innerHTML = `Last updated: ${lastUpdate}`;
    
    // We can even do calculations inside template literals
    const satPrice = (bitcoinPrice / 100000000).toFixed(8);
    console.log(`1 satoshi = $${satPrice}`);
</script>
```

### Step 6: Complete Working Version
Here's your complete, working Bitcoin price tracker:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Price Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .price-container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .price {
            font-size: 3em;
            color: #ff9500;
            font-weight: bold;
            margin: 20px 0;
        }
        .update-time {
            color: #666;
            font-style: italic;
        }
        .stats {
            margin-top: 20px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <div class="price-container">
        <h1>🟠 Bitcoin Price Tracker</h1>
        <div id="price-display" class="price">Loading...</div>
        <div id="update-time" class="update-time">Initializing...</div>
        <div id="stats" class="stats"></div>
    </div>
    
    <script>
        // Variables and data types
        let bitcoinPrice = 95000;
        const currency = "USD";
        let lastUpdate = "2025-01-15 10:30 AM";
        
        // DOM manipulation
        const priceDisplay = document.getElementById("price-display");
        const updateTime = document.getElementById("update-time");
        const stats = document.getElementById("stats");
        
        // Template literals for dynamic content
        priceDisplay.innerHTML = `$${bitcoinPrice.toLocaleString()} ${currency}`;
        updateTime.innerHTML = `Last updated: ${lastUpdate}`;
        
        // Calculate some fun Bitcoin stats
        const satPrice = (bitcoinPrice / 100000000).toFixed(8);
        const marketCap = bitcoinPrice * 21000000; // Assuming max supply
        
        stats.innerHTML = `
            <p><strong>1 Satoshi:</strong> $${satPrice}</p>
            <p><strong>Total Supply:</strong> 21,000,000 BTC</p>
            <p><strong>Theoretical Max Cap:</strong> $${marketCap.toLocaleString()}</p>
        `;
    </script>
</body>
</html>
```

## Testing Your Code

1. Save your `index.html` file
2. Right-click the file in VS Code and select "Open with Live Server" (if you have the extension)
3. Or simply double-click the file to open it in your browser
4. You should see your Bitcoin price tracker in action!

## Key Takeaways

### Variables
- `let` for values that change (like prices that update)
- `const` for values that stay the same (like currency symbols)
- Always use meaningful names that describe your data

### Data Types
- **Numbers**: `95000` for prices and calculations
- **Strings**: `"USD"` for text and labels
- JavaScript automatically handles type conversion when needed

### DOM Manipulation
- `document.getElementById()` finds HTML elements by their ID
- `.innerHTML` lets you add HTML content
- `.textContent` adds plain text content

### Template Literals
- Use backticks `` ` `` instead of quotes
- Insert variables with `${variableName}`
- Much cleaner than old-style string concatenation
- Perfect for creating dynamic content

## Challenge Yourself

Ready to take it further? Try these modifications:

1. **Add More Currencies**: Show the price in EUR, GBP, or JPY
2. **Add Styling**: Make it look even better with more CSS
3. **Add More Stats**: Show price per satoshi in different formats
4. **Mock Updates**: Change the price and timestamp to simulate updates

## Common Issues and Solutions

### Price Not Showing?
- Check that your element IDs match: `price-display`, `update-time`
- Make sure your script tag is inside the `<body>` after your HTML elements

### Styling Looks Off?
- Ensure your CSS is in the `<head>` section
- Check for typos in class names and selectors

### Console Errors?
- Open browser developer tools (F12) to see error messages
- Check for typos in variable names and function calls

## Next Steps

Congratulations! You've built your first Bitcoin application and learned fundamental JavaScript concepts. In the next lesson, we'll add interactivity with functions and user input by building a Satoshi calculator.

### What's Coming Next
- **Lesson 2**: Functions and user interaction
- **Real-world skills**: Converting between BTC and satoshis
- **New concepts**: Event handling and number formatting

## Resources for Going Deeper

- [MDN JavaScript Variables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#Variables)
- [Template Literals Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [DOM Manipulation Basics](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

## Share Your Success!

🎉 **You built your first Bitcoin application!** 

Don't forget to:
1. Push your code to GitHub
2. Share your project with the PlebDevs community
3. Take a screenshot and celebrate this milestone

Remember: Every expert was once a beginner. You're now officially on your way to becoming a Bitcoin developer!

Ready for the next challenge? Let's build a Satoshi calculator! 🚀

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 