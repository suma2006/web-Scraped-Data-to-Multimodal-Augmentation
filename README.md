# End-to-End NLP Pipeline: Web-Scraped Data to Multimodal Augmentation

> **Course Project** — Software Tools & Techniques for AI | IIT Gandhinagar

---

## Overview

This repository contains two interconnected assignments that together form a complete NLP data engineering pipeline — starting from raw web scraping, through rigorous data validation and API enrichment, all the way to multi-modal text augmentation with LLMs and audio feature extraction.

---

## Project Structure

```
├── Assignment1_Web_Scraping_Pipeline.ipynb       # Web scraping, Pydantic validation, API enrichment
└── Assignment3_Multimodal_Sentiment_Engine.ipynb # Text augmentation, LLM synthesis, audio pipeline
```

---

## Assignment 1 — Automated Data Pipeline: Web Scraping to Enriched Dataset

### What it does
An end-to-end automated data pipeline built around Wikipedia's *List of Biggest Box-Office Bombs*:

- **Web Scraping**: Scraped **139 records** from Wikipedia using `BeautifulSoup`, handling nested table headers, footnote symbols (`§`, `†`), and raw budget/loss strings like `"$100–160M"`
- **Pydantic Validation**: Cleaned and validated all fields using `Pydantic` schemas — normalizing currency ranges, stripping malformed strings, and enforcing type constraints
- **OMDb API Enrichment**: Enriched every record via the OMDb REST API (genre, director, ratings), with graceful handling of missing/failed responses
- **Cross-Source Consistency Check**: Reconciled scraped vs API-sourced fields, achieving **137/139 (98.6%) verified matches**
- **Categorized Dataset**: Produced an analysis-ready, deduplicated dataset with categorized loss tiers

### Key Libraries
`requests` · `BeautifulSoup` · `pydantic` · `pandas` · `re`

---

## Assignment 3 — The Multimodal Sentiment Engine

### What it does
A multi-stage data augmentation and validation pipeline for a sentiment analysis dataset:

- **Data Consolidation**: Merged three CSV sources (`gold_standard`, `weak_labels`, `llm_labels`), filtered by TF-IDF baseline confidence ≥ 0.65, deduplicated, and handled class imbalance
- **Classical Augmentation**: Synonym replacement using WordNet + back-translation pipeline
- **LLM-Based Synthetic Data Generation**: Few-shot prompted via the **OpenRouter API** to generate high-quality synthetic reviews; filtered low-quality samples using **Jaccard similarity** and **Self-BLEU** diversity scoring
- **Multilingual Validation**: English → Hindi → English back-translation via `deep-translator`, scored with **BLEU**
- **Multimodal Audio Pipeline**:
  - Text-to-speech via `gTTS`
  - MFCC and spectral feature extraction with **Librosa**
  - ASR transcription validated via **OpenAI Whisper** at **96.7% pass rate / 0.012 WER**
- **Benchmarking**: Consolidated all sources into a final augmented training set and benchmarked accuracy gains over a TF-IDF + Logistic Regression baseline

### Key Libraries
`nltk` · `deep-translator` · `gTTS` · `librosa` · `openai-whisper` · `scikit-learn` · `pandas` · `openrouter API`

---

## Results Summary

| Pipeline Stage | Metric | Result |
|---|---|---|
| Web Scraping | Records scraped | 139 |
| Consistency Check | Verified matches | 137/139 (98.6%) |
| Whisper ASR Validation | Pass rate | 96.7% |
| Whisper ASR Validation | Word Error Rate (WER) | 0.012 |

---

## How to Run

### Assignment 1
```bash
pip install requests beautifulsoup4 pydantic pandas
jupyter notebook Assignment1_Web_Scraping_Pipeline.ipynb
```

### Assignment 3
```bash
pip install -r requirements.txt   # see notebook setup cell
jupyter notebook Assignment3_Multimodal_Sentiment_Engine.ipynb
```

> **Note:** Assignment 3 requires API keys for OpenRouter (LLM augmentation). Set them as environment variables in a `.env` file before running.

---

## Author

**Manemoni Sumanjali**  
B.Tech, Artificial Intelligence | IIT Gandhinagar  
[LinkedIn](https://linkedin.com/in/manemoni-sumanjali) · [GitHub](https://github.com/suma2006)
