# Backend

Backend services for ingestion, processing, and storage.

Install the project and test dependencies into the active environment from the
repository root with `python -m pip install -e ".[test]"`. Run the test suite
with `python -m pytest` (do not execute the test file directly). Generate seed data with
`python data_seed/seed_enterprise_data.py`, then configure `OPENAI_API_KEY` and
`NEON_DATABASE_URL` before running the ingestion pipeline:

```sh
PYTHONPATH=backend python -m aegisops.ingestion
```

The pipeline uses Presidio's default English NLP model. Install the model
configured by Presidio (for example, `python -m spacy download en_core_web_lg`)
in the runtime image before processing documents.
`OPENAI_BASE_URL`, `OPENAI_EMBEDDING_MODEL`, and
`OPENAI_EMBEDDING_DIMENSIONS` can select an OpenAI-compatible embedding service
and its vector size. SLA metrics are loaded locally into DuckDB.
