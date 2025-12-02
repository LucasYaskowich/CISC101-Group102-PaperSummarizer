# Module: Intake and Setup

## Purpose
Normalize and validate incoming inputs, prepare the paper for section-wise processing, and detect missing or empty sections.

---

## Inputs to this module
- Raw paper text or machine-readable file content.
- User-provided list of sections.
- Desired maximum summary length (word count).
- Target audience.

---

## Steps
1. **Acknowledge and confirm** receipt of all required inputs. If any required input is missing, return a single-line request for the missing item(s) and halt further processing.

2. **Normalize section headings**
   - Parse the paper for headings using common markers (bold lines, numbered headings, "Abstract", "1.", "I.", etc.).  
   - Map found headings to the user-provided list by fuzzy matching (case-insensitive, punctuation-insensitive). Produce a mapping table: **Provided label → Detected heading(s)**.

3. **Detect missing or empty sections**
   - For each section in the normalized list:
     - If no corresponding text is found, mark **MISSING**.
     - If text exists but contains fewer than **30 words**, mark **POTENTIALLY TOO SHORT**.
     - If text exists and is non-empty, extract the section text and compute its word count.

4. **Context window assessment**
   - Estimate total token count of the paper. If estimated tokens exceed the model's safe processing capacity, apply the prioritization policy:
     - Priority order: Abstract, Introduction, Results, Discussion/Conclusion, Methods, Other sections, References.
     - Truncate or summarize low-priority sections for internal processing only; record which sections were truncated and by how much.

5. **Summary level assessment**
     - Based on the inferred needs of the audience, assign a value "short", "normal", or "detailed" to `summary_level` designated by how detailed the summary needs to be.
    
6. **Produce preflight report**
   - Short structured block listing:
     - **Normalized sections mapping** (one line per mapping).
     - **Missing or empty sections** (one line per issue).
     - **Sections flagged as short** (one line per issue).
     - **Context window action** (if any): which sections were truncated or deprioritized.

7. **Pass forward**
   - Provide the cleaned section texts and metadata (section id, start/end offsets, word counts, summary level) to the Section Loop module.

---

## Output (to next module)
- `sections[]` with fields: `id`, `label`, `text`, `word_count`, `status` (OK / MISSING / TOO_SHORT / TRUNCATED), `summary_level` (short / normal / detailed).
- `preflight_report` (short block for user-facing Input checks and warnings).
