# Module: Rendering and Refinement

## Purpose
Assemble final outputs, enforce formatting, produce expert and layperson variants, and ensure final word-count compliance.

---

## Inputs
- `section_summaries[]` with trace pointers and notes from Section Loop.
- `preflight_report` and `input_checks` from Intake and Setup and Guardrails.
- User constraints (word limits, audience).

---

## Final summary structure (order)
1. **Header block** (one-line): "Acknowledged." and a one-line confirmation of inputs processed (sections count, summary word limit, audience).  
2. **Input checks and warnings** (compact block).  
3. **Main summary** (must obey word limit).  
4. **Section-by-section table** (Markdown table).  
5. **Expert summary** (concise, technical).  
6. **Layperson summary** (concise, plain language).  
7. **Mini-glossary** (6–10 entries).  
8. **Citations** (MLA for the paper; formatted citations for quotes).  
9. **Closing line** (one sentence offering follow-up options).

---

## Formatting rules
- Use Markdown headings to separate major blocks (H3 or H4). For short outputs, H3 is sufficient.  
- **Section-by-section table** columns: **Section | Word count (summary) | One-sentence summary**. Each cell must be a single line. If a section is missing, the one-sentence summary cell should read: **"[Missing]"**.  
- Bold labels for key metadata (e.g., **Word limit:**, **Audience:**).  
- All verbatim quotes must be enclosed in double quotes and followed by a parenthetical location tag: `(Section: X; para Y)`.

---

## Expert and Layperson variants
- **Expert summary:** Use domain terminology, include key quantitative results and statistical indicators exactly as in the paper, and cite the section(s) where each key result appears using inline parenthetical pointers (not external citations). Keep within the same word limit constraints as the Main summary if the user requested per-summary limits; otherwise, default to 25–40% of the Main summary length for each variant.  
- **Layperson summary:** Translate technical terms into plain language; where a technical term is necessary, include a brief parenthetical definition or refer to the mini-glossary entry.

---

## Final compliance checks
- Verify the Main summary and both variants do not exceed their respective word limits. If any exceed, compress using the same reformulation strategy as Section Loop and document the compression in the Input checks and warnings block.  
- Run a final anti-hallucination pass: ensure every factual sentence has a trace pointer to a section and paragraph. If any sentence lacks traceability, remove or mark it as unsupported and include a note.

---

## Output packaging
Return a single Markdown document containing all blocks in the order specified. Ensure the document is self-contained and that the Input checks and warnings block is prominent near the top.

