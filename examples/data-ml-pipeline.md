[Overview](../README.md) › [Worked examples](README.md)

# Example 3 — Event-driven data/ML pipeline (stress test: "the code is not the architecture")

**GOOD (excerpt):**

```markdown
## Data flow (logical)
raw events (Kafka) → bronze (Iceberg) → silver (dedup, PII-tokenized) → gold features → feature store → training / online serving
## Invariants
- PII never leaves tokenization in clear text; silver and downstream are tokenized.
- Training and serving read the same feature definitions (no skew by construction).
- Backfills are idempotent per partition date.
## Data contracts: producer-owned schemas in registry; IDs + freshness SLOs only here.
## Elsewhere: DAG code, generated lineage, per-model cards.
```

**BAD:** a table of every Airflow task with schedules and retries, DAG UI screenshots, and copied warehouse column lists.

**Where the guidance breaks down:** a codemap matters less than a *dataset/lineage map*, which is best generated from a data catalog. Keep human-authored invariants (PII, skew, idempotency) as the core.

A fuller AWS version (SageMaker, Feature Store, Lake Formation) is the churn-prediction system in [Concepts explained](../concepts/README.md#churn-prediction-data-analytics-and-ml): its [invariants](../concepts/invariants.md#examples-churn-prediction), [C4 diagrams](../concepts/c4-diagrams.md#44-churn-prediction) and [ARCHITECTURE.md](../concepts/architecture-md-excerpts.md#data-and-ml-churn-modelarchitecturemd).
