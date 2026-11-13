# AI-Research-Assistant
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

## Evaluation

An initial five-question benchmark covered language, retrieval-augmented generation, machine learning, transfer learning, and semantic search.

| Metric | Result |
|---|---|
| Completed questions | 5/5 |
| Claims rated supported by Qwen | 10/10 |
| Reports passing quotation checks | 5/5 |
| Unexpected crashes | 0 |
| Median execution time | 18.55 seconds |

These are prototype benchmark results, not independently established factual accuracy.

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
