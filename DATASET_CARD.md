# Dataset Card

**Current Version**: `ds-2025-11-09`
**Owner**: Security ML Team
**Scope**: URLs + optional WHOIS/HTML/reputation

## 1) Composition
- Total samples:  N=
- Class balance:  phishing = %, legitimate = %
- Regions/languages:  en, hi, es, …

## 2) Collection & Licensing
- Sources: PhishTank, OpenPhish (+ terms reviewed)
- Pull dates: 2025-11-01 … 2025-11-08
- Usage: research & internal evaluation

## 3) Schema (CSV)
- url:string
- label:{phishing,legitimate}
- url_len:int, has_ip:boolean, num_subdomains:int, …
- whois_domain_age_days:int?, registrar:str?
- reputation_blacklist_hit:boolean?
- html_iframe_count:int?, js_external_ratio:float?

## 4) Quality & Bias
- Deduping: applied / not applied
- Leakage checks: (e.g., same domain across splits) result = …
- Known biases: over-representation of TLDs X/Y, language Z

## 5) Splits
- Train/Val/Test = 70/15/15 (stratified by label & domain family)

## 6) Changes in this Version
- Added WHOIS fields; improved blacklist coverage
- Reduced domain leakage by grouping splits by eTLD+1

## 7) Prior Versions
- `ds-2025-10-25`: initial drop (no WHOIS)
