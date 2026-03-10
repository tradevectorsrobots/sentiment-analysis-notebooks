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
| v1 | `sentiment_analysis_v1.ipynb` | Basic sentiment analysis using **TextBlob** with polarity scoring on FinViz news headlines |
| v2 | `sentiment_analysis_v2-03-02-2026.ipynb` | Enhanced version using **TextBlob** with polarity & subjectivity analysis on FinViz data |
| v3 | *(not uploaded)* | Originally planned for transformers-based approach but skipped due to dependency issues |
| v4 | `sentiment_analysis_alphavantage_v4-04-01-2026.ipynb` | Alternative approach using **Alpha Vantage API** for news data with sentiment scoring |
| v5 | `sentiment_analysis_v5-04-01-2026.ipynb` | Advanced sentiment analysis using **VADER (NLTK)** on FinViz headlines with compound scoring |

> More versions may be added over time as methodologies and models evolve.

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
pip install requests beautifulsoup4 pandas nltk textblob transformers finvizfinance matplotlib plotly alpha-vantage
```

---

## Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/tradevectorsrobots/sentiment-analysis-notebooks.git
   cd sentiment-analysis-notebooks
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Open any notebook version:
   ```bash
   jupyter notebook sentiment_analysis_v1.ipynb
   ```

4. Run the cells and analyze sentiment results!

---

## Contributing

Contributions are welcome! Feel free to:
- Submit issues for bugs or feature requests
- Create pull requests with improvements
- Share feedback on model accuracy and performance

---

## License

MIT License - feel free to use and modify for your projects.

---

## Contact

Developed by [Trade Vectors LLP](https://github.com/tradevectorsrobots)

For questions or collaboration opportunities, please open an issue or contact us through GitHub.
