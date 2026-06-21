# Logistics Medallion Comparative Blueprint

This repository is the comparative entry point for the logistics medallion blueprint. It keeps the cross-route architecture, guardrails, cloud comparison, and delivery roadmap in one place while each implementation route lives in its own repository.

## Included Documents

- `docs/01_architecture_principles.md`: canonical workflow and platform rules
- `docs/02_cloud_comparison.md`: AWS vs Azure vs GCP comparison
- `docs/03_guardrails.md`: delivery, security, and data quality controls
- `docs/04_delivery_roadmap.md`: staged execution plan
- `docs/05_tooling_matrix.md`: tool-by-tool comparison across routes
- `docs/06_architecture_workflows.md`: route workflow view
- `docs/07_integrity_security_controls.md`: deletion, duplication, and intrusion controls

## Route Repositories

- `logistics-medallion-aws`
- `logistics-medallion-azure`
- `logistics-medallion-gcp`
- `logistics-medallion-hybrid`

## Scenario Baseline

The target platform must handle:

- `120M-130M` daily events
- `3,500-4,500` peak events per second
- `<5 minute` operational freshness
- `<10 second` dashboard queries over `90 days`
- Budget reduction from roughly `$61K/month` to `<= $42K/month`
- Strong protection against schema drift and silent corruption

## Route Summary

| Route | Ingestion | Lake | Processing | Governance | Serving |
|---|---|---|---|---|---|
| AWS | Kinesis | S3 | Glue Streaming / Flink | Glue Schema Registry + Lake Formation | Snowflake |
| Azure | Event Hubs | ADLS Gen2 | Stream Analytics / Databricks | Event Hubs Schema Registry + Purview | Snowflake |
| GCP | Pub/Sub | Cloud Storage / BigQuery | Dataflow + BigQuery | Pub/Sub Schemas + Dataplex | BigQuery by default, Snowflake optional |
| Hybrid | Kafka | MinIO | Spark / Flink | Schema Registry + OpenMetadata | Trino by default, Snowflake optional |

## Notes

- `GCP` keeps BigQuery-specific documentation and Terraform wiring because native serving should default to BigQuery.
- `Hybrid` keeps platform-selection documentation because provider choice depends on the target on-prem environment.
