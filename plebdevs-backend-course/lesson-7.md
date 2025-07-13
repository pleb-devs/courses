## Lesson Slides
- [Lesson 7 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-7.pdf)
- [Lesson 7 link to slides](https://docs.google.com/presentation/d/1SRjrewHGZ61dbucV4u7_9wpKM24R0v4O6poXVeHD3bY/edit?usp=sharing)

# Lesson 7: Building on LND

## Overview

Welcome to Lesson 7! This is where we transition from theory to practice with the Lightning Network. Building on the foundational knowledge from Lesson 6, we'll now write code to communicate with our Lightning node and create our first Lightning-powered backend methods.

**What We'll Cover:**
- Understanding what makes a Lightning app
- Connecting to your Lightning node with gRPC
- Working with the LND API
- Building essential LND methods (balance, invoices, payments)
- Implementing real-time invoice event streams
- Testing our Lightning functionality

**Note:** This is a hands-on coding lesson where we'll move real (fake) money with our own code!

## What is a Lightning App?

A Lightning App is just like any other application but with Lightning added. However, this opens up many architectural decisions:

### Key Questions to Consider

When building a Lightning app, you need to decide:

1. **Node Management**
   - Does your app need a dedicated node?
   - Will you use a Lightning Service Provider (LSP)?
   - Will users run their own nodes?

2. **Wallet Custody**
   - Will users get a custodial wallet from you?
   - Will they connect their own wallet?
   - Will they get a non-custodial wallet?
   - Will they even know they're using Lightning?

3. **User Experience**
   - How much Lightning complexity do you expose?
   - What trade-offs will you make for ease of use?

### Our Architecture

For the Pleb Wallet, we're building:
- **One hosted node** (Alice in our Polar setup)
- **Multiple users** interacting with our wallet
- **Custodial setup** where only we (admins) can spend
- **Users can create invoices** and receive payments

## Typical Full Stack Lightning App Architecture

```
┌─────────────────────────────────────────────┐
│                Frontend                     │
│        (User Interface & Experience)       │
└─────────────────────────────────────────────┘
                        ↕
┌─────────────────────────────────────────────┐
│                   API                       │
│    (HTTP Communication Layer)               │
│  • GET, POST, UPDATE, DELETE                │
│  • Call Lightning node methods              │
│  • Handle authentication                    │
└─────────────────────────────────────────────┘
                        ↕
┌─────────────────────────────────────────────┐
│                Database                     │
│     (Optional - App Data Storage)           │
│  • User data                               │
│  • Invoice records                         │
│  • Payment history                         │
└─────────────────────────────────────────────┘
                        ↕
┌─────────────────────────────────────────────┐
│            Lightning Node                   │
│    (Wallet, Network Node, Payment DB)      │
│  • Manages Bitcoin/Lightning funds          │
│  • Connects to Lightning Network            │
│  • Processes payments                       │
└─────────────────────────────────────────────┘
```

## Common Lightning Development Hurdles

### 1. Running a Node
- **Challenge:** Hosting, maintaining, and keeping nodes operational
- **Solutions:** 
  - Voltage (managed Lightning infrastructure)
  - Umbrel (self-hosted solution)

### 2. Talking to Your Node
- **Challenge:** Understanding how to communicate with Lightning nodes
- **Solutions:**
  - gRPC wrappers (LND-GRPC, ln-service)
  - Pre-built interfaces (LNBits, RTL, WebLN, LNC)

### 3. Development Environment
- **Challenge:** Safe environment for testing without real money
- **Solutions:**
  - Polar (what we're using)
  - Workbench (CLI alternative)

### 4. Getting Liquidity
- **Challenge:** Opening channels and managing Lightning liquidity
- **Solutions:**
  - FLOW by Voltage
  - Magma by Amboss

## Understanding gRPC

### What is gRPC?

**gRPC** stands for "gRPC Remote Procedure Calls" - a modern, high-performance framework for machine-to-machine communication.

#### Key Features:
- **Binary Protocol:** More efficient than JSON/HTTP
- **Strongly Typed:** Reduces errors and ensures consistency
- **Streaming Support:** Real-time data pipelines
- **Multi-Language:** Works with JavaScript, Python, Go, etc.
- **Connection Persistence:** No request/response handshake needed

#### gRPC vs HTTP Comparison:

| Feature | HTTP/REST | gRPC |
|---------|-----------|------|
| Data Format | JSON (text) | Protocol Buffers (binary) |
| Connection | Request/Response | Persistent streaming |
| Real-time | Requires polling | Native streaming |
| Performance | Good | Excellent |
| Complexity | Simple | Moderate |

### Benefits of gRPC Wrapper Libraries

Instead of writing raw gRPC code like this:
```javascript
// Raw gRPC - 40+ lines just to call getInfo!
const grpc = require('grpc');
const protoLoader = require('@grpc/proto-loader');
// ... 35+ more lines of boilerplate ...
client.getInfo(request, (err, response) => {
  console.log(response);
});
```

We can use LND-GRPC wrapper:
```javascript
// With LND-GRPC wrapper - simple and clean!
const info = await lnd.services.Lightning.getInfo();
```

## Setting Up LND Connection

### Step 1: Get Connection Credentials

From your Polar Alice node, go to the **Connect** tab and gather:

1. **HOST:** gRPC Host (e.g., `127.0.0.1:10001`)
2. **CERT:** TLS Cert File Path
3. **MACAROON:** Admin Macaroon File Path

### Step 2: Add to Environment Variables

Add these to your `.env` file:
```env
# Lightning node connection
HOST=127.0.0.1:10001
CERT=/path/to/your/tls.cert
MACAROON=/path/to/your/admin.macaroon
```

**Security Note:** These credentials provide full admin access to your node. Never commit them to version control!

### Step 3: Install LND-GRPC

```bash
npm install lnd-grpc
```

### Step 4: Create LND Connection File

Create `lnd.js` in your project root:

```javascript
const LndGrpc = require("lnd-grpc");
const dotenv = require("dotenv");

dotenv.config();

const options = {
  host: process.env.HOST,
  cert: process.env.CERT,
  macaroon: process.env.MACAROON,
};

const lnd = new LndGrpc(options);

const connect = async () => {
  try {
    await lnd.connect();

    if (lnd.state !== "active") {
      throw new Error(
        "LND did not reach 'active' state within the expected time"
      );
    }

    console.log(`LND gRPC connection state: ${lnd.state}`);
  } catch (e) {
    console.log("error", e);
  }
};

module.exports = { connect };
```

### Step 5: Connect on Server Startup

In your `index.js`, add the connection:

```javascript
const { connect } = require("./lnd");

// ... other middleware ...
server.use(express.json());

// Connect to our LND node
connect();
```

## Building LND Methods

### Balance Methods

Add these methods to your `lnd.js`:

```javascript
/**
 * Get the on-chain Bitcoin balance
 * @returns {Promise<Object>} Wallet balance information
 */
const getBalance = async () => {
  const balance = await lnd.services.Lightning.walletBalance();
  return balance;
};

/**
 * Get the Lightning channel balance
 * @returns {Promise<Object>} Channel balance information
 */
const getChannelBalance = async () => {
  const channelBalance = await lnd.services.Lightning.channelBalance();
  return channelBalance;
};

module.exports = {
  connect,
  getBalance,
  getChannelBalance,
};
```

### Invoice Creation Method

```javascript
/**
 * Create a Lightning invoice
 * @param {number} value - Amount in satoshis
 * @param {string} memo - Invoice description
 * @returns {Promise<Object>} Invoice object with payment_request
 */
const createInvoice = async ({ value, memo }) => {
  const invoice = await lnd.services.Lightning.addInvoice({
    value: value,
    memo: memo,
  });

  // TODO: Save invoice to database
  
  return invoice;
};
```

### Payment Method

```javascript
/**
 * Pay a Lightning invoice
 * @param {string} payment_request - Lightning invoice to pay
 * @returns {Promise<Object>} Payment result
 */
const payInvoice = async ({ payment_request }) => {
  const paidInvoice = await lnd.services.Lightning.sendPaymentSync({
    payment_request: payment_request,
  });

  return paidInvoice;
};
```

## Real-Time Invoice Updates with Event Streams

### Understanding Event Streams

Event streams provide real-time updates without polling:

- **Traditional Polling:** Ask "Is my invoice paid?" every second
- **Event Streams:** Get notified instantly when invoice is paid

### Implementing Invoice Event Stream

```javascript
/**
 * Subscribe to invoice events for real-time updates
 * Listens for when invoices are created, paid, or expire
 */
const invoiceEventStream = async () => {
  await lnd.services.Lightning.subscribeInvoices({
    add_index: 0,    // Start from beginning of invoice history
    settle_index: 0, // Start from beginning of settlement history
  })
    .on("data", async (data) => {
      if (data.settled) {
        console.log("Invoice settled:", data);
        
        // Check if invoice exists in database
        const existingInvoice = false; // TODO: Check database
        
        if (existingInvoice) {
          // TODO: Update invoice status in database
          console.log("Updating invoice in database");
        } else {
          console.log("Invoice not found in database");
        }
      }
    })
    .on("error", (err) => {
      console.error("Invoice stream error:", err);
    });
};
```

### Start Event Stream on Connection

Update your `connect` function:

```javascript
const connect = async () => {
  try {
    await lnd.connect();

    if (lnd.state !== "active") {
      throw new Error(
        "LND did not reach 'active' state within the expected time"
      );
    }

    // Start the invoice event stream
    invoiceEventStream();

    console.log(`LND gRPC connection state: ${lnd.state}`);
  } catch (e) {
    console.log("error", e);
  }
};
```

## Adding Lightning Routes

### Import LND Methods

In your `lightningRouter.js`:

```javascript
const {
  getBalance,
  createInvoice,
  getChannelBalance,
  payInvoice,
} = require("../lnd.js");
```

### Balance Endpoints

```javascript
// GET the on-chain balance
router.get("/balance", (req, res) => {
  getBalance()
    .then((balance) => {
      res.status(200).json(balance);
    })
    .catch((err) => {
      res.status(500).json(err);
    });
});

// GET the Lightning channel balance
router.get("/channelbalance", (req, res) => {
  getChannelBalance()
    .then((channelBalance) => {
      res.status(200).json(channelBalance);
    })
    .catch((err) => {
      res.status(500).json(err);
    });
});
```

### Invoice Endpoints

```javascript
// POST - Create a new invoice
router.post("/invoice", authenticate, (req, res) => {
  const { value, memo } = req.body;

  createInvoice({ value, memo })
    .then((invoice) => {
      res.status(200).json(invoice);
    })
    .catch((err) => {
      res.status(500).json(err);
    });
});

// POST - Pay an invoice (admin only)
router.post("/pay", authenticateAdmin, async (req, res) => {
  const { payment_request } = req.body;

  try {
    const pay = await payInvoice({ payment_request });

    if (pay.payment_error) {
      return res.status(500).json(pay.payment_error);
    }

    if (pay?.payment_route) {
      // TODO: Save successful payment to database
      res.status(200).json(pay);
    }
  } catch (err) {
    res.status(500).json(err);
  }
});
```

## Testing Your Lightning Integration

### Using Insomnia/Postman

1. **Start your server** and ensure Polar network is running
2. **Login** to get a JWT token
3. **Test Balance Endpoints:**
   - GET `/lightning/balance`
   - GET `/lightning/channelbalance`

4. **Test Invoice Creation:**
   - POST `/lightning/invoice`
   - Body: `{ "value": 1000, "memo": "Test invoice" }`

5. **Test Payment:**
   - Create invoice in Polar (Bob creates invoice)
   - POST `/lightning/pay`
   - Body: `{ "payment_request": "lnbc..." }`

### Understanding Balance Types

- **On-chain Balance:** Bitcoin sitting on the blockchain
- **Channel Balance:** 
  - `local_balance`: Sats you can send
  - `remote_balance`: Sats you can receive

### Troubleshooting Common Issues

1. **Connection Failed**
   - Check Polar network is running
   - Verify credentials in `.env`
   - Check file paths are correct

2. **Invoice Creation Fails**
   - Ensure you're authenticated
   - Check value is a number
   - Verify memo is a string

3. **Payment Fails**
   - Check you have channel balance
   - Verify invoice is valid
   - Ensure route exists between nodes

## Key Takeaways

1. **gRPC is Powerful:** Enables real-time communication with Lightning nodes
2. **Wrapper Libraries Help:** LND-GRPC reduces complexity significantly
3. **Event Streams Are Essential:** Real-time updates without polling
4. **Security Matters:** Macaroons provide fine-grained access control
5. **Testing is Key:** Use Polar for safe development environment

## What's Next?

In the next lesson, we'll:
- Set up a proper database to store our invoice and payment data
- Learn SQL fundamentals
- Integrate our Lightning methods with persistent storage
- Build a complete data flow from invoice creation to payment confirmation

## Practice Exercises

1. **Explore the LND API:** Browse the [LND documentation](https://lightning.engineering/api-docs/) and try calling different methods
2. **Create Multiple Invoices:** Test creating invoices with different values and memos
3. **Test Payment Routes:** Create invoices on different nodes and pay them
4. **Monitor Event Streams:** Watch the console logs when invoices are paid
5. **Experiment with Channels:** Open new channels in Polar and test routing

## Resources

### Essential Reading
- [LND gRPC Documentation](https://lightning.engineering/api-docs/)
- [LND-GRPC NPM Package](https://www.npmjs.com/package/lnd-grpc)
- [Protocol Buffers Documentation](https://developers.google.com/protocol-buffers)

### Video Resources
- [Build Bitcoin into Your App: Getting Started with the Lightning Network](https://www.youtube.com/watch?v=6P0DZ74DmFA)
- [LND Overview and Developer Guide](https://dev.lightning.community/overview/)

### Development Tools
- [Polar Lightning](https://polarlightning.com) - Local Lightning development
- [Voltage](https://voltage.cloud) - Managed Lightning infrastructure
- [Insomnia](https://insomnia.rest) - API testing tool

### Lightning Development Resources
- [Lightning Labs Build Your First LAPP](https://docs.lightning.engineering/lapps/guides/polar-lapps)
- [A crash course in Lightning App Development](https://medium.com/@rheedio/a-crash-course-in-lightning-app-development-5be5b8d2d558)
- [Express / React Lightning app template](https://github.com/AustinKelsay/pleb-node-template)

Remember: This is just the beginning! Lightning development opens up incredible possibilities for micropayments, instant settlements, and innovative financial applications. Take your time to understand these concepts - they're the foundation for everything we'll build going forward. 