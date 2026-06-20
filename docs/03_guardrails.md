# Guardrails

## Data Guardrails

- Enforce producer contracts for all required fields before silver writes.
- Quarantine invalid payloads instead of coercing them silently.
- Persist `raw_payload`, `schema_version`, `event_time`, `ingestion_time`, and `source_system` on every event.
- Deduplicate with deterministic business keys when a source `event_id` is absent.
- Use partition-scoped replay and backfill; avoid full historical reloads by default.

## Platform Guardrails

- Every environment must have isolated state, credentials, topics, buckets, and alert targets.
- All production infrastructure changes must flow through reviewed Terraform plans.
- Remote state must be encrypted, versioned, and access controlled.
- Secrets must come from managed secret stores, never plain text `tfvars`.
- All storage layers must use encryption at rest and private network access where supported.

## Security Guardrails

- Least-privilege identities only.
- Break-glass access must be logged and time-bound.
- Customer-managed encryption keys are preferred for silver and gold data.
- Audit logs must be immutable and retained per policy.
- Cross-region replication must be explicit and tested.

## CI/CD Guardrails

- Block `apply` from pull request contexts.
- Require `fmt`, `validate`, `lint`, and policy checks before merge.
- Require manual approval for `test` and `prod` applies.
- Tag every release with an immutable version and deployment artifact reference.

## Operational Guardrails

- Alert on freshness, backlog, quarantine rate, and duplicate rate.
- Alert on storage growth anomalies and Snowflake credit spikes.
- Maintain runbooks for schema incidents, replay, failover, and warehouse load lag.
- Test replay and disaster recovery at least quarterly.

## FinOps Guardrails

- Budget alerts per environment and per major service.
- Lifecycle policies for bronze retention and compaction for silver/gold.
- Separate compute for streaming, finance, and DS workloads.
- Default to autosuspend, autoscaling, or ephemeral job clusters unless justified otherwise.
