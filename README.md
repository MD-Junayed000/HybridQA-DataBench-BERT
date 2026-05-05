# DataBench Question Answering System (SemEval 2024 Task 8)

## Topics
- question-answering
- bert
- transformers
- tabular-data
- semeval-2024
- databench
- nlp
- python

## Overview
This repository contains the implementation of our **DataBench Question Answering System** for **SemEval 2024 Task 8**. The system extracts answers from structured datasets provided in the competition and combines **data preprocessing, rule-based extraction, and a transformer-based QA model** to cover boolean, numerical, categorical, and list-based questions.

## Repository Contents
- `databench-qa-system-deepset-bert-base-cased-ipynb.ipynb`: end-to-end notebook for preprocessing, rule-based QA, and BERT fallback.
- `README.md`: developer-focused documentation.

## Dataset
We utilized the **DataBench dataset**, which consists of multiple structured datasets in `.parquet` format.

🔗 **Dataset link:** [DataBench (SemEval 2024 Task 8)](https://www.codabench.org/competitions/3360/)

Each dataset is structured as follows:
- `all.parquet` - full dataset.
- `sample.parquet` - small subset (first 20 rows).
- `test_qa.csv` - questions and dataset identifiers.

The competition contains 15 datasets across domains such as HR analytics, finance, sports, and healthcare.

## Requirements
- Python 3.10+
- Jupyter Notebook or JupyterLab
- Key dependencies: `pandas`, `numpy`, `torch`, `transformers`, `sentence-transformers`, `nltk`, `scikit-learn`

## Setup
1. Create and activate a virtual environment.
2. Install dependencies:
   ```bash
   pip install pandas numpy torch transformers sentence-transformers nltk scikit-learn
   ```
3. Run the notebook to download NLTK data (`punkt`, `wordnet`) when prompted.

## Running the Pipeline
1. Download the DataBench dataset and extract it locally.
2. Open `databench-qa-system-deepset-bert-base-cased-ipynb.ipynb`.
3. Update the `base_folder` path in the preprocessing section to point at the extracted dataset root.
4. Run the notebook cells in order.

## Methodology
### 1) Data Preprocessing
- Convert `all.parquet` and `sample.parquet` to `cleaned_all.csv` and `cleaned_sample.csv`.
- Normalize missing values, standardize text, and apply dataset-specific cleaning rules.

### 2) Hybrid QA Pipeline
**Rule-based QA**
- Boolean checks (e.g., `max(DailyRate) < 0`).
- Aggregations (sum/mean/min/max/median).
- Category and ranking extraction (mode and `nlargest`).

**Transformer fallback**
- Model: [deepset/bert-base-cased-squad2](https://huggingface.co/deepset/bert-base-cased-squad2).
- Uses dataset context + question for answer extraction in a zero-shot setup.

### 3) Output Formatting
- `predictions.txt` → answers for full datasets.
- `predictions_lite.txt` → answers for sample datasets.

Both files are zipped into `Archive.zip` for submission.

## Results & Observations
✅ Fast rule-based processing for structured queries  
✅ Robust extraction using NLP for ambiguous questions  
✅ High accuracy using structured statistical methods

## Future Improvements
- Integrate BM25 retrieval for improved context matching.
- Explore T5-based generative QA models.
- Parallelize preprocessing for faster iteration.

## Acknowledgments
We thank the **SemEval 2024 Task 8 organizers** for providing the dataset and defining this challenge. Their work has contributed significantly to advancing QA on structured data.
