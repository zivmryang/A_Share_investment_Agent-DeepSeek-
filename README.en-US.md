

# AI Investment System

This is a proof-of-concept project for an AI-based investment system. The project's goal is to explore how AI can be used to assist in investment decision-making. This project is for **educational purposes only** and is not intended for actual trading or investment.

## System Composition

The system consists of the following collaborating agents:

- **Market Data Analyst**: Responsible for collecting and preprocessing market data
- **Valuation Agent**: Calculates intrinsic stock value and generates trading signals  
- **Sentiment Agent**: Analyzes market sentiment and generates trading signals
- **Fundamentals Agent**: Analyzes fundamental data and generates trading signals
- **Technical Analyst**: Analyzes technical indicators and generates trading signals
- **Risk Manager**: Calculates risk metrics and sets position limits
- **Portfolio Manager**: Makes final trading decisions and generates orders

![Screenshot 2024-12-27 at 5 49 56 PM](https://github.com/user-attachments/assets/c281b8c3-d8e6-431e-a05e-d309d306e967)

Note: The system only simulates trading decisions and does not execute actual trades.

## Disclaimer

This project is for **educational and research purposes only**.

- Not intended for actual trading or investment
- Provided without any warranties
- Past performance does not guarantee future results
- The creators assume no liability for any financial losses
- Consult a professional financial advisor for investment decisions

By using this software, you agree to use it solely for learning purposes.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
  - [Running the Hedge Fund](#running-the-hedge-fund)
  - [Running Backtests](#running-backtests)
- [Project Structure](#project-structure)
- [Contribution Guidelines](#contribution-guidelines)
- [License](#license)

## Installation

1. Clone the repository:

```bash
git clone https://github.com/zivmryang/A_Share_investment_Agent-DeepSeek-.git
cd A_Share_investent_Agent
```

2. Install Poetry (if not already installed):

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

3. Install dependencies:

```bash
poetry install
```

4. Set up environment variables:

```bash
# Create .env file to store API keys
cp .env.example .env

# Get DeepSeek API key from https://platform.deepseek.com/
export DEEP_SEEK_API_KEY='your-deepseek-api-key-here'
export DEEP_SEEK_MODEL='deepseek-chat'
```

## DeepSeek API Key Request Guide

1. Visit the [DeepSeek Platform](https://platform.deepseek.com/)
2. Register or log in to your account
3. Go to the API Keys page
4. Click the "Create new API key" button
5. Copy the generated API key
6. Fill the API key into the DEEP_SEEK_API_KEY variable in the .env file

## Usage

### Running the Hedge Fund

The system supports multiple run modes. You can combine parameters as needed:

1. **Basic Run**

```bash
poetry run python src/main.py --ticker 301155
```

This will run the system with default parameters, including:

- Default analysis of 5 news articles (`num_of_news=5`)
- Does not show the detailed analysis process (`show_reasoning=False`)
- Uses default initial capital (`initial_capital=100,000`)

2. **Show Analysis Reasoning Process**

```bash
poetry run python src/main.py --ticker 301155 --show-reasoning
```

This will display the analysis process and reasoning results for each agent (Market Data Agent, Technical Analyst, Fundamentals Agent, Sentiment Agent, Risk Manager, Portfolio Manager).

This allows you to set:

- initial_capital: Initial cash amount (optional, defaults to 100,000)

3. **Customize News Analysis Count and Specific Date Investment Advice**

```bash
poetry run python src/main.py --ticker 301157 --show-reasoning --end-date 2024-12-11 --num-of-news 20
```

This will:

- Analyze the 20 most recent news articles within the specified date range for sentiment analysis
- start-date and end-date format is YYYY-MM-DD

4. **Backtesting Feature**

```bash
poetry run python src/backtester.py --ticker 301157 --start-date 2024-12-11 --end-date 2025-01-07 --num-of-news 20
```

The backtesting feature supports the following parameters:

- ticker: Stock ticker symbol
- start-date: Backtest start date (YYYY-MM-DD)
- end-date: Backtest end date (YYYY-MM-DD)
- initial-capital: Initial capital (optional, defaults to 100,000)
- num-of-news: Number of news articles for sentiment analysis (optional, defaults to 5, max 100)

### Parameter Description

- `--ticker`: Stock ticker symbol (required)
- `--show-reasoning`: Show analysis reasoning process (optional, defaults to false)
- `--initial-capital`: Initial cash amount (optional, defaults to 100,000)
- `--num-of-news`: Number of news articles for sentiment analysis (optional, defaults to 5, max 100)
- `--start-date`: Start date, format YYYY-MM-DD (optional)
- `--end-date`: End date, format YYYY-MM-DD (optional)

### Output Description

The system will output the following information:

1. Fundamental analysis results
2. Valuation analysis results
3. Technical analysis results
4. Sentiment analysis results
5. Risk management evaluation
6. Final trading decision

If the `--show-reasoning` parameter is used, the detailed analysis process for each agent will also be displayed.

**Example Output:**

```
正在获取 301157 的历史行情数据...
开始日期：2024-12-11
结束日期：2024-12-11
成功获取历史行情数据，共 242 条记录

警告：以下指标存在NaN值：
- momentum_1m: 20条
- momentum_3m: 60条
- momentum_6m: 120条
...（这些警告是正常的，是由于某些技术指标需要更长的历史数据才能计算）

正在获取 301157 的财务指标数据...
获取实时行情...
成功获取实时行情数据

获取新浪财务指标...
成功获取新浪财务指标数据，共 3 条记录
最新数据日期：2024-09-30 00:00:00

获取利润表数据...
成功获取利润表数据

构建指标数据...
成功构建指标数据

Final Result:
{
  "action": "buy",
  "quantity": 12500,
  "confidence": 0.42,
  "agent_signals": [
    {
      "agent": "Technical Analysis",
      "signal": "bullish",
      "confidence": 0.6
    },
    {
      "agent": "Fundamental Analysis",
      "signal": "neutral",
      "confidence": 0.5
    },
    {
      "agent": "Sentiment Analysis",
      "signal": "neutral",
      "confidence": 0.8
    },
    {
      "agent": "Valuation Analysis",
      "signal": "bearish",
      "confidence": 0.99
    },
    {
      "agent": "Risk Management",
      "signal": "buy",
      "confidence": 1.0
    }
  ],
  "reasoning": "Risk Management allows a buy action with a maximum quantity of 12500..."
}
```

### Log File Description

The system generates the following types of log files in the `logs/` directory:

1. **Backtest Logs**
   - Filename format: `backtest_{ticker}_{current_date}_{backtest_start_date}_{backtest_end_date}.log`
   - Example: `backtest_301157_20250107_20241201_20241230.log`
   - Contains: Analysis results, trading decisions, and portfolio status for each trading day

2. **API Call Logs**
   - Filename format: `api_calls_{current_date}.log`
   - Example: `api_calls_20250107.log`
   - Contains: Detailed information and responses of all API calls

All dates are in YYYY-MM-DD format. If the `--show-reasoning` parameter is used, detailed analysis processes will also be recorded in the log files.

## Project Structure

```
ai-hedge-fund/
├── src/                         # Source code directory
│   ├── agents/                  # Agent definitions and workflows
│   │   ├── fundamentals.py      # Fundamentals Analysis Agent
│   │   ├── market_data.py       # Market Data Analysis Agent
│   │   ├── portfolio_manager.py # Portfolio Management Agent
│   │   ├── risk_manager.py      # Risk Management Agent
│   │   ├── sentiment.py         # Sentiment Analysis Agent
│   │   ├── state.py            # Agent State Management
│   │   ├── technicals.py       # Technical Analysis Agent
│   │   └── valuation.py        # Valuation Analysis Agent
│   ├── data/                   # Data storage directory
│   │   ├── sentiment_cache.json # Sentiment analysis cache
│   │   └── stock_news/         # Stock news data
│   ├── tools/                  # Tools and utility modules
│   │   ├── api.py              # API interface and data fetching
│   │   ├── data_analyzer.py    # Data analysis utilities
│   │   ├── news_crawler.py     # News crawler utility
│   │   ├── openrouter_config.py # OpenRouter configuration
│   │   └── test_*.py           # Test files
│   ├── utils/                  # General utility functions
│   ├── backtester.py          # Backtesting system
│   └── main.py                # Main program entry point
├── logs/                      # Log files directory
│   ├── api_calls_*.log        # API call logs
│   └── backtest_*.log         # Backtest result logs
├── .env                       # Environment variable configuration
├── .env.example              # Environment variable example
├── poetry.lock               # Poetry dependency lock file
├── pyproject.toml            # Poetry project configuration
└── README.md                 # Project documentation
```

## Contribution Guidelines

Contributions are welcome! Please follow these steps:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Create a Pull Request

## License Information

This project is licensed under the MIT License. For specific terms, please refer to the LICENSE file in the project root.

Key terms include:
- Allows free use, copying, modification, merger, publication, distribution, sublicensing, and sale of the software and its copies
- Allows modification of the source code, but the copyright and license notices must be included in all copies
- The software is provided "as is", without any form of warranty

## Acknowledgements and Reference Projects

This project is modified and extended based on the following open-source projects. Special thanks to the authors:

1. [ai-hedge-fund](https://github.com/virattt/ai-hedge-fund.git)
   - Provides the foundational AI investment system framework
   - Implements the core agent collaboration mechanism
   - Includes a complete backtesting system implementation

2. [24mlight/A_Share_investment_Agent](https://github.com/24mlight/A_Share_investment_Agent.git)
   - Provides A-share market adaptation solutions
   - Implements akshare data API integration
   - Contains Chinese documentation and localization support

We sincerely thank the original authors for their excellent work and inspiration. These projects laid a solid foundation for our adaptation and improvements for the A-share market.

## Detailed Project Description

### System Architecture Design

This project adopts a modular multi-agent architecture, with each agent having a clear division of responsibilities. The system architecture is as follows:

```
Market Data Analysis → [Technical / Fundamental / Sentiment / Valuation Analysis] → Risk Management → Portfolio Management → Trading Decisions
```

#### Agent Function Descriptions

1. **Market Data Analysis Agent**
   - System entry point, responsible for data collection and preprocessing
   - Obtains A-share market data via the akshare API
   - Data sources include:
     - East Money
     - Sina Finance
     - Tonghuashun (10jqka)

2. **Technical Analysis Agent**
   - Analyzes the following technical indicators:
     - Price trends
     - Trading volume
     - Momentum indicators
   - Generates technical analysis trading signals
   - Focuses on short-term market trends

3. **Fundamental Analysis Agent**
   - Analyzes company financial metrics:
     - Profitability
     - Growth
     - Financial health
   - Evaluates long-term corporate development potential
   - Generates fundamental trading signals

4. **Sentiment Analysis Agent**
   - Analyzes market news and public opinion
   - Assesses market sentiment
   - Generates sentiment trading signals
   - Supports multiple data sources:
     - Sina Finance
     - East Money
     - Xueqiu

5. **Valuation Analysis Agent**
   - Performs corporate valuation analysis
   - Evaluates intrinsic stock value
   - Main valuation methods:
     - DCF (Discounted Cash Flow) model
     - Relative valuation methods
     - Market comparison approach

6. **Risk Management Agent**
   - Integrates signals from various agents
   - Assesses potential risks
   - Sets risk control parameters:
     - Maximum position limits
     - Stop-loss and take-profit levels
     - Trading size limits

7. **Portfolio Management Agent**
   - Final decision-maker
   - Considers comprehensively:
     - Signals from each agent
     - Risk factors
     - Portfolio status
   - Generates trading decisions:
     - Buy
     - Sell
     - Hold

### Data Processing Workflow

#### Data Type Descriptions

1. **Market Data**

```python
{
    "market_cap": float,        # Total Market Cap
    "volume": float,            # Trading Volume
    "average_volume": float,    # Average Volume
    "fifty_two_week_high": float,  # 52-Week High
    "fifty_two_week_low": float    # 52-Week Low
}
```

2. **Financial Metrics Data**

```python
{
    # Market Data
    "market_cap": float,          # Total Market Cap
    "float_market_cap": float,    # Circulating Market Cap

    # Profitability Data
    "revenue": float,             # Total Operating Revenue
    "net_income": float,          # Net Income
    "return_on_equity": float,    # Return on Equity (ROE)
    "net_margin": float,          # Net Profit Margin
    "operating_margin": float,    # Operating Margin

    # Growth Metrics
    "revenue_growth": float,      # Revenue Growth Rate
    "earnings_growth": float,     # Earnings Growth Rate
    "book_value_growth": float,   # Book Value Growth Rate

    # Financial Health Metrics
    "current_ratio": float,       # Current Ratio
    "debt_to_equity": float,      # Debt-to-Equity Ratio
    "free_cash_flow_per_share": float,  # Operating Cash Flow per Share
    "earnings_per_share": float,  # Earnings Per Share (EPS)

    # Valuation Ratios
    "pe_ratio": float,           # Price-to-Earnings Ratio (Dynamic)
    "price_to_book": float,      # Price-to-Book Ratio
    "price_to_sales": float      # Price-to-Sales Ratio
}
```

3. **Financial Statement Data**

```python
{
    "net_income": float,          # Net Income
    "operating_revenue": float,    # Total Operating Revenue
    "operating_profit": float,     # Operating Profit
    "working_capital": float,      # Working Capital
    "depreciation_and_amortization": float,  # Depreciation and Amortization
    "capital_expenditure": float,  # Capital Expenditure
    "free_cash_flow": float       # Free Cash Flow
}
```

4. **Trading Signal Data**

```python
{
    "action": str,               # Trading action: buy/sell/hold
    "quantity": int,             # Trading quantity
    "confidence": float,         # Confidence (0-1)
    "agent_signals": [           # Signals from each Agent
        {
            "agent": str,        # Agent name
            "signal": str,       # Signal type: bullish/bearish/neutral
            "confidence": float  # Confidence (0-1)
        }
    ],
    "reasoning": str            # Reasoning
}
```

#### Data Processing Workflow

1. **Data Collection**
   - Obtain the following data via the akshare API:
     - Real-time market data
     - Historical market data
     - Financial metric data
     - Financial statement data
   - Obtain news data via the Sina Finance API
   - Data standardization processing

2. **Data Analysis**
   - Technical Analysis:
     - Calculate technical indicators
     - Analyze price patterns
     - Generate trading signals
   - Fundamental Analysis:
     - Analyze financial statements
     - Evaluate company fundamentals
     - Generate trading signals
   - Sentiment Analysis:
     - Analyze market news
     - Assess market sentiment
     - Generate trading signals
   - Valuation Analysis:
     - Calculate valuation metrics
     - Perform DCF valuation
     - Generate trading signals

3. **Risk Management**
   - Assess market risk
   - Calculate position sizing
   - Set stop-loss and take-profit
   - Control portfolio risk

4. **Investment Decision**
   - Synthesize signals from all agents
   - Assess market conditions
   - Consider portfolio status
   - Generate final trading decision

5. **Data Storage**
   - Cache sentiment analysis results
   - Store news data
   - Record log files
   - Record API calls

6. **System Monitoring**
   - Monitor API calls
   - Track agent analysis
   - Record decision processes
   - Evaluate backtest results

### Agent Collaboration Mechanism

#### Information Sharing
- All agents share the same state object
- Communicate via a message passing mechanism
- Each agent has access to necessary historical data

#### Decision Weights
The Portfolio Management Agent considers different signal weights when making decisions:
- Valuation Analysis: 35%
- Fundamental Analysis: 30% 
- Technical Analysis: 25%
- Sentiment Analysis: 10%

#### Risk Control
- Mandatory risk limits
- Maximum position limits
- Trading size limits
- Stop-loss and take-profit settings

#### System Features
1. **Modular Design**
   - Each agent is an independent module
   - Easy to maintain and upgrade
   - Can be tested and optimized individually

2. **Scalability**
   - Easily add new analysts
   - Supports adding new data sources
   - Can expand decision strategies

3. **Risk Management**
   - Multi-layered risk control
   - Real-time risk assessment
   - Automatic stop-loss mechanism

4. **Intelligent Decision-Making**
   - Based on multi-dimensional analysis
   - Considers multiple market factors
   - Dynamically adjusts strategies

#### Future Outlook
1. **Data Source Expansion**
   - Add more A-share data sources
   - Integrate with more financial data platforms
   - Add social media sentiment data
   - Expand to HK and US stock markets

2. **Feature Enhancement**
   - Add more technical indicators
   - Implement automated backtesting
   - Support multi-stock portfolio management

3. **Performance Optimization**
   - Improve data processing efficiency
   - Optimize decision algorithms
   - Increase parallel processing capabilities

### Sentiment Analysis Feature

The Sentiment Agent is a key component of the system, responsible for analyzing the potential impact of market news and public opinion on stocks.

#### Feature Highlights
1. **News Data Collection**
   - Automatically scrapes the latest stock-related news
   - Supports multiple news sources
   - Updates news data in real-time

2. **Sentiment Analysis Processing**
   - Uses advanced AI models to analyze news sentiment
   - Sentiment score range: -1 (extremely negative) to 1 (extremely positive)
   - Considers news relevance and timeliness

3. **Trading Signal Generation**
   - Generates trading signals based on sentiment analysis results
   - Includes signal type (bullish/bearish)
   - Provides confidence assessment
   - Accompanied by detailed analysis reasoning

#### Sentiment Score Description
| Score Range | Sentiment Level | Typical Scenario |
|----------|----------|----------|
| 1.0 | Extremely Positive | Major positive news, earnings beating expectations, industry policy support |
| 0.5 to 0.9 | Positive | Revenue growth, new project launches, securing orders |
| 0.1 to 0.4 | Slightly Positive | Small contract signings, normal daily operations |
| 0.0 | Neutral | Daily announcements, personnel changes, news with no significant impact |
| -0.1 to -0.4 | Slightly Negative | Minor lawsuits, non-core business losses |
| -0.5 to -0.9 | Negative | Revenue decline, loss of key clients, tightening industry policies |
| -1.0 | Extremely Negative | Major compliance violations, severe core business losses, regulatory penalties |

#### Result Display
![alt text](image.png)
