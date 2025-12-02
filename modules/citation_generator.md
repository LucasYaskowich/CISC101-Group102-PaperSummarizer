# Module: Citation Generator

## Purpose
Produce an MLA-style citation for the paper and formatted citations for each verbatim quote identified by the Citation Extractor.

---

## Inputs
- `paper_metadata` from Citation Extractor.
- `extracted_quotes[]` and `extracted_citations[]`.

---

## MLA citation rules (applied to the paper)
- Use the MLA 9 format for journal articles when metadata is available:
  **Author(s). "Title of Article." Title of Journal, vol. X, no. Y, Year, pp. start–end. DOI.**
- If the paper is a preprint or technical report, adapt to MLA conventions for reports or online sources, including URL or DOI when present.
- If metadata fields are missing, include only the available fields and mark the citation as **incomplete** in a one-line note.

---

## Quote citations
- For each verbatim quote extracted, produce a short citation line in this format:
  **"Quote excerpt" — (Author(s), "Section Name", para N).**
- If author names are unavailable, use the paper title short-form in place of the author.

---

## Output
- `mla_citation` — single-line MLA citation for the paper.
- `quote_citations[]` — one-line formatted citation for each quote with precise location.
- `citation_notes` — one-line notes about any missing metadata or incomplete citation fields.

---

## Example outputs (format templates)
- **MLA (journal):** Smith, Jane, and John Doe. "Title of Article." *Journal Name*, vol. 12, no. 3, 2024, pp. 45–62. https://doi.org/xxxxx.  
- **Quote citation:** "We observe a significant increase in X" — (Smith and Doe, "Results", para 4).

