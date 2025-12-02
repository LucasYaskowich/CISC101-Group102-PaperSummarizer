# Module: Citation Extractor

## Purpose
Locate and extract exact in-text citations, quoted phrases, and bibliographic metadata from the supplied paper that are especially relevant to the summaries.

---

## Inputs
- Full paper text (as parsed by Intake and Setup).
- Section offsets and paragraph numbering.

---

## Extraction rules
1. **In-text citations and references**
   - Extract all in-text citations (author-year, numeric, or footnote markers) that are directly used in Results, Discussion, or Conclusion sections.
   - For each extracted citation, record: `citation_text` (exact string as it appears), `location` (section, paragraph number, line range), and `context_snippet` (up to 30 words surrounding the citation).

2. **Verbatim quotes**
   - Identify any phrases or sentences that are likely to be quoted in summaries (e.g., explicit claims, definitions, key results). Extract the exact quote and record its `location` and `surrounding_sentence_index`.

3. **Bibliographic metadata**
   - If the paper includes a title page or header with metadata (title, authors, journal, year, DOI), extract these fields exactly as presented.

4. **Traceability index**
   - Produce a compact index mapping each candidate summary sentence to the exact source location(s) that support it.

---

## Output
- `extracted_citations[]` with fields: `citation_text`, `location`, `context_snippet`.
- `extracted_quotes[]` with fields: `quote_text`, `location`.
- `paper_metadata` with fields: `title`, `authors[]`, `journal`, `year`, `volume`, `issue`, `pages`, `doi` (as available).
- `traceability_index` mapping summary sentence IDs to source locations.

---

## Notes
- Do not attempt to resolve incomplete bibliographic entries beyond what is present in the paper. If DOI or journal metadata are missing, leave fields blank and flag in the preflight report.

