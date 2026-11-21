# AegisOps FDE Platform

A role-aware incident assistant combining read-only DuckDB SLA metrics,
Markdown runbooks, and (when configured) redacted post-mortems from Neon
pgvector. The FastAPI backend builds its incident-analysis workflow with
LangGraph; the Streamlit UI includes role switching and an SRE control plane.

## Local demo

Install the project and test dependencies, then generate seed data and load the
local metrics database:

```sh
python -m pip install -e ".[test]"
python data_seed/seed_enterprise_data.py
python -c "from aegisops.ingestion import DuckDBSlaStore; store = DuckDBSlaStore('data_seed/generated/aegisops.duckdb'); store.load_csv('data_seed/generated/tenant_sla.csv'); store.close()"
```

Configure a private demo signing key and an OpenAI-compatible chat API key.
Demo role issuance is disabled unless explicitly enabled:

```sh
export JWT_SECRET='use-a-private-random-secret-at-least-32-bytes-long'
export DEMO_TOKEN_ISSUER_ENABLED=true
export OPENAI_API_KEY='...'
PYTHONPATH=backend uvicorn aegisops.api:app --reload
```

In another terminal:

```sh
streamlit run ui/app.py
```

The demo issuer can mint all three demonstration roles and is not an identity
provider. Keep it disabled outside a controlled, isolated demo. Before
production, replace the demo HS256 issuer and validator with the organization's
identity-provider integration and trusted signing keys. Point `AEGISOPS_API_URL`
at the API if it is not at `http://localhost:8000`.

To enable semantic post-mortem retrieval, set `POSTMORTEM_DATABASE_URL` to a
read-only connection for the already-ingested, PII-redacted pgvector index and
provide the embedding settings used during ingestion. The API combines these
SRE-only results with local Markdown runbooks. The API's `/metrics` endpoint
exports Prometheus metrics; restrict it to trusted monitoring networks.

Run tests with `python -m pytest`.
