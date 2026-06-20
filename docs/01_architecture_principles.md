# Architecture Principles

## Canonical Workflow

1. Producers emit shipment, vehicle, temperature, and hub scan events into a managed streaming layer.
2. Raw events land immutably in the lake as the bronze layer.
3. Streaming or micro-batch processing validates schema, deduplicates, enriches, and quarantines bad events.
4. Validated events are written to silver in a columnar open-table format where practical.
5. Aggregated facts, dimensions, and dashboard-ready tables are written to gold.
6. Snowflake receives curated data through controlled batch or streaming ingestion.
7. BI, finance, and data science workloads consume fit-for-purpose serving layers.

## Medallion Mapping

| Layer | Purpose | Typical Data |
|---|---|---|
| Bronze | Audit, replay, forensic recovery | Raw JSON/Avro events, source metadata, ingestion timestamps |
| Silver | Trusted operational model | Flattened events, canonical schemas, deduplicated streams, conformed reference joins |
| Gold | Serving and analytics | KPI marts, SLA aggregates, finance reconciliations, DS feature extracts |

## Core Design Decisions

- Use near-real-time streaming or micro-batching instead of nightly full reloads.
- Keep `event_time` and `ingestion_time` separate.
- Treat schema drift as an operational incident, not a data cleanup task.
- Use incremental `MERGE` logic and partition-scoped backfills.
- Prefer open columnar formats and file compaction to reduce scan cost.
- Separate compute for ops, finance, and DS consumers to avoid interference.

## Required Non-Functional Targets

- Freshness SLO: `<5 minutes`
- Warehouse query SLA: `<10 seconds` over `90 days`
- Replayability: `>= 7 days`
- RPO: `<15 minutes` for metadata and orchestration state
- RTO: `<4 hours` for the end-to-end ingestion path

## Mandatory Delivery Pattern

- `dev` for feature integration and synthetic test data
- `test` for contract, performance, and failover verification
- `prod` for controlled rollout with approvals and rollback hooks

## Terraform Module Boundaries

- `security`
- `streaming`
- `data_lake`
- `processing`
- `warehouse_integration`
- `observability`

Those boundaries stay stable across all routes so teams can compare implementations directly.
