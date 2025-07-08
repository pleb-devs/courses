[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/frontend-lesson-5.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-5.mp4)

# Learn React: Building the Pleb Wallet Frontend

## Lesson Overview
Welcome to lesson 5 of the PlebDevs Frontend Course! This is where everything comes together. After learning HTML, CSS, JavaScript, and setting up your development environment, we're now going to rebuild our Lightning wallet using React - the industry-standard library for building modern web applications.

This lesson transforms you from someone who writes individual HTML, CSS, and JavaScript files into a developer who can build sophisticated, component-based applications that scale and are maintainable.

## Prerequisites
- **Completion of Lessons 1-4** - HTML, CSS, JavaScript fundamentals, and development environment setup
- **Working development environment** - VS Code, Node.js, Git, GitHub account
- **Basic Lightning wallet** from lesson 3 (we'll be recreating this in React)
- **Understanding of APIs** - Making HTTP requests and handling responses
- **Familiarity with command line** - Running npm commands and Git operations

## Key Learning Objectives
- **Master React fundamentals** - Components, JSX, and the Virtual DOM
- **Understand component hierarchy** - Building applications as trees of reusable components
- **Learn state management** - Using React hooks to manage dynamic data
- **Handle side effects** - API calls, timers, and cleanup with useEffect
- **Pass data between components** - Props and component communication
- **Build real-time features** - Live Bitcoin price updates and transaction monitoring
- **Integrate with Lightning Network** - LNbits API for wallet functionality
- **Deploy a production React app** - From development to live deployment

## Why React Matters for Bitcoin Development

### The Problem with Vanilla JavaScript
Our previous wallet worked, but it had limitations:
- **Scattered code** - HTML, CSS, and JavaScript in separate files
- **Manual DOM manipulation** - Tedious and error-prone updates
- **Difficult state management** - Hard to keep UI in sync with data
- **Poor reusability** - Copying code instead of reusing components
- **Complex debugging** - Hard to trace problems across files

### The React Solution
React solves these problems with:
- **Component-based architecture** - Everything in logical, reusable pieces
- **Declarative programming** - Describe what UI should look like, React handles how
- **Automatic updates** - React updates DOM when data changes
- **Developer tools** - Excellent debugging and development experience
- **Industry adoption** - Used by Facebook, Netflix, Airbnb, and most Bitcoin companies

## React from a High Level

### Understanding the Virtual DOM
React's secret weapon is the Virtual DOM - a JavaScript representation of your actual webpage:

```javascript
// Instead of directly manipulating DOM like this:
document.getElementById('price').innerHTML = '$45000';

// React does this:
const [price, setPrice] = useState(45000);
return <div>Price: ${price}</div>; // React handles DOM updates
```

**How it works:**
1. **Virtual representation** - React creates a virtual copy of your DOM in memory
2. **Efficient diffing** - When data changes, React compares virtual DOM snapshots
3. **Minimal updates** - Only changes what actually needs updating
4. **Better performance** - Especially important for real-time Bitcoin price updates

### Why This Matters for Our Wallet
Imagine our wallet updating the Bitcoin price every 5 seconds:
- **Without React** - Re-render entire page, slow and inefficient
- **With React** - Update only the price display, smooth and fast

## React Components: The Building Blocks

### What Are Components?
Components are **reusable pieces of UI** that combine HTML, CSS, and JavaScript:

```jsx
// A simple Bitcoin price component
function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    
    return (
        <div className="price-display">
            <h2>Bitcoin Price</h2>
            <p>${price.toLocaleString()}</p>
        </div>
    );
}
```

### Component Benefits
- **Reusability** - Write once, use everywhere
- **Modularity** - Each component has a single responsibility
- **Testability** - Easy to test individual pieces
- **Maintainability** - Changes are isolated to specific components

### Functional vs Class Components

#### Class Components (Legacy - Don't Use)
```jsx
class BitcoinPrice extends React.Component {
    constructor(props) {
        super(props);
        this.state = { price: 0 };
    }
    
    render() {
        return <div>Price: ${this.state.price}</div>;
    }
}
```

#### Functional Components (Modern - Use This)
```jsx
function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    return <div>Price: ${price}</div>;
}
```

**Why functional components:**
- **Simpler syntax** - Less boilerplate code
- **Hooks support** - Modern React features
- **Better performance** - Easier for React to optimize
- **Industry standard** - What everyone uses now

## Component Hierarchy and Architecture

### Building Component Trees
React applications are built as **trees of components**:

```
App
├── Header
├── Buttons
├── BalanceCards
│   ├── BalanceCard (Balance)
│   └── BalanceCard (Price)
└── MainContent
    ├── Transactions
    └── Chart
```

### Planning Our Wallet Structure
For our Lightning wallet, we'll create:
- **App** - Main component that holds everything
- **Buttons** - Send/Receive functionality
- **BalanceCards** - Display balance and Bitcoin price
- **Transactions** - List of Lightning payments
- **Chart** - Real-time price visualization
- **PaymentsModal** - Send/receive Lightning payments

## JSX: HTML in JavaScript

### What is JSX?
JSX lets you write HTML-like syntax directly in JavaScript:

```jsx
// JSX (what you write)
const greeting = <h1>Hello, Bitcoin developers!</h1>;

// JavaScript (what it becomes)
const greeting = React.createElement('h1', null, 'Hello, Bitcoin developers!');
```

### JSX Differences from HTML
```jsx
// className instead of class
<div className="wallet-container">

// camelCase event handlers
<button onClick={handleClick}>

// Self-closing tags need /
<img src="bitcoin.png" />

// JavaScript expressions in {}
<p>Price: ${price}</p>
```

## Props: Passing Data Between Components

### What Are Props?
Props (properties) are how we pass data from parent to child components:

```jsx
// Parent component
function App() {
    const [price, setPrice] = useState(50000);
    return <PriceDisplay price={price} />;
}

// Child component
function PriceDisplay({ price }) {
    return <div>Bitcoin: ${price}</div>;
}
```

### Props Flow Rules
- **One-way flow** - Data only flows down (parent → child)
- **Read-only** - Child components cannot modify props
- **Any data type** - Strings, numbers, objects, arrays, functions

### Prop Destructuring
```jsx
// Method 1: Props object
function Header(props) {
    return <h1>{props.title}</h1>;
}

// Method 2: Destructuring (preferred)
function Header({ title }) {
    return <h1>{title}</h1>;
}
```

## React Hooks: Powerful State Management

### useState Hook: Managing Component State
State allows components to remember and change data:

```jsx
import { useState } from 'react';

function BitcoinPrice() {
    // [currentValue, setterFunction] = useState(initialValue)
    const [price, setPrice] = useState(0);
    const [loading, setLoading] = useState(true);
    
    const updatePrice = () => {
        setPrice(50000);
        setLoading(false);
    };
    
    return (
        <div>
            {loading ? 'Loading...' : `Price: $${price}`}
            <button onClick={updatePrice}>Update Price</button>
        </div>
    );
}
```

**State Rules:**
- **Always use setter function** - Never modify state directly
- **State updates trigger re-renders** - UI automatically updates
- **State is local** - Each component instance has its own state

### useEffect Hook: Side Effects and Lifecycle

useEffect handles operations that happen **outside** of rendering:

```jsx
import { useState, useEffect } from 'react';

function BitcoinPrice() {
    const [price, setPrice] = useState(0);
    
    useEffect(() => {
        // This runs after component mounts
        fetchBitcoinPrice().then(setPrice);
        
        // Set up interval for real-time updates
        const interval = setInterval(() => {
            fetchBitcoinPrice().then(setPrice);
        }, 5000);
        
        // Cleanup function (prevents memory leaks)
        return () => clearInterval(interval);
    }, []); // Empty array = run once on mount
    
    return <div>Price: ${price}</div>;
}
```

### useEffect Dependency Array
Controls **when** the effect runs:

```jsx
// No dependency array - runs on every render
useEffect(() => {
    console.log('Runs every render');
});

// Empty array - runs once on mount
useEffect(() => {
    console.log('Runs once on mount');
}, []);

// With dependencies - runs when dependencies change
useEffect(() => {
    console.log('Runs when price changes');
}, [price]);
```

## Building Our Lightning Wallet in React

### Project Setup
Starting from our Create React App foundation:

```bash
# Navigate to your project
cd pleb-wallet-react

# Install additional dependencies
npm install axios react-modal react-linechart

# Start development server
npm start
```

### App.js - Main Component Structure
```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import Buttons from './components/Buttons';
import Transactions from './components/Transactions';
import Chart from './components/Chart';
import PaymentsModal from './components/PaymentsModal';
import './App.css';

function App() {
    // State for Bitcoin price and wallet data
    const [price, setPrice] = useState(null);
    const [balance, setBalance] = useState(null);
    const [transactions, setTransactions] = useState([]);
    const [chartData, setChartData] = useState(null);
    const [modalState, setModalState] = useState({
        type: "",
        open: false,
    });

    // API functions
    const getPrice = () => {
        axios
            .get("https://api.coinbase.com/v2/prices/BTC-USD/spot")
            .then((res) => {
                setPrice(res.data.data.amount);
                updateChartData(res.data.data.amount);
            })
            .catch((err) => console.log(err));
    };

    const getWalletBalance = () => {
        const headers = {
            "X-Api-Key": "your-api-key-here",
        };
        axios
            .get("https://legend.lnbits.com/api/v1/wallet", { headers })
            .then((res) => {
                setBalance(res.data.balance / 1000); // Convert from millisats
            })
            .catch((err) => console.log(err));
    };

    const getTransactions = () => {
        const headers = {
            "X-Api-Key": "your-api-key-here",
        };
        axios
            .get("https://legend.lnbits.com/api/v1/payments", { headers })
            .then((res) => {
                setTransactions(res.data);
            })
            .catch((err) => console.log(err));
    };

    const updateChartData = (currentPrice) => {
        const timestamp = Date.now();
        setChartData((prevState) => {
            if (!prevState) {
                return [{
                    x: timestamp,
                    y: Number(currentPrice),
                }];
            }
            
            // Don't add duplicate data points
            if (prevState[prevState.length - 1].y === Number(currentPrice)) {
                return prevState;
            }
            
            return [
                ...prevState,
                {
                    x: timestamp,
                    y: Number(currentPrice),
                }
            ];
        });
    };

    // Initial data fetch
    useEffect(() => {
        getPrice();
        getWalletBalance();
        getTransactions();
    }, []);

    // Real-time updates every 5 seconds
    useEffect(() => {
        const interval = setInterval(() => {
            getPrice();
            getWalletBalance();
            getTransactions();
        }, 5000);
        
        return () => clearInterval(interval);
    }, []);

    return (
        <div className="App">
            <header>
                <h1>pleb wallet</h1>
            </header>
            
            <Buttons modalState={modalState} setModalState={setModalState} />
            
            <div className="row">
                <div className="balance-card">
                    <h2>Balance</h2>
                    <p>{balance} sats</p>
                </div>
                <div className="balance-card">
                    <h2>Price</h2>
                    <p>${price}</p>
                </div>
            </div>
            
            <div className="row">
                <div className="row-item">
                    <Transactions transactions={transactions} />
                </div>
                <div className="row-item">
                    <Chart chartData={chartData} />
                </div>
            </div>
            
            <PaymentsModal 
                modalState={modalState} 
                setModalState={setModalState} 
            />
            
            <footer>
                <p>Made by plebs, for plebs.</p>
            </footer>
        </div>
    );
}

export default App;
```

### Buttons Component
```jsx
import React from "react";
import "./Buttons.css";

const Buttons = ({ modalState, setModalState }) => {
    const handleSendClick = () => {
        setModalState({ open: true, type: "send" });
    };

    const handleReceiveClick = () => {
        setModalState({ open: true, type: "receive" });
    };

    return (
        <div className="buttons">
            <button className="button" onClick={handleSendClick}>
                Send
            </button>
            <button className="button" onClick={handleReceiveClick}>
                Receive
            </button>
        </div>
    );
};

export default Buttons;
```

### Transactions Component
```jsx
import React from "react";
import "./Transactions.css";

const Transactions = ({ transactions }) => {
    const parseTx = (tx) => {
        const date = new Date(tx.time * 1000);
        const formattedDate = date.toLocaleDateString("en-US");
        
        // Skip pending transactions
        if (tx.pending) return null;

        if (tx.amount > 0) {
            return (
                <div key={tx.checking_id} className="tx-item">
                    <p>Received from {tx.bolt11.substring(0, 25)}...</p>
                    <p>+{tx.amount / 1000} sats</p>
                    <p className="transaction-date">{formattedDate}</p>
                </div>
            );
        }

        if (tx.amount < 0) {
            return (
                <div key={tx.checking_id} className="tx-item">
                    <p>Sent with {tx.bolt11.substring(0, 25)}...</p>
                    <p>{tx.amount / 1000} sats</p>
                    <p className="transaction-date">{formattedDate}</p>
                </div>
            );
        }
    };

    return (
        <div>
            <h3>Transactions</h3>
            {transactions.map((transaction) => parseTx(transaction))}
        </div>
    );
};

export default Transactions;
```

### Chart Component
```jsx
import React from "react";
import LineChart from "react-linechart";
import "../../node_modules/react-linechart/dist/styles.css";
import "./Chart.css";

const Chart = ({ chartData }) => {
    if (!chartData || !chartData.length) return null;

    const data = [
        {
            color: "steelblue",
            points: chartData,
        },
    ];

    return (
        <div className="chart-container">
            {chartData.length <= 1 ? (
                <p>Loading chart...</p>
            ) : (
                <LineChart
                    xLabel="Time"
                    height={300}
                    width={550}
                    data={data}
                    onPointHover={(obj) => `price: $${obj.y}<br />time: ${obj.x}`}
                    ticks={4}
                    hideYLabel={true}
                    hideXLabel={true}
                    xDisplay={(timestamp) =>
                        new Date(timestamp).toLocaleTimeString("en-US")
                    }
                />
            )}
        </div>
    );
};

export default Chart;
```

### PaymentsModal Component
```jsx
import React, { useState } from "react";
import Modal from "react-modal";
import axios from "axios";
import "./PaymentsModal.css";

const customStyles = {
    content: {
        top: "20%",
        left: "40%",
        right: "40%",
        bottom: "auto",
    },
};

const PaymentsModal = ({ modalState, setModalState }) => {
    const [formData, setFormData] = useState({
        amount: 0,
        invoiceToPay: "",
    });
    const [invoice, setInvoice] = useState("");
    const [paymentInfo, setPaymentInfo] = useState({
        paymentHash: "",
        checkingId: "",
    });

    const handleSend = (e) => {
        e.preventDefault();
        const headers = {
            "X-Api-Key": "your-api-key-here",
        };
        const data = {
            bolt11: formData.invoiceToPay,
            out: true,
        };
        
        axios
            .post("https://legend.lnbits.com/api/v1/payments", data, { headers })
            .then((res) =>
                setPaymentInfo({
                    paymentHash: res.data.payment_hash,
                    checkingId: res.data.checking_id,
                })
            )
            .catch((err) => console.log(err));
    };

    const handleReceive = (e) => {
        e.preventDefault();
        const headers = {
            "X-Api-Key": "your-api-key-here",
        };
        const data = {
            amount: formData.amount,
            out: false,
            memo: "LNBits",
        };
        
        axios
            .post("https://legend.lnbits.com/api/v1/payments", data, { headers })
            .then((res) => setInvoice(res.data.payment_request))
            .catch((err) => console.log(err));
    };

    const clearForms = () => {
        setModalState({ type: "", open: false });
        setInvoice("");
        setPaymentInfo({ paymentHash: "", checkingId: "" });
        setFormData({ amount: 0, invoiceToPay: "" });
    };

    return (
        <Modal
            isOpen={modalState.open}
            style={customStyles}
            contentLabel="Payments Modal"
            appElement={document.getElementById("root")}
        >
            <p className="close-button" onClick={clearForms}>
                X
            </p>
            
            {modalState.type === "send" && (
                <form>
                    <label>Paste an invoice</label>
                    <input
                        type="text"
                        value={formData.invoiceToPay}
                        onChange={(e) =>
                            setFormData({ ...formData, invoiceToPay: e.target.value })
                        }
                    />
                    <button className="button" onClick={handleSend}>
                        Submit
                    </button>
                </form>
            )}
            
            {modalState.type === "receive" && (
                <form>
                    <label>Enter amount</label>
                    <input
                        type="number"
                        min="0"
                        value={formData.amount}
                        onChange={(e) =>
                            setFormData({ ...formData, amount: e.target.value })
                        }
                    />
                    <button className="button" onClick={handleReceive}>
                        Submit
                    </button>
                </form>
            )}
            
            {invoice && (
                <section>
                    <h3>Invoice created</h3>
                    <p>{invoice}</p>
                </section>
            )}
            
            {paymentInfo.paymentHash && (
                <section>
                    <h3>Payment sent</h3>
                    <p>Payment hash: {paymentInfo.paymentHash}</p>
                    <p>Checking id: {paymentInfo.checkingId}</p>
                </section>
            )}
        </Modal>
    );
};

export default PaymentsModal;
```

## Understanding Callback Functions

### What Are Callbacks?
A callback is a **function passed into another function** to be called at the appropriate time:

```jsx
// Higher-order function that accepts a callback
function fetchData(callback) {
    // Simulate API call
    setTimeout(() => {
        const data = { price: 50000 };
        callback(data); // Call the callback with data
    }, 1000);
}

// Using the callback
fetchData((data) => {
    console.log('Price received:', data.price);
});
```

### Why Callbacks Matter
Callbacks handle **timing and asynchronous operations**:
- **API calls** - Don't know when they'll complete
- **User interactions** - Don't know when users will click
- **Timers** - Need to run code at specific times

### Callbacks in React
```jsx
// Event handler callback
<button onClick={(e) => handleClick(e)}>

// useEffect callback
useEffect(() => {
    // This function is a callback
    fetchData();
}, []);

// Array method callbacks
transactions.map((tx) => <div key={tx.id}>{tx.amount}</div>)
```

## Package Management with NPM

### Installing Packages
```bash
# Install a package for your project
npm install axios                  # HTTP client
npm install react-modal            # Modal dialogs
npm install react-linechart        # Charts
```

### Using Installed Packages
```jsx
// Import and use packages
import axios from 'axios';
import Modal from 'react-modal';
import LineChart from 'react-linechart';

// Use them in your components
const response = await axios.get('/api/data');
```

### Managing Dependencies
```json
// package.json automatically updated
{
  "dependencies": {
    "axios": "^1.6.0",
    "react": "^18.2.0",
    "react-modal": "^3.16.1",
    "react-linechart": "^1.3.1"
  }
}
```

## Styling in React

### CSS Modules and Component Styling
Each component has its own CSS file:

```css
/* Buttons.css */
.buttons {
    width: 50%;
    margin: 0 auto;
    margin-top: 3%;
    display: flex;
    justify-content: space-around;
}

.button {
    background-color: #ffbf46;
    border: 2px solid #8a4fff;
    border-radius: 5px;
    padding: 5px;
    font-size: 1.2rem;
    font-family: monospace;
    font-weight: bold;
    width: 100px;
}

.button:hover {
    cursor: pointer;
    opacity: 0.6;
}
```

### Responsive Design
```css
/* Mobile-first responsive design */
@media (max-width: 876px) {
    .row-item {
        width: 100%;
        height: 200px;
    }

    .balance-card {
        width: 100%;
        text-align: center;
        margin-right: 2%;
        margin-left: 2%;
    }
}

@media (max-width: 615px) {
    .row {
        flex-direction: column;
    }

    .balance-card {
        width: 80%;
        margin: 0 auto;
        margin-top: 1%;
    }
}
```

## Lightning Network Integration

### LNbits API Integration
Our wallet integrates with LNbits for Lightning functionality:

```jsx
// Wallet configuration
const LNBITS_URL = "https://legend.lnbits.com";
const API_KEY = "your-api-key-here"; // Store securely in production

// API headers
const headers = {
    "X-Api-Key": API_KEY,
};

// Get wallet balance
const getBalance = async () => {
    const response = await axios.get(`${LNBITS_URL}/api/v1/wallet`, { headers });
    return response.data.balance / 1000; // Convert from millisats
};

// Create Lightning invoice
const createInvoice = async (amount, memo) => {
    const data = { amount, out: false, memo };
    const response = await axios.post(`${LNBITS_URL}/api/v1/payments`, data, { headers });
    return response.data.payment_request;
};

// Pay Lightning invoice
const payInvoice = async (bolt11) => {
    const data = { bolt11, out: true };
    const response = await axios.post(`${LNBITS_URL}/api/v1/payments`, data, { headers });
    return response.data;
};
```

### Real-time Updates
```jsx
// Update wallet data every 5 seconds
useEffect(() => {
    const interval = setInterval(() => {
        getPrice();
        getWalletBalance();
        getTransactions();
    }, 5000);
    
    return () => clearInterval(interval);
}, []);
```

## Deployment and Production

### Building for Production
```bash
# Create optimized production build
npm run build

# This creates a 'build' folder with optimized files
```

### Environment Variables
Store sensitive data like API keys:

```bash
# .env file (never commit to Git)
REACT_APP_LNBITS_URL=https://legend.lnbits.com
REACT_APP_API_KEY=your-secret-key
```

```jsx
// Use in your app
const apiKey = process.env.REACT_APP_API_KEY;
const lnbitsUrl = process.env.REACT_APP_LNBITS_URL;
```

### Deployment to Vercel
```bash
# Push to GitHub
git add .
git commit -m "Complete React wallet implementation"
git push origin main

# Vercel automatically builds and deploys
```

## Hands-on Exercises

### Exercise 1: Component Creation Practice
Create additional wallet components:

1. **WalletStats Component**
   ```jsx
   // Display wallet statistics
   - Total transactions
   - Average transaction amount
   - Wallet age
   - Favorite payment memo
   ```

2. **QRCode Component**
   ```jsx
   // Generate QR codes for Lightning invoices
   npm install qrcode.react
   // Display QR code in PaymentsModal
   ```

3. **Settings Component**
   ```jsx
   // Wallet settings
   - Currency display (sats/BTC/USD)
   - Update frequency
   - Theme selection
   ```

### Exercise 2: Advanced State Management
Enhance the wallet with more sophisticated state:

1. **Local Storage Persistence**
   ```jsx
   // Save settings to localStorage
   useEffect(() => {
       const savedSettings = localStorage.getItem('walletSettings');
       if (savedSettings) {
           setSettings(JSON.parse(savedSettings));
       }
   }, []);
   ```

2. **Error Handling**
   ```jsx
   // Add error states and retry logic
   const [error, setError] = useState(null);
   const [retryCount, setRetryCount] = useState(0);
   ```

3. **Loading States**
   ```jsx
   // Better loading indicators
   const [loading, setLoading] = useState({
       price: false,
       balance: false,
       transactions: false
   });
   ```

### Exercise 3: Enhanced Features
Add new wallet capabilities:

1. **Transaction Filtering**
   ```jsx
   // Filter transactions by:
   - Date range
   - Amount range
   - Payment type (sent/received)
   ```

2. **Export Functionality**
   ```jsx
   // Export transaction history
   - CSV format
   - JSON format
   - PDF reports
   ```

3. **Multiple Wallets**
   ```jsx
   // Support multiple LNbits wallets
   - Wallet switching
   - Combined balance view
   - Wallet comparison
   ```

### Exercise 4: Performance Optimization
Optimize your React wallet:

1. **Memoization**
   ```jsx
   import { useMemo, useCallback } from 'react';
   
   // Memoize expensive calculations
   const expensiveValue = useMemo(() => {
       return calculateComplexStats(transactions);
   }, [transactions]);
   ```

2. **Component Optimization**
   ```jsx
   import { memo } from 'react';
   
   // Prevent unnecessary re-renders
   const TransactionItem = memo(({ transaction }) => {
       return <div>{transaction.amount}</div>;
   });
   ```

3. **Code Splitting**
   ```jsx
   import { lazy, Suspense } from 'react';
   
   // Lazy load heavy components
   const Chart = lazy(() => import('./components/Chart'));
   ```

## Common React Patterns and Best Practices

### State Management Best Practices
```jsx
// ✅ Good: Keep state as local as possible
function TransactionList({ transactions }) {
    const [filter, setFilter] = useState('all');
    // Filter only affects this component
}

// ❌ Avoid: Lifting state unnecessarily
function App() {
    const [filter, setFilter] = useState('all'); // Too high in tree
}
```

### Effect Dependencies
```jsx
// ✅ Good: Include all dependencies
useEffect(() => {
    fetchTransactions(walletId, currency);
}, [walletId, currency]);

// ❌ Avoid: Missing dependencies
useEffect(() => {
    fetchTransactions(walletId, currency);
}, []); // Missing walletId, currency
```

### Component Organization
```jsx
// ✅ Good: Single responsibility
function BitcoinPrice({ price }) {
    return <div>Bitcoin: ${price}</div>;
}

// ❌ Avoid: Too many responsibilities
function WalletDashboard() {
    // 500 lines of code doing everything
}
```

### Error Boundaries
```jsx
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false };
    }

    static getDerivedStateFromError(error) {
        return { hasError: true };
    }

    render() {
        if (this.state.hasError) {
            return <h1>Something went wrong with the wallet.</h1>;
        }

        return this.props.children;
    }
}

// Wrap your app
<ErrorBoundary>
    <App />
</ErrorBoundary>
```

## Testing Your React Wallet

### Basic Component Testing
```jsx
import { render, screen } from '@testing-library/react';
import BitcoinPrice from './BitcoinPrice';

test('displays bitcoin price', () => {
    render(<BitcoinPrice price={50000} />);
    expect(screen.getByText('Bitcoin: $50000')).toBeInTheDocument();
});
```

### Testing Hooks
```jsx
import { renderHook, act } from '@testing-library/react';
import { useState } from 'react';

test('wallet balance updates correctly', () => {
    const { result } = renderHook(() => useState(0));
    
    act(() => {
        result.current[1](1000); // setBalance(1000)
    });
    
    expect(result.current[0]).toBe(1000);
});
```

### Integration Testing
```jsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import App from './App';

test('can create lightning invoice', async () => {
    render(<App />);
    
    fireEvent.click(screen.getByText('Receive'));
    fireEvent.change(screen.getByLabelText('Enter amount'), {
        target: { value: '1000' }
    });
    fireEvent.click(screen.getByText('Submit'));
    
    await waitFor(() => {
        expect(screen.getByText('Invoice created')).toBeInTheDocument();
    });
});
```

## Learning Resources

### React Documentation
- **[React.dev](https://react.dev/)** - Official React documentation with interactive examples
- **[React Hooks Reference](https://react.dev/reference/react)** - Complete hooks documentation
- **[React Patterns](https://reactpatterns.com/)** - Common patterns and best practices

### React Learning Paths
- **[React Tutorial](https://react.dev/learn)** - Step-by-step interactive tutorial
- **[FreeCodeCamp React Course](https://www.freecodecamp.org/learn/front-end-development-libraries/)** - Comprehensive free course
- **[React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)** - For adding TypeScript

### Lightning Development
- **[LNbits Documentation](https://lnbits.com/)** - Lightning wallet and account system
- **[Lightning Network Paper](https://lightning.network/lightning-network-paper.pdf)** - Understanding Lightning Network
- **[Bitcoin Development Guide](https://developer.bitcoin.org/)** - Official Bitcoin development resources

### Development Tools
- **[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/)** - Browser extension for debugging
- **[Axios Documentation](https://axios-http.com/)** - HTTP client for API calls
- **[React Modal](https://reactcommunity.org/react-modal/)** - Modal component documentation

## Next Steps and Advanced Topics

### Immediate Next Steps
1. **Complete the wallet project** - Implement all features from the lesson
2. **Deploy to production** - Get your wallet live on the internet
3. **Add error handling** - Make your wallet robust and user-friendly
4. **Implement testing** - Add unit and integration tests
5. **Share your work** - Show the Bitcoin community what you built

### Advanced React Topics
- **Context API** - Global state management
- **React Router** - Multi-page applications
- **Server-Side Rendering** - Next.js framework
- **React Native** - Mobile app development
- **Performance Optimization** - Profiling and optimization techniques

### Lightning Development Path
- **Watchtower Implementation** - Lightning security features
- **Multi-signature Wallets** - Enhanced security
- **Lightning Service Provider** - Channel management
- **BTCPay Server Integration** - Merchant payment processing
- **Lightning Node Management** - Running your own node

### Career Development
- **Build Portfolio Projects** - Create multiple Lightning applications
- **Contribute to Open Source** - Lightning protocol implementations
- **Join Bitcoin Companies** - Companies building on Lightning
- **Start Your Own Project** - Lightning-powered business ideas
- **Teach Others** - Share knowledge with the Bitcoin community

## Key Takeaways

1. **React transforms development** - Component-based architecture scales much better than vanilla JavaScript
2. **State management is crucial** - useState and useEffect are the foundation of dynamic React apps
3. **Component hierarchy matters** - Plan your component tree before building
4. **Props flow down only** - Data flows from parent to child components
5. **Hooks enable powerful features** - Modern React is built on functional components and hooks
6. **Lightning integration is straightforward** - LNbits makes Lightning development accessible
7. **Real-time updates enhance UX** - Users expect live data in Bitcoin applications
8. **Package management saves time** - npm ecosystem provides solutions for common needs
9. **Testing ensures reliability** - Critical for handling real Bitcoin transactions
10. **Deployment should be automatic** - Continuous deployment from GitHub to production

Congratulations! You've built a fully functional Lightning wallet using React. You now understand modern frontend development and can build sophisticated Bitcoin applications. The skills you've learned here are directly applicable to working at Bitcoin companies and building your own Lightning-powered projects.

This React wallet is just the beginning. You can now extend it with advanced features, integrate with other Lightning services, or use these skills to build entirely new Bitcoin applications. Welcome to the world of Bitcoin development! ⚡️🧡

---

## What's Next?

In the final lesson (Lesson 6), you'll learn:
- **Advanced React patterns** - Context API, custom hooks, performance optimization
- **Production deployment strategies** - Environment variables, monitoring, scaling
- **Security best practices** - Protecting user funds and data
- **Lightning Network deep dive** - Advanced Lightning concepts and implementations
- **Career guidance** - Building your portfolio and finding Bitcoin development opportunities

The journey to becoming a Bitcoin developer continues! 🚀 