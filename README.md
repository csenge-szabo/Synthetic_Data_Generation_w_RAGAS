# Stop Guessing, Start Measuring

### Evaluating RAG with synthetic test data — a live demo with RAGAS

Companion repo for the talk "Stop Guessing, Start Measuring. Evaluating RAG Systems with Synthetic Data" presented online at the [WeAreDevelopers World Congress](https://www.wearedevelopers.com/world-congress) (Berlin, 2026). The notebook in this repo
builds a minimal RAG setup over the public Hugging Face docs, then uses the open-source
[RAGAS](https://docs.ragas.io) library to **generate a synthetic test set** from those docs and automatically **evaluate**
the RAG system against it.

It walks through, end to end:

1. A tiny RAG system over the openly accessible [`m-ric/huggingface_doc`](https://huggingface.co/datasets/m-ric/huggingface_doc) corpus.
2. Synthetic test-set generation via RAGAS's **knowledge-graph** pipeline.
3. Running the RAG over the generated test set.
4. Evaluation with five RAG metrics through the current `ragas.metrics.collections` API: **Context Precision, Context Recall, Faithfulness, Answer Relevancy, Noise Sensitivity**.
5. Reading the scores to localize failures to *retrieval* vs *generation*.

> Written against **ragas 0.4.3** — the current `collections` metrics API + `llm_factory` +
> `@experiment`.

## Prerequisites

- [`uv`](https://docs.astral.sh/uv/) ([install](https://docs.astral.sh/uv/getting-started/installation/))
- Python 3.10–3.12 — you don't need this installed already; `uv` will fetch it for you (the pinned stack does not yet run on 3.13+).
- An **OpenAI API key** — the demo uses OpenAI for embeddings, RAG generation (`gpt-4o-mini`), and the LLM-as-judge.

## Setup

```bash
# 1. Create a virtual environment (uv downloads Python 3.12 if you don't have it)
uv venv --python 3.12 && source .venv/bin/activate

# 2. Install dependencies
uv pip install -e .

# 3. Add your API key
cp .env.example .env      
# then edit .env and paste your OpenAI key
```

The notebook calls `load_dotenv()`, so it picks up `OPENAI_API_KEY` from `.env` automatically. If
no key is found there, it falls back to prompting you for one at runtime.

## Run

```bash
uv run jupyter lab        # or: uv run jupyter notebook
```

Open `RAGAS_demo_notebook.ipynb` and run it top to bottom.

## A note on synthetic data

Synthetic data generation is only the first step in the evaluation process. It is a bootstrap, not a final verdict. LLM-as-judge labels aren't equivalent to ground truth, generated
questions can be shallow, and metrics are only proxies. Including a human-in-the-loop for evaluating any RAG system is strongly encouraged.
