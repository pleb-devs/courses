# Building Your Lightning Wallet with React

[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-6.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-6.mp4)

## Lesson Overview
Welcome to the final lesson of the PlebDevs Frontend Course! This is where we bring everything together and build a fully functional Lightning wallet using React. You'll transform from someone who understands individual web technologies into a developer who can build modern, component-based applications that scale.

In this lesson, we'll rebuild our Lightning wallet from lesson 3, but this time using React's powerful component system, state management, and modern development practices. By the end, you'll have a professional-grade wallet that updates in real-time and can be deployed to production.

## Prerequisites
- **Completion of Lessons 1-5** - HTML, CSS, JavaScript, development environment, and React fundamentals
- **Working React development environment** - Node.js, npm, VS Code, Git
- **Understanding of Lightning Network basics** - LNbits API, invoices, payments
- **Familiarity with APIs** - Making HTTP requests with axios
- **Command line comfort** - Running npm commands and Git operations

## Key Learning Objectives
- **Master React component architecture** - Building scalable, reusable UI components
- **Implement advanced state management** - Multiple useState hooks and complex state updates
- **Handle real-time data** - Live Bitcoin price feeds and wallet balance updates
- **Integrate external libraries** - React Modal, React LineChart, and axios
- **Build interactive user interfaces** - Modal dialogs, forms, and dynamic content
- **Implement Lightning functionality** - Creating invoices, sending payments, transaction history
- **Deploy a production React app** - From development to live wallet
- **Follow React best practices** - Component hierarchy, props flow, and clean code

## Why React for Lightning Development

### The Evolution from Vanilla JavaScript
Our previous wallet worked, but had significant limitations:
- **Scattered architecture** - Logic spread across multiple files
- **Manual DOM updates** - Tedious and error-prone element manipulation
- **State synchronization issues** - Keeping UI in sync with changing data
- **Code duplication** - Repeating similar functionality
- **Debugging difficulties** - Hard to trace data flow and component interactions

### React's Lightning-Powered Solution
React solves these problems with:
- **Component-based architecture** - Logical, reusable pieces of functionality
- **Automatic UI updates** - React handles DOM changes when data changes
- **Predictable data flow** - Props flow down, events flow up
- **Developer experience** - Excellent debugging tools and development workflow
- **Industry adoption** - Used by major Bitcoin companies like Coinbase, Kraken, and Lightning Labs

## React Component Hierarchy Planning

### Understanding the Wallet Structure
Before building, let's plan our component tree:

```
App (Main wallet container)
├── Header (Wallet title)
├── Buttons (Send/Receive actions)
├── BalanceCards (Balance and Price display)
├── MainContent
│   ├── Transactions (Payment history)
│   └── Chart (Real-time Bitcoin price chart)
└── PaymentsModal (Send/Receive forms)
```

### Data Flow Architecture
Understanding how data flows through our wallet:
- **App component** - Holds all state (price, balance, transactions, chart data)
- **Child components** - Receive data via props, send events via callbacks
- **State updates** - Trigger automatic re-renders throughout the component tree
- **API integration** - Centralized in App component, shared via props

## Building the React Lightning Wallet

### Project Setup and Initial Structure

Starting with our Create React App foundation:

```bash
# Navigate to your project
cd pleb-wallet-frontend

# Install required dependencies
npm install axios react-modal react-linechart

# Start development server
npm start
```

### App.js - The Heart of Our Wallet

Our main App component will manage all wallet state and functionality:

```jsx
import React, { useEffect, useState } from "react";
import axios from "axios";
import Buttons from "./components/Buttons";
import Transactions from "./components/Transactions";
import Chart from "./components/Chart";
import PaymentsModal from "./components/PaymentsModal";
import "./App.css";

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

  // API function to get Bitcoin price
  const getPrice = () => {
    axios
      .get("https://api.coinbase.com/v2/prices/BTC-USD/spot")
      .then((res) => {
        setPrice(res.data.data.amount);
        updateChartData(res.data.data.amount);
      })
      .catch((err) => console.log(err));
  };

  // API function to get wallet balance from LNbits
  const getWalletBalance = () => {
    const headers = {
      "X-Api-Key": "your-api-key-here", // Replace with your LNbits API key
    };
    axios
      .get("https://legend.lnbits.com/api/v1/wallet", { headers })
      .then((res) => {
        setBalance(res.data.balance / 1000); // Convert from millisats
      })
      .catch((err) => console.log(err));
  };

  // API function to get transaction history
  const getTransactions = () => {
    const headers = {
      "X-Api-Key": "your-api-key-here", // Replace with your LNbits API key
    };
    axios
      .get("https://legend.lnbits.com/api/v1/payments", { headers })
      .then((res) => {
        setTransactions(res.data);
      })
      .catch((err) => console.log(err));
  };

  // Function to update chart data with new price points
  const updateChartData = (currentPrice) => {
    const timestamp = Date.now();
    setChartData((prevState) => {
      // If no previous data, create initial point
      if (!prevState) {
        return [{
          x: timestamp,
          y: Number(currentPrice),
        }];
      }
      
      // Don't add duplicate data points
      if (
        prevState[prevState.length - 1].x === timestamp ||
        prevState[prevState.length - 1].y === Number(currentPrice)
      ) {
        return prevState;
      }
      
      // Add new data point to existing array
      return [
        ...prevState,
        {
          x: timestamp,
          y: Number(currentPrice),
        }
      ];
    });
  };

  // Initial data fetch on component mount
  useEffect(() => {
    getPrice();
    getWalletBalance();
    getTransactions();
  }, []);

  // Set up real-time updates every 5 seconds
  useEffect(() => {
    const interval = setInterval(() => {
      getPrice();
      getWalletBalance();
      getTransactions();
    }, 5000);
    
    // Cleanup interval on component unmount
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

### Understanding React Hooks in Practice

#### useState for State Management
Each piece of dynamic data gets its own state:

```jsx
// Bitcoin price from Coinbase API
const [price, setPrice] = useState(null);

// Wallet balance from LNbits
const [balance, setBalance] = useState(null);

// Transaction history array
const [transactions, setTransactions] = useState([]);

// Chart data points for price visualization
const [chartData, setChartData] = useState(null);

// Modal state for send/receive dialogs
const [modalState, setModalState] = useState({
  type: "",
  open: false,
});
```

#### useEffect for Side Effects
Managing when and how our effects run:

```jsx
// Run once on component mount
useEffect(() => {
  getPrice();
  getWalletBalance();
  getTransactions();
}, []); // Empty dependency array = run once

// Run repeatedly every 5 seconds
useEffect(() => {
  const interval = setInterval(() => {
    getPrice();
    getWalletBalance();
    getTransactions();
  }, 5000);
  
  // Cleanup function prevents memory leaks
  return () => clearInterval(interval);
}, []); // Empty dependency = set up once, cleanup on unmount
```

### Building the Buttons Component

Creating reusable UI components with clear responsibilities:

```jsx
// components/Buttons.js
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

#### Buttons Component Styles
```css
/* components/Buttons.css */
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

### Building the Transactions Component

Displaying and parsing Lightning transaction data:

```jsx
// components/Transactions.js
import React from "react";
import "./Transactions.css";

const Transactions = ({ transactions }) => {
  const parseTx = (tx) => {
    const date = new Date(tx.time * 1000);
    const formattedDate = date.toLocaleDateString("en-US");
    
    // Skip pending transactions
    if (tx.pending) return null;

    // Handle received payments
    if (tx.amount > 0) {
      return (
        <div key={tx.checking_id} className="tx-item">
          <p>Received from {tx.bolt11.substring(0, 25)}...</p>
          <p>+{tx.amount / 1000} sats</p>
          <p className="transaction-date">{formattedDate}</p>
        </div>
      );
    }

    // Handle sent payments
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

### Building the Chart Component

Real-time Bitcoin price visualization:

```jsx
// components/Chart.js
import React from "react";
import LineChart from "react-linechart";
import "../../node_modules/react-linechart/dist/styles.css";
import "./Chart.css";

const Chart = ({ chartData }) => {
  // Don't render if no data
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

### Building the PaymentsModal Component

Complex modal for Lightning payments:

```jsx
// components/PaymentsModal.js
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
  // Form data for sending/receiving
  const [formData, setFormData] = useState({
    amount: 0,
    invoiceToPay: "",
  });
  
  // Generated invoice for receiving
  const [invoice, setInvoice] = useState("");
  
  // Payment confirmation data
  const [paymentInfo, setPaymentInfo] = useState({
    paymentHash: "",
    checkingId: "",
  });

  // Handle sending Lightning payments
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

  // Handle creating Lightning invoices
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

  // Clear all form state when modal closes
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
      
      {/* Send payment form */}
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
      
      {/* Receive payment form */}
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
      
      {/* Display generated invoice */}
      {invoice && (
        <section>
          <h3>Invoice created</h3>
          <p>{invoice}</p>
        </section>
      )}
      
      {/* Display payment confirmation */}
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

## Advanced React Concepts

### Callback Functions and Event Handling

Understanding how React components communicate:

```jsx
// Parent component passes callback to child
<Buttons 
  modalState={modalState} 
  setModalState={setModalState}  // This is a callback
/>

// Child component calls the callback
const handleSendClick = () => {
  setModalState({ open: true, type: "send" });  // Calling parent's callback
};
```

### State Updates and Functional Updates

Complex state updates using previous state:

```jsx
// Functional update for complex state changes
setChartData((prevState) => {
  if (!prevState) {
    return [{ x: timestamp, y: Number(currentPrice) }];
  }
  
  return [
    ...prevState,  // Spread operator to copy existing data
    { x: timestamp, y: Number(currentPrice) }  // Add new data point
  ];
});
```

### Prop Destructuring and Component Props

Clean ways to handle props in components:

```jsx
// Method 1: Props object
function Transactions(props) {
  return <div>{props.transactions.map(...)}</div>;
}

// Method 2: Destructuring (preferred)
function Transactions({ transactions }) {
  return <div>{transactions.map(...)}</div>;
}

// Method 3: Multiple props destructuring
function PaymentsModal({ modalState, setModalState }) {
  // Clean access to multiple props
}
```

## Package Management with NPM

### Installing and Managing Dependencies

```bash
# Install a package and save to package.json
npm install axios
npm install react-modal
npm install react-linechart

# Install development dependencies
npm install --save-dev eslint

# Install specific version
npm install react@18.2.0

# Update packages
npm update

# Check for vulnerabilities
npm audit
```

### Understanding package.json

```json
{
  "name": "pleb-wallet-frontend",
  "version": "1.0.0",
  "dependencies": {
    "axios": "^1.6.0",
    "react": "^18.2.0",
    "react-modal": "^3.16.1",
    "react-linechart": "^1.3.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

## Lightning Network Integration Deep Dive

### LNbits API Integration

Complete API integration for Lightning functionality:

```jsx
// Configuration constants
const LNBITS_URL = "https://legend.lnbits.com";
const API_KEY = process.env.REACT_APP_API_KEY; // Use environment variables

// Helper function for API headers
const getHeaders = () => ({
  "X-Api-Key": API_KEY,
  "Content-Type": "application/json"
});

// Get wallet balance
const getWalletBalance = async () => {
  try {
    const response = await axios.get(
      `${LNBITS_URL}/api/v1/wallet`, 
      { headers: getHeaders() }
    );
    return response.data.balance / 1000; // Convert from millisats
  } catch (error) {
    console.error("Error fetching balance:", error);
    throw error;
  }
};

// Create Lightning invoice
const createInvoice = async (amount, memo = "LNbits") => {
  try {
    const data = { amount, out: false, memo };
    const response = await axios.post(
      `${LNBITS_URL}/api/v1/payments`, 
      data, 
      { headers: getHeaders() }
    );
    return response.data.payment_request;
  } catch (error) {
    console.error("Error creating invoice:", error);
    throw error;
  }
};

// Pay Lightning invoice
const payInvoice = async (bolt11) => {
  try {
    const data = { bolt11, out: true };
    const response = await axios.post(
      `${LNBITS_URL}/api/v1/payments`, 
      data, 
      { headers: getHeaders() }
    );
    return response.data;
  } catch (error) {
    console.error("Error paying invoice:", error);
    throw error;
  }
};
```

### Error Handling and User Feedback

Implementing robust error handling:

```jsx
const [error, setError] = useState(null);
const [loading, setLoading] = useState(false);

const handlePayment = async (invoice) => {
  setLoading(true);
  setError(null);
  
  try {
    const result = await payInvoice(invoice);
    setPaymentInfo(result);
  } catch (error) {
    setError("Payment failed: " + error.message);
  } finally {
    setLoading(false);
  }
};

// In your JSX
{error && (
  <div className="error-message">
    {error}
  </div>
)}

{loading && (
  <div className="loading-spinner">
    Processing payment...
  </div>
)}
```

## Styling and Responsive Design

### Complete App.css Styles

```css
/* App.css */
body {
  background-color: #192734;
  font-family: monospace;
}

header {
  border-bottom: 2px solid #ffbf46;
}

h1 {
  text-align: center;
  color: #8a4fff;
}

footer {
  border-top: 2px solid #ffbf46;
  padding: 1%;
  text-align: center;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}

footer p {
  color: #8a4fff;
}

.balance-card {
  background-color: #ffbf46;
  border: 2px solid #8a4fff;
  padding: 1%;
  width: 25%;
  margin-top: 3%;
  margin-bottom: 1%;
  border-radius: 5px;
}

.balance-card p {
  font-size: 1.2rem;
  font-weight: bold;
}

.row {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.row-item {
  background-color: ghostwhite;
  border: 2px solid #8a4fff;
  border-radius: 5px;
  height: 300px;
  width: 40%;
  overflow: scroll;
}

/* Responsive design for mobile */
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

  .row-item {
    margin-top: 1%;
  }

  .balance-card {
    width: 80%;
    margin: 0 auto;
    margin-top: 1%;
  }
}
```

### Component-Specific Styling

Each component has its own CSS file for better organization:

```css
/* components/PaymentsModal.css */
form {
  margin: 2% auto;
  display: flex;
  flex-direction: column;
  text-align: center;
}

form input {
  margin: 10px 0;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

form .button {
  margin: 10% auto;
}

section h3 {
  text-align: center;
}

section p {
  word-wrap: break-word;
}

.close-button {
  cursor: pointer;
  text-align: end;
  font-weight: bold;
  font-size: 1.2rem;
}
```

## Testing Your React Wallet

### Manual Testing Checklist

Before deploying, test all functionality:

1. **Initial Load**
   - [ ] Bitcoin price displays correctly
   - [ ] Wallet balance loads from LNbits
   - [ ] Transaction history appears
   - [ ] Chart initializes properly

2. **Real-time Updates**
   - [ ] Price updates every 5 seconds
   - [ ] Balance reflects new transactions
   - [ ] Chart adds new data points
   - [ ] No duplicate API calls

3. **Send Functionality**
   - [ ] Modal opens when Send clicked
   - [ ] Can paste Lightning invoice
   - [ ] Payment processes successfully
   - [ ] Payment confirmation displays
   - [ ] Balance updates after payment

4. **Receive Functionality**
   - [ ] Modal opens when Receive clicked
   - [ ] Can enter amount
   - [ ] Invoice generates correctly
   - [ ] QR code displays (if implemented)
   - [ ] Invoice can be paid externally

### Debugging React Applications

Using React Developer Tools:

```jsx
// Add console logs for debugging
useEffect(() => {
  console.log('Price updated:', price);
  console.log('Chart data:', chartData);
}, [price, chartData]);

// Debug state changes
const handleSend = (e) => {
  console.log('Send clicked with data:', formData);
  // ... rest of function
};
```

## Environment Variables and Security

### Securing API Keys

Never commit API keys to Git. Use environment variables:

```bash
# Create .env file in project root
REACT_APP_LNBITS_URL=https://legend.lnbits.com
REACT_APP_API_KEY=your-secret-api-key-here
```

```jsx
// Use in your React app
const apiKey = process.env.REACT_APP_API_KEY;
const lnbitsUrl = process.env.REACT_APP_LNBITS_URL;

if (!apiKey) {
  console.error('API key not found. Please set REACT_APP_API_KEY in .env file');
}
```

```gitignore
# Add to .gitignore
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
```

## Deployment and Production

### Building for Production

```bash
# Create optimized production build
npm run build

# This creates a 'build' folder with:
# - Minified JavaScript
# - Optimized CSS
# - Compressed assets
# - Service worker for caching
```

### Deployment to Vercel

```bash
# Push to GitHub
git add .
git commit -m "Complete React Lightning wallet"
git push origin main

# Vercel automatically:
# 1. Detects the push
# 2. Runs npm run build
# 3. Deploys to production URL
# 4. Provides preview deployments
```

### Environment Variables in Production

Set environment variables in Vercel dashboard:
1. Go to your project settings
2. Add environment variables
3. Redeploy to apply changes

## Hands-on Exercises

### Exercise 1: Enhanced Error Handling
Add comprehensive error handling to your wallet:

```jsx
// Add error states
const [errors, setErrors] = useState({
  price: null,
  balance: null,
  transactions: null,
  payment: null
});

// Enhanced error handling in API calls
const getPrice = async () => {
  try {
    setErrors(prev => ({ ...prev, price: null }));
    const response = await axios.get("https://api.coinbase.com/v2/prices/BTC-USD/spot");
    setPrice(response.data.data.amount);
    updateChartData(response.data.data.amount);
  } catch (error) {
    setErrors(prev => ({ 
      ...prev, 
      price: "Failed to fetch Bitcoin price" 
    }));
  }
};

// Display errors in UI
{errors.price && (
  <div className="error-message">
    {errors.price}
  </div>
)}
```

### Exercise 2: Loading States
Add loading indicators for better UX:

```jsx
const [loading, setLoading] = useState({
  price: false,
  balance: false,
  transactions: false,
  payment: false
});

// Loading spinner component
const LoadingSpinner = () => (
  <div className="loading-spinner">
    <div className="spinner"></div>
    <p>Loading...</p>
  </div>
);

// Use in components
{loading.balance ? (
  <LoadingSpinner />
) : (
  <p>{balance} sats</p>
)}
```

### Exercise 3: QR Code Integration
Add QR codes for Lightning invoices:

```bash
npm install qrcode.react
```

```jsx
import QRCode from 'qrcode.react';

// In PaymentsModal component
{invoice && (
  <section>
    <h3>Invoice created</h3>
    <QRCode value={invoice} size={256} />
    <p>{invoice}</p>
  </section>
)}
```

### Exercise 4: Local Storage Persistence
Save wallet settings to localStorage:

```jsx
// Save settings to localStorage
useEffect(() => {
  const settings = {
    currency: 'sats',
    updateInterval: 5000,
    theme: 'dark'
  };
  localStorage.setItem('walletSettings', JSON.stringify(settings));
}, []);

// Load settings from localStorage
useEffect(() => {
  const savedSettings = localStorage.getItem('walletSettings');
  if (savedSettings) {
    const settings = JSON.parse(savedSettings);
    // Apply settings to your app
  }
}, []);
```

### Exercise 5: Multiple Wallet Support
Extend to support multiple LNbits wallets:

```jsx
const [wallets, setWallets] = useState([]);
const [activeWallet, setActiveWallet] = useState(0);

const addWallet = (apiKey, name) => {
  setWallets(prev => [...prev, { apiKey, name, balance: 0 }]);
};

const switchWallet = (index) => {
  setActiveWallet(index);
  // Fetch data for new wallet
};
```

## Advanced React Patterns

### Custom Hooks
Create reusable logic with custom hooks:

```jsx
// Custom hook for API calls
function useApi(url, headers) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchData = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await axios.get(url, { headers });
      setData(response.data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  return { data, loading, error, fetchData };
}

// Use in components
function App() {
  const { data: balance, loading, error, fetchData } = useApi(
    'https://legend.lnbits.com/api/v1/wallet',
    { 'X-Api-Key': apiKey }
  );

  useEffect(() => {
    fetchData();
  }, []);
}
```

### Context API for Global State
For larger applications, use Context API:

```jsx
// Create context
const WalletContext = createContext();

// Provider component
export function WalletProvider({ children }) {
  const [balance, setBalance] = useState(0);
  const [transactions, setTransactions] = useState([]);

  const value = {
    balance,
    setBalance,
    transactions,
    setTransactions
  };

  return (
    <WalletContext.Provider value={value}>
      {children}
    </WalletContext.Provider>
  );
}

// Use context in components
function BalanceCard() {
  const { balance } = useContext(WalletContext);
  return <p>{balance} sats</p>;
}
```

### Memoization for Performance
Optimize expensive operations:

```jsx
import { useMemo, useCallback } from 'react';

function TransactionList({ transactions }) {
  // Memoize expensive calculations
  const totalSpent = useMemo(() => {
    return transactions
      .filter(tx => tx.amount < 0)
      .reduce((total, tx) => total + Math.abs(tx.amount), 0);
  }, [transactions]);

  // Memoize callback functions
  const handleTransactionClick = useCallback((txId) => {
    console.log('Transaction clicked:', txId);
  }, []);

  return (
    <div>
      <p>Total spent: {totalSpent} sats</p>
      {transactions.map(tx => (
        <div key={tx.id} onClick={() => handleTransactionClick(tx.id)}>
          {tx.amount} sats
        </div>
      ))}
    </div>
  );
}
```

## Performance Optimization

### Code Splitting
Split your app into smaller chunks:

```jsx
import { lazy, Suspense } from 'react';

// Lazy load heavy components
const Chart = lazy(() => import('./components/Chart'));
const PaymentsModal = lazy(() => import('./components/PaymentsModal'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading chart...</div>}>
        <Chart chartData={chartData} />
      </Suspense>
      
      <Suspense fallback={<div>Loading modal...</div>}>
        <PaymentsModal modalState={modalState} />
      </Suspense>
    </div>
  );
}
```

### React.memo for Component Optimization
Prevent unnecessary re-renders:

```jsx
import { memo } from 'react';

// Only re-render when props actually change
const TransactionItem = memo(({ transaction, onClick }) => {
  return (
    <div onClick={() => onClick(transaction.id)}>
      <p>{transaction.amount} sats</p>
      <p>{transaction.date}</p>
    </div>
  );
});

// Custom comparison function
const TransactionItem = memo(({ transaction }) => {
  return <div>{transaction.amount}</div>;
}, (prevProps, nextProps) => {
  // Only re-render if amount changed
  return prevProps.transaction.amount === nextProps.transaction.amount;
});
```

## Learning Resources

### Official React Documentation
- **[React.dev](https://react.dev/)** - Official documentation with interactive examples
- **[React Hooks Reference](https://react.dev/reference/react)** - Complete hooks documentation
- **[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/)** - Browser debugging extension

### Lightning Development Resources
- **[LNbits Documentation](https://lnbits.com/)** - Lightning wallet and account system
- **[Lightning Network Specification](https://github.com/lightning/bolts)** - Technical specifications
- **[Bitcoin Development Guide](https://developer.bitcoin.org/)** - Bitcoin development resources

### React Learning Paths
- **[FreeCodeCamp React Course](https://www.freecodecamp.org/learn/front-end-development-libraries/)** - Comprehensive free course
- **[React Tutorial](https://react.dev/learn)** - Step-by-step interactive tutorial
- **[React Patterns](https://reactpatterns.com/)** - Common patterns and best practices

### Package Documentation
- **[Axios Documentation](https://axios-http.com/)** - HTTP client for API calls
- **[React Modal](https://reactcommunity.org/react-modal/)** - Accessible modal dialogs
- **[React LineChart](https://www.npmjs.com/package/react-linechart)** - Line chart visualization

## Career and Next Steps

### Building Your Portfolio
1. **Deploy your wallet** - Get it live and share the URL
2. **Add unique features** - Multi-signature, watchtowers, channel management
3. **Write about your experience** - Blog posts, tutorials, documentation
4. **Contribute to open source** - Lightning protocol implementations
5. **Join the community** - Bitcoin developer meetups, conferences

### Advanced Lightning Development
- **BTCPay Server Integration** - Merchant payment processing
- **Lightning Node Management** - Channel opening, closing, rebalancing
- **Watchtower Implementation** - Security services for offline nodes
- **LNURL Integration** - Lightning authentication and payments
- **Multi-signature Wallets** - Enhanced security features

### React Ecosystem Expansion
- **Next.js** - Server-side rendering and full-stack React
- **React Native** - Mobile app development
- **TypeScript** - Type safety for larger applications
- **Testing** - Jest, React Testing Library, Cypress
- **State Management** - Redux, Zustand, Jotai

### Bitcoin Company Opportunities
Companies building on Lightning and Bitcoin:
- **Lightning Labs** - Lightning protocol development
- **Blockstream** - Bitcoin infrastructure
- **River Financial** - Bitcoin financial services
- **Strike** - Lightning payments
- **Fountain** - Lightning-powered podcasting
- **Zap** - Lightning wallet and services

## Key Takeaways

1. **React revolutionizes development** - Component-based architecture scales infinitely better than vanilla JavaScript
2. **State management is everything** - useState and useEffect are the foundation of dynamic React applications
3. **Component hierarchy planning matters** - Design your component tree before writing code
4. **Props flow down, events flow up** - Understanding data flow prevents bugs and confusion
5. **Hooks enable modern React** - Functional components with hooks are cleaner and more powerful
6. **Real-time updates enhance UX** - Users expect live data in Bitcoin applications
7. **Package management accelerates development** - NPM ecosystem provides solutions for most needs
8. **Error handling builds trust** - Robust error handling is crucial for financial applications
9. **Performance optimization matters** - Memoization and code splitting improve user experience
10. **Security is paramount** - Proper environment variable handling protects user funds

## Final Project Checklist

- [ ] **Bitcoin price display** - Real-time price from Coinbase API
- [ ] **Wallet balance** - Live balance from LNbits wallet
- [ ] **Transaction history** - List of sent and received payments
- [ ] **Send functionality** - Pay Lightning invoices
- [ ] **Receive functionality** - Generate Lightning invoices
- [ ] **Real-time chart** - Bitcoin price visualization
- [ ] **Responsive design** - Works on desktop and mobile
- [ ] **Error handling** - Graceful error messages
- [ ] **Loading states** - User feedback during operations
- [ ] **Environment variables** - Secure API key handling
- [ ] **Production deployment** - Live on Vercel
- [ ] **Clean code** - Well-organized components and styles

Congratulations! 🎉 You've built a fully functional Lightning wallet using React. You now have the skills to build sophisticated Bitcoin applications and join the growing ecosystem of Lightning developers.

This wallet is just the beginning. You can extend it with advanced features, integrate with other Lightning services, or use these skills to build entirely new Bitcoin applications. The Lightning Network needs more developers, and you're now equipped to contribute to this revolutionary technology.

Welcome to the Bitcoin developer community! ⚡️🧡

---

## What's Next?

You've completed the PlebDevs Frontend Course! Consider these next steps:

- **Build more Lightning apps** - Explore different use cases
- **Learn backend development** - Complete the full stack
- **Join Bitcoin development** - Contribute to open source projects
- **Start your own project** - Build the Lightning app you wish existed
- **Teach others** - Share your knowledge with the community

The journey to becoming a Bitcoin developer continues! 🚀 