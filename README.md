# Podcast Search Engine

A course project for information retrieval that turns a large podcast transcript corpus into a searchable product. The system combines lexical retrieval, semantic vector search, hybrid ranking, and LLM-assisted result exploration in a single web app.

This system was built as part of the DD2477 Information Retrieval project.

## Team Members
- [Daniel Ruijs](https://github.com/danielruijs)
- [Pavlos Spanoudakis](https://github.com/pspanoudakis)
- [Bhaswat Raj](https://github.com/Slash10241)
- [Parniansadat Razaviour](https://github.com/parnianrazavipour)

## What This Project Does

Podcast transcripts are long, noisy, and difficult to search well with exact keywords alone. This project explores the full retrieval stack:

- lexical search for precision on exact terms,
- vector search for semantic similarity,
- hybrid search to balance precision and recall,
- metadata enrichment to make results easier to scan,
- LLM-powered tools for highlighting, summarization, feedback, and retrieval-augmented question answering.

The result is both a working demo and an evaluation platform for comparing retrieval strategies on the same dataset.

## Project Highlights

- Full-stack search experience built with Django, HTMX, and Elasticsearch.
- Three retrieval modes exposed directly in the UI: lexical, vector, and hybrid.
- LLM sidebar for asking follow-up questions over retrieved context.
- Highlight extraction, query-aware summaries, and result-quality feedback.
- Offline evaluation for ranking metrics, highlight quality, and embedding model comparisons.
- Presentation deck included in the repository for a concise project walkthrough.

## How It Works

The main app lives in `engine/` and exposes a transcript search interface with live updates. Users can switch search modes, inspect ranked results, and use the LLM panel to explore the retrieved evidence.

The backend supports:

- query parsing and mode selection,
- search execution across lexical, vector, and hybrid pipelines,
- metadata enrichment before rendering,
- highlight extraction from retrieved snippets,
- summary generation for result sets,
- feedback scoring and RAG-style question answering.

## Repository Structure

- `engine/` contains the Django backend, HTMX frontend, and LLM-assisted UI.
- `indexing/` contains the Elasticsearch indexing scripts and embedding benchmarks.
- `evaluation/` contains ranking, metric, and highlight evaluation scripts.
- `embedding eval/` contains embedding-model evaluation experiments and generated outputs.

## Quick Start

### 1. Start Elasticsearch

Install Docker, then run:

```bash
curl -fsSL https://elastic.co/start-local | sh
```

This creates a local Elasticsearch and Kibana setup and an `elastic-start-local` directory with the credentials needed for the rest of the project.

After installation:

- Elasticsearch: http://localhost:9200
- Kibana: http://localhost:5601

Test the connection with:

```bash
cd elastic-start-local/
source .env
curl $ES_LOCAL_URL -H "Authorization: ApiKey ${ES_LOCAL_API_KEY}"
```

### 2. Run the Web App

From the `engine` directory:

```bash
cd engine
uv sync
uv run python manage.py check
uv run python manage.py runserver
```

Open the app at:

http://127.0.0.1:8000/

### 3. Configure Required Environment Variables

Create `engine/web/services/.env` with:

```env
API_KEY=your_elasticsearch_api_key
METADATA_TSV_PATH=/absolute/path/to/metadata.tsv
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.5-flash-lite
GEMINI_FEEDBACK_MODEL=gemini-3.1-flash-lite-preview
```

### 4. Index the Dataset

Follow the instructions in [indexing/README.md](indexing/README.md) to load the podcast transcript corpus into Elasticsearch before using the demo end to end.

## Evaluation and Experiments

This repository includes reproducible evaluation workflows for the retrieval stack:

- ranking evaluation across lexical, vector, and hybrid retrieval,
- highlight-quality evaluation for LLM-generated snippets,
- embedding benchmark scripts,
- LLM-judged embedding retrieval experiments,
- saved metrics and result files for comparison.

Good entry points:

- [indexing/README.md](indexing/README.md)
- [evaluation/README.md](evaluation/README.md)
- [embedding eval/README.md](embedding%20eval/README.md)

## Local Elasticsearch Setup

If you want to manage the local Elasticsearch instance manually:

```bash
cd elastic-start-local
./start.sh
```

To stop it:

```bash
cd elastic-start-local
./stop.sh
```

To uninstall it completely:

```bash
cd elastic-start-local
./uninstall.sh
```
