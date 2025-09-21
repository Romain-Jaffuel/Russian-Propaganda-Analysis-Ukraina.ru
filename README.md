# Ukraina.ru — Narrative Analysis 

## Goal
Map and characterize narratives on ukraina.ru (History / Opinion / Interview). Track dominant themes, temporal dynamics, and linguistic patterns.

## Corpus
- 4000+ articles scrapped
- Raw and cleaned data in JSON/CSV
- External samples for propaganda/fake-news experiments

## What works (priority)

### Scraping & cleaning pipeline
- Section-wise scraping to JSON/CSV
- Cleaning: HTML strip, unicode normalization, boilerplate removal, sentence splitting, lowercasing, language detection, simple dedup
- Consolidated outputs in data_cleaned_*.json for History and Opinion

### Diachronic analysis
- Weekly/monthly aggregations
- Keyword lists, n-grams, co-occurrences
- Clear spikes around 2022
- Figures saved in Graph/

### Unsupervised clustering
- Document embeddings (ruBERT / RoBERTa)
- K-means (k via silhouette) and HDBSCAN
- Stable clusters for recurring blocks: WWII/memory, Maidan–Bandera, Crimea–Donbas, “historical unity”
- Practical use: flagging extreme or atypical articles

### Targeted LDA
- Global and sub-corpus LDA (e.g., “Bucha” window)
- Readable topics for major narratives; mainly exploratory features

### Sentiment baseline
- Lexicon-based polarity for Russian, aggregated per article
- Mostly neutral overall; higher negativity on event windows
- Weak alone, useful when fused with topics/clusters

## Notable attempts (brief)
- VADER + LDA combined: weak signal on long-form Russian news
- Generic fake-news classifiers: domain mismatch and overfit
- High-k full-corpus LDA: diluted topics, sub-corpus modeling works better

## Methods

### ETL
1) Scraping_*.ipynb — fetch by section, parse HTML, export JSON/CSV  
2) Data_Cleaning_*.ipynb — strip tags, normalize, segment, language detect, dedup, export data_cleaned_*.json  
3) Indexing: article id, date, section, URL, author when present

### Representations
- Russian tokenization, custom stopwords, optional lemmatization (pymorphy2, razdel)
- Embeddings from ruBERT/RoBERTa; CLS or sentence-mean pooled to document vectors

### Clustering
- K-means (k via silhouette), HDBSCAN for density
- Post-label clusters with top TF-IDF terms
- Stability checks via resampling

### Topic modeling
- Gensim LDA with bigrams/trigrams; coherence (UMass/NPMI) for selection
- Sub-corpus seeding by keyword windows to avoid dilution

### Diachrony
- Time-bucketed counts, n-grams, co-occurrences
- Export series and plots to Graph/

### Propaganda / fake-news
- Fine-tuning with external sets (datasets_propaganda, translated SemEval tried using Argos library)
- Transfer kept as weak features pending domain-specific calibration

## Results (summary)
- Dominant themes are stable and machine-detectable (clustering + LDA)
- Temporal dynamics align with major milestones (2022 full-scale invasion)
- Polarity alone is low-value; adds signal when combined with topics/clusters
- Propaganda classifiers too generic at this stage; require in-domain supervision

## Known limits
- Low amount of articles around 2015-2016
- State-of-art for propaganda techniques detection still low (F1-score < 0.5)
- Heterogeneous metadata (authors, views, sections)
- Strong source bias: automated claims need human review
- No systematic evaluation against academic baselines yet

## Data and ethics
Research use only. Dataset available for reasonable request.
