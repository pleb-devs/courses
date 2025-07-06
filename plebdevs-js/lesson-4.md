[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-4.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-4.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-4.webm)

# Lesson 4: Transaction Fee Calculator

## Making Smart Decisions in Code!

Welcome back, PlebDev! 🔥 You've built some impressive projects so far. Now we're adding intelligence to your code with conditional logic - the ability to make decisions based on different conditions.

In this lesson, you'll build a Bitcoin transaction fee calculator that helps users choose the right fee for their transaction priority. This is essential knowledge for any Bitcoin developer and introduces you to the decision-making patterns used in all sophisticated applications.

## What You'll Learn

### JavaScript Concepts
- **Conditional Logic**: Making decisions with if/else statements
- **Comparison Operators**: Checking values against each other
- **Logical Operators**: Combining multiple conditions
- **User Input Validation**: Ensuring data quality before processing

### Bitcoin Development Skills
- Understanding Bitcoin transaction fees
- Implementing fee estimation logic
- Creating user-friendly fee selection
- Building smart validation systems

## Prerequisites
- Completed Lessons 1-3
- Understanding of functions and data structures
- Ready to add intelligence to your applications

## Project Overview: Bitcoin Fee Estimator

We're building a calculator that:
- Estimates fees based on transaction priority (Fast, Medium, Low)
- Shows confirmation time estimates
- Validates user input intelligently
- Provides warnings for high fees
- Demonstrates real-world Bitcoin fee logic

## Key Concepts Explained

This lesson introduces decision-making logic that transforms simple calculators into intelligent Bitcoin applications:

### Conditional Logic: The Brain of Your Application
Conditional statements are how programs make decisions. They're the "if this, then that" logic that makes applications smart:

```javascript
// Basic if statement - single condition
let feeRate = 50; // sats per vByte
if (feeRate > 30) {
    console.log("⚠️ High fee warning!");
    // Only runs if condition is true
}

// if/else statement - two possibilities
if (feeRate > 30) {
    console.log("🔥 High priority - expensive but fast");
} else {
    console.log("✅ Normal fee - good value");
}

// if/else if/else chain - multiple conditions
if (feeRate > 50) {
    console.log("🚨 Very high fee - next block confirmation");
} else if (feeRate > 25) {
    console.log("⚡ Medium fee - ~30 minute confirmation");
} else if (feeRate > 10) {
    console.log("🐌 Low fee - ~1 hour confirmation");
} else {
    console.log("⏳ Very low fee - could take hours");
}

// Nested conditions
if (feeRate > 0) {
    if (feeRate > 100) {
        console.log("💸 Extremely expensive!");
        if (confirm("Are you sure you want to pay this much?")) {
            console.log("Proceeding with high fee...");
        }
    } else {
        console.log("Reasonable fee range");
    }
} else {
    console.log("❌ Invalid fee rate");
}

// Complex conditions with logical operators
let txSize = 250; // vBytes
let urgency = "high";
let walletBalance = 2.5; // BTC

if (feeRate > 50 && urgency === "high") {
    console.log("High fee but urgent - proceeding");
} else if (feeRate > 20 || txSize > 1000) {
    console.log("Either high fee or large transaction");
} else if (!(walletBalance > 0.01)) {
    console.log("Insufficient balance for fees");
}
```

**Real Bitcoin conditional examples:**
```javascript
// Transaction validation
function validateTransaction(amount, fee, balance) {
    if (amount <= 0) {
        return { valid: false, error: "Amount must be positive" };
    }
    
    if (amount + fee > balance) {
        return { valid: false, error: "Insufficient funds (including fee)" };
    }
    
    if (amount > 50) { // Large transaction check
        return { 
            valid: true, 
            warning: "Large transaction - double check recipient address" 
        };
    }
    
    return { valid: true };
}

// Fee recommendation logic
function recommendFee(memPoolSize, urgency) {
    let baseFee = 10; // sats/vByte
    
    // Adjust for mempool congestion
    if (memPoolSize > 100000) {
        baseFee *= 2; // Double fee when congested
    } else if (memPoolSize > 50000) {
        baseFee *= 1.5; // 50% increase for medium congestion
    }
    
    // Adjust for user urgency
    switch (urgency) {
        case "urgent":
            return baseFee * 2;
        case "normal":
            return baseFee;
        case "patient":
            return Math.max(baseFee * 0.5, 1); // At least 1 sat/vByte
        default:
            return baseFee;
    }
}

// Address type detection
function getAddressType(address) {
    if (address.startsWith('1')) {
        return 'P2PKH (Legacy)';
    } else if (address.startsWith('3')) {
        return 'P2SH (Script)';
    } else if (address.startsWith('bc1q')) {
        return 'P2WPKH (SegWit)';
    } else if (address.startsWith('bc1p')) {
        return 'P2TR (Taproot)';
    } else {
        return 'Unknown';
    }
}
```

### Comparison Operators: The Building Blocks of Logic
These operators compare values and return true or false (boolean values):

```javascript
let currentPrice = 95000;
let targetPrice = 100000;
let fee = 25;
let minFee = 10;
let maxFee = 100;

// Numeric comparisons
console.log(currentPrice > targetPrice);    // false - 95000 > 100000
console.log(currentPrice < targetPrice);    // true  - 95000 < 100000
console.log(fee >= minFee);                 // true  - 25 >= 10
console.log(fee <= maxFee);                 // true  - 25 <= 100

// Equality comparisons (IMPORTANT: use === not ==)
console.log(fee === 25);                    // true  - exact match
console.log(fee !== 30);                    // true  - not equal
console.log(fee == "25");                   // true  - but avoid this!
console.log(fee === "25");                  // false - number vs string

// String comparisons
let address1 = "bc1q742d6ccq93c9cxh6f5nj...";
let address2 = "bc1q742d6ccq93c9cxh6f5nj...";
console.log(address1 === address2);         // true - exact string match

// Logical operators for combining conditions
let isHighFee = fee > 50;
let isUrgent = true;
let hasBalance = true;

console.log(isHighFee && isUrgent);         // AND - both must be true
console.log(isHighFee || isUrgent);         // OR - either can be true
console.log(!isHighFee);                    // NOT - opposite of isHighFee

// Complex logical expressions
let shouldProceed = hasBalance && (isUrgent || !isHighFee);
let needsWarning = isHighFee && !isUrgent;

// Truthiness and falsiness (advanced but important)
if (0) console.log("Won't run");            // 0 is falsy
if ("") console.log("Won't run");           // empty string is falsy
if (null) console.log("Won't run");         // null is falsy
if (undefined) console.log("Won't run");    // undefined is falsy
if (false) console.log("Won't run");        // false is falsy

if (1) console.log("Will run");             // non-zero numbers are truthy
if ("text") console.log("Will run");        // non-empty strings are truthy
if ([]) console.log("Will run");            // arrays are truthy (even empty)
if ({}) console.log("Will run");            // objects are truthy (even empty)
```

### User Input Validation: Professional Data Handling
Real Bitcoin applications must validate every piece of user input to prevent errors and security issues:

```javascript
// Comprehensive amount validation
function validateBitcoinAmount(input) {
    // Step 1: Check if input exists
    if (!input || input.trim() === '') {
        return { valid: false, error: "Amount is required" };
    }
    
    // Step 2: Convert to number
    const amount = parseFloat(input);
    
    // Step 3: Check if conversion worked
    if (isNaN(amount)) {
        return { valid: false, error: "Please enter a valid number" };
    }
    
    // Step 4: Check positive value
    if (amount <= 0) {
        return { valid: false, error: "Amount must be greater than zero" };
    }
    
    // Step 5: Check maximum supply
    if (amount > 21000000) {
        return { valid: false, error: "Amount exceeds total Bitcoin supply" };
    }
    
    // Step 6: Check decimal precision (Bitcoin has 8 decimal places)
    const decimalPlaces = (input.split('.')[1] || '').length;
    if (decimalPlaces > 8) {
        return { valid: false, error: "Bitcoin amounts cannot have more than 8 decimal places" };
    }
    
    // Step 7: Check dust amount (very small amounts)
    const satoshis = amount * 100000000;
    if (satoshis < 546) { // 546 sats is typical dust limit
        return { 
            valid: false, 
            error: "Amount too small - minimum is 546 satoshis (0.00000546 BTC)" 
        };
    }
    
    return { valid: true, amount: amount };
}

// Address validation
function validateBitcoinAddress(address) {
    if (!address || typeof address !== 'string') {
        return { valid: false, error: "Address is required" };
    }
    
    address = address.trim();
    
    // Check length
    if (address.length < 26 || address.length > 62) {
        return { valid: false, error: "Invalid address length" };
    }
    
    // Check format
    const validFormats = [
        /^1[a-km-z1-9]{25,34}$/i,    // P2PKH (Legacy)
        /^3[a-km-z1-9]{25,34}$/i,    // P2SH (Script)
        /^bc1q[a-z0-9]{39}$/i,       // P2WPKH (SegWit)
        /^bc1p[a-z0-9]{59}$/i        // P2TR (Taproot)
    ];
    
    const isValidFormat = validFormats.some(regex => regex.test(address));
    
    if (!isValidFormat) {
        return { valid: false, error: "Invalid Bitcoin address format" };
    }
    
    return { valid: true, address: address };
}

// Fee rate validation
function validateFeeRate(input, txSize = 250) {
    const validation = validateBitcoinAmount(input);
    if (!validation.valid) {
        return validation;
    }
    
    const feeRate = validation.amount;
    
    // Check reasonable limits
    if (feeRate < 1) {
        return { valid: false, error: "Fee rate too low - minimum 1 sat/vByte" };
    }
    
    if (feeRate > 1000) {
        return { 
            valid: false, 
            error: "Fee rate extremely high - please double-check" 
        };
    }
    
    // Calculate total fee
    const totalFee = feeRate * txSize;
    const totalFeeBTC = totalFee / 100000000;
    
    // Warn about high fees
    if (totalFeeBTC > 0.001) { // More than 0.001 BTC fee
        return {
            valid: true,
            feeRate: feeRate,
            warning: `High fee: ${totalFeeBTC.toFixed(8)} BTC ($${(totalFeeBTC * 95000).toFixed(2)})`
        };
    }
    
    return { valid: true, feeRate: feeRate };
}

// Comprehensive form validation
function validateTransactionForm(formData) {
    const errors = [];
    const warnings = [];
    
    // Validate recipient
    const recipientValidation = validateBitcoinAddress(formData.recipient);
    if (!recipientValidation.valid) {
        errors.push(`Recipient: ${recipientValidation.error}`);
    }
    
    // Validate amount
    const amountValidation = validateBitcoinAmount(formData.amount);
    if (!amountValidation.valid) {
        errors.push(`Amount: ${amountValidation.error}`);
    }
    
    // Validate fee
    const feeValidation = validateFeeRate(formData.feeRate);
    if (!feeValidation.valid) {
        errors.push(`Fee: ${feeValidation.error}`);
    } else if (feeValidation.warning) {
        warnings.push(feeValidation.warning);
    }
    
    // Cross-validation (check relationships between fields)
    if (amountValidation.valid && feeValidation.valid) {
        const totalNeeded = amountValidation.amount + (feeValidation.feeRate * 250 / 100000000);
        const availableBalance = parseFloat(formData.balance || 0);
        
        if (totalNeeded > availableBalance) {
            errors.push(`Insufficient funds: Need ${totalNeeded.toFixed(8)} BTC, have ${availableBalance.toFixed(8)} BTC`);
        }
    }
    
    return {
        valid: errors.length === 0,
        errors: errors,
        warnings: warnings
    };
}
```

### Ternary Operator: Concise Conditional Logic
The ternary operator is a shorthand for simple if/else statements:

```javascript
// Basic ternary syntax: condition ? valueIfTrue : valueIfFalse
let fee = 45;
let feeStatus = fee > 30 ? "High" : "Normal";
console.log(feeStatus); // "High"

// Equivalent if/else statement
let feeStatusLong;
if (fee > 30) {
    feeStatusLong = "High";
} else {
    feeStatusLong = "Normal";
}

// Multiple ternary operators (chaining)
let priority = fee > 50 ? "Urgent" : fee > 25 ? "Medium" : "Low";

// Using ternary in template literals
let message = `Fee: ${fee} sats/vByte (${fee > 30 ? 'Expensive' : 'Reasonable'})`;

// Ternary for function calls
let displayColor = fee > 50 ? setRedColor() : setGreenColor();

// Ternary for object properties
let transactionDetails = {
    amount: 1.5,
    fee: fee,
    status: fee > 100 ? 'warning' : 'normal',
    canProceed: fee <= 1000,
    displayClass: fee > 50 ? 'fee-high' : fee > 20 ? 'fee-medium' : 'fee-low'
};

// Real Bitcoin examples with ternary
function formatTransactionStatus(confirmations) {
    return confirmations >= 6 ? 'Confirmed ✅' : 
           confirmations >= 1 ? `${confirmations} confirmations ⏳` : 
           'Unconfirmed ⏸️';
}

function getNetworkIcon(network) {
    return network === 'mainnet' ? '🟠' : 
           network === 'testnet' ? '🟡' : 
           network === 'regtest' ? '🔵' : '❓';
}

function calculateRecommendedAction(price, targetBuyPrice, targetSellPrice) {
    return price < targetBuyPrice ? 'BUY 📈' : 
           price > targetSellPrice ? 'SELL 📉' : 
           'HOLD 💎';
}

// Complex ternary with Bitcoin logic
function getTransactionPriority(feeRate, mempoolSize, userUrgency) {
    return userUrgency === 'urgent' ? 
        (feeRate > 50 ? 'Next Block' : 'High Priority') :
        mempoolSize > 100000 ? 
            (feeRate > 20 ? 'Medium Priority' : 'Low Priority') :
            (feeRate > 10 ? 'Normal' : 'Economy');
}
```

**Why conditional logic is essential for Bitcoin development:**
- **Safety**: Validates user input before processing Bitcoin transactions
- **User Experience**: Provides helpful feedback and warnings
- **Business Logic**: Implements fee estimation, priority levels, and transaction rules
- **Error Handling**: Gracefully handles edge cases and invalid data
- **Smart Defaults**: Automatically adjusts behavior based on network conditions

**Common Bitcoin conditional patterns:**
```javascript
// Network-aware logic
const isMainnet = network === 'bitcoin';
const addressPrefix = isMainnet ? 'bc1' : 'tb1';
const explorerURL = isMainnet ? 'blockstream.info' : 'blockstream.info/testnet';

// Confirmation-based logic
const isFullyConfirmed = confirmations >= 6;
const isSafeToSpend = isFullyConfirmed || (amount < 0.01 && confirmations >= 1);

// Fee-based recommendations
const isEconomical = feeRate < 20;
const isFast = feeRate > 50;
const recommendation = isEconomical ? 'Good value' : isFast ? 'Fast confirmation' : 'Balanced choice';
```

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `bitcoin-fee-calculator`
2. Open in Code Editor
3. Create `index.html`

### Step 2: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Fee Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 40px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            margin: 0;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .calculator {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 600px;
        }
        .priority-section {
            margin: 25px 0;
        }
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #333;
        }
        select {
            width: 100%;
            padding: 15px;
            font-size: 1.1em;
            border: 2px solid #ddd;
            border-radius: 8px;
            background: white;
        }
        select:focus {
            border-color: #667eea;
            outline: none;
        }
        .calculate-btn {
            width: 100%;
            padding: 18px;
            font-size: 1.2em;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin: 20px 0;
        }
        .calculate-btn:hover {
            opacity: 0.9;
        }
        .result-section {
            margin-top: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 10px;
            min-height: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        .fee-display {
            font-size: 2em;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }
        .fee-warning {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            color: #856404;
            padding: 15px;
            border-radius: 5px;
            margin-top: 15px;
        }
        .fee-good {
            background: #d4edda;
            border: 1px solid #c3e6cb;
            color: #155724;
            padding: 15px;
            border-radius: 5px;
            margin-top: 15px;
        }
        .advanced-options {
            margin-top: 20px;
            padding: 20px;
            background: #f1f3f4;
            border-radius: 8px;
        }
        .input-group {
            margin: 15px 0;
        }
        input[type="number"] {
            width: 100%;
            padding: 12px;
            font-size: 1em;
            border: 2px solid #ddd;
            border-radius: 6px;
            box-sizing: border-box;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h1>⚡ Bitcoin Fee Calculator</h1>
        <p>Choose your transaction priority and get fee estimates</p>
        
        <div class="priority-section">
            <label for="priority">Transaction Priority:</label>
            <select id="priority">
                <option value="">Select Priority Level</option>
                <option value="high">🚀 High Priority (Next Block ~10 min)</option>
                <option value="medium">🚶 Medium Priority (~30 min)</option>
                <option value="low">🐌 Low Priority (~1 hour)</option>
                <option value="custom">⚙️ Custom Fee Rate</option>
            </select>
        </div>
        
        <div id="custom-section" class="advanced-options" style="display: none;">
            <div class="input-group">
                <label for="custom-rate">Custom Rate (sats/vByte):</label>
                <input type="number" id="custom-rate" placeholder="Enter rate..." min="1">
            </div>
        </div>
        
        <div class="advanced-options">
            <div class="input-group">
                <label for="tx-size">Transaction Size (vBytes):</label>
                <input type="number" id="tx-size" value="250" min="100" max="10000">
                <small>Average single transaction: 250 vBytes</small>
            </div>
        </div>
        
        <button class="calculate-btn" onclick="calculateFee()">
            Calculate Transaction Fee
        </button>
        
        <div id="result" class="result-section">
            <p>Select a priority level above to calculate fees</p>
        </div>
    </div>
    
    <script>
        // Show/hide custom fee input
        document.getElementById('priority').addEventListener('change', function() {
            const customSection = document.getElementById('custom-section');
            if (this.value === 'custom') {
                customSection.style.display = 'block';
            } else {
                customSection.style.display = 'none';
            }
        });
    </script>
</body>
</html>
```

### Step 3: Understanding Conditional Logic
The foundation of smart applications:

```javascript
// Basic if statement
if (condition)  {
    // Code runs if condition is true
}

// if/else statement  
if (condition) {
    // Code runs if condition is true
} else {
    // Code runs if condition is false
}

// Multiple conditions
if (condition1) {
    // First option
} else if (condition2) {
    // Second option  
} else {
    // Default option
}
```

### Step 4: Comparison Operators
```javascript
// Equality
const price = 50000;
if (price === 50000) {
    console.log("Price matches exactly");
}

// Inequality  
if (price !== 45000) {
    console.log("Price is not 45000");
}

// Greater than / Less than
if (price > 40000) {
    console.log("Price is above 40k");
}

if (price <= 60000) {
    console.log("Price is 60k or below");
}
```

### Step 5: Building the Fee Calculator Logic
```javascript
function calculateFee() {
    // Get user selections
    const priority = document.getElementById("priority").value;
    const txSize = parseInt(document.getElementById("tx-size").value);
    
    // Input validation
    if (!priority) {
        showError("Please select a priority level");
        return;
    }
    
    if (!txSize || txSize < 100) {
        showError("Please enter a valid transaction size (minimum 100 vBytes)");
        return;
    }
    
    // Determine fee rate based on priority
    let satsPerByte;
    let confirmationTime;
    
    if (priority === "high") {
        satsPerByte = 50;
        confirmationTime = "~10 minutes (next block)";
    } else if (priority === "medium") {
        satsPerByte = 25;
        confirmationTime = "~30 minutes (2-3 blocks)";
    } else if (priority === "low") {
        satsPerByte = 10;
        confirmationTime = "~1 hour (6+ blocks)";
    } else if (priority === "custom") {
        satsPerByte = parseInt(document.getElementById("custom-rate").value);
        if (!satsPerByte || satsPerByte < 1) {
            showError("Please enter a valid custom fee rate");
            return;
        }
        confirmationTime = "Depends on network conditions";
    }
    
    // Calculate total fee
    const totalFee = satsPerByte * txSize;
    
    // Display result
    displayResult(totalFee, satsPerByte, confirmationTime, txSize);
}
```

### Step 6: Smart Fee Warnings
```javascript
function displayResult(totalFee, satsPerByte, confirmationTime, txSize) {
    const resultDiv = document.getElementById("result");
    
    // Determine if fee is reasonable
    let feeStatus = "";
    let statusClass = "";
    
    if (totalFee > 50000) {  // More than 50,000 sats
        feeStatus = "⚠️ HIGH FEE WARNING - This is expensive!";
        statusClass = "fee-warning";
    } else if (totalFee > 20000) {  // 20,000-50,000 sats
        feeStatus = "💰 Moderate fee - Consider if this is necessary";
        statusClass = "fee-warning";
    } else if (totalFee > 5000) {   // 5,000-20,000 sats
        feeStatus = "✅ Reasonable fee for current network conditions";
        statusClass = "fee-good";
    } else {  // Under 5,000 sats
        feeStatus = "✅ Low fee - Good for non-urgent transactions";
        statusClass = "fee-good";
    }
    
    resultDiv.innerHTML = `
        <div>
            <div class="fee-display">${totalFee.toLocaleString()} sats</div>
            <p><strong>Fee Rate:</strong> ${satsPerByte} sats/vByte</p>
            <p><strong>Transaction Size:</strong> ${txSize} vBytes</p>
            <p><strong>Confirmation Time:</strong> ${confirmationTime}</p>
            <div class="${statusClass}">
                ${feeStatus}
            </div>
        </div>
    `;
}

function showError(message) {
    document.getElementById("result").innerHTML = `
        <div style="color: #dc3545;">
            ❌ ${message}
        </div>
    `;
}
```

### Step 7: Complete Application with Advanced Features
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Fee Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 40px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            margin: 0;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .calculator {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            width: 100%;
            max-width: 600px;
        }
        .priority-section {
            margin: 25px 0;
        }
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #333;
        }
        select, input[type="number"] {
            width: 100%;
            padding: 15px;
            font-size: 1.1em;
            border: 2px solid #ddd;
            border-radius: 8px;
            background: white;
            box-sizing: border-box;
        }
        select:focus, input:focus {
            border-color: #667eea;
            outline: none;
        }
        .calculate-btn {
            width: 100%;
            padding: 18px;
            font-size: 1.2em;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin: 20px 0;
        }
        .calculate-btn:hover {
            opacity: 0.9;
        }
        .result-section {
            margin-top: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 10px;
            min-height: 100px;
        }
        .fee-display {
            font-size: 2em;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 15px;
            text-align: center;
        }
        .fee-warning {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            color: #856404;
            padding: 15px;
            border-radius: 5px;
            margin-top: 15px;
        }
        .fee-good {
            background: #d4edda;
            border: 1px solid #c3e6cb;
            color: #155724;
            padding: 15px;
            border-radius: 5px;
            margin-top: 15px;
        }
        .advanced-options {
            margin-top: 20px;
            padding: 20px;
            background: #f1f3f4;
            border-radius: 8px;
        }
        .input-group {
            margin: 15px 0;
        }
        .comparison-table {
            margin-top: 20px;
            width: 100%;
            border-collapse: collapse;
        }
        .comparison-table th,
        .comparison-table td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }
        .comparison-table th {
            background: #f2f2f2;
        }
        small {
            color: #666;
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h1>⚡ Bitcoin Fee Calculator</h1>
        <p>Choose your transaction priority and get accurate fee estimates</p>
        
        <div class="priority-section">
            <label for="priority">Transaction Priority:</label>
            <select id="priority">
                <option value="">Select Priority Level</option>
                <option value="high">🚀 High Priority (Next Block ~10 min)</option>
                <option value="medium">🚶 Medium Priority (~30 min)</option>
                <option value="low">🐌 Low Priority (~1 hour)</option>
                <option value="custom">⚙️ Custom Fee Rate</option>
            </select>
        </div>
        
        <div id="custom-section" class="advanced-options" style="display: none;">
            <div class="input-group">
                <label for="custom-rate">Custom Rate (sats/vByte):</label>
                <input type="number" id="custom-rate" placeholder="Enter rate..." min="1">
            </div>
        </div>
        
        <div class="advanced-options">
            <div class="input-group">
                <label for="tx-size">Transaction Size (vBytes):</label>
                <input type="number" id="tx-size" value="250" min="100" max="10000">
                <small>Average single transaction: 250 vBytes | Complex transaction: 500+ vBytes</small>
            </div>
        </div>
        
        <button class="calculate-btn" onclick="calculateFee()">
            Calculate Transaction Fee
        </button>
        
        <div id="result" class="result-section">
            <p style="text-align: center; color: #666;">Select a priority level above to calculate fees</p>
        </div>
    </div>
    
    <script>
        // Show/hide custom fee input based on selection
        document.getElementById('priority').addEventListener('change', function() {
            const customSection = document.getElementById('custom-section');
            if (this.value === 'custom') {
                customSection.style.display = 'block';
            } else {
                customSection.style.display = 'none';
            }
        });
        
        // Main fee calculation function
        function calculateFee() {
            // Get user inputs
            const priority = document.getElementById("priority").value;
            const txSize = parseInt(document.getElementById("tx-size").value);
            
            // Validate inputs
            if (!priority) {
                showError("Please select a priority level");
                return;
            }
            
            if (!txSize || txSize < 100 || txSize > 10000) {
                showError("Please enter a valid transaction size (100-10,000 vBytes)");
                return;
            }
            
            // Determine fee parameters based on priority
            let satsPerByte;
            let confirmationTime;
            let priorityDescription;
            
            if (priority === "high") {
                satsPerByte = 50;
                confirmationTime = "~10 minutes (next block)";
                priorityDescription = "High Priority";
            } else if (priority === "medium") {
                satsPerByte = 25;
                confirmationTime = "~30 minutes (2-3 blocks)";
                priorityDescription = "Medium Priority";
            } else if (priority === "low") {
                satsPerByte = 10;
                confirmationTime = "~1 hour (6+ blocks)";
                priorityDescription = "Low Priority";
            } else if (priority === "custom") {
                satsPerByte = parseInt(document.getElementById("custom-rate").value);
                if (!satsPerByte || satsPerByte < 1) {
                    showError("Please enter a valid custom fee rate (minimum 1 sat/vByte)");
                    return;
                }
                confirmationTime = "Depends on network conditions";
                priorityDescription = "Custom Rate";
            }
            
            // Calculate total fee
            const totalFee = satsPerByte * txSize;
            
            // Display result with smart analysis
            displayResult(totalFee, satsPerByte, confirmationTime, txSize, priorityDescription);
        }
        
        // Display results with intelligent fee analysis
        function displayResult(totalFee, satsPerByte, confirmationTime, txSize, priorityDescription) {
            const resultDiv = document.getElementById("result");
            
            // Intelligent fee analysis
            let feeStatus = "";
            let statusClass = "";
            let advice = "";
            
            if (totalFee > 100000) {  // More than 100,000 sats (0.001 BTC)
                feeStatus = "🚨 VERY HIGH FEE - Double check this is correct!";
                statusClass = "fee-warning";
                advice = "This fee is extremely high. Consider waiting for lower network congestion.";
            } else if (totalFee > 50000) {  // 50,000-100,000 sats
                feeStatus = "⚠️ HIGH FEE - This is expensive for most transactions";
                statusClass = "fee-warning";
                advice = "High fee period. Consider if this transaction is urgent.";
            } else if (totalFee > 20000) {  // 20,000-50,000 sats
                feeStatus = "💰 Moderate fee - Reasonable for urgent transactions";
                statusClass = "fee-warning";
                advice = "Moderate fee. Good for important transactions during busy periods.";
            } else if (totalFee > 5000) {   // 5,000-20,000 sats
                feeStatus = "✅ Reasonable fee for current network conditions";
                statusClass = "fee-good";
                advice = "Good balance of speed and cost.";
            } else {  // Under 5,000 sats
                feeStatus = "✅ Low fee - Economical choice";
                statusClass = "fee-good";
                advice = "Great for non-urgent transactions. May take longer during busy periods.";
            }
            
            // Calculate fee as percentage of common amounts
            const feeInBTC = totalFee / 100000000;
            const feePercentage1BTC = (feeInBTC / 1 * 100).toFixed(4);
            const feePercentage01BTC = (feeInBTC / 0.1 * 100).toFixed(2);
            
            resultDiv.innerHTML = `
                <div class="fee-display">${totalFee.toLocaleString()} sats</div>
                
                <table style="width: 100%; margin-bottom: 20px;">
                    <tr>
                        <td><strong>Priority:</strong></td>
                        <td>${priorityDescription}</td>
                    </tr>
                    <tr>
                        <td><strong>Fee Rate:</strong></td>
                        <td>${satsPerByte} sats/vByte</td>
                    </tr>
                    <tr>
                        <td><strong>Transaction Size:</strong></td>
                        <td>${txSize} vBytes</td>
                    </tr>
                    <tr>
                        <td><strong>Confirmation Time:</strong></td>
                        <td>${confirmationTime}</td>
                    </tr>
                    <tr>
                        <td><strong>Fee in BTC:</strong></td>
                        <td>${feeInBTC.toFixed(8)} BTC</td>
                    </tr>
                </table>
                
                <div class="${statusClass}">
                    <strong>${feeStatus}</strong><br>
                    ${advice}
                </div>
                
                <div style="margin-top: 15px; padding: 10px; background: #f8f9fa; border-radius: 5px; font-size: 0.9em;">
                    <strong>Fee Context:</strong><br>
                    • ${feePercentage1BTC}% of 1 BTC transaction<br>
                    • ${feePercentage01BTC}% of 0.1 BTC transaction
                </div>
            `;
        }
        
        // Show error messages
        function showError(message) {
            document.getElementById("result").innerHTML = `
                <div style="color: #dc3545; text-align: center; padding: 20px;">
                    ❌ ${message}
                </div>
            `;
        }
        
        // Show comparison when page loads
        window.onload = function() {
            document.getElementById("result").innerHTML = `
                <div style="text-align: center;">
                    <h3>Current Fee Rates (Estimated)</h3>
                    <table class="comparison-table">
                        <tr>
                            <th>Priority</th>
                            <th>Rate (sats/vByte)</th>
                            <th>250 vByte TX</th>
                            <th>Time</th>
                        </tr>
                        <tr>
                            <td>High 🚀</td>
                            <td>50</td>
                            <td>12,500 sats</td>
                            <td>~10 min</td>
                        </tr>
                        <tr>
                            <td>Medium 🚶</td>
                            <td>25</td>
                            <td>6,250 sats</td>
                            <td>~30 min</td>
                        </tr>
                        <tr>
                            <td>Low 🐌</td>
                            <td>10</td>
                            <td>2,500 sats</td>
                            <td>~1 hour</td>
                        </tr>
                    </table>
                    <p><small>Select a priority above to calculate your specific transaction fee</small></p>
                </div>
            `;
        };
    </script>
</body>
</html>
```

## Key Takeaways

### Conditional Logic
- **if statements**: Execute code only when conditions are true
- **else statements**: Provide alternative actions
- **else if**: Handle multiple different conditions
- **Logical flow**: Make applications respond intelligently

### Comparison Operators
- **===**: Strict equality (recommended)
- **!==**: Strict inequality  
- **>**, **<**: Greater than, less than
- **>=**, **<=**: Greater/less than or equal
- **Logical operators**: `&&` (and), `||` (or), `!` (not)

### Input Validation
- **Always validate**: Never trust user input
- **Provide feedback**: Tell users what went wrong
- **Handle edge cases**: Consider extreme values
- **Graceful degradation**: Fail safely with helpful messages

### Smart User Experience
- **Progressive disclosure**: Show advanced options when needed
- **Contextual help**: Provide relevant information
- **Visual feedback**: Use colors and icons effectively
- **Intelligent defaults**: Start with sensible values

## Real-World Applications

These patterns are everywhere in Bitcoin development:
- **Wallet Applications**: Validating addresses and amounts
- **Exchange Interfaces**: Setting trading limits and warnings
- **Payment Processors**: Implementing fraud detection
- **DeFi Protocols**: Managing risk parameters
- **Lightning Apps**: Route selection and channel management

## Challenge Yourself

1. **Add USD Conversion**: Show fee costs in fiat currency
2. **Historical Data**: Compare current fees to historical averages
3. **Batch Transactions**: Calculate fees for multiple outputs
4. **RBF Support**: Add Replace-By-Fee calculation options
5. **Network Stats**: Show current mempool information

## Common Issues and Solutions

### Conditions Not Working
- Use `===` instead of `=` (assignment vs comparison)
- Check for typos in variable names
- Remember parentheses for complex conditions: `(a && b) || c`

### Validation Failing
- Check data types: `parseInt()` for numbers, `typeof` for checking types
- Handle empty strings: `if (!value || value === "")`
- Consider edge cases: negative numbers, very large numbers

### Logic Errors
- Test all paths: what happens with each possible input?
- Use console.log() to debug: see what values you're actually getting
- Break complex conditions into smaller, testable pieces

## Next Steps

Excellent work! You've mastered conditional logic - the foundation of intelligent applications. In Lesson 5, we'll add time-based functionality to build a live block explorer interface.

### What's Coming Next
- **Lesson 5**: Working with time and intervals
- **Real-world skills**: Creating live, updating interfaces
- **New concepts**: setTimeout, setInterval, and Date objects

## Resources for Going Deeper

- [MDN Conditional Statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [Comparison Operators Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)
- [Logical Operators Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators)

## Share Your Success!

🎉 **You built an intelligent fee calculator!** 

This demonstrates real Bitcoin development skills used in professional applications. Your code can make smart decisions and guide users effectively.

Don't forget to:
1. Test different fee scenarios and transaction sizes
2. Push your code to GitHub
3. Share your calculator with the Bitcoin community

Ready to add real-time features? Let's build a live block explorer! ⛏️

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 