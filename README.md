# Phishing Website Detection — Project Documentation

> Purpose: Document the problem, data, workflow, metrics, and how we update/refine the system over time.

## 1) Problem Overview
Binary classification of URLs/websites into **phishing** or **legitimate** with ≥90% accuracy and <1s inference per URL to enable early warning and automated takedowns.

## 2) Goals & Success Criteria
- **Primary**: Accuracy ≥ 90%, F1 (phishing) ≥ 0.88, Latency < 1s/URL (p95).
- **Secondary**: False Positive Rate (legit → phishing) ≤ 1.0% in production.

## 3) Data Summary
- Sources: PhishTank, OpenPhish (+ optional WHOIS, blacklist, HTML features)
- Format: CSV/JSON
- Latest dataset version: see **DATASET_CARD.md**
- Bias & Privacy: multilingual/region diversity; remove PII; comply with ToS.

## 4) Features (current plan)
- URL: length, digits/‘-’/‘@’/‘.’ counts, subdomain depth, use of IP, suspicious tokens
- WHOIS: domain age, registrar, expiration gap
- Reputation: blacklist flags (boolean / score)
- HTML: presence of iframes, onload scripts, form actions, external JS ratio

## 5) Modeling
- **Baselines**: Logistic Regression, Decision Tree
- **Advanced**: LightGBM; URL-transformer (e.g., URL-BERT) for ablation
- Explainability: SHAP for feature importance
- Model releases tracked in **MODEL_CARD.md**

## 6) Evaluation
- Offline: Accuracy, Precision, Recall, F1 (macro & phishing-class), AUC
- Online (post-pilot): alert acceptance, manual review load, takedown lead time
- Results & experiment lineage in **EVAL_LOG.md**

## 7) Versioning & Refinement Workflow (Main)
Used Git for code & docs, and maintain structured logs:
- **DATASET_CARD.md** — every dataset drop (schema, size, class balance, collection date, known bias)
- **MODEL_CARD.md** — each model version (training data, features, metrics, fairness checks, latency)
- **EVAL_LOG.md** — experiment runs with seeds/configs; add confusion matrices & PR curves
- **CHANGELOG.md** — human-readable changes (data, features, code, configs)
- **FEEDBACK_LOG.md** — stakeholder feedback → tracked with status & resolution

**Update cadence**
- Weekly: small fixes, feature tweaks, EVAL_LOG updates
- Bi-weekly: dataset refresh (if available) + MODEL_CARD update if retrained
- Monthly: stakeholder sync → FEEDBACK_LOG + CHANGELOG

**Refinement triggers**
- F1(phishing) < 0.88 in EVAL_LOG
- Drift: PSI > 0.2 on key features vs. training
- Latency p95 > 1s
- Feedback: false-positive spike from ops/users

**How to add an update**
1. Create a branch: `docs/update-YYYY-MM-DD-short-title`
2. Edit relevant files (see checklists below)
3. Open PR with before/after metrics, link to EVAL_LOG row
4. On merge, bump version in MODEL_CARD or DATASET_CARD if applicable
5. Append entry to **CHANGELOG.md**

**Checklists**
- **Data refresh**
  - [ ] Log new version in DATASET_CARD.md
  - [ ] Recompute class balance, leakage check
  - [ ] Update EVAL_LOG.md with baseline vs old model
  - [ ] Note drift/composition changes in CHANGELOG.md
- **Model change**
  - [ ] Add new row to MODEL_CARD.md (version, config, metrics, latency, SHAP)
  - [ ] Update EVAL_LOG.md with comparison tables
  - [ ] Add breaking/non-breaking notes to CHANGELOG.md
- **Stakeholder feedback**
  - [ ] Add entry to FEEDBACK_LOG.md with “Proposed action”
  - [ ] Link to PR/issue; set Status (Open / In Progress / Done)

## 8) Reproducibility
- Fix seeds: `42`
- Configs in code repo (e.g., `configs/*.yaml`)
- Record: dataset version, feature list, model params, commit hash, environment

## 9) Ethics & Risk
Minimize false positives to avoid harming legitimate sites; document FP/TP trade-offs; provide takedown escalation rules.

## 10) Architecture & Workflow
See **docs/workflow.md** (mermaid diagram, ingestion → features → model → decisions).
