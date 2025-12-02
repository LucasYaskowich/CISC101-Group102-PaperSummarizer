Change log: 
2/12/25: added accommodation for evidence mode
2/12/25: added section warning messages

# Module: Guardrails

## Purpose
Provide rules and fallback behaviors to prevent hallucination, handle edge cases, and manage context-window limitations.

---

## Anti-hallucination rules
- **Source-only policy:** Do not introduce any fact, number, or interpretation not present in the supplied paper. If a claim seems implied but not explicit, mark it as **inferred** and include the exact source phrase that supports the inference. Prefer quoting the source phrase when ambiguity exists.
- **Quote policy:** Limit verbatim quotes to short excerpts (≤ two lines). Each quote must be accompanied by a precise location (section and paragraph number or line range).
- **Uncertainty preservation:** If the source uses hedging language, preserve hedging in the summary (e.g., "authors report", "data suggest", "may indicate").

---

## Handling missing or very short sections
- **Missing sections:** Do not fabricate. Insert a placeholder summary line: **"[Section X missing in source; summary unavailable]"** and include a recommended action (e.g., "Provide the missing section or full paper to enable summarization").
- **Very short sections (<30 words):** Summarize verbatim if appropriate, but flag as **POTENTIALLY INCOMPLETE** and include the original text in the Input checks and warnings block.
- **References-only sections:** Do not attempt to summarize references as content. Extract citations only via the Citation Extractor module.

---

## Context-window and truncation handling
- **Prioritization:** If the paper exceeds processing capacity, prioritize Abstract, Results, Discussion/Conclusion, then Methods, then Introduction, then other sections. Document any truncation in the preflight report.
- **Truncation transparency:** For any truncated section, include a one-line note in that section's summary: **"[Section truncated for processing; full text may contain additional details]"**.
- **Chunking strategy:** When necessary, chunk long sections into contiguous sub-blocks and summarize each chunk separately in the Section Loop, then merge chunk summaries with explicit trace pointers.

---

## Verification and cross-checks
- After section summaries are produced, run a **consistency pass**:
  - Ensure that the Main summary does not assert anything contradicted by any section summary.
  - Ensure numeric values are consistent across summaries; if discrepancies exist in the source, report them verbatim with locations.
- If contradictions or ambiguous claims are detected in the source, include a one-line note in the Input checks and warnings block describing the contradiction and its locations.

---

## Error escalation
- If the module detects that more than 30% of required sections are missing or truncated such that a faithful summary is impossible, abort summarization and return only the Input checks and warnings block with explicit next steps.

---

## Strict Evidence mode 
- If `evidence_mode` = "strict", then the summarizer should only include information that appears in the provided texrt, nothing else. If it cannot find enough information, it must output the following message: "The text does not provide enough to summarize this section in strict summary mode

---

## Section warning messages
- If a given section is missing, empty, or too short, the module must output the following warning: "Section too short: summary may not be complete."

  
