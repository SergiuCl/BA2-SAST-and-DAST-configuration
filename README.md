# BA2-SAST-and-DAST-configuration

GitHub Actions workflows and OWASP ZAP Automation Framework plan templates used for the empirical evaluation in the bachelor thesis *Comparative Evaluation of Open-Source SAST Tools Combined with DAST in DevSecOps CI/CD Pipelines*.

The configuration covers two evaluation targets:

- **OWASP Benchmark** (Java) — the primary target. Three SAST workflows (CodeQL, Semgrep PRO, Opengrep OSS) and one DAST workflow (OWASP ZAP) are defined.
- **OWASP Juice Shop** (Node.js) — the supplementary target used for the RQ3 complementarity analysis. One SAST workflow (CodeQL) and one DAST workflow (OWASP ZAP) are defined.

## Layout

```
OWASP Benchmark/
├── .github/
│   ├── workflows/              SAST and DAST GitHub Actions workflows
│   │   ├── codeql-analysis.yml
│   │   ├── semgrep.yaml
│   │   ├── opengrep.yaml
│   │   └── zap-scan.yml
│   └── codeql/codeql-config.yml  CodeQL query suite selection
├── .semgrepignore              Paths excluded from Semgrep / Opengrep scans
└── OWASP-ZAP/zap-plans/        AF plan templates (baseline, full)

OWASP Juice Shop/
├── .github/
│   ├── workflows/              SAST and DAST GitHub Actions workflows
│   │   ├── codeql-analysis.yml
│   │   └── zap-scan.yml
│   └── codeql/codeql-config.yml  CodeQL query suite selection
└── OWASP-ZAP/zap-plans/        AF plan templates (baseline, full, full.authenticated)
```

Both CodeQL workflows use the same structure: queries (`security-extended`, `security-experimental`, `security-and-quality`) and threat models (`local`, `remote`) are loaded from `.github/codeql/codeql-config.yml`, and the CodeQL bundle is pinned to the same version on both targets for reproducibility.

## Usage

Workflow files belong under `.github/workflows/` in the corresponding target repository, with the CodeQL configuration under `.github/codeql/codeql-config.yml`. ZAP plan templates belong under `OWASP-ZAP/zap-plans/` in the same repository. The DAST workflows substitute the `${TARGET_URL}` and `${REPORT_DIR}` placeholders into the selected plan template at workflow runtime.

Implementation details, including version pinning, plan structure, workflow shape, triggers, and result routing, are described in Chapter 3 of the thesis.
