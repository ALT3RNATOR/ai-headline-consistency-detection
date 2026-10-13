
# AI-Based Headline Consistency Detection

An NLP-based system for detecting semantic inconsistencies between news headlines and their corresponding news articles. This project compares **TF-IDF + Cosine Similarity** with **Sentence Embeddings + Cosine Similarity** and evaluates their ability to identify misleading or deliberately modified headlines.

## Project Overview

Misleading and clickbait headlines can misrepresent the content of a news article. This project investigates whether traditional text similarity and modern semantic similarity techniques can identify inconsistencies between a headline and its corresponding article text.

The experiment uses news datasets, prepares data for binary classification, and introduces deliberately modified headlines to evaluate model robustness.

## Objectives

- Detect semantic consistency between headlines and news text.
- Compare traditional and embedding-based similarity approaches.
- Test the models using original and deliberately modified headlines.
- Evaluate performance using Accuracy, Precision, Recall, Macro-F1, and ROC-AUC.
- Analyze the strengths and limitations of lexical versus semantic similarity.

## Dataset Download

The datasets used in this project are hosted on Google Drive to keep the GitHub repository lightweight.

**[Download All Project Datasets from Google Drive](https://drive.google.com/drive/folders/1ppKWCanMYHJ7gOKSAB0nVhZPE5op66qC?usp=sharing)**

Open the folder and download the required CSV files before running the notebook.

### Datasets Included

| File | Description |
|---|---|
| `ISOT_DS.csv` | News dataset containing article titles, article text, subject, date, and label columns. |
| `FNC1_binary.csv` | Binary-labelled version of the FNC-1 dataset prepared for experimentation. |
| `FNC1_merged.csv` | Merged FNC-1 dataset intended for consolidated preprocessing and analysis. |
| `FNC1_binary_analysis.csv` | Dataset file intended for examining and analyzing the prepared binary FNC-1 data. |

### Dataset Preparation and Alterations

**1. ISOT Dataset (`ISOT_DS.csv`)**

The dataset contains the following columns:

- `title`: News headline or title.
- `text`: Corresponding news article text.
- `subject`: Article subject or category.
- `date`: Publication date, where available.
- `label`: Numeric label retained or prepared for the dataset.

The headline and article text are the primary inputs for the similarity models. Subject and date can be retained as supplementary metadata.

**Label clarification:** If the numeric labels originate from the original ISOT dataset, they indicate article authenticity categories rather than verified headline–article consistency. These labels must be independently checked or replaced with consistency labels for this project.

**2. FNC-1 Binary Dataset (`FNC1_binary.csv`)**

The original Fake News Challenge (FNC-1) dataset contains stance categories such as `agree`, `disagree`, `discuss`, and `unrelated`.

The binary version is intended to support experiments with two classes.

The exact mapping from the original stance categories to `0` and `1` must be documented according to the preprocessing code. Stance classification is not identical to headline-consistency detection.

**3. FNC-1 Merged Dataset (`FNC1_merged.csv`)**

This file contains a merged version of FNC-1 data intended for consolidated preprocessing and experimentation. The exact source tables, merge keys, and filtering operations depend on the preprocessing procedure.

**4. FNC-1 Binary Analysis Dataset (`FNC1_binary_analysis.csv`)**

This file is intended to support inspection and analysis of the prepared binary dataset, such as checking class distributions, labels, duplicates, and missing values. Its exact differences from `FNC1_binary.csv` should be confirmed from the processing code.

### Important Dataset Notes

- Verify the meaning of every label before using it as a ground-truth consistency label.
- Do not assume that article authenticity labels indicate whether a headline matches its article.
- Document the binary-label mapping used for FNC-1.
- Check for duplicate articles and overlapping examples across dataset splits.
- Keep modified versions of an original article in the same data split to prevent leakage.
- Respect the original datasets' licenses and redistribution conditions.

## Headline Modification Strategy

Deliberately modified headlines will be used to test the ability of the models to identify subtle inconsistencies.

| Modification | Example |
|---|---|
| Negation | `The proposal was approved` → `The proposal was not approved` |
| Entity substitution | `India announces a policy` → `China announces a policy` |
| Numerical modification | `Sales increased by 10%` → `Sales increased by 90%` |
| Event reversal | `Team wins the match` → `Team loses the match` |
| Temporal modification | `Company plans a launch next year` → `Company launches it this year` |
| Context reversal | `Prices increased` → `Prices decreased` |
| Exaggeration | `Potential treatment discovered` → `Guaranteed cure discovered` |

These examples describe the intended modification strategy. Each generated headline must be checked against its corresponding article and assigned a reliable ground-truth label.

## Approaches Compared

### 1. TF-IDF + Cosine Similarity

TF-IDF (Term Frequency–Inverse Document Frequency) converts text into numerical vectors based on word-frequency statistics.

Cosine similarity measures the lexical overlap between the headline and article vectors.

**Advantages**
- Computationally inexpensive.
- Easy to implement and interpret.
- Provides a traditional baseline.

**Limitations**
- Limited understanding of synonyms and paraphrases.
- Sensitive to vocabulary differences.
- May assign high similarity to texts that share words but contradict each other.

### 2. Sentence Embeddings + Cosine Similarity

This approach uses a pretrained Sentence Transformer to convert headlines and article text into dense numerical vectors.

**Recommended model:** `sentence-transformers/all-MiniLM-L6-v2`

Cosine similarity is calculated between the resulting embeddings to estimate semantic relatedness.

**Advantages**
- Captures semantic relationships beyond exact word overlap.
- Can better handle paraphrasing and related expressions.
- Provides a modern embedding-based baseline.

**Limitations**
- Requires additional computational resources.
- High semantic similarity does not guarantee factual consistency.
- May miss subtle changes to numbers, entities, negation, or event status.

## Evaluation Metrics

Both approaches will be evaluated using the same held-out test examples and ground-truth labels.

- **Accuracy:** Overall proportion of correct predictions.
- **Precision:** Proportion of predicted positive cases that are actually positive.
- **Recall:** Proportion of actual positive cases correctly identified.
- **Macro-F1:** Average F1-score across both classes, giving each class equal importance.
- **ROC-AUC:** Measures the ability to rank positive examples above negative examples across different thresholds.

**Note:** ROC-AUC should be calculated using continuous similarity scores with a consistent score direction, rather than only binary class predictions.

## Project Flowcharts

### Project Workflow

![Project Flowchart](project-flowchart.png)

### Model Comparison Workflow

![Comparison Flowchart](comparison-flowchart.png)

## Technology Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- Sentence Transformers
- Matplotlib
- Jupyter Notebook

## How to Run the Project

### Step 1: Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd ai-headline-consistency-detection
```

Replace the placeholder with your actual GitHub repository URL.

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 3: Download the Datasets

Download the required CSV files from the [Google Drive dataset folder](https://drive.google.com/drive/folders/1ppKWCanMYHJ7gOKSAB0nVhZPE5op66qC?usp=sharing).

Create a `data` folder in the repository and place the downloaded files inside it.

The directory should look like this:

```text
data/
├── ISOT_DS.csv
├── FNC1_binary.csv
├── FNC1_binary_analysis.csv
└── FNC1_merged.csv
```

Update the paths in the notebook if your filenames or directory structure differ.

### Step 4: Run the Notebook

Open and execute:

```text
notebooks/headline_consistency_analysis.ipynb
```

The notebook will implement the similarity approaches, perform the evaluation, and generate the comparison results.

## Repository Structure

```text
ai-headline-consistency-detection/
│
├── data/                         # Downloaded datasets (not necessarily committed)
│
├── notebooks/
│   └── headline_consistency_analysis.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── tfidf_similarity.py
│   ├── embedding_similarity.py
│   └── evaluation.py
│
├── results/
│   ├── metrics/
│   └── visualizations/
│
├── project-flowchart.png
├── comparison-flowchart.png
├── requirements.txt
└── README.md
```

The structure is illustrative. Update it to match the files actually present in the repository.

## Experimental Procedure

1. Download and inspect the datasets.
2. Validate columns, missing values, duplicates, and label distributions.
3. Define and verify the binary-label mapping for each dataset.
4. Prepare or verify headline-consistency labels.
5. Split original articles into training, validation, and test sets before generating modified headlines.
6. Keep modified variants of each original article in the same split.
7. Calculate TF-IDF vectors and cosine similarity.
8. Generate sentence embeddings using `all-MiniLM-L6-v2` and calculate cosine similarity.
9. Select classification thresholds using validation data.
10. Evaluate both approaches on the same test examples.
11. Compare the five evaluation metrics and inspect incorrect predictions.

## Expected Outcomes

The project aims to compare:

- Lexical similarity using TF-IDF.
- Semantic similarity using sentence embeddings.
- Performance on original headline–article pairs.
- Robustness against deliberately modified headlines.
- Differences in Accuracy, Precision, Recall, Macro-F1, and ROC-AUC.
- Computational requirements and common prediction errors.

Actual results will be reported after the experiments have been completed.

## Limitations

This project evaluates headline–article consistency rather than independently verifying the truthfulness of a news article.

A headline can accurately represent a false claim made in an article and still be consistent with that article. Similarity scores alone cannot reliably establish factual entailment or contradiction.

The quality of the results depends on reliable labels, realistic headline modifications, and leakage-free evaluation.

## Academic Context

**Assignment 14:** Misleading or clickbait headlines are common on news and social-media platforms. This project compares a traditional text-similarity approach with a modern embedding-based approach to determine whether a headline is semantically consistent with its corresponding news text.
