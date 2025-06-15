[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-5.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-5.mp4)
- [WebM Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/js-lesson-5.webm)

# Lesson 5: Block Explorer Interface

## Welcome to Real-Time Bitcoin Development!

Amazing progress, PlebDev! 🎯 You've mastered the fundamentals and built some seriously cool applications. Now we're diving into one of the most exciting aspects of Bitcoin development: real-time, live-updating interfaces.

In this lesson, you'll build a live block explorer interface that updates automatically - just like the professional block explorers you see online. This introduces you to time-based programming and dynamic updates that make Bitcoin applications feel alive and responsive.

## What You'll Learn

### JavaScript Concepts
- **setTimeout**: Executing code after a delay
- **setInterval**: Running code repeatedly at intervals
- **Date Objects**: Working with time and timestamps
- **Dynamic Updates**: Changing content without page reloads

### Bitcoin Development Skills
- Understanding Bitcoin block timing
- Creating live updating interfaces
- Simulating real-time blockchain data
- Building engaging user experiences

## Prerequisites
- Completed Lessons 1-4
- Understanding of conditional logic and DOM manipulation
- Ready to build dynamic, live applications

## Project Overview: Live Block Height Display

We're building a block explorer that:
- Shows current block height with live updates
- Displays countdown to next block
- Updates timestamps in real-time
- Simulates realistic Bitcoin block timing
- Provides an engaging, professional interface

## Step-by-Step Build

### Step 1: Project Setup
1. Create folder `bitcoin-block-explorer`
2. Open in VS Code
3. Create `index.html`

### Step 2: HTML Foundation
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Block Explorer</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            color: white;
        }
        .explorer-container {
            max-width: 900px;
            margin: 0 auto;
            padding: 40px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        .header {
            text-align: center;
            margin-bottom: 40px;
        }
        .current-block {
            background: rgba(255, 255, 255, 0.15);
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 30px;
        }
        .block-height {
            font-size: 4em;
            font-weight: bold;
            color: #ff9500;
            margin: 20px 0;
            text-shadow: 0 2px 10px rgba(255, 149, 0, 0.5);
        }
        .block-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 30px;
        }
        .info-card {
            background: rgba(255, 255, 255, 0.1);
            padding: 25px;
            border-radius: 12px;
            text-align: center;
        }
        .info-title {
            font-size: 0.9em;
            opacity: 0.8;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .info-value {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff9500;
        }
        .countdown {
            font-size: 2.5em;
            font-weight: bold;
            color: #4CAF50;
            font-family: 'Courier New', monospace;
        }
        .timestamp {
            font-size: 1.1em;
            opacity: 0.9;
            margin-top: 10px;
        }
        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 4px;
            margin-top: 15px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #4CAF50, #ff9500);
            transition: width 1s ease;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 30px;
        }
        .pulse {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="explorer-container">
        <div class="header">
            <h1>⛏️ Bitcoin Block Explorer</h1>
            <p>Live blockchain monitoring and block countdown</p>
        </div>
        
        <div class="current-block">
            <div class="info-title">Current Block Height</div>
            <div id="block-height" class="block-height">0</div>
            <div id="last-update" class="timestamp">Starting up...</div>
        </div>
        
        <div class="block-info">
            <div class="info-card">
                <div class="info-title">Next Block In</div>
                <div id="countdown" class="countdown">10:00</div>
                <div class="progress-bar">
                    <div id="progress-fill" class="progress-fill" style="width: 0%;"></div>
                </div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Average Block Time</div>
                <div class="info-value">~10 minutes</div>
                <div style="font-size: 0.9em; opacity: 0.8; margin-top: 5px;">
                    Target: 600 seconds
                </div>
            </div>
        </div>
        
        <div class="stats-grid">
            <div class="info-card">
                <div class="info-title">Blocks Today</div>
                <div id="blocks-today" class="info-value">0</div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Time Since Genesis</div>
                <div id="time-since-genesis" class="info-value">15+ years</div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Next Difficulty Adjustment</div>
                <div id="next-adjustment" class="info-value">~14 days</div>
            </div>
        </div>
    </div>
    
    <script>
        // Our JavaScript will go here
    </script>
</body>
</html>
```

### Step 3: Understanding Time in JavaScript
```javascript
// Creating dates
const now = new Date();
console.log(now); // Current date and time

// Getting time components
const hours = now.getHours();
const minutes = now.getMinutes();
const seconds = now.getSeconds();

// Formatting time
const timeString = now.toLocaleTimeString();
console.log(timeString); // "10:30:45 AM"

// Timestamps (milliseconds since Jan 1, 1970)
const timestamp = now.getTime();
console.log(timestamp); // 1642694445000
```

### Step 4: Understanding setTimeout and setInterval
```javascript
// setTimeout - run code once after a delay
setTimeout(function() {
    console.log("This runs after 3 seconds");
}, 3000);

// setInterval - run code repeatedly
const intervalId = setInterval(function() {
    console.log("This runs every second");
}, 1000);

// Stop an interval
clearInterval(intervalId);
```

### Step 5: Building the Block Counter Logic
```javascript
// Global variables for our block data
let currentBlock = 825000; // Starting block height
let secondsLeft = 600; // 10 minutes in seconds
let totalBlockTime = 600; // For progress calculation

// Update block height and reset countdown
function updateBlock() {
    currentBlock++;
    secondsLeft = 600; // Reset to 10 minutes
    
    // Update the display
    document.getElementById("block-height").textContent = currentBlock.toLocaleString();
    
    // Update timestamp
    const now = new Date();
    document.getElementById("last-update").textContent = 
        `Last updated: ${now.toLocaleTimeString()}`;
    
    // Add visual feedback
    const blockElement = document.getElementById("block-height");
    blockElement.classList.add("pulse");
    setTimeout(() => {
        blockElement.classList.remove("pulse");
    }, 2000);
    
    console.log(`New block mined! Block #${currentBlock}`);
}
```

### Step 6: Creating the Countdown Function
```javascript
function updateCountdown() {
    secondsLeft--;
    
    // Check if we should mine a new block
    if (secondsLeft <= 0) {
        updateBlock();
        return; // Exit early, countdown will reset
    }
    
    // Calculate minutes and seconds
    const minutes = Math.floor(secondsLeft / 60);
    const seconds = secondsLeft % 60;
    
    // Format with leading zeros
    const formattedTime = `${minutes}:${seconds.toString().padStart(2, '0')}`;
    
    // Update countdown display
    document.getElementById("countdown").textContent = formattedTime;
    
    // Update progress bar
    const progress = ((totalBlockTime - secondsLeft) / totalBlockTime) * 100;
    document.getElementById("progress-fill").style.width = progress + "%";
}
```

### Step 7: Adding Statistics and Polish
```javascript
function updateStats() {
    // Calculate blocks mined today (simplified)
    const blocksToday = Math.floor(Math.random() * 10) + 140; // ~144 blocks per day
    document.getElementById("blocks-today").textContent = blocksToday;
    
    // Calculate time since Bitcoin genesis (January 3, 2009)
    const genesis = new Date('2009-01-03');
    const now = new Date();
    const diffTime = Math.abs(now - genesis);
    const diffYears = Math.floor(diffTime / (1000 * 60 * 60 * 24 * 365));
    
    document.getElementById("time-since-genesis").textContent = `${diffYears}+ years`;
    
    // Next difficulty adjustment (simplified - every 2016 blocks)
    const blocksUntilAdjustment = 2016 - (currentBlock % 2016);
    const daysUntilAdjustment = Math.floor(blocksUntilAdjustment * 10 / 1440); // 10 min per block, 1440 min per day
    
    document.getElementById("next-adjustment").textContent = `~${daysUntilAdjustment} days`;
}
```

### Step 8: Complete Working Application
```html
<!DOCTYPE html>
<html>
<head>
    <title>Bitcoin Block Explorer</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            color: white;
        }
        .explorer-container {
            max-width: 900px;
            margin: 0 auto;
            padding: 40px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            backdrop-filter: blur(10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        .header {
            text-align: center;
            margin-bottom: 40px;
        }
        .current-block {
            background: rgba(255, 255, 255, 0.15);
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 30px;
        }
        .block-height {
            font-size: 4em;
            font-weight: bold;
            color: #ff9500;
            margin: 20px 0;
            text-shadow: 0 2px 10px rgba(255, 149, 0, 0.5);
        }
        .block-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 30px;
        }
        .info-card {
            background: rgba(255, 255, 255, 0.1);
            padding: 25px;
            border-radius: 12px;
            text-align: center;
        }
        .info-title {
            font-size: 0.9em;
            opacity: 0.8;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .info-value {
            font-size: 1.8em;
            font-weight: bold;
            color: #ff9500;
        }
        .countdown {
            font-size: 2.5em;
            font-weight: bold;
            color: #4CAF50;
            font-family: 'Courier New', monospace;
        }
        .timestamp {
            font-size: 1.1em;
            opacity: 0.9;
            margin-top: 10px;
        }
        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 4px;
            margin-top: 15px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #4CAF50, #ff9500);
            transition: width 1s ease;
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 30px;
        }
        .pulse {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }
        .control-panel {
            text-align: center;
            margin-top: 30px;
        }
        .control-btn {
            background: rgba(255, 149, 0, 0.2);
            border: 2px solid #ff9500;
            color: #ff9500;
            padding: 10px 20px;
            margin: 5px;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        .control-btn:hover {
            background: #ff9500;
            color: white;
        }
    </style>
</head>
<body>
    <div class="explorer-container">
        <div class="header">
            <h1>⛏️ Bitcoin Block Explorer</h1>
            <p>Live blockchain monitoring and block countdown</p>
        </div>
        
        <div class="current-block">
            <div class="info-title">Current Block Height</div>
            <div id="block-height" class="block-height">0</div>
            <div id="last-update" class="timestamp">Starting up...</div>
        </div>
        
        <div class="block-info">
            <div class="info-card">
                <div class="info-title">Next Block In</div>
                <div id="countdown" class="countdown">10:00</div>
                <div class="progress-bar">
                    <div id="progress-fill" class="progress-fill" style="width: 0%;"></div>
                </div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Average Block Time</div>
                <div class="info-value">~10 minutes</div>
                <div style="font-size: 0.9em; opacity: 0.8; margin-top: 5px;">
                    Target: 600 seconds
                </div>
            </div>
        </div>
        
        <div class="stats-grid">
            <div class="info-card">
                <div class="info-title">Blocks Today</div>
                <div id="blocks-today" class="info-value">0</div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Time Since Genesis</div>
                <div id="time-since-genesis" class="info-value">15+ years</div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Network Hashrate</div>
                <div id="hashrate" class="info-value">0 EH/s</div>
            </div>
            
            <div class="info-card">
                <div class="info-title">Next Difficulty Adjustment</div>
                <div id="next-adjustment" class="info-value">~14 days</div>
            </div>
        </div>
        
        <div class="control-panel">
            <button class="control-btn" onclick="fastForward()">⏩ Fast Forward</button>
            <button class="control-btn" onclick="addRandomDelay()">🎲 Random Timing</button>
            <button class="control-btn" onclick="resetExplorer()">🔄 Reset</button>
        </div>
    </div>
    
    <script>
        // Global variables for our block explorer
        let currentBlock = 825000; // Starting block height
        let secondsLeft = 600; // 10 minutes in seconds
        const totalBlockTime = 600; // For progress calculation
        let countdownInterval;
        let isRealTime = true;
        
        // Initialize the explorer
        function initializeExplorer() {
            updateBlock();
            updateStats();
            startCountdown();
        }
        
        // Start the countdown timer
        function startCountdown() {
            if (countdownInterval) {
                clearInterval(countdownInterval);
            }
            countdownInterval = setInterval(updateCountdown, 1000);
        }
        
        // Update block height and reset countdown
        function updateBlock() {
            currentBlock++;
            secondsLeft = 600; // Reset to 10 minutes
            
            // Update the display
            document.getElementById("block-height").textContent = currentBlock.toLocaleString();
            
            // Update timestamp
            const now = new Date();
            document.getElementById("last-update").textContent = 
                `Block found at: ${now.toLocaleTimeString()}`;
            
            // Add visual feedback
            const blockElement = document.getElementById("block-height");
            blockElement.classList.add("pulse");
            setTimeout(() => {
                blockElement.classList.remove("pulse");
            }, 2000);
            
            // Update statistics
            updateStats();
            
            console.log(`⛏️ New block mined! Block #${currentBlock}`);
        }
        
        // Update countdown display
        function updateCountdown() {
            secondsLeft--;
            
            // Check if we should mine a new block
            if (secondsLeft <= 0) {
                updateBlock();
                return; // Exit early, countdown will reset
            }
            
            // Calculate minutes and seconds
            const minutes = Math.floor(secondsLeft / 60);
            const seconds = secondsLeft % 60;
            
            // Format with leading zeros
            const formattedTime = `${minutes}:${seconds.toString().padStart(2, '0')}`;
            
            // Update countdown display
            document.getElementById("countdown").textContent = formattedTime;
            
            // Update progress bar
            const progress = ((totalBlockTime - secondsLeft) / totalBlockTime) * 100;
            document.getElementById("progress-fill").style.width = progress + "%";
        }
        
        // Update statistics
        function updateStats() {
            // Calculate blocks mined today (144 blocks per day target)
            const blocksToday = Math.floor(Math.random() * 20) + 135;
            document.getElementById("blocks-today").textContent = blocksToday;
            
            // Calculate time since Bitcoin genesis (January 3, 2009)
            const genesis = new Date('2009-01-03');
            const now = new Date();
            const diffTime = Math.abs(now - genesis);
            const diffYears = Math.floor(diffTime / (1000 * 60 * 60 * 24 * 365));
            
            document.getElementById("time-since-genesis").textContent = `${diffYears}+ years`;
            
            // Network hashrate (simulated)
            const hashrate = (Math.random() * 200 + 300).toFixed(0);
            document.getElementById("hashrate").textContent = `${hashrate} EH/s`;
            
            // Next difficulty adjustment (every 2016 blocks)
            const blocksUntilAdjustment = 2016 - (currentBlock % 2016);
            const daysUntilAdjustment = Math.floor(blocksUntilAdjustment * 10 / 1440);
            
            document.getElementById("next-adjustment").textContent = `~${daysUntilAdjustment} days`;
        }
        
        // Control functions for interaction
        function fastForward() {
            secondsLeft = Math.min(secondsLeft, 30); // Speed up to 30 seconds
            console.log("⏩ Fast forwarding to next block...");
        }
        
        function addRandomDelay() {
            const randomDelay = Math.floor(Math.random() * 300) + 300; // 5-10 minutes
            secondsLeft = randomDelay;
            console.log(`🎲 Random block time: ${Math.floor(randomDelay/60)}:${(randomDelay%60).toString().padStart(2, '0')}`);
        }
        
        function resetExplorer() {
            currentBlock = 825000;
            secondsLeft = 600;
            updateBlock();
            console.log("🔄 Explorer reset to starting values");
        }
        
        // Initialize when page loads
        window.onload = function() {
            console.log("🚀 Bitcoin Block Explorer starting...");
            initializeExplorer();
        };
        
        // Clean up intervals when page unloads
        window.onbeforeunload = function() {
            if (countdownInterval) {
                clearInterval(countdownInterval);
            }
        };
    </script>
</body>
</html>
```

## Key Takeaways

### setTimeout and setInterval
- **setTimeout(fn, delay)**: Run code once after a delay
- **setInterval(fn, interval)**: Run code repeatedly at intervals  
- **clearInterval(id)**: Stop a running interval
- **Real-time updates**: Make applications feel alive and responsive

### Date Objects and Time
- **new Date()**: Current date and time
- **getHours(), getMinutes(), getSeconds()**: Time components
- **toLocaleTimeString()**: User-friendly time formatting
- **Time calculations**: Math with dates and durations

### Dynamic Updates
- **Live data**: Change content without page reloads
- **Visual feedback**: Use animations and transitions
- **Progress indicators**: Show time-based progress
- **User engagement**: Keep interfaces feeling alive

### Performance Considerations
- **Clean up intervals**: Prevent memory leaks
- **Efficient updates**: Only change what's necessary
- **Resource management**: Stop timers when not needed

## Real-World Applications

These patterns are essential in Bitcoin development:
- **Block Explorers**: Real-time blockchain monitoring
- **Trading Interfaces**: Live price updates and order books
- **Wallet Applications**: Transaction status monitoring
- **Lightning Network**: Channel state updates
- **Mining Dashboards**: Hashrate and difficulty tracking
- **DeFi Protocols**: Yield farming and liquidity updates

## Challenge Yourself

1. **Historical Data**: Show previous block times and compare to average
2. **Difficulty Visualization**: Add difficulty adjustment countdown
3. **Transaction Mempool**: Simulate mempool size changes
4. **Price Integration**: Show Bitcoin price updates alongside blocks
5. **Sound Notifications**: Add audio alerts for new blocks
6. **Mobile Responsive**: Optimize for mobile devices

## Common Issues and Solutions

### Timers Not Working
- Check that intervals are being set correctly
- Use `clearInterval()` before setting new intervals
- Ensure functions are defined before being called

### Memory Leaks
- Always clean up intervals with `clearInterval()`
- Stop timers when navigating away from page
- Use `window.onbeforeunload` for cleanup

### Time Formatting Issues
- Use `padStart(2, '0')` for leading zeros: "05" instead of "5"
- Handle edge cases: what happens when seconds become negative?
- Test with different time zones and locales

## Next Steps

Fantastic work! You've mastered time-based programming and real-time interfaces. In Lesson 6, we'll dive into string manipulation and regular expressions by building a Lightning invoice generator.

### What's Coming Next
- **Lesson 6**: String methods and pattern matching
- **Real-world skills**: Parsing and validating Bitcoin data
- **New concepts**: Regular expressions and input sanitization

## Resources for Going Deeper

- [MDN Date Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [setTimeout and setInterval Guide](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)
- [Time and Date in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Numbers_and_dates)

## Share Your Success!

🎉 **You built a live block explorer!** 

This demonstrates advanced Bitcoin development skills used in professional blockchain applications. You're working with real-time data patterns that power major Bitcoin services.

Don't forget to:
1. Watch your block explorer update in real-time
2. Try the interactive controls
3. Push your code to GitHub
4. Share your explorer with the community

Ready to master string manipulation and Bitcoin data parsing? Let's build a Lightning invoice generator! ⚡

---

*Questions? Need help? Reach out to the PlebDevs community at plebdevs.com* 