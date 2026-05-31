# Evidence Object Schema

Use structured evidence so reports, human review, and future reruns stay reproducible.

## Minimal Evidence Object

```json
{
  "evidence_id": "paperid-fig4c-lastdigit-001",
  "type": "last_digit_anomaly",
  "severity": "medium",
  "paper": {
    "title": "",
    "doi": "",
    "source_files": [
      {
        "path_or_url": "",
        "source_type": "pdf|markdown|docx|spreadsheet|csv|image|raw_instrument_export|analysis_script|author_response|corrected_file|other",
        "source_version": "original|author_response|correction|reanalysis|unknown",
        "sha256": "",
        "downloaded_at": ""
      }
    ]
  },
  "location": {
    "file": "",
    "sheet": "",
    "page": null,
    "figure": "",
    "panel": "",
    "range": "",
    "rows": [],
    "columns": []
  },
  "claim": "",
  "method": {
    "detector": "",
    "parameters": {},
    "statistic": {},
    "software": ""
  },
  "raw_values": [],
  "alternative_explanations": [],
  "review": {
    "status": "needs_human_review",
    "reviewer_notes": ""
  }
}
```

## Report Structure

1. Paper and source file summary.
2. Methods used for extraction and detection.
3. Findings ordered by severity and reproducibility.
4. For each finding: location, raw values, detector logic, statistic, and caveats.
5. Alternative explanations considered.
6. Requests for authors/editors/institution: raw data, instrument exports, analysis scripts, or clarification.
7. Appendix: hashes, commands, environment, exported tables.

## Language Guardrails

- Prefer: "The public supplementary table contains..."
- Prefer: "This pattern is difficult to reconcile with..."
- Prefer: "Please provide the raw instrument export or analysis script..."
- Avoid: "This proves fabrication" unless an authorized investigation or explicit user context supports that wording.
