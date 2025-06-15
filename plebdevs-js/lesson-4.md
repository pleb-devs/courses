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

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `bitcoin-fee-calculator`
2. Open in VS Code
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