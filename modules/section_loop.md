# Module: Section Loop

## Purpose
Iteratively summarize each section, enforce constraints, and reformulate summaries until they meet word-count and boundary requirements.

---

## Inputs
- `sections[]` from Intake and Setup (each with `id`, `label`, `text`, `word_count`, `status`).
- Global constraints: maximum summary length (per user), tone, anti-hallucination rules.

---

## Workflow (for each section in order)
1. **Skip or flag**
   - If `status == MISSING`: produce no summary; add a one-line placeholder noting the missing section and the inability to summarize it.
   - If `status == TOO_SHORT`: produce a concise summary but mark it as derived from a short section and include the original word count.

2. **Initial summary generation**
   - Produce a **one-paragraph** summary of the section that:
     - Is strictly grounded in the section text.
     - Uses only information present in the section (no cross-section inference unless explicitly supported by text).
     - Contains no more than the section-specific word budget (see "Word budget allocation" below).

3. **Constraint checks**
   - Verify **fidelity**: every factual claim must be traceable to a phrase or sentence in the section. If a claim cannot be traced, remove or rephrase it to reflect uncertainty.
   - Verify **word count**: the section summary must not exceed its allocated word budget.
   - Verify **tone** and **format**: formal, academic register; no first-person.

4. **Reformulation loop**
   - If any check fails, iteratively reformulate the summary:
     - First, remove non-essential qualifiers or examples.
     - Second, compress sentences and prefer noun phrases.
     - Third, if still over budget, prioritize results and conclusions over background details.
   - Repeat until all checks pass or until three reformulation attempts have been made. If still failing, produce the best-compressed valid summary and add a one-line note explaining which constraint could not be fully met.

5. **Traceability mapping**
   - For each sentence in the section summary, record a minimal trace pointer: `section_id`, `paragraph_number` (or line range) from which the sentence was derived.

6. **Output per section**
   - `section_summary` (one-paragraph).
   - `summary_word_count`.
   - `trace_pointers[]` for sentences.
   - `notes` (e.g., short source, truncated source, reformulation attempts).

---

## Word budget allocation (recommended)
- If the user-specified maximum applies to the Main summary only, allocate section budgets proportionally to section word counts, with minimum 20 words per non-empty section.  
- If the user-specified maximum applies to each summary variant, set per-section budgets so that the combined section summaries will not exceed the Main summary limit. Document allocation decisions in the preflight report.

