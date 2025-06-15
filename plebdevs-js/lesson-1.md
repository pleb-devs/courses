[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-1.webm)

# Lesson 1: Bitcoin Price Display

## Welcome to Your First JavaScript Project!

Hey PlebDev! 👋

Welcome to your first hands-on JavaScript lesson. I'm excited to guide you through building your very first Bitcoin application - a live price tracker that will display the current Bitcoin price in a beautiful, dynamic format.

This lesson introduces you to the fundamental building blocks of JavaScript through a project that actually matters to us as Bitcoin developers. By the end, you'll have a working Bitcoin price display and understand core JavaScript concepts you'll use in every project moving forward.

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
- Visual Studio Code installed
- Basic HTML knowledge (don't worry, we'll walk through everything)
- A desire to build something cool! 🚀

## Project Overview: Bitcoin Price Tracker

We're building a simple but elegant Bitcoin price display that shows:
- Current Bitcoin price in USD
- Clean, professional formatting
- Last update timestamp
- Foundation for more advanced features

This might seem simple, but you're learning concepts that power every Bitcoin web application!

## Step-by-Step Build

### Step 1: Create Your Project
1. Create a new folder called `bitcoin-price-tracker`
2. Open it in VS Code
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