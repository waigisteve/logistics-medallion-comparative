# Architecture Workflows

This document adds route workflow views across the four target implementations.

## AWS Workflow

`Kinesis Data Streams -> Amazon S3 bronze -> AWS Glue Streaming or Flink -> S3 silver/gold -> Snowflake`

## Azure Workflow

`Event Hubs -> ADLS Gen2 bronze -> Stream Analytics or Databricks -> ADLS silver/gold -> Snowflake`

## GCP Workflow

`Pub/Sub -> Cloud Storage bronze -> Dataflow -> Cloud Storage silver -> BigQuery`

Optional serving path:

`Pub/Sub -> Cloud Storage bronze -> Dataflow -> Cloud Storage silver -> Snowflake`

## Hybrid Workflow

`Kafka -> MinIO bronze -> Spark or Flink -> MinIO/Iceberg silver-gold -> Trino`

Optional serving path:

`Kafka -> MinIO bronze -> Spark or Flink -> MinIO/Iceberg silver-gold -> Snowflake`
