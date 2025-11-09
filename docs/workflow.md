# End-to-End Workflow

```mermaid
flowchart LR
  A[Ingestion: URLs] --> B[Enrichment: WHOIS / Reputation / Optional HTML]
  B --> C[Feature Engineering]
  C --> D[Train: Baseline + LGBM + URL-Transformer(ablate)]
  D --> E[Evaluate: offline metrics, SHAP, latency]
  E --> F{Meets gates?}
  F -- Yes --> G[Package & Release model vX.Y.Z]
  F -- No --> H[Refine features / data / thresholds]
  G --> I[Deploy Scorer API / Batch Scanner]
  I --> J[Monitoring: metrics, drift, FP reports]
  J --> K[Feedback loop -> FEEDBACK_LOG / CHANGELOG / retrain]
