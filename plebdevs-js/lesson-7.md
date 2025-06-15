[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-7.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-7.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-7.webm)

# Lesson 7: HODL Portfolio Tracker

## Welcome to Data Persistence, PlebDev! 💎

Incredible progress! You've mastered the core concepts and built some amazing Bitcoin applications. Now we're adding one of the most important features of modern web applications: the ability to save and remember user data between sessions.

In this lesson, you'll build a HODL portfolio tracker that remembers your Bitcoin positions even after closing the browser. This introduces you to client-side storage, JSON data handling, and building applications that truly serve users over time.

## What You'll Learn

### JavaScript Concepts
- **localStorage**: Storing data in the browser permanently
- **JSON**: Converting between objects and text for storage
- **Data Persistence**: Making applications remember user data
- **State Management**: Keeping track of application data

### Bitcoin Development Skills
- Building portfolio tracking applications
- Managing Bitcoin position data
- Creating persistent user experiences
- Calculating investment performance

## Prerequisites
- Completed Lessons 1-6
- Understanding of objects, arrays, and functions
- Ready to build applications that save data

## Project Overview: Personal HODL Tracker

We're building a portfolio tracker that:
- Saves Bitcoin positions permanently in the browser
- Calculates total portfolio value and performance
- Tracks buy dates and prices for each position
- Persists data between browser sessions
- Provides portfolio analytics and insights

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `hodl-portfolio-tracker`
2. Open in VS Code
3. Create `index.html`

### Step 2: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>HODL Portfolio Tracker</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #ff9500 0%, #ff7700 100%);
            min-height: 100vh;
            color: #333;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            overflow: hidden;
        }
        .header {
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            padding: 30px;
            text-align: center;
        }
        .content {
            padding: 40px;
        }
        .add-position {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 12px;
            margin-bottom: 30px;
            border: 2px dashed #dee2e6;
        }
        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }
        .input-group {
            display: flex;
            flex-direction: column;
        }
        label {
            font-weight: bold;
            margin-bottom: 5px;
            color: #555;
        }
        input {
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 1em;
        }
        input:focus {
            border-color: #ff9500;
            outline: none;
        }
        .add-btn {
            background: #ff9500;
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 8px;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }
        .add-btn:hover {
            background: #e08600;
        }
        .portfolio-stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        .stat-card {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 25px;
            border-radius: 12px;
            text-align: center;
        }
        .stat-value {
            font-size: 2em;
            font-weight: bold;
            margin-bottom: 5px;
        }
        .stat-label {
            font-size: 0.9em;
            opacity: 0.9;
        }
        .positions-list {
            background: #f8f9fa;
            border-radius: 12px;
            overflow: hidden;
        }
        .position-item {
            background: white;
            margin: 10px;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #ff9500;
            display: grid;
            grid-template-columns: 2fr 1fr 1fr 1fr auto;
            gap: 15px;
            align-items: center;
        }
        .position-date {
            font-size: 0.9em;
            color: #666;
        }
        .position-amount {
            font-weight: bold;
            color: #ff9500;
        }
        .position-price {
            color: #666;
        }
        .delete-btn {
            background: #dc3545;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9em;
        }
        .delete-btn:hover {
            background: #c82333;
        }
        .empty-state {
            text-align: center;
            padding: 40px;
            color: #666;
        }
        .controls {
            text-align: center;
            margin-top: 30px;
        }
        .control-btn {
            background: #6c757d;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            margin: 5px;
        }
        .control-btn:hover {
            background: #5a6268;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>💎 HODL Portfolio Tracker</h1>
            <p>Track your Bitcoin journey to the moon! 🚀</p>
        </div>
        
        <div class="content">
            <!-- Add New Position Form -->
            <div class="add-position">
                <h3>📈 Add New Bitcoin Position</h3>
                <div class="form-row">
                    <div class="input-group">
                        <label for="btc-amount">BTC Amount:</label>
                        <input type="number" id="btc-amount" placeholder="0.00000001" step="0.00000001" min="0">
                    </div>
                    <div class="input-group">
                        <label for="buy-price">Buy Price (USD):</label>
                        <input type="number" id="buy-price" placeholder="50000" step="0.01" min="0">
                    </div>
                    <div class="input-group">
                        <label for="buy-date">Buy Date:</label>
                        <input type="date" id="buy-date">
                    </div>
                </div>
                <button class="add-btn" onclick="addPosition()">
                    🔥 Add to HODL Stack
                </button>
            </div>
            
            <!-- Portfolio Statistics -->
            <div class="portfolio-stats">
                <div class="stat-card">
                    <div id="total-btc" class="stat-value">0.00000000</div>
                    <div class="stat-label">Total BTC</div>
                </div>
                <div class="stat-card">
                    <div id="total-invested" class="stat-value">$0</div>
                    <div class="stat-label">Total Invested</div>
                </div>
                <div class="stat-card">
                    <div id="avg-price" class="stat-value">$0</div>
                    <div class="stat-label">Average Price</div>
                </div>
                <div class="stat-card">
                    <div id="position-count" class="stat-value">0</div>
                    <div class="stat-label">Positions</div>
                </div>
            </div>
            
            <!-- Positions List -->
            <div class="positions-list">
                <div style="padding: 20px; border-bottom: 1px solid #dee2e6;">
                    <h3>🏦 Your HODL Positions</h3>
                </div>
                <div id="positions-container">
                    <!-- Positions will be displayed here -->
                </div>
            </div>
            
            <!-- Controls -->
            <div class="controls">
                <button class="control-btn" onclick="exportData()">📥 Export Data</button>
                <button class="control-btn" onclick="importData()">📤 Import Data</button>
                <button class="control-btn" onclick="clearAllData()">🗑️ Clear All</button>
            </div>
        </div>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 3: Understanding localStorage
```javascript
// localStorage stores data permanently in the browser
// Data survives browser restarts, computer restarts, etc.

// Store data (must be strings)
localStorage.setItem("key", "value");

// Retrieve data
const value = localStorage.getItem("key");

// Remove specific item
localStorage.removeItem("key");

// Clear all data
localStorage.clear();

// Check if item exists
if (localStorage.getItem("key") !== null) {
    // Item exists
}
```

### Step 4: Understanding JSON
```javascript
// JSON converts objects to strings and back
const myObject = { name: "Bitcoin", price: 50000 };

// Convert object to string for storage
const jsonString = JSON.stringify(myObject);
localStorage.setItem("data", jsonString);

// Convert string back to object
const savedString = localStorage.getItem("data");
const restoredObject = JSON.parse(savedString);

console.log(restoredObject.name); // "Bitcoin"
```

### Step 5: Building the Portfolio Data Management
```javascript
// Global array to hold portfolio positions
let portfolio = [];

// Load portfolio from localStorage when app starts
function loadPortfolio() {
    const saved = localStorage.getItem("btcPortfolio");
    if (saved) {
        try {
            portfolio = JSON.parse(saved);
            console.log("Portfolio loaded:", portfolio.length, "positions");
        } catch (error) {
            console.error("Error loading portfolio:", error);
            portfolio = [];
        }
    } else {
        portfolio = [];
        console.log("No saved portfolio found, starting fresh");
    }
}

// Save portfolio to localStorage
function savePortfolio() {
    try {
        const jsonString = JSON.stringify(portfolio);
        localStorage.setItem("btcPortfolio", jsonString);
        console.log("Portfolio saved successfully");
    } catch (error) {
        console.error("Error saving portfolio:", error);
        alert("Error saving portfolio. Check browser storage limits.");
    }
}
```

### Step 6: Adding New Positions
```javascript
function addPosition() {
    // Get input values
    const btcAmount = parseFloat(document.getElementById("btc-amount").value);
    const buyPrice = parseFloat(document.getElementById("buy-price").value);
    const buyDate = document.getElementById("buy-date").value;
    
    // Validate inputs
    if (!btcAmount || btcAmount <= 0) {
        alert("Please enter a valid BTC amount");
        return;
    }
    
    if (!buyPrice || buyPrice <= 0) {
        alert("Please enter a valid buy price");
        return;
    }
    
    if (!buyDate) {
        alert("Please select a buy date");
        return;
    }
    
    // Create new position object
    const newPosition = {
        id: Date.now(), // Simple ID using timestamp
        btcAmount: btcAmount,
        buyPrice: buyPrice,
        buyDate: buyDate,
        totalInvested: btcAmount * buyPrice,
        dateAdded: new Date().toISOString()
    };
    
    // Add to portfolio array
    portfolio.push(newPosition);
    
    // Save to localStorage
    savePortfolio();
    
    // Update display
    displayPortfolio();
    updateStats();
    
    // Clear form
    clearForm();
    
    console.log("New position added:", newPosition);
}

function clearForm() {
    document.getElementById("btc-amount").value = "";
    document.getElementById("buy-price").value = "";
    document.getElementById("buy-date").value = "";
}
```

### Step 7: Displaying Portfolio Data
```javascript
function displayPortfolio() {
    const container = document.getElementById("positions-container");
    
    if (portfolio.length === 0) {
        container.innerHTML = `
            <div class="empty-state">
                <h3>🌱 Start Your HODL Journey</h3>
                <p>Add your first Bitcoin position above to start tracking your portfolio!</p>
                <p style="font-size: 0.9em; color: #888;">
                    💡 Tip: Your data is saved locally and persists between browser sessions
                </p>
            </div>
        `;
        return;
    }
    
    let html = "";
    
    // Sort positions by date (newest first)
    const sortedPositions = [...portfolio].sort((a, b) => new Date(b.buyDate) - new Date(a.buyDate));
    
    sortedPositions.forEach((position, index) => {
        const formattedDate = new Date(position.buyDate).toLocaleDateString();
        
        html += `
            <div class="position-item">
                <div>
                    <div class="position-amount">${position.btcAmount.toFixed(8)} BTC</div>
                    <div class="position-date">${formattedDate}</div>
                </div>
                <div class="position-price">$${position.buyPrice.toLocaleString()}</div>
                <div class="position-price">$${position.totalInvested.toLocaleString()}</div>
                <div class="position-price">${((Date.now() - new Date(position.buyDate)) / (1000 * 60 * 60 * 24)).toFixed(0)} days</div>
                <div>
                    <button class="delete-btn" onclick="deletePosition(${position.id})">
                        🗑️
                    </button>
                </div>
            </div>
        `;
    });
    
    container.innerHTML = html;
}
```

### Step 8: Portfolio Statistics and Analytics
```javascript
function updateStats() {
    if (portfolio.length === 0) {
        document.getElementById("total-btc").textContent = "0.00000000";
        document.getElementById("total-invested").textContent = "$0";
        document.getElementById("avg-price").textContent = "$0";
        document.getElementById("position-count").textContent = "0";
        return;
    }
    
    // Calculate totals
    let totalBTC = 0;
    let totalInvested = 0;
    
    portfolio.forEach(position => {
        totalBTC += position.btcAmount;
        totalInvested += position.totalInvested;
    });
    
    // Calculate average price
    const avgPrice = totalInvested / totalBTC;
    
    // Update display
    document.getElementById("total-btc").textContent = totalBTC.toFixed(8);
    document.getElementById("total-invested").textContent = "$" + totalInvested.toLocaleString();
    document.getElementById("avg-price").textContent = "$" + avgPrice.toLocaleString();
    document.getElementById("position-count").textContent = portfolio.length;
}

function deletePosition(positionId) {
    if (confirm("Are you sure you want to delete this position?")) {
        // Remove from array
        portfolio = portfolio.filter(position => position.id !== positionId);
        
        // Save updated portfolio
        savePortfolio();
        
        // Update display
        displayPortfolio();
        updateStats();
        
        console.log("Position deleted:", positionId);
    }
}
```

### Step 9: Data Import/Export Features
```javascript
function exportData() {
    if (portfolio.length === 0) {
        alert("No data to export!");
        return;
    }
    
    const dataStr = JSON.stringify(portfolio, null, 2);
    const dataBlob = new Blob([dataStr], {type: 'application/json'});
    
    const link = document.createElement('a');
    link.href = URL.createObjectURL(dataBlob);
    link.download = `hodl-portfolio-${new Date().toISOString().split('T')[0]}.json`;
    link.click();
    
    console.log("Portfolio data exported");
}

function importData() {
    const input = document.createElement('input');
    input.type = 'file';
    input.accept = '.json';
    
    input.addEventListener('change', function(event) {
        const file = event.target.files[0];
        if (!file) return;
        
        const reader = new FileReader();
        reader.onload = function(e) {
            try {
                const importedData = JSON.parse(e.target.result);
                
                if (Array.isArray(importedData) && importedData.length > 0) {
                    if (confirm(`Import ${importedData.length} positions? This will replace your current data.`)) {
                        portfolio = importedData;
                        savePortfolio();
                        displayPortfolio();
                        updateStats();
                        alert("Data imported successfully!");
                    }
                } else {
                    alert("Invalid data format");
                }
            } catch (error) {
                alert("Error reading file: " + error.message);
            }
        };
        
        reader.readAsText(file);
    });
    
    input.click();
}

function clearAllData() {
    if (portfolio.length === 0) {
        alert("No data to clear!");
        return;
    }
    
    if (confirm("Are you sure you want to delete ALL positions? This cannot be undone!")) {
        portfolio = [];
        savePortfolio();
        displayPortfolio();
        updateStats();
        alert("All data cleared!");
    }
}
```

### Step 10: Complete Application with Initialization
```javascript
// Initialize the application when page loads
function initializeApp() {
    console.log("🚀 Initializing HODL Portfolio Tracker...");
    
    // Set default date to today
    document.getElementById("buy-date").value = new Date().toISOString().split('T')[0];
    
    // Load saved portfolio
    loadPortfolio();
    
    // Update display
    displayPortfolio();
    updateStats();
    
    console.log("✅ App initialized successfully");
}

// Run when page loads
window.onload = initializeApp;

// Save data before user leaves page (backup)
window.onbeforeunload = function() {
    savePortfolio();
};
```

## Key Takeaways

### localStorage Fundamentals
- **Persistent Storage**: Data survives browser restarts and computer reboots
- **String Only**: Must convert objects to JSON strings for storage
- **5-10MB Limit**: Sufficient for most application data
- **Domain Specific**: Each website has its own storage space

### JSON Operations
- **JSON.stringify()**: Convert objects/arrays to strings for storage
- **JSON.parse()**: Convert JSON strings back to objects/arrays
- **Error Handling**: Always wrap in try/catch for malformed data
- **Data Integrity**: Validate data structure after parsing

### Data Persistence Patterns
- **Load on Start**: Retrieve saved data when application initializes
- **Save on Change**: Update storage whenever data changes
- **Backup on Exit**: Save before user leaves (window.onbeforeunload)
- **Error Recovery**: Handle storage failures gracefully

### State Management
- **Single Source of Truth**: Keep one authoritative data structure
- **Update Flows**: Data changes → save to storage → update display
- **Validation**: Check data integrity at every step
- **User Feedback**: Confirm actions, especially destructive ones

## Real-World Applications

These patterns power modern Bitcoin applications:
- **Wallet Applications**: Saving transaction history and preferences
- **Trading Interfaces**: Storing watchlists and custom settings
- **Portfolio Trackers**: Maintaining investment records
- **DeFi Dashboards**: Tracking yield farming positions
- **Lightning Apps**: Remembering channel states and routing preferences
- **Mining Tools**: Storing rig configurations and historical data

## Challenge Yourself

1. **Current Price Integration**: Add real-time Bitcoin price for P&L calculation
2. **Multiple Assets**: Support other cryptocurrencies beyond Bitcoin
3. **Advanced Analytics**: Add charts showing portfolio performance over time
4. **Backup Integration**: Sync with cloud storage services
5. **Mobile Responsive**: Optimize for mobile portfolio tracking
6. **DCA Tracking**: Add dollar-cost averaging analysis features

## Common Issues and Solutions

### Data Not Persisting
- Check browser privacy settings (some browsers block localStorage)
- Ensure you're calling `savePortfolio()` after data changes
- Verify localStorage isn't full (check browser storage limits)
- Test with browser developer tools → Application → Local Storage

### JSON Errors
- Always wrap `JSON.parse()` in try/catch blocks
- Validate data structure after parsing: `Array.isArray(data)`
- Check for corrupted data: `typeof data === 'object'`
- Provide fallback for invalid data

### Performance Issues
- Avoid saving on every keystroke - save on blur or form submit
- Consider data size - don't store unnecessary information
- Use debouncing for frequent updates
- Clean up old/unnecessary data periodically

## Next Steps

Outstanding work! You've mastered data persistence and built a professional portfolio tracker. In Lesson 8, we'll integrate external libraries to create data visualizations with a mining difficulty chart.

### What's Coming Next
- **Lesson 8**: Using external libraries and CDNs
- **Real-world skills**: Integrating third-party tools
- **New concepts**: Library configuration and data visualization

## Resources for Going Deeper

- [MDN Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [Working with JSON](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)
- [localStorage Best Practices](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

## Share Your Success!

🎉 **You built a persistent portfolio tracker!** 

This demonstrates real Bitcoin application development skills. You're building the same data persistence patterns used in professional financial applications.

Don't forget to:
1. Add some Bitcoin positions and see them persist
2. Try the export/import features
3. Test the data survival across browser sessions
4. Push your code to GitHub
5. Share your HODL tracker with the community

Ready to add beautiful charts and visualizations? Let's integrate Chart.js and build a mining difficulty visualizer! 📊

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 