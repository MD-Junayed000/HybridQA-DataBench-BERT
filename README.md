# DataBench Competition - Task 8 (SemEval 2024)
## Question Answering from Structured Datasets

🚀 A hybrid NLP pipeline for answering dataset-based questions using structured queries and transformer-based QA models.

---

## 📌 Project Overview
This repository contains the implementation for Task 8 of the SemEval 2024 DataBench Competition, which involves automated question answering from structured datasets.

The primary objective of this competition is to develop a system that can extract accurate answers to given questions using only the provided datasets, without relying on external knowledge.

- 🔹 Our solution combines structured data processing, rule-based extraction, and a transformer-based QA model (BERT) to extract the most relevant answers.
- 🔹 The methodology follows data preprocessing, structured query-based answering, and contextual LLM inference to ensure maximum accuracy.
- 🔹 The final predictions are formatted and submitted in the required format (predictions.txt & predictions_lite.txt).

---

## 📂 Dataset Information
The competition datasets can be found on the official [https://www.codabench.org/competitions/3360/]

Upon downloading, will receive:

- A test_qa.csv file containing questions and dataset references.
- 15 folders, each representing a different dataset (e.g., 066_IBM_HR, 080_Books, etc.).
- Each folder contains:
  - **all.parquet**: Full dataset.
  - **sample.parquet**: A sample of the full dataset (first 20 rows).

---

## 🚀 Approach and Methodology
### Step 1: Data Preprocessing
- Converted all.parquet and sample.parquet files into *cleaned CSV files* (cleaned_all.csv, cleaned_sample.csv).
- Applied *dataset-specific preprocessing* to standardize text fields, handle missing values, and ensure numerical consistency.
- *Key processing tasks included:*
  - *Text Standardization:* Lowercasing, trimming, and handling special characters.
  - *Handling Missing Values:* Filling NaNs with appropriate placeholders (Unknown, 0, etc.).
  - *Numerical Conversion:* Ensuring proper numeric formats for calculations.
  - *Feature Extraction:* Extracting relevant columns for structured queries.

### Step 2: Answer Extraction
Two different methods were used to extract answers:

#### 📌 1️⃣ Structured Query-Based Answering
- For *simple numerical or categorical questions, a **rule-based approach* was used.
- Used *TF-IDF similarity* to match question keywords with dataset column names.
- Applied *direct lookup and aggregation*:
  - *Boolean Questions:* Checking conditions (Yes/No).
  - *Numerical Answers:* Extracting max, min, avg, sum, etc.
  - *List-Based Answers:* Ranking items based on frequency.

#### 📌 2️⃣ Transformer-Based Question Answering
- Used **bert-large-uncased-whole-word-masking-finetuned-squad** as the *fallback for complex queries*.
- Extracted *relevant dataset context* and *fed it into the QA model* for inference.
- Applied *answer length restrictions* and *score filtering* to avoid incorrect outputs.

### Step 3: Answer Formatting
- *Ensured competition-compliant output formats*:
  - *Boolean:* "Yes" / "No".
  - *Number:* Integer or floating-point values.
  - *Category:* Most frequent categorical value.
  - *List:* Properly formatted ['item1', 'item2'].
- Saved results to:
  - predictions.txt (for all.parquet datasets).
  - predictions_lite.txt (for sample.parquet datasets).
- Compressed and submitted the final **Enhanced_Archive.zip**.

---

## 📌 Technologies Used
| Tool/Library  | Purpose  |
|--------------|----------|
| *Pandas*  | Dataset loading and structured querying |
| *NumPy*  | Numerical operations and aggregations |
| *Hugging Face Transformers*  | BERT-based QA inference |
| *Scikit-learn (TF-IDF)*  | Column similarity matching |
| *Regex & NLTK*  | Text normalization and pattern matching |
| *Zipfile*  | File packaging for submission |

---

## ⚡ Model Details
| Model  | Description  |
|--------|-------------|
| **bert-large-uncased-whole-word-masking-finetuned-squad**  | Transformer-based QA model trained on SQuAD v2 (340M parameters). Used for extracting answers from structured text data. |

---

## 💡 Key Features & Enhancements
✅ *Multi-Step Processing Pipeline*: Preprocessing → Query Matching → QA Model → Answer Formatting.  
✅ *Hybrid Approach: Combines **structured querying + transformer inference*.  
✅ *TF-IDF Similarity Matching*: To dynamically identify relevant columns.  
✅ *Robust Data Cleaning: Dataset-specific preprocessing for **better accuracy*.  
✅ *Competition-Compliant Formatting: Ensures **valid answers in required formats*.  
✅ *Parallel Execution & GPU Support*: Optimized for faster processing.  

---



