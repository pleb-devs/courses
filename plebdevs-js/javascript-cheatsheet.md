# PlebDevs JavaScript Cheatsheet
*Quick Reference for Bitcoin Development*

## Welcome, PlebDev! 🟠

This cheatsheet covers all the JavaScript concepts from the PlebDevs JavaScript Course. Use it as your go-to reference while building Bitcoin applications and projects.

**Perfect for:**
- Quick syntax lookups while coding
- Reviewing concepts from the course lessons
- Reference during Bitcoin project development
- Sharing with other aspiring Bitcoin developers

## Table of Contents

1. [Variable Declaration](#variable-declaration)
2. [Basic Data Types](#basic-data-types)
3. [DOM Manipulation](#dom-manipulation)
4. [Template Literals](#template-literals)
5. [Functions](#functions)
6. [Number Methods & Type Conversion](#number-methods--type-conversion)
7. [Event Handlers](#event-handlers)
8. [Arrays & Objects](#arrays--objects)
9. [Loops](#loops)
10. [Conditional Logic](#conditional-logic)
11. [Date Objects & Timing](#date-objects--timing)
12. [String Methods](#string-methods)
13. [Local Storage & JSON](#local-storage--json)
14. [External Libraries & CDNs](#external-libraries--cdns)
15. [Fetch API & Promises](#fetch-api--promises)
16. [Async/Await](#asyncawait)
17. [Error Handling](#error-handling)
18. [Event Delegation & CSS Classes](#event-delegation--css-classes)

---

## Variable Declaration

Use `let` for values that change, `const` for constants.

```javascript
// Bitcoin price that changes
let bitcoinPrice = 95000;

// Fixed conversion rate
const SATS_PER_BTC = 100000000;

// User's wallet balance
let walletBalance = 0.5;
```

**Key Points:**
- `let` - block-scoped, can be reassigned
- `const` - block-scoped, cannot be reassigned
- Avoid `var` in modern JavaScript

---

## Basic Data Types

```javascript
let blockHeight = 825000;              // Number
let walletAddress = "bc1q...";         // String
let isConfirmed = true;                // Boolean
let pendingTx = null;                  // Null
let mempoolSize;                       // Undefined (declared but not assigned)
```

**Bitcoin Context:**
- Numbers: prices, amounts, block heights, fees
- Strings: addresses, transaction IDs, invoice codes
- Booleans: confirmation status, validation results

---

## DOM Manipulation

Interact with HTML elements on your Bitcoin applications.

```javascript
// Selecting elements
const priceDisplay = document.getElementById("btc-price");
const walletCards = document.getElementsByClassName("wallet-card");
const firstWallet = document.querySelector(".wallet-card");
const allButtons = document.querySelectorAll("button");

// Modifying elements
priceDisplay.textContent = "$95,000";
priceDisplay.innerHTML = "<strong>$95,000</strong>";
priceDisplay.style.color = "#ff9500";

// Adding/removing CSS classes
priceDisplay.classList.add("price-up");
priceDisplay.classList.remove("price-down");
priceDisplay.classList.toggle("highlighted");

// Creating new elements
const newWallet = document.createElement("div");
newWallet.className = "wallet-card";
newWallet.innerHTML = "<h3>Hardware Wallet</h3><p>2.5 BTC</p>";
document.body.appendChild(newWallet);
```

---

## Template Literals

Create dynamic strings with Bitcoin data.

```javascript
const btcAmount = 2.5;
const usdPrice = 95000;
const totalValue = btcAmount * usdPrice;

// Template literal with backticks
const display = `You have ${btcAmount} BTC worth $${totalValue.toLocaleString()}`;

// Multi-line template
const walletInfo = `
    Wallet Balance: ${btcAmount} BTC
    Current Price: $${usdPrice}
    Total Value: $${totalValue.toLocaleString()}
`;
```

---

## Functions

Organize code into reusable blocks.

```javascript
// Function declaration
function convertBtcToSats(btcAmount) {
    return btcAmount * 100000000;
}

// Function expression
const calculateFee = function(priority) {
    if (priority === "high") return 50;
    if (priority === "medium") return 25;
    return 10;
};

// Arrow function (modern syntax)
const formatPrice = (price) => `$${price.toLocaleString()}`;

// Arrow function with multiple lines
const validateAddress = (address) => {
    if (!address) return false;
    if (address.length < 26) return false;
    return address.startsWith("bc1") || address.startsWith("1") || address.startsWith("3");
};

// Default parameters
function calculatePortfolio(btcAmount, currentPrice = 95000) {
    return btcAmount * currentPrice;
}

// Usage
console.log(convertBtcToSats(1));        // 100000000
console.log(calculateFee("high"));       // 50
console.log(formatPrice(95000));         // "$95,000"
console.log(calculatePortfolio(2.5));    // 237500
```

---

## Number Methods & Type Conversion

Handle Bitcoin amounts and prices properly.

```javascript
// Type conversion
const userInput = "0.00123456";
const btcAmount = parseFloat(userInput);        // 0.00123456
const blockHeight = parseInt("825000");         // 825000
const satsString = Number.parseFloat("2500000"); // 2500000 (static method)
const heightString = Number.parseInt("825000");  // 825000 (static method)

// Number validation
const amount = parseFloat("invalid");
console.log(isNaN(amount));                     // true (legacy)
console.log(Number.isNaN(amount));              // true (preferred)
console.log(Number.isFinite(95000));            // true
console.log(Number.isInteger(95000));           // true
console.log(Number.isInteger(95000.5));         // false
console.log(Number.isSafeInteger(95000));       // true

// Number formatting
const price = 95123.456789;
console.log(price.toFixed(2));                  // "95123.46"
console.log(price.toFixed(8));                  // "95123.45678900"
console.log(price.toPrecision(5));              // "95123"
console.log(price.toExponential(2));            // "9.51e+4"
console.log(price.toLocaleString());            // "95,123.457"
console.log(price.toString());                  // "95123.456789"
console.log(price.valueOf());                   // 95123.456789

// Bitcoin-specific formatting
const satoshis = 250000000;
const btc = (satoshis / 100000000).toFixed(8);  // "2.50000000"
const displaySats = satoshis.toLocaleString();  // "250,000,000"

// Locale-specific formatting
const price2 = 95123.45;
console.log(price2.toLocaleString('en-US'));    // "95,123.45"
console.log(price2.toLocaleString('de-DE'));    // "95.123,45"
console.log(price2.toLocaleString('ja-JP'));    // "95,123.45"

// Math operations for Bitcoin
const fee = Math.floor(250.7 * 25);             // 6267 sats
const rounded = Math.round(95123.456);          // 95123
const ceiling = Math.ceil(95123.1);             // 95124
const absolute = Math.abs(-1500);               // 1500
const maximum = Math.max(100, 200, 50);         // 200
const minimum = Math.min(100, 200, 50);         // 50
const random = Math.random() * 10000 + 90000;   // Random price 90000-100000

// Type checking
console.log(typeof 95000);                      // "number"
console.log(typeof "95000");                    // "string"
console.log(typeof NaN);                        // "number" (surprising!)
console.log(typeof Infinity);                   // "number"
```

---

## Event Handlers

Make your Bitcoin apps interactive.

```javascript
// Button click handler
function updatePrice() {
    const newPrice = Math.floor(Math.random() * 10000) + 90000;
    document.getElementById("price").textContent = `$${newPrice}`;
}

// Inline event handler (in HTML)
// <button onclick="updatePrice()">Refresh Price</button>

// Event listener (preferred method)
const refreshButton = document.getElementById("refresh-btn");
refreshButton.addEventListener("click", function() {
    updatePrice();
});

// Arrow function event listener
refreshButton.addEventListener("click", () => {
    console.log("Button clicked!");
});

// Event with parameters
function convertCurrency(fromCurrency, toCurrency) {
    console.log(`Converting from ${fromCurrency} to ${toCurrency}`);
}

// Passing parameters to event handlers
document.getElementById("convert-btn").addEventListener("click", () => {
    convertCurrency("BTC", "USD");
});
```

---

## Arrays & Objects

Store and organize Bitcoin data.

```javascript
// Arrays - ordered lists
const walletBalances = [2.5, 0.15, 0.003, 1.2];
const cryptoCurrencies = ["Bitcoin", "Lightning", "Ethereum"];

// Array methods
walletBalances.push(0.5);                    // Add to end
walletBalances.unshift(3.0);                 // Add to beginning
const lastBalance = walletBalances.pop();    // Remove from end
const firstBalance = walletBalances.shift(); // Remove from beginning

// Higher-order array methods
const totalBtc = walletBalances.reduce((sum, balance) => sum + balance, 0);
const formattedBalances = walletBalances.map(balance => balance.toFixed(8));
const largeBalances = walletBalances.filter(balance => balance > 1);

// Objects - key-value pairs
const wallet = {
    name: "Hardware Wallet",
    balance: 2.5,
    address: "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh",
    isHardware: true,
    
    // Object method
    getBalanceUsd: function(price) {
        return this.balance * price;
    }
};

// Object access
console.log(wallet.name);                    // "Hardware Wallet"
console.log(wallet["balance"]);              // 2.5
console.log(wallet.getBalanceUsd(95000));    // 237500

// Object destructuring
const { name, balance, address } = wallet;
console.log(name, balance);                  // "Hardware Wallet" 2.5

// Array of objects (common pattern)
const portfolio = [
    { name: "Hardware Wallet", balance: 2.5, type: "cold" },
    { name: "Mobile Wallet", balance: 0.15, type: "hot" },
    { name: "Lightning Wallet", balance: 0.003, type: "lightning" }
];
```

---

## Loops

Iterate through Bitcoin data.

```javascript
const wallets = ["Hardware", "Mobile", "Lightning"];
const balances = [2.5, 0.15, 0.003];

// for loop - when you know the count
for (let i = 0; i < wallets.length; i++) {
    console.log(`${wallets[i]}: ${balances[i]} BTC`);
}

// for...of loop - iterate through array values
for (let balance of balances) {
    console.log(`Balance: ${balance} BTC`);
}

// for...in loop - iterate through object keys
const wallet = { name: "Hardware", balance: 2.5, type: "cold" };
for (let key in wallet) {
    console.log(`${key}: ${wallet[key]}`);
}

// while loop - continues while condition is true
let blockHeight = 825000;
while (blockHeight < 825010) {
    console.log(`Block: ${blockHeight}`);
    blockHeight++;
}

// Array forEach method
balances.forEach((balance, index) => {
    console.log(`Wallet ${index + 1}: ${balance} BTC`);
});
```

---

## Conditional Logic

Make decisions in your Bitcoin applications.

```javascript
const fee = 15000; // satoshis

// if...else statements
if (fee > 10000) {
    console.log("⚠️ High fee warning!");
} else if (fee > 5000) {
    console.log("✅ Moderate fee");
} else {
    console.log("💰 Low fee");
}

// Ternary operator (shorthand)
const feeStatus = fee > 10000 ? "High" : "Normal";
const priorityClass = fee > 10000 ? "priority-high" : "priority-normal";

// Switch statement
const networkType = "mainnet";
switch (networkType) {
    case "mainnet":
        console.log("Using Bitcoin mainnet");
        break;
    case "testnet":
        console.log("Using testnet");
        break;
    case "regtest":
        console.log("Using regtest");
        break;
    default:
        console.log("Unknown network");
}

// Comparison operators
const currentPrice = 95000;
const targetPrice = 100000;

console.log(currentPrice > targetPrice);     // false
console.log(currentPrice >= 95000);          // true
console.log(currentPrice === 95000);         // true (strict equality)
console.log(currentPrice == "95000");        // true (with type conversion)
console.log(currentPrice !== targetPrice);   // true

// Logical operators
const isConfirmed = true;
const hasBalance = true;
const canSpend = isConfirmed && hasBalance;   // true
const needsAction = !isConfirmed || !hasBalance; // false
```

---

## Date Objects & Timing

Work with time in Bitcoin applications.

```javascript
// Creating dates
const now = new Date();
const specificDate = new Date("2025-01-15");
const blockTime = new Date(1642723200000); // From timestamp

// Formatting dates
console.log(now.toLocaleDateString());    // "1/15/2025"
console.log(now.toLocaleTimeString());    // "2:30:45 PM"
console.log(now.toISOString());           // "2025-01-15T19:30:45.123Z"

// setTimeout - execute after delay
function updateBlockHeight() {
    console.log("Block height updated!");
}
setTimeout(updateBlockHeight, 5000); // After 5 seconds

// setInterval - execute repeatedly
let blockHeight = 825000;
const blockTimer = setInterval(() => {
    blockHeight++;
    console.log(`New block: ${blockHeight}`);
    document.getElementById("height").textContent = blockHeight;
}, 600000); // Every 10 minutes (600,000ms)

// Clear interval
setTimeout(() => {
    clearInterval(blockTimer);
    console.log("Block timer stopped");
}, 3600000); // Stop after 1 hour

// Countdown timer example
let secondsLeft = 600; // 10 minutes
const countdown = setInterval(() => {
    secondsLeft--;
    const minutes = Math.floor(secondsLeft / 60);
    const seconds = secondsLeft % 60;
    const display = `${minutes}:${seconds.toString().padStart(2, '0')}`;
    document.getElementById("countdown").textContent = display;
    
    if (secondsLeft <= 0) {
        clearInterval(countdown);
        console.log("Next block expected!");
    }
}, 1000);
```

---

## String Methods

Process Bitcoin addresses, invoices, and text data.

```javascript
const invoice = "  lnbc250n1p3xnhl2pp5j685t...  ";
const address = "bc1qxy2kgdygjrsqtzq2n0yrf2493p83kkfjhx0wlh";

// String cleaning and validation
const cleanInvoice = invoice.trim().toLowerCase();
const isValidInvoice = cleanInvoice.startsWith("lnbc");
const addressType = address.startsWith("bc1") ? "bech32" : "legacy";

// String extraction
const shortAddress = address.substring(0, 10) + "...";
const invoiceAmount = invoice.match(/(\d+)/); // Basic regex

// String formatting
const btcAmount = "2.50000000";
const displayAmount = parseFloat(btcAmount).toString(); // "2.5"

// String methods
console.log(address.length);                    // 42
console.log(address.toUpperCase());            // "BC1QXY2..."
console.log(address.includes("qxy"));          // true
console.log(address.indexOf("qxy"));           // 4
console.log(address.slice(0, 4));             // "bc1q"
console.log(address.split("q"));              // ["bc1", "xy2kgdygjrsqt...]

// Input sanitization
function sanitizeInput(input) {
    return input.trim()
                .replace(/[<>]/g, '') // Remove dangerous characters
                .substring(0, 1000);  // Limit length
}

// Validation patterns
function isValidBtcAddress(address) {
    if (!address || typeof address !== 'string') return false;
    if (address.length < 26 || address.length > 62) return false;
    return /^[13][a-km-zA-HJ-NP-Z1-9]{25,34}$/.test(address) || // Legacy
           /^bc1[a-z0-9]{39,59}$/.test(address);                  // Bech32
}
```

---

## Local Storage & JSON

Persist Bitcoin data between sessions.

```javascript
// Saving data to localStorage
const portfolio = [
    { name: "Hardware Wallet", balance: 2.5, buyPrice: 45000 },
    { name: "Mobile Wallet", balance: 0.15, buyPrice: 92000 }
];

// Convert to JSON string and save
localStorage.setItem("btcPortfolio", JSON.stringify(portfolio));
localStorage.setItem("lastPrice", "95000");
localStorage.setItem("settings", JSON.stringify({
    currency: "USD",
    notifications: true,
    theme: "dark"
}));

// Loading data from localStorage
function loadPortfolio() {
    const saved = localStorage.getItem("btcPortfolio");
    return saved ? JSON.parse(saved) : [];
}

function getLastPrice() {
    return localStorage.getItem("lastPrice") || "0";
}

function loadSettings() {
    const saved = localStorage.getItem("settings");
    return saved ? JSON.parse(saved) : {
        currency: "USD",
        notifications: false,
        theme: "light"
    };
}

// Adding new data
function addToPortfolio(wallet) {
    const portfolio = loadPortfolio();
    portfolio.push(wallet);
    localStorage.setItem("btcPortfolio", JSON.stringify(portfolio));
}

// Removing data
function clearPortfolio() {
    localStorage.removeItem("btcPortfolio");
}

// Check if data exists
if (localStorage.getItem("btcPortfolio")) {
    console.log("Portfolio found!");
} else {
    console.log("No saved portfolio");
}

// Usage example
const currentPortfolio = loadPortfolio();
const lastKnownPrice = getLastPrice();
const userSettings = loadSettings();

console.log(`You have ${currentPortfolio.length} wallets`);
console.log(`Last price: $${lastKnownPrice}`);
```

---

## External Libraries & CDNs

Use external libraries like Chart.js for Bitcoin visualizations.

```html
<!-- Loading Chart.js from CDN -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```

```javascript
// Chart.js configuration for Bitcoin price chart
const priceData = {
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    datasets: [{
        label: 'Bitcoin Price (USD)',
        data: [45000, 52000, 47000, 62000, 58000, 65000],
        borderColor: '#ff9500',
        backgroundColor: '#ff950020',
        tension: 0.1,
        fill: true
    }]
};

const config = {
    type: 'line',
    data: priceData,
    options: {
        responsive: true,
        plugins: {
            title: {
                display: true,
                text: 'Bitcoin Price History'
            }
        },
        scales: {
            y: {
                beginAtZero: false,
                title: {
                    display: true,
                    text: 'Price (USD)'
                }
            }
        }
    }
};

// Create chart instance
const priceChart = new Chart(
    document.getElementById('priceChart'),
    config
);

// Update chart data
function updateChart(newPrice) {
    priceData.datasets[0].data.push(newPrice);
    priceData.labels.push('Jul');
    priceChart.update();
}

// Working with library methods
updateChart(67000);
```

---

## Fetch API & Promises

Get Bitcoin data from APIs.

```javascript
// Basic fetch request
fetch('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(response => response.json())
    .then(data => {
        const price = data.bpi.USD.rate_float;
        console.log(`Bitcoin price: $${price}`);
    })
    .catch(error => {
        console.error('Error fetching price:', error);
    });

// Creating your own Promise
function simulateBitcoinPrice() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const price = Math.floor(Math.random() * 10000) + 90000;
            if (price > 0) {
                resolve(price);
            } else {
                reject(new Error("Invalid price"));
            }
        }, 1000);
    });
}

// Using the Promise
simulateBitcoinPrice()
    .then(price => {
        console.log(`Simulated price: $${price}`);
    })
    .catch(error => {
        console.error("Price simulation failed:", error);
    });

// Promise chaining
fetch('/api/bitcoin-price')
    .then(response => {
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        return response.json();
    })
    .then(data => {
        return data.price * 0.00000001; // Convert to BTC
    })
    .then(btcPrice => {
        console.log(`Price in BTC: ${btcPrice}`);
    })
    .catch(error => {
        console.error('Fetch error:', error);
    });
```

---

## Async/Await

Cleaner syntax for handling asynchronous Bitcoin operations.

```javascript
// Basic async function
async function getBitcoinPrice() {
    try {
        const response = await fetch('https://api.coindesk.com/v1/bpi/currentprice.json');
        const data = await response.json();
        return data.bpi.USD.rate_float;
    } catch (error) {
        console.error('Error fetching price:', error);
        return 0;
    }
}

// Using async function
async function updatePriceDisplay() {
    const price = await getBitcoinPrice();
    const priceElement = document.getElementById("btc-price");
    priceElement.textContent = `$${price.toLocaleString()}`;
}

// Multiple async operations
async function loadDashboard() {
    try {
        // These run in parallel
        const [price, blockHeight, mempool] = await Promise.all([
            getBitcoinPrice(),
            getBlockHeight(),
            getMempoolInfo()
        ]);
        
        console.log(`Price: $${price}`);
        console.log(`Block Height: ${blockHeight}`);
        console.log(`Mempool: ${mempool.size} transactions`);
    } catch (error) {
        console.error('Dashboard load failed:', error);
    }
}

// Sequential async operations
async function processWalletData() {
    try {
        const wallets = await loadWalletList();
        
        for (let wallet of wallets) {
            const balance = await getWalletBalance(wallet.address);
            wallet.balance = balance;
            await saveWalletData(wallet);
        }
        
        console.log("All wallets processed");
    } catch (error) {
        console.error("Wallet processing failed:", error);
    }
}

// Simulation functions
async function getBlockHeight() {
    await new Promise(resolve => setTimeout(resolve, 500));
    return 825000 + Math.floor(Math.random() * 100);
}

async function getMempoolInfo() {
    await new Promise(resolve => setTimeout(resolve, 300));
    return { size: Math.floor(Math.random() * 50000) + 10000 };
}
```

---

## Error Handling

Handle errors gracefully in Bitcoin applications.

```javascript
// Try-catch with synchronous code
try {
    const btcAmount = parseFloat("invalid");
    if (isNaN(btcAmount)) {
        throw new Error("Invalid Bitcoin amount");
    }
    const satoshis = btcAmount * 100000000;
    console.log(`${satoshis} satoshis`);
} catch (error) {
    console.error("Conversion error:", error.message);
} finally {
    console.log("Conversion attempt completed");
}

// Try-catch with async/await
async function safePriceFetch() {
    try {
        const response = await fetch('/api/bitcoin-price');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const data = await response.json();
        return data.price;
    } catch (error) {
        if (error instanceof TypeError) {
            console.error("Network error:", error.message);
        } else {
            console.error("Price fetch error:", error.message);
        }
        
        // Return cached price or default
        return getCachedPrice() || 95000;
    }
}

// Custom error types
class BitcoinValidationError extends Error {
    constructor(message, code) {
        super(message);
        this.name = "BitcoinValidationError";
        this.code = code;
    }
}

function validateBitcoinAddress(address) {
    if (!address) {
        throw new BitcoinValidationError("Address is required", "MISSING_ADDRESS");
    }
    
    if (address.length < 26) {
        throw new BitcoinValidationError("Address too short", "INVALID_LENGTH");
    }
    
    return true;
}

// Using custom errors
try {
    validateBitcoinAddress("");
} catch (error) {
    if (error instanceof BitcoinValidationError) {
        console.error(`Validation failed: ${error.message} (${error.code})`);
    } else {
        console.error("Unexpected error:", error);
    }
}

// Error handling in event handlers
function handleWalletAction(action) {
    try {
        switch (action) {
            case 'send':
                // Send Bitcoin logic
                break;
            case 'receive':
                // Receive Bitcoin logic
                break;
            default:
                throw new Error(`Unknown action: ${action}`);
        }
    } catch (error) {
        showUserError(`Action failed: ${error.message}`);
    }
}

function showUserError(message) {
    const errorDiv = document.getElementById("error-message");
    errorDiv.textContent = message;
    errorDiv.style.display = "block";
    
    // Hide error after 5 seconds
    setTimeout(() => {
        errorDiv.style.display = "none";
    }, 5000);
}
```

---

## Event Delegation & CSS Classes

Efficiently handle events and manage styling in Bitcoin applications.

```javascript
// Event delegation - handle multiple elements with one listener
const walletContainer = document.getElementById("wallet-container");

walletContainer.addEventListener("click", function(event) {
    // Check what was clicked
    if (event.target.matches(".wallet-card")) {
        const walletName = event.target.dataset.wallet;
        console.log(`Wallet clicked: ${walletName}`);
    }
    
    if (event.target.matches(".send-btn")) {
        const walletId = event.target.closest(".wallet-card").dataset.id;
        handleSendBitcoin(walletId);
    }
    
    if (event.target.matches(".receive-btn")) {
        const walletId = event.target.closest(".wallet-card").dataset.id;
        handleReceiveBitcoin(walletId);
    }
});

// CSS class manipulation for Bitcoin app states
const priceElement = document.getElementById("bitcoin-price");

function updatePriceDisplay(newPrice, oldPrice) {
    // Remove existing price direction classes
    priceElement.classList.remove("price-up", "price-down", "price-unchanged");
    
    // Add appropriate class based on price movement
    if (newPrice > oldPrice) {
        priceElement.classList.add("price-up");
    } else if (newPrice < oldPrice) {
        priceElement.classList.add("price-down");
    } else {
        priceElement.classList.add("price-unchanged");
    }
    
    // Update the price text
    priceElement.textContent = `$${newPrice.toLocaleString()}`;
}

// Toggle wallet visibility
function toggleWalletDetails(walletElement) {
    walletElement.classList.toggle("expanded");
    
    const details = walletElement.querySelector(".wallet-details");
    if (walletElement.classList.contains("expanded")) {
        details.style.display = "block";
    } else {
        details.style.display = "none";
    }
}

// Working with multiple classes
function setTransactionStatus(txElement, status) {
    // Remove all status classes
    txElement.classList.remove("pending", "confirmed", "failed");
    
    // Add the current status
    txElement.classList.add(status);
    
    // Update status text
    const statusText = txElement.querySelector(".status-text");
    statusText.textContent = status.charAt(0).toUpperCase() + status.slice(1);
}

// Advanced CSS class operations
function highlightLargeBalances() {
    const walletCards = document.querySelectorAll(".wallet-card");
    
    walletCards.forEach(card => {
        const balance = parseFloat(card.dataset.balance);
        
        // Remove existing highlight classes
        card.classList.remove("highlight-small", "highlight-medium", "highlight-large");
        
        // Add appropriate highlight class
        if (balance >= 1.0) {
            card.classList.add("highlight-large");
        } else if (balance >= 0.1) {
            card.classList.add("highlight-medium");
        } else {
            card.classList.add("highlight-small");
        }
    });
}

// Dynamic styling based on Bitcoin data
function updateFeeWarning(feeAmount) {
    const feeDisplay = document.getElementById("fee-display");
    
    // Reset styles
    feeDisplay.style.color = "";
    feeDisplay.style.fontWeight = "";
    
    if (feeAmount > 50000) { // High fee (50k+ sats)
        feeDisplay.style.color = "#ff4444";
        feeDisplay.style.fontWeight = "bold";
        feeDisplay.classList.add("fee-warning");
    } else if (feeAmount > 20000) { // Medium fee
        feeDisplay.style.color = "#ff9500";
        feeDisplay.classList.remove("fee-warning");
    } else { // Low fee
        feeDisplay.style.color = "#4CAF50";
        feeDisplay.classList.remove("fee-warning");
    }
}

// Event delegation with data attributes
document.addEventListener("click", function(event) {
    // Handle any element with data-action attribute
    const action = event.target.dataset.action;
    if (!action) return;
    
    switch (action) {
        case "refresh-price":
            updateBitcoinPrice();
            break;
        case "convert-units":
            convertBtcToSats();
            break;
        case "toggle-wallet":
            const walletCard = event.target.closest(".wallet-card");
            toggleWalletDetails(walletCard);
            break;
    }
});
```

---

## Quick Reference

### Common Bitcoin Calculations
```javascript
// BTC to Satoshis
const sats = btcAmount * 100_000_000;

// Satoshis to BTC
const btc = satoshis / 100_000_000;

// Fee calculation (sats/vB * transaction size)
const totalFee = feeRate * txSize;

// Portfolio value
const totalValue = btcAmount * currentPrice;
```

### Essential DOM Operations
```javascript
// Get element
const element = document.getElementById("id");
const element = document.querySelector(".class");

// Update content
element.textContent = "New text";
element.innerHTML = "<b>HTML content</b>";

// Update styles
element.style.color = "#ff9500";
element.classList.add("active");
```

### Event Handling Pattern
```javascript
document.getElementById("button").addEventListener("click", function() {
    // Handle click
});
```

### Async Data Loading
```javascript
async function loadData() {
    try {
        const response = await fetch("/api/data");
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Error:", error);
        return null;
    }
}
```

---

## Course Integration

**Want to learn these concepts step-by-step?**

This cheatsheet corresponds directly to the PlebDevs JavaScript Course lessons:

- **Lessons 1-2**: Variables, DOM, Functions, Events
- **Lessons 3-4**: Arrays, Objects, Loops, Conditionals  
- **Lessons 5-6**: Timing, Dates, Strings, Validation
- **Lessons 7-8**: Storage, JSON, External Libraries
- **Lessons 9-10**: Async, Promises, Advanced DOM
- **Final Project**: Combines all concepts

Each concept here is taught through hands-on Bitcoin projects in the full course!

---

## Remember: Practice Makes Perfect

The best way to master these concepts:

1. **Build projects** - Don't just read, code!
2. **Use Bitcoin contexts** - Every example should relate to your goals
3. **Test everything** - Always verify your code works
4. **Start simple** - Master basics before advanced topics
5. **Ask questions** - The PlebDevs community is here to help

---

*This cheatsheet covers all the JavaScript concepts taught in the PlebDevs course. Keep it handy while building your Bitcoin applications!* 🟠

**Ready to dive deeper?** Check out the full course lessons and start building real Bitcoin projects today.

*Questions or need help? Reach out to the PlebDevs community at plebdevs.com* 