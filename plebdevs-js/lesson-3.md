[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-3.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-3.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-3.webv)

# Lesson 3: Bitcoin Wallet Balance Tracker

## Welcome to Data Structures, PlebDev!

Excellent progress! 🚀 You've mastered variables, functions, and user interaction. Now we're diving into one of the most important concepts in programming: working with multiple pieces of data using arrays and objects.

In this lesson, you'll build a multi-wallet balance tracker - exactly the kind of tool Bitcoin hodlers need to manage their various wallets. This introduces you to data structures that power every complex Bitcoin application.

## What You'll Learn

### JavaScript Concepts
- **Arrays**: Storing lists of related data
- **Objects**: Grouping related information together
- **Loops**: Processing multiple items efficiently  
- **Array Methods**: Modern techniques for working with data

### Bitcoin Development Skills
- Managing multiple wallet addresses
- Calculating portfolio totals
- Displaying financial data clearly
- Building scalable data structures

## Prerequisites
- Completed Lessons 1 & 2
- Understanding of variables and functions
- Ready to work with more complex data

## Project Overview: Multi-Wallet Balance Viewer

We're building a tracker that:
- Displays multiple Bitcoin wallets
- Shows individual balances
- Calculates total portfolio value
- Uses realistic wallet types (Hardware, Mobile, Lightning)
- Demonstrates professional data handling

## Key Concepts Explained

This lesson introduces powerful data structures that make complex applications possible:

### Objects: Your Data Organization System
Objects are containers that group related information together. Think of them as digital filing cabinets where each drawer has a label:

```javascript
// Basic object structure
const wallet = {
    name: "Hardware Wallet",        // Property: value
    balance: 2.5,                   // Number property
    type: "cold storage",           // String property
    isActive: true,                 // Boolean property
    transactions: [],               // Array property
    lastUsed: new Date()           // Date object property
};

// Accessing object properties (two ways)
console.log(wallet.name);          // Dot notation: "Hardware Wallet"
console.log(wallet["balance"]);    // Bracket notation: 2.5

// Adding new properties
wallet.address = "bc1q742d6ccq93c9cxh6f5nj...";
wallet["publicKey"] = "0279BE667EF9DCBBAC55A06295CE870B07029...";

// Modifying existing properties
wallet.balance = 3.0;
wallet.lastUsed = new Date();

// Complex nested objects
const bitcoinWallet = {
    owner: "Satoshi",
    created: "2009-01-03",
    balance: {
        btc: 2.5,
        sats: 250000000,
        usd: 237500
    },
    addresses: {
        receiving: "bc1q742d6ccq93c9cxh6f5nj...",
        change: "bc1q8c6fshw2dlwun7ekn9qwf37cu2rn755upcp6el"
    },
    security: {
        encrypted: true,
        backupExists: true,
        lastBackup: "2025-01-15"
    },
    // Methods (functions inside objects)
    updateBalance: function(newAmount) {
        this.balance.btc = newAmount;
        this.balance.sats = newAmount * 100000000;
        this.balance.usd = newAmount * 95000; // Assuming $95k price
    },
    getFormattedBalance: function() {
        return `${this.balance.btc.toFixed(8)} BTC (${this.balance.sats.toLocaleString()} sats)`;
    }
};

// Using object methods
bitcoinWallet.updateBalance(3.0);
console.log(bitcoinWallet.getFormattedBalance()); // "3.00000000 BTC (300,000,000 sats)"
```

**Real Bitcoin object patterns:**
```javascript
// Transaction object
const transaction = {
    txid: "a1b2c3d4e5f6789012345678901234567890123456789012345678901234567890",
    amount: 0.5,
    fee: 0.00025,
    confirmations: 6,
    timestamp: 1705123456789,
    inputs: [
        { address: "bc1q...", amount: 0.75 }
    ],
    outputs: [
        { address: "bc1q...", amount: 0.5 },
        { address: "bc1q...", amount: 0.24975 } // Change
    ],
    isConfirmed: function() {
        return this.confirmations >= 6;
    }
};

// Portfolio object
const portfolio = {
    totalBTC: 0,
    totalUSD: 0,
    wallets: [],
    
    addWallet: function(wallet) {
        this.wallets.push(wallet);
        this.recalculateTotal();
    },
    
    recalculateTotal: function() {
        this.totalBTC = this.wallets.reduce((sum, wallet) => sum + wallet.balance, 0);
        this.totalUSD = this.totalBTC * 95000; // Current price
    }
};
```

### Arrays: Ordered Lists of Data
Arrays store multiple items in sequence, like a numbered list:

```javascript
// Basic array creation
const prices = [95000, 96000, 94500, 97000];
const currencies = ["USD", "EUR", "GBP", "JPY"];
const mixed = [95000, "Bitcoin", true, null];

// Array of objects (very common in Bitcoin apps)
const wallets = [
    { 
        name: "Hardware Wallet", 
        balance: 2.5, 
        type: "hardware",
        brand: "Ledger"
    },
    { 
        name: "Mobile Wallet", 
        balance: 0.15, 
        type: "software",
        brand: "BlueWallet"
    },
    { 
        name: "Lightning Wallet", 
        balance: 0.03, 
        type: "lightning",
        brand: "Phoenix"
    }
];

// Accessing array items (zero-indexed)
console.log(prices[0]);         // 95000 (first item)
console.log(prices[3]);         // 97000 (fourth item)
console.log(wallets[1].name);   // "Mobile Wallet"

// Array properties
console.log(prices.length);     // 4 (number of items)
console.log(wallets.length);    // 3

// Complex nested arrays
const priceHistory = [
    { date: "2025-01-01", prices: [94000, 95000, 93500, 94800] },
    { date: "2025-01-02", prices: [94800, 96000, 94200, 95500] },
    { date: "2025-01-03", prices: [95500, 97000, 95000, 96200] }
];

// Accessing nested data
console.log(priceHistory[0].prices[1]); // 95000
```

### Loops: Processing Every Item
Loops let you perform operations on each item in an array:

```javascript
const wallets = [
    { name: "Hardware", balance: 2.5, type: "cold" },
    { name: "Mobile", balance: 0.3, type: "hot" },
    { name: "Lightning", balance: 0.05, type: "hot" }
];

// for...of loop (modern, clean, recommended)
for (let wallet of wallets) {
    console.log(`${wallet.name}: ${wallet.balance} BTC`);
    
    // You can use the full object
    if (wallet.type === "cold") {
        console.log("🔒 Cold storage wallet");
    }
}

// Traditional for loop (when you need the index)
for (let i = 0; i < wallets.length; i++) {
    console.log(`Wallet ${i + 1}: ${wallets[i].name}`);
    
    // Modify the original array
    wallets[i].index = i;
}

// while loop (for conditional iteration)
let totalBalance = 0;
let i = 0;
while (i < wallets.length && totalBalance < 1.0) {
    totalBalance += wallets[i].balance;
    i++;
}

// Building HTML with loops
function displayWallets(wallets) {
    let html = "<ul>";
    
    for (let wallet of wallets) {
        const satoshis = (wallet.balance * 100000000).toLocaleString();
        html += `
            <li>
                <strong>${wallet.name}</strong><br>
                Balance: ${wallet.balance} BTC (${satoshis} sats)<br>
                Type: ${wallet.type}
                ${wallet.type === 'cold' ? '🔒' : '📱'}
            </li>
        `;
    }
    
    html += "</ul>";
    return html;
}
```

### Array Methods: Powerful Built-in Tools
Arrays come with many helpful methods for data manipulation:

```javascript
const wallets = [
    { name: "Hardware", balance: 2.5, type: "cold" },
    { name: "Mobile", balance: 0.3, type: "hot" },
    { name: "Lightning", balance: 0.05, type: "hot" },
    { name: "Paper", balance: 1.2, type: "cold" }
];

// Adding/removing items
wallets.push({ name: "New Wallet", balance: 0.1, type: "hot" });     // Add to end
wallets.unshift({ name: "First", balance: 0.01, type: "hot" });      // Add to beginning
let removed = wallets.pop();                                          // Remove from end
let first = wallets.shift();                                         // Remove from beginning

// Finding items
let hardwareWallet = wallets.find(wallet => wallet.name === "Hardware");
let hotWallets = wallets.filter(wallet => wallet.type === "hot");
let walletIndex = wallets.findIndex(wallet => wallet.balance > 1.0);

// Transforming data
let balances = wallets.map(wallet => wallet.balance);
let formattedWallets = wallets.map(wallet => ({
    ...wallet,
    formattedBalance: `${wallet.balance.toFixed(8)} BTC`,
    satoshis: wallet.balance * 100000000
}));

// Calculations
let totalBalance = wallets.reduce((sum, wallet) => sum + wallet.balance, 0);
let avgBalance = totalBalance / wallets.length;
let hasLargeBalance = wallets.some(wallet => wallet.balance > 1.0);
let allPositive = wallets.every(wallet => wallet.balance > 0);

// Sorting
let sortedByBalance = [...wallets].sort((a, b) => b.balance - a.balance);
let sortedByName = [...wallets].sort((a, b) => a.name.localeCompare(b.name));

// Grouping by type
let walletsByType = wallets.reduce((groups, wallet) => {
    const type = wallet.type;
    if (!groups[type]) {
        groups[type] = [];
    }
    groups[type].push(wallet);
    return groups;
}, {});

// Real Bitcoin portfolio calculations
function calculatePortfolioStats(wallets, currentPrice = 95000) {
    const stats = {
        totalBTC: wallets.reduce((sum, w) => sum + w.balance, 0),
        totalUSD: 0,
        largestWallet: null,
        smallestWallet: null,
        coldStorageTotal: 0,
        hotWalletTotal: 0,
        walletCount: wallets.length
    };
    
    stats.totalUSD = stats.totalBTC * currentPrice;
    stats.largestWallet = wallets.reduce((max, w) => w.balance > max.balance ? w : max);
    stats.smallestWallet = wallets.reduce((min, w) => w.balance < min.balance ? w : min);
    stats.coldStorageTotal = wallets
        .filter(w => w.type === 'cold')
        .reduce((sum, w) => sum + w.balance, 0);
    stats.hotWalletTotal = wallets
        .filter(w => w.type === 'hot')
        .reduce((sum, w) => sum + w.balance, 0);
    
    return stats;
}
```

**Why these concepts are crucial for Bitcoin development:**
- **Objects** model real-world Bitcoin entities (wallets, transactions, blocks)
- **Arrays** handle lists of Bitcoin data (transaction history, multiple wallets, price data)
- **Loops** process Bitcoin data efficiently (calculating totals, generating displays)
- **Array methods** provide powerful tools for Bitcoin analytics and portfolio management

**Common Bitcoin development patterns:**
```javascript
// Portfolio management
const portfolio = {
    wallets: [],
    addWallet(wallet) { this.wallets.push(wallet); },
    getTotalBalance() { return this.wallets.reduce((sum, w) => sum + w.balance, 0); },
    getWalletsByType(type) { return this.wallets.filter(w => w.type === type); }
};

// Transaction processing
const transactions = [...];
const confirmedTxs = transactions.filter(tx => tx.confirmations >= 6);
const totalReceived = transactions
    .filter(tx => tx.type === 'receive')
    .reduce((sum, tx) => sum + tx.amount, 0);
```

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `bitcoin-wallet-tracker`
2. Open in Code Editor
3. Create `index.html`

### Step 2: HTML Structure
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Wallet Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 30px auto;
            padding: 20px;
            background: #f5f5f5;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        .wallet-list {
            margin: 30px 0;
        }
        .wallet-item {
            background: #f9f9f9;
            padding: 20px;
            margin: 15px 0;
            border-radius: 8px;
            border-left: 4px solid #ff9500;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .wallet-info h3 {
            margin: 0;
            color: #333;
        }
        .wallet-info p {
            margin: 5px 0 0 0;
            color: #666;
            font-size: 0.9em;
        }
        .wallet-balance {
            text-align: right;
        }
        .balance-btc {
            font-size: 1.3em;
            font-weight: bold;
            color: #ff9500;
        }
        .balance-sats {
            font-size: 0.9em;
            color: #666;
        }
        .total-section {
            background: linear-gradient(135deg, #ff9500, #ff7700);
            color: white;
            padding: 25px;
            border-radius: 10px;
            text-align: center;
            margin-top: 30px;
        }
        .total-btc {
            font-size: 2.5em;
            font-weight: bold;
            margin: 10px 0;
        }
        .add-wallet {
            background: #007bff;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
            margin-top: 20px;
        }
        .add-wallet:hover {
            background: #0056b3;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🟠 My Bitcoin Wallets</h1>
        <p>Track your Bitcoin across multiple wallets</p>
        
        <div id="wallet-list" class="wallet-list">
            <!-- Wallets will be displayed here -->
        </div>
        
        <div id="total-section" class="total-section">
            <h2>Total Portfolio</h2>
            <div id="total-btc" class="total-btc">0.00000000 BTC</div>
            <div id="total-sats">0 satoshis</div>
        </div>
        
        <button class="add-wallet" onclick="addRandomWallet()">
            + Add Random Wallet
        </button>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 3: Understanding Arrays
Arrays store multiple related items in order:

```javascript
// Array of wallet objects - this is our main data structure
const wallets = [
    { name: "Hardware Wallet", balance: 2.5 },
    { name: "Mobile Wallet", balance: 0.15 },
    { name: "Lightning Wallet", balance: 0.03 }
];

console.log("First wallet:", wallets[0]);
console.log("Total wallets:", wallets.length);
```

### Step 4: Understanding Objects
Objects group related information:

```javascript
// Each wallet is an object with properties
const wallet = {
    name: "Hardware Wallet",
    balance: 2.5,
    type: "cold",
    address: "bc1q..."
};

// Access object properties
console.log(wallet.name);      // "Hardware Wallet"
console.log(wallet.balance);   // 2.5
```

### Step 5: Looping Through Data
```javascript
const wallets = [
    { name: "Hardware Wallet", balance: 2.5, type: "Hardware" },
    { name: "Mobile Wallet", balance: 0.15, type: "Mobile" },
    { name: "Lightning Wallet", balance: 0.03, type: "Lightning" }
];

// for...of loop - modern and clean
for (let wallet of wallets) {
    console.log(`${wallet.name}: ${wallet.balance} BTC`);
}

// Traditional for loop - more control
for (let i = 0; i < wallets.length; i++) {
    console.log(`Wallet ${i + 1}: ${wallets[i].name}`);
}
```

### Step 6: Building the Display Function
```javascript
function displayWallets() {
    const walletList = document.getElementById("wallet-list");
    let html = "";
    
    // Loop through each wallet and build HTML
    for (let wallet of wallets) {
        const satoshis = Math.round(wallet.balance * 100000000);
        
        html += `
            <div class="wallet-item">
                <div class="wallet-info">
                    <h3>${wallet.name}</h3>
                    <p>Type: ${wallet.type}</p>
                </div>
                <div class="wallet-balance">
                    <div class="balance-btc">${wallet.balance.toFixed(8)} BTC</div>
                    <div class="balance-sats">${satoshis.toLocaleString()} sats</div>
                </div>
            </div>
        `;
    }
    
    walletList.innerHTML = html;
}
```

### Step 7: Calculating Totals
```javascript
function calculateTotal() {
    let total = 0;
    
    // Sum up all wallet balances
    for (let wallet of wallets) {
        total += wallet.balance;
    }
    
    return total;
}

function updateTotalDisplay() {
    const total = calculateTotal();
    const totalSats = Math.round(total * 100000000);
    
    document.getElementById("total-btc").textContent = 
        `${total.toFixed(8)} BTC`;
    document.getElementById("total-sats").textContent = 
        `${totalSats.toLocaleString()} satoshis`;
}
```

### Step 8: Complete Working Application
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Wallet Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 30px auto;
            padding: 20px;
            background: #f5f5f5;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        .wallet-list {
            margin: 30px 0;
        }
        .wallet-item {
            background: #f9f9f9;
            padding: 20px;
            margin: 15px 0;
            border-radius: 8px;
            border-left: 4px solid #ff9500;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .wallet-info h3 {
            margin: 0;
            color: #333;
        }
        .wallet-info p {
            margin: 5px 0 0 0;
            color: #666;
            font-size: 0.9em;
        }
        .wallet-balance {
            text-align: right;
        }
        .balance-btc {
            font-size: 1.3em;
            font-weight: bold;
            color: #ff9500;
        }
        .balance-sats {
            font-size: 0.9em;
            color: #666;
        }
        .total-section {
            background: linear-gradient(135deg, #ff9500, #ff7700);
            color: white;
            padding: 25px;
            border-radius: 10px;
            text-align: center;
            margin-top: 30px;
        }
        .total-btc {
            font-size: 2.5em;
            font-weight: bold;
            margin: 10px 0;
        }
        .portfolio-stats {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 15px;
            margin-top: 20px;
        }
        .stat-item {
            background: rgba(255,255,255,0.1);
            padding: 15px;
            border-radius: 8px;
            text-align: center;
        }
        .add-wallet {
            background: #007bff;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
            margin-top: 20px;
        }
        .add-wallet:hover {
            background: #0056b3;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🟠 My Bitcoin Wallets</h1>
        <p>Track your Bitcoin across multiple wallets</p>
        
        <div id="wallet-list" class="wallet-list">
            <!-- Wallets will be displayed here -->
        </div>
        
        <div id="total-section" class="total-section">
            <h2>Total Portfolio</h2>
            <div id="total-btc" class="total-btc">0.00000000 BTC</div>
            <div id="total-sats">0 satoshis</div>
            
            <div class="portfolio-stats">
                <div class="stat-item">
                    <div><strong id="wallet-count">0</strong></div>
                    <div>Wallets</div>
                </div>
                <div class="stat-item">
                    <div><strong id="largest-wallet">0</strong></div>
                    <div>Largest (BTC)</div>
                </div>
                <div class="stat-item">
                    <div><strong id="avg-balance">0</strong></div>
                    <div>Average (BTC)</div>
                </div>
            </div>
        </div>
        
        <button class="add-wallet" onclick="addRandomWallet()">
            + Add Random Wallet
        </button>
    </div>
    
    <script>
        // Array of wallet objects - our main data structure
        const wallets = [
            { 
                name: "Hardware Wallet", 
                balance: 2.5, 
                type: "Hardware",
                description: "Secure cold storage"
            },
            { 
                name: "Mobile Wallet", 
                balance: 0.15, 
                type: "Mobile",
                description: "Daily spending money"
            },
            { 
                name: "Lightning Wallet", 
                balance: 0.03, 
                type: "Lightning",
                description: "Instant payments"
            }
        ];
        
        // Display all wallets
        function displayWallets() {
            const walletList = document.getElementById("wallet-list");
            let html = "";
            
            // Loop through wallets and create HTML
            for (let wallet of wallets) {
                const satoshis = Math.round(wallet.balance * 100000000);
                
                html += `
                    <div class="wallet-item">
                        <div class="wallet-info">
                            <h3>${wallet.name}</h3>
                            <p>${wallet.description} • ${wallet.type}</p>
                        </div>
                        <div class="wallet-balance">
                            <div class="balance-btc">${wallet.balance.toFixed(8)} BTC</div>
                            <div class="balance-sats">${satoshis.toLocaleString()} sats</div>
                        </div>
                    </div>
                `;
            }
            
            walletList.innerHTML = html;
        }
        
        // Calculate portfolio statistics
        function calculateStats() {
            let total = 0;
            let largest = 0;
            
            for (let wallet of wallets) {
                total += wallet.balance;
                if (wallet.balance > largest) {
                    largest = wallet.balance;
                }
            }
            
            const average = wallets.length > 0 ? total / wallets.length : 0;
            
            return {
                total: total,
                largest: largest,
                average: average,
                count: wallets.length
            };
        }
        
        // Update the total display with statistics
        function updateTotalDisplay() {
            const stats = calculateStats();
            const totalSats = Math.round(stats.total * 100000000);
            
            // Update main totals
            document.getElementById("total-btc").textContent = 
                `${stats.total.toFixed(8)} BTC`;
            document.getElementById("total-sats").textContent = 
                `${totalSats.toLocaleString()} satoshis`;
            
            // Update statistics
            document.getElementById("wallet-count").textContent = stats.count;
            document.getElementById("largest-wallet").textContent = stats.largest.toFixed(4);
            document.getElementById("avg-balance").textContent = stats.average.toFixed(4);
        }
        
        // Add a random wallet for demonstration
        function addRandomWallet() {
            const walletTypes = [
                { name: "Exchange Wallet", type: "Exchange", description: "Trading funds" },
                { name: "Paper Wallet", type: "Paper", description: "Long-term storage" },
                { name: "Multisig Wallet", type: "Multisig", description: "Shared custody" },
                { name: "Web Wallet", type: "Web", description: "Quick access" }
            ];
            
            const randomType = walletTypes[Math.floor(Math.random() * walletTypes.length)];
            const randomBalance = parseFloat((Math.random() * 5).toFixed(8));
            
            const newWallet = {
                name: randomType.name,
                balance: randomBalance,
                type: randomType.type,
                description: randomType.description
            };
            
            // Add to our array
            wallets.push(newWallet);
            
            // Update the display
            displayWallets();
            updateTotalDisplay();
        }
        
        // Initialize the display when page loads
        displayWallets();
        updateTotalDisplay();
    </script>
</body>
</html>
```

## Key Takeaways

### Arrays
- **Storage**: Hold multiple related items in order
- **Access**: Use index numbers: `array[0]`, `array[1]`
- **Length**: Check how many items with `array.length`
- **Adding**: Use `array.push(item)` to add new items

### Objects  
- **Properties**: Store related data together
- **Access**: Use dot notation: `object.property`
- **Flexible**: Can contain numbers, strings, arrays, even other objects
- **Real-world**: Perfect for representing things like wallets, users, transactions

### Loops
- **for...of**: Modern, clean way to process each item
- **Traditional for**: More control, useful for indexes
- **Purpose**: Avoid writing the same code multiple times
- **Efficiency**: Process hundreds or thousands of items easily

### Array Methods (Preview)
- **forEach()**: Execute function for each item  
- **map()**: Transform each item into something new
- **filter()**: Keep only items that match criteria
- **reduce()**: Combine all items into single value

## Real-World Applications

This pattern appears everywhere in Bitcoin development:
- **Wallet Applications**: Managing multiple addresses
- **Exchange Interfaces**: Displaying trading pairs
- **Portfolio Trackers**: Aggregating investment data
- **Transaction History**: Showing payment records
- **Lightning Channels**: Managing channel states

## Challenge Yourself

1. **Add Wallet Types**: Create different icons for each wallet type
2. **Sort Wallets**: Add buttons to sort by balance or name
3. **Remove Wallets**: Add delete functionality
4. **Import/Export**: Save wallet data to localStorage
5. **Currency Conversion**: Show values in USD

## Common Issues and Solutions

### Total Not Updating
- Make sure to call `updateTotalDisplay()` after changing wallet data
- Check that your loop is accessing the right property: `wallet.balance`

### HTML Not Showing
- Verify element IDs match between HTML and JavaScript
- Check browser console for errors
- Ensure you're building the HTML string correctly

### Loops Not Working
- Remember: `for (let item of array)` for items
- Traditional: `for (let i = 0; i < array.length; i++)`
- Don't mix up array access: `array[i]` vs just `item` in for...of

## Next Steps

Outstanding work! You've mastered data structures - arrays and objects are the foundation of all complex applications. In Lesson 4, we'll add decision-making logic to build a transaction fee calculator.

### What's Coming Next
- **Lesson 4**: Conditional logic and comparisons
- **Real-world skills**: Making decisions in code
- **New concepts**: if/else statements and validation

## Resources for Going Deeper

- [MDN Arrays Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN Objects Basics](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
- [JavaScript Loops Tutorial](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)

## Share Your Success! 

🎉 **You built a professional portfolio tracker!** 

This demonstrates real Bitcoin development skills. You're working with the same data structures used in major Bitcoin applications.

Don't forget to:
1. Test adding and viewing different wallets
2. Push your code to GitHub
3. Share your tracker with the community

Ready to add smart decision-making to your code? Let's build a fee calculator! ⚡

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 