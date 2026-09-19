# Phishing Email Detection: Classical ML vs BERT

An end-to-end natural language processing project comparing classical machine learning and transformer-based approaches for phishing email detection.

**Author:** João Bernardo Sousa Faria

## Overview

This project treats phishing detection as a binary text classification task. It compares the performance, interpretability, computational cost, and cross-dataset generalization of TF-IDF with Logistic Regression and a fine-tuned BERT model.

The workflow includes data cleaning, deduplication, stratified train/validation/test splits, validation-based threshold tuning, error analysis, model explainability, and cross-dataset evaluation.

## Models

- TF-IDF with Logistic Regression
- Class-weighted TF-IDF with Logistic Regression
- Fine-tuned `bert-base-uncased`
- Zero-shot natural language inference baseline

## Results

| Model | Accuracy | Phishing precision | Phishing recall | Phishing F1 |
|---|---:|---:|---:|---:|
| TF-IDF + Logistic Regression | 0.9814 | 0.9708 | 0.9797 | 0.9752 |
| TF-IDF + Logistic Regression (balanced) | 0.9814 | 0.9698 | 0.9807 | 0.9752 |
| Fine-tuned BERT | **0.9875** | **0.9847** | **0.9817** | **0.9832** |
| Zero-shot NLI (200-email subset) | 0.6200 | 0.4615 | 0.0800 | 0.1364 |

BERT achieved the best held-out performance. TF-IDF with Logistic Regression remained highly competitive while being faster and easier to interpret. Cross-dataset tests revealed a significant performance drop, showing that distribution shift remains an important deployment concern.

## Dataset

The primary dataset is [`zefang-liu/phishing-email-dataset`](https://huggingface.co/datasets/zefang-liu/phishing-email-dataset). After cleaning and deduplication, it contains 17,537 emails:

- 10,979 safe emails
- 6,558 phishing emails

The DIFraud phishing benchmark is used to evaluate cross-dataset generalization. The notebook downloads datasets through the Hugging Face `datasets` library; the datasets are not stored in this repository.

## Methodology

- Removed missing values and duplicate email text before splitting
- Used stratified 70/15/15 training, validation, and test sets
- Tuned classification thresholds on validation data using phishing F1
- Evaluated accuracy, precision, recall, F1, macro-F1, micro-F1, and weighted-F1
- Inspected Logistic Regression coefficients for global and instance-level explanations
- Analyzed misclassified emails and cross-dataset performance

## Tech Stack

Python, pandas, NumPy, scikit-learn, PyTorch, Hugging Face Datasets, Hugging Face Transformers, and Matplotlib.

## Repository Structure

```text
.
|-- BSP S6/
|   |-- BSP S6 (Code + Presentation + Declaration)/
|   |   |-- BSP_S6.ipynb
|   |   |-- BSP S6 Declaration.pdf
|   |   |-- BSP S6 Presentation.pdf
|   |   `-- BSP S6 Presentation.pptx
|   |-- BSP S6 Report.pdf
|   `-- BSP S6 Report - Seconday Language.pdf
|-- .gitignore
|-- GITHUB_PROFILE_SNIPPET.md
|-- PUBLISH_WITH_GIT_BASH.md
|-- README.md
`-- requirements.txt
```

## Run Locally

```bash
git clone https://github.com/joburn-git/phishing-email-detection.git
cd phishing-email-detection

python -m venv .venv
source .venv/Scripts/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook "BSP S6/BSP S6 (Code + Presentation + Declaration)/BSP_S6.ipynb"
```

A CUDA-capable GPU is recommended for BERT fine-tuning. The classical models can run on a CPU. Running the notebook downloads public datasets and pretrained model weights from Hugging Face.

## Limitations

- BERT inputs were truncated to 128 tokens.
- Only one fine-tuned transformer architecture was evaluated.
- Public phishing datasets may contain noisy labels or dataset-specific patterns.
- The zero-shot baseline used a smaller test subset because of its computational cost.

