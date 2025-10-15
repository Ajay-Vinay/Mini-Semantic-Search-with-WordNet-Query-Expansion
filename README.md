
# Mini Semantic Search with WordNet Query Expansion — **Enhanced**

A compact, Colab‑ready **Information Retrieval** project for Text Mining courses.  
You’ll build a **semantic search** engine on a subset of **20 Newsgroups**, compare **TF‑IDF** vs **BM25**, and improve queries with **WordNet synonym expansion** and optional **bigram expansion**. The notebook also includes a **query analyzer**, clear **metrics (Precision@5, MRR)**, and demo searches with term highlighting.

> Notebook: `TextMining_SemanticSearch_WordNet_v4.ipynb`  
> Requirements: `requirements_textmining.txt`

---

## 1) What you will deliver
- A single Colab notebook that:
  - Indexes ~5 categories from 20 Newsgroups
  - Runs **TF‑IDF** and **BM25** retrieval
  - Adds **WordNet** synonym expansion (toggle: nouns‑only or all POS)
  - Optionally adds **bigram expansion** for query phrases found in the corpus vocab
  - Evaluates Baseline vs Expanded (**Precision@5**, **MRR**) and plots bar charts
  - Prints demo results with **highlighted matched terms**
- A short, readable **README** (this file) explaining method, results, and how to run

Good for a quick, unique project that’s easy to explain in viva.

---

## 2) Quick Start (Google Colab)
1. Upload to a repo (or your Drive) and open **`TextMining_SemanticSearch_WordNet_v4.ipynb`** in Colab.  
2. At the top, **uncomment the `pip install` cell** if Colab doesn’t have `rank-bm25` already:
   ```python
   # !pip -q install scikit-learn nltk pandas numpy matplotlib rank-bm25
   ```
3. **Run all cells** top‑to‑bottom. The notebook downloads the dataset automatically.  
4. Scroll to the **metrics section** to view bar charts for **Precision@5** and **MRR**.  
5. Try the **“Query Analyzer & Demo”** cell to see tokens kept, synonyms/bigrams added, and highlighted results.

> Tip: If NLTK WordNet isn’t found, the notebook already runs `nltk.download('wordnet')`. Re‑run the first import cell if needed.

---

## 3) Run Locally (optional)
```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\\Scripts\\activate
pip install -r requirements_textmining.txt
# Start Jupyter (or VS Code) and open TextMining_SemanticSearch_WordNet_v4.ipynb
```

`requirements_textmining.txt` (minimal):
```
scikit-learn
nltk
pandas
numpy
matplotlib
rank-bm25
scipy
```

---

## 4) Project Structure
```
.
├─ TextMining_SemanticSearch_WordNet_v4.ipynb   # main notebook (enhanced)
├─ requirements_textmining.txt
└─ README_TextMining.md                         # (optional) short readme; this file can replace it
```

---

## 5) Dataset
**20 Newsgroups (train subset)** — five categories for speed and clarity:
- `sci.space`
- `comp.windows.x`
- `rec.sport.hockey`
- `talk.politics.mideast`
- `sci.med`

Preprocessing choices:
- We remove headers/footers/quotes (to avoid “category leaks”).
- **TF‑IDF** uses **unigram + bigram** features with English stop‑words, `min_df=2`, `max_df=0.9`.
- **BM25** uses a simple **regex unigram tokenizer** (same notebook function for both index and queries).

---

## 6) Method Overview

### 6.1 Baselines
- **TF‑IDF cosine ranking**: `TfidfVectorizer(ngram_range=(1,2), stop_words='english', min_df=2, max_df=0.9)` → cosine similarity via `linear_kernel`.
- **BM25Okapi** (from `rank-bm25`) on tokenized unigrams.

### 6.2 Query Expansion
- **WordNet synonyms** (default: **nouns‑only**). For each non‑stopword query token:
  - Get up to 3 synonyms from WordNet.
  - Keep **single‑token synonyms** that **exist** in the TF‑IDF vocabulary (prevents “dead” terms).
  - Global cap on total synonyms (default 8).
- **Bigram expansion (optional)**:
  - If adjacent query tokens form a **bigram present in the corpus vocab**, include the bigram as a phrase feature (helps TF‑IDF).

### 6.3 Ranking with Expansion
- **TF‑IDF (Expanded)**: Build an **expanded string** = `kept_tokens + synonyms + bigrams`, then rank with TF‑IDF.
- **BM25 (Expanded)**: Use **unigram** tokens only (`kept_tokens + synonyms`); bigrams are ignored for BM25 in this simple setup.

### 6.4 Evaluation
- **Queries**: A tiny set (10) mapped to expected categories (e.g., “space shuttle orbit” → `sci.space`).  
- **Metrics**: Macro‑averaged **Precision@5 (P@5)** and **Mean Reciprocal Rank (MRR)**.  
- **Comparisons**:
  - TF‑IDF **Baseline** vs **Expanded**
  - BM25 **Baseline** vs **Expanded**

---

## 7) How to Use / Modify

### Toggle POS for expansion
In the config cell:
```python
NOUNS_ONLY = True   # change to False to consider all POS
```
All‑POS yields more recall but can add noise (polysemy).

### Turn bigram expansion on/off
```python
USE_BIGRAM_EXPANSION = True  # set False to disable
```

### Change categories
```python
CATEGORIES = ['sci.space','comp.windows.x','rec.sport.hockey','talk.politics.mideast','sci.med']
# Add or replace with other 20NG categories for a different domain mix.
```

### Add your own query set
Edit `EVAL_QUERIES`:
```python
EVAL_QUERIES = [
    ("space shuttle orbit", "sci.space"),
    ("knee injury surgery", "sci.med"),
    # ... add pairs of (query, target_category_name)
]
```

### Inspect what expansion did
Use the **Query Analyzer & Demo** cell:
- Prints **Tokens kept**, **Synonyms added**, **Bigrams added**, **Expanded query**.
- Shows top‑5 results with **matched terms highlighted**.

---

## 8) Results & What to Expect
- On this subset, **expansion usually increases P@5 and MRR**, especially for TF‑IDF (because synonyms and bigrams align with the vocabulary).
- **BM25** is already strong; unigram synonym expansion often helps but may be smaller gains than TF‑IDF with bigrams.

---

## 11) Troubleshooting
- **`ImportError: rank_bm25`** → run `pip install rank-bm25` (uncomment in the first cell).  
- **NLTK WordNet not found** → the notebook calls `nltk.download('wordnet')`. If still missing, re‑run the first cell.  
- **Slow/Memory issues** → keep 5 categories; 20 Newsgroups is small but doubling categories adds cost.  

---

## 14) Appendix: Key Formulas (informal)
- **TF‑IDF cosine**: Rank by cosine similarity between TF‑IDF vectors of query & document.  
- **BM25**: Scores documents using term frequency saturation + IDF weighting; the widely used IR baseline for lexical search.

---
