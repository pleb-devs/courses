[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/frontend-lesson-3.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/frontend-lesson-3.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/frontend-lesson-3.webm)

# Learn JavaScript: Building Your First Bitcoin Wallet

## Lesson Overview
Welcome to lesson 3 of the PlebDevs Frontend Course! In this lesson, we'll dive deep into JavaScript - the programming language of the web - and use it to build a fully functional Bitcoin Lightning wallet. We'll transform the static HTML/CSS wallet from lesson 2 into a dynamic, interactive application that connects to real Bitcoin services.

## Prerequisites
- Completion of Lesson 1 (HTML fundamentals) and Lesson 2 (CSS styling)
- Text editor (VS Code, Sublime Text, or similar)
- Web browser with developer tools
- Basic understanding of HTML elements and CSS styling
- The Pleb Wallet project from lesson 2

## Key Learning Objectives
- Master JavaScript fundamentals: data types, variables, functions, and control flow
- Understand asynchronous programming with async/await and Promises
- Learn DOM manipulation to dynamically update web pages
- Make API calls to external services and parse JSON responses
- Build a complete Bitcoin Lightning wallet with real functionality
- Implement real-time data updates and user interactions
- Connect to LNbits for actual Bitcoin transactions
- Apply JavaScript best practices and debugging techniques

## What is JavaScript?

### Definition
JavaScript is the **only** programming language that runs natively in web browsers. While other languages can run on servers or desktop applications, JavaScript is unique because it's built into every web browser, making it the universal language of web development.

### Why JavaScript Matters
- **Interactivity** - Make websites respond to user actions
- **Dynamic content** - Update pages without refreshing
- **API integration** - Connect to external services and databases
- **Real-time updates** - Display live data like Bitcoin prices
- **User experience** - Create smooth, app-like experiences

### JavaScript's Role in Web Development
```
HTML (Structure) + CSS (Style) + JavaScript (Behavior) = Complete Web Application
```

## Getting Started with JavaScript

### Browser Console
Before writing JavaScript files, let's explore the browser console - your JavaScript playground.

#### Opening Developer Tools
1. **Chrome/Brave/Edge**: Menu → More Tools → Developer Tools
2. **Firefox**: Menu → More Tools → Web Developer Tools
3. **Safari**: Develop → Show Web Inspector

#### Your First JavaScript
Navigate to `example.com` and open the console, then try:

```javascript
console.log(`Hello, World!`);
console.log(`This is my first time using JavaScript`);
console.log(`Wow, this is easy!`);
```

### How JavaScript Executes
JavaScript runs **line by line**, from top to bottom:

```javascript
console.log(`First line`);    // Runs first
console.log(`Second line`);   // Runs second  
console.log(`Third line`);    // Runs third
```

This sequential execution is crucial to understand as we build more complex applications.

## JavaScript Fundamentals

### Data Types

#### Numbers vs Strings
Understanding the difference between numbers and strings is critical:

```javascript
// Numbers (green in Firefox console)
console.log(Number(3));
console.log(3 + 2); // = 5 (arithmetic)

// Strings (black in Firefox console)  
console.log(String(3));
console.log(String(3) + 2); // = "32" (concatenation)
```

#### Key Differences
- **Numbers**: Can perform arithmetic operations (+, -, *, /)
- **Strings**: Text data, concatenation with +
- **Loose Typing**: JavaScript tries to guess data types, but be explicit for reliability

#### Best Practice
Always specify data types explicitly:

```javascript
// Good - explicit typing
let price = Number("16725.45");
let message = String("Bitcoin price: ");

// Avoid - relying on JavaScript's guessing
let price = "16725.45"; // Might cause unexpected behavior
``` 

### Data Structures

#### Arrays: Ordered Lists
Arrays store multiple values in a numbered list:

```javascript
// Creating an array
var fruits = [];
fruits.push(`lemons`);
fruits.push(`apples`);  
fruits.push(`oranges`);

console.log(fruits);        // Shows entire array
console.log(fruits[0]);     // "lemons" (first item)
console.log(fruits[1]);     // "apples" (second item)
```

**Key Points:**
- Arrays are indexed starting from 0
- Use `push()` to add items
- Access items with bracket notation: `array[index]`
- Perfect for lists of transactions, prices, etc.

#### Objects: Key-Value Pairs
Objects store data as key-value pairs:

```javascript
var fruits = {};
fruits[`yellow fruit`] = `lemon`;
fruits[`red fruit`] = `apple`;
fruits[`orange fruit`] = `orange`;

console.log(fruits);                    // Shows entire object
console.log(fruits[`red fruit`]);       // "apple"
```

**Key Points:**
- Objects use keys instead of numeric indexes
- Order is not guaranteed (browsers may reorder)
- Access values with bracket notation: `object[key]`
- Great for structured data like API responses

#### Arrays vs Objects
| Arrays | Objects |
|--------|---------|
| Ordered, indexed by numbers | Unordered, indexed by keys |
| `fruits[0]` | `fruits['red fruit']` |
| Order preserved | Order not guaranteed |
| Use for lists | Use for structured data |

### Variables: Short Names for Values

Variables let you store and reuse values throughout your code:

```javascript
// Creating and updating a variable
var message = `This is a long piece of text`;
message = message + ` and I can make it even longer`;
message = message + ` and even stretch it out to ridiculous lengths`;
message = message + ` but manage it easily through JavaScript`;

console.log(message);
console.log(message + ` and that's the way it is`);
```

#### Variable Rules
- Use `var`, `let`, or `const` to declare variables
- Variable names should be descriptive
- Use `=` to assign values
- Variables can be updated by reassigning

#### Modern Variable Declaration
```javascript
// const - cannot be reassigned (recommended for values that don't change)
const API_URL = `https://api.coinbase.com/v2/prices/BTC-USD/spot`;

// let - can be reassigned (recommended for values that change)
let bitcoinPrice = 0;
let balance = 1000;

// var - older style (avoid in modern code)
var oldStyle = `avoid this`;
```

### Functions: Reusable Code Blocks

Functions let you write code once and use it many times:

```javascript
// Basic function
function sayHello() {
    console.log(`Look ma, I'm using a function!`);
}

// Call the function
sayHello(); // Outputs: "Look ma, I'm using a function!"
```

#### Functions with Parameters
Parameters let you pass data into functions:

```javascript
function greetUser(name, age, location) {
    console.log(`Hello ${name}! You are ${age} years old and from ${location}.`);
}

// Call with different values
greetUser("Alice", 25, "New York");
greetUser("Bob", 30, "London");
```

#### Template Literals (Backticks)
Use backticks (\`) for string interpolation:

```javascript
// Good - template literals with ${}
function greetUser(name) {
    console.log(`Hello ${name}!`);
}

// Avoid - concatenation
function greetUser(name) {
    console.log("Hello " + name + "!");
}

// Why backticks are better:
// 1. No conflicts with apostrophes: "I'm a developer"
// 2. Easy variable insertion with ${}
// 3. Multi-line strings supported
```

### Control Flow: If/Else Statements

If statements are the core of programming logic:

```javascript
// Basic if/else
if (1 == 1) {
    console.log("I am running option 1");
} else {
    console.log("I am running option 2");
}
```

#### Comparison Operators
```javascript
// Equality (double equals for comparison)
if (price == 50000) { /* Bitcoin hit $50k! */ }

// AND operator (&&)
if (balance > 1000 && price < 20000) {
    console.log("Good time to buy!");
}

// OR operator (||)  
if (balance == 0 || price > 100000) {
    console.log("Either broke or Bitcoin mooned!");
}

// NOT operator (!)
if (!userLoggedIn) {
    console.log("Please log in");
}
```

#### Real-World Example
```javascript
function checkWalletStatus(balance, price) {
    if (balance > 1000 && price < 30000) {
        console.log("Great time to stack sats!");
    } else if (balance == 0) {
        console.log("Time to buy some Bitcoin!");
    } else {
        console.log("HODL strong!");
    }
}
``` 

## Asynchronous Programming and APIs

### Getting Data from the Internet

Modern web applications need to fetch data from external services. Here's our versatile `getData` function:

```javascript
function getData(url, apikey, content_type) {
    return new Promise(function(resolve, reject) {
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function() {
            if (this.readyState == 4 && (this.status >= 200 && this.status < 300)) {
                resolve(xhttp.responseText);
            }
        }
        xhttp.open(`GET`, url, true);
        if (apikey) {
            xhttp.setRequestHeader(`X-Api-Key`, apikey);
        }
        if (content_type) {
            xhttp.setRequestHeader(`Content-Type`, content_type);
        }
        xhttp.send();
    });
}
```

### Understanding Promises

When you request data from the internet, JavaScript returns a **Promise** - a container that will eventually hold your data:

```javascript
// This returns a Promise immediately
let dataPromise = getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
console.log(dataPromise); // Shows: Promise {<pending>}

// The Promise will resolve when data arrives
// Click to see: Promise {<fulfilled>: "...data..."}
```

### Async/Await: Making Promises Easy

Instead of dealing with complex Promise syntax, use `async/await`:

```javascript
// Without await (confusing)
function getBitcoinPrice() {
    var data = getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
    console.log(data); // Promise object, not the actual data!
}

// With await (clear and simple)
async function getBitcoinPrice() {
    var data = await getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
    console.log(data); // Actual data!
}
```

#### Why JavaScript Doesn't Wait by Default

JavaScript is designed for speed. It fires off requests and immediately continues to the next line:

```javascript
function demonstrateAsyncBehavior() {
    console.log("First: Starting request");
    var data = getData("https://api.example.com/data");
    console.log("Second: This runs immediately");
    console.log("Third: Data isn't ready yet:", data);
}
```

Output:
```
First: Starting request
Second: This runs immediately  
Third: Data isn't ready yet: Promise {<pending>}
```

#### Async/Await Fixes This

```javascript
async function demonstrateAsyncBehavior() {
    console.log("First: Starting request");
    var data = await getData("https://api.example.com/data");
    console.log("Second: Data is ready:", data);
    console.log("Third: This runs after data arrives");
}
```

### Working with JSON Data

#### What is JSON?
JSON (JavaScript Object Notation) is how computers exchange data on the web. It looks like JavaScript objects:

```json
{
    "data": {
        "base": "BTC",
        "currency": "USD", 
        "amount": "16725.45"
    }
}
```

#### Parsing JSON Responses

Raw API responses are strings. Convert them to JavaScript objects with `JSON.parse()`:

```javascript
async function getBitcoinPrice() {
    // Get raw string data
    var rawData = await getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
    console.log(typeof rawData); // "string"
    
    // Parse into JavaScript object
    var jsonData = JSON.parse(rawData);
    console.log(typeof jsonData); // "object"
    
    // Access nested data
    var price = jsonData.data.amount;
    console.log(`Bitcoin price: $${price}`);
}
```

#### Working with Arrays of Objects

Many APIs return arrays of objects. Here's how to work with them:

```javascript
async function getComments() {
    var data = await getData(`https://jsonplaceholder.typicode.com/posts/1/comments`);
    var comments = JSON.parse(data);
    
    console.log(comments);           // Array of comment objects
    console.log(comments[0]);        // First comment object
    console.log(comments[0].email);  // Email from first comment
    
    // Loop through all comments
    comments.forEach(function(comment) {
        console.log(`${comment.name}: ${comment.body}`);
    });
}
```

### Best Practices for Async Functions

#### Always Use Async for Network Calls
```javascript
// Good - async function
async function fetchData() {
    const result = await getData("https://api.example.com/data");
    return result;
}

// Avoid - mixing sync and async
function fetchData() {
    return getData("https://api.example.com/data"); // Returns Promise, not data
}
```

#### Error Handling
```javascript
async function safeFetchData() {
    try {
        const data = await getData("https://api.example.com/data");
        return JSON.parse(data);
    } catch (error) {
        console.error("Failed to fetch data:", error);
        return null;
    }
}
```

## DOM Manipulation: Updating Web Pages

### What is the DOM?
The DOM (Document Object Model) represents your HTML as a JavaScript object. Every HTML element becomes a JavaScript object you can modify.

### Query Selectors: Finding Elements

First, set up easy-to-use query functions:

```javascript
// Shorthand for document.querySelector and document.querySelectorAll
var $ = document.querySelector.bind(document);
var $$ = document.querySelectorAll.bind(document);
```

Now you can easily find and modify elements:

```javascript
// Find single elements
var header = $(`h1`);                    // First h1 element
var balanceCard = $(`.balance-card`);     // First element with class "balance-card"
var priceDisplay = $(`#price-display`);  // Element with ID "price-display"

// Find multiple elements  
var allCards = $$(`.balance-card`);       // All elements with class "balance-card"
var allParagraphs = $$(`p`);             // All paragraph elements
```

### Updating Content

#### Changing Text Content
```javascript
// Update the Bitcoin price display
$(`.balance-card p`).innerHTML = `$16,725.45`;

// Update multiple elements
$$(`.balance-card p`)[0].innerHTML = `100 sats`;  // First balance card
$$(`.balance-card p`)[1].innerHTML = `$16,725`;   // Second balance card
```

#### Adding New Content
```javascript
// Add a new transaction to the list
var transactionList = $(`.row-item`);
transactionList.innerHTML += `
    <p class="transaction">Received payment</p>
    <p class="transaction-amount">+50 sats</p>
`;
```

### Practical Example: Live Price Updates

```javascript
async function updateBitcoinPrice() {
    try {
        // Fetch current price
        var data = await getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
        var json = JSON.parse(data);
        var price = Number(json.data.amount);
        
        // Format with commas
        var formattedPrice = price.toLocaleString();
        
        // Update the display
        $$(`.balance-card p`)[1].innerHTML = `$${formattedPrice}`;
        
        console.log(`Updated Bitcoin price: $${formattedPrice}`);
    } catch (error) {
        console.error(`Failed to update price:`, error);
    }
}

// Update price immediately and then every 10 seconds
updateBitcoinPrice();
setInterval(updateBitcoinPrice, 10000);
``` 

## Building the Pleb Wallet Project

### Project Overview
We'll transform the static wallet from lesson 2 into a fully functional Bitcoin Lightning wallet that:
- Displays real-time Bitcoin prices from Coinbase
- Shows actual wallet balance from LNbits
- Lists real transactions
- Allows sending and receiving Bitcoin payments
- Updates automatically every 10 seconds

### Step 1: Project Setup

#### Download the CSS Wallet
Clone Austin's wallet project from lesson 2:

```bash
git clone https://github.com/AustinKelsay/learn-css
cd learn-css
```

#### Open in Browser and Editor
1. Open `index.html` in your web browser
2. Open the project folder in your text editor
3. You should see the static wallet with fake data

### Step 2: Adding JavaScript to HTML

JavaScript goes inside `<script>` tags. Add this before the closing `</body>` tag:

```html
<footer>
    <p>Made by plebs, for plebs.</p>
</footer>

<!-- Add JavaScript here -->
<script>
    console.log(`test`);
</script>
</body>
</html>
```

**Test it works:**
1. Save the file
2. Refresh your browser
3. Open Developer Tools → Console
4. You should see "test" logged

### Step 3: Set Up DOM Manipulation

Replace the test script with our query selectors:

```html
<script>
    // Easy DOM manipulation
    var $ = document.querySelector.bind(document);
    var $$ = document.querySelectorAll.bind(document);
    
    // Test updating the price
    $$('.balance-card p')[1].innerHTML = 'Testing...';
</script>
```

You should see the price change from "$19,364" to "Testing..."

### Step 4: Add the getData Function

Add our HTTP request function:

```html
<script>
    function getData(url, apikey, content_type) {
        return new Promise(function(resolve, reject) {
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && (this.status >= 200 && this.status < 300)) {
                    resolve(xhttp.responseText);
                }
            }
            xhttp.open(`GET`, url, true);
            if (apikey) {
                xhttp.setRequestHeader(`X-Api-Key`, apikey);
            }
            if (content_type) {
                xhttp.setRequestHeader(`Content-Type`, content_type);
            }
            xhttp.send();
        });
    }
</script>
```

### Step 5: Real Bitcoin Prices

Create a function to fetch Bitcoin prices from Coinbase:

```html
<script>
    async function getBitcoinPrice() {
        var data = await getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
        var json = JSON.parse(data);
        var price = json.data.amount;
        return price;
    }
</script>
```

### Step 6: Update the Display

Create the main app function that updates our wallet:

```html
<script>
    var $ = document.querySelector.bind(document);
    var $$ = document.querySelectorAll.bind(document);
    
    async function app() {
        // Get Bitcoin price
        var price = await getBitcoinPrice();
        var formattedPrice = Number(price).toLocaleString();
        
        // Update the display
        $$('.balance-card p')[1].innerHTML = `$${formattedPrice}`;
        
        // Update again in 10 seconds
        setTimeout(function() {
            app();
        }, 10000);
    }
    
    // Start the app
    app();
</script>
```

**Important:** Put this script **after** the getBitcoinPrice function, or move all scripts to the bottom of the page.

### Step 7: Setting Up LNbits Wallet

#### Create an LNbits Wallet
1. Go to [demo.lnbits.com](https://demo.lnbits.com) (updated from legend.lnbits.com)
2. Click "Create new wallet"
3. Give it a name like "Pleb Wallet"
4. Save your wallet - you now have a Bitcoin Lightning wallet!

#### Get Your API Keys
In your LNbits wallet:
1. Click on "API docs"
2. Copy your **Admin key** (the longer one)
3. This key lets your JavaScript app access your wallet

#### Understanding the API
LNbits provides several endpoints:
- `/api/v1/wallet` - Get wallet balance
- `/api/v1/payments` - Get transaction list
- We'll use GET requests to read data

### Step 8: Add LNbits Integration

#### Wallet Balance Function
```html
<script>
    async function getLnbitsBalance() {
        var apiKey = 'YOUR_ADMIN_KEY_HERE'; // Replace with your actual key
        var data = await getData(
            'https://demo.lnbits.com/api/v1/wallet', 
            apiKey
        );
        var json = JSON.parse(data);
        var balance = Number(json.balance) / 1000; // Convert millisats to sats
        return balance;
    }
</script>
```

#### Transactions Function
```html
<script>
    async function getLnbitsTransactions() {
        var apiKey = 'YOUR_ADMIN_KEY_HERE'; // Replace with your actual key
        var data = await getData(
            'https://demo.lnbits.com/api/v1/payments', 
            apiKey, 
            'application/json'
        );
        var json = JSON.parse(data);
        return json;
    }
</script>
```

### Step 9: Complete Wallet App

Update your main app function to handle everything:

```html
<script>
    async function app() {
        try {
            // Update Bitcoin price
            var price = await getBitcoinPrice();
            var formattedPrice = Number(price).toLocaleString();
            $$('.balance-card p')[1].innerHTML = `$${formattedPrice}`;
            
            // Update wallet balance
            var balance = await getLnbitsBalance();
            $$('.balance-card p')[0].innerHTML = `${balance} sats`;
            
            // Update transactions
            var transactions = await getLnbitsTransactions();
            updateTransactionsList(transactions);
            
        } catch (error) {
            console.error('App update failed:', error);
        }
        
        // Update again in 10 seconds
        setTimeout(function() {
            app();
        }, 10000);
    }
    
    function updateTransactionsList(transactions) {
        var transactionContainer = $('.row-item');
        
        // Reset transactions list but keep header
        transactionContainer.innerHTML = '<h3>Transactions</h3>';
        
        transactions.forEach(function(tx) {
            var amount = Math.floor(Number(tx.amount) / 1000); // Convert to sats
            
            // Only show completed transactions
            if (!tx.pending) {
                if (amount > 0) {
                    // Incoming payment
                    transactionContainer.innerHTML += `
                        <p class="transaction" data-checking-id="${tx.checking_id}">
                            Received with ${tx.bolt11.substring(0, 25)}...
                        </p>
                        <p class="transaction-amount">+${amount} sats</p>
                    `;
                } else if (amount < 0 && tx.preimage !== "0000000000000000000000000000000000000000000000000000000000000000") {
                    // Outgoing payment (only if actually paid)
                    transactionContainer.innerHTML += `
                        <p class="transaction" data-checking-id="${tx.checking_id}">
                            Sent with ${tx.bolt11.substring(0, 25)}...
                        </p>
                        <p class="transaction-amount">${amount} sats</p>
                    `;
                }
            }
        });
    }
    
    // Start the app
    app();
</script>
```

### Step 10: Add Send/Receive Functionality

#### Create Input Forms
Add these divs after your buttons in the HTML:

```html
<div class="buttons">
    <button>Send</button>
    <button>Receive</button>
</div>

<!-- Add these new divs -->
<div style="background-color: white; padding: 20px; display: none;" align="center" class="paste_invoice">
    <p style="font-family: Helvetica, sans-serif; font-size: 1.25em;">Paste an invoice</p>
    <p><input class="invoice_to_pay" style="font-size: 1.15em;"></p>
    <p><button type="button" onclick="submitInvoiceToPay($('.invoice_to_pay').value)" style="font-size: 1.15em;">Submit</button></p>
</div>

<div style="background-color: white; padding: 20px; display: none;" align="center" class="create_invoice">
    <p style="font-family: Helvetica, sans-serif; font-size: 1.25em;">Enter an amount</p>
    <p><input class="amount_of_new_invoice" style="font-size: 1.15em;"></p>
    <p><button type="button" onclick="getInvoice($('.amount_of_new_invoice').value)" style="font-size: 1.15em;">Submit</button></p>
</div>
```

#### Button Click Handlers
```html
<script>
    // Send button functionality
    $$('button')[0].onclick = function() {
        var pasteDiv = $('.paste_invoice');
        if (pasteDiv.style.display !== 'block') {
            pasteDiv.style.display = 'block';
        } else {
            pasteDiv.style.display = 'none';
        }
        $('.invoice_to_pay').value = '';
    }
    
    // Receive button functionality
    $$('button')[1].onclick = function() {
        var createDiv = $('.create_invoice');
        if (createDiv.style.display !== 'block') {
            createDiv.style.display = 'block';
        } else {
            createDiv.style.display = 'none';
        }
        $('.amount_of_new_invoice').value = '';
    }
</script>
```

#### POST Function for Sending Data
```html
<script>
    function postJson(url, apikey, content_type, json) {
        return new Promise(function(resolve, reject) {
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && (this.status >= 200 && this.status < 300)) {
                    resolve(xhttp.responseText);
                }
            }
            xhttp.open('POST', url, true);
            if (apikey) {
                xhttp.setRequestHeader('X-Api-Key', apikey);
            }
            if (content_type) {
                xhttp.setRequestHeader('Content-Type', content_type);
            }
            xhttp.send(json);
        });
    }
</script>
```

#### Invoice Creation Function
```html
<script>
    async function getInvoice(amount) {
        var apiKey = 'YOUR_ADMIN_KEY_HERE'; // Replace with your key
        
        var requestData = {
            out: false,
            amount: amount,
            memo: "LNBits"
        };
        
        var response = await postJson(
            'https://demo.lnbits.com/api/v1/payments',
            apiKey,
            'application/json',
            JSON.stringify(requestData)
        );
        
        var responseData = JSON.parse(response);
        $('.create_invoice').innerHTML += responseData.payment_request;
    }
</script>
```

#### Payment Function
```html
<script>
    async function submitInvoiceToPay(invoice) {
        if (!confirm(`Are you sure you want to pay this invoice? ${invoice}`)) {
            return;
        }
        
        var apiKey = 'YOUR_ADMIN_KEY_HERE'; // Replace with your key
        
        var requestData = {
            out: true,
            bolt11: invoice
        };
        
        var response = await postJson(
            'https://demo.lnbits.com/api/v1/payments',
            apiKey,
            'application/json',
            JSON.stringify(requestData)
        );
        
        var responseData = JSON.parse(response);
        $('.paste_invoice').innerHTML += JSON.stringify(responseData);
    }
</script>
```

### Step 11: Testing Your Wallet

#### Test Receiving
1. Click "Receive"
2. Enter an amount (like 10)
3. Click "Submit"
4. You'll get a Lightning invoice
5. Pay it with a Lightning wallet app
6. Watch your balance update!

#### Test Sending
1. Create an invoice in another Lightning wallet
2. Click "Send" in your web wallet
3. Paste the invoice
4. Click "Submit"
5. Confirm the payment
6. Watch the transaction appear!

### Understanding Lightning Network Basics

#### Millisatoshis vs Satoshis
- Lightning Network uses **millisatoshis** (1/1000 of a satoshi)
- We convert to satoshis for display: `amount / 1000`
- This allows for more precise micropayments

#### Preimages and Payment Proofs
- Lightning payments use cryptographic proofs called **preimages**
- A preimage of all zeros means the payment failed
- Non-zero preimages prove the payment was completed
- This is how we determine if outgoing payments succeeded

#### Invoice Format
Lightning invoices (bolt11) contain:
- Payment amount
- Destination
- Payment hash
- Expiry time
- Description

### Common Issues and Solutions

#### CORS Errors
If you get CORS errors, LNbits might be blocking requests:
- Use the demo server: demo.lnbits.com
- Some browsers are more restrictive than others
- Try Firefox or Chrome

#### API Key Issues
- Make sure you're using the **Admin** key, not the Invoice key
- Copy the entire key without extra spaces
- Replace 'YOUR_ADMIN_KEY_HERE' with your actual key

#### Function Order
JavaScript functions must be defined before they're called:
```html
<!-- Define first -->
<script>
    function getData() { /* ... */ }
</script>

<!-- Use second -->
<script>
    async function app() {
        var data = await getData(); // This works
    }
</script>
``` 

## Advanced JavaScript Techniques

### Array Methods for Processing Data

Lightning wallets often work with arrays of transactions. Here are essential array methods:

```javascript
// Filter transactions by type
var incomingTxs = transactions.filter(function(tx) {
    return Number(tx.amount) > 0;
});

var outgoingTxs = transactions.filter(function(tx) {
    return Number(tx.amount) < 0;
});

// Transform data with map
var transactionAmounts = transactions.map(function(tx) {
    return Math.floor(Number(tx.amount) / 1000); // Convert to sats
});

// Sum all transaction amounts
var totalAmount = transactionAmounts.reduce(function(sum, amount) {
    return sum + amount;
}, 0);

// Find specific transactions
var largeTx = transactions.find(function(tx) {
    return Math.abs(Number(tx.amount)) > 100000; // Over 100 sats
});
```

### Error Handling Best Practices

Always handle potential errors when working with APIs:

```javascript
async function safeGetBitcoinPrice() {
    try {
        var data = await getData(`https://api.coinbase.com/v2/prices/BTC-USD/spot`);
        var json = JSON.parse(data);
        
        if (json && json.data && json.data.amount) {
            return Number(json.data.amount);
        } else {
            throw new Error('Invalid price data received');
        }
    } catch (error) {
        console.error('Failed to get Bitcoin price:', error);
        return 0; // Default fallback value
    }
}
```

### Performance Optimization

#### Debouncing API Calls
Avoid overwhelming APIs with too many requests:

```javascript
var updateTimeout;

function scheduleUpdate() {
    // Cancel previous update if still pending
    if (updateTimeout) {
        clearTimeout(updateTimeout);
    }
    
    // Schedule new update
    updateTimeout = setTimeout(function() {
        app();
    }, 10000);
}
```

#### Efficient DOM Updates
Batch DOM changes to improve performance:

```javascript
function updateTransactionsList(transactions) {
    var html = '<h3>Transactions</h3>';
    
    // Build HTML string first
    transactions.forEach(function(tx) {
        if (!tx.pending) {
            var amount = Math.floor(Number(tx.amount) / 1000);
            html += `<p class="transaction">${tx.description}</p>`;
            html += `<p class="transaction-amount">${amount} sats</p>`;
        }
    });
    
    // Single DOM update
    $('.row-item').innerHTML = html;
}
```

## Hands-on Exercises

### Exercise 1: Enhanced Price Display
Extend the Bitcoin price functionality:

1. **Add Price Change Indicator**
   ```javascript
   var previousPrice = 0;
   
   async function updatePriceWithIndicator() {
       var currentPrice = await getBitcoinPrice();
       var priceElement = $$('.balance-card p')[1];
       
       if (currentPrice > previousPrice) {
           priceElement.style.color = 'green';
           priceElement.innerHTML = `$${currentPrice.toLocaleString()} ↗`;
       } else if (currentPrice < previousPrice) {
           priceElement.style.color = 'red';
           priceElement.innerHTML = `$${currentPrice.toLocaleString()} ↘`;
       }
       
       previousPrice = currentPrice;
   }
   ```

2. **Add Multiple Currencies**
   - Fetch EUR and GBP prices
   - Display all three currencies
   - Let users switch between them

3. **Price History Graph**
   - Store last 10 price updates
   - Display a simple ASCII graph
   - Use local storage to persist data

### Exercise 2: Transaction Analytics
Add analytics to your wallet:

1. **Transaction Summary**
   ```javascript
   function calculateTransactionStats(transactions) {
       var stats = {
           totalReceived: 0,
           totalSent: 0,
           transactionCount: 0,
           averageAmount: 0
       };
       
       transactions.forEach(function(tx) {
           if (!tx.pending) {
               var amount = Math.floor(Number(tx.amount) / 1000);
               if (amount > 0) {
                   stats.totalReceived += amount;
               } else {
                   stats.totalSent += Math.abs(amount);
               }
               stats.transactionCount++;
           }
       });
       
       stats.averageAmount = stats.transactionCount > 0 ? 
           (stats.totalReceived + stats.totalSent) / stats.transactionCount : 0;
       
       return stats;
   }
   ```

2. **Transaction Filtering**
   - Add buttons to filter by sent/received
   - Add date range filters
   - Search transactions by amount

3. **Export Functionality**
   - Export transactions as CSV
   - Generate transaction reports
   - Create backup of wallet data

### Exercise 3: User Experience Improvements

1. **Loading States**
   ```javascript
   function showLoading(message) {
       $('.loading-indicator').innerHTML = message || 'Loading...';
       $('.loading-indicator').style.display = 'block';
   }
   
   function hideLoading() {
       $('.loading-indicator').style.display = 'none';
   }
   ```

2. **Notifications System**
   - Show success/error messages
   - Auto-hide notifications after 3 seconds
   - Different styles for different message types

3. **Responsive Improvements**
   - Make forms work better on mobile
   - Add touch-friendly buttons
   - Optimize for different screen sizes

### Exercise 4: Security Enhancements

1. **Input Validation**
   ```javascript
   function validateAmount(amount) {
       var num = Number(amount);
       if (isNaN(num) || num <= 0) {
           throw new Error('Amount must be a positive number');
       }
       if (num > 1000000) {
           throw new Error('Amount too large');
       }
       return num;
   }
   ```

2. **API Key Management**
   - Store API keys securely
   - Add key validation
   - Handle expired keys gracefully

3. **Rate Limiting**
   - Limit API calls per minute
   - Show user when rate limited
   - Queue requests during high traffic

## Learning Resources

### Essential JavaScript References
- **[MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)** - Comprehensive official documentation
- **[W3Schools JavaScript Tutorial](https://www.w3schools.com/js/)** - Beginner-friendly tutorials with examples
- **[JavaScript.info](https://javascript.info/)** - Modern JavaScript tutorial
- **[Can I Use](https://caniuse.com/)** - Check browser compatibility

### API and Async Programming
- **[Promises and Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)** - Deep dive into asynchronous JavaScript
- **[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)** - Modern alternative to XMLHttpRequest
- **[JSON Working Group](https://www.json.org/)** - Official JSON specification

### Bitcoin and Lightning Development
- **[LNbits Documentation](https://lnbits.com/)** - Complete LNbits API reference
- **[Lightning Network Basics](https://github.com/lightningnetwork/lnd/blob/master/docs/INSTALL.md)** - Understanding Lightning Network
- **[Bitcoin Developer Guide](https://developer.bitcoin.org/)** - Official Bitcoin development resources
- **[Lightning Address](https://lightningaddress.com/)** - Human-readable Lightning addresses

### DOM Manipulation and Web APIs
- **[DOM Manipulation Guide](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)** - Complete DOM reference
- **[Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)** - Local and session storage
- **[Progressive Web Apps](https://web.dev/progressive-web-apps/)** - Making web apps feel native

### Advanced JavaScript
- **[Array Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** - Complete array method reference
- **[JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)** - Organizing code into modules
- **[Error Handling](https://javascript.info/try-catch)** - Comprehensive error handling guide

## Debugging and Troubleshooting

### Browser Developer Tools

#### Console Tab
- **View errors and warnings** - Red errors, yellow warnings
- **Test code snippets** - Run JavaScript directly in console
- **Monitor network requests** - See all API calls
- **Debug variables** - Type variable names to see their values

#### Network Tab
- **Monitor API calls** - See all requests to external services
- **Check response data** - Verify API responses are correct
- **Identify slow requests** - Find performance bottlenecks
- **Debug CORS issues** - See cross-origin request problems

#### Elements Tab
- **Inspect HTML structure** - See generated DOM
- **Modify CSS live** - Test styling changes
- **View element properties** - Check classes, IDs, attributes

### Common JavaScript Errors

#### ReferenceError: function is not defined
```javascript
// Problem: Function called before definition
app(); // Error: app is not defined

function app() {
    console.log("Hello");
}

// Solution: Define functions first, call them after
function app() {
    console.log("Hello");
}
app(); // Works!
```

#### TypeError: Cannot read property of undefined
```javascript
// Problem: Accessing property on undefined object
var json = JSON.parse(data);
console.log(json.data.amount); // Error if json.data is undefined

// Solution: Check if objects exist
if (json && json.data && json.data.amount) {
    console.log(json.data.amount);
}
```

#### SyntaxError: Unexpected token
```javascript
// Problem: Missing quotes, brackets, or semicolons
var message = Hello World; // Error: missing quotes

// Solution: Check syntax carefully
var message = "Hello World"; // Correct
```

### API-Related Issues

#### CORS (Cross-Origin Resource Sharing) Errors
```
Access to XMLHttpRequest at 'https://api.example.com' from origin 'file://' has been blocked by CORS policy
```

**Solutions:**
- Use APIs that support CORS (like Coinbase)
- Use LNbits demo server instead of local instances
- Serve your HTML from a web server instead of opening files directly

#### API Key Issues
```
Error 401: Unauthorized
```

**Solutions:**
- Verify you're using the Admin key, not the Invoice key
- Check for extra spaces when copying keys
- Ensure key hasn't expired or been revoked

#### Rate Limiting
```
Error 429: Too Many Requests
```

**Solutions:**
- Increase interval between requests
- Implement exponential backoff
- Cache responses when possible

### Performance Issues

#### Slow API Responses
- Check Network tab for slow requests
- Implement loading indicators
- Add timeout handling
- Cache frequently requested data

#### Memory Leaks
```javascript
// Problem: Creating intervals without clearing them
setInterval(function() {
    updatePrice();
}, 1000);

// Solution: Store reference and clear when needed
var priceInterval = setInterval(function() {
    updatePrice();
}, 1000);

// Clear when page unloads
window.addEventListener('beforeunload', function() {
    clearInterval(priceInterval);
});
```

## Best Practices Summary

### Code Organization
1. **Functions first, calls second** - Define all functions before calling them
2. **One function, one purpose** - Keep functions focused and small
3. **Use descriptive names** - `getBitcoinPrice()` not `getPrice()`
4. **Comment complex logic** - Explain why, not what

### Error Handling
1. **Always use try/catch with async functions**
2. **Provide fallback values** - Don't let errors break the app
3. **Log errors for debugging** - Use `console.error()`
4. **Show user-friendly error messages**

### Performance
1. **Minimize DOM manipulation** - Batch updates when possible
2. **Cache API responses** - Don't fetch same data repeatedly
3. **Use appropriate intervals** - 10 seconds for prices, not 1 second
4. **Clean up resources** - Clear intervals and timeouts

### Security
1. **Validate all user input** - Check amounts, formats, lengths
2. **Use HTTPS APIs only** - Never send sensitive data over HTTP
3. **Keep API keys secure** - Don't expose in client-side code in production
4. **Handle expired sessions** - Gracefully handle auth failures

## Next Steps

### Immediate Actions
1. **Complete the full wallet** with send/receive functionality
2. **Test with real Lightning payments** using small amounts
3. **Add error handling** to all API calls
4. **Implement loading states** for better UX

### Prepare for Advanced Topics
In future lessons, you'll learn:
- **React** - Building more complex UIs with components
- **State management** - Managing application data efficiently
- **Modern JavaScript** - ES6+ features and modules
- **Build tools** - Webpack, npm, and development workflows
- **Testing** - Writing tests for your JavaScript code

### Building Your Portfolio
- **Enhance the wallet** with additional features
- **Create new Bitcoin tools** - Price trackers, converters, etc.
- **Contribute to open source** - Help improve LNbits or other projects
- **Share your work** - Deploy to GitHub Pages or Vercel

## Key Takeaways

1. **JavaScript is essential** - The only language that runs in browsers
2. **Async/await simplifies promises** - Use it for all API calls
3. **DOM manipulation enables interactivity** - Update pages without refreshing
4. **APIs connect your app to the world** - Fetch real data from services
5. **Error handling is crucial** - Always expect and handle failures
6. **Practice builds understanding** - Build projects to solidify knowledge
7. **Bitcoin development is accessible** - You can build Lightning apps today
8. **User experience matters** - Loading states and error messages improve apps

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
            <button>Send</button>
            <button>Receive</button>
        </div>
        
        <div style="background-color: white; padding: 20px; display: none;" align="center" class="paste_invoice">
            <p style="font-family: Helvetica, sans-serif; font-size: 1.25em;">Paste an invoice</p>
            <p><input class="invoice_to_pay" style="font-size: 1.15em;"></p>
            <p><button type="button" onclick="submitInvoiceToPay($('.invoice_to_pay').value)" style="font-size: 1.15em;">Submit</button></p>
        </div>
        
        <div style="background-color: white; padding: 20px; display: none;" align="center" class="create_invoice">
            <p style="font-family: Helvetica, sans-serif; font-size: 1.25em;">Enter an amount</p>
            <p><input class="amount_of_new_invoice" style="font-size: 1.15em;"></p>
            <p><button type="button" onclick="getInvoice($('.amount_of_new_invoice').value)" style="font-size: 1.15em;">Submit</button></p>
        </div>
        
        <div class="row">
            <div class="balance-card">
                <h2>Balance</h2>
                <p>0 sats</p>
            </div>
            <div class="balance-card">
                <h2>Price</h2>
                <p>Loading...</p>
            </div>
        </div>
        
        <div class="row">
            <div class="row-item">
                <h3>Transactions</h3>
            </div>
            <div class="row-item">
                <img src="./BTCUSD.png" alt="Bitcoin Price Chart" />
            </div>
        </div>
    </main>
    
    <footer>
        <p>Made by plebs, for plebs.</p>
    </footer>

    <!-- JavaScript -->
    <script>
        // HTTP request functions
        function getData(url, apikey, content_type) {
            return new Promise(function(resolve, reject) {
                var xhttp = new XMLHttpRequest();
                xhttp.onreadystatechange = function() {
                    if (this.readyState == 4 && (this.status >= 200 && this.status < 300)) {
                        resolve(xhttp.responseText);
                    }
                }
                xhttp.open("GET", url, true);
                if (apikey) {
                    xhttp.setRequestHeader("X-Api-Key", apikey);
                }
                if (content_type) {
                    xhttp.setRequestHeader("Content-Type", content_type);
                }
                xhttp.send();
            });
        }

        function postJson(url, apikey, content_type, json) {
            return new Promise(function(resolve, reject) {
                var xhttp = new XMLHttpRequest();
                xhttp.onreadystatechange = function() {
                    if (this.readyState == 4 && (this.status >= 200 && this.status < 300)) {
                        resolve(xhttp.responseText);
                    }
                }
                xhttp.open("POST", url, true);
                if (apikey) {
                    xhttp.setRequestHeader("X-Api-Key", apikey);
                }
                if (content_type) {
                    xhttp.setRequestHeader("Content-Type", content_type);
                }
                xhttp.send(json);
            });
        }
    </script>

    <script>
        // API functions
        async function getBitcoinPrice() {
            var data = await getData("https://api.coinbase.com/v2/prices/BTC-USD/spot");
            var json = JSON.parse(data);
            var price = json.data.amount;
            return price;
        }

        async function getLnbitsBalance() {
            var apiKey = "YOUR_ADMIN_KEY_HERE"; // Replace with your actual key
            var data = await getData("https://demo.lnbits.com/api/v1/wallet", apiKey);
            var json = JSON.parse(data);
            var balance = Number(json.balance) / 1000;
            return balance;
        }

        async function getLnbitsTransactions() {
            var apiKey = "YOUR_ADMIN_KEY_HERE"; // Replace with your actual key
            var data = await getData("https://demo.lnbits.com/api/v1/payments", apiKey, "application/json");
            var json = JSON.parse(data);
            return json;
        }
    </script>

    <script>
        // DOM manipulation and main app
        var $ = document.querySelector.bind(document);
        var $$ = document.querySelectorAll.bind(document);

        async function app() {
            try {
                // Update balance
                var balance = await getLnbitsBalance();
                $$('.balance-card p')[0].innerHTML = balance + ' sats';

                // Update price
                var price = await getBitcoinPrice();
                var formattedPrice = Number(price).toLocaleString();
                $$('.balance-card p')[1].innerHTML = '$' + formattedPrice;

                // Update transactions
                var transactions = await getLnbitsTransactions();
                $('.row-item').innerHTML = '<h3>Transactions</h3>';
                
                transactions.forEach(function(tx) {
                    var amount = Math.floor(Number(tx.amount) / 1000);
                    
                    if (amount > 0 && !tx.pending) {
                        $('.row-item').innerHTML += `
                            <p class="transaction" data-checking-id="${tx.checking_id}">
                                Received with ${tx.bolt11.substring(0, 25)}...
                            </p>
                            <p class="transaction-amount">+${amount} sats</p>
                        `;
                    }
                    
                    if (amount < 0 && tx.preimage !== "0000000000000000000000000000000000000000000000000000000000000000") {
                        $('.row-item').innerHTML += `
                            <p class="transaction" data-checking-id="${tx.checking_id}">
                                Sent with ${tx.bolt11.substring(0, 25)}...
                            </p>
                            <p class="transaction-amount">${amount} sats</p>
                        `;
                    }
                });

            } catch (error) {
                console.error('App update failed:', error);
            }

            setTimeout(function() { app(); }, 10000);
        }

        // Button handlers
        $$('button')[0].onclick = function() {
            var pasteDiv = $('.paste_invoice');
            pasteDiv.style.display = pasteDiv.style.display !== 'block' ? 'block' : 'none';
            $('.invoice_to_pay').value = '';
        }

        $$('button')[1].onclick = function() {
            var createDiv = $('.create_invoice');
            createDiv.style.display = createDiv.style.display !== 'block' ? 'block' : 'none';
            $('.amount_of_new_invoice').value = '';
        }

        // Payment functions
        async function submitInvoiceToPay(invoice) {
            if (!confirm(`Are you sure you want to pay this invoice? ${invoice}`)) return;
            
            var apiKey = "YOUR_ADMIN_KEY_HERE";
            var json = { out: true, bolt11: invoice };
            
            var response = await postJson(
                "https://demo.lnbits.com/api/v1/payments",
                apiKey,
                "application/json",
                JSON.stringify(json)
            );
            
            $('.paste_invoice').innerHTML += JSON.stringify(JSON.parse(response));
        }

        async function getInvoice(amount) {
            var apiKey = "YOUR_ADMIN_KEY_HERE";
            var json = { out: false, amount: amount, memo: "LNBits" };
            
            var response = await postJson(
                "https://demo.lnbits.com/api/v1/payments",
                apiKey,
                "application/json",
                JSON.stringify(json)
            );
            
            var responseData = JSON.parse(response);
            $('.create_invoice').innerHTML += responseData.payment_request;
        }

        // Start the app
        app();
    </script>
</body>
</html>
```

Remember to replace `"YOUR_ADMIN_KEY_HERE"` with your actual LNbits admin key!

Congratulations! You've built your first Bitcoin Lightning wallet with JavaScript. You now understand the fundamentals of web development and can create interactive Bitcoin applications. Keep building, keep learning, and welcome to the Bitcoin developer community! ⚡️🧡

---

## What's Next?

In the next lesson, we'll dive into **React** - a powerful library that makes building complex user interfaces much easier. We'll refactor our wallet using modern development practices and add advanced features like:

- Component-based architecture
- State management
- Modern JavaScript (ES6+)
- Professional development workflow
- Deployment to production

The journey from here only gets more exciting! 🚀 