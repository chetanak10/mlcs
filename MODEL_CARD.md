# Model Card

| Field | Value |
|------|-------|
| Model Name | `phish-detector-lgbm` |
| Version | `v0.3.0` |
| Commit | `<git-sha>` |
| Train Data | `ds-2025-11-09` |
| Task | Binary classification (phishing vs legitimate) |
| Inference Budget | < 1s/URL (p95) |

## Intended Use
Early warning for phishing URLs; integrates with scanners/browser/email filters.

## Training & Config
- Features: URL + WHOIS + reputation (+ optional lightweight HTML)
- Params: learning_rate=…, num_leaves=…, max_depth=…
- Environment: Python 3.11, LightGBM X.Y, CPU inference

## Evaluation (Offline)
| Metric | Val | Test |
|---|---:|---:|
| Accuracy |  |  |
| Precision (phish) |  |  |
| Recall (phish) |  |  |
| F1 (phish) |  |  |
| AUC |  |  |

## Latency
- p50=…, p95=…, hardware: …

## Explainability
- Top SHAP features: url_len, has_ip, domain_age_days, js_external_ratio, …

## Safety & Fairness
- FP monitoring on popular legitimate domains
- Regional/language subset checks

## Limitations
- Short-lived domains with clean WHOIS may evade features
- Heavy reliance on URL patterns if HTML is unavailable

## Change Log
- **v0.3.0**: added WHOIS; +2.1 F1 on test; p95 latency unchanged
- v0.2.1: regex features tuned
- v0.2.0: HTML features added (optional)
