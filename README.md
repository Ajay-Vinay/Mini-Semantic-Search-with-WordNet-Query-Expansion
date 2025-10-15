
# Mini Semantic Search with WordNet Query Expansion

A small, **Colab-ready** Text Mining/IR project: build a **TF-IDF** search index over a subset of **20 Newsgroups** and improve retrieval by **expanding queries** with **WordNet synonyms**. Compare **Baseline** (no expansion) vs **Expanded** using **Precision@5** and **MRR**.

## How to Run (Colab)
1. Open `TextMining_SemanticSearch_WordNet_v2.ipynb` in Google Colab.
2. Run cells top-to-bottom. The notebook fetches 20NG automatically.
3. See:
   - Metric bars for **P@5** and **MRR**
   - A demo search flow (try your own queries)
   - Highlighted terms (original tokens + synonyms)

## Files
- `TextMining_SemanticSearch_WordNet_v2.ipynb` — main notebook
- `requirements_textmining.txt` — minimal deps

