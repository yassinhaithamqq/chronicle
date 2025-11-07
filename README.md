# Chronicle — Real‑time Event Clustering and Timeline Builder

> **Intelligent news aggregation powered by semantic embeddings, MinHash deduplication, and adaptive clustering algorithms.**

Chronicle is a production-ready event detection system that transforms noisy real-time news streams into coherent, clustered timelines. Built for scale and accuracy, it combines modern NLP techniques with robust ML pipelines to automatically discover trending topics and extract signal from noise.

## Architecture & Features

### 🧠 Advanced NLP Pipeline
- **Semantic Embeddings**: Sentence-Transformers (`all-MiniLM-L6-v2`) for high-quality vector representations
- **Intelligent Fallback**: Graceful degradation to TF-IDF when GPU resources unavailable
- **MinHash LSH Deduplication**: Sub-linear time complexity for near-duplicate detection (85% similarity threshold)
- **Extractive Summarization**: TF-IDF weighted sentence extraction for multi-document summaries

### 🔬 Adaptive Clustering
- **HDBSCAN**: Density-based clustering with automatic outlier detection and probabilistic membership scores
- **Agglomerative Fallback**: Distance-threshold clustering with cosine similarity for deterministic results
- **Dynamic Event Formation**: Minimum cluster size validation ensures signal over noise

### 🏗️ Production-Ready Design
- **Async I/O**: Non-blocking HTTP client for concurrent article fetching
- **Readability Extraction**: DOM-based content extraction with BeautifulSoup + lxml parsing
- **SQLite Storage**: Zero-config persistence with indexed queries for sub-millisecond lookups
- **FastAPI**: High-performance async REST API with automatic OpenAPI documentation
- **Docker Compose**: Single-command deployment with isolated collector and API services

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

- `GET /events` — Current event clusters ranked by size and confidence, with extractive summaries
- `GET /events/{cluster_id}` — Detailed event view with all associated documents and metadata
- `GET /health` — System health check

## Technical Implementation

### Data Pipeline
1. **Ingestion**: Async fetcher polls Hacker News API every 60s (top 60 stories)
2. **Extraction**: Readability algorithm extracts clean article text from HTML
3. **Deduplication**: MinHash LSH identifies and filters near-duplicates in O(n) time
4. **Embedding**: Documents encoded to 384-dimensional semantic vectors
5. **Clustering**: HDBSCAN groups semantically similar documents with confidence scores
6. **Summarization**: TF-IDF ranks sentences across cluster for representative summary

### Algorithm Details
- **MinHash**: 128 permutations, 4-gram shingling, 0.85 Jaccard similarity threshold
- **HDBSCAN**: Min cluster size 3, Euclidean metric, probability-based membership
- **Embeddings**: Normalized L2 vectors for cosine similarity clustering
- **Summarization**: Top-k sentence selection by aggregate TF-IDF scores

## Notes

- **Storage**: SQLite at `data/chronicle.db` for zero-config portability
- **Embeddings**: Sentence-Transformers preferred; automatic TF-IDF fallback (4096 features, bigrams)
- **Clustering**: HDBSCAN when available; Agglomerative with 0.6 cosine distance threshold as fallback
- **Performance**: Batch processing of 400 recent documents with incremental clustering

## Data Source
- **Hacker News**: Top stories API with full article text extraction when available
