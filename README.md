# Passive OSINT and GRC Assessment Platform

A platform I designed and built in Python on Linux. It maps an organisation's external exposure the way an attacker's reconnaissance would, using public sources only. It then turns what it finds into a governance, risk and compliance (GRC) risk register that a board, an auditor and an IT team can each act on.

I built it for my own practice, BlackFlag Advisory, and I use it on paid client engagements across government, financial services, health, utilities and professional services.

> **About this repository.** This repo is a showcase. It explains how the platform works and shows the kind of output it produces. The source code is private because it is a commercial tool. I can give read access to the code to interviewers or reviewers on request.
>
> The sample report, register and policies are about a **fictional company**. All domains use the reserved `.example` top-level domain, and all IP addresses come from documentation ranges (RFC 5737). No client data appears anywhere in this repository.

---

## The problem it solves

Most organisations can't see their own internet-facing footprint the way an outsider sees it. Forgotten subdomains, staging systems, exposed origin servers, third-party scripts, public code and outdated privacy disclosures all sit in plain view. The people responsible for them often don't know they're there.

Commercial attack-surface tools are good at listing assets. They rarely tell a risk owner what each item means for their obligations, who should own it, or how confident anyone can be about it. This platform does both. It finds the exposure, then writes it up as a register entry with evidence, an owner and a treatment.

## What it does

1. **Discovers** the organisation's external footprint from public sources: domains, hosts, cloud and email services, mobile apps, public code and package registries, and third-party services loaded by its websites.
2. **Enriches** each finding with context. That includes vulnerability exploitation data (CISA Known Exploited Vulnerabilities and EPSS), edge and CDN detection, and checks of the Australian Business Register so findings are tied to the right legal entity.
3. **Grades the evidence** for every finding. Each one is marked confirmed, probable or possible, observed or inferred, and rated by type, severity and confidence. See [`evidence-grading.md`](evidence-grading.md).
4. **Maps** each finding to control frameworks and obligations, including ISO/IEC 27001:2022 Annex A, the ASD Essential Eight, the Privacy Act 1988 (APPs), APRA CPS 234 and the SOCI Act where they apply.
5. **Writes the register.** Findings become risk register entries with a priority, the evidence behind them, a control domain, a suggested owner on the client side and a treatment.
6. **Synthesises a summary** for executives using a local language model (run through Ollama on my own machine, so client data never goes to a cloud AI service). A wording check flags overstated or alarmist language before anything reaches a report.
7. **Tracks change between assessments.** It compares runs over time and only marks an item as resolved when the module that found it actually ran again.

Every output is reviewed by me before it goes to a client. The platform does the repetitive collection and drafting work, and a person makes the judgment calls.

## Design principles

- **Passive only.** No port scanning, no login attempts, no exploitation, no traffic beyond what any normal visitor or public data source would generate. It shows what's already exposed. It doesn't test or break anything.
- **Evidence before claim.** A finding is only as strong as its evidence, and the report says so. A version that can't be seen from outside is reported as *possible*, never as confirmed.
- **No false all-clears.** If a data source is rate-limited or returns nothing, the check is reported as *Not assessed*, never as "no issue found".
- **No alarmism.** Ratings are framed around evidence, because this is external observation, not an inside view. The platform does not use a "critical" rating.
- **The client owns remediation.** Reports say who in the client organisation should fix each item and how. They don't assign that work to the assessor.

## Outputs

| Output | Audience | Purpose |
|---|---|---|
| Assessment report (HTML and PDF) | Executives, board, risk committee | Headline rating, summary and register |
| Risk register (Excel and CSV) | Risk and compliance teams | Entries ready to merge into an existing enterprise risk register |
| Evidence pack | Auditors, IT teams | The raw observation behind every finding, with timestamps |
| Framework mapping export | Compliance owners | Findings grouped by ISO 27001, Essential Eight, CPS 234 or SOCI |
| Batch export (CSV) | Portfolio view | Multi-entity or sector-wide comparison |

See a full worked example in [`sample-report.md`](sample-report.md) and the matching register in [`sample-risk-register.csv`](sample-risk-register.csv).

## Sample policy set

[`POLICIES.md`](POLICIES.md) lists a core ISO/IEC 27001:2022 policy set for the same fictional company. It's written in Markdown and kept under version control, with document control on every policy and a [changelog](CHANGELOG.md) for the repository. It also shows which assessment findings each policy addresses, linking what the assessment found to the governance that prevents it.

## Architecture

A summary diagram and module list is in [`architecture.md`](architecture.md).

## Stack

Python on Linux, a local LLM through Ollama for summary drafting, Excel and CSV export for registers, and HTML and PDF report rendering.

## About me

Cluny Archibald, Sydney. Bachelor of Cyber Security (Torrens University). I have passed the PECB ISO/IEC 27001 Lead Auditor examination. I have twenty years of experience working with enterprise and government clients before moving into cyber GRC.

[linkedin.com/in/cluny-archibald](https://www.linkedin.com/in/cluny-archibald)

---

© 2026 Cluny Archibald. All rights reserved. This repository is published for portfolio purposes only.
