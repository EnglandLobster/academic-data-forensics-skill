---
name: academic-data-forensics
description: Audit academic papers, manuscripts, PDFs, Markdown/Word text, figures/images, supplementary tables, CSV/Excel datasets, raw instrument exports, author responses, corrected files, extracted PDF tables, and reported summary statistics for numeric or image-linked anomalies that may indicate research data integrity problems. Use when Codex is asked to do academic numeric fraud detection, paper data forensics, research misconduct triage, data anomaly screening, PubPeer-style evidence preparation, or Chinese requests such as 论文数字打假, 学术数据打假, 论文数据异常分析, 补充数据审计, 原始数据核查, or 科研诚信数字取证.
---

# Academic Data Forensics

Use this skill to produce reproducible numeric evidence, not final misconduct verdicts. State findings as anomalies that require author explanation or institutional review unless the user explicitly asks for internal triage language.

## Source-Agnostic Intake

Do not assume the input is a spreadsheet. The user may provide PDFs, Markdown extracted from a paper, Word files, screenshots, figure images, source-data tables, CSV/Excel supplements, raw instrument exports, analysis scripts, archives, PubPeer posts, author replies, or corrected datasets.

Treat every input as a source artifact first, then normalize only the relevant observations into a common evidence model:

- manuscript text and figure legends define intended experiment semantics, groups, n, units, statistics, and claims.
- source-data tables and supplements provide reported raw/processed values.
- raw instrument exports, FACS gates, image-analysis outputs, microscopy object counts, sequencing count matrices, and scripts provide provenance and reproducibility checks.
- figures/images can corroborate numeric anomalies through duplication, relabeling, altered overlays, reused specimens, or mismatch with source data.
- author responses and corrected files are separate evidence versions, not replacements for the original source.

Use the right parser/workflow for the artifact type, preserving the original file unchanged. Convert to structured tables only after recording provenance and original formatting.

## Core Workflow

1. Preserve sources.
   - Keep original PDFs, manuscripts, screenshots/images, supplementary files, exported tables, raw instrument files, archives, scripts, and author responses immutable.
   - Record URL/path, download time if available, file hash when practical, figure/table location, sheet/range, and raw numeric strings.
   - Preserve author replies, corrected files, and newly supplied raw data as separate source versions; never merge them into the original evidence set without version labels.
   - Do not convert all numbers to floats before preserving decimal precision and original formatting.

2. Build structured data.
   - Extract tables into rows with provenance: paper, file, sheet/page, figure/table/panel, row, column, raw string, parsed value, unit, group, time point, and sample id when available.
   - Separate observed raw data, derived values, summary statistics, p-values, and author response data.
   - When available, link reported percentages/ratios back to raw counts, denominators, instrument exports, gates, image objects, or measurement events.

3. Classify panels before testing.
   - Do not run a generic whole-sheet scanner as the primary analysis. Supplementary spreadsheets often mix panel titles, group labels, time points, raw observations, derived values, and summary statistics in one sheet.
   - Split data by figure/panel/table section and classify each block as raw replicates, summary statistics, time series, score/count, percentage, calibration/standard curve, image quantification, or author response data.
   - For repeated-measure or multi-condition panels, reconstruct the intended data grid before testing: group/treatment, time point/dose, replicate, batch, sample id, and whether rows or columns encode replicates.
   - Use the paper text, figure legend, methods, units, column labels, and neighboring rows to decide what each block means.

4. Select detectors by data semantics.
   - Last digit and last-two-digit concentration.
   - Decimal precision, fractional-part reuse, and local precision/formatting shifts.
   - Exact and near duplicate values, columns, row blocks, cross-table vectors, and cross-domain vectors.
   - Structured block similarity across related groups: replicate-level cell matches, copied time-point rows, copied condition blocks with small edits, and repeated within-block rank/order patterns.
   - Constant differences, constant ratios, near-linear relationships, arithmetic progressions.
   - Formula consistency and back-calculation checks: percentage/count/denominator, mean/SD/SEM/n, p-value/test-statistic, concentration/count/volume.
   - Sample-size, missingness, unit, measurement-resolution, and biological range checks.
   - Treat standard curves, calibration series, interpolation grids, generated dose gradients, and explicitly derived quantities differently from independent experimental observations.

5. Add domain semantics.
   - Ask what measurement process generated the numbers before judging the distribution.
   - Treat instrument resolution, rounding policy, bounded scales, integer counts, normalization, batch processing, paired samples, and reused controls as alternative explanations.
   - Compare reported precision with what the measurement process can plausibly produce, especially for manual counts, animal weights, FACS gates, image quantification, and normalized percentages.
   - Upgrade severity when an anomaly violates the experiment definition, appears across independent detector families, or crosses unrelated biological quantities.
   - If figures/images are part of the task, record image reuse or altered-overlay concerns as separate corroborating evidence and use an image-forensics workflow rather than forcing them into numeric detectors.

6. Produce evidence objects and a report.
   - Include exact source locations, raw values, method, statistic, alternative explanations, and review status.
   - Avoid unsupported statements like "this proves fraud"; prefer "public materials contain numeric patterns that are difficult to reconcile with the stated measurement process."

## Coding Guidance

Write task-specific analysis code only after panel classification. Prefer small, inspectable scripts or notebooks that encode the current paper's data layout and detector choice. Make the script output source locations, raw values, and intermediate calculations so a human can audit the claim.

When a dependency issue blocks correct parsing, fix the environment or use an available runtime with the needed library. Do not simplify the analysis merely to fit an underpowered environment.

## When To Read References

- Read `references/detector_catalog.md` when choosing which numeric checks to apply or explaining caveats.
- Read `references/evidence_schema.md` when creating machine-readable evidence objects or a review/report pipeline.

## Severity Guidance

- Low: isolated weak anomaly, likely compatible with rounding or instrument constraints.
- Medium: statistically unusual pattern in one coherent table/column, needs manual source verification.
- High: exact/near duplicate vectors, fixed algebraic relationships, impossible summary statistics, or formula violations.
- Critical: multiple independent high-severity anomalies across raw data, derived data, figures, supplements, and author-provided responses.

## Reporting Rules

- Lead with the strongest reproducible evidence, not speculation about intent.
- Show enough raw values for the reader to reproduce the claim, but avoid overwhelming tables in the main body.
- List plausible benign explanations and whether the public data supports or contradicts them.
- Keep accusatory language out of public-facing drafts unless the user explicitly asks for a rhetorical/social-media style.
