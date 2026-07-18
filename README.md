# 🌀 Atomberg — Share of Voice (SoV) & Sentiment Analysis

A data-driven marketing intelligence case study that measures **Atomberg's digital Share of Voice (SoV)** against competing ceiling fan brands across **Google Search** and **YouTube**, and analyzes **customer sentiment** to surface actionable brand insights.

> 📊 Built as a case study to demonstrate an end-to-end pipeline: web data collection → cleaning → vector embeddings → SoV metrics → sentiment analysis → keyword extraction → visualization.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Key Questions Answered](#-key-questions-answered)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Methodology / Pipeline](#-methodology--pipeline)
- [Setup & Installation](#-setup--installation)
- [Usage](#-usage)
- [Results & Insights](#-results--insights)
- [Recommendations](#-recommendations)
- [Limitations & Future Work](#-limitations--future-work)
- [Author](#-author)
- [License](#-license)

---

## 🧭 Overview

**Atomberg** is a smart-appliance brand best known for its BLDC (Brushless DC) energy-efficient ceiling fans. This project scrapes real-world search and video data for a set of fan-related queries (e.g. *"best smart fan in India"*, *"BLDC ceiling fan review"*) and answers:

- How much of the online conversation does Atomberg *own* compared to competitors like Orient, Crompton, Havells, Usha, Bajaj, etc.?
- Is that conversation mostly positive, negative, or neutral?
- What specific themes/keywords are driving positive vs. negative sentiment?

The result is a **Share of Voice (SoV)** and **Share of Positive Voice (SoPV)** report, backed by visual charts and a semantic vector database of the collected content for further exploration (e.g. RAG-based Q&A over the dataset).

## ❓ Key Questions Answered

| Question | Method |
|---|---|
| How visible is Atomberg vs. competitors on Google & YouTube? | Share of Voice (SoV) — brand mention frequency per platform |
| What's the public sentiment around Atomberg? | VADER sentiment scoring (Positive / Neutral / Negative) |
| What language/topics drive that sentiment? | KeyBERT keyword extraction on sentiment-grouped text |
| How does Atomberg compare across platforms? | Cross-platform SoV & SoPV comparison charts |
| Can this content be searched semantically later? | Sentence-Transformer embeddings stored in a FAISS vector index |

## 🛠 Tech Stack

| Component | Tools Used | Purpose |
|---|---|---|
| **Language** | Python 3.13 | Core coding & analysis |
| **Data Collection** | `requests`, SerpAPI (Google), YouTube Data API v3 | Scraping/fetching search & video results |
| **Data Wrangling** | `pandas`, `numpy` | Cleaning, deduplication, transformation |
| **Embeddings / Vector DB** | `sentence-transformers`, `faiss-cpu` | Semantic embeddings for future RAG/semantic search |
| **Sentiment Analysis** | `vaderSentiment` | Rule-based sentiment scoring of text snippets |
| **Keyword Extraction** | `KeyBERT` | Extracting representative keywords per sentiment class |
| **Visualization** | `matplotlib` | SoV and sentiment comparison charts |
| **Environment Config** | `python-dotenv` | Secure API key management via `.env` |

> Note: The original case study report also lists `transformers` (DistilBERT) and a locally-run **Ollama** LLM as tools explored for advanced sentiment classification and insight summarization — these are natural next steps documented under [Future Work](#-limitations--future-work).

## 📁 Project Structure

```
atomberg-sov-analysis/
├── Atom.ipynb                 # Main analysis notebook (data collection → insights)
├── ATOMBERG_Case_Study_Report.pdf   # Final write-up: methodology, findings, recommendations
├── requirements.txt            # Python dependencies
├── .env.example                 # Template for required API keys
├── .gitignore                    # Files/folders excluded from version control
├── LICENSE
└── README.md
```

## 🔄 Methodology / Pipeline

```
1. Query Definition
   → 10 seed queries around smart/BLDC ceiling fans

2. Data Collection
   → Google organic results  (SerpAPI)
   → YouTube video results   (YouTube Data API v3)
   → Combined into a single DataFrame (title, snippet, link, platform, query)

3. Cleaning
   → Duplicate detection & removal (by title, then by link)

4. Vector Database
   → Embed (title + snippet) with `all-MiniLM-L6-v2`
   → Store in a FAISS index (fan_sov_index.faiss) for semantic search

5. Share of Voice (SoV)
   → Count brand-name mentions per platform across a 22-brand competitor list
   → SoV (%) = (brand mentions / total docs on platform) × 100

6. Sentiment Analysis
   → VADER compound score → Positive / Neutral / Negative label per document

7. Share of Positive Voice (SoPV)
   → SoPV (%) = (positive mentions / total mentions) × 100, per brand & platform

8. Keyword Extraction
   → KeyBERT top keywords for Positive / Neutral / Negative content buckets

9. Visualization
   → Bar charts: Top-7 brands by SoV (Google & YouTube)
   → Sentiment distribution charts per brand/platform
```

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/atomberg-sov-analysis.git
cd atomberg-sov-analysis
```

### 2. Create a virtual environment (recommended)
```bash
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure API keys
Copy the example env file and add your own keys:
```bash
cp .env.example .env
```

You'll need:
- **SERPAPI_KEY** — from [serpapi.com](https://serpapi.com/) (for Google search results)
- **YOUTUBE_API_KEY** — from the [Google Cloud Console](https://console.cloud.google.com/) (enable "YouTube Data API v3")

```env
SERPAPI_KEY=your_serpapi_key_here
YOUTUBE_API_KEY=your_youtube_api_key_here
```

## ▶️ Usage

Launch Jupyter and run the notebook top-to-bottom:

```bash
jupyter notebook Atom.ipynb
```

The notebook will:
1. Fetch Google + YouTube results for the defined queries
2. Clean and deduplicate the combined dataset
3. Build sentence embeddings + FAISS vector index
4. Compute SoV and SoPV tables
5. Run sentiment classification and keyword extraction
6. Render comparison charts inline

> 💡 Tip: Reduce the `num_results` / `max_results` parameters in the scraping functions if you're on a rate-limited API tier.

## 📈 Results & Insights

- **Share of Voice:** Atomberg's SoV performs strongly relative to competitor brands across the fetched dataset — consistently placing among the top brands mentioned on both Google and YouTube.
- **Sentiment (Google):** Sentiment around Atomberg from Google-sourced content is largely favorable, with minimal negative signal.
- **Sentiment (YouTube):** YouTube shows a small but notable negative sentiment share for Atomberg (**~2.33%**), primarily tied to customer dissatisfaction around product/service experience.
- **Share of Positive Voice (SoPV):** Atomberg's SoPV is high relative to the overall brand set, indicating that when the brand is mentioned, the surrounding sentiment tends to be favorable.

## ✅ Recommendations

1. **Address service-related dissatisfaction** surfaced in YouTube comments/negative sentiment — proactive customer support or resolution offers could convert detractors.
2. **Sustain the strong SoPV performance** by continuing to reinforce the themes (energy efficiency, smart features, remote control) that drive positive sentiment.
3. **Monitor platform-level sentiment gaps** (Google vs. YouTube) separately, since video comment sentiment can behave differently from search snippet sentiment.

## 🚧 Limitations & Future Work

- Sample size is capped at N=30 results per query per platform, chosen to balance relevance, redundancy, and processing speed — a larger N would improve statistical confidence.
- Sentiment is currently rule-based (VADER); the case study report identifies `transformers` (DistilBERT) as a planned upgrade for more nuanced classification.
- The FAISS vector index is built but not yet wired into a full **RAG (Retrieval-Augmented Generation)** pipeline — a local LLM (e.g. via **Ollama**) could be layered on top for natural-language Q&A over the scraped brand data.
- Brand detection uses simple substring matching, which can be extended with fuzzy matching or NER for more robust attribution.

## 👤 Author

**Ashutosh Singh**

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
