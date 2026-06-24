# The role of context in forward-looking disclosure

> **Note**
> This is the public version of the thesis repository. The official assessed thesis remains the version submitted through the university’s thesis submission system. This repository is shared for transparency and educational purposes and excludes all licensed WRDS-derived data, including raw data, processed datasets, transcript text, and intermediate files. Any later changes to the code, notebooks, or documentation should be interpreted as independent post-submission refinements.


**The role of context in forward-looking disclosure: Evidence from earnings conference calls**

The thesis examines whether large language model-based measures of forward-looking information provide incremental explanatory power for abnormal stock returns around earnings conference calls, relative to a traditional dictionary-based measure.

## Overview

Forward-looking disclosure is commonly measured using dictionary-based methods that identify forward-looking sentences through cue words such as *will*, *expect*, *anticipate*, and *forecast*. While this approach is transparent and replicable, it may not fully capture the context, specificity, tone, certainty, or economic meaning of what is being said.

This project explores whether LLM-based document-level measures can add explanatory value by capturing contextual characteristics of forward-looking information in earnings conference call Q&A sections.

The analysis focuses on:

* dictionary-based forward-looking information;
* LLM-based forward-looking intensity;
* specificity;
* economic substance;
* tone;
* certainty;
* contextualized FLI composite measures;
* cumulative abnormal returns around earnings conference calls.

## Research question

> Does a large language model-based forward-looking information measure provide incremental explanatory power for abnormal stock returns around earnings conference calls relative to a traditional dictionary-based measure?

## Data availability

The data used in this project were obtained through WRDS and related licensed data providers. This includes financial market data, accounting data, and earnings conference call transcript data.

Due to licensing restrictions, **raw data, processed datasets, transcript text, and intermediate data files are not included in this repository**.

The repository is intended to make the code structure, empirical workflow, and methodological choices transparent. Users with authorized WRDS access may reproduce the data construction steps using their own credentials and institutional permissions.

The relevant data folders and file types are excluded through `.gitignore`.

## Repository structure

```text
.
├── notebooks/
│   ├── benchmarks and tokens/
│   │   └── Notebooks related to token counts, model benchmarking, and context-window checks
│   ├── control variable/
│   │   └── Notebooks for constructing and preparing control variables
│   ├── data retrieval/
│   │   └── Notebooks for retrieving and organizing source data
│   ├── methods/
│   │   └── Notebooks related to measurement design and methodological implementation
│   ├── regressions & analysis/
│   │   └── Notebooks for regression analysis, robustness checks, tables, and interpretation
│   └── transformation/
│       └── Notebooks for cleaning, transforming, and merging datasets
│
├── validation/
│   ├── validation.ipynb
│   ├── validation_2_step.ipynb
│   └── validation_all.ipynb
│
├── .gitignore
├── README.md
└── requirements.txt
```

## Methodology

The empirical workflow consists of several main steps.

### 1. Transcript preparation

Earnings conference call Q&A sections are extracted and prepared for analysis. The focus on Q&A sections allows the analysis to examine more dynamic forward-looking communication between managers and analysts.

### 2. Dictionary-based FLI measurement

A traditional forward-looking information score is constructed using forward-looking cue words and sentence-level classification.

### 3. LLM-based disclosure measurement

Earnings call Q&A sections are evaluated using instruction-tuned language models. The models produce document-level scores for forward-looking intensity and several contextual disclosure characteristics.

The main LLM-based variables are:

* **Forward-looking intensity**: the extent to which the Q&A section discusses future-oriented information.
* **Specificity**: the degree of precision, concreteness, and detail in forward-looking statements.
* **Economic substance**: the extent to which the disclosure contains economically meaningful information.
* **Tone**: the sentiment of the forward-looking content.
* **Certainty**: the degree of confidence or commitment expressed in forward-looking statements.

### 4. Abnormal return calculation

Cumulative abnormal returns are calculated around earnings conference call dates using an event-study framework.

### 5. Regression analysis

The explanatory power of dictionary-based and LLM-based measures is tested using panel regressions with financial controls and fixed effects.

### 6. Validation and robustness checks

The validation notebooks compare model outputs, manual validation scores, and alternative prompting strategies. Additional robustness checks evaluate alternative model specifications, event windows, fixed-effect structures, and composite disclosure measures.

## Reproducing the analysis

To run the notebooks, users need:

* Python 3.x;
* the required packages listed in `requirements.txt`;
* authorized access to the required WRDS datasets;
* local copies of the licensed data;
* access to the relevant LLM environment or locally stored model outputs.

A typical setup is:

```bash
git clone <repository-url>
cd <repository-name>
pip install -r requirements.txt
```

Then run the notebooks in the relevant folders.

Because the underlying data are not publicly available, the notebooks will not run end-to-end without authorized access to the required WRDS datasets and local data files.

## Notes on data and licensing

This repository does **not** include:

* raw WRDS downloads;
* CRSP, Compustat, or Capital IQ data;
* earnings conference call transcript text;
* cleaned or merged datasets derived from licensed sources;
* intermediate data files;
* private credentials or access tokens.

The repository may include code, prompts, SQL/query logic, analysis notebooks, and table-generation scripts.

## Notes on LLM use

The LLM-based measures were generated using instruction-tuned open-weight models. The prompts and scoring logic are included to make the measurement approach transparent.

The LLM outputs should be interpreted as computational disclosure measures rather than definitive human judgments. Manual validation and robustness checks were used to assess the consistency and usefulness of the measures.

## Limitations

This repository reflects the empirical workflow of a master thesis project. Important limitations include:

* the use of licensed financial and transcript data that cannot be redistributed;
* dependence on the selected LLM, prompt design, and scoring definitions;
* measurement uncertainty in subjective disclosure characteristics such as certainty and economic substance;
* limited ability to fully validate document-level LLM judgments against human-coded benchmarks;
* the exploratory nature of several contextual disclosure measures.

## Acknowledgement

Wharton Research Data Services (WRDS) was used in preparing this research. This service and the data available thereon constitute valuable intellectual property and trade secrets of WRDS and/or its third-party suppliers.

## Author

**Joost Gaasbeek**
MSc Business Analytics & Management
Rotterdam School of Management, Erasmus University

## Disclaimer

This repository is shared for transparency and educational purposes. The official assessed thesis remains the version submitted through the university’s thesis submission system. Any later changes to this repository, notebooks, or accompanying documents should be interpreted as independent post-submission refinements.
