[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-9.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-9.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-9.webm)

# Lesson 9: Mempool Monitor

## Welcome to Asynchronous Programming! 🌐

Amazing work, PlebDev! You've mastered synchronous programming and external libraries. Now we're diving into one of the most important aspects of modern web development: asynchronous programming and API integration.

In this lesson, you'll build a mempool monitor that simulates fetching real-time Bitcoin network data. This teaches you how to work with promises, async/await, error handling, and the patterns used in every modern Bitcoin application that connects to external services.

## What You'll Learn

### JavaScript Concepts
- **Fetch API**: Making HTTP requests to external services
- **Promises**: Handling asynchronous operations
- **Async/Await**: Modern syntax for asynchronous code
- **Error Handling**: Gracefully managing network failures

### Bitcoin Development Skills
- Understanding Bitcoin mempool dynamics
- Simulating real-time blockchain data
- Building resilient network applications
- Creating responsive user interfaces

## Prerequisites
- Completed Lessons 1-8
- Understanding of functions, objects, and timing
- Ready to work with asynchronous operations

## Project Overview: Bitcoin Mempool Monitor

We're building a monitor that:
- Simulates fetching mempool data from Bitcoin APIs
- Displays transaction pool statistics
- Shows fee recommendations based on mempool state
- Handles network errors gracefully
- Updates data automatically with proper loading states

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `bitcoin-mempool-monitor`
2. Open in VS Code
3. Create `index.html`

### Step 2: Understanding Asynchronous JavaScript
```javascript
// Synchronous code (blocking)
console.log("Start");
// This would block for 5 seconds
console.log("End");

// Asynchronous code (non-blocking)
console.log("Start");
setTimeout(() => {
    console.log("This runs after 2 seconds");
}, 2000);
console.log("End"); // This runs immediately

// Promises - modern way to handle async operations
const myPromise = new Promise((resolve, reject) => {
    // Simulate async operation
    setTimeout(() => {
        resolve("Success!");
    }, 1000);
});

myPromise.then(result => {
    console.log(result); // "Success!"
});
```

### Step 3: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Mempool Monitor</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: white;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 40px;
            backdrop-filter: blur(10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        .header {
            text-align: center;
            margin-bottom: 40px;
        }
        .status-indicator {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #4CAF50;
            animation: pulse 2s infinite;
        }
        .status-dot.loading {
            background: #ff9500;
        }
        .status-dot.error {
            background: #f44336;
        }
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }
        .refresh-section {
            text-align: center;
            margin-bottom: 30px;
        }
        .refresh-btn {
            padding: 15px 30px;
            background: #ff9500;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        .refresh-btn:hover {
            background: #e08600;
            transform: translateY(-2px);
        }
        .refresh-btn:disabled {
            background: #666;
            cursor: not-allowed;
            transform: none;
        }
        .mempool-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        .stat-card {
            background: rgba(255, 255, 255, 0.15);
            padding: 25px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        .stat-value {
            font-size: 2.2em;
            font-weight: bold;
            color: #ff9500;
            margin-bottom: 10px;
        }
        .stat-label {
            font-size: 0.9em;
            opacity: 0.8;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .fee-recommendations {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
        }
        .fee-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        .fee-card {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            border: 2px solid transparent;
            transition: all 0.3s ease;
        }
        .fee-card:hover {
            border-color: #ff9500;
            transform: translateY(-3px);
        }
        .fee-priority {
            font-size: 1.5em;
            margin-bottom: 10px;
        }
        .fee-rate {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff9500;
            margin: 10px 0;
        }
        .fee-time {
            font-size: 0.9em;
            opacity: 0.8;
        }
        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .error-message {
            background: rgba(244, 67, 54, 0.2);
            border: 2px solid rgba(244, 67, 54, 0.5);
            border-radius: 10px;
            padding: 20px;
            margin: 20px 0;
            text-align: center;
        }
        .network-info {
            background: rgba(255, 149, 0, 0.1);
            border: 1px solid rgba(255, 149, 0, 0.3);
            border-radius: 12px;
            padding: 25px;
            margin-top: 30px;
        }
        .auto-refresh {
            margin-top: 20px;
            text-align: center;
        }
        .auto-refresh label {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📊 Bitcoin Mempool Monitor</h1>
            <p>Real-time transaction pool analysis and fee recommendations</p>
            
            <div class="status-indicator">
                <div id="status-dot" class="status-dot"></div>
                <span id="status-text">Ready to fetch data</span>
            </div>
        </div>
        
        <div class="refresh-section">
            <button id="refresh-btn" class="refresh-btn" onclick="fetchMempoolData()">
                <span id="refresh-text">🔄 Fetch Mempool Data</span>
            </button>
            
            <div class="auto-refresh">
                <label>
                    <input type="checkbox" id="auto-refresh" onchange="toggleAutoRefresh()">
                    Auto-refresh every 30 seconds
                </label>
            </div>
        </div>
        
        <div id="mempool-content">
            <div class="mempool-stats">
                <div class="stat-card">
                    <div id="mempool-size" class="stat-value">-</div>
                    <div class="stat-label">Transactions in Mempool</div>
                </div>
                <div class="stat-card">
                    <div id="mempool-bytes" class="stat-value">-</div>
                    <div class="stat-label">Total Size (MB)</div>
                </div>
                <div class="stat-card">
                    <div id="avg-fee" class="stat-value">-</div>
                    <div class="stat-label">Average Fee (sats/vB)</div>
                </div>
                <div class="stat-card">
                    <div id="last-update" class="stat-value">-</div>
                    <div class="stat-label">Last Updated</div>
                </div>
            </div>
            
            <div class="fee-recommendations">
                <h3>⚡ Fee Recommendations</h3>
                <div class="fee-grid">
                    <div class="fee-card">
                        <div class="fee-priority">🚀 High Priority</div>
                        <div id="high-fee" class="fee-rate">- sats/vB</div>
                        <div class="fee-time">~10 minutes</div>
                    </div>
                    <div class="fee-card">
                        <div class="fee-priority">🚶 Medium Priority</div>
                        <div id="medium-fee" class="fee-rate">- sats/vB</div>
                        <div class="fee-time">~30 minutes</div>
                    </div>
                    <div class="fee-card">
                        <div class="fee-priority">🐌 Low Priority</div>
                        <div id="low-fee" class="fee-rate">- sats/vB</div>
                        <div class="fee-time">~1 hour</div>
                    </div>
                </div>
            </div>
        </div>
        
        <div id="error-container"></div>
        
        <div class="network-info">
            <h3>🔗 About the Bitcoin Mempool</h3>
            <p><strong>What is the mempool?</strong> The mempool (memory pool) is where unconfirmed Bitcoin transactions wait to be included in a block by miners.</p>
            <ul>
                <li><strong>Higher fees</strong> = faster confirmation (miners prioritize higher-fee transactions)</li>
                <li><strong>Mempool size</strong> indicates network congestion</li>
                <li><strong>Fee rates</strong> are measured in satoshis per virtual byte (sats/vB)</li>
                <li><strong>Confirmation times</strong> are estimates based on current network conditions</li>
            </ul>
        </div>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 4: Understanding Promises and Async/Await
```javascript
// Traditional Promise syntax
function fetchDataOldWay() {
    fetch('https://api.example.com/data')
        .then(response => response.json())
        .then(data => {
            console.log(data);
        })
        .catch(error => {
            console.error('Error:', error);
        });
}

// Modern async/await syntax (preferred)
async function fetchDataNewWay() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error);
    }
}

// Simulating API calls with Promise
function simulateAPICall() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (Math.random() > 0.2) { // 80% success rate
                resolve({ status: 'success', data: {} });
            } else {
                reject(new Error('Network error'));
            }
        }, 1000);
    });
}
```

### Step 5: Building the Mempool Data Simulator
```javascript
// Global variables
let autoRefreshInterval = null;
let isLoading = false;

// Simulate fetching mempool data from Bitcoin API
async function getMempoolData() {
    // Simulate network delay
    await new Promise(resolve => setTimeout(resolve, 1000 + Math.random() * 1000));
    
    // Simulate occasional network errors (10% chance)
    if (Math.random() < 0.1) {
        throw new Error("Network timeout - unable to reach Bitcoin node");
    }
    
    // Generate realistic mempool data
    const baseSize = 50000 + Math.random() * 100000; // 50k-150k transactions
    const congestionMultiplier = 0.5 + Math.random() * 1.5; // 0.5x to 2x normal
    
    return {
        size: Math.floor(baseSize * congestionMultiplier),
        bytes: Math.floor((baseSize * congestionMultiplier * 250) / 1000000), // Average 250 bytes per tx, convert to MB
        fees: {
            fast: Math.floor((20 + Math.random() * 50) * congestionMultiplier),
            medium: Math.floor((10 + Math.random() * 25) * congestionMultiplier),
            slow: Math.floor((5 + Math.random() * 15) * congestionMultiplier)
        },
        avgFee: Math.floor((15 + Math.random() * 30) * congestionMultiplier),
        timestamp: new Date().toISOString()
    };
}

// Main function to fetch and display mempool data
async function fetchMempoolData() {
    if (isLoading) {
        console.log("Already fetching data, please wait...");
        return;
    }
    
    isLoading = true;
    updateUIState('loading');
    
    try {
        console.log("🔄 Fetching mempool data...");
        
        // Fetch data from simulated API
        const data = await getMempoolData();
        
        // Update UI with new data
        displayMempoolData(data);
        updateUIState('success');
        
        console.log("✅ Mempool data updated successfully");
        
    } catch (error) {
        console.error("❌ Error fetching mempool data:", error);
        showError(error.message);
        updateUIState('error');
        
    } finally {
        isLoading = false;
    }
}
```

### Step 6: UI State Management
```javascript
// Update UI state (loading, success, error)
function updateUIState(state) {
    const statusDot = document.getElementById('status-dot');
    const statusText = document.getElementById('status-text');
    const refreshBtn = document.getElementById('refresh-btn');
    const refreshText = document.getElementById('refresh-text');
    
    // Clear any existing error messages
    document.getElementById('error-container').innerHTML = '';
    
    switch (state) {
        case 'loading':
            statusDot.className = 'status-dot loading';
            statusText.textContent = 'Fetching data...';
            refreshBtn.disabled = true;
            refreshText.innerHTML = '<span class="loading-spinner"></span> Fetching...';
            break;
            
        case 'success':
            statusDot.className = 'status-dot';
            statusText.textContent = 'Data updated successfully';
            refreshBtn.disabled = false;
            refreshText.innerHTML = '🔄 Fetch Mempool Data';
            break;
            
        case 'error':
            statusDot.className = 'status-dot error';
            statusText.textContent = 'Error fetching data';
            refreshBtn.disabled = false;
            refreshText.innerHTML = '🔄 Retry Fetch';
            break;
    }
}

// Display mempool data in the UI
function displayMempoolData(data) {
    // Update main statistics
    document.getElementById('mempool-size').textContent = data.size.toLocaleString();
    document.getElementById('mempool-bytes').textContent = data.bytes.toFixed(1);
    document.getElementById('avg-fee').textContent = data.avgFee;
    document.getElementById('last-update').textContent = new Date().toLocaleTimeString();
    
    // Update fee recommendations
    document.getElementById('high-fee').textContent = data.fees.fast + ' sats/vB';
    document.getElementById('medium-fee').textContent = data.fees.medium + ' sats/vB';
    document.getElementById('low-fee').textContent = data.fees.slow + ' sats/vB';
    
    // Add visual feedback for fee levels
    updateFeeCardColors(data.fees);
    
    console.log("📊 UI updated with mempool data:", data);
}

// Update fee card colors based on congestion
function updateFeeCardColors(fees) {
    const feeCards = document.querySelectorAll('.fee-card');
    
    feeCards.forEach((card, index) => {
        const feeValue = index === 0 ? fees.fast : index === 1 ? fees.medium : fees.slow;
        
        // Color based on fee level
        if (feeValue > 50) {
            card.style.backgroundColor = 'rgba(244, 67, 54, 0.2)'; // Red for high fees
        } else if (feeValue > 25) {
            card.style.backgroundColor = 'rgba(255, 193, 7, 0.2)'; // Yellow for medium fees
        } else {
            card.style.backgroundColor = 'rgba(76, 175, 80, 0.2)'; // Green for low fees
        }
    });
}

// Show error messages
function showError(message) {
    const errorContainer = document.getElementById('error-container');
    errorContainer.innerHTML = `
        <div class="error-message">
            <h3>⚠️ Connection Error</h3>
            <p><strong>${message}</strong></p>
            <p style="font-size: 0.9em; margin-top: 15px;">
                This is a simulated error for learning purposes. In a real application, this might indicate:
            </p>
            <ul style="text-align: left; font-size: 0.9em;">
                <li>Bitcoin node is temporarily unavailable</li>
                <li>Network connectivity issues</li>
                <li>API rate limiting</li>
                <li>Server maintenance</li>
            </ul>
        </div>
    `;
}
```

### Step 7: Auto-Refresh and Advanced Features
```javascript
// Toggle auto-refresh functionality
function toggleAutoRefresh() {
    const checkbox = document.getElementById('auto-refresh');
    
    if (checkbox.checked) {
        startAutoRefresh();
        console.log("🔄 Auto-refresh enabled (30 second intervals)");
    } else {
        stopAutoRefresh();
        console.log("⏹️ Auto-refresh disabled");
    }
}

// Start auto-refresh
function startAutoRefresh() {
    if (autoRefreshInterval) {
        clearInterval(autoRefreshInterval);
    }
    
    autoRefreshInterval = setInterval(async () => {
        console.log("🔄 Auto-refresh triggered");
        await fetchMempoolData();
    }, 30000); // 30 seconds
}

// Stop auto-refresh
function stopAutoRefresh() {
    if (autoRefreshInterval) {
        clearInterval(autoRefreshInterval);
        autoRefreshInterval = null;
    }
}

// Enhanced error handling with retry logic
async function fetchMempoolDataWithRetry(maxRetries = 3) {
    let retries = 0;
    
    while (retries < maxRetries) {
        try {
            await fetchMempoolData();
            return; // Success, exit retry loop
        } catch (error) {
            retries++;
            console.log(`Retry ${retries}/${maxRetries} after error:`, error.message);
            
            if (retries < maxRetries) {
                // Wait before retrying (exponential backoff)
                const delay = Math.pow(2, retries) * 1000; // 2s, 4s, 8s
                await new Promise(resolve => setTimeout(resolve, delay));
            }
        }
    }
    
    console.error("❌ All retry attempts failed");
}

// Simulate real-time mempool changes
function simulateMempoolActivity() {
    setInterval(() => {
        if (document.getElementById('mempool-size').textContent !== '-') {
            // Slightly adjust current values to simulate activity
            const currentSize = parseInt(document.getElementById('mempool-size').textContent.replace(/,/g, ''));
            const change = Math.floor((Math.random() - 0.5) * 1000); // ±500 transactions
            const newSize = Math.max(10000, currentSize + change);
            
            document.getElementById('mempool-size').textContent = newSize.toLocaleString();
            
            // Visual indicator of activity
            const sizeElement = document.getElementById('mempool-size');
            sizeElement.style.color = change > 0 ? '#4CAF50' : '#f44336';
            setTimeout(() => {
                sizeElement.style.color = '#ff9500';
            }, 1000);
        }
    }, 5000); // Every 5 seconds
}
```

### Step 8: Application Initialization and Cleanup
```javascript
// Initialize the application
function initializeApp() {
    console.log("🚀 Initializing Bitcoin Mempool Monitor...");
    
    // Initial state
    updateUIState('ready');
    
    // Start simulated activity
    simulateMempoolActivity();
    
    // Fetch initial data
    setTimeout(() => {
        fetchMempoolData();
    }, 1000);
    
    console.log("✅ Mempool monitor initialized");
}

// Cleanup when page unloads
function cleanup() {
    if (autoRefreshInterval) {
        clearInterval(autoRefreshInterval);
    }
    console.log("🧹 Cleanup completed");
}

// Event listeners
window.addEventListener('load', initializeApp);
window.addEventListener('beforeunload', cleanup);

// Handle visibility change (pause when tab is hidden)
document.addEventListener('visibilitychange', function() {
    if (document.hidden) {
        console.log("📴 Tab hidden, pausing auto-refresh");
        stopAutoRefresh();
    } else {
        console.log("👁️ Tab visible, resuming if enabled");
        const checkbox = document.getElementById('auto-refresh');
        if (checkbox && checkbox.checked) {
            startAutoRefresh();
        }
    }
});

// Keyboard shortcuts
document.addEventListener('keydown', function(event) {
    // Press 'R' to refresh
    if (event.key === 'r' || event.key === 'R') {
        if (!isLoading) {
            fetchMempoolData();
        }
    }
    
    // Press 'A' to toggle auto-refresh
    if (event.key === 'a' || event.key === 'A') {
        const checkbox = document.getElementById('auto-refresh');
        checkbox.checked = !checkbox.checked;
        toggleAutoRefresh();
    }
});

// Export functions for testing (optional)
if (typeof module !== 'undefined' && module.exports) {
    module.exports = {
        getMempoolData,
        fetchMempoolData,
        displayMempoolData
    };
}
```

## Key Takeaways

### Asynchronous Programming
- **Promises**: Represent future values from async operations
- **async/await**: Modern, readable syntax for handling promises
- **Non-blocking**: Code continues running while waiting for responses
- **Error handling**: Always use try/catch with async operations

### Network Programming Patterns
- **Loading states**: Show users when operations are in progress
- **Error handling**: Gracefully manage network failures
- **Retry logic**: Automatically attempt failed operations
- **Timeout handling**: Don't wait forever for responses

### User Experience in Async Apps
- **Visual feedback**: Loading spinners, status indicators
- **Progressive enhancement**: App works even when network fails
- **Auto-refresh**: Keep data current without user intervention
- **Responsive UI**: Never block the interface

### Real-World API Integration
- **HTTP status codes**: Check response.ok for successful requests
- **Rate limiting**: Respect API usage limits
- **Data validation**: Verify API responses before using
- **Caching**: Store data locally to reduce API calls

## Real-World Applications

These patterns are essential for Bitcoin development:
- **Trading Applications**: Real-time price feeds, order book updates
- **Wallet Interfaces**: Balance updates, transaction broadcasting
- **Block Explorers**: Live blockchain data, transaction status
- **Lightning Apps**: Channel state monitoring, route discovery
- **Mining Dashboards**: Pool statistics, profitability calculations
- **DeFi Protocols**: Yield updates, liquidity monitoring

## Challenge Yourself

1. **Real API Integration**: Connect to actual Bitcoin mempool APIs
2. **WebSocket Implementation**: Add real-time data streaming
3. **Offline Support**: Handle network disconnections gracefully
4. **Data Caching**: Store recent data in localStorage
5. **Performance Optimization**: Implement request debouncing
6. **Mobile Optimization**: Adapt for mobile network conditions

## Common Issues and Solutions

### Promises Not Working
- Always use try/catch with async/await
- Check that functions are marked as `async`
- Don't forget to `await` promise-returning functions
- Handle both success and error cases

### Network Errors
- Implement proper error boundaries
- Provide meaningful error messages to users
- Add retry logic for transient failures
- Test with network throttling/offline conditions

### Memory Leaks
- Clear intervals and timeouts when components unmount
- Remove event listeners when no longer needed
- Cancel pending requests when navigating away
- Use `AbortController` for request cancellation

### CORS Issues
- Understand Cross-Origin Resource Sharing restrictions
- Use CORS-enabled APIs or proxies for development
- Consider server-side solutions for production
- Test with different browser security settings

## Next Steps

Excellent work! You've mastered asynchronous programming and network integration. In Lesson 10, we'll build an interactive Bitcoin whitepaper reader using event delegation and advanced DOM manipulation.

### What's Coming Next
- **Lesson 10**: Event delegation and advanced interactivity
- **Real-world skills**: Building engaging user interfaces
- **New concepts**: Advanced DOM manipulation patterns

## Resources for Going Deeper

- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Async/Await Guide](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
- [Promise Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Bitcoin Mempool APIs](https://mempool.space/docs/api)

## Share Your Success!

🎉 **You built a real-time monitoring application!** 

This demonstrates advanced Bitcoin development skills. You're using the same asynchronous patterns that power professional Bitcoin services and trading platforms.

Don't forget to:
1. Test the auto-refresh functionality
2. Try triggering errors to see error handling
3. Use keyboard shortcuts (R to refresh, A for auto-refresh)
4. Push your code to GitHub
5. Share your monitor with the Bitcoin community

Ready to build advanced interactive interfaces? Let's create a Bitcoin whitepaper reader! 📖

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 