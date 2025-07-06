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

## Key Concepts Explained

This lesson introduces asynchronous programming - the foundation of modern Bitcoin applications that interact with networks, APIs, and real-time data:

### Promises: Professional Asynchronous Operations
Promises represent future values and are essential for Bitcoin applications that fetch data from APIs:

```javascript
// Basic Promise creation
const fetchBitcoinPrice = new Promise((resolve, reject) => {
    // Simulate API call delay
    setTimeout(() => {
        const success = Math.random() > 0.1; // 90% success rate
        
        if (success) {
            const price = Math.floor(Math.random() * 10000) + 90000;
            resolve({ 
                price: price, 
                currency: 'USD',
                timestamp: Date.now(),
                source: 'simulated-api'
            });
        } else {
            reject(new Error("API rate limit exceeded"));
        }
    }, Math.random() * 2000 + 500); // 500-2500ms delay
});

// Using promises with .then() and .catch()
fetchBitcoinPrice
    .then(data => {
        console.log(`Bitcoin price: $${data.price.toLocaleString()}`);
        return data; // Pass data to next .then()
    })
    .then(data => {
        // Chain additional operations
        const formattedTime = new Date(data.timestamp).toLocaleTimeString();
        console.log(`Updated at: ${formattedTime}`);
    })
    .catch(error => {
        console.error('Price fetch failed:', error.message);
    })
    .finally(() => {
        console.log('Price fetch attempt completed');
    });

// Advanced Promise patterns
class BitcoinAPIClient {
    constructor(baseURL = 'https://api.example.com') {
        this.baseURL = baseURL;
        this.requestCount = 0;
        this.maxRetries = 3;
    }
    
    // Create a promise-based HTTP request
    request(endpoint, options = {}) {
        return new Promise((resolve, reject) => {
            this.requestCount++;
            
            // Simulate network conditions
            const delay = Math.random() * 1000 + 200;
            const failureRate = options.failureRate || 0.1;
            
            setTimeout(() => {
                if (Math.random() < failureRate) {
                    reject(new Error(`Network error on ${endpoint}`));
                    return;
                }
                
                // Simulate different response types
                const mockData = this.generateMockData(endpoint);
                resolve({
                    data: mockData,
                    status: 200,
                    headers: { 'content-type': 'application/json' },
                    requestId: this.requestCount
                });
            }, delay);
        });
    }
    
    // Promise-based retry logic
    async requestWithRetry(endpoint, options = {}) {
        let lastError;
        
        for (let attempt = 1; attempt <= this.maxRetries; attempt++) {
            try {
                const result = await this.request(endpoint, options);
                console.log(`Request succeeded on attempt ${attempt}`);
                return result;
            } catch (error) {
                lastError = error;
                console.warn(`Attempt ${attempt} failed:`, error.message);
                
                if (attempt < this.maxRetries) {
                    // Exponential backoff
                    const delay = Math.pow(2, attempt) * 1000;
                    await this.delay(delay);
                }
            }
        }
        
        throw new Error(`All ${this.maxRetries} attempts failed. Last error: ${lastError.message}`);
    }
    
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
    
    generateMockData(endpoint) {
        const mockResponses = {
            '/mempool/stats': {
                size: Math.floor(Math.random() * 50000) + 10000,
                vsize: Math.floor(Math.random() * 30000) + 8000,
                total_fee: Math.floor(Math.random() * 500) + 100,
                fee_histogram: this.generateFeeHistogram()
            },
            '/fees/recommended': {
                fastestFee: Math.floor(Math.random() * 50) + 20,
                halfHourFee: Math.floor(Math.random() * 30) + 10,
                hourFee: Math.floor(Math.random() * 20) + 5,
                economyFee: Math.floor(Math.random() * 10) + 2
            },
            '/blocks/tip/height': {
                height: 825000 + Math.floor(Math.random() * 100)
            }
        };
        
        return mockResponses[endpoint] || { message: 'Mock data not available' };
    }
    
    generateFeeHistogram() {
        const histogram = [];
        for (let i = 1; i <= 50; i++) {
            histogram.push([i, Math.floor(Math.random() * 1000)]);
        }
        return histogram;
    }
}

// Promise.all for concurrent requests
async function fetchAllBitcoinData() {
    const client = new BitcoinAPIClient();
    
    try {
        // Execute multiple requests concurrently
        const [mempoolStats, feeRecommendations, blockHeight] = await Promise.all([
            client.request('/mempool/stats'),
            client.request('/fees/recommended'),
            client.request('/blocks/tip/height')
        ]);
        
        return {
            mempool: mempoolStats.data,
            fees: feeRecommendations.data,
            height: blockHeight.data,
            fetchedAt: new Date().toISOString()
        };
    } catch (error) {
        console.error('Failed to fetch all data:', error);
        throw error;
    }
}

// Promise.allSettled for handling partial failures
async function fetchDataWithPartialFailures() {
    const client = new BitcoinAPIClient();
    
    const requests = [
        client.request('/mempool/stats'),
        client.request('/fees/recommended', { failureRate: 0.5 }), // Higher failure rate
        client.request('/blocks/tip/height')
    ];
    
    const results = await Promise.allSettled(requests);
    
    const successfulData = {};
    const errors = [];
    
    results.forEach((result, index) => {
        const endpoints = ['/mempool/stats', '/fees/recommended', '/blocks/tip/height'];
        const endpoint = endpoints[index];
        
        if (result.status === 'fulfilled') {
            successfulData[endpoint] = result.value.data;
        } else {
            errors.push({ endpoint, error: result.reason.message });
        }
    });
    
    return { data: successfulData, errors };
}

### Async/Await: Modern Promise Handling
Async/await syntax makes asynchronous code more readable and maintainable:

```javascript
// Traditional promise chains vs async/await
// Old way - promise chains
function fetchAndProcessDataOldWay() {
    return fetchBitcoinPrice()
        .then(priceData => {
            console.log('Got price:', priceData.price);
            return fetchMempoolData();
        })
        .then(mempoolData => {
            console.log('Got mempool:', mempoolData.size);
            return calculateFeeRecommendation(mempoolData);
        })
        .then(feeData => {
            console.log('Calculated fees:', feeData);
            return feeData;
        })
        .catch(error => {
            console.error('Chain failed:', error);
            throw error;
        });
}

// Modern way - async/await
async function fetchAndProcessDataModernWay() {
    try {
        const priceData = await fetchBitcoinPrice();
        console.log('Got price:', priceData.price);
        
        const mempoolData = await fetchMempoolData();
        console.log('Got mempool:', mempoolData.size);
        
        const feeData = await calculateFeeRecommendation(mempoolData);
        console.log('Calculated fees:', feeData);
        
        return feeData;
    } catch (error) {
        console.error('Process failed:', error);
        throw error;
    }
}

// Advanced async/await patterns
class MempoolMonitor {
    constructor() {
        this.isRunning = false;
        this.updateInterval = 30000; // 30 seconds
        this.retryDelay = 5000; // 5 seconds
        this.maxConsecutiveErrors = 5;
        this.consecutiveErrors = 0;
    }
    
    async start() {
        if (this.isRunning) {
            console.warn('Monitor already running');
            return;
        }
        
        this.isRunning = true;
        console.log('Starting mempool monitor...');
        
        // Initial fetch
        await this.fetchAndUpdate();
        
        // Set up recurring updates
        this.scheduleNextUpdate();
    }
    
    async stop() {
        this.isRunning = false;
        if (this.timeoutId) {
            clearTimeout(this.timeoutId);
        }
        console.log('Mempool monitor stopped');
    }
    
    async fetchAndUpdate() {
        if (!this.isRunning) return;
        
        try {
            this.updateStatus('fetching');
            
            // Fetch data with timeout
            const data = await this.fetchWithTimeout(this.fetchMempoolData(), 10000);
            
            // Update UI
            this.updateDisplay(data);
            this.updateStatus('connected');
            
            // Reset error counter on success
            this.consecutiveErrors = 0;
            
        } catch (error) {
            this.handleFetchError(error);
        }
    }
    
    async fetchMempoolData() {
        // Simulate API call
        const response = await this.simulateAPICall();
        
        // Validate response
        if (!response || typeof response.size !== 'number') {
            throw new Error('Invalid API response format');
        }
        
        return response;
    }
    
    async simulateAPICall() {
        // Simulate network delay
        await this.delay(Math.random() * 1000 + 200);
        
        // Simulate occasional failures
        if (Math.random() < 0.1) {
            const errors = [
                'Network timeout',
                'API rate limit exceeded',
                'Service temporarily unavailable',
                'Invalid API key'
            ];
            throw new Error(errors[Math.floor(Math.random() * errors.length)]);
        }
        
        // Return mock data
        return {
            size: Math.floor(Math.random() * 50000) + 10000,
            vsize: Math.floor(Math.random() * 30000) + 8000,
            fees: {
                fast: Math.floor(Math.random() * 50) + 20,
                medium: Math.floor(Math.random() * 30) + 10,
                slow: Math.floor(Math.random() * 20) + 5
            },
            timestamp: Date.now()
        };
    }
    
    async fetchWithTimeout(promise, timeoutMs) {
        const timeoutPromise = new Promise((_, reject) => {
            setTimeout(() => reject(new Error('Request timeout')), timeoutMs);
        });
        
        return Promise.race([promise, timeoutPromise]);
    }
    
    handleFetchError(error) {
        this.consecutiveErrors++;
        console.error(`Fetch error (${this.consecutiveErrors}/${this.maxConsecutiveErrors}):`, error.message);
        
        this.updateStatus('error', error.message);
        
        if (this.consecutiveErrors >= this.maxConsecutiveErrors) {
            console.error('Too many consecutive errors, stopping monitor');
            this.stop();
            return;
        }
        
        // Schedule retry with exponential backoff
        const retryDelay = this.retryDelay * Math.pow(2, this.consecutiveErrors - 1);
        console.log(`Retrying in ${retryDelay / 1000} seconds...`);
        
        this.timeoutId = setTimeout(() => {
            this.fetchAndUpdate();
        }, retryDelay);
    }
    
    scheduleNextUpdate() {
        if (!this.isRunning) return;
        
        this.timeoutId = setTimeout(() => {
            this.fetchAndUpdate();
            this.scheduleNextUpdate();
        }, this.updateInterval);
    }
    
    updateDisplay(data) {
        // Update DOM elements
        const elements = {
            size: document.getElementById('mempool-size'),
            fastFee: document.getElementById('fast-fee'),
            mediumFee: document.getElementById('medium-fee'),
            slowFee: document.getElementById('slow-fee'),
            timestamp: document.getElementById('last-update')
        };
        
        if (elements.size) {
            elements.size.textContent = data.size.toLocaleString();
        }
        
        if (elements.fastFee) {
            elements.fastFee.textContent = `${data.fees.fast} sat/vB`;
        }
        
        if (elements.mediumFee) {
            elements.mediumFee.textContent = `${data.fees.medium} sat/vB`;
        }
        
        if (elements.slowFee) {
            elements.slowFee.textContent = `${data.fees.slow} sat/vB`;
        }
        
        if (elements.timestamp) {
            elements.timestamp.textContent = new Date(data.timestamp).toLocaleTimeString();
        }
    }
    
    updateStatus(status, message = '') {
        const statusElement = document.getElementById('connection-status');
        if (!statusElement) return;
        
        statusElement.className = `status ${status}`;
        
        const statusMessages = {
            fetching: '🔄 Fetching data...',
            connected: '✅ Connected',
            error: `❌ Error: ${message}`,
            stopped: '⏸️ Stopped'
        };
        
        statusElement.textContent = statusMessages[status] || status;
    }
    
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}

// Concurrent async operations
async function fetchMultipleBitcoinAPIs() {
    const apis = [
        { name: 'BlockchainInfo', url: '/api/blockchain-info' },
        { name: 'Mempool.space', url: '/api/mempool-space' },
        { name: 'Blockstream', url: '/api/blockstream' }
    ];
    
    // Start all requests concurrently
    const requests = apis.map(async (api) => {
        try {
            const startTime = Date.now();
            const response = await fetch(api.url);
            const data = await response.json();
            const duration = Date.now() - startTime;
            
            return {
                api: api.name,
                success: true,
                data: data,
                duration: duration
            };
        } catch (error) {
            return {
                api: api.name,
                success: false,
                error: error.message,
                duration: null
            };
        }
    });
    
    // Wait for all to complete
    const results = await Promise.all(requests);
    
    // Process results
    const successful = results.filter(r => r.success);
    const failed = results.filter(r => !r.success);
    
    console.log(`${successful.length}/${apis.length} APIs responded successfully`);
    
    return {
        successful,
        failed,
        fastest: successful.reduce((fastest, current) => 
            current.duration < fastest.duration ? current : fastest
        )
    };
}

### Error Handling: Professional Resilience Patterns
Robust error handling is crucial for Bitcoin applications dealing with network requests:

```javascript
// Comprehensive error handling strategies
class BitcoinAPIErrorHandler {
    constructor() {
        this.errorCounts = new Map();
        this.circuitBreakers = new Map();
    }
    
    // Categorize different types of errors
    categorizeError(error) {
        if (error.name === 'TypeError' && error.message.includes('fetch')) {
            return 'NETWORK_ERROR';
        }
        
        if (error.message.includes('timeout')) {
            return 'TIMEOUT_ERROR';
        }
        
        if (error.message.includes('rate limit')) {
            return 'RATE_LIMIT_ERROR';
        }
        
        if (error.message.includes('404')) {
            return 'NOT_FOUND_ERROR';
        }
        
        if (error.message.includes('500') || error.message.includes('502')) {
            return 'SERVER_ERROR';
        }
        
        return 'UNKNOWN_ERROR';
    }
    
    // Handle errors with appropriate strategies
    async handleError(error, context = {}) {
        const errorType = this.categorizeError(error);
        const errorKey = `${context.endpoint || 'unknown'}_${errorType}`;
        
        // Track error frequency
        this.errorCounts.set(errorKey, (this.errorCounts.get(errorKey) || 0) + 1);
        
        console.error(`Error in ${context.operation || 'operation'}:`, {
            type: errorType,
            message: error.message,
            count: this.errorCounts.get(errorKey),
            context
        });
        
        // Apply error-specific handling
        switch (errorType) {
            case 'NETWORK_ERROR':
                return this.handleNetworkError(error, context);
            
            case 'TIMEOUT_ERROR':
                return this.handleTimeoutError(error, context);
            
            case 'RATE_LIMIT_ERROR':
                return this.handleRateLimitError(error, context);
            
            case 'SERVER_ERROR':
                return this.handleServerError(error, context);
            
            default:
                return this.handleUnknownError(error, context);
        }
    }
    
    async handleNetworkError(error, context) {
        // Check if we should use circuit breaker
        if (this.shouldTripCircuitBreaker(context.endpoint)) {
            throw new Error('Circuit breaker open - too many network failures');
        }
        
        // Try alternative endpoints if available
        if (context.alternativeEndpoints && context.alternativeEndpoints.length > 0) {
            console.log('Trying alternative endpoint...');
            return { shouldRetry: true, useAlternative: true };
        }
        
        // Suggest offline mode
        return {
            shouldRetry: false,
            userMessage: 'Network connection failed. Please check your internet connection.',
            suggestOfflineMode: true
        };
    }
    
    async handleTimeoutError(error, context) {
        const timeoutCount = this.errorCounts.get(`${context.endpoint}_TIMEOUT_ERROR`) || 0;
        
        if (timeoutCount < 3) {
            // Increase timeout for retry
            return {
                shouldRetry: true,
                retryDelay: 2000 * timeoutCount,
                increaseTimeout: true
            };
        }
        
        return {
            shouldRetry: false,
            userMessage: 'Server is responding slowly. Please try again later.'
        };
    }
    
    async handleRateLimitError(error, context) {
        // Extract retry-after header if available
        const retryAfter = this.extractRetryAfter(error);
        
        return {
            shouldRetry: true,
            retryDelay: retryAfter || 60000, // Default 1 minute
            userMessage: 'API rate limit reached. Retrying automatically...'
        };
    }
    
    async handleServerError(error, context) {
        const serverErrorCount = this.errorCounts.get(`${context.endpoint}_SERVER_ERROR`) || 0;
        
        if (serverErrorCount < 2) {
            return {
                shouldRetry: true,
                retryDelay: 5000,
                userMessage: 'Server temporarily unavailable. Retrying...'
            };
        }
        
        return {
            shouldRetry: false,
            userMessage: 'Service is currently unavailable. Please try again later.',
            suggestCachedData: true
        };
    }
    
    async handleUnknownError(error, context) {
        return {
            shouldRetry: false,
            userMessage: 'An unexpected error occurred. Please refresh the page.',
            logError: true
        };
    }
    
    shouldTripCircuitBreaker(endpoint) {
        const errorCount = this.errorCounts.get(`${endpoint}_NETWORK_ERROR`) || 0;
        return errorCount >= 5;
    }
    
    extractRetryAfter(error) {
        // Try to extract retry-after from error message
        const match = error.message.match(/retry after (\d+)/i);
        return match ? parseInt(match[1]) * 1000 : null;
    }
    
    // Reset error counts (call periodically)
    resetErrorCounts() {
        this.errorCounts.clear();
        console.log('Error counts reset');
    }
}

// Retry mechanism with exponential backoff
async function retryWithBackoff(asyncFunction, maxRetries = 3, baseDelay = 1000) {
    let lastError;
    
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await asyncFunction();
        } catch (error) {
            lastError = error;
            
            if (attempt === maxRetries) {
                break; // Don't delay on final attempt
            }
            
            const delay = baseDelay * Math.pow(2, attempt - 1);
            const jitter = Math.random() * 0.1 * delay; // Add 10% jitter
            const totalDelay = delay + jitter;
            
            console.log(`Attempt ${attempt} failed, retrying in ${Math.round(totalDelay)}ms...`);
            await new Promise(resolve => setTimeout(resolve, totalDelay));
        }
    }
    
    throw new Error(`All ${maxRetries} attempts failed. Last error: ${lastError.message}`);
}

// Usage example
async function fetchMempoolDataWithRetry() {
    const errorHandler = new BitcoinAPIErrorHandler();
    
    return retryWithBackoff(async () => {
        try {
            const response = await fetch('/api/mempool');
            
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            
            return await response.json();
        } catch (error) {
            const handling = await errorHandler.handleError(error, {
                endpoint: '/api/mempool',
                operation: 'fetchMempoolData'
            });
            
            if (!handling.shouldRetry) {
                throw error;
            }
            
            // If we should retry, the retry mechanism will handle it
            throw error;
        }
    }, 3, 1000);
}

### API Simulation: Professional Testing Patterns
Create realistic API simulations for development and testing:

```javascript
// Advanced API simulation for Bitcoin applications
class BitcoinAPISimulator {
    constructor() {
        this.baseLatency = 200; // Base network latency in ms
        this.failureRate = 0.05; // 5% failure rate
        this.rateLimitWindow = 60000; // 1 minute
        this.rateLimitRequests = 100;
        this.requestCounts = new Map();
        
        // Simulate realistic Bitcoin data
        this.currentBlockHeight = 825000;
        this.currentDifficulty = 52.35;
        this.mempoolSize = 25000;
        this.lastBlockTime = Date.now() - (Math.random() * 600000); // Last block 0-10 min ago
    }
    
    // Simulate mempool API endpoint
    async getMempoolStats() {
        await this.simulateNetworkDelay();
        this.checkRateLimit('mempool');
        this.simulateRandomFailure();
        
        // Simulate realistic mempool fluctuations
        const sizeVariation = (Math.random() - 0.5) * 0.2; // ±10%
        this.mempoolSize = Math.max(1000, this.mempoolSize * (1 + sizeVariation));
        
        return {
            size: Math.round(this.mempoolSize),
            vsize: Math.round(this.mempoolSize * 0.7),
            total_fee: Math.round(this.mempoolSize * 0.001 * Math.random() * 50),
            fee_histogram: this.generateRealisticFeeHistogram(),
            timestamp: Date.now()
        };
    }
    
    // Simulate fee recommendation API
    async getFeeRecommendations() {
        await this.simulateNetworkDelay();
        this.checkRateLimit('fees');
        this.simulateRandomFailure();
        
        // Base fees on mempool congestion
        const congestionMultiplier = Math.max(0.5, this.mempoolSize / 30000);
        
        return {
            fastestFee: Math.round((20 + Math.random() * 30) * congestionMultiplier),
            halfHourFee: Math.round((10 + Math.random() * 20) * congestionMultiplier),
            hourFee: Math.round((5 + Math.random() * 15) * congestionMultiplier),
            economyFee: Math.round((2 + Math.random() * 8) * congestionMultiplier),
            minimumFee: 1
        };
    }
    
    // Simulate block height API
    async getBlockHeight() {
        await this.simulateNetworkDelay();
        this.checkRateLimit('blocks');
        this.simulateRandomFailure();
        
        // Simulate new blocks (average 10 minutes)
        const timeSinceLastBlock = Date.now() - this.lastBlockTime;
        if (timeSinceLastBlock > 600000 && Math.random() < 0.1) { // 10% chance per call after 10 min
            this.currentBlockHeight++;
            this.lastBlockTime = Date.now();
            this.mempoolSize *= 0.7; // Reduce mempool size after new block
        }
        
        return {
            height: this.currentBlockHeight,
            hash: this.generateBlockHash(),
            timestamp: this.lastBlockTime,
            time_since_last: Math.round(timeSinceLastBlock / 1000)
        };
    }
    
    // Simulate network conditions
    async simulateNetworkDelay() {
        // Simulate variable network conditions
        const networkCondition = Math.random();
        let delay;
        
        if (networkCondition < 0.7) {
            // Good connection (70%)
            delay = this.baseLatency + Math.random() * 200;
        } else if (networkCondition < 0.9) {
            // Slow connection (20%)
            delay = this.baseLatency + Math.random() * 1000 + 500;
        } else {
            // Very slow connection (10%)
            delay = this.baseLatency + Math.random() * 3000 + 1000;
        }
        
        await new Promise(resolve => setTimeout(resolve, delay));
    }
    
    // Simulate API failures
    simulateRandomFailure() {
        if (Math.random() < this.failureRate) {
            const errors = [
                'Service temporarily unavailable',
                'Rate limit exceeded',
                'Internal server error',
                'Gateway timeout',
                'Network connection failed'
            ];
            
            const randomError = errors[Math.floor(Math.random() * errors.length)];
            throw new Error(randomError);
        }
    }
    
    // Simulate rate limiting
    checkRateLimit(endpoint) {
        const now = Date.now();
        const windowStart = now - this.rateLimitWindow;
        
        // Clean old requests
        if (!this.requestCounts.has(endpoint)) {
            this.requestCounts.set(endpoint, []);
        }
        
        const requests = this.requestCounts.get(endpoint);
        const recentRequests = requests.filter(time => time > windowStart);
        
        if (recentRequests.length >= this.rateLimitRequests) {
            throw new Error(`Rate limit exceeded for ${endpoint}. Try again in ${Math.round((this.rateLimitWindow - (now - recentRequests[0])) / 1000)} seconds.`);
        }
        
        recentRequests.push(now);
        this.requestCounts.set(endpoint, recentRequests);
    }
    
    // Generate realistic fee histogram
    generateRealisticFeeHistogram() {
        const histogram = [];
        
        // Create realistic fee distribution
        for (let feeRate = 1; feeRate <= 100; feeRate++) {
            let txCount;
            
            if (feeRate <= 5) {
                // Many low-fee transactions
                txCount = Math.floor(Math.random() * 2000) + 500;
            } else if (feeRate <= 20) {
                // Moderate number of medium-fee transactions
                txCount = Math.floor(Math.random() * 1000) + 200;
            } else if (feeRate <= 50) {
                // Fewer high-fee transactions
                txCount = Math.floor(Math.random() * 500) + 50;
            } else {
                // Very few very-high-fee transactions
                txCount = Math.floor(Math.random() * 100);
            }
            
            if (txCount > 0) {
                histogram.push([feeRate, txCount]);
            }
        }
        
        return histogram;
    }
    
    generateBlockHash() {
        // Generate a realistic-looking block hash
        const chars = '0123456789abcdef';
        let hash = '00000'; // Bitcoin blocks start with zeros
        
        for (let i = 5; i < 64; i++) {
            hash += chars[Math.floor(Math.random() * chars.length)];
        }
        
        return hash;
    }
    
    // Simulate different network conditions
    setNetworkCondition(condition) {
        switch (condition) {
            case 'fast':
                this.baseLatency = 50;
                this.failureRate = 0.01;
                break;
            case 'slow':
                this.baseLatency = 1000;
                this.failureRate = 0.1;
                break;
            case 'unreliable':
                this.baseLatency = 500;
                this.failureRate = 0.2;
                break;
            default:
                this.baseLatency = 200;
                this.failureRate = 0.05;
        }
    }
}

// Usage in application
const apiSimulator = new BitcoinAPISimulator();

async function fetchRealTimeBitcoinData() {
    try {
        // Fetch multiple endpoints concurrently
        const [mempoolStats, feeRecommendations, blockHeight] = await Promise.all([
            apiSimulator.getMempoolStats(),
            apiSimulator.getFeeRecommendations(),
            apiSimulator.getBlockHeight()
        ]);
        
        return {
            mempool: mempoolStats,
            fees: feeRecommendations,
            blocks: blockHeight,
            fetchedAt: new Date().toISOString()
        };
    } catch (error) {
        console.error('Failed to fetch Bitcoin data:', error.message);
        throw error;
    }
}
```

### Loading States: Professional User Experience
Provide clear feedback during asynchronous operations:

```javascript
// Comprehensive loading state management
class LoadingStateManager {
    constructor() {
        this.loadingStates = new Map();
        this.loadingElements = new Map();
    }
    
    // Register loading elements
    registerElement(key, elementId, options = {}) {
        const element = document.getElementById(elementId);
        if (!element) {
            console.warn(`Element with id '${elementId}' not found`);
            return;
        }
        
        this.loadingElements.set(key, {
            element,
            originalContent: element.innerHTML,
            options: {
                showSpinner: options.showSpinner !== false,
                showProgress: options.showProgress || false,
                loadingText: options.loadingText || 'Loading...',
                errorText: options.errorText || 'Error occurred',
                ...options
            }
        });
    }
    
    // Set loading state
    setLoading(key, isLoading = true, progress = null) {
        this.loadingStates.set(key, { isLoading, progress });
        this.updateElement(key);
    }
    
    // Set error state
    setError(key, error) {
        this.loadingStates.set(key, { isLoading: false, error });
        this.updateElement(key);
    }
    
    // Set success state
    setSuccess(key, data = null) {
        this.loadingStates.set(key, { isLoading: false, success: true, data });
        this.updateElement(key);
    }
    
    // Update DOM element based on state
    updateElement(key) {
        const elementData = this.loadingElements.get(key);
        const state = this.loadingStates.get(key);
        
        if (!elementData || !state) return;
        
        const { element, originalContent, options } = elementData;
        
        if (state.isLoading) {
            this.showLoadingState(element, options, state.progress);
        } else if (state.error) {
            this.showErrorState(element, options, state.error);
        } else if (state.success) {
            this.showSuccessState(element, originalContent);
        }
    }
    
    showLoadingState(element, options, progress) {
        let content = '';
        
        if (options.showSpinner) {
            content += '<div class="loading-spinner">⏳</div>';
        }
        
        content += `<div class="loading-text">${options.loadingText}</div>`;
        
        if (options.showProgress && progress !== null) {
            content += `
                <div class="loading-progress">
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: ${progress}%"></div>
                    </div>
                    <div class="progress-text">${Math.round(progress)}%</div>
                </div>
            `;
        }
        
        element.innerHTML = content;
        element.className = 'loading-state';
    }
    
    showErrorState(element, options, error) {
        const errorMessage = error.message || error.toString();
        
        element.innerHTML = `
            <div class="error-state">
                <div class="error-icon">❌</div>
                <div class="error-text">${options.errorText}</div>
                <div class="error-details">${errorMessage}</div>
                <button class="retry-button" onclick="retryOperation('${element.id}')">
                    Retry
                </button>
            </div>
        `;
        element.className = 'error-state';
    }
    
    showSuccessState(element, originalContent) {
        element.innerHTML = originalContent;
        element.className = 'success-state';
    }
    
    // Clear all states
    clearAll() {
        this.loadingStates.clear();
        
        for (const [key, elementData] of this.loadingElements) {
            elementData.element.innerHTML = elementData.originalContent;
            elementData.element.className = '';
        }
    }
}

// Advanced loading patterns for Bitcoin applications
class BitcoinDataLoader {
    constructor() {
        this.loadingManager = new LoadingStateManager();
        this.cache = new Map();
        this.cacheTimeout = 30000; // 30 seconds
    }
    
    async loadMempoolData(forceRefresh = false) {
        const cacheKey = 'mempool-data';
        
        // Check cache first
        if (!forceRefresh && this.isCacheValid(cacheKey)) {
            return this.cache.get(cacheKey).data;
        }
        
        try {
            this.loadingManager.setLoading('mempool', true);
            
            // Simulate progressive loading
            this.loadingManager.setLoading('mempool', true, 25);
            await this.delay(200);
            
            this.loadingManager.setLoading('mempool', true, 50);
            const data = await apiSimulator.getMempoolStats();
            
            this.loadingManager.setLoading('mempool', true, 75);
            await this.delay(100);
            
            // Cache the result
            this.cache.set(cacheKey, {
                data,
                timestamp: Date.now()
            });
            
            this.loadingManager.setLoading('mempool', true, 100);
            await this.delay(100);
            
            this.loadingManager.setSuccess('mempool', data);
            return data;
            
        } catch (error) {
            this.loadingManager.setError('mempool', error);
            throw error;
        }
    }
    
    async loadAllBitcoinData() {
        const operations = [
            { key: 'mempool', loader: () => this.loadMempoolData() },
            { key: 'fees', loader: () => apiSimulator.getFeeRecommendations() },
            { key: 'blocks', loader: () => apiSimulator.getBlockHeight() }
        ];
        
        // Start all loading states
        operations.forEach(op => {
            this.loadingManager.setLoading(op.key, true);
        });
        
        const results = {};
        const errors = {};
        
        // Load data concurrently but handle each result individually
        await Promise.allSettled(
            operations.map(async (op) => {
                try {
                    const data = await op.loader();
                    results[op.key] = data;
                    this.loadingManager.setSuccess(op.key, data);
                } catch (error) {
                    errors[op.key] = error;
                    this.loadingManager.setError(op.key, error);
                }
            })
        );
        
        return { results, errors };
    }
    
    isCacheValid(key) {
        const cached = this.cache.get(key);
        if (!cached) return false;
        
        return (Date.now() - cached.timestamp) < this.cacheTimeout;
    }
    
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}

// Usage example
const dataLoader = new BitcoinDataLoader();

// Register loading elements
dataLoader.loadingManager.registerElement('mempool', 'mempool-display', {
    loadingText: 'Fetching mempool data...',
    showProgress: true,
    errorText: 'Failed to load mempool data'
});

dataLoader.loadingManager.registerElement('fees', 'fee-display', {
    loadingText: 'Calculating fee recommendations...',
    showSpinner: true
});

// Load data with proper loading states
async function updateBitcoinDashboard() {
    try {
        const { results, errors } = await dataLoader.loadAllBitcoinData();
        
        console.log('Loaded data:', results);
        if (Object.keys(errors).length > 0) {
            console.warn('Some data failed to load:', errors);
        }
    } catch (error) {
        console.error('Dashboard update failed:', error);
    }
}
```

**Why asynchronous programming is essential for Bitcoin applications:**
- **Network Operations**: All Bitcoin APIs require network requests
- **Real-time Updates**: Bitcoin data changes constantly and needs live updates
- **User Experience**: Non-blocking operations keep interfaces responsive
- **Error Resilience**: Proper async handling manages network failures gracefully
- **Performance**: Concurrent requests load data faster than sequential calls

**Common Bitcoin async patterns:**
```javascript
// Fetch current Bitcoin price
const price = await fetch('https://api.coindesk.com/v1/bpi/currentprice.json');

// Monitor mempool in real-time
setInterval(async () => {
    const mempool = await fetchMempoolData();
    updateUI(mempool);
}, 30000);

// Handle multiple API failures gracefully
const data = await Promise.allSettled([
    fetchPrice(),
    fetchMempool(),
    fetchBlocks()
]);
```

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