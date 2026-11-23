# Document ingestion starter

A small multi-format ingestion project with an attributed open-source sample corpus.

## Setup

```sh
python3 -m venv .venv
./.venv/bin/python -m pip install -r requirements.txt
```

## Extract and summarize

```sh
./.venv/bin/python -m src.extract_text datal
```

`extract_text(path)` returns a list of page strings for PDF files and section strings for DOCX/TXT files. The CLI prints each file's type, extracted page or section count, and character count. Empty or unreadable files, encrypted PDFs, image-only PDFs, and text decoding problems are logged with the file path.

See [sources.md](sources.md) for the origin of every corpus file.

## Chunking

```sh
./.venv/bin/python -m src.chunking datal --output chunks.jsonl
```

The default is 500 estimated lexical tokens per chunk with 75 tokens of overlap. The checkout did not contain the Day 3 chunk-size setting, so this practical retrieval default is used and can be overridden with `--chunk-size` and `--overlap`. Chunk IDs are deterministic SHA-256 hashes of the source filename, page/section, chunk order, and text. Metadata records the source filename and type, page or section locator, chunk index, and chunk settings. Statistics are printed after writing JSONL.

Scanned PDFs trigger a best-effort OCR attempt through PyMuPDF's Tesseract integration. Install the Tesseract executable and English language data for OCR to work; otherwise the file is skipped with a warning.

## Embedding

```sh
./.venv/bin/python -m src.embed_corpus chunks.jsonl
```

This embeds in batches of 64 with `sentence-transformers/all-MiniLM-L6-v2`, displays progress, retries failed batches up to three times, and records exhausted failures in `failed_batches.jsonl`. Matching vectors are cached in `embeddings_cache.sqlite3` by model, chunk ID, and text hash. It writes one vector per chunk to `embeddings.jsonl`, records the latest elapsed time and local inference cost in `embedding_summary.json`, and appends each run to `embedding_runs.jsonl`. It verifies the vector count before succeeding. Local model cost is reported as $0; hardware cost is not included. If the model is already cached and the machine is offline, add `--local-files-only`.

To change the model, use `--model MODEL_NAME`. The expected 3,480 count is an example from a different corpus; this checkout currently contains 820 chunks in `chunks.jsonl`, which is the input embedded by default.

## Qdrant vector storage and search

Qdrant Server is separate from the Python client. [Install the server binary](https://qdrant.tech/documentation/install/) or run it with Docker, then start the local service (this script binds to loopback and stores data in `qdrant_storage/`):

```sh
./scripts/run_qdrant.sh
```

In another terminal, upload the existing vectors and metadata, then search:

```sh
./.venv/bin/python -m src.vector_store index
./.venv/bin/python -m src.vector_store search "How does the World Bank handle information requests?" --top-k 3
./.venv/bin/python -m src.vector_store search "FY 2015 response times" --doc-type docx --top-k 3
```

The `document_chunks` collection uses 384-dimensional cosine vectors, HNSW (`m=16`, `ef_construct=100`), and indexed payload fields for `doc_type`, `source`, and page/section locators. Its indexing threshold is lowered for this small training corpus so Qdrant builds the HNSW graph. `search(query, top_k, filters)` accepts metadata filters and applies a cosine score threshold of `0.35` by default; pass `score_threshold=None` to disable it or a different value to tune it. Query embeddings use the same cached SentenceTransformers model as the corpus.

See [search_evaluation.md](search_evaluation.md) for the ten-query manual relevance check and weakest queries.

## Search API

Set a private API key and start the app after Qdrant is running:

```sh
export SEARCH_API_KEY='replace-with-a-long-random-secret'
export QDRANT_URL='http://127.0.0.1:6333'
./.venv/bin/python -m uvicorn src.api:app --host 127.0.0.1 --port 8000
```

`GET /health` is public. `GET /documents` and `POST /search` require the `X-API-Key` header. Search request example:

```sh
curl -X POST http://127.0.0.1:8000/search \
  -H 'Content-Type: application/json' \
  -H "X-API-Key: $SEARCH_API_KEY" \
  -d '{"query":"How does the World Bank respond to information requests?","top_k":5,"doc_type":"pdf","min_score":0.4}'
```

`top_k` must be 1–20. `doc_type`, `source`, and `min_score` are optional. The embedding model and Qdrant client are loaded once in the app lifespan; blocking inference and storage calls run in a worker thread. If `SEARCH_API_KEY` is missing, protected endpoints fail closed with HTTP 503.

## Document lifecycle and batch search

`POST /documents` accepts multipart form data with a `file` field and supports `.pdf`, `.docx`, and `.txt`; it stores the upload under `datal/uploads/`, extracts/chunks/embeds it, and adds its chunks to Qdrant. `DELETE /documents/{source}` removes all matching chunks and the uploaded copy, if one exists. Unsupported extensions return 415, empty or non-extractable documents return 400, and Qdrant/storage errors return 503.

`POST /search/batch` accepts `{"queries":[{"query":"...","top_k":5}, ...]}` and returns one ranked result set per query. See [task6_submission.md](task6_submission.md) for the endpoint status table, six-test result, and twenty-request latency measurements.
