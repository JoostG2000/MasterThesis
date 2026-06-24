# The role of context in forward-looking disclosure

> **Note**
> This is the public version of the thesis repository. The official assessed thesis remains the version submitted through the university's thesis submission system. This repository is shared for transparency and educational purposes and excludes all licensed WRDS-derived data, including raw data, processed datasets, transcript text, model outputs, and intermediate files. Any later changes to the code, notebooks, or documentation should be interpreted as independent post-submission refinements.

**The role of context in forward-looking disclosure: Evidence from earnings conference calls**

This repository contains the notebook-based empirical workflow for a master thesis on forward-looking disclosure in earnings conference calls. The project compares traditional dictionary-based measurement of forward-looking information with large language model-based measures that attempt to capture richer contextual properties of management disclosure.

The analysis is built around earnings conference call Q&A sections, abnormal stock returns around the call date, and a set of disclosure measures covering forward-looking intensity, specificity, economic substance, tone, certainty, and qualitative disclosure categories.

## Research question

> Does a large language model-based forward-looking information measure provide incremental explanatory power for abnormal stock returns around earnings conference calls relative to a traditional dictionary-based measure?

## Project summary

Forward-looking disclosure is often measured with dictionary methods that classify sentences as forward-looking when they contain cue words such as *will*, *expect*, *anticipate*, or *forecast*. These approaches are transparent and relatively easy to reproduce, but they can miss the context in which forward-looking language appears. A sentence can be future-oriented while still being vague, boilerplate, uncertain, economically empty, or disconnected from information that investors can price.

This thesis investigates whether LLM-based document-level measures add useful information beyond that traditional dictionary approach. The notebooks construct a sample of S&P 500 earnings conference call Q&A sections, generate dictionary and LLM disclosure measures, merge them with market and accounting controls, calculate cumulative abnormal returns, and estimate regression specifications that compare the explanatory power of alternative disclosure measures.

At a high level, the empirical workflow covers:

* retrieving S&P 500 constituent, transcript, CRSP, and accounting data through licensed data sources;
* preparing earnings call Q&A text and event-date inputs;
* calculating dictionary-based forward-looking information;
* generating LLM-based scores with local or OpenAI-compatible model endpoints;
* validating LLM scores against manually reviewed observations and alternative model settings;
* merging disclosure scores with returns, controls, and event-study outputs;
* running descriptive statistics, multicollinearity diagnostics, and panel regression specifications.

## Public repository scope

This public repository is intended to document the structure and logic of the thesis workflow. It is not a self-contained replication package because the underlying data cannot be redistributed.

Included:

* Jupyter notebooks for data retrieval, transformation, measurement, validation, and regression analysis;
* prompt and parsing logic used for LLM-based disclosure scoring;
* dictionary-based forward-looking information logic;
* regression-building notebooks and diagnostics;
* a minimal Python dependency list in `requirements.txt`.

Excluded:

* raw WRDS downloads;
* CRSP, Compustat, IBES, or Capital IQ data;
* earnings conference call transcript text;
* cleaned or merged datasets derived from licensed data;
* LLM output files generated from licensed transcript text;
* intermediate data files, benchmark outputs, private credentials, and access tokens.

The `.gitignore` file excludes the local `data/` and `benchmarks/` directories. Those directories may exist in a private working copy, but they are intentionally not part of the public repository.

## Repository structure

```text
.
|-- notebooks/
|   |-- benchmarks and tokens/
|   |   |-- tokens.ipynb
|   |   |-- llm_speed _reasoning.ipynb
|   |   `-- llm_speed _reasoning_2step.ipynb
|   |-- control variable/
|   |   |-- control_vars.ipynb
|   |   `-- word_count.ipynb
|   |-- data retrieval/
|   |   |-- sp500cons.ipynb
|   |   |-- stock_data.ipynb
|   |   `-- wrds_transcripts.ipynb
|   |-- methods/
|   |   |-- dictionary.ipynb
|   |   `-- llm_local.ipynb
|   |-- regressions & analysis/
|   |   |-- descriptive_stats_llama.ipynb
|   |   |-- descriptive_stats_qwen.ipynb
|   |   |-- regression builder.ipynb
|   |   |-- regression builder_contextual.ipynb
|   |   |-- regression builder_contextual_alt_tone_regression_.ipynb
|   |   |-- regression builder_dictfirst.ipynb
|   |   `-- VIF.IPYNB
|   `-- transformation/
|       |-- final_datasets.ipynb
|       |-- output_merging_llama.ipynb
|       |-- output_merging_qwen.ipynb
|       |-- output_sheet copy.ipynb
|       `-- transcript_txt_saver.ipynb
|
|-- validation/
|   |-- validation.ipynb
|   |-- validation_2_step.ipynb
|   `-- validation_all.ipynb
|
|-- .gitignore
|-- README.md
`-- requirements.txt
```

Expected local-only working directories:

```text
data/
|-- raw/                         # WRDS downloads, transcript text, event-study inputs
|-- processed/                   # constructed measures, merged datasets, regression outputs
|-- processed/regressions/       # regression result tables
|-- processed/regression_tables_latex/
|-- output/final/                # completed LLM batch outputs
|-- output/partial/              # checkpointed LLM batch outputs
|-- samples/                     # validation and sampled observations
|-- batches/                     # optional batch partitions
`-- other/                       # supporting local files

benchmarks/
`-- final/                       # local model timing and benchmark outputs
```

## Methodology

### 1. Data retrieval

The retrieval notebooks collect and align the core data sources needed for the empirical sample.

* `notebooks/data retrieval/sp500cons.ipynb` builds S&P 500 membership and link tables.
* `notebooks/data retrieval/wrds_transcripts.ipynb` retrieves earnings conference call metadata and Q&A transcript text.
* `notebooks/data retrieval/stock_data.ipynb` retrieves CRSP market data used for return calculations and event-study inputs.

These notebooks require authorized access to WRDS and the relevant licensed databases. They are not expected to run in a public clone without credentials and institutional permissions.

### 2. Transcript preparation

The thesis focuses on earnings conference call Q&A sections because they contain interactive exchanges between managers and analysts. The transformation notebooks prepare transcript-level observations and save text or merged outputs for later scoring.

Relevant notebooks:

* `notebooks/transformation/transcript_txt_saver.ipynb`
* `notebooks/benchmarks and tokens/tokens.ipynb`

The token notebook checks document lengths against model context limits and creates tokenized transcript outputs used to identify observations that may need separate handling.

### 3. Dictionary-based measurement

`notebooks/methods/dictionary.ipynb` constructs a traditional forward-looking information measure using rule-based sentence classification and forward-looking cue terms. This serves as the transparent baseline against which LLM-based disclosure measures are compared.

### 4. LLM-based disclosure scoring

`notebooks/methods/llm_local.ipynb` contains the main LLM scoring workflow. It uses an OpenAI-compatible client interface so that local inference servers can be used with instruction-tuned open-weight models.

The LLM scoring workflow produces document-level measures such as:

* **Forward-looking intensity** - how strongly the Q&A section discusses future-oriented information.
* **Specificity** - how concrete, detailed, and precise the forward-looking disclosure is.
* **Economic substance** - whether the disclosure contains economically meaningful information.
* **Tone** - whether the future-oriented content is positive, neutral, negative, or mixed.
* **Certainty** - how confident or committed the language is.
* **Main and secondary focus categories** - qualitative labels describing the dominant content of the disclosure.
* **Managerial horizon and outlook variables** - qualitative descriptors of time horizon and expected business conditions.

The current notebooks reference Llama 3.1 8B Instruct as the main local model and Qwen-based outputs as an additional comparison or robustness path. Some validation notebooks also compare alternative model variants and prompting strategies.

### 5. Output parsing and merging

LLM outputs are generated in batches and then parsed into structured score files.

Relevant notebooks:

* `notebooks/transformation/output_merging_llama.ipynb`
* `notebooks/transformation/output_merging_qwen.ipynb`
* `notebooks/transformation/output_sheet copy.ipynb`

These notebooks merge model output files, parse score fields, align outputs to company and call identifiers, and prepare model-specific processed datasets.

### 6. Control variables

The control-variable notebooks prepare firm, market, transcript, and event-date controls.

Relevant notebooks:

* `notebooks/control variable/word_count.ipynb`
* `notebooks/control variable/control_vars.ipynb`

Typical constructed inputs include word-count controls, accounting and financial ratios, event dates, CRSP-derived variables, and merged control datasets.

### 7. Final dataset construction

`notebooks/transformation/final_datasets.ipynb` combines the core empirical inputs:

* cumulative abnormal return files;
* dictionary-based FLI output;
* parsed LLM disclosure scores;
* control variables;
* event dates and company identifiers.

The resulting processed datasets are used by the descriptive statistics, diagnostics, and regression notebooks.

### 8. Validation and robustness checks

The validation notebooks compare LLM outputs across temperatures, prompting strategies, model variants, and manually reviewed observations.

* `validation/validation.ipynb` evaluates the main one-step scoring setup across temperature settings.
* `validation/validation_2_step.ipynb` evaluates a two-step scoring approach.
* `validation/validation_all.ipynb` compares multiple model outputs on a validation sample.

These notebooks are important because several LLM-based variables, such as certainty and economic substance, involve subjective judgment. Validation helps assess whether the model outputs are stable and interpretable enough to use as empirical disclosure measures.

### 9. Regression analysis

The regression notebooks estimate the main empirical specifications and robustness checks.

* `notebooks/regressions & analysis/descriptive_stats_llama.ipynb`
* `notebooks/regressions & analysis/descriptive_stats_qwen.ipynb`
* `notebooks/regressions & analysis/regression builder.ipynb`
* `notebooks/regressions & analysis/regression builder_contextual.ipynb`
* `notebooks/regressions & analysis/regression builder_contextual_alt_tone_regression_.ipynb`
* `notebooks/regressions & analysis/regression builder_dictfirst.ipynb`
* `notebooks/regressions & analysis/VIF.IPYNB`

The regression builders compare dictionary-first, LLM-first, contextual, and alternative tone specifications. The VIF notebook produces multicollinearity diagnostics, including numerical VIF, categorical dummy VIF, mixed VIF, and grouped categorical diagnostics.

## Typical execution order

There is no single pipeline script. The project is organized as a set of staged notebooks. A typical private reproduction workflow is:

1. Install dependencies and configure WRDS credentials.
2. Run `notebooks/data retrieval/sp500cons.ipynb`.
3. Run `notebooks/data retrieval/wrds_transcripts.ipynb`.
4. Run `notebooks/data retrieval/stock_data.ipynb`.
5. Run transcript preparation and token checks.
6. Run `notebooks/methods/dictionary.ipynb`.
7. Run `notebooks/methods/llm_local.ipynb` with the intended local model endpoint.
8. Merge and parse LLM outputs with the transformation notebooks.
9. Construct word-count and financial controls.
10. Build final model-specific datasets with `final_datasets.ipynb`.
11. Run validation notebooks.
12. Run descriptive statistics, VIF diagnostics, and regression notebooks.

Because some notebooks depend on local files produced by earlier notebooks, check the expected input filenames at the top of each notebook before executing it.

## Environment setup

Create a Python environment and install the listed dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

The dependency file currently includes packages for WRDS access, data handling, tokenization, LLM calls, model utilities, plotting, and regression analysis:

* `wrds`
* `pyarrow`
* `tiktoken`
* `tqdm`
* `matplotlib`
* `pandas`
* `nltk`
* `openai`
* `openpyxl`
* `transformers`
* `huggingface_hub`
* `statsmodels`
* `jinja2`

Depending on your local Jupyter setup, you may also need notebook tooling such as `jupyter` or `ipykernel`.

## LLM setup

The LLM notebooks use the `openai` Python client against an OpenAI-compatible API endpoint. In the thesis workflow, this pattern was used for local inference with open-weight models.

Before running the LLM notebooks, make sure that:

* the local model server is running;
* the notebook `base_url` points to the server endpoint;
* the selected model name matches the model loaded by the server;
* the API key handling matches the requirements of the local server;
* context-window limits have been checked with the token notebooks;
* batch output folders exist under `data/output/final/` and `data/output/partial/`.

The public repository does not include generated LLM outputs because those outputs are derived from licensed transcript text.

## Reproducibility notes

This repository is useful for understanding and auditing the empirical design, but a public clone will not run end-to-end without the missing licensed inputs. To reproduce the workflow privately, you need:

* Python 3.x and the packages in `requirements.txt`;
* Jupyter or another notebook runner;
* authorized WRDS access;
* access to CRSP, Compustat, IBES, Capital IQ transcript data, or equivalent licensed sources used in the thesis;
* local `data/` folders following the expected layout;
* an OpenAI-compatible LLM endpoint or equivalent scoring outputs;
* enough compute and storage for transcript processing and LLM batch scoring.

The notebooks use project-root discovery based on the presence of the local `data/` directory. If the notebooks cannot find paths correctly, confirm that the `data/` folder exists at the repository root.

## Main limitations

Important limitations of the project and repository include:

* the underlying licensed data cannot be redistributed;
* document-level LLM judgments may contain measurement error;
* prompt wording, model choice, temperature, and parsing rules can affect scores;
* validation samples are necessarily smaller than the full empirical sample;
* some contextual concepts, such as certainty and economic substance, are inherently subjective;
* the notebooks reflect a thesis research workflow rather than a production-grade data pipeline.

## Acknowledgement

Wharton Research Data Services (WRDS) was used in preparing this research. This service and the data available thereon constitute valuable intellectual property and trade secrets of WRDS and/or its third-party suppliers.

## Author

**Joost Gaasbeek**

MSc Business Analytics & Management

Rotterdam School of Management, Erasmus University

## Disclaimer

This repository is shared for transparency and educational purposes. The official assessed thesis remains the version submitted through the university's thesis submission system. Any later changes to this repository, notebooks, or accompanying documents should be interpreted as independent post-submission refinements.
