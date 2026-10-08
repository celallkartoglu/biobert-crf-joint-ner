# Joint Multi-Corpus Fine-Tuning for Biomedical Disease NER: A BioBERT–CRF Framework with Interpretability Analysis

## 📌 Project Overview
This repository provides the code for a controlled empirical study of **joint multi-corpus
fine-tuning** of a **BioBERT–CRF** sequence-labeling model for **biomedical disease named
entity recognition (NER)**, evaluated on the **NCBI-Disease** and **BC5CDR-Disease** corpora.

The study examines two empirical questions within an established encoder–CRF framework:
- whether joint fine-tuning on both corpora improves performance over single-corpus training
  on each corpus's held-out test set, and
- whether a fixed natural-language task-instruction prefix provides an additional benefit.

Model predictions are additionally examined with **Integrated Gradients** on a single
qualitative example. All experiments are run across three random seeds with paired
significance testing.

## 🧠 What Does This Project Do?
- Converts PubTator annotations (NCBI-Disease, BC5CDR-Disease) into BIO token labels (B-Disease, I-Disease, O)
- Fine-tunes a BioBERT encoder with a CRF decoder for disease NER
- Compares joint multi-corpus training vs. single-corpus training (and cross-corpus transfer)
- Benchmarks four encoder backbones under a shared protocol: BioBERT, PubMedBERT, BioMedRoBERTa, SciBERT (all with a CRF head)
- Ablates a fixed task-instruction prefix ("Extract disease entities from the following biomedical text.")
- Evaluates with entity-level exact-match F1, reported as mean ± SD over three seeds (42, 123, 2024)
- Reports paired two-sided t-tests between configurations
- Applies Integrated Gradients for token-level attribution on an example input

## 📊 Datasets
| Corpus | Train | Dev | Test |
|---|---|---|---|
| NCBI-Disease | 593 | 100 | 100 |
| BC5CDR-Disease | 500 | 500 | 500 |

Only **disease** annotations are used (chemical annotations in BC5CDR are excluded), giving a
shared label space of B-Disease / I-Disease / O.

### Third-Party Data Sources
- NCBI Disease Corpus — https://www.ncbi.nlm.nih.gov/CBBresearch/Dogan/DISEASE/
- BC5CDR (BioCreative V CDR) — https://biocreative.bioinformatics.udel.edu/resources/corpora/biocreative-v-cdr-corpus/

These datasets are used strictly for research and academic purposes.

## ⚙️ Model
- **Encoder:** BioBERT-base (`dmis-lab/biobert-base-cased-v1.1`); baselines use PubMedBERT, BioMedRoBERTa, SciBERT
- **Head:** dropout (0.1) → linear emission layer → CRF
- **Parameters:** ~108.31 M (all fine-tuned)

## ▶️ Usage
### 1. Install dependencies
```bash
pip install torch==2.11.0 transformers==5.18.0 captum==0.9.0 pandas scikit-learn numpy matplotlib
```

### 2. Data preparation
- Download the corpora from the links above
- Run the PubTator → BIO conversion scripts

### 3. Training
- Fine-tune BioBERT–CRF with joint and single-corpus configurations
- Each configuration is trained across three seeds (42, 123, 2024) with early stopping
  (patience 5 on development-set F1)

### 4. Evaluation
- Entity-level exact-match F1 on each corpus's held-out test set
- Paired t-tests between configurations

### 5. Interpretability
- Run the Integrated Gradients script to produce token-level attributions for an example

## 🖥️ Computational Environment
- Platform: Google Colab
- GPU: NVIDIA A100 (40 GB)
- Python 3.10+, PyTorch 2.11.0 (CUDA 13.0), Hugging Face Transformers 5.18.0, Captum 0.9.0
- Hyperparameters: max sequence length 512, batch size 4, learning rate 3e-5, AdamW,
  warm-up ratio 0.1, dropout 0.1, max 20 epochs, early-stopping patience 5, gradient clipping 1.0

## 📚 Citation
If you use this repository, please cite:

> Kartoğlu, C. Ş., Can, Ü., & Aslan, S. *Joint Multi-Corpus Fine-Tuning for Biomedical Disease
> Named Entity Recognition: A BioBERT–CRF Framework with Interpretability Analysis.*

## 📜 License
MIT License.

## ✉️ Contact
Celal Şamil Kartoğlu — Malatya Turgut Özal University, Department of Software Engineering