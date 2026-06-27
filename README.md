# Web-Scraped Data to Multimodal Augmentation

Course project for Software Tools & Techniques for AI at IIT Gandhinagar.

This repo has two assignments that I worked on back-to-back — the first one is about pulling real data from the web and cleaning it properly, and the third one takes that idea further into text augmentation and audio processing. Together they cover a decent chunk of what a real NLP data pipeline looks like.

---

## What's inside

```
Assignment1_Web_Scraping_Pipeline.ipynb       — scraping, validation, API enrichment
Assignment3_Multimodal_Sentiment_Engine.ipynb — augmentation, LLM synthesis, audio pipeline
```

---

## Assignment 1 — Box Office Bombs: Scraping to Dataset

The goal was to scrape Wikipedia's list of biggest box-office bombs, clean the mess, and enrich it with external API data.

Wikipedia tables are never clean — this one had nested headers, footnote symbols stuck to film titles (like `§` and `†`), and budget fields written as ranges (`$100–160M`). I scraped 139 records using BeautifulSoup, then wrote Pydantic schemas to validate and normalize every field.

After cleaning, I hit the OMDb API to pull in extra details (genre, director, ratings) for each film, with proper error handling for the ones that didn't return anything useful. The last step was a cross-source consistency check — comparing what I scraped vs what the API returned. 137 out of 139 matched up (98.6%), and the final dataset came out categorized and ready for analysis.

**Libraries used:** `requests`, `beautifulsoup4`, `pydantic`, `pandas`

---

## Assignment 3 — Multimodal Sentiment Engine

This one was more involved. The task was to take a small sentiment dataset and expand it using multiple augmentation techniques, then validate the quality of what was generated — including an audio pipeline at the end.

I started by merging three CSVs (gold standard labels, weak labels, LLM-labeled data), ran a TF-IDF baseline to filter out low-confidence samples, and deduplicated everything into a consolidated base.

For augmentation:
- Classical methods: synonym replacement via WordNet, back-translation
- LLM-based: few-shot prompting through the OpenRouter API to generate synthetic reviews. I filtered these using Jaccard similarity and Self-BLEU so only diverse, non-repetitive samples made it in

For multilingual validation, I ran English → Hindi → English back-translation using `deep-translator` and scored the round-trip with BLEU.

The audio pipeline converted text to speech with gTTS, extracted MFCC and spectral features using Librosa, then passed the audio through OpenAI Whisper for ASR validation. Whisper passed 96.7% of samples with a WER of 0.012.

At the end, I compared accuracy on the augmented training set vs the original baseline to measure whether the augmentation actually helped.

**Libraries used:** `nltk`, `deep-translator`, `gTTS`, `librosa`, `openai-whisper`, `scikit-learn`, `pandas`, OpenRouter API

---

## Numbers

| What | Result |
|---|---|
| Records scraped (Assignment 1) | 139 |
| Cross-source consistency | 137/139 — 98.6% |
| Whisper ASR pass rate | 96.7% |
| Word Error Rate (WER) | 0.012 |

---

## Running locally

**Assignment 1**
```bash
pip install requests beautifulsoup4 pydantic pandas
jupyter notebook Assignment1_Web_Scraping_Pipeline.ipynb
```

**Assignment 3**
```bash
# follow the setup cell at the top of the notebook
pip install -r requirements.txt
jupyter notebook Assignment3_Multimodal_Sentiment_Engine.ipynb
```

For Assignment 3, you'll need an OpenRouter API key set in a `.env` file for the LLM augmentation step.

---

Manemoni Sumanjali — B.Tech AI, IIT Gandhinagar  
[LinkedIn](https://linkedin.com/in/manemoni-sumanjali) · [GitHub](https://github.com/suma2006)
