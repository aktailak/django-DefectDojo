# DefectDojo Gitleaks Deduplication Service

A lightweight, external deduplication service for **Gitleaks → DefectDojo** pipelines.

This service reduces noise in DefectDojo by deduplicating Gitleaks findings **before submission**, preventing repeated secrets from being re-imported across CI runs, branches, and repositories.

---

## Why this exists

When running Gitleaks in CI/CD (GitLab, GitHub Actions, Jenkins, etc.), teams often face:

- Repeated secrets being reported in every pipeline run
- Large volumes of duplicate findings in DefectDojo
- Slower imports and noisy dashboards
- Difficulty applying consistent deduplication logic across scanners

DefectDojo performs deduplication **after import**, but by that point the findings are already stored, indexed, and processed.

This service shifts deduplication **left**, filtering findings *before* they reach DefectDojo.

---

## What this service does

- Accepts **Gitleaks JSON reports**
- Fetches existing findings from DefectDojo via API
- Deduplicates findings based on:
  - Secret value
  - File path
- Returns only **new findings**
- Stores results temporarily in Redis
- Designed to be fast, stateless, and CI-friendly

---

## What this service is NOT

- ❌ Not a Gitleaks replacement
- ❌ Not a DefectDojo plugin
- ❌ Not a new scanner

This is an **external integration component** that sits between secret scanners and DefectDojo.

---

## Typical architecture

Gitleaks (CI job)
↓ JSON report
Deduplication Service
↓ filtered findings
DefectDojo API


---

## Who should use this

- Teams running **Gitleaks in CI/CD**
- DefectDojo users dealing with high duplicate volumes
- Security teams wanting consistent deduplication policies
- Kubernetes / Helm-based deployments

---

## Key features

- FastAPI-based REST API
- Redis-backed temporary storage
- Token-based authentication
- Kubernetes & Helm friendly
- Works with existing DefectDojo instances
- No changes required in DefectDojo or Gitleaks

---

## Roadmap ideas

- Support for additional scanners (Semgrep, Trivy, Detect-Secrets)
- Pluggable deduplication strategies
- Async submission to DefectDojo
- Metrics and observability

---

## Disclaimer

This project is **not affiliated with or endorsed by OWASP DefectDojo or Gitleaks**.  
It is provided as an optional external integration for teams who need more control over deduplication behavior.

---

