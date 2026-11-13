# Self-Correcting AI Research Assistant

An autonomous research agent built with **LangGraph, LangChain, Ollama, and Qwen3**.

The agent researches a user-provided question, retrieves web sources, selects relevant evidence, generates a structured report, checks citations and claims, and automatically retries research when claims are not sufficiently supported.

## Features

- LLM-based search-query planning
- Web search and webpage content extraction
- TF-IDF evidence passage retrieval
- Structured research reports using Pydantic
- Source quotation verification
- LLM-based claim support assessment
- Adaptive research and bounded self-correction
- Failure handling and evaluation metrics

## Architecture

```text
User question
    ↓
Plan search queries
    ↓
Search web → Read webpages
    ↓
Extract and rank evidence
    ↓
Generate structured report
    ↓
Validate quotations
    ↓
Assess factual claims
    ├── Supported → Complete
    └── Unsupported or uncertain
          ↓
       Generate improved queries
          ↓
       Research again (maximum 2 retries)
          ↓
       Revalidate
```

## Technology Stack

Python · LangGraph · LangChain · Ollama · Qwen3 4B · Pydantic · scikit-learn · BeautifulSoup · DDGS · Google Colab

## Setup and run (Google Colab)

[Open the notebook in Colab](https://colab.research.google.com/github/MoneebAliKarrar/AI-Research-Assistant/blob/main/01_research_assistant_milestone3.ipynb).

1. Connect a Python 3 runtime. Choose **Runtime → Change runtime type → T4 GPU** (or another available NVIDIA GPU). The recorded run used a T4; GPU availability and runtime lifetime are [not guaranteed by Colab](https://research.google.com/colaboratory/faq.html). Internet access is needed for installation, model downloads, search, and page retrieval. No paid API key is required.
2. Before running the notebook's imports, add a setup cell and run:

   ```python
   !git clone https://github.com/MoneebAliKarrar/AI-Research-Assistant.git
   %pip install -r /content/AI-Research-Assistant/requirements.txt
   ```

   Opening a GitHub notebook in Colab does not clone its other files. The requirements cover every third-party import, including pandas for evaluation and scikit-learn for retrieval. The existing inline installation cells may also run; they install only a subset and use upgrades. Requirements are intentionally unpinned because the saved notebook does not record a complete tested package environment; this is not a reproducibility lockfile.
3. Run the notebook cells in order through graph compilation. The setup cells check `nvidia-smi`, install `zstd` and [Ollama for Linux](https://docs.ollama.com/linux), start `ollama serve` as a background subprocess, poll `http://localhost:11434/api/tags`, then pull [`qwen3:4b-instruct`](https://ollama.com/library/qwen3:4b-instruct). The model download is about 2.5 GB. A version-check connection warning before the server starts is expected. Colab does not run systemd, so use the notebook's subprocess cell to start the server. The server and Python notebook must run in the **same runtime**; a server on your laptop is not Colab's localhost.
4. Run the cell under **“Make the research agent work with any topic”** and enter a question when prompted. Empty input uses the built-in RAG-versus-fine-tuning question. Inspect the printed status, report JSON (up to two findings with source URLs and passages), limitations, quotation checks, and model claim verdicts. To ask another question, rerun this input cell.
5. Run `test_graceful_failures()` in the next cell for deterministic early-exit checks; these checks do not call Ollama or the internet.

**Evaluation is optional but currently executes when its cell is run.** Despite the “Uncomment” comment, `evaluate_research_agent()` is already uncommented. Stop before the final evaluation cell unless you want five additional live research runs. That cell displays per-case metrics and writes `research_eval_m3.csv` in the runtime's current directory; download it from Colab's Files panel before the runtime is discarded. **Run all** also prompts for a question and runs this benchmark.

If startup fails, inspect `/tmp/ollama.log`. If port 11434 is already in use, check whether the existing Ollama server is healthy before starting another. If the model is missing, rerun `!ollama pull qwen3:4b-instruct` after the readiness check. After a runtime reset, reinstall dependencies, restart Ollama, pull the model, and rerun definitions in order. Search-engine throttling and unreadable webpages can produce an explicit insufficient-evidence result.

## Repository contents

- `01_research_assistant_milestone3.ipynb` — Colab setup, agent, interactive run, smoke checks, and evaluation harness.
- `requirements.txt` — direct Python dependencies derived from notebook imports; Ollama itself is installed separately.
- `README.md` — setup, architecture, recorded benchmark, and limitations.

## Evaluation

An initial five-question benchmark covered language, retrieval-augmented generation, machine learning, transfer learning, and semantic search.

| Metric | Result |
|---|---|
| Completed questions | 5/5 |
| Claims rated supported by Qwen | 10/10 |
| Reports passing quotation checks | 5/5 |
| Unexpected crashes | 0 |
| Median execution time | 18.55 seconds |

These are prototype benchmark results, not independently established factual accuracy. They are preserved from the notebook's saved outputs, not a fresh rerun.

| Case | Time (seconds) | Findings | Quotation valid | Claims rated supported | Expected-term recall |
|---|---:|---:|---|---|---:|
| Latin prefix omni- | 13.69 | 2 | Yes | 2/2 | 1.0 |
| RAG vs fine-tuning | 39.95 | 2 | Yes | 2/2 | 0.5 |
| Supervised vs unsupervised learning | 16.06 | 2 | Yes | 2/2 | 1.0 |
| Transfer learning | 18.55 | 2 | Yes | 2/2 | 0.5 |
| Embeddings for semantic search | 67.40 | 2 | Yes | 2/2 | 0.0 |

All five cases had status `completed` and zero quotation revisions. Expected-term recall is a simple keyword coverage diagnostic, not accuracy: its gaps show that supported claims and valid quotations do not guarantee complete answers. Timing depends on hardware, model state, websites, and search availability. Saved execution outputs were cleared for readability; the evaluation code remains intact.

## Limitations

- Evaluation covers only five questions.
- Claim validation uses the same underlying LLM family as report generation.
- Web search and page extraction depend on external websites.
- Answer completeness can vary even when citations are valid.
- The research report currently contains at most two findings.
- Google Colab requires an active runtime and Ollama setup.

## Future Improvements

Potential improvements include independent evaluation datasets, semantic retrieval using embeddings, better answer completeness, and a standalone interface.

## Project Status

**Version 1.0 — Research prototype**

An educational portfolio project demonstrating practical agent orchestration, retrieval, validation, recovery workflows, and evaluation.
