# sentiment-analysis-notebooks

> **Sentiment Analysis on Financial News using NLP & Machine Learning**
> Multiple versioned Jupyter Notebooks by [Trade Vectors LLP](https://github.com/tradevectorsrobots)

---

## Overview

This repository contains a series of Jupyter Notebooks that perform **Sentiment Analysis on financial news headlines and articles**. The news data — both live (real-time) and historical — is sourced from **[FinViz](https://finviz.com/)**, a leading financial market visualization and screener platform.

Each notebook represents a different version or approach to sentiment analysis, allowing comparison of methods, models, and accuracy over time.

---

## Data Source: FinViz

[FinViz (Financial Visualizations)](https://finviz.com/) is used as the primary news data source for this project.

### Why FinViz?
- Provides **live (real-time) news headlines** for individual stocks and the broader market
- Provides access to **historical news** headlines tied to specific ticker symbols
- News is aggregated from multiple financial media sources (Reuters, Bloomberg, MarketWatch, etc.)
- Easy to scrape using Python (`requests` + `BeautifulSoup`) or via the `finvizfinance` library

### How News is Fetched

```python
# Example: Fetching news for a stock ticker using finvizfinance
from finvizfinance.quote import finvizfinance

stock = finvizfinance('AAPL')
news_df = stock.ticker_news()
print(news_df.head())
```

Or using direct web scraping:

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

def get_finviz_news(ticker):
    url = f'https://finviz.com/quote.ashx?t={ticker}'
    headers = {'User-Agent': 'Mozilla/5.0'}
    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.text, 'html.parser')
    news_table = soup.find(id='news-table')
    rows = news_table.findAll('tr')
    parsed = []
    for row in rows:
        title = row.a.text
        date_data = row.td.text.split()
        parsed.append([date_data, title])
    return pd.DataFrame(parsed, columns=['Date', 'Headline'])
```

---

## Notebook Versions

| Version | Notebook | Description |
|---------|----------|-------------|
| v1 | `sentiment_v1_vader.ipynb` | Basic sentiment scoring using VADER (NLTK) on FinViz headlines |
| v2 | `sentiment_v2_textblob.ipynb` | Sentiment analysis using TextBlob polarity & subjectivity |
| v3 | `sentiment_v3_transformers.ipynb` | Deep learning approach using HuggingFace FinBERT model |
| v4 | `sentiment_v4_visualization.ipynb` | Aggregated sentiment scores with chart visualizations |

> More versions will be added over time.

---

## Workflow

```
FinViz Website
    |
    |-- Live News (real-time headlines per ticker)
    |-- Historical News (past headlines per ticker)
    |
    v
Data Scraping (Python: requests / finvizfinance)
    |
    v
Data Cleaning & Preprocessing
    |
    v
Sentiment Analysis (VADER / TextBlob / FinBERT)
    |
    v
Scoring & Labeling (Positive / Negative / Neutral)
    |
    v
Visualization & Insights (Matplotlib / Plotly)
```

---

## Requirements

```bash
pip install requests beautifulsoup4 pandas nltk textblob transformers finvizfinance matplotlib plotly
```

---

## Libraries Used

- `requests` & `BeautifulSoup4` - Web scraping FinViz news
- `finvizfinance` - Python wrapper for FinViz data
- `NLTK / VADER` - Rule-based sentiment analysis
- `TextBlob` - Simple NLP sentiment scoring
- `HuggingFace Transformers (FinBERT)` - Finance-domain BERT model
- `Pandas` - Data manipulation
- `Matplotlib / Plotly` - Visualization

---

## Notes

- FinViz free tier is used for scraping; please respect their [Terms of Service](https://finviz.com/terms-of-service.ashx) and add appropriate delays between requests.
- For high-frequency or bulk historical data, consider using FinViz Elite API.
- Notebooks are independent and can be run separately.

---

## Author

**Trade Vectors LLP** | Mumbai, India
Algorithmic Trading | Financial Research | NLP & AI Applications
GitHub: [@tradevectorsrobots](https://github.com/tradevectorsrobots)
