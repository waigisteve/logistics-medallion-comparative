# Integrity And Security Controls

This document explains how deletion, duplication, and security infiltration are handled across all routes.

## Data Deletion

- Replay retention exists at the ingestion layer.
- Raw storage is versioned, soft-deleted, or object-locked depending on platform.
- Curated layers are recoverable from upstream truth rather than treated as irreplaceable.

## Data Duplication

- Bronze keeps raw truth, including duplicates.
- Silver processing uses deterministic keys, watermarking, and idempotent writes.
- Gold uses incremental `MERGE` or equivalent idempotent serving logic.

## Security Infiltration

- Separate runtime identities for producers, processors, loaders, and admins.
- Secrets are kept outside code and Terraform vars.
- Audit logs, anomaly detection, and versioned storage make tampering visible and recoverable.

## Route Summary

| Route | Delete Recovery | Duplicate Control | Intrusion Detection |
|---|---|---|---|
| AWS | Kinesis replay + S3 versioning | Glue or Flink + Snowflake `MERGE` | CloudWatch and IAM telemetry |
| Azure | Event Hubs retention + ADLS soft delete | Stream Analytics or Databricks + `MERGE` | Azure Monitor and identity telemetry |
| GCP | Pub/Sub retention + Cloud Storage versioning | Dataflow + BigQuery or Snowflake idempotent loads | Security Command Center and audit logs |
| Hybrid | Kafka retention + MinIO versioning + Iceberg snapshots | Spark or Flink + Iceberg upserts | SIEM, Vault audit, and platform telemetry |
