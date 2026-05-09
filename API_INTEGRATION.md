# API Integration Guide

## Overview

This guide explains how to integrate real-world APIs with the Fear & Greed Index application.

## Available Data Sources

### 1. NSE (National Stock Exchange) API

**Official NSE Website**: https://www.nseindia.com/

**Available Endpoints**:
```
GET /api/equity-stockIndices
GET /api/equity-quote/{symbol}
GET /api/corporate-actions
GET /api/market-data
```

**Data Available**:
- Stock prices and quotes
- Market indices (SENSEX, NIFTY, NIFTY50)
- Volume and open interest
- Company fundamentals
- Corporate actions

**Integration Example**:
```javascript
// src/utils/dataService.js
async fetchNSEData(symbol) {
  try {
    const response = await axios.get(
      `https://www.nseindia.com/api/equity-quote/${symbol}`,
      {
        headers: {
          'User-Agent': 'Mozilla/5.0...',
          'Accept': 'application/json'
        }
      }
    )
    return response.data
  } catch (error) {
    console.error('NSE API Error:', error)
  }
}
```

**Rate Limiting**: Reasonable limits for educational use
**Cost**: Free

### 2. BSE (Bombay Stock Exchange) API

**Official BSE Website**: https://www.bseindia.com/

**Available Endpoints**:
```
GET /api/quote/{symbol}
GET /api/indices
GET /api/company-info
```

**Data Available**:
- Stock quotes and historical data
- BSE Indices (SENSEX, MIDCAP)
- Company information
- Market breadth data

**Integration Example**:
```javascript
async fetchBSEData(symbol) {
  const response = await axios.get(
    `https://www.bseindia.com/api/quote/${symbol}`,
    { timeout: 10000 }
  )
  return response.data
}
```

**Rate Limiting**: Standard API limits
**Cost**: Free

### 3. Yahoo Finance API

**Website**: https://finance.yahoo.com/

**Available Data**:
- Stock price and historical data
- Technical indicators
- Financial statements
- Earnings data
- Dividend information

**JavaScript Library**:
```bash
npm install yahoo-finance2
```

**Integration Example**:
```javascript
import yahooFinance from 'yahoo-finance2'

async fetchYahooData(symbol) {
  try {
    const quote = await yahooFinance.quote(`${symbol}.NS`)
    return {
      price: quote.regularMarketPrice,
      high52Week: quote.fiftyTwoWeekHigh,
      low52Week: quote.fiftyTwoWeekLow,
      pe: quote.trailingPE,
      marketCap: quote.marketCap,
      volume: quote.volume
    }
  } catch (error) {
    console.error('Yahoo Finance Error:', error)
  }
}
```

**Rate Limiting**: No official API, use unofficial `yahoo-finance2`
**Cost**: Free (unofficial)

### 4. Alpha Vantage

**Website**: https://www.alphavantage.co/

**Data Available**:
- Stock price data
- Technical indicators (SMA, RSI, MACD, etc.)
- Historical data

**Get API Key**: https://www.alphavantage.co/api/

**Integration Example**:
```javascript
async fetchTechnicalIndicators(symbol) {
  const apiKey = process.env.VITE_ALPHA_VANTAGE_KEY
  
  const response = await axios.get(
    `https://www.alphavantage.co/query`,
    {
      params: {
        function: 'RSI',
        symbol: `${symbol}.NS`,
        interval: 'daily',
        apikey: apiKey
      }
    }
  )
  return response.data
}
```

**Rate Limiting**: 5 requests/min (free tier), 500 requests/min (paid)
**Cost**: Free tier available

### 5. News APIs

#### NewsAPI
**Website**: https://newsapi.org/

**Integration Example**:
```javascript
async fetchNewsForCompany(company) {
  const apiKey = process.env.VITE_NEWS_API_KEY
  
  const response = await axios.get(
    `https://newsapi.org/v2/everything`,
    {
      params: {
        q: `${company} stock market OR India`,
        sortBy: 'publishedAt',
        language: 'en',
        apiKey: apiKey,
        pageSize: 5
      }
    }
  )
  
  return response.data.articles
}
```

#### CNBC India (Web Scraping)
```javascript
// Note: Check terms of service before scraping
async fetchCNBCNews() {
  const response = await axios.get('https://www.cnbctv18.com/market/')
  // Parse HTML and extract news
}
```

#### Economic Times (RSS Feed)
```javascript
async fetchEconomicTimesNews() {
  const response = await axios.get(
    'https://economictimes.indiatimes.com/feed'
  )
  // Parse RSS and extract relevant news
}
```

**Rate Limiting**: Varies by service
**Cost**: Free tier available

## Implementation Steps

### Step 1: Add Environment Variables
Create `.env` file:
```
VITE_NSE_API_BASE=https://www.nseindia.com/api
VITE_BSE_API_BASE=https://www.bseindia.com/api
VITE_YAHOO_FINANCE_KEY=your_key
VITE_ALPHA_VANTAGE_KEY=your_key
VITE_NEWS_API_KEY=your_key
```

### Step 2: Update dataService.js

```javascript
// src/utils/dataService.js
import axios from 'axios'

class DataService {
  constructor() {
    this.nseAPI = axios.create({
      baseURL: import.meta.env.VITE_NSE_API_BASE,
      timeout: 10000
    })
    
    this.newsAPI = axios.create({
      baseURL: 'https://newsapi.org/v2',
      timeout: 10000
    })
  }

  async fetchCompanyData(symbol) {
    try {
      const nseData = await this.nseAPI.get(`/equity-quote/${symbol}`)
      return nseData.data
    } catch (error) {
      console.error(`Error fetching ${symbol}:`, error)
      return null
    }
  }

  async fetchNewsForCompany(company) {
    try {
      const response = await this.newsAPI.get('/everything', {
        params: {
          q: company,
          sortBy: 'publishedAt',
          language: 'en',
          pageSize: 5,
          apiKey: import.meta.env.VITE_NEWS_API_KEY
        }
      })
      return response.data.articles || []
    } catch (error) {
      console.error(`Error fetching news for ${company}:`, error)
      return []
    }
  }

  // Implement other methods...
}

export default new DataService()
```

### Step 3: Error Handling & Fallbacks

```javascript
async fetchWithFallback(primaryAPI, fallbackAPI, symbol) {
  try {
    return await primaryAPI(symbol)
  } catch (error) {
    console.warn(`Primary API failed, trying fallback for ${symbol}`)
    try {
      return await fallbackAPI(symbol)
    } catch (fallbackError) {
      console.error(`Both APIs failed for ${symbol}:`, fallbackError)
      return null
    }
  }
}
```

### Step 4: Caching Strategy

```javascript
class CachedDataService extends DataService {
  constructor() {
    super()
    this.cache = new Map()
    this.cacheExpiry = 5 * 60 * 1000 // 5 minutes
  }

  getCachedData(key) {
    const cached = this.cache.get(key)
    if (cached && Date.now() - cached.timestamp < this.cacheExpiry) {
      return cached.data
    }
    return null
  }

  setCachedData(key, data) {
    this.cache.set(key, {
      data,
      timestamp: Date.now()
    })
  }

  async fetchCompanyData(symbol) {
    const cached = this.getCachedData(`company_${symbol}`)
    if (cached) return cached

    const data = await super.fetchCompanyData(symbol)
    if (data) {
      this.setCachedData(`company_${symbol}`, data)
    }
    return data
  }
}
```

## Component Calculation Examples

### Market Momentum
```javascript
calculateMarketMomentum(currentPrice, ma50, ma200) {
  const momentum = ((currentPrice - ma50) / ma50) * 100
  const trend = currentPrice > ma200 ? 1 : -1
  return Math.min(100, Math.max(0, 50 + momentum * trend))
}
```

### P/E Ratio Score
```javascript
calculatePERatioScore(currentPE, averagePE, minPE, maxPE) {
  const normalized = (currentPE - minPE) / (maxPE - minPE)
  return Math.min(100, Math.max(0, 50 + (0.5 - normalized) * 100))
}
```

### Volatility Score
```javascript
calculateVolatilityScore(currentVIX, maxVIX) {
  return Math.max(0, 100 - (currentVIX / maxVIX) * 100)
}
```

### Earnings Quality
```javascript
calculateEarningsQuality(eps, revenue, cashFlow) {
  const earningsGrowth = (eps / previousEPS - 1) * 100
  const cashConversion = (cashFlow / revenue) * 100
  return (earningsGrowth * 0.6 + cashConversion * 0.4)
}
```

## Rate Limiting & Best Practices

### Implement Request Queuing
```javascript
class APIQueue {
  constructor(maxRequests = 5, timeWindow = 60000) {
    this.queue = []
    this.maxRequests = maxRequests
    this.timeWindow = timeWindow
    this.requests = []
  }

  async add(fn) {
    return new Promise((resolve, reject) => {
      this.queue.push({ fn, resolve, reject })
      this.process()
    })
  }

  process() {
    if (this.queue.length === 0) return

    const now = Date.now()
    this.requests = this.requests.filter(t => now - t < this.timeWindow)

    if (this.requests.length < this.maxRequests) {
      const { fn, resolve, reject } = this.queue.shift()
      this.requests.push(now)
      
      fn()
        .then(resolve)
        .catch(reject)
        .finally(() => this.process())
    }
  }
}
```

### Implement Retry Logic
```javascript
async function retryAPI(fn, maxRetries = 3, delay = 1000) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn()
    } catch (error) {
      if (i === maxRetries - 1) throw error
      await new Promise(resolve => setTimeout(resolve, delay * Math.pow(2, i)))
    }
  }
}
```

## Testing APIs Locally

### Using Postman
1. Download Postman: https://www.postman.com/downloads/
2. Create new collection for Fear & Greed Index
3. Add requests for each API endpoint
4. Save responses as JSON for mock data

### Using cURL
```bash
# NSE API
curl -X GET https://www.nseindia.com/api/equity-quote/RIL

# News API
curl -X GET "https://newsapi.org/v2/everything?q=Reliance&apiKey=YOUR_KEY"
```

## Monitoring & Debugging

### Add Logging
```javascript
const logger = {
  log: (message, data) => {
    console.log(`[${new Date().toISOString()}] ${message}`, data)
  },
  error: (message, error) => {
    console.error(`[${new Date().toISOString()}] ERROR: ${message}`, error)
  }
}
```

### Monitor API Response Times
```javascript
async function measureAPITime(fn, name) {
  const start = performance.now()
  const result = await fn()
  const duration = performance.now() - start
  logger.log(`${name} took ${duration.toFixed(2)}ms`)
  return result
}
```

## Security Considerations

1. **Never commit API keys** - Use `.env` files
2. **Use HTTPS** - All API calls should be encrypted
3. **Validate data** - Sanitize all API responses
4. **CORS handling** - Use backend proxy if needed
5. **Rate limiting** - Implement on frontend and backend

## Production Checklist

- [ ] API keys in environment variables
- [ ] Error handling for all endpoints
- [ ] Retry logic for failed requests
- [ ] Response caching implemented
- [ ] Request queuing for rate limits
- [ ] Logging and monitoring setup
- [ ] Tests for API integration
- [ ] Performance metrics tracked
- [ ] CORS properly configured
- [ ] Security headers added

---

**For questions or issues, refer to TECHNICAL_DOCS.md**
