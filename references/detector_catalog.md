# Numeric Detector Catalog

Use this reference when auditing academic numeric data. Combine detectors; a single weak detector rarely justifies a strong claim.

## Digit And Precision Detectors

- Last digit concentration: count the final decimal/integer digit within a comparable measurement column. Flag dominant digits, missing digits, and 0/5 heaping.
- Last-two-digit concentration: useful for large n and values reported with at least two decimal places. Flag repeated suffixes such as 34 or 87.
- Decimal scale consistency: compare the number of digits after the decimal point. Flag mixed precision, sudden local precision shifts, and columns where decimal parts are copied.
- Precision islands: adjacent rows, treatment blocks, or highlighted subsets suddenly switch from one decimal policy to another, drop decimals, or add extra decimals not used elsewhere.
- Fractional-part reuse: integer parts differ but decimal tails, last two digits, or longer suffix vectors are identical or nearly identical across groups/columns.
- Rounding compatibility: test whether a claimed rounding rule can produce the reported values.
- Conditional digit tests: if a benign explanation applies to one subset such as integer counts or coarse recording, rerun digit tests within the remaining comparable decimal-precision subset.

Caveats: bounded scores, integer counts, device step sizes, manual rounding policies, and normalized values can create non-uniform digits. Always classify measurement resolution first.

## Duplication And Similarity Detectors

- Exact duplicates within a column, row block, sheet, or full paper.
- Near duplicates after rounding or tolerance.
- Duplicate columns or vectors across different figures/tables.
- One-off differences: vectors that differ at only one or two positions.
- Permutation duplicates: same multiset in different order.
- Cross-domain similarity: similar vectors across unrelated quantities such as gene expression and tumor volume.

High-value patterns: duplicated vectors across unrelated experiments, duplicated raw and derived data, or duplicated values with small manual edits.

## Structured Block Similarity Detectors

Use these when a panel contains comparable condition blocks, such as treatments, inhibitors, genotypes, tissues, doses, time points, batches, or author-response repeats. Reconstruct the intended grid first: group × time/dose × replicate/sample.

- Cell-wise exact matches between condition blocks after aligning by time/dose and replicate position.
- Row-level reuse: an entire time point, dose, or subgroup row is identical across multiple conditions.
- Column/replicate reuse: the same replicate trajectory appears under different conditions.
- Copy-edit patterns: two blocks share many cells exactly, while differing cells are changed by small round increments such as 0.01, 0.1, 1, 5, or 10.
- Rank/order reuse: different condition blocks preserve the same replicate ordering at many time points even when values are shifted.
- Near-block similarity: low mean absolute difference or high correlation across aligned cells, especially when exact cell matches cluster in rows or columns.

Useful statistics: number and fraction of exact matched cells, exact matches excluding structural baselines such as day 0 or zero-dose controls, per-row match counts, per-column match counts, mean absolute difference, edit-step histogram, and a small aligned raw-value matrix.

Caveats: shared baselines, paired samples, copied controls, plate layouts, normalization to the same reference, and deterministic simulation outputs can legitimately create block similarity. Escalate when exact replicate-level values recur across independently treated groups, when the methods imply independent measurements, or when differences look like sparse manual edits rather than expected measurement noise.

## Algebraic Detectors

- Constant difference: `x_i - y_i = c`.
- Constant ratio: `x_i / y_i = c`.
- Near-linear relation: `y = ax + b` with implausibly high fit across independent samples.
- Arithmetic/geometric progressions.
- Repeated increments or over-regular time-series trajectories.
- Simple column arithmetic: one column or block can be obtained from another by adding, subtracting, scaling, or rounding with a small constant offset; check both directions and partial blocks.

Caveats: derived variables, unit conversion, baseline subtraction, normalization, and standard curves can be algebraic by design. Check captions and methods before escalating.

## Formula Consistency Detectors

- Percentage equals count divided by denominator.
- Back-calculated raw data: supplied counts/denominators appear reverse-engineered from reported rounded percentages rather than independently measured.
- Integer compatibility: for a percentage, enumerate possible integer numerator/denominator pairs under the stated rounding rule; flag values that require impossible or highly constrained divisibility.
- Denominator granularity: denominators are implausibly rounded or fixed, such as all multiples of 50, when the measurement process or instrument records exact object counts.
- Mean, SD, SEM, CI, n consistency.
- P-value/test-statistic/degrees-of-freedom consistency.
- Fold change and normalization consistency.
- Concentration, mass, volume, and dilution consistency.
- Tumor volume formulas and other domain equations.

Strong signals: mathematically impossible summaries, raw counts that look back-calculated from rounded percentages, or author-supplied raw data that cannot reproduce published summaries.

## Measurement-Resolution And Feasibility Detectors

- Reported precision exceeds practical measurement resolution, such as live-animal weights reported to unstable hundredths of a gram, manual microscopy counts converted to overprecise percentages, or flow/image outputs rounded inconsistently.
- Values have too little expected measurement noise for the stated instrument, organism, assay, or manual counting process.
- The same instrument is claimed to count one quantity at exact single-object resolution while another linked quantity is always rounded to a coarse grid.
- Biological or physical bounds are violated, or values cluster unnaturally at assay boundaries.

Caveats: automated instruments, exported software percentages, fixed acquisition templates, and preregistered rounding rules may justify high or irregular precision. Ask for raw instrument exports and analysis scripts.

## Author-Response And Corrected-Data Detectors

- Treat author replies, corrected spreadsheets, and newly supplied raw data as new evidence versions; compare them to the original source rather than replacing it.
- Test whether a correction introduces fresh anomalies, such as formula violations, impossible count/percentage compatibility, copied denominators, or new precision islands.
- Check provenance continuity: sample IDs, group labels, order, units, and row counts should map cleanly from original publication to response data.
- Escalate when a response explains one detector while contradicting another independent detector.

## Distribution Detectors

- Variance too small or too uniform across groups.
- Too-smooth trajectories in longitudinal data.
- Lack of expected outliers or biological noise.
- Rank-order similarity across groups.
- P-value bunching near 0.05.
- Benford checks only for data that plausibly spans orders of magnitude.

Caveats: distribution tests are triage signals. They are weaker than direct formula violations or exact cross-table reuse.

## Sample, Missingness, And Unit Detectors

- Reported n does not match visible/raw data count.
- Methods, figure legend, and supplement disagree on n.
- Missing values cluster in unfavorable groups or time points.
- Sample IDs skip, repeat, or change labels unexpectedly.
- Units or scales are mixed: percent vs fraction, log vs linear, mg vs g.
- Values fall outside biological or physical range.

## Evidence Escalation

Escalate when:

- The anomaly is reproducible from public files.
- The same paper has independent numeric and image anomalies.
- An author response or corrected file introduces new numeric inconsistencies.
- The anomaly concerns key experiments, main claims, funded deliverables, or representative works.
- Alternative explanations such as rounding, instrument resolution, or derived variables are incompatible with the data.
