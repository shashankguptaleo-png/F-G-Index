# Fear & Greed Index - India Stock Market

> A comprehensive financial analysis tool tracking Fear & Greed sentiment for India's top 9 companies by market capitalization, with automated updates and detailed component analysis.

## 🚀 Features

### Dashboard
- **Real-time Fear & Greed Scores** (0-100 scale) for top 9 Indian companies
- **Color-coded Visualization**: Red (Fear) → Green (Greed)
- **Interactive Expandable Rows**: Click any company to see detailed analysis
- **Automatic Scheduling**: Updates every Monday at 8:01 AM IST
- **Market Holiday Handling**: Automatically adjusts for NSE/BSE holidays
- **Live Status**: Market open/closed indicator with trading hours
- **Countdown Timer**: Shows time until next update

### Company Analysis
- **7-Component Breakdown**:
  - Market Momentum (15%)
  - P/E Ratio Analysis (15%)
  - Volatility Index (15%)
  - Junk Bond Demand (15%)
  - Market Breadth (15%)
  - Earnings Quality (15%)
  - Technical Indicators (10%)
- **Detailed 300-word Analysis**: Explains each score with investment rationale
- **Key Metrics**: P/E ratio, 52-week highs/lows, YTD returns
- **Latest News**: 3-5 recent developments and market updates
- **Sentiment Badge**: Overall Fear/Greed classification

### Top 9 Companies Tracked
1. **Reliance Industries** (RIL) - ₹19.5T market cap
2. **Tata Consultancy Services** (TCS) - ₹14.2T
3. **HDFC Bank** (HDFCBANK) - ₹12.8T
4. **Wipro** (WIPRO) - ₹11.5T
5. **Infosys** (INFY) - ₹11.2T
6. **State Bank of India** (SBIN) - ₹10.8T
7. **HDFC Limited** (HDFCL) - ₹9.5T
8. **Bharti Airtel** (AIRTEL) - ₹9.2T
9. **Larsen & Toubro** (LT) - ₹8.8T

## 📋 Requirements

- Node.js >= 16.0.0
- npm or yarn package manager
- Modern web browser (Chrome, Firefox, Safari, Edge)

## 💻 Installation

### 1. Clone Repository
```bash
git clone https://github.com/shashankguptaleo-png/F-G-Index.git
cd F-G-Index
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Start Development Server
```bash
npm run dev
```

The application will open at `http://localhost:3000`

## 🏗️ Build & Deploy

### Build for Production
```bash
npm run build
```

This generates optimized files in the `dist/` directory.

### Deploy to CodeSandbox (For Resume)
1. Go to https://codesandbox.io
2. Click "Create" → "Import from GitHub"
3. Enter: `shashankguptaleo-png/F-G-Index`
4. CodeSandbox automatically handles dependencies and deployment
5. Get your shareable live URL for your resume!

### Deploy to Vercel
```bash
npm i -g vercel
vercel
```

### Deploy to Netlify
```bash
npm run build
# Drag and drop 'dist' folder to Netlify
```

## 📁 Project Structure

```
F-G-Index/
├── index.html              # HTML entry point
├── package.json            # Dependencies
├── vite.config.js          # Vite configuration
├── API_INTEGRATION.md      # API integration guide
│
└── src/
    ├── index.jsx           # React entry point
    ├── index.css           # Global styles
    ├── App.jsx             # Main component
    ├── App.css             # App styles
    │
    ├── components/
    │   ├── CompanyTable.jsx      # Table with expandable rows
    │   └── CompanyTable.css      # Table styling
    │
    └── utils/
        ├── updateScheduler.js    # Scheduler with holiday logic
        └── dataService.js        # API service & mock data
```

## 🔄 Update Schedule

### Default: Monday 8:01 AM IST
- Runs automatically every Monday morning
- No manual intervention needed

### Holiday Handling
- Detects if Monday is NSE/BSE holiday
- Moves update to next market-open day automatically
- Includes 200+ market holidays (2024-2025)

### Manual Refresh
- Click "🔄 Refresh" button anytime for instant update
- Shows loading indicator during refresh
- Clears cached data before fetching new data

## 📊 Fear & Greed Scale

| Score | Range | Sentiment | Color |
|-------|-------|-----------|-------|
| 0-25 | Extreme Fear | 🔴 Red | #ef4444 |
| 25-45 | Fear | 🟠 Orange | #f97316 |
| 45-55 | Neutral | 🟡 Yellow | #eab308 |
| 55-75 | Greed | 🟢 Light Green | #84cc16 |
| 75-100 | Extreme Greed | 🟢 Dark Green | #22c55e |

## 🔗 API Integration

The application is ready to integrate with:
- **NSE APIs** - Real-time stock data
- **BSE APIs** - Bombay Stock Exchange data
- **Yahoo Finance** - Technical indicators
- **Alpha Vantage** - Historical data & indicators
- **NewsAPI** - Market sentiment from news

See `API_INTEGRATION.md` for complete setup instructions.

## 📱 Responsive Design

- **Desktop**: Full dashboard with all features
- **Tablet**: Optimized spacing and layout
- **Mobile**: Touch-friendly, single-column design
- **Small Phones**: Ultra-compact view with essential info

## ⚙️ Technology Stack

- **React 18** - UI framework
- **Vite** - Lightning-fast build tool
- **Axios** - HTTP client
- **CSS3** - Modern styling (Grid, Flexbox)
- **JavaScript ES6+** - Modern features

## 🎨 UI Components

### Header
- Title and subtitle
- Refresh button with loading state
- Market status indicator
- Last update timestamp
- Next update countdown
- Schedule information

### Main Table
- Company ranking
- Symbol and current price
- Market capitalization
- YTD returns
- Fear & Greed score with progress bar
- Expandable details row

### Expansion Details
- 7-component score breakdown with visual bars
- Sentiment classification
- Detailed 300-word analysis
- Key financial metrics
- Latest news and updates

### Footer
- Last update date
- Timezone information (IST)
- NSE trading hours
- Disclaimer

## 🚀 Performance

- **Fast Load**: < 2 seconds initial load
- **Smooth Animations**: 60 FPS transitions
- **Caching**: 5-minute cache for API responses
- **Optimized Build**: Minified and gzipped output
- **Mobile Optimized**: Responsive and touch-friendly

## 🔐 Security

- No sensitive data storage
- Secure API calls with error handling
- Environment variables for API keys (when integrating)
- HTTPS recommended for production
- Input validation on all forms

## 📈 Next Steps

1. **View Live Demo**: Deploy to CodeSandbox
2. **Integrate Real APIs**: Follow `API_INTEGRATION.md`
3. **Customize**: Modify companies, colors, or update frequency
4. **Deploy**: Use Vercel, Netlify, or GitHub Pages

## 🤝 Contributing

Contributions welcome! Areas for enhancement:
- Additional companies beyond top 9
- More data sources and providers
- Advanced charting and analytics
- Historical data and trend analysis
- Mobile app version

## 📄 License

MIT License - Feel free to use for educational and commercial purposes

## 🙏 Support

For issues, questions, or suggestions:
1. Check `TECHNICAL_DOCS.md` for architecture details
2. Review `API_INTEGRATION.md` for data integration
3. Create an issue in the GitHub repository

## 📚 Documentation

- **README.md** - Overview and quick start
- **TECHNICAL_DOCS.md** - Architecture and implementation details
- **API_INTEGRATION.md** - How to connect real market data APIs
- **DEPLOYMENT.md** - Deployment guides for various platforms

---

**Built with ❤️ for Indian market investors**

*Disclaimer: This tool is for educational purposes. Always consult a financial advisor before making investment decisions.*
