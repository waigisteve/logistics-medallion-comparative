# Tooling Matrix

This document defines the tools named in the route plans so each cloud and hybrid option is comparable at the service role level.

## Canonical Platform Functions

| Function | What It Must Do |
|---|---|
| Ingestion bus | Absorb peak producer traffic, preserve replay, and decouple producers from processors |
| Raw landing | Persist immutable source payloads for replay, audit, and incident recovery |
| Stream processor | Validate schema, deduplicate, enrich, handle late events, and route bad records |
| Table format | Support efficient silver and gold reads with partition pruning and governed schema evolution |
| Orchestrator | Run batch backfills, warehouse loads, data quality checks, and promotions |
| Warehouse integration | Move curated outputs into Snowflake staging and production marts |
| Observability | Track freshness, lag, failures, quarantine growth, and cost anomalies |
| Governance | Control schema, metadata, lineage, access, and policy enforcement |

## AWS Tools

| Function | Tool | Defined Role |
|---|---|---|
| Ingestion bus | `Amazon Kinesis Data Streams` | Real-time ingestion buffer with partition keys, replay window, and scale-out capacity |
| Raw landing | `Kinesis Firehose` + `Amazon S3` | Buffered immutable bronze landing with compression and object partitioning |
| Stream processor | `AWS Glue Streaming` or `Managed Service for Apache Flink` | Streaming transforms, schema enforcement, deduplication, watermarking, and quarantine routing |
| Table format | `Parquet` or `Apache Iceberg` on `S3` | Silver and gold curated open-table storage |
| Orchestrator | `AWS Step Functions` or `MWAA/Airflow` | Backfills, quality gates, and controlled warehouse load orchestration |
| Warehouse integration | `Snowpipe` or `Snowpipe Streaming` | Curated feed into Snowflake staging and modeled layers |
| Observability | `CloudWatch`, `CloudTrail`, `SNS` | Metrics, logs, audit trail, and alert fan-out |
| Governance | `Glue Schema Registry`, `Lake Formation` | Contract governance, data permissions, and metadata control |

## Azure Tools

| Function | Tool | Defined Role |
|---|---|---|
| Ingestion bus | `Azure Event Hubs` | High-throughput ingestion buffer with partitions and retention |
| Raw landing | `Event Hubs Capture` + `ADLS Gen2` | Immutable bronze landing in a hierarchical lake |
| Stream processor | `Azure Stream Analytics` or `Azure Databricks Structured Streaming` | Real-time validation, enrichment, event-time processing, and quarantine writes |
| Table format | `Delta Lake` or `Parquet` on `ADLS Gen2` | Silver and gold storage with efficient downstream reads |
| Orchestrator | `Azure Data Factory` or `Airflow` | Batch dependencies, replay, and governed promotion workflows |
| Warehouse integration | `Snowpipe` or `Snowpipe Streaming` | Curated data movement into Snowflake |
| Observability | `Azure Monitor`, `Log Analytics`, `Application Insights` | Metrics, diagnostic logs, alerting, and incident visibility |
| Governance | `Event Hubs Schema Registry`, `Microsoft Purview` | Contract, catalog, lineage, and governance management |

## GCP Tools

| Function | Tool | Defined Role |
|---|---|---|
| Ingestion bus | `Pub/Sub` | Decoupled event ingestion with replay and optional ordering keys |
| Raw landing | `Dataflow` or Pub/Sub export into `Cloud Storage` | Bronze persistence of immutable source events |
| Stream processor | `Dataflow` using `Apache Beam` | Stateful stream processing, deduplication, watermarks, allowed lateness, and quarantine writes |
| Table format | `Parquet` or `Apache Iceberg` on `Cloud Storage` | Curated silver and gold storage with partitioning and compaction |
| Native warehouse | `BigQuery` | GCP-native SQL analytics, gold marts, and BI-facing serving layer |
| Orchestrator | `Cloud Composer` or `Workflows` | Backfills, quality checks, warehouse loads, and environment promotion |
| Warehouse integration | `BigQuery` native datasets or `Snowpipe`, `Snowpipe Streaming`, or staged GCS loads | Native GCP serving or controlled feed into Snowflake |
| Observability | `Cloud Logging`, `Cloud Monitoring`, `Error Reporting` | Freshness, job failures, backlog, and cost-related alarms |
| Governance | `Pub/Sub Schemas`, `Dataplex`, `Data Catalog lineage equivalents` | Schema governance, metadata policy, lineage, and access control |

## Hybrid / On-Prem Tools

| Function | Tool | Defined Role |
|---|---|---|
| Ingestion bus | `Apache Kafka` | Durable event backbone with replay, partitions, and source isolation |
| Raw landing | `MinIO` | S3-compatible immutable bronze object store |
| Stream processor | `Spark Structured Streaming` or `Apache Flink` | Canonical event processing, late data handling, and quarantine routing |
| Table format | `Apache Iceberg` | Governed silver and gold tables with partition evolution and time travel |
| Orchestrator | `Apache Airflow` | Batch control plane for replays, promotions, and downstream loads |
| Warehouse integration | `Trino` and optional `Snowflake` sync | Interactive serving locally and optional cloud warehouse export |
| Observability | `Prometheus`, `Grafana`, `Loki`, `OpenTelemetry` | Metrics, logs, dashboards, traces, and SLO monitoring |
| Governance | `Schema Registry`, `OpenMetadata` or `DataHub`, `Vault` | Schema control, metadata, lineage, secrets, and governance workflows |
