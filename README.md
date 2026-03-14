# Seeds: Multi-Modal Equity Intelligence Engine

**Seeds** is an automated data pipeline that synthesizes retail sentiment, fundamental financial health, and technical indicators into institutional-grade investment briefs. The system transforms unstructured alternative data into actionable market intelligence using Large Language Models (LLMs).

---

## Pipeline Architecture

The application executes a six-phase enrichment process to generate comprehensive stock profiles:

### 1. Social Sentiment Scraping
* Monitors 19 high-signal subreddits via the **Reddit API (PRAW)**, including `r/ValueInvesting`, `r/wallstreetbets`, and `r/options`.
* Captures 50 top posts per subreddit using a weekly time filter.
* Extracts the top 50 comments per post to identify community counter-theses and sentiment nuances.

### 2. LLM Sentiment Extraction
* Utilizes **Gemini 2.0 Flash** via OpenRouter for high-precision information extraction.
* Identifies and filters for valid US-listed tickers, excluding Crypto and Forex.
* Categorizes tickers into investment styles: Hype, Value, Short Term Play, or Long Term Hold.
* Weights community comments more heavily than original posts to detect potential manipulation or "pump" narratives.

### 3. Fundamental Financial Health
* Integrates real-time financial data via **yfinance**.
* Evaluates solvency using Interest Coverage and Current Ratio.
* Assesses profitability through Operating Margin and Free Cash Flow (FCF) Margin.
* Measures capital efficiency using Return on Invested Capital (ROIC) and valuation via PEG Ratio.

### 4. Technical Indicator Calculation
* Calculates a proprietary suite of technical markers for market timing.
* **Momentum:** MACD Histogram, ADX (Trend Strength), and 10-day Rate of Change (ROC).
* **Volatility:** Beta and Bollinger Band width.
* **Oscillators:** RSI (14), Money Flow Index (MFI), and Stochastic %K.
* **Trend:** SMA 50/200 crossovers and Parabolic SAR (PSAR).

### 5. Multi-Persona Narrative Synthesis
* Generates dual-persona summaries to cater to different end-users:
    * **Financial Professional:** Metric-heavy, technical analysis focusing on balance sheet health and capital allocation.
    * **Smart Layman:** Accessible explanations of business safety, profitability, and valuation.

### 6. Final Investment Decision
* Produces a decisive Chief Market Strategist brief.
* Assigns a final **Signal**: Strong Buy, Buy, Hold, Sell, or Strong Sell.
* Applies an **Opportunity Tag**: Sure-Shot, Speculative, High Risk High Reward, or Minimal Opportunity.
* Provides a direct Action Note (e.g., "Action: BUY" or "Action: SHORT").

---

## Technical Stack

| Component | Technology |
| :--- | :--- |
| **Language** | Python 3.9+ |
| **Data Orchestration** | Pandas, Numpy |
| **Alternative Data** | PRAW (Reddit API) |
| **Market Data** | yfinance |
| **AI / LLM** | Google Gemini 2.0 Flash (via OpenRouter) |
| **Concurrency** | ThreadPoolExecutor |

---

## Data Output

The final result is consolidated in `seeds_df_final.csv`. This dataset includes:
* **Quantitative Metrics:** Mentions, price, RSI, ROIC, and technical scores.
* **Qualitative Assessments:** Risk Level, Ticker Category, and Trade Signal.
* **Narrative Synthesis:** Structured briefs at 200, 100, 50, and 25-word lengths.
* **Metadata:** Company descriptions and detailed subreddit breakdowns.

---

## Execution and Setup

1.  **API Keys:** Valid credentials for the Reddit API and OpenRouter are required.
2.  **Environment:** Requires `praw`, `yfinance`, `pandas`, `requests`, and `tqdm`.
3.  **Process:** Execute `Generate_Seeds_Data.ipynb` sequentially. The script utilizes interim checkpointing (`v1` through `v5`) to ensure data persistence across API calls.
