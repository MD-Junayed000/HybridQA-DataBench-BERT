# DataBench Question Answering System (SemEval 2024 Task 8)

## *Introduction*
This repository contains the implementation of our **DataBench Question Answering System,** developed for **SemEval 2024 Task 8**. The system is designed to extract answers from structured datasets provided in the competition, following the constraints that only provided data can be used for answering questions.

Our approach combines **data preprocessing, rule-based extraction, and a transformer-based Question Answering (QA) model** to ensure high accuracy in responses. The system is capable of handling multiple question formats, ranging from boolean to numerical and categorical queries.

---

## *Dataset*
We utilized the **DataBench dataset**, which consists of multiple structured datasets in .parquet format. Each dataset contains structured tabular data, and questions must be answered using only the provided data.

🔗 *Dataset Link:* [DataBench (SemEval 2024 Task 8)]https://www.codabench.org/competitions/3360/

Each dataset is structured as follows:
- all.parquet - The full dataset.
- sample.parquet - A small subset (first 20 rows) of the dataset.
- test_qa.csv - Contains the questions and corresponding dataset identifiers.

The competition consists of 15 datasets, each representing a different domain such as HR analytics, finance, sports, healthcare, and more.
---

## *Methodology*
### *1. Data Preprocessing*
Before running the model, we preprocess the data for consistency and efficiency. The steps include:
- **Conversion**: We convert all.parquet and sample.parquet into structured CSV files (cleaned_all.csv and cleaned_sample.csv).
- **Cleaning & Normalization**: Missing values are handled appropriately:
  - Categorical values are filled with 'Unknown'.
  - Numerical values are filled using *median imputation*.
  - Text columns are standardized (lowercasing, trimming extra spaces).
- **Feature Engineering**: Extracting key numerical and categorical statistics to aid question-answering.
- **Text Normalization**: We apply stemming, lemmatization, and remove special characters for better matching.

### *2. Question Answering Pipeline*
Our system processes each question from test_qa.csv and determines the appropriate extraction method. We use a **hybrid approach** consisting of:

#### *A) Rule-Based Question Answering*
We extract answers directly from structured datasets using logical rules:
- *Boolean Questions (Yes/No)*:
  - If the question contains words like Is, Does, Can, Are, we check the dataset conditionally.
  - Example: "Is the highest DailyRate negative?" → Check if max(DailyRate) < 0.
- *Numerical & Statistical Questions*:
  - Sum, average, min/max, median values are computed for relevant fields.
  - Example: "What is the average employee age?" → Compute mean(Age).
- *Category Extraction*:
  - The most frequent category from a column is retrieved.
  - Example: "What is the most common author in the dataset?" → Find mode(Author).
- *List-Based Answers*:
  - For ranking-based queries (e.g., "List the top 3 players by points"), we use nlargest() on numerical columns.

#### *B) Transformer-Based QA Model*
If rule-based extraction fails, we fall back to a *pretrained transformer model*:
- Model Used: [deepset/bert-base-cased-squad2](https://huggingface.co/deepset/bert-base-cased-squad2) (110M parameters)
- *How it Works*:
  - Extracts relevant *context* from the dataset.
  - Passes the context and question to BERT for answer generation.
  - Example: "Who is the top player in the NBA dataset?" → BERT extracts the best match.
- The QA model runs in a *zero-shot setting* (no fine-tuning was done).

### *3. Output Formatting & Submission*
The extracted answers are stored in:
- predictions.txt → Answers for full datasets.
- predictions_lite.txt → Answers for sample.parquet datasets.

Both files are compressed into Archive.zip and submitted to the competition platform.

---

## *Results & Observations*
Our hybrid approach ensures:

✅ *Fast rule-based processing for structured queries*

✅ *Robust extraction using NLP for ambiguous questions*

✅ *High accuracy using structured statistical methods*

---

## *Future Improvements*
We plan to enhance our system by Trial and Error can :
- Integrating *BM25 retrieval* for better matching.
- Exploring *T5-based generative answering models*.
- Parallelizing data processing for speed improvements.


---

## *Acknowledgments*
We thank the **SemEval 2024 Task 8 organizers** for providing the dataset and defining this challenge.Their work has contributed significantly to advancing question answering on structured data.

---
