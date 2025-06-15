[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-6.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-6.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-6.webm)

# Lesson 6: Lightning Invoice Generator

## Welcome to the Lightning Network, PlebDev! ⚡

Outstanding progress! You've built real-time applications and mastered the fundamentals. Now we're diving into one of the most exciting parts of the Bitcoin ecosystem: the Lightning Network. 

In this lesson, you'll build a Lightning invoice parser and generator that handles real Lightning Network data. This introduces you to string manipulation, pattern matching, and input sanitization - essential skills for working with Bitcoin and Lightning data formats.

## What You'll Learn

### JavaScript Concepts
- **String Methods**: Manipulating and analyzing text data
- **Regular Expressions**: Pattern matching for data validation
- **Input Sanitization**: Cleaning and validating user input
- **Error Handling**: Graceful handling of invalid data

### Bitcoin Development Skills
- Understanding Lightning Network invoices (BOLT-11)
- Parsing Bitcoin and Lightning data formats
- Creating robust input validation
- Building user-friendly Bitcoin interfaces

## Prerequisites
- Completed Lessons 1-5
- Understanding of conditional logic and functions
- Ready to work with complex data formats

## Project Overview: Lightning Invoice Helper

We're building a tool that:
- Parses Lightning Network invoices (BOLT-11 format)
- Extracts key information (amount, description, expiry)
- Validates invoice format and structure
- Provides user-friendly error messages
- Demonstrates professional data handling

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `lightning-invoice-helper`
2. Open in VS Code
3. Create `index.html`

### Step 2: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Lightning Invoice Helper</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            overflow: hidden;
        }
        .header {
            background: linear-gradient(135deg, #ff9500, #ff7700);
            color: white;
            padding: 30px;
            text-align: center;
        }
        .content {
            padding: 40px;
        }
        .input-section {
            margin-bottom: 30px;
        }
        label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
            color: #333;
        }
        .invoice-input {
            width: 100%;
            padding: 15px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 14px;
            font-family: 'Courier New', monospace;
            resize: vertical;
            min-height: 120px;
            box-sizing: border-box;
        }
        .invoice-input:focus {
            border-color: #667eea;
            outline: none;
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }
        .parse-btn {
            width: 100%;
            padding: 18px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.2em;
            font-weight: bold;
            cursor: pointer;
            margin: 20px 0;
        }
        .parse-btn:hover {
            opacity: 0.9;
        }
        .result-section {
            margin-top: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 12px;
            min-height: 100px;
        }
        .error {
            background: #fff5f5;
            border: 2px solid #fed7d7;
            color: #c53030;
            padding: 20px;
            border-radius: 10px;
        }
        .success {
            background: #f0fff4;
            border: 2px solid #9ae6b4;
            color: #2f855a;
        }
        .invoice-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 20px;
        }
        .info-item {
            background: white;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }
        .info-label {
            font-weight: bold;
            color: #666;
            font-size: 0.9em;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }
        .info-value {
            font-size: 1.2em;
            color: #333;
            word-break: break-all;
        }
        .amount-display {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff9500;
        }
        .tools-section {
            margin-top: 30px;
            padding: 25px;
            background: #f1f3f4;
            border-radius: 12px;
        }
        .tool-btn {
            background: #ff9500;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            margin: 5px;
            font-weight: bold;
        }
        .tool-btn:hover {
            background: #e08600;
        }
        .sample-section {
            margin-top: 20px;
            padding: 15px;
            background: #e6f3ff;
            border-radius: 8px;
            border-left: 4px solid #0066cc;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>⚡ Lightning Invoice Helper</h1>
            <p>Parse and analyze Lightning Network invoices (BOLT-11)</p>
        </div>
        
        <div class="content">
            <div class="input-section">
                <label for="invoice-input">Lightning Network Invoice:</label>
                <textarea 
                    id="invoice-input" 
                    class="invoice-input" 
                    placeholder="Paste your Lightning Network invoice here (starts with lnbc, lntb, or lnbcrt)..."
                ></textarea>
            </div>
            
            <button class="parse-btn" onclick="parseInvoice()">
                🔍 Parse Invoice
            </button>
            
            <div id="result" class="result-section">
                <p style="text-align: center; color: #666;">
                    Paste a Lightning Network invoice above to analyze it
                </p>
            </div>
            
            <div class="tools-section">
                <h3>Quick Tools</h3>
                <button class="tool-btn" onclick="loadSampleInvoice()">Load Sample Invoice</button>
                <button class="tool-btn" onclick="clearInput()">Clear Input</button>
                <button class="tool-btn" onclick="generateRandomAmount()">Generate Test Amount</button>
            </div>
            
            <div class="sample-section">
                <h4>💡 What is a Lightning Invoice?</h4>
                <p>Lightning Network invoices (BOLT-11) are payment requests that contain:</p>
                <ul>
                    <li><strong>Amount:</strong> How many satoshis to pay</li>
                    <li><strong>Description:</strong> What the payment is for</li>
                    <li><strong>Payment Hash:</strong> Unique identifier</li>
                    <li><strong>Expiry:</strong> When the invoice expires</li>
                    <li><strong>Routing Info:</strong> How to reach the recipient</li>
                </ul>
            </div>
        </div>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 3: Understanding String Methods
```javascript
// Essential string methods for data processing
const invoice = "lnbc1500n1pjkx7fhpp5...";

// Basic string operations
const length = invoice.length;
const upperCase = invoice.toUpperCase();
const lowerCase = invoice.toLowerCase();

// Checking string contents
const startsWithLNBC = invoice.startsWith("lnbc");
const endsWithSomething = invoice.endsWith("xyz");
const includesText = invoice.includes("pp5");

// Extracting parts of strings
const first20Chars = invoice.substring(0, 20);
const fromPosition = invoice.substring(4); // Skip "lnbc"

// Cleaning user input
const cleanInput = invoice.trim(); // Remove whitespace
```

### Step 4: Introduction to Regular Expressions
```javascript
// Regular expressions (regex) are patterns for matching text
const invoicePattern = /^ln(bc|tb|bcrt)/; // Starts with lnbc, lntb, or lnbcrt

// Testing if a string matches a pattern
const isValidStart = invoicePattern.test("lnbc1500n1pjkx7fh...");
console.log(isValidStart); // true

// Extracting data with regex
const amountMatch = invoice.match(/lnbc(\d+)/);
if (amountMatch) {
    const amount = amountMatch[1]; // The captured group
}

// Common Bitcoin/Lightning patterns
const addressPattern = /^(bc1|[13])[a-zA-HJ-NP-Z0-9]{25,62}$/; // Bitcoin address
const nodeIdPattern = /^[0-9a-f]{66}$/; // Lightning node ID
```

### Step 5: Building the Invoice Parser
```javascript
function parseInvoice() {
    // Get and clean the input
    let invoice = document.getElementById("invoice-input").value;
    invoice = invoice.trim().toLowerCase();
    
    // Basic validation
    if (!invoice) {
        showError("Please enter a Lightning Network invoice");
        return;
    }
    
    // Check if it looks like a Lightning invoice
    if (!invoice.startsWith("lnbc") && !invoice.startsWith("lntb") && !invoice.startsWith("lnbcrt")) {
        showError("Invalid invoice format. Lightning invoices should start with 'lnbc', 'lntb', or 'lnbcrt'");
        return;
    }
    
    // Parse the invoice components
    try {
        const invoiceData = extractInvoiceData(invoice);
        displayInvoiceInfo(invoiceData);
    } catch (error) {
        showError("Error parsing invoice: " + error.message);
    }
}

function extractInvoiceData(invoice) {
    const data = {
        network: "unknown",
        amount: null,
        description: "Not specified",
        paymentHash: "Not available",
        timestamp: new Date(),
        expiry: 3600, // Default 1 hour
        shortInvoice: invoice.substring(0, 30) + "..."
    };
    
    // Determine network
    if (invoice.startsWith("lnbc")) {
        data.network = "Bitcoin Mainnet";
    } else if (invoice.startsWith("lntb")) {
        data.network = "Bitcoin Testnet";
    } else if (invoice.startsWith("lnbcrt")) {
        data.network = "Bitcoin Regtest";
    }
    
    // Extract amount (simplified - real BOLT-11 parsing is more complex)
    const amountMatch = invoice.match(/lnbc(\d+)([munp]?)/);
    if (amountMatch) {
        let amount = parseInt(amountMatch[1]);
        const unit = amountMatch[2];
        
        // Convert to satoshis based on unit
        switch (unit) {
            case 'm': // millisatoshi
                amount = Math.floor(amount / 1000);
                break;
            case 'u': // microsatoshi  
                amount = Math.floor(amount / 1000000);
                break;
            case 'n': // nanosatoshi
                amount = Math.floor(amount / 1000000000);
                break;
            case 'p': // picosatoshi
                amount = Math.floor(amount / 1000000000000);
                break;
            default:
                // No unit means millisatoshi
                amount = Math.floor(amount / 1000);
        }
        
        data.amount = amount;
    }
    
    return data;
}
```

### Step 6: Display Functions and Error Handling
```javascript
function displayInvoiceInfo(data) {
    const resultDiv = document.getElementById("result");
    
    resultDiv.innerHTML = `
        <div class="success">
            <h3>✅ Valid Lightning Network Invoice</h3>
            
            <div class="invoice-info">
                <div class="info-item">
                    <div class="info-label">Network</div>
                    <div class="info-value">${data.network}</div>
                </div>
                
                <div class="info-item">
                    <div class="info-label">Amount</div>
                    <div class="info-value amount-display">
                        ${data.amount ? data.amount.toLocaleString() + ' satoshis' : 'No amount specified'}
                    </div>
                </div>
                
                <div class="info-item">
                    <div class="info-label">Description</div>
                    <div class="info-value">${data.description}</div>
                </div>
                
                <div class="info-item">
                    <div class="info-label">Expires In</div>
                    <div class="info-value">${Math.floor(data.expiry / 60)} minutes</div>
                </div>
            </div>
            
            <div style="margin-top: 20px; padding: 15px; background: rgba(255,255,255,0.5); border-radius: 8px;">
                <div class="info-label">Invoice Preview</div>
                <div style="font-family: 'Courier New', monospace; font-size: 0.9em; word-break: break-all;">
                    ${data.shortInvoice}
                </div>
            </div>
            
            ${data.amount ? `
                <div style="margin-top: 15px; padding: 15px; background: #e6f3ff; border-radius: 8px;">
                    <strong>💰 Payment Amount Analysis:</strong><br>
                    • ${data.amount} satoshis<br>
                    • ${(data.amount / 100000000).toFixed(8)} BTC<br>
                    • ${data.amount * 1000} millisatoshis (msat)
                </div>
            ` : ''}
        </div>
    `;
}

function showError(message) {
    document.getElementById("result").innerHTML = `
        <div class="error">
            <h3>❌ Error</h3>
            <p>${message}</p>
            <div style="margin-top: 15px; font-size: 0.9em;">
                <strong>Common issues:</strong>
                <ul>
                    <li>Invoice doesn't start with lnbc, lntb, or lnbcrt</li>
                    <li>Invoice is incomplete or corrupted</li>
                    <li>Copy/paste error with extra characters</li>
                </ul>
            </div>
        </div>
    `;
}
```

### Step 7: Helper Functions and Polish
```javascript
// Sample invoice for testing
function loadSampleInvoice() {
    const sampleInvoice = "lnbc1500n1pjkx7fhpp5h2ykq9xy2z6k7k0w2e2lkf0q9x7x6k5h4g3f2d1s0a9z8y7x6w5v4u3t2r1q0p9o8n7m6l5k4j3h2g1f0e9d8c7b6a5z4y3x2w1v0u9t8s7r6q5p4o3n2m1l0k9j8h7g6f5e4d3c2b1a0z9y8x7w6v5u4t3s2r1q0p9o8n7m6l5k4j3h2g1f0e9d8c7b6a5z4y3x2w1v0u9t8s7r6q5p4o3n2m1l0k9j8h7g6f5e4d3c2b1a0";
    
    document.getElementById("invoice-input").value = sampleInvoice;
    parseInvoice();
}

function clearInput() {
    document.getElementById("invoice-input").value = "";
    document.getElementById("result").innerHTML = `
        <p style="text-align: center; color: #666;">
            Paste a Lightning Network invoice above to analyze it
        </p>
    `;
}

function generateRandomAmount() {
    const amounts = [1000, 5000, 10000, 21000, 50000, 100000, 500000];
    const randomAmount = amounts[Math.floor(Math.random() * amounts.length)];
    
    // Create a mock invoice with the amount
    const mockInvoice = `lnbc${randomAmount}n1pjkx7fhpp5h2ykq9xy2z6k7k0w2e2lkf0q9x7x6k5h4g3f2d1s0a9z8y7x6w5v4u3t2r1q0p9o8n7m6l5k4j3h2g1f0e9d8c7b6a5z4y3x2w1v0u9t8s7r6q5p4o3n2m1l0k9j8h7g6f5e4d3c2b1a0z9y8x7w6v5`;
    
    document.getElementById("invoice-input").value = mockInvoice;
    parseInvoice();
}
```

### Step 8: Complete Application
```html
<!DOCTYPE html>
<html>
<head>
    <title>Lightning Invoice Helper</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            overflow: hidden;
        }
        .header {
            background: linear-gradient(135deg, #ff9500, #ff7700);
            color: white;
            padding: 30px;
            text-align: center;
        }
        .content {
            padding: 40px;
        }
        .input-section {
            margin-bottom: 30px;
        }
        label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
            color: #333;
        }
        .invoice-input {
            width: 100%;
            padding: 15px;
            border: 2px solid #ddd;
            border-radius: 10px;
            font-size: 14px;
            font-family: 'Courier New', monospace;
            resize: vertical;
            min-height: 120px;
            box-sizing: border-box;
        }
        .invoice-input:focus {
            border-color: #667eea;
            outline: none;
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.3);
        }
        .parse-btn {
            width: 100%;
            padding: 18px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.2em;
            font-weight: bold;
            cursor: pointer;
            margin: 20px 0;
        }
        .parse-btn:hover {
            opacity: 0.9;
        }
        .result-section {
            margin-top: 30px;
            padding: 25px;
            background: #f8f9fa;
            border-radius: 12px;
            min-height: 100px;
        }
        .error {
            background: #fff5f5;
            border: 2px solid #fed7d7;
            color: #c53030;
            padding: 20px;
            border-radius: 10px;
        }
        .success {
            background: #f0fff4;
            border: 2px solid #9ae6b4;
            color: #2f855a;
        }
        .invoice-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 20px;
        }
        .info-item {
            background: white;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }
        .info-label {
            font-weight: bold;
            color: #666;
            font-size: 0.9em;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }
        .info-value {
            font-size: 1.2em;
            color: #333;
            word-break: break-all;
        }
        .amount-display {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff9500;
        }
        .tools-section {
            margin-top: 30px;
            padding: 25px;
            background: #f1f3f4;
            border-radius: 12px;
        }
        .tool-btn {
            background: #ff9500;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            margin: 5px;
            font-weight: bold;
        }
        .tool-btn:hover {
            background: #e08600;
        }
        .sample-section {
            margin-top: 20px;
            padding: 15px;
            background: #e6f3ff;
            border-radius: 8px;
            border-left: 4px solid #0066cc;
        }
        .validation-tips {
            margin-top: 20px;
            padding: 15px;
            background: #fff3cd;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>⚡ Lightning Invoice Helper</h1>
            <p>Parse and analyze Lightning Network invoices (BOLT-11)</p>
        </div>
        
        <div class="content">
            <div class="input-section">
                <label for="invoice-input">Lightning Network Invoice:</label>
                <textarea 
                    id="invoice-input" 
                    class="invoice-input" 
                    placeholder="Paste your Lightning Network invoice here (starts with lnbc, lntb, or lnbcrt)..."
                ></textarea>
            </div>
            
            <button class="parse-btn" onclick="parseInvoice()">
                🔍 Parse Invoice
            </button>
            
            <div id="result" class="result-section">
                <p style="text-align: center; color: #666;">
                    Paste a Lightning Network invoice above to analyze it
                </p>
            </div>
            
            <div class="tools-section">
                <h3>🛠️ Quick Tools</h3>
                <button class="tool-btn" onclick="loadSampleInvoice()">Load Sample Invoice</button>
                <button class="tool-btn" onclick="clearInput()">Clear Input</button>
                <button class="tool-btn" onclick="generateRandomAmount()">Generate Test Amount</button>
                <button class="tool-btn" onclick="validateInvoiceFormat()">Validate Format Only</button>
            </div>
            
            <div class="sample-section">
                <h4>💡 What is a Lightning Invoice?</h4>
                <p>Lightning Network invoices (BOLT-11) are payment requests that contain:</p>
                <ul>
                    <li><strong>Network Prefix:</strong> lnbc (mainnet), lntb (testnet), lnbcrt (regtest)</li>
                    <li><strong>Amount:</strong> Encoded with unit suffixes (m, u, n, p)</li>
                    <li><strong>Description:</strong> What the payment is for</li>
                    <li><strong>Payment Hash:</strong> Unique identifier for the payment</li>
                    <li><strong>Expiry:</strong> When the invoice expires (default 1 hour)</li>
                    <li><strong>Routing Hints:</strong> Information to help route the payment</li>
                </ul>
            </div>
            
            <div class="validation-tips">
                <h4>⚠️ Security Tips</h4>
                <ul>
                    <li>Always verify invoice details before paying</li>
                    <li>Check the amount matches what you expect</li>
                    <li>Ensure the invoice isn't expired</li>
                    <li>Only use invoices from trusted sources</li>
                    <li>Never reuse payment hashes</li>
                </ul>
            </div>
        </div>
    </div>
    
    <script>
        // Main parsing function
        function parseInvoice() {
            // Get and clean the input
            let invoice = document.getElementById("invoice-input").value;
            invoice = sanitizeInput(invoice);
            
            // Comprehensive validation
            const validation = validateInvoice(invoice);
            if (!validation.isValid) {
                showError(validation.error);
                return;
            }
            
            // Parse the invoice components
            try {
                const invoiceData = extractInvoiceData(invoice);
                displayInvoiceInfo(invoiceData);
            } catch (error) {
                showError("Parsing error: " + error.message);
            }
        }
        
        // Input sanitization
        function sanitizeInput(input) {
            return input
                .trim()                    // Remove whitespace
                .toLowerCase()             // Convert to lowercase
                .replace(/\s+/g, '')       // Remove all whitespace
                .replace(/[^a-z0-9]/g, ''); // Keep only alphanumeric
        }
        
        // Comprehensive validation
        function validateInvoice(invoice) {
            if (!invoice) {
                return { isValid: false, error: "Please enter a Lightning Network invoice" };
            }
            
            if (invoice.length < 50) {
                return { isValid: false, error: "Invoice appears too short to be valid" };
            }
            
            if (!invoice.match(/^ln(bc|tb|bcrt)/)) {
                return { 
                    isValid: false, 
                    error: "Invalid invoice format. Lightning invoices must start with 'lnbc' (mainnet), 'lntb' (testnet), or 'lnbcrt' (regtest)" 
                };
            }
            
            // Check for obviously invalid patterns
            if (invoice.includes('oooo') || invoice.includes('llll')) {
                return { isValid: false, error: "Invoice contains suspicious patterns that suggest it may be invalid" };
            }
            
            return { isValid: true };
        }
        
        // Extract invoice data (simplified BOLT-11 parsing)
        function extractInvoiceData(invoice) {
            const data = {
                network: determineNetwork(invoice),
                amount: extractAmount(invoice),
                description: "Payment request",
                paymentHash: "Available in full parser",
                timestamp: new Date(),
                expiry: 3600, // Default 1 hour
                shortInvoice: invoice.substring(0, 40) + "...",
                fullLength: invoice.length,
                networkType: invoice.substring(0, 4).toUpperCase()
            };
            
            return data;
        }
        
        function determineNetwork(invoice) {
            if (invoice.startsWith("lnbc")) return "Bitcoin Mainnet";
            if (invoice.startsWith("lntb")) return "Bitcoin Testnet";  
            if (invoice.startsWith("lnbcrt")) return "Bitcoin Regtest";
            return "Unknown Network";
        }
        
        function extractAmount(invoice) {
            // Match amount pattern: lnbc + digits + optional unit
            const amountMatch = invoice.match(/ln(?:bc|tb|bcrt)(\d+)([munp]?)/);
            if (!amountMatch) return null;
            
            let amount = parseInt(amountMatch[1]);
            const unit = amountMatch[2] || 'm'; // Default to millisatoshi
            
            // Convert to satoshis based on unit
            const conversions = {
                'm': 0.001,        // millisatoshi to satoshi
                'u': 0.000001,     // microsatoshi to satoshi  
                'n': 0.000000001,  // nanosatoshi to satoshi
                'p': 0.000000000001 // picosatoshi to satoshi
            };
            
            return Math.floor(amount * (conversions[unit] || 0.001));
        }
        
        // Display successful parse results
        function displayInvoiceInfo(data) {
            const resultDiv = document.getElementById("result");
            
            resultDiv.innerHTML = `
                <div class="success">
                    <h3>✅ Valid Lightning Network Invoice</h3>
                    
                    <div class="invoice-info">
                        <div class="info-item">
                            <div class="info-label">Network</div>
                            <div class="info-value">${data.network}</div>
                        </div>
                        
                        <div class="info-item">
                            <div class="info-label">Amount</div>
                            <div class="info-value amount-display">
                                ${data.amount ? data.amount.toLocaleString() + ' sats' : 'No amount specified'}
                            </div>
                        </div>
                        
                        <div class="info-item">
                            <div class="info-label">Invoice Type</div>
                            <div class="info-value">${data.networkType}</div>
                        </div>
                        
                        <div class="info-item">
                            <div class="info-label">Invoice Length</div>
                            <div class="info-value">${data.fullLength} characters</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 20px; padding: 15px; background: rgba(255,255,255,0.7); border-radius: 8px;">
                        <div class="info-label">Invoice Preview</div>
                        <div style="font-family: 'Courier New', monospace; font-size: 0.8em; word-break: break-all; color: #555;">
                            ${data.shortInvoice}
                        </div>
                    </div>
                    
                    ${data.amount ? `
                        <div style="margin-top: 15px; padding: 15px; background: #e8f5e8; border-radius: 8px; border-left: 4px solid #4caf50;">
                            <strong>💰 Payment Amount Breakdown:</strong><br>
                            • <strong>${data.amount.toLocaleString()} satoshis</strong><br>
                            • ${(data.amount / 100000000).toFixed(8)} BTC<br>
                            • ${(data.amount * 1000).toLocaleString()} millisatoshis (msat)<br>
                            • ~$${((data.amount / 100000000) * 50000).toFixed(2)} USD (est.)
                        </div>
                    ` : `
                        <div style="margin-top: 15px; padding: 15px; background: #fff3cd; border-radius: 8px; border-left: 4px solid #ffc107;">
                            <strong>💡 No Amount Specified</strong><br>
                            This is a "zero-amount" invoice. The sender can choose how much to pay.
                        </div>
                    `}
                </div>
            `;
        }
        
        // Show error messages with helpful context
        function showError(message) {
            document.getElementById("result").innerHTML = `
                <div class="error">
                    <h3>❌ Invalid Invoice</h3>
                    <p><strong>${message}</strong></p>
                    <div style="margin-top: 15px; font-size: 0.9em;">
                        <strong>Common issues:</strong>
                        <ul>
                            <li>Invoice doesn't start with lnbc, lntb, or lnbcrt</li>
                            <li>Invoice is incomplete or truncated</li>
                            <li>Copy/paste error introduced extra characters</li>
                            <li>Invoice has expired or is malformed</li>
                        </ul>
                        <strong>💡 Tip:</strong> Try the "Load Sample Invoice" button to see a working example.
                    </div>
                </div>
            `;
        }
        
        // Helper functions
        function loadSampleInvoice() {
            const sampleInvoice = "lnbc2500u1pvjluezpp5qqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqypqdq5xysxxatsyp3k7enxv4jsxqzpuaztrnwngzn3kdzw5hydlzf03qdgm2hdq27cqv3agm2awhz5se903vruatfhq77w3ls4evs3ch9zw97j25emudupq63nyw24cg27h2rspfj9srp";
            
            document.getElementById("invoice-input").value = sampleInvoice;
            parseInvoice();
        }
        
        function clearInput() {
            document.getElementById("invoice-input").value = "";
            document.getElementById("result").innerHTML = `
                <p style="text-align: center; color: #666;">
                    Paste a Lightning Network invoice above to analyze it
                </p>
            `;
        }
        
        function generateRandomAmount() {
            const amounts = [1000, 5000, 10000, 21000, 50000, 100000];
            const randomAmount = amounts[Math.floor(Math.random() * amounts.length)];
            
            const mockInvoice = `lnbc${randomAmount}u1pvjluezpp5qqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqypqdq5xysxxatsyp3k7enxv4jsxqzpuaztrnwngzn3kdzw5hydlzf03qdgm2hdq27cqv3agm2awhz5se903vruatfhq77w3ls4evs3ch9zw97j25emudupq63nyw24cg27h2rspfj9srp`;
            
            document.getElementById("invoice-input").value = mockInvoice;
            parseInvoice();
        }
        
        function validateInvoiceFormat() {
            let invoice = document.getElementById("invoice-input").value;
            invoice = sanitizeInput(invoice);
            
            const validation = validateInvoice(invoice);
            
            if (validation.isValid) {
                document.getElementById("result").innerHTML = `
                    <div class="success">
                        <h3>✅ Invoice Format Valid</h3>
                        <p>The invoice format appears correct. Click "Parse Invoice" for detailed analysis.</p>
                    </div>
                `;
            } else {
                showError(validation.error);
            }
        }
        
        // Initialize with helpful message
        window.onload = function() {
            document.getElementById("result").innerHTML = `
                <div style="text-align: center; padding: 20px;">
                    <h3>⚡ Lightning Invoice Analyzer</h3>
                    <p style="color: #666; margin: 10px 0;">
                        Paste a Lightning Network invoice above to get started, or try a sample invoice.
                    </p>
                    <div style="background: #f0f8ff; padding: 15px; border-radius: 8px; margin-top: 20px; text-align: left;">
                        <strong>What you'll learn:</strong>
                        <ul style="margin: 10px 0;">
                            <li>How to parse Lightning Network payment requests</li>
                            <li>Extract amounts, network info, and metadata</li>
                            <li>Validate invoice format and structure</li>
                            <li>Handle Bitcoin/Lightning data safely</li>
                        </ul>
                    </div>
                </div>
            `;
        };
    </script>
</body>
</html>
```

## Key Takeaways

### String Methods
- **trim()**: Remove whitespace from beginning and end
- **toLowerCase()/toUpperCase()**: Change case for consistency
- **startsWith()/endsWith()**: Check string prefixes and suffixes
- **substring()**: Extract portions of strings
- **replace()**: Find and replace text patterns

### Regular Expressions
- **Pattern matching**: Find specific text patterns in data
- **Validation**: Check if input matches expected format
- **Extraction**: Pull specific data from complex strings
- **Common patterns**: Bitcoin addresses, Lightning invoices, node IDs

### Input Sanitization
- **Always clean user input**: Remove unwanted characters
- **Validate early**: Check format before processing
- **Provide helpful errors**: Tell users exactly what's wrong
- **Handle edge cases**: Empty input, wrong format, corrupted data

### Error Handling
- **Graceful failures**: Never crash, always provide feedback
- **Descriptive messages**: Help users understand problems
- **Recovery options**: Suggest how to fix issues
- **User experience**: Make errors helpful, not frustrating

## Real-World Applications

These skills are essential for Bitcoin development:
- **Payment Processing**: Validating Bitcoin addresses and amounts
- **Invoice Generation**: Creating and parsing payment requests
- **Wallet Development**: Handling user input safely
- **Exchange Integration**: Processing trading data
- **Lightning Applications**: Managing channel data and routing
- **Security**: Preventing injection attacks and data corruption

## Challenge Yourself

1. **Full BOLT-11 Parser**: Implement complete Lightning invoice parsing
2. **QR Code Integration**: Generate QR codes for invoices
3. **Expiry Checking**: Add real-time expiry validation
4. **Multiple Format Support**: Handle Bitcoin addresses, Lightning addresses
5. **Batch Processing**: Parse multiple invoices at once
6. **Historical Data**: Save and compare invoice patterns

## Common Issues and Solutions

### Regex Not Matching
- Test patterns with simple cases first
- Use online regex testers to debug patterns
- Remember to escape special characters: `\.` `\(` `\[`
- Check for case sensitivity issues

### String Methods Failing
- Always check if string exists before calling methods
- Handle empty strings: `if (str && str.length > 0)`
- Remember that strings are immutable - methods return new strings

### Input Validation Problems
- Validate step by step: format, then content, then business rules
- Provide specific error messages for each type of problem
- Consider international characters and edge cases

## Next Steps

Excellent work! You've mastered string manipulation and data validation - crucial skills for handling Bitcoin and Lightning data. In Lesson 7, we'll add data persistence with localStorage to build a HODL portfolio tracker.

### What's Coming Next
- **Lesson 7**: Data persistence and localStorage
- **Real-world skills**: Saving user data between sessions
- **New concepts**: JSON serialization and client-side storage

## Resources for Going Deeper

- [MDN String Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [Regular Expressions Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [BOLT-11 Specification](https://github.com/lightning/bolts/blob/master/11-payment-encoding.md)

## Share Your Success!

🎉 **You built a Lightning invoice parser!** 

This demonstrates real Lightning Network development skills. You're working with the same data formats used in professional Lightning applications.

Don't forget to:
1. Test with different types of Lightning invoices
2. Try the validation and parsing features
3. Push your code to GitHub
4. Share your parser with the Lightning community

Ready to add data persistence and build a portfolio tracker? Let's HODL some data! 💎

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 