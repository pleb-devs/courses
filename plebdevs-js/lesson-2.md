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
- Code editor and browser ready to go

## Project Overview: BTC to Satoshi Converter

We're building a calculator that:
- Converts BTC amounts to satoshis
- Handles decimal inputs properly
- Formats large numbers with commas
- Provides instant feedback to users
- Looks professional and Bitcoin-themed

## Key Concepts Explained

Let's thoroughly explore the JavaScript concepts that make interactive applications possible:

### Functions: Your Code Organization System
Functions are the building blocks of any application. Think of them as reusable machines that take inputs, process them, and give you outputs:

```javascript
// Basic function declaration
function convertToSats() {
    console.log("Converting BTC to sats!");
    // This function does work but doesn't return anything
}

// How to call it:
convertToSats(); // Output: "Converting BTC to sats!"

// Function with parameters (inputs)
function multiply(a, b) {
    return a * b; // Returns the result
}

// How to call it:
const result = multiply(5, 3);
console.log(result); // Output: 15

const btcPrice = multiply(50000, 2.5);
console.log(btcPrice); // Output: 125000

// Function with multiple parameters and complex logic
function calculateSatoshis(btcAmount, includeFormatting = true) {
    // Parameter validation
    if (typeof btcAmount !== 'number' || btcAmount < 0) {
        return "Invalid BTC amount";
    }
    
    const satoshis = btcAmount * 100000000;
    
    // Conditional return based on parameter
    if (includeFormatting) {
        return satoshis.toLocaleString() + " sats";
    }
    return satoshis;
}

// How to call it:
const formatted = calculateSatoshis(1.5);
console.log(formatted); // Output: "150,000,000 sats"

const unformatted = calculateSatoshis(1.5, false);
console.log(unformatted); // Output: 150000000

const invalidInput = calculateSatoshis("not a number");
console.log(invalidInput); // Output: "Invalid BTC amount"

// Arrow functions (modern, concise syntax)
const fastConvert = (btc) => btc * 100000000;
const formatPrice = (price) => `$${price.toLocaleString()}`;

// How to call them:
const satsAmount = fastConvert(0.5);
console.log(satsAmount); // Output: 50000000

const formattedPrice = formatPrice(95432.50);
console.log(formattedPrice); // Output: "$95,432.50"

// Function expressions (stored in variables)
const calculateValue = function(amount, price) {
    return amount * price;
};

// How to call it:
const portfolioValue = calculateValue(2.5, 50000);
console.log(portfolioValue); // Output: 125000

// Functions calling other functions
function getPortfolioSummary(btcAmount, currentPrice) {
    const sats = calculateSatoshis(btcAmount, false);
    const value = calculateValue(btcAmount, currentPrice);
    const formattedValue = formatPrice(value);
    
    return `${btcAmount} BTC = ${sats.toLocaleString()} sats = ${formattedValue}`;
}

// How to call it:
const summary = getPortfolioSummary(1.5, 50000);
console.log(summary); // Output: "1.5 BTC = 150,000,000 sats = $75,000"

const smallAmount = getPortfolioSummary(0.001, 50000);
console.log(smallAmount); // Output: "0.001 BTC = 100,000 sats = $50"
```

**Real Bitcoin function examples:**
```javascript
function validateAddress(address) {
    // Simple Bitcoin address validation with type detection
    if (!address || typeof address !== 'string') {
        return false;
    }
    
    // Check length first
    if (address.length < 26 || address.length > 62) {
        return false;
    }
    
    // Determine address type based on prefix
    if (address.startsWith('1')) {
        return { isValid: true, type: 'Legacy (P2PKH)' };
    }
    if (address.startsWith('3')) {
        return { isValid: true, type: 'Script Hash (P2SH)' };
    }
    if (address.startsWith('bc1q')) {
        return { isValid: true, type: 'Native SegWit (P2WPKH/P2WSH)' };
    }
    if (address.startsWith('bc1p')) {
        return { isValid: true, type: 'Taproot (P2TR)' };
    }
    
    return false;
}

function calculateFee(priority, txSize = 250) {
    const feeRates = {
        high: 50,   // sats/vByte
        medium: 25,
        low: 10
    };
    
    const rate = feeRates[priority] || feeRates.medium;
    return rate * txSize;
}

function formatBitcoinAmount(amount, unit = 'BTC') {
    switch(unit) {
        case 'BTC':
            return `${amount.toFixed(8)} BTC`;
        case 'sats':
            return `${Math.round(amount * 100000000).toLocaleString()} sats`;
        case 'mBTC':
            return `${(amount * 1000).toFixed(5)} mBTC`;
        default:
            return amount;
    }
}
```

### Event Handlers: Connecting User Actions to Code
Event handlers make your applications interactive by responding to user actions:

Events in JavaScript from a high level are interactions that happen in the browser. For example, when a user clicks a button, a click event is triggered. When a user types in a form, an input event is triggered. When a user scrolls a page, a scroll event is triggered.

```javascript
// HTML button with inline event handler - BAD
<button onclick="calculateSats()">Convert to Sats</button>

// Better approach: Event listeners in JavaScript
const button = document.getElementById('convert-button');
button.addEventListener('click', calculateSats);

// Function that handles the click event
function calculateSats() {
    const btcInput = document.getElementById('btc-input');
    const resultDiv = document.getElementById('result');
    
    // Get the value and convert
    const btcAmount = parseFloat(btcInput.value);
    const satoshis = btcAmount * 100000000;
    
    // Update the display
    resultDiv.textContent = `${btcAmount} BTC = ${satoshis.toLocaleString()} satoshis`;
}

// Multiple event types
const input = document.getElementById('amount');
input.addEventListener('input', validateInput);     // As user types
input.addEventListener('focus', highlightField);    // When field is selected
input.addEventListener('blur', formatInput);        // When field loses focus

// Event object contains useful information
function handleKeyPress(event) {
    console.log('Key pressed:', event.key);
    console.log('Input value:', event.target.value);
    
    // Enter key to submit
    if (event.key === 'Enter') {
        calculateSats();
    }
    
    // Only allow numbers and decimal point
    if (!/[0-9.]/.test(event.key) && event.key !== 'Backspace') {
        event.preventDefault(); // Stop the keypress
    }
}
```

**Real Bitcoin event handler patterns:**
```javascript
// Form submission handler
function handleTransactionForm(event) {
    event.preventDefault(); // Stop form from reloading page
    
    // use the FormData object to get the form data in a structured way
    const formData = new FormData(event.target);
    // get the recipient and amount from the form data
    const recipient = formData.get('recipient');
    // get the amount from the form data and convert it to a number
    const amount = parseFloat(formData.get('amount'));
    
    // validate the recipient and amount
    if (validateAddress(recipient) && amount > 0) {
        createTransaction(recipient, amount);
    } else {
        showError('Invalid recipient address or amount');
    }
}
```

**HTML form with event handler to test the form handler**
```html
<!DOCTYPE html>
<html>
<head><title>Simple Form Handler</title></head>
<body>
    <form id="transactionForm">
        <input name="recipient" placeholder="Enter recipient address" required>
        <input name="amount" type="number" placeholder="Enter amount" step="0.01" required>
        <button type="submit">Send Transaction</button>
    </form>
    <div id="result"></div>
</body>
</html>
```

**Real-time input validation**
```javascript
function validateBitcoinInput(event) {
    const value = event.target.value;
    const errorElement = document.getElementById('input-error');
    
    // check if the value is a valid number
    if (value && isNaN(parseFloat(value))) {
        // if the value is not a valid number, show the error
        errorElement.textContent = 'Please enter a valid number';
        // add the error class to the input
        event.target.classList.add('error');
    } else {
        // if the value is a valid number, remove the error
        errorElement.textContent = '';
        // remove the error class from the input
        event.target.classList.remove('error');
    }
}
```

**HTML form with real-time input validation**
```html
<input type="text" id="amount">
<div id="input-error"></div>

<script>
// Set up input validation when page loads
document.addEventListener("DOMContentLoaded", function() {
    document.getElementById("amount").addEventListener("input", validateBitcoinInput);
});
</script>
```

### Type Conversion: Handling User Input Safely
User input always comes as strings, but Bitcoin calculations need numbers. Safe conversion is crucial:

```javascript
// The problem: All form input is text
let userInput = "2.5";        // This is a string, not a number
let calculation = userInput * 100000000; // "2.5000000000000000" (wrong!)

// Safe conversion methods
let btcAmount = parseFloat(userInput);    // 2.5 (correct number)
let wholeNumber = parseInt("42.7");       // 42 (drops decimal)
let withRadix = parseInt("10", 10);       // 10 (base 10)

// Always validate conversions
function safeConvertToNumber(input) {
    const number = parseFloat(input);
    
    if (isNaN(number)) {
        throw new Error(`"${input}" is not a valid number`);
    }
    
    if (number < 0) {
        throw new Error('Bitcoin amounts cannot be negative');
    }
    
    if (number > 21000000) {
        throw new Error('Amount exceeds total Bitcoin supply');
    }
    
    return number;
}

// Robust input handling
function handleUserInput() {
    const input = document.getElementById('btc-input').value;
    const resultDiv = document.getElementById('result');
    
    try {
        const btcAmount = safeConvertToNumber(input);
        const satoshis = btcAmount * 100000000;
        
        resultDiv.innerHTML = `
            <div class="success">
                ${btcAmount} BTC = ${satoshis.toLocaleString()} satoshis
            </div>
        `;
        resultDiv.className = 'result success';
    } catch (error) {
        resultDiv.innerHTML = `
            <div class="error">
                ❌ ${error.message}
            </div>
        `;
        resultDiv.className = 'result error';
    }
}

function getInputType(value) {
    if (value === '') return 'empty';
    // check if the value is NaN (Not a Number) by parsing it to a number and checking if it is NaN
    if (isNaN(parseFloat(value))) return 'not-a-number';
    // check if the value is less than 0 (negative)
    if (parseFloat(value) < 0) return 'negative';
    // check if the value is equal to 0
    if (parseFloat(value) === 0) return 'zero';
    // None of the above if conditions are true, so the value is a valid number
    return 'valid-number';
}
```

### Number Methods: Professional Number Handling
JavaScript provides powerful methods for formatting and calculating with numbers:

```javascript
let price = 95432.123456789;
let btcAmount = 2.50000000;
let satAmount = 250000000;

// Decimal control
price.toFixed(2);           // "95432.12" (exactly 2 decimals)
price.toFixed(0);           // "95432" (no decimals)
btcAmount.toFixed(8);       // "2.50000000" (Bitcoin precision)

// Localized formatting (adds commas)
price.toLocaleString();     // "95,432.123" (depends on locale)
satAmount.toLocaleString(); // "250,000,000"

// Currency formatting
price.toLocaleString('en-US', {
    style: 'currency',
    currency: 'USD'
});  // "$95,432.12"

// Custom Bitcoin formatting
function formatBTC(amount, showUnit = true) {
    const formatted = amount.toFixed(8);
    return showUnit ? `${formatted} BTC` : formatted;
}

function formatSats(amount, showUnit = true) {
    const formatted = Math.round(amount).toLocaleString();
    return showUnit ? `${formatted} sats` : formatted;
}

// Mathematical operations
let fee = 0.00025;
let total = btcAmount + fee;          // Addition
let remaining = btcAmount - fee;      // Subtraction
let value = btcAmount * price;        // Multiplication
let avgPrice = value / btcAmount;     // Division

// Rounding for Bitcoin (important!)
function roundToBitcoin(amount) {
    return Math.round(amount * 100000000) / 100000000; // Round to satoshi precision
}

// Percentage calculations
function calculateChange(oldPrice, newPrice) {
    const change = ((newPrice - oldPrice) / oldPrice) * 100;
    return change.toFixed(2);
}

// Advanced number validation
function isValidBitcoinAmount(amount) {
    const num = parseFloat(amount);
    
    // Check if it's a valid number
    if (isNaN(num)) return false;
    
    // Check positive
    if (num <= 0) return false;
    
    // Check Bitcoin supply limit
    if (num > 21000000) return false;
    
    // Check precision (Bitcoin has 8 decimal places max)
    const decimalPlaces = (amount.split('.')[1] || '').length;
    if (decimalPlaces > 8) return false;
    
    return true;
}
```

**Why these concepts matter for Bitcoin development:**
- **Functions** organize complex Bitcoin calculations into reusable pieces
- **Event handlers** make Bitcoin apps interactive and user-friendly
- **Type conversion** ensures accurate Bitcoin amount calculations
- **Number formatting** presents Bitcoin data professionally and clearly

## Step-by-Step Build

### Step 1: Create Your Project Structure
1. Create a new folder called `satoshi-calculator`
2. Open it in Code Editor
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
        
        <button id="convert-button">Convert to Satoshis</button>
        
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
We will create functions to capture and reuse blocks of code and important logic that is used in multiple places.

```javascript
// Function declaration - this is how we create reusable code
function convertToSats() {
    console.log("Convert function called!");
}

// This function will be set up to run when the button is clicked
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

### Step 7: Setting Up Event Listeners (Best Practice)
Now let's connect our function to the button properly using event listeners instead of inline handlers:

```javascript
// Wait for the page to load, then set up event listeners
document.addEventListener("DOMContentLoaded", function() {
    // Connect the button to our function
    document.getElementById("convert-button").addEventListener("click", convertToSats);
    
    // Allow Enter key to trigger conversion
    document.getElementById("btc-input").addEventListener("keypress", function(event) {
        if (event.key === "Enter") {
            convertToSats();
        }
    });
});

// Our complete conversion function
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

**Why this approach is better:**
- **Separation of concerns**: HTML handles structure, JavaScript handles behavior
- **Maintainability**: Easier to manage all event listeners in one place
- **Flexibility**: Can easily add/remove event listeners dynamically
- **Best practices**: Industry standard approach for modern web development

### Step 8: Enhanced Version with Extra Features
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
            <button class="quick-btn" data-amount="0.00000001">1 Satoshi</button>
            <button class="quick-btn" data-amount="0.00001">1000 Sats</button>
            <button class="quick-btn" data-amount="0.001">100K Sats</button>
            <button class="quick-btn" data-amount="1">1 BTC</button>
        </div>
        
        <button id="convert-button">Convert to Satoshis</button>
        
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
        
        // Set up event listeners when page loads
        document.addEventListener("DOMContentLoaded", function() {
            // Main convert button
            document.getElementById("convert-button").addEventListener("click", convertToSats);
            
            // Quick amount buttons
            document.querySelectorAll(".quick-btn").forEach(button => {
                button.addEventListener("click", function() {
                    const amount = parseFloat(this.dataset.amount);
                    setAmount(amount);
                });
            });
            
            // Allow Enter key to trigger conversion
            document.getElementById("btc-input").addEventListener("keypress", function(event) {
                if (event.key === "Enter") {
                    convertToSats();
                }
            });
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
- **addEventListener**: Modern way to connect user actions to functions
- **Event listeners**: Proper separation of HTML structure and JavaScript behavior
- **User Interaction**: Making web pages respond to user actions professionally

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
- Verify your event listeners are set up correctly with `addEventListener`
- Check that button IDs match what you're selecting in JavaScript
- Make sure `DOMContentLoaded` event fires before setting up listeners
- Check browser console (F12) for JavaScript errors

## Real-World Applications

This calculator demonstrates patterns you'll use in:
- **Lightning Network**: Converting between base units and millisatoshis
- **Payment Processors**: Handling different denomination displays
- **Wallet Applications**: Showing amounts in user-preferred units

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

🎉 **You built your very own Bitcoin calculator!** 

This is exactly the kind of tool Bitcoin developers create every day. You're building real skills that matter in the Bitcoin ecosystem.

Don't forget to:
1. Test your calculator with different amounts
2. Push your code to GitHub  
3. Share it with the PlebDevs community

Ready to level up with data structures? Let's build a wallet tracker! 🚀

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 