# Chronicle — Real‑time Event Clustering and Timeline Builder (MVP)

Python MVP that ingests live text (Hacker News), deduplicates near‑duplicates, embeds, clusters into events, and exposes a FastAPI API.

## Quick start

```bash
# 1) Create venv and install
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2) Start the collector in one shell (pulls HN every 60s)
python apps/collector/run.py

# 3) In another shell, start the API
uvicorn apps.api.main:app --reload

# 4) Open docs
# http://127.0.0.1:8000/docs
```

## Endpoints

- `GET /events` — current clusters with summaries
- `GET /events/{cluster_id}` — one cluster with items
- `GET /health` — health check

## Notes

- Storage is SQLite at `data/chronicle.db` for portability.
- Embeddings prefer Sentence‑Transformers. Fallback to TF‑IDF if the model is not available.
- Clustering uses HDBSCAN when available; fallback to Agglomerative with cosine threshold.

## Seed source
- Hacker News (titles + article text when fetchable).
