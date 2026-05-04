# 🔐 LLM-Based Security Classification System

> NLP & Prompt Engineering · Google Gemini API · HuggingFace Transformers · Python  
> **Aug 2024 – Sep 2024**

---

## What It Does

Automatically classifies URLs into 4 threat categories using a **4-level cost-sensitive LLM pipeline**:

| Label | Description |
|---|---|
| `benign` | Safe, legitimate website |
| `phishing` | Credential / data stealing page |
| `malware` | Distributes malicious software |
| `defacement` | Hacked or visually altered website |

---

## Architecture

```
Input URL
   ↓
[Level 0] Gemini 1.5 Flash — Full Prompt
   ↓ (if quality score < 0.55)
[Level 1] Gemini 1.5 Flash — Simplified Prompt (~40% cheaper)
   ↓ (if quality score < 0.55)
[Level 2] DistilBERT NLI — Local Zero-Shot (no API, free)
   ↓ (if all above fail)
[Level 3] Heuristic Rules — Instant, zero cost
   ↓
Validated JSON Output (Pydantic Schema)
```

---

## Key Results

- ✅ **20% reduction** in false positives vs. baseline zero-shot
- ✅ **~0.80 Macro F1-Score** across 4 threat classes
- ✅ **~80% of URLs** resolved at Level 0 (highest quality)
- ✅ **100% coverage** — no URL goes unclassified

---

## Quick Start

### 1. Clone & Install
```bash
git clone https://github.com/yourname/llm-security-classifier
cd llm-security-classifier
pip install google-generativeai pydantic transformers scikit-learn pandas matplotlib tqdm
```

### 2. Set API Key
```bash
export GEMINI_API_KEY="your_key_here"
# Get free key → https://aistudio.google.com/apikey
```

### 3. Run on Kaggle
- Add dataset: **Malicious URLs** by sid321axn
- Add secret: `GEMINI_API_KEY`
- Run all cells in `LLM_Security_Classifier.ipynb`

---

## Project Structure

```
├── LLM_Security_Classifier.ipynb   ← Main notebook (Kaggle)
├── README.md
└── outputs/
    ├── security_classification_results.csv
    ├── quality_validation_log.csv
    ├── pipeline_summary.json
    └── security_dashboard.png
```

---

## Output Schema

Every prediction returns a validated JSON object:

```json
{
  "classification":  "phishing",
  "confidence":      0.91,
  "risk_indicators": ["brand impersonation", "suspicious TLD"],
  "reasoning":       "URL mimics PayPal login page on a .xyz domain.",
  "severity":        "high"
}
```

---

## Dataset

**Malicious URLs Dataset** — [Kaggle](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset)  
651,191 URLs · 4 classes · Working sample: 240 URLs (60/class, stratified)

---

## Tech Stack

`Python 3.12` · `google-generativeai` · `pydantic` · `transformers` · `scikit-learn` · `pandas` · `matplotlib`
