## Lesson Slides
- [Lesson 13 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-13.pdf)
- [Lesson 13 link to slides](https://docs.google.com/presentation/d/10Nn6H5QXXrzyulw43hgIPDXCLcsPsyUzkdgXjrdQqJE/edit?usp=sharing)

# Lesson 13: Deploy Your Production Database and Node

## Overview

Welcome to Lesson 13 - the grand finale of our backend course! This is where we take everything we've built locally and deploy it to the cloud, creating a real production Lightning wallet that anyone can access and use.

**What We'll Cover:**
- Considerations before deploying to production
- Deploying your server and database to Heroku
- Setting up a production Lightning node on Voltage
- Connecting your production services together
- Deploying the frontend application to Vercel
- Testing the complete production system
- Security considerations and best practices
- Cost analysis and optimization
- Troubleshooting common deployment issues

**Warning:** We'll be dealing with real money and real Lightning networks in this lesson. We'll discuss security considerations and provide options for using testnet vs mainnet.

## The Journey So Far

Congratulations on making it this far! Let's recap what we've accomplished:

- ✅ **Express Server:** RESTful API with proper routing and middleware
- ✅ **Database System:** SQLite with Knex.js migrations and seeds
- ✅ **Lightning Integration:** LND connection with invoice/payment functionality
- ✅ **Authentication:** JWT-based auth with admin permissions
- ✅ **Database Models:** Clean abstraction layer for data operations
- ✅ **API Testing:** Complete endpoints tested with Postman/Insomnia
- ✅ **Frontend Integration:** React app connected to backend

Now we're ready to take this entire system live and make it accessible to the world!

## Considerations Before Deploying

Before we dive into deployment, let's discuss some important considerations:

### Security Considerations

**Real Money Risk:**
- Your node and app will be susceptible to attacks
- The app has admin rights to your node, so funds could be stolen
- Consider using testnet for learning/portfolio purposes
- Use small amounts if deploying to mainnet

**Privacy Concerns:**
- The wallet activity isn't private unless you update the backend
- API endpoints like `/lightning/invoices` are publicly accessible
- Consider implementing proper access controls

**Potential Vulnerabilities:**
- This is an educational project, not production-ready
- There may be security flaws and attack vectors
- Always tread carefully with real funds

### Cost Considerations

**Monthly Costs:**
- Server hosting: ~$7/month (Heroku basic dyno)
- Database hosting: ~$5/month (Heroku Postgres)
- Lightning node: ~$12/month (Voltage light node)
- Frontend hosting: Free (Vercel)

**Total:** ~$24/month for a complete Lightning wallet infrastructure

**Free Alternatives:**
- Use testnet instead of mainnet
- Use Heroku's built-in SQLite (resets every 24 hours)
- Consider if deployment is necessary for your goals

### Goal Assessment

Ask yourself:
- Are you trying to show off a mainnet wallet?
- Does testnet suffice for your portfolio?
- Do you need it live and deployed?
- Could a local demo video be enough?

## What is Heroku?

Heroku is a cloud platform service that allows developers to deploy, manage, and scale applications without a lot of overhead. It abstracts away the complexities of managing servers, infrastructure, and databases, allowing you to focus purely on the code.

**Key Features:**
- **Platform as a Service (PaaS):** Provides both environment and infrastructure
- **Simple Deployment:** Deploy with Git push
- **Multiple Languages:** Supports Node.js, Ruby, Java, Python, and more
- **Dynos:** Containerized instances that run your application
- **Add-ons:** Marketplace of services (databases, email, etc.)
- **Scalability:** Easy scaling as your app grows

**Why Heroku for Beginners:**
- Optimized for ease of use
- Consistent performance
- Easy to set up and scale
- Great for first-time deployments

## Deploying Server and Database

### Step 1: Create a New Heroku App

First, create a Heroku account and set up a new application:

1. Go to [heroku.com](https://heroku.com) and sign up
2. Click "New" → "Create new app"
3. Name your app: `pleb-wallet-backend` (or similar)
4. Choose your region
5. Click "Create app"

### Step 2: Connect Your Backend Repository

1. Go to the "Deploy" tab
2. Select "GitHub" as deployment method
3. Connect your GitHub account
4. Search for your `pleb-wallet-backend` repository
5. Click "Connect"

### Step 3: Initial Deployment

Deploy immediately to test basic functionality:

1. Click "Deploy Branch" (master/main)
2. Watch the build logs
3. Once complete, click "Open app"
4. You should see: "I'm alive" message

**Expected URL format:** `https://your-app-name.herokuapp.com/`

### Step 4: Enable Web Dyno

Ensure your server stays persistently online:

1. Go to "Resources" tab
2. Toggle the web dyno to "ON"
3. This will cost ~$7/month but keeps your server running

### Step 5: Add Environment Variables

1. Go to "Settings" tab
2. Click "Reveal Config Vars"
3. Add these initial variables:

```
ADMIN_KEY=1234
JWT_SECRET=your-secure-secret-here
NODE_ENV=development
```

**Important:** Use secure secrets for production!

### Step 6: Test Basic Functionality

Test user creation and authentication:

```bash
# Create a user
POST https://your-app-name.herokuapp.com/users/register
{
  "username": "testuser",
  "password": "password123"
}

# Login
POST https://your-app-name.herokuapp.com/users/login
{
  "username": "testuser",
  "password": "password123"
}

# Get all users (requires admin token)
GET https://your-app-name.herokuapp.com/users
Authorization: your-jwt-token
```

## Adding PostgreSQL Database

### Step 7: Add Postgres Buildpack

1. Go to "Resources" tab
2. Search for "Heroku Postgres"
3. Select the basic plan (~$5/month)
4. Click "Submit Order Form"

Heroku will automatically add a `DATABASE_URL` environment variable.

### Step 8: Update Environment Variables

Change your environment to production:

```
NODE_ENV=production
```

### Step 9: Run Database Migrations

1. Go to "More" → "Run console"
2. Run the migration command:

```bash
npx knex migrate:latest
```

This will create your production database tables.

### Step 10: Test Production Database

Create and test a production user:

```bash
# Create admin user
POST https://your-app-name.herokuapp.com/users/register
{
  "username": "admin",
  "password": "securepassword",
  "admin_key": "1234"
}

# Login and get users
POST https://your-app-name.herokuapp.com/users/login
GET https://your-app-name.herokuapp.com/users
```

## Common Deployment Errors

### Express Rate Limit Error

**Error:** `express-rate-limit` warning about reverse proxy

**Fix:** Add this to your `index.js`:

```javascript
// Add this line to trust the first proxy
app.set('trust proxy', 1);
```

### DateTime Field Error

**Error:** `date/time field value out of range`

**Fix:** Update your `lnd.js` file:

```javascript
// If the invoice exists, update it in the database
if (existingInvoice) {
  const settleDate = new Date(data.settle_date * 1000).toISOString();
  await Invoice.update(data.payment_request, {
    settled: data.settled,
    settle_date: settleDate,
  });
} else {
  console.log("Invoice not found in the database");
}
```

### PostgreSQL SSL Error

**Error:** `no pg_hba.conf entry for host`

**Fix:** Update your `knexfile.js` production config:

```javascript
production: {
  client: "pg",
  connection: {
    connectionString: process.env.DATABASE_URL,
    ssl: { rejectUnauthorized: false },
  },
  migrations: {
    directory: "./db/migrations",
  },
  seeds: {
    directory: "./db/seeds",
  },
},
```

## Deploying Lightning Node with Voltage

### What is Voltage?

Voltage is a cloud-based Lightning node hosting service that makes it easy to deploy and manage Lightning nodes without dealing with infrastructure.

**Why Voltage:**
- Easiest Lightning node deployment
- No hardware management
- Encrypted cloud storage
- Great for development and side projects

### Step 11: Create a Voltage Node

1. Go to [nodes.voltage.cloud](https://nodes.voltage.cloud)
2. Create an account
3. Click "Create Node"
4. Choose "LND" as your implementation
5. Select "Lite Node" (~$12/month)
6. Choose network: **mainnet** or **testnet**
7. Create username and password (write them down!)
8. Wait for node initialization

### Step 12: Get Free Inbound Channel

Voltage offers a free inbound channel to new users:

1. Look for the popup on your node dashboard
2. Click "Request Free Channel"
3. This gives you 500,000 sats inbound capacity
4. Allows immediate receiving from the Lightning network

### Step 13: Connect Node to Server

We'll use LND Connect URI instead of separate host/cert/macaroon variables.

**Update your `lnd.js` file:**

```javascript
const options = {
  // Replace the individual connection options with LND Connect URI
  lndConnectUri: process.env.LND_CONNECT_URI,
};
```

**Get your LND Connect URI:**

1. Go to your Voltage node dashboard
2. Click "Connect" → "LND Connect"
3. Copy the long URI string

**Add to Heroku environment variables:**

```
LND_CONNECT_URI=lndconnect://your-long-uri-string-here
```

### Step 14: Deploy and Test Lightning Connection

1. Deploy your updated code to Heroku
2. Watch the logs - you should see: "LND gRPC connection state is active"
3. Test a Lightning endpoint:

```bash
GET https://your-app-name.herokuapp.com/lightning/channelbalance
```

You should see your Lightning channel balance!

## Frontend Deployment with Vercel

### Step 15: Deploy Frontend

1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Click "New Project"
4. Import your `pleb-wallet-frontend` repository
5. Add environment variable:

```
REACT_APP_BACKEND_URL=https://your-app-name.herokuapp.com
```

**Important:** Don't add a trailing slash!

6. Click "Deploy"

### Step 16: Test Frontend Connection

1. Visit your Vercel deployment URL
2. You should see:
   - Bitcoin price loading
   - Wallet balance displaying
   - Transaction history (empty initially)

## Complete System Testing

### Test Scenario 1: Non-Admin User Receiving Payment

1. **Create regular user:**
   - Sign up through frontend
   - Username: `testuser`
   - Password: `password123`

2. **Generate invoice:**
   - Click "Receive"
   - Enter amount: `100`
   - Click "Create Invoice"

3. **Pay invoice externally:**
   - Use another Lightning wallet (Phoenix, Alby, etc.)
   - Pay the generated invoice
   - Watch transaction appear in frontend

### Test Scenario 2: Admin User Sending Payment

1. **Create admin user:**
   ```bash
   POST https://your-app-name.herokuapp.com/users/register
   {
     "username": "admin",
     "password": "securepassword",
     "admin_key": "your-secure-admin-key"
   }
   ```

2. **Login as admin:**
   - Use frontend to login
   - Should see "Welcome admin"

3. **Send payment:**
   - Get invoice from external wallet
   - Click "Send" in frontend
   - Paste invoice and pay
   - Payment should complete successfully

### Test Scenario 3: Permission Verification

1. **Login as regular user**
2. **Try to send payment**
3. **Should fail with 401 error**
4. **Confirms admin-only sending works**

## Security Hardening

### Step 17: Secure Your Secrets

**Update environment variables with secure values:**

```bash
# Generate secure admin key
openssl rand -base64 32

# Generate secure JWT secret
openssl rand -base64 64
```

**Add to Heroku:**
```
ADMIN_KEY=your-secure-32-char-key
JWT_SECRET=your-secure-64-char-secret
```

### API Security Improvements

**Rate limiting adjustments:**

```javascript
// Increase rate limit for frontend polling
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 1000, // Increased from 100
  message: 'Too many requests from this IP, please try again later.'
});
```

**CORS configuration:**

```javascript
app.use(cors({
  origin: ['https://your-frontend-domain.vercel.app'],
  credentials: true
}));
```

## Cost Optimization

### Monthly Costs Breakdown

**Required Services:**
- Heroku Web Dyno: $7/month
- Heroku Postgres: $5/month
- Voltage Lightning Node: $12/month
- Vercel Frontend: Free

**Total: $24/month**

### Cost-Saving Options

**Use Testnet:**
- Same functionality, no real money risk
- Perfect for portfolios and learning

**Heroku Eco Plan:**
- Sleep after 30 minutes of inactivity
- Fine for demo purposes

**Alternative Hosting:**
- Railway: Cheaper alternatives
- Digital Ocean: More control, similar cost
- AWS/GCP: More complex but potentially cheaper

## Monitoring and Maintenance

### Heroku Logs

Monitor your application:

```bash
# View live logs
heroku logs --tail --app your-app-name

# View specific number of lines
heroku logs -n 200 --app your-app-name
```

### Voltage Monitoring

Monitor your Lightning node:

1. Use Voltage dashboard
2. Set up ThunderHub dashboard
3. Monitor channel balance and connectivity

### Database Monitoring

Monitor your database:

1. Heroku Postgres dashboard
2. View connection counts
3. Monitor query performance

## Troubleshooting Common Issues

### Frontend Can't Connect to Backend

**Check:**
- Environment variable spelling
- No trailing slashes in URLs
- CORS configuration
- Heroku dyno is running

### Lightning Node Connection Issues

**Check:**
- LND_CONNECT_URI is correct
- Node is running on Voltage
- Network connectivity
- Firewall issues

### Database Connection Problems

**Check:**
- DATABASE_URL is set
- SSL configuration
- Migration status
- Connection limits

### Payment Failures

**Check:**
- Channel liquidity
- Node connectivity
- Invoice validity
- Admin permissions

## Performance Optimization

### Database Optimization

**Connection Pooling:**

```javascript
// In knexfile.js
pool: {
  min: 2,
  max: 10,
  acquireTimeoutMillis: 30000,
  idleTimeoutMillis: 30000
}
```

**Query Optimization:**
- Add database indexes
- Optimize N+1 queries
- Use database views for complex queries

### API Optimization

**Response Caching:**

```javascript
// Add simple caching for price data
const cache = new Map();
const CACHE_DURATION = 30000; // 30 seconds

app.get('/api/price', (req, res) => {
  const cached = cache.get('btc-price');
  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return res.json(cached.data);
  }
  
  // Fetch fresh data and cache it
  // ... price fetching logic
});
```

## Advanced Deployment Features

### Automatic Deployments

Set up automatic deployments:

1. Go to Heroku Deploy tab
2. Enable "Automatic deploys"
3. Choose "Wait for CI to pass"
4. Every push to main triggers deployment

### Environment-Specific Configurations

Create staging environment:

```javascript
// config/environments.js
const environments = {
  development: {
    host: 'localhost:5500',
    logLevel: 'debug'
  },
  staging: {
    host: 'your-staging-app.herokuapp.com',
    logLevel: 'info'
  },
  production: {
    host: 'your-production-app.herokuapp.com',
    logLevel: 'error'
  }
};

module.exports = environments[process.env.NODE_ENV];
```

### Health Checks

Add health check endpoint:

```javascript
app.get('/health', async (req, res) => {
  try {
    // Check database connection
    await knex.raw('SELECT 1');
    
    // Check Lightning node connection
    const info = await lnd.getInfo({});
    
    res.json({
      status: 'healthy',
      timestamp: new Date().toISOString(),
      services: {
        database: 'connected',
        lightning: 'connected',
        node_pubkey: info.identity_pubkey.slice(0, 10) + '...'
      }
    });
  } catch (error) {
    res.status(500).json({
      status: 'unhealthy',
      error: error.message
    });
  }
});
```

## Backup and Recovery

### Database Backups

Heroku automatically backs up Postgres:

```bash
# Create manual backup
heroku pg:backups:capture --app your-app-name

# List backups
heroku pg:backups --app your-app-name

# Download backup
heroku pg:backups:download --app your-app-name
```

### Lightning Node Backups

Voltage handles node backups automatically, but you should:

1. Save your node credentials securely
2. Export channel backup file
3. Store seed phrase safely

### Code Backups

Ensure your code is backed up:

1. Multiple Git remotes
2. Regular commits
3. Tagged releases

## What's Next?

Congratulations! You've successfully deployed a complete Lightning wallet system. Here are some next steps:

### Immediate Improvements

1. **Add more security features:**
   - Rate limiting per user
   - Input validation
   - SQL injection protection
   - XSS protection

2. **Enhance user experience:**
   - Real-time WebSocket updates
   - Better error handling
   - Loading states
   - Offline support

3. **Add new features:**
   - Lightning Address support
   - Recurring payments
   - Multi-user support
   - Invoice templates

### Advanced Topics

1. **Scaling:**
   - Load balancing
   - Database sharding
   - CDN integration
   - Caching strategies

2. **Monitoring:**
   - Application metrics
   - Error tracking
   - Performance monitoring
   - Alerting systems

3. **DevOps:**
   - CI/CD pipelines
   - Infrastructure as code
   - Container orchestration
   - Blue-green deployments

## Key Takeaways

1. **Deployment is a Process:** Deploying to production involves many steps and considerations beyond just running code

2. **Security is Paramount:** Always prioritize security, especially when dealing with real money and Lightning networks

3. **Testing is Critical:** Thoroughly test each component and integration point in production

4. **Monitoring is Essential:** Set up proper monitoring and logging from day one

5. **Start Small:** Begin with testnet and small amounts before scaling up

6. **Documentation Matters:** Keep detailed records of your deployment process and configurations

7. **Backup Everything:** Have backup strategies for your database, node, and code

8. **Plan for Costs:** Understand the ongoing costs and plan accordingly

## Practice Exercises

1. **Deploy to Testnet:**
   - Create a testnet version of your entire stack
   - Practice the deployment process without risk

2. **Implement Health Checks:**
   - Add comprehensive health check endpoints
   - Set up monitoring alerts

3. **Add Security Features:**
   - Implement API key authentication
   - Add request validation
   - Set up HTTPS redirects

4. **Create a Staging Environment:**
   - Set up a staging deployment
   - Practice blue-green deployments

5. **Implement Backup Strategy:**
   - Set up automated database backups
   - Create disaster recovery procedures

## Resources

### Deployment Platforms
- [Heroku Documentation](https://devcenter.heroku.com/) - Complete deployment guide
- [Vercel Documentation](https://vercel.com/docs) - Frontend deployment
- [Railway](https://railway.app/) - Alternative to Heroku
- [Digital Ocean App Platform](https://www.digitalocean.com/products/app-platform/) - Another alternative

### Lightning Node Services
- [Voltage](https://voltage.cloud/) - Managed Lightning nodes
- [Umbrel](https://umbrel.com/) - Self-hosted Lightning node
- [Start9](https://start9.com/) - Personal server OS
- [RaspiBlitz](https://github.com/rootzoll/raspiblitz) - DIY Lightning node

### Security Resources
- [OWASP API Security](https://owasp.org/www-project-api-security/) - API security best practices
- [Node.js Security Checklist](https://blog.risingstack.com/node-js-security-checklist/) - Security guidelines
- [Lightning Security](https://github.com/lightningnetwork/lnd/blob/master/docs/safety.md) - Lightning security guide

### Monitoring Tools
- [Heroku Metrics](https://devcenter.heroku.com/articles/metrics) - Application monitoring
- [Sentry](https://sentry.io/) - Error tracking
- [DataDog](https://www.datadoghq.com/) - Comprehensive monitoring
- [New Relic](https://newrelic.com/) - Application performance monitoring

### Learning Resources
- [The Twelve-Factor App](https://12factor.net/) - Methodology for building SaaS apps
- [Lightning Network Specifications](https://github.com/lightning/bolts) - Technical specifications
- [LND Developer Documentation](https://docs.lightning.engineering/) - LND-specific guides

## Final Thoughts

You've just completed an incredible journey! From learning basic JavaScript to deploying a complete Lightning wallet system, you've covered:

- **Backend Development:** Express.js, APIs, middleware
- **Database Management:** SQL, migrations, data modeling
- **Authentication:** JWT tokens, authorization
- **Lightning Network:** Payment processing, node management
- **Frontend Integration:** React, API consumption
- **DevOps:** Cloud deployment, monitoring, security

You're now equipped to build Lightning-enabled applications and have hands-on experience with the entire development lifecycle. The Lightning Network is rapidly growing, and you're now positioned to be part of that growth.

Remember: This is just the beginning. The skills you've learned here can be applied to build any Lightning-enabled application. Keep experimenting, keep learning, and most importantly, keep building!

**Welcome to the Lightning developer community!** 🚀⚡️

---

**Course Complete!** You've successfully completed the PlebDevs Backend Course. Join our Discord community to connect with other developers, share your projects, and continue learning together. 