[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-2.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-2.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-2.webm)

# Lesson 2: Satoshi Calculator

## Welcome Back, PlebDev! 

Great job completing your first Bitcoin project! 🎉 Now we're diving into something every Bitcoin developer needs to master: converting between Bitcoin and satoshis. This is where JavaScript starts to get really powerful with functions and user interaction.

In this lesson, you'll build a professional Satoshi calculator that handles user input, performs calculations, and displays results beautifully. These are skills you'll use in every Bitcoin application you build.

## What You'll Learn

### JavaScript Concepts
- **Functions**: How to organize and reuse code
- **Event Handlers**: Making buttons and inputs interactive
- **Number Methods**: Formatting and converting numbers properly
- **Type Conversion**: Safely handling user input

### Bitcoin Development Skills
- Converting between BTC and satoshis (essential for all Bitcoin apps)
- Handling user input validation
- Creating interactive Bitcoin tools

## Prerequisites
- Completed Lesson 1 (Bitcoin Price Display)
- Understanding of variables and DOM manipulation
- VS Code and browser ready to go

## Project Overview: BTC to Satoshi Converter

We're building a calculator that:
- Converts BTC amounts to satoshis
- Handles decimal inputs properly
- Formats large numbers with commas
- Provides instant feedback to users
- Looks professional and Bitcoin-themed

## Step-by-Step Build

### Step 1: Create Your Project Structure
1. Create a new folder called `satoshi-calculator`
2. Open it in VS Code
3. Create `index.html`

### Step 2: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Satoshi Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #ff9500, #ff7700);
            min-height: 100vh;
        }
        .calculator {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        .input-group {
            margin: 20px 0;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }
        input {
            width: 100%;
            padding: 12px;
            font-size: 1.2em;
            border: 2px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
        }
        input:focus {
            border-color: #ff9500;
            outline: none;
        }
        button {
            width: 100%;
            padding: 15px;
            font-size: 1.3em;
            background: #ff9500;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin: 15px 0;
        }
        button:hover {
            background: #e08600;
        }
        .result {
            padding: 20px;
            background: #f0f8ff;
            border-radius: 8px;
            margin-top: 20px;
            font-size: 1.2em;
            text-align: center;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .satoshi-count {
            font-size: 1.5em;
            color: #ff9500;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h1>🟠 Satoshi Calculator</h1>
        <p>Convert Bitcoin (BTC) to Satoshis</p>
        
        <div class="input-group">
            <label for="btc-input">Enter BTC Amount:</label>
            <input type="number" id="btc-input" placeholder="0.00000001" step="0.00000001">
        </div>
        
        <button onclick="convertToSats()">Convert to Satoshis</button>
        
        <div id="result" class="result">
            Enter an amount above and click convert!
        </div>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 3: Understanding Functions
Functions are like recipes - they take ingredients (inputs) and produce a dish (output):

```javascript
// Function declaration - this is how we create reusable code
function convertToSats() {
    console.log("Convert function called!");
}

// This function will run when the button is clicked
```

### Step 4: Getting User Input
```javascript
function convertToSats() {
    // Getting the input value from the HTML element
    const btcInput = document.getElementById("btc-input");
    const btcValue = btcInput.value;
    
    console.log("User entered:", btcValue);
}
```

### Step 5: Type Conversion - Handling Numbers Safely
```javascript
function convertToSats() {
    // Get the input value
    const btcInput = document.getElementById("btc-input");
    const btcValue = btcInput.value;
    
    // Convert string to number - this is crucial!
    const btcAmount = parseFloat(btcValue);
    
    // Check if the conversion worked
    if (isNaN(btcAmount) || btcAmount <= 0) {
        document.getElementById("result").innerHTML = 
            "⚠️ Please enter a valid BTC amount";
        return; // Exit the function early
    }
    
    console.log("Valid BTC amount:", btcAmount);
}
```

### Step 6: The Satoshi Conversion
```javascript
function convertToSats() {
    // Get and validate input
    const btcInput = document.getElementById("btc-input");
    const btcAmount = parseFloat(btcInput.value);
    
    if (isNaN(btcAmount) || btcAmount <= 0) {
        document.getElementById("result").innerHTML = 
            "⚠️ Please enter a valid BTC amount";
        return;
    }
    
    // The core conversion: 1 BTC = 100,000,000 satoshis
    const satoshis = btcAmount * 100000000;
    
    // Display the result
    const resultElement = document.getElementById("result");
    resultElement.innerHTML = `
        <div>
            <div class="satoshi-count">${satoshis.toLocaleString()} sats</div>
            <div>${btcAmount} BTC = ${satoshis.toLocaleString()} satoshis</div>
        </div>
    `;
}
```

### Step 7: Enhanced Version with Extra Features
Here's the complete, enhanced calculator:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Satoshi Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #ff9500, #ff7700);
            min-height: 100vh;
        }
        .calculator {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        .input-group {
            margin: 20px 0;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333;
        }
        input {
            width: 100%;
            padding: 12px;
            font-size: 1.2em;
            border: 2px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
        }
        input:focus {
            border-color: #ff9500;
            outline: none;
        }
        button {
            width: 100%;
            padding: 15px;
            font-size: 1.3em;
            background: #ff9500;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin: 15px 0;
        }
        button:hover {
            background: #e08600;
        }
        .result {
            padding: 20px;
            background: #f0f8ff;
            border-radius: 8px;
            margin-top: 20px;
            font-size: 1.2em;
            text-align: center;
            min-height: 60px;
        }
        .satoshi-count {
            font-size: 1.8em;
            color: #ff9500;
            font-weight: bold;
            margin: 10px 0;
        }
        .conversion-details {
            margin-top: 15px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 5px;
            font-size: 0.9em;
        }
        .quick-amounts {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 20px 0;
        }
        .quick-btn {
            padding: 10px;
            background: #f0f0f0;
            border: 1px solid #ddd;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9em;
        }
        .quick-btn:hover {
            background: #e0e0e0;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h1>🟠 Satoshi Calculator</h1>
        <p>Convert Bitcoin (BTC) to Satoshis</p>
        
        <div class="input-group">
            <label for="btc-input">Enter BTC Amount:</label>
            <input type="number" id="btc-input" placeholder="0.00000001" step="0.00000001">
        </div>
        
        <div class="quick-amounts">
            <button class="quick-btn" onclick="setAmount(0.00000001)">1 Satoshi</button>
            <button class="quick-btn" onclick="setAmount(0.00001)">1000 Sats</button>
            <button class="quick-btn" onclick="setAmount(0.001)">100K Sats</button>
            <button class="quick-btn" onclick="setAmount(1)">1 BTC</button>
        </div>
        
        <button onclick="convertToSats()">Convert to Satoshis</button>
        
        <div id="result" class="result">
            Enter an amount above and click convert!
        </div>
    </div>
    
    <script>
        // Main conversion function
        function convertToSats() {
            // Get and validate input
            const btcInput = document.getElementById("btc-input");
            const btcAmount = parseFloat(btcInput.value);
            
            // Input validation
            if (isNaN(btcAmount)) {
                showError("Please enter a valid number");
                return;
            }
            
            if (btcAmount <= 0) {
                showError("Please enter an amount greater than 0");
                return;
            }
            
            if (btcAmount > 21000000) {
                showError("Amount exceeds Bitcoin's maximum supply!");
                return;
            }
            
            // The conversion: 1 BTC = 100,000,000 satoshis
            const satoshis = Math.round(btcAmount * 100000000);
            
            // Display the result
            displayResult(btcAmount, satoshis);
        }
        
        // Function to display successful conversion
        function displayResult(btcAmount, satoshis) {
            const resultElement = document.getElementById("result");
            
            resultElement.innerHTML = `
                <div class="satoshi-count">${satoshis.toLocaleString()} satoshis</div>
                <div>${btcAmount} BTC</div>
                <div class="conversion-details">
                    <strong>Conversion Details:</strong><br>
                    ${btcAmount} × 100,000,000 = ${satoshis.toLocaleString()} sats<br>
                    <small>1 BTC = 100 million satoshis</small>
                </div>
            `;
        }
        
        // Function to show error messages
        function showError(message) {
            document.getElementById("result").innerHTML = `⚠️ ${message}`;
        }
        
        // Function for quick amount buttons
        function setAmount(amount) {
            document.getElementById("btc-input").value = amount;
            convertToSats(); // Automatically convert
        }
        
        // Allow Enter key to trigger conversion
        document.getElementById("btc-input").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                convertToSats();
            }
        });
    </script>
</body>
</html>
```

## Key Takeaways

### Functions
- **Function Declaration**: `function name() { }` creates reusable code blocks
- **Parameters**: Functions can accept inputs to work with
- **Return Values**: Functions can send results back
- **Organization**: Functions make code cleaner and easier to maintain

### Event Handlers
- **onclick**: Connects buttons to functions
- **addEventListener**: More advanced event handling
- **User Interaction**: Making web pages respond to user actions

### Number Methods & Conversion
- **parseFloat()**: Converts strings to decimal numbers
- **toLocaleString()**: Formats numbers with commas (95000 → 95,000)
- **Math.round()**: Ensures we get whole satoshis
- **isNaN()**: Checks if something is "Not a Number"

### Input Validation
- Always check user input before using it
- Provide helpful error messages
- Handle edge cases (negative numbers, too large amounts)
- Give users feedback about what went wrong

## Challenge Yourself

1. **Add Reverse Conversion**: Create a satoshi-to-BTC converter
2. **Multiple Currencies**: Show equivalent values in USD, EUR
3. **Historical Context**: Show what the satoshis were worth at different Bitcoin prices
4. **Keyboard Shortcuts**: Add more keyboard interactions

## Common Issues and Solutions

### Input Not Converting
- Check that you're using `parseFloat()` for decimal numbers
- Make sure to validate with `isNaN()` before calculating

### Numbers Look Wrong  
- Use `Math.round()` for satoshi calculations to avoid floating point errors
- Remember: 1 BTC = exactly 100,000,000 satoshis

### Button Not Working
- Verify the `onclick` attribute matches your function name exactly
- Check browser console (F12) for JavaScript errors

## Real-World Applications

This calculator demonstrates patterns you'll use in:
- **Lightning Network**: Converting between base units and millisatoshis
- **Payment Processors**: Handling different denomination displays
- **Wallet Applications**: Showing amounts in user-preferred units
- **DeFi Applications**: Converting between different token amounts

## Next Steps

Fantastic work! You've mastered functions and user interaction. In Lesson 3, we'll work with arrays and objects to build a multi-wallet balance tracker - essential skills for any Bitcoin application.

### What's Coming Next
- **Lesson 3**: Arrays, objects, and loops
- **Real-world skills**: Managing multiple data items
- **New concepts**: Data structures and iteration

## Resources for Going Deeper

- [MDN Functions Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [Number Methods Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [Event Handling Basics](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

## Share Your Success!

🎉 **You built a professional Bitcoin calculator!** 

This is exactly the kind of tool Bitcoin developers create every day. You're building real skills that matter in the Bitcoin ecosystem.

Don't forget to:
1. Test your calculator with different amounts
2. Push your code to GitHub  
3. Share it with the PlebDevs community

Ready to level up with data structures? Let's build a wallet tracker! 🚀

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 