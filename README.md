# RAG: Baseline vs Re-Ranking

A small question-answering system over one long PDF (the IFAB *Laws of the Game 2024/25*), comparing two
retrieval pipelines over the same chunks:

1. **Baseline** — dense Bi-Encoder retrieval, top 10 by cosine similarity, top 3 sent to the LLM.
2. **Re-ranking** — the same top 10 re-scored by a Cross-Encoder, re-sorted, top 3 sent to the LLM.

Both pipelines use the same LLM and the same prompt, so any difference in the answers comes from which 3
chunks were selected.

## How to run

1. Get a Gemini API key and put it in a file called `.env` next to the notebook: `GEMINI_API_KEY=...` (the file is gitignored).
2. Download the PDF (see `data/SOURCE.md`) into `data/`.
3. `python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`
4. Open `rag_reranking.ipynb` and **Restart and run all**.

Only slow or hand-made things are saved to disk. Cheap steps (text extraction, embeddings, retrieval) are
recomputed on every run, so a fresh kernel always works.

## Layout

| Path | What it is |
|---|---|
| `rag_reranking.ipynb` | The whole pipeline, in stages (setup, corpus, chunks, index, baseline, re-rank, analysis, latency) |
| `data/config.json` | Models, chunk size, top-k and top-n, LLM settings, which sections of the PDF to use |
| `data/chunks.jsonl` | The chunks, **frozen**: chunk ids are what the hand-labelled queries refer to |
| `data/queries.json` | The test queries with their gold chunk and gold answer (4 failure-type queries, 2 controls) |
| `data/SOURCE.md` | Where the PDF is from and how it was cleaned. The PDF itself is not committed |
| `results/` | Written by the notebook: `answers.json` (Answer 1 and 2 per query), `rank_shifts.csv`, `retrieval_metrics.csv` (Recall@3 and MRR), `latency.csv` |

## Models

| Role | Model |
|---|---|
| Answer LLM | Gemini API `gemini-3.7-flash`, temperature 0 |
| Bi-Encoder | `all-MiniLM-L6-v2` (reads at most 256 tokens; its vectors have 384 numbers) |
| Cross-Encoder | `cross-encoder/ms-marco-MiniLM-L-6-v2` |

## Chunking

230 chunks, at most 160 words each, built from Laws 1–17, the notes and modifications, the VAR protocol
and the practical guidelines for match officials. Chunks never cross a Law boundary or a numbered
heading. One chunk (Law 1, field markings) is 2 tokens over the Bi-Encoder limit, so its last word is
not embedded.
