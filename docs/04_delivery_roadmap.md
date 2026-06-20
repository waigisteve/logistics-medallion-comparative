# Delivery Roadmap

## Phase 1: Foundation

- Finalize one route as the initial implementation candidate.
- Stand up remote Terraform state, identity, networking, and secrets management.
- Create bronze landing with replay retention.

## Phase 2: Trusted Ingestion

- Implement schema registry and producer contract tests.
- Build streaming ingestion and quarantine handling.
- Prove `<5 minute` end-to-end latency in `test`.

## Phase 3: Silver and Gold

- Add canonical silver schemas and incremental gold marts.
- Wire Snowflake staging and `MERGE` patterns.
- Validate dashboard and finance use cases independently.

## Phase 4: Hardening

- Add DR, backfill automation, policy enforcement, and workload budgets.
- Execute scale and failure drills.
- Promote the route into an independent project repository.
