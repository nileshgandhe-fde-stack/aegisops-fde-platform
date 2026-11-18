# aegisops-fde-platform

Capstone Phase 1: AegisOps Ingestion & Storage Core.

This repository is organized as a monorepo:

- `backend/` — ingestion, processing, and storage services.
- `ui/` — user interface.
- `deploy/helm/` — Helm charts and deployment configuration.
- `data_seed/` — seed datasets and initialization assets.

The platform combines enterprise data ingestion, Presidio PII scrubbing,
DuckDB analytics, and Neon pgvector storage.
