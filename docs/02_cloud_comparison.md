# AWS vs Azure vs GCP Comparison

## Comparison Matrix

| Area | AWS | Azure | GCP |
|---|---|---|---|
| Event ingestion | Kinesis Data Streams | Event Hubs | Pub/Sub |
| Raw landing | Firehose to S3 | Event Hubs Capture to ADLS Gen2 | Pub/Sub export or Dataflow to Cloud Storage |
| Stream processing | Glue Streaming or Flink | Stream Analytics or Databricks | Dataflow |
| Curated lake | S3 + Parquet/Iceberg | ADLS Gen2 + Delta/Parquet | Cloud Storage + Parquet/Iceberg with BigQuery as native analytical serving |
| Governance | Glue Schema Registry, Lake Formation | Event Hubs Schema Registry, Purview | Pub/Sub Schemas, Dataplex |
| Warehouse feed | Snowpipe / Snowpipe Streaming | Snowpipe / Snowpipe Streaming | BigQuery native serving or Snowpipe / Snowpipe Streaming |
| Best fit | Broad AWS estate, Lake Formation model, mature Kinesis operations | Strong enterprise governance and Microsoft identity controls | Strong autoscaling, native analytics, and simpler streaming development model |

## Workflow Differences

### AWS

- Strongest fit when the platform team already operates `IAM`, `S3`, and `Kinesis` well.
- Usually cheaper than over-sized Spark-centric designs when incremental streaming replaces nightly full reloads.
- Operational complexity rises if teams mix `Kinesis`, `Glue`, `Flink`, and `Snowflake` without clear ownership.

### Azure

- Strongest fit when `Microsoft Entra ID`, `Purview`, and enterprise networking controls are first-class requirements.
- Often the easiest route for organizations already invested in Azure landing zones and centralized governance.
- Cost discipline depends on avoiding permanently running Databricks clusters when Stream Analytics or job clusters are sufficient.

### GCP

- Strongest fit when rapid autoscaling and managed streaming ergonomics matter most.
- Pub/Sub plus Dataflow is operationally clean for event-driven architectures.
- `BigQuery` is the native GCP warehouse and should be considered the default analytical serving layer unless Snowflake standardization is a hard requirement.
- Cost control improves when autoscaling, Streaming Engine choices, and file compaction are designed up front.

## Feature Comparison

| Feature | AWS | Azure | GCP |
|---|---|---|---|
| Ordering control | Shards and partition keys | Partitions and throughput units | Ordering keys where required |
| Lake governance | Lake Formation | Purview + RBAC | Dataplex + IAM |
| Schema tooling | Glue Schema Registry | Event Hubs Schema Registry | Pub/Sub Schemas |
| Batch + stream unification | Good, but tool choice matters | Good with Databricks-heavy shops | Very strong with Beam/Dataflow + BigQuery |
| Ops burden | Medium | Medium | Low to medium |

## Cost Implications

Exact monthly cost depends on region, retention, record size, number of consumers, and Snowflake strategy. The following points are based on official pricing pages current as of `June 20, 2026`.

| Cost Area | AWS | Azure | GCP |
|---|---|---|---|
| Streaming billing shape | Per stream/shard or per GB depending on Kinesis mode | Throughput-unit based for Event Hubs namespaces | Throughput based for Pub/Sub |
| Processing billing shape | Glue DPU-hour or Flink runtime | Streaming Units or Databricks runtime | Worker vCPU/memory plus Streaming Engine or DCU usage |
| Lake storage billing shape | S3 storage, request, lifecycle, and transfer charges | ADLS storage, transactions, redundancy, and transfer | Cloud Storage storage, operation, and transfer charges, plus BigQuery storage if used for gold serving |
| Main cost risk | Over-retention, many consumers, full reprocessing | Always-on clusters, over-sized TUs, excess replication | Poorly bounded streaming jobs, high transfer, under-compacted data |
| Main savings lever | Incremental processing + tiered storage + right-sized consumers | Job clusters + capture/lifecycle + separated workloads | Autoscaling + Dataflow tuning + storage lifecycle + deliberate BigQuery vs Snowflake serving choice |

## Pricing Notes From Official Sources

- AWS Kinesis supports `on-demand` and `provisioned` modes. Official AWS pricing states on-demand charges are driven by GB written/read plus stream-hour, while provisioned mode is based on shard-hours and capacity assumptions. Source: <https://aws.amazon.com/kinesis/data-streams/pricing/>
- AWS Glue pricing is based on `DPU-hour`, billed by the second, and the Glue Schema Registry itself is listed at no additional charge. Source: <https://aws.amazon.com/glue/pricing/>
- Azure Event Hubs billing is centered on `Throughput Units`, with one TU allowing up to `1 MB/s` ingress and `2 MB/s` egress, billed hourly. Source: <https://azure.microsoft.com/en-us/pricing/details/event-hubs/>
- Azure Stream Analytics pricing is based on provisioned `Streaming Units`. Microsoft notes the listed figures are estimates and actual prices vary by agreement and region. Source: <https://azure.microsoft.com/en-us/pricing/details/stream-analytics/>
- GCP Pub/Sub pricing is throughput-based. Google states the first `10 GiB` of basic throughput per billing account each month is free and then charges per `TiB`; export and import modes have different rates. Source: <https://cloud.google.com/pubsub/pricing>
- GCP Dataflow charges for worker `vCPU`, memory, shuffle, and streaming resources, and Google documents discounted `FlexRS` for certain batch workloads. Source: <https://cloud.google.com/dataflow/pricing>
- BigQuery pricing is based on storage plus the selected compute model such as on-demand bytes scanned or capacity reservations. Source: <https://cloud.google.com/bigquery/pricing>

## Recommendation Logic

- Choose `AWS` when AWS landing zone maturity is already high and governance can be kept simple.
- Choose `Azure` when identity, network isolation, and enterprise governance dominate the decision.
- Choose `GCP` when the team wants the cleanest managed streaming developer workflow plus a strong native warehouse in `BigQuery`.
- Choose `Hybrid` when sovereignty, steady-state cost control, or private data locality are mandatory enough to justify higher platform ownership.
