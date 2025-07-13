## Lesson Slides
- [Lesson 12 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-12.pdf)
- [Lesson 12 link to slides](https://docs.google.com/presentation/d/1ByWC0wNA91zmZYKd_w7gDskv02rWZXxjgXDuAuJhL6k/edit?usp=sharing)

# Lesson 12: Connecting the Frontend

## Overview

Welcome to Lesson 12! This is the exciting finale where we bring everything together. We've built a complete backend system with API endpoints, database integration, and Lightning functionality. Now it's time to connect it to a real frontend application and see our full-stack Lightning wallet come to life!

**What We'll Cover:**
- Overview of the Pleb Wallet React frontend application
- Understanding the frontend architecture and components
- Updating the frontend to connect to our custom backend
- Setting up environment variables and configuration
- Testing the complete full-stack application
- User authentication flows (signup, login, logout)
- Testing invoice creation and payment flows
- Admin vs regular user permission testing
- Debugging frontend-backend integration issues

**Note:** This lesson represents the culmination of our entire backend course! By the end, you'll have a fully functional Lightning wallet that users can interact with through a beautiful React interface, powered by your own custom backend infrastructure.

## The Journey So Far

Before we dive into the frontend integration, let's appreciate what we've accomplished:

- ✅ **Backend Server:** Express.js API with proper routing
- ✅ **Database System:** SQLite with Knex.js migrations and seeds
- ✅ **Lightning Integration:** LND connection with invoice/payment functionality
- ✅ **User Authentication:** JWT-based auth with admin permissions
- ✅ **Database Models:** Clean abstraction layer for data operations
- ✅ **Complete API:** All endpoints tested and working with Postman/Insomnia

Now we're ready to provide a user-friendly interface that makes all this functionality accessible to real users!

## Understanding the Pleb Wallet Frontend

The Pleb Wallet is a React-based Lightning wallet interface that provides:

### Core Features

**Lightning Operations:**
- Create Lightning invoices for receiving payments
- Pay Lightning invoices for sending payments
- Real-time balance updates
- Transaction history display

**User Management:**
- User registration and login
- JWT token-based authentication
- Role-based permissions (admin vs regular users)

**Visual Interface:**
- Bitcoin price chart with real-time updates
- Transaction list with payment details
- Modal-based invoice creation and payment flows
- Clean, responsive design

### Frontend Architecture

The Pleb Wallet follows a simple but effective React architecture:

```
src/
├── App.js              # Main application component
├── components/
│   ├── Header.js       # Navigation and user info
│   ├── Buttons.js      # Send/Receive action buttons
│   ├── Chart.js        # Bitcoin price chart
│   ├── Transactions.js # Transaction history display
│   └── PaymentsModal.js # Invoice creation/payment modal
└── utils/
    └── axiosWithAuth.js # HTTP client with JWT auth
```

**Key Concepts:**
- **Single Page Application (SPA):** All functionality in one page
- **Component-Based:** Modular React components for different features
- **State Management:** React hooks for managing application state
- **API Integration:** Axios for HTTP requests to our backend
- **Real-time Updates:** Periodic polling for fresh data

## Getting the Updated Frontend

### Option 1: Clone the Updated Repository

The easiest way to get started is to clone the pre-updated frontend:

```bash
git clone https://github.com/plebdevs/pleb-wallet-frontend.git
cd pleb-wallet-frontend
npm install
```

### Option 2: Update Your Existing Frontend

If you have the original frontend from Course #1, you can update it manually by following the changes we'll outline in this lesson.

## Frontend Code Walkthrough

Let's examine the key components and understand how they work with our backend.

### App.js - The Main Application

The heart of our application contains all the core logic:

```javascript
import React, { useState, useEffect } from 'react';
import axiosWithAuth from './utils/axiosWithAuth';
import Header from './components/Header';
import Buttons from './components/Buttons';
import Chart from './components/Chart';
import Transactions from './components/Transactions';
import PaymentsModal from './components/PaymentsModal';

const App = () => {
  // State for various data
  const [price, setPrice] = useState(0);
  const [walletBalance, setWalletBalance] = useState(0);
  const [channelBalance, setChannelBalance] = useState(0);
  const [transactions, setTransactions] = useState([]);
  const [chartData, setChartData] = useState([]);
  const [user, setUser] = useState(null);
  const [modalState, setModalState] = useState('');

  // Backend URL from environment variables
  const backendUrl = process.env.REACT_APP_BACKEND_URL;

  // Authentication token handling
  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      // Get user info when token exists
      getUserInfo();
    }
  }, []);

  // Function to get Bitcoin price from Coinbase API
  const getPrice = () => {
    fetch("https://api.coinbase.com/v2/exchange-rates?currency=BTC")
      .then(res => res.json())
      .then(data => {
        setPrice(data.data.rates.USD);
      });
  };

  // Function to get wallet balance from our backend
  const getWalletBalance = () => {
    axiosWithAuth()
      .get(`${backendUrl}/lightning/balance`)
      .then(res => {
        setWalletBalance(res.data.total_balance);
      })
      .catch(err => console.log(err));
  };

  // Function to get channel balance from our backend
  const getChannelBalance = () => {
    axiosWithAuth()
      .get(`${backendUrl}/lightning/channelbalance`)
      .then(res => {
        setChannelBalance(res.data.balance);
      })
      .catch(err => console.log(err));
  };

  // Function to get transactions from our backend
  const getTransactions = () => {
    axiosWithAuth()
      .get(`${backendUrl}/lightning/invoices`)
      .then(res => {
        setTransactions(res.data);
      })
      .catch(err => console.log(err));
  };

  // Function to get user info
  const getUserInfo = () => {
    axiosWithAuth()
      .get(`${backendUrl}/users/user`)
      .then(res => {
        setUser(res.data);
      })
      .catch(err => console.log(err));
  };

  // Initial data loading
  useEffect(() => {
    getPrice();
    getWalletBalance();
    getChannelBalance();
    getTransactions();
  }, []);

  // Periodic updates
  useEffect(() => {
    const interval = setInterval(() => {
      getPrice();
      getWalletBalance();
      getChannelBalance();
      getTransactions();
    }, 30000); // Update every 30 seconds

    return () => clearInterval(interval);
  }, []);

  return (
    <div className="App">
      <Header user={user} setUser={setUser} />
      <div className="main-content">
        <div className="balance-section">
          <h2>Wallet Balance: {walletBalance} sats</h2>
          <h3>Channel Balance: {channelBalance} sats</h3>
          <h3>Bitcoin Price: ${price}</h3>
        </div>
        <Buttons setModalState={setModalState} />
        <Chart chartData={chartData} />
        <Transactions transactions={transactions} />
      </div>
      <PaymentsModal 
        modalState={modalState} 
        setModalState={setModalState}
        user={user}
        backendUrl={backendUrl}
      />
    </div>
  );
};

export default App;
```

### axiosWithAuth.js - Authenticated HTTP Client

This utility handles JWT authentication for API requests:

```javascript
import axios from 'axios';

const axiosWithAuth = () => {
  const backendUrl = process.env.REACT_APP_BACKEND_URL;
  const token = localStorage.getItem('token');

  return axios.create({
    baseURL: backendUrl,
    headers: {
      authorization: token,
    },
  });
};

export default axiosWithAuth;
```

**Key Features:**
- **Automatic Token Inclusion:** Adds JWT token to all requests
- **Base URL Configuration:** Uses environment variable for backend URL
- **Consistent Interface:** Same API as regular Axios but with auth

### PaymentsModal.js - Invoice and Payment Interface

This component handles both creating invoices and paying them:

```javascript
import React, { useState } from 'react';
import axiosWithAuth from '../utils/axiosWithAuth';

const PaymentsModal = ({ modalState, setModalState, user, backendUrl }) => {
  const [amount, setAmount] = useState('');
  const [invoice, setInvoice] = useState('');
  const [paymentRequest, setPaymentRequest] = useState('');
  const [paymentResult, setPaymentResult] = useState(null);

  // Handle creating an invoice
  const handleReceive = () => {
    axiosWithAuth()
      .post(`${backendUrl}/lightning/invoice`, {
        value: parseInt(amount),
        memo: 'Pleb Wallet Invoice',
        user_id: user.id
      })
      .then(res => {
        setPaymentRequest(res.data.payment_request);
      })
      .catch(err => console.log(err));
  };

  // Handle paying an invoice
  const handleSend = () => {
    axiosWithAuth()
      .post(`${backendUrl}/lightning/pay`, {
        payment_request: invoice,
        user_id: user.id
      })
      .then(res => {
        setPaymentResult(res.data);
      })
      .catch(err => {
        console.log(err);
        alert('Payment failed - you may not have admin permissions');
      });
  };

  // Clear form data
  const clearForm = () => {
    setAmount('');
    setInvoice('');
    setPaymentRequest('');
    setPaymentResult(null);
  };

  if (!modalState) return null;

  return (
    <div className="modal-overlay">
      <div className="modal">
        {modalState === 'receive' && (
          <div>
            <h3>Receive Payment</h3>
            {!paymentRequest ? (
              <div>
                <input
                  type="number"
                  placeholder="Amount in sats"
                  value={amount}
                  onChange={(e) => setAmount(e.target.value)}
                />
                <button onClick={handleReceive}>Create Invoice</button>
              </div>
            ) : (
              <div>
                <p>Payment Request:</p>
                <textarea value={paymentRequest} readOnly />
                <button onClick={() => navigator.clipboard.writeText(paymentRequest)}>
                  Copy Invoice
                </button>
              </div>
            )}
          </div>
        )}

        {modalState === 'send' && (
          <div>
            <h3>Send Payment</h3>
            {!paymentResult ? (
              <div>
                <textarea
                  placeholder="Paste lightning invoice here"
                  value={invoice}
                  onChange={(e) => setInvoice(e.target.value)}
                />
                <button onClick={handleSend}>Pay Invoice</button>
              </div>
            ) : (
              <div>
                <p>Payment Successful!</p>
                <p>Amount: {paymentResult.value} sats</p>
                <p>Fees: {paymentResult.fees} sats</p>
              </div>
            )}
          </div>
        )}

        <button onClick={() => {
          setModalState('');
          clearForm();
        }}>
          Close
        </button>
      </div>
    </div>
  );
};

export default PaymentsModal;
```

## Setting Up the Frontend

### Prerequisites

Before setting up the frontend, ensure you have:

1. **Backend Running:** Your Pleb Wallet backend from previous lessons
2. **Lightning Network:** Polar running with your test network
3. **Node.js:** Version 14 or higher
4. **Git:** For cloning the repository

### Step 1: Clone and Install

```bash
# Clone the updated frontend
git clone https://github.com/plebdevs/pleb-wallet-frontend.git
cd pleb-wallet-frontend

# Install dependencies
npm install
```

### Step 2: Environment Configuration

Create a `.env` file in the root directory:

```bash
# .env file
REACT_APP_BACKEND_URL=http://localhost:5500
```

**Important Environment Variables:**
- `REACT_APP_BACKEND_URL`: The URL where your backend server is running
- Must start with `REACT_APP_` for React to recognize it
- Use `http://localhost:5500` for local development

### Step 3: Start the Development Server

```bash
npm start
```

The frontend will start on `http://localhost:3000` and automatically open in your browser.

## Backend Preparation

Before testing the frontend, ensure your backend is properly configured:

### Step 1: Update Rate Limiting

The frontend makes frequent API calls, so we need to increase the rate limit:

```javascript
// In index.js, update the rate limiter
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 1000, // Increased from 100 to 1000 requests per windowMs
  message: 'Too many requests from this IP, please try again later.'
});
```

### Step 2: Ensure CORS is Configured

Make sure your backend allows requests from the frontend:

```javascript
// In index.js, ensure CORS is properly configured
app.use(cors({
  origin: ['http://localhost:3000', 'http://localhost:3001'],
  credentials: true
}));
```

### Step 3: Start All Services

Start your services in this order:

```bash
# Terminal 1: Start Docker Desktop and Polar
# Open Docker Desktop
# Start Polar and your Lightning network

# Terminal 2: Start Backend
cd pleb-wallet-backend
npm run start

# Terminal 3: Start Frontend
cd pleb-wallet-frontend
npm start
```

## Testing the Complete System

Now let's test the entire full-stack application with real user scenarios.

### Test Scenario 1: New User Registration and Invoice Creation

**Step 1: Create a New User**

1. Open `http://localhost:3000` in your browser
2. Click "Sign Up"
3. Enter username: `testuser`
4. Enter password: `testpass`
5. Click "Sign Up"
6. You should see: "Your user was successfully created, you can now log in"

**Step 2: Login**

1. Click "Login"
2. Enter the same credentials
3. Click "Login"
4. You should see: "Welcome testuser" in the header

**Step 3: Create an Invoice**

1. Click "Receive"
2. Enter amount: `150`
3. Click "Create Invoice"
4. You should see a payment request and QR code

**Step 4: Pay the Invoice (Using Polar)**

1. Copy the payment request
2. Open Polar
3. Click on Bob's node
4. Click "Pay Invoice"
5. Paste the payment request
6. Click "Pay Invoice"
7. Return to the frontend - you should see the payment appear in transactions

### Test Scenario 2: Admin User Payment Flow

**Step 1: Login as Admin**

1. Logout from the current user
2. Login with admin credentials:
   - Username: `Alice`
   - Password: `pass1`
3. You should see: "Welcome Alice"

**Step 2: Create an Invoice in Polar**

1. Open Polar
2. Click on Bob's node
3. Click "Create Invoice"
4. Set amount: `1000`
5. Set memo: "Test payment to Bob"
6. Click "Create Invoice"
7. Copy the payment request

**Step 3: Pay the Invoice from Frontend**

1. Return to the frontend
2. Click "Send"
3. Paste the payment request
4. Click "Pay Invoice"
5. You should see the payment processed successfully
6. The transaction should appear in your transaction list

### Test Scenario 3: Permission Testing

**Step 1: Test Regular User Sending (Should Fail)**

1. Login as a regular user (not admin)
2. Try to send a payment
3. You should get an error: "Request failed with status 401"
4. This confirms that only admin users can send payments

**Step 2: Test Regular User Receiving (Should Work)**

1. Create an invoice as a regular user
2. Pay it from Polar
3. The payment should be received successfully
4. This confirms that regular users can receive payments

## Understanding the Data Flow

### Frontend to Backend Communication

```
1. User Action → React Event Handler → API Call
2. API Call → Backend Route → Middleware → Controller
3. Controller → Database/Lightning → Response
4. Response → Frontend State Update → UI Update
```

### Example: Creating an Invoice

```javascript
// 1. User clicks "Create Invoice"
const handleReceive = () => {
  // 2. Frontend makes API call with JWT token
  axiosWithAuth()
    .post(`${backendUrl}/lightning/invoice`, {
      value: parseInt(amount),
      memo: 'Pleb Wallet Invoice',
      user_id: user.id
    })
    .then(res => {
      // 6. Update frontend state with invoice data
      setPaymentRequest(res.data.payment_request);
    })
    .catch(err => console.log(err));
};

// 3. Backend receives request → authenticate middleware
// 4. Create invoice with LND → Save to database
// 5. Return invoice data to frontend
```

### Real-time Updates

The frontend polls the backend every 30 seconds for updates:

```javascript
useEffect(() => {
  const interval = setInterval(() => {
    getPrice();           // Bitcoin price from Coinbase
    getWalletBalance();   // On-chain balance from LND
    getChannelBalance();  // Lightning balance from LND
    getTransactions();    // Transaction history from database
  }, 30000);

  return () => clearInterval(interval);
}, []);
```

## Common Issues and Troubleshooting

### CORS Errors

**Error:** `Access to XMLHttpRequest at 'http://localhost:5500' from origin 'http://localhost:3000' has been blocked by CORS policy`

**Solution:** Ensure CORS is properly configured in your backend:

```javascript
app.use(cors({
  origin: ['http://localhost:3000'],
  credentials: true
}));
```

### Environment Variable Issues

**Error:** `Cannot read property 'REACT_APP_BACKEND_URL' of undefined`

**Solution:** 
1. Ensure your `.env` file is in the root directory
2. Restart the React development server
3. Check that the variable name starts with `REACT_APP_`

### Authentication Errors

**Error:** `Request failed with status 401`

**Solution:**
1. Check that you're logged in
2. Verify the JWT token is stored in localStorage
3. Ensure the backend JWT secret matches
4. Check that the user has appropriate permissions

### Rate Limiting Issues

**Error:** `Too many requests from this IP`

**Solution:** Increase the rate limit in your backend:

```javascript
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 1000, // Increase this number
  message: 'Too many requests from this IP, please try again later.'
});
```

### Lightning Network Issues

**Error:** `No route found` or `Unable to find path`

**Solution:**
1. Ensure Polar is running
2. Check that nodes have channels with sufficient balance
3. Verify your backend is connected to the correct LND node

## Frontend Customization Ideas

The current frontend is a basic template. Here are some enhancement ideas:

### UI/UX Improvements

**QR Code Generation:**
```javascript
// Add QR code display for invoices
import QRCode from 'qrcode.react';

<QRCode value={paymentRequest} size={200} />
```

**Loading Spinners:**
```javascript
const [loading, setLoading] = useState(false);

const handleReceive = () => {
  setLoading(true);
  axiosWithAuth()
    .post(`${backendUrl}/lightning/invoice`, data)
    .then(res => {
      setPaymentRequest(res.data.payment_request);
      setLoading(false);
    });
};
```

**Real-time Invoice Updates:**
```javascript
// WebSocket connection for real-time payment notifications
useEffect(() => {
  const ws = new WebSocket('ws://localhost:5500');
  
  ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    if (data.type === 'invoice_paid') {
      getTransactions(); // Refresh transactions
    }
  };
}, []);
```

### Feature Additions

**Transaction Filtering:**
```javascript
const [filter, setFilter] = useState('all');

const filteredTransactions = transactions.filter(tx => {
  if (filter === 'sent') return tx.send;
  if (filter === 'received') return !tx.send;
  return true;
});
```

**Balance History Chart:**
```javascript
// Store balance history in database
const [balanceHistory, setBalanceHistory] = useState([]);

// Update chart with balance over time instead of just price
```

**Invoice Expiration:**
```javascript
// Add expiration time to invoices
const [expirationTime, setExpirationTime] = useState(null);

useEffect(() => {
  if (paymentRequest) {
    const timer = setTimeout(() => {
      setPaymentRequest('');
      alert('Invoice expired');
    }, 600000); // 10 minutes

    return () => clearTimeout(timer);
  }
}, [paymentRequest]);
```

## Responsive Design

The current frontend works on desktop but could be improved for mobile:

```css
/* Add responsive breakpoints */
@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }
  
  .chart-container {
    width: 100%;
    height: 300px;
  }
  
  .modal {
    width: 90%;
    max-width: 400px;
  }
}
```

## Security Considerations

### Frontend Security Best Practices

**Token Storage:**
```javascript
// Consider using secure storage for JWT tokens
// Current: localStorage (simple but less secure)
// Better: httpOnly cookies or secure storage libraries
```

**Input Validation:**
```javascript
const validateAmount = (amount) => {
  const num = parseInt(amount);
  if (isNaN(num) || num <= 0) {
    throw new Error('Invalid amount');
  }
  return num;
};
```

**Error Handling:**
```javascript
// Don't expose sensitive error details to users
.catch(err => {
  console.log(err); // Log for debugging
  alert('Payment failed. Please try again.'); // Generic user message
});
```

## Performance Optimization

### API Call Optimization

**Batch Requests:**
```javascript
// Instead of multiple individual calls
const getInitialData = async () => {
  const [price, balance, channelBalance, transactions] = await Promise.all([
    fetch('https://api.coinbase.com/v2/exchange-rates?currency=BTC'),
    axiosWithAuth().get(`${backendUrl}/lightning/balance`),
    axiosWithAuth().get(`${backendUrl}/lightning/channelbalance`),
    axiosWithAuth().get(`${backendUrl}/lightning/invoices`)
  ]);
  
  // Process all responses together
};
```

**Smart Polling:**
```javascript
// Only poll when tab is active
useEffect(() => {
  const handleVisibilityChange = () => {
    if (document.hidden) {
      clearInterval(pollInterval);
    } else {
      startPolling();
    }
  };
  
  document.addEventListener('visibilitychange', handleVisibilityChange);
  return () => document.removeEventListener('visibilitychange', handleVisibilityChange);
}, []);
```

## Testing the Full Stack

Here's a comprehensive test checklist:

### Basic Functionality Tests

- [ ] Frontend loads without errors
- [ ] Bitcoin price displays correctly
- [ ] Wallet balances display correctly
- [ ] Transaction history displays correctly

### Authentication Tests

- [ ] User can sign up
- [ ] User can log in
- [ ] User can log out
- [ ] JWT token is stored and used correctly
- [ ] Protected routes work properly

### Lightning Functionality Tests

- [ ] User can create invoices
- [ ] Invoices can be paid externally (via Polar)
- [ ] Payments appear in transaction history
- [ ] Admin users can pay invoices
- [ ] Regular users cannot pay invoices
- [ ] Invoice amounts are correct
- [ ] Transaction details are accurate

### Error Handling Tests

- [ ] Invalid login credentials show error
- [ ] Network errors are handled gracefully
- [ ] Invalid payment requests show error
- [ ] Insufficient balance errors work
- [ ] Rate limiting works correctly

### Performance Tests

- [ ] Page loads quickly
- [ ] API calls complete in reasonable time
- [ ] Chart renders smoothly
- [ ] Modal interactions are responsive

## Deployment Considerations

While we'll cover deployment in the next lesson, here are some frontend deployment considerations:

### Environment Variables for Production

```bash
# Production .env
REACT_APP_BACKEND_URL=https://your-backend-domain.com
REACT_APP_ENVIRONMENT=production
```

### Build Optimization

```bash
# Create production build
npm run build

# This creates a 'build' folder with optimized static files
```

### Hosting Options

**Static Site Hosting:**
- Vercel (recommended)
- Netlify
- GitHub Pages
- AWS S3 + CloudFront

**Server-Side Rendering:**
- Next.js (if converting to SSR)
- Gatsby (for static generation)

## What's Next?

Congratulations! You now have a complete full-stack Lightning wallet application running locally. In the next lesson, we'll:

1. **Deploy the Backend:** Set up your server on a cloud provider
2. **Deploy the Lightning Node:** Configure a real Lightning node
3. **Deploy the Frontend:** Host your React app on Vercel
4. **Connect Everything:** Make your deployed services work together
5. **Domain Setup:** Configure custom domains
6. **SSL Certificates:** Secure your application with HTTPS
7. **Monitoring:** Set up logging and monitoring
8. **Backup Strategies:** Protect your data and funds

## Key Takeaways

1. **Full-Stack Integration:** Successfully connecting frontend and backend requires careful attention to CORS, authentication, and API contracts
2. **Real-time Updates:** User interfaces need to stay synchronized with backend state through polling or WebSockets
3. **Error Handling:** Robust error handling is crucial for good user experience
4. **Security:** Frontend security depends on proper token handling and input validation
5. **Performance:** Optimize API calls and use smart polling to reduce server load
6. **Testing:** Comprehensive testing ensures all user flows work correctly
7. **Customization:** The basic template can be extensively customized for your specific needs

## Practice Exercises

1. **Add QR Code Generation:**
   - Install a QR code library
   - Display QR codes for generated invoices
   - Add click-to-copy functionality

2. **Implement Real-time Updates:**
   - Add WebSocket connection to backend
   - Update UI immediately when invoices are paid
   - Show live payment notifications

3. **Enhanced Transaction Display:**
   - Add transaction filtering (sent/received)
   - Implement pagination for large transaction lists
   - Add transaction search functionality

4. **Mobile Responsiveness:**
   - Improve mobile layout
   - Add touch-friendly interactions
   - Optimize for different screen sizes

5. **Advanced Features:**
   - Add invoice expiration countdown
   - Implement balance history chart
   - Add user profile management

## Resources

### Frontend Development
- [React Documentation](https://react.dev/) - Complete React reference
- [Axios Documentation](https://axios-http.com/) - HTTP client library
- [React Router](https://reactrouter.com/) - For multi-page applications

### Lightning Development
- [Lightning Network Specifications](https://github.com/lightning/bolts) - Technical specs
- [LND API Reference](https://lightning.engineering/api-docs/) - Complete API documentation
- [Lightning Design Guidelines](https://lightning.engineering/guides/) - Best practices

### Deployment Resources
- [Vercel Documentation](https://vercel.com/docs) - Frontend deployment
- [Netlify Documentation](https://docs.netlify.com/) - Alternative hosting
- [React Build Process](https://create-react-app.dev/docs/production-build/) - Production optimization

### Security Resources
- [OWASP JavaScript Security](https://owasp.org/www-project-top-ten/) - Security best practices
- [JWT Best Practices](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/) - Token security
- [React Security](https://pragmaticwebsecurity.com/articles/spasecurity.html) - Frontend security guide

You've now completed the full backend course! Your Lightning wallet application is functional, tested, and ready for deployment. The next lesson will take you through deploying everything to production, making your application accessible to users worldwide.

Remember: This is just the beginning. The patterns and techniques you've learned here can be applied to build any Lightning-enabled application. The Lightning ecosystem is growing rapidly, and you're now equipped to be part of that growth! 