<div align="center">

# QAVANTIS
### AI-Powered Quality Engineering Platform

**Intelligence Behind Every Release.**

[![Status](https://img.shields.io/badge/status-planning-blue)](#project-status)
[![Quality Engineering](https://img.shields.io/badge/focus-quality%20engineering-5b6ee1)](#scope)

</div>

---

## Architecture

> Place the approved full-flow architecture image at `docs/images/qavantis-architecture.png` for it to render below on GitHub.

![QAVANTIS full-flow architecture](docs/images/qavantis-architecture.png)

## Overview

QAVANTIS is a planned AI-powered quality engineering platform connecting requirements, test design, automation, execution intelligence, defect management, and release decisions in one traceable workflow.

AI can recommend and explain, but must not silently override required approvals or deterministic quality-gate rules.

## Planned end-to-end flow

1. **Ingest inputs** — requirements, user stories, acceptance criteria, application changes, and existing test assets.
2. **Analyze requirements** — identify business rules, risks, and acceptance-criteria coverage.
3. **Design test cases** — generate steps, data, expected results, priorities, and requirement links.
4. **Review and classify** — assess coverage and duplicates; require human approval where appropriate.
5. **Recommend automation** — recommend `AUTOMATE`, `KEEP_MANUAL`, or `REVIEW`, with rationale.
6. **Generate and run tests** — execute through framework-specific generators and runners.
7. **Collect evidence** — results, screenshots, traces, logs, and environment metadata.
8. **Triage failures** — distinguish likely application defects from automation, test-data, and environment issues.
9. **Manage defects and retests** — prepare defect records, identify possible duplicates, track fixes, and verify retests.
10. **Evaluate release gates** — apply explicit policies before allowing or blocking deployment.
11. **Report quality** — coverage, execution results, defect trends, healing events, and release readiness.

## Automation frameworks

| Workflow | Planned stack | Intended use |
|---|---|---|
| Selenium + TestNG | Java, Selenium WebDriver, TestNG, Page Object Model | UI automation with Java |
| Selenium + Cucumber | Java, Selenium, Cucumber/Gherkin, TestNG, Page Object Model | BDD-style UI automation |
| Playwright | TypeScript, Playwright Test, Page Object Model | Modern browser automation |

These are separate generator and execution workflows with shared contracts for test cases, run results, evidence, and defects.

## Test classification

### Suite membership

A test can belong to more than one suite; suite membership is **not** mutually exclusive.

- **Smoke** — critical checks for basic build or deployment health.
- **Sanity** — targeted checks around a change or bug fix.
- **Regression** — broader checks for existing behavior.

### Independent test-type labels

Examples: `POSITIVE`, `NEGATIVE`, `BOUNDARY`, `EDGE`, `UI`, `API`, `PERFORMANCE`, and `SECURITY`. These describe the nature of a test and are independent of suite membership.

## Automation suitability

The planned engine will consider repeatability, regression value, frequency, stability, business risk, implementation effort, and maintenance cost. It will provide a recommendation and rationale rather than automatically treating every test as a candidate for automation.

## Self-healing safeguards

- Capture diagnostics when a locator fails.
- Distinguish locator failures from assertion failures.
- Evaluate candidate locators for uniqueness and intended-element match.
- Retry only under controlled conditions.
- Re-verify the original business assertion after recovery.
- Record changes and evidence; send ambiguous cases for human review.

A changed assertion outcome must never be treated as a successful locator repair.

## Defect intelligence

QAVANTIS is planned to integrate with the existing [AI QA Bug Triage n8n project](https://github.com/ankit19apr/ai-qa-bug-triage-n8n), extending it rather than rebuilding it.

Planned capabilities:
- Normalize failure payloads from each automation framework.
- Classify likely application, automation, test-data, or environment failures.
- Prepare structured defect reports with reproduction steps and evidence references.
- Identify potential duplicates and suggest severity/priority.
- Produce Jira-ready payloads with human approval.
- Track retests and defect closure according to explicit policy.

## Quality gates for clean deployments

Release decisions should use explicit, auditable policies. Planned checks include:

- Build, compile, lint, and type-check results.
- Required smoke-suite results and configured coverage thresholds.
- Defect severity limits and documented exceptions.
- Security and dependency checks where configured.
- Artifact, environment, configuration, and approval checks.
- Post-deployment health and smoke verification.

**PASS** authorizes the next configured release step. **BLOCK** stops the pipeline and reports failed policy checks. Exceptions must be authorized and audited. AI recommendations cannot override a deterministic gate.

## Planned architecture

```text
Requirements / Changes / Existing Test Assets
                    |
                    v
       Requirement Analysis & Test Design
                    |
                    v
        Review, Traceability & Classification
                    |
                    v
          Automation Suitability Engine
                    |
          +---------+----------+
          |         |          |
          v         v          v
     Selenium   Selenium    Playwright
     + TestNG   + Cucumber  + TypeScript
          |         |          |
          +---------+----------+
                    |
                    v
       Execution Orchestration & Evidence
                    |
                    v
      Failure Classification / Defect Triage
                    |
                    v
          Retest & Defect Lifecycle
                    |
                    v
        Deterministic Quality Gate Engine
                    |
             +------+------+
             |             |
             v             v
       Allow release   Block release
                    |
                    v
       Dashboard, Metrics & Notifications
```

## Planned technology choices

The implementation will be incremental and local-first/cost-conscious, starting with mocked AI behavior before optional model-provider integration.

- **Automation:** Java, Selenium WebDriver, TestNG, Cucumber, TypeScript, Playwright Test
- **Orchestration:** n8n; test runners execute as separate processes
- **Persistence:** PostgreSQL as the planned source of truth
- **CI/CD:** GitHub Actions
- **Integrations:** Jira, GitHub, Slack/email, test management, artifact storage, optional security tooling
- **AI:** provider-independent interfaces with local/mock implementations for early development

Never commit secrets, embed them in prompts, or write them to logs.

## Planned repository structure

```text
qavantis/
├── .github/workflows/
│   ├── java-testng.yml
│   ├── selenium-cucumber.yml
│   ├── playwright.yml
│   ├── n8n-validation.yml
│   └── quality-gate.yml
├── docs/
│   ├── images/qavantis-architecture.png
│   ├── architecture.md
│   ├── implementation-roadmap.md
│   └── data-model.md
├── workflows/n8n/
├── database/migrations/
├── generators/
│   ├── selenium-testng/
│   ├── selenium-cucumber/
│   └── playwright-typescript/
├── defect-management/
├── tests/
├── .gitignore
├── CONTRIBUTING.md
└── README.md
```

This is the intended structure, not a claim that these components or workflows have already been implemented.

## Delivery roadmap

1. Repository conventions, architecture docs, and shared schemas.
2. Requirement-to-test-case design and human review.
3. Persistence, traceability, and manual execution tracking.
4. Automation-suitability recommendations.
5. Selenium + Java + TestNG generator and runner.
6. Selenium + Cucumber + TestNG generator and runner.
7. Playwright + TypeScript generator and runner.
8. Evidence capture and controlled self-healing.
9. Integrate existing n8n defect triage and add retest tracking.
10. Deterministic quality gates and GitHub Actions.
11. Dashboards, reports, and operational safeguards.

## Project status

**Planning / architecture stage.** Implementation, GitHub Actions workflows, repository creation, and deployment gates should be marked complete only after they have been built and verified.

## Contributing

Contribution guidance, coding standards, test commands, and pull-request checks will be added as implementation progresses.

## License

License not yet selected. Choose a license before distributing the project or accepting external contributions.
