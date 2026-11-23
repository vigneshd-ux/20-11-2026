# Task 6 submission

## Endpoint status table

| Endpoint | Status codes | Tested (Y/N) |
|---|---|---|
| `POST /search` | 200, 401, 422, 503 | Y |
| `POST /search/batch` | 200, 401, 422, 503 | Y |
| `GET /health` | 200, 503 | Y |
| `GET /documents` | 200, 401, 503 | Y |
| `POST /documents` | 200, 400, 401, 415, 503 | Y |
| `DELETE /documents/{source}` | 200, 400, 401, 404, 503 | Y |

“Tested” means the route was exercised by the six pytest cases and/or live requests; the table lists supported status paths, not a claim that every listed code was independently triggered.

## Search latency

Twenty authenticated `POST /search` requests were measured end-to-end over a reused local HTTP connection. One warm-up request was excluded. Percentiles use NumPy's default linear interpolation.

- **p50:** 14.149 ms
- **p95:** 163.993 ms
- **Mean:** 61.027 ms

The raw measurements are in [search_latency.json](search_latency.json), with a short summary in [search_latency.md](search_latency.md).

## API tests

`pytest -q tests/test_api.py`: **6 passed**. Captured output: [test_results_task6.txt](test_results_task6.txt).
