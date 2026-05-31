# Academic Data Forensics Skill

Academic Data Forensics is a Codex skill for conservative academic numeric forensics. It helps review papers, supplements, raw exports, figures, author replies, and corrected datasets for reproducible anomalies that may require author explanation, editorial review, or institutional follow-up.

The skill is designed for evidence preparation, not for declaring misconduct. Its default language is cautious: findings are treated as numeric or source-data anomalies until they are explained, reproduced, or reviewed by the appropriate human process.

## What It Does

- Preserves original source artifacts and records provenance before analysis.
- Extracts observations into structured evidence with source location, raw strings, parsed values, units, groups, panels, and sample identifiers where available.
- Requires panel and data-type classification before any detector is selected.
- Guides detector choice by semantics: raw replicates, summary statistics, percentages, time series, standard curves, image quantification, author responses, and corrected files are handled differently.
- Produces reviewable evidence objects and reports with source locations, raw values, detector logic, caveats, and alternative explanations.

## Core Principle

Do not run a generic whole-sheet scanner as the primary analysis.

Academic supplementary spreadsheets often mix figure labels, panel titles, group names, raw observations, derived values, time points, percentages, standard curves, and summary statistics in the same sheet. Scanning everything together creates false positives and hides the actual evidence path.

The workflow is:

1. Preserve sources.
2. Build structured data with provenance.
3. Classify panels before testing.
4. Select detectors by data semantics.
5. Add domain and measurement-process context.
6. Produce evidence objects and a cautious report.

## Detector Families

The skill includes a detector catalog covering:

- last-digit and precision anomalies
- exact, near, vector, and block duplication
- structured block similarity across groups, doses, batches, and time points
- constant differences, ratios, linear relationships, and arithmetic patterns
- percentage, count, denominator, mean, SD, SEM, CI, p-value, and formula consistency checks
- measurement-resolution and biological feasibility checks
- author-response and corrected-data consistency checks
- sample-size, missingness, unit, and range checks

Each detector is framed with caveats because many academic datasets contain legitimate structure from instruments, rounding policies, normalization, paired samples, reused controls, standard curves, or derived quantities.

## Evidence Model

The reference schema encourages each finding to include:

- evidence id, type, and severity
- paper metadata and source file versions
- file, sheet, page, figure, panel, range, row, and column location
- claim being evaluated
- detector method, parameters, statistics, and software
- raw values needed for reproduction
- plausible benign explanations
- human review status

This keeps reports auditable and makes future reruns or peer review possible.

## Installation

Clone the repository into a Codex skills directory:

```bash
git clone https://github.com/EnglandLobster/academic-data-forensics-skill.git ~/.codex/skills/academic-data-forensics
```

Then ask Codex to use the skill:

```text
Use $academic-data-forensics to audit this paper and its supplementary tables for numeric anomalies.
```

## Repository Layout

```text
.
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- detector_catalog.md
    `-- evidence_schema.md
```

## Intended Use

Use this skill for:

- academic numeric anomaly triage
- paper and supplementary data review
- PubPeer-style evidence preparation
- author-response or corrected-data comparison
- reproducible research-integrity reports

Avoid using it as:

- a misconduct verdict generator
- a generic spreadsheet scanner
- a substitute for domain review
- a tool for bypassing journal access controls or non-public data boundaries

## Reporting Style

Prefer language such as:

- "The public supplementary table contains..."
- "This pattern is difficult to reconcile with..."
- "Please provide the raw instrument export or analysis script..."

Avoid unsupported claims such as "this proves fabrication" unless an authorized investigation or explicit context supports that wording.
