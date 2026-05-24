# BPE Tokenizer

## Overview

This project covers three tasks:

1. **Data preparation & vocabulary size selection** — loading the Brown corpus, plotting cumulative word coverage, and identifying the minimal vocabulary that covers ≥90% of the text.
2. **BPE tokenizer implementation** — a fully custom `BPETokenizer` class with `train` and `tokenize` methods.
3. **Training & analysis** — training the tokenizer at the selected vocabulary size and measuring fertility and token length statistics on 1 000 random sentences.

## Repository Structure

```
.
├── assignment1_tokenization.ipynb   # Main notebook with all tasks
├── requirements.txt
└── README.md
```

## Tasks

### Task 1 — Vocabulary Size Selection

- Loaded the Brown corpus (~1M words) from Kaggle.
- Computed cumulative coverage as a function of vocabulary size using word frequencies.
- Plotted coverage vs. vocabulary size and located the 90% threshold.
- **Result:** 7 354 unique words cover 90% of the corpus.

**Key findings:**
- Coverage slows down because the most frequent words (e.g., *the*, *and*) appear thousands of times, while later words are rare and contribute little.
- This behavior is explained by **Zipf's Law** (word frequency is inversely proportional to rank) and the **Pareto Principle** (~20% of words cover ~80% of text).

### Task 2 — BPE Tokenizer Implementation

The `BPETokenizer` class in the notebook implements the full BPE algorithm:

| Method | Description |
|---|---|
| `train(corpus)` | Learns merge rules by iteratively finding and merging the most frequent character pair |
| `tokenize(text)` | Applies learned merge rules to split new text into subwords |
| `get_vocab_size()` | Returns the number of unique subwords in the learned vocabulary |

**Algorithm outline:**
1. Initialize vocabulary as individual characters + `</w>` end-of-word marker.
2. Count all adjacent token pairs weighted by word frequency.
3. Merge the most frequent pair into a new token.
4. Repeat for `num_merges` iterations.
5. Apply the learned merge sequence to tokenize new text.

### Task 3 — Training & Analysis

Trained with `num_merges=7000` (targeting the 7 354-word vocabulary from Task 1), then evaluated on 1 000 random sentences.

| Metric | Mean | Std |
|---|---|---|
| **Fertility** (tokens per word) | 2.08 | 0.45 |
| **Tokenized sentence length** | 42.37 tokens | 28.32 |

**Tokenization example:**

```
Original : Sally left her choring to stand beside Dan .
Tokens   : ['s', 'ally</w>', 'le', 'ft</w>', 'her</w>', 'ch', 'or', 'ing</w>', ...]
Fertility: 1.89
```

## Setup

```bash
pip install -r requirements.txt
```

**requirements.txt**
```
pandas
numpy
matplotlib
tqdm
kaggle        # for dataset download
```

Download the dataset:
```bash
kaggle datasets download nltkdata/brown-corpus
```

Then run all cells in `Assignment1.ipynb`.

## Results Summary

| Parameter | Value |
|---|---|
| Corpus size | ~1 000 000 words |
| 90% coverage vocabulary | 7 354 words |
| BPE merges trained | 7 000 |
| Final vocab size | 33 815 subwords |
| Mean fertility | 2.08 |
| Mean token length | 42.37 |
