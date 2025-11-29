# /modules/02_section_loop.md

## Change Log

- **v1.0:** Initial creation of Section Loop module specification with `summary_level` logic.

---

## Purpose

The Section Loop module performs section-by-section summarization under PS2 constraints. It uses the `summary_level` variable to determine the granularity of each summary and ensures that all content is strictly grounded in the source text. It also respects missing/short section flags and contributes to the global word budget.

---

## Input rules

- **Inputs from Intake & Setup:**
  - Ordered section structures, each with:
    - Original and normalized section name.
    - Section text.
    - Flags: `is_missing`, `is_very_short`.
    - Approximate word count.
  - Audience type (`expert` or `lay`).
  - `summary_level` (`"short"` or `"detailed"`).
  - PS2 word budget info: `used_words`, `max_words = 500`.

- **Preconditions:**
  - No summarization occurs unless section mapping has been completed.
  - Strict evidence mode is assumed to be active by default.

---

## Key variable: `summary_level`

- **Definition:** A string specifying the desired level of detail.
- **Allowed values:**
  - `"short"`
  - `"detailed"`
- **Behavior:**
  - If `"short"`:
    - Produce a 1–2 sentence summary for each usable section.
  - If `"detailed"`:
    - Produce a short paragraph plus 3–5 bullet points for each usable section.

---

## Logic steps

1. **Iterate over sections in order**
   - For each section:
     - If `is_missing` is true:
       - Do not attempt a summary.
       - Mark the section with the standardized warning:
         - “Section skipped: no usable text provided.”
       - Summary content is set to `N/A` or equivalent.
       - Continue to the next section.

2. **Handle very short sections**
   - If `is_very_short` is true but text is present:
     - Still attempt a summary using the appropriate `summary_level` behavior.
     - Attach warning:
       - “Section very short: summary may be incomplete.”

3. **Generate section summary by `summary_level`**
   - For sections with usable text:
     - **If `summary_level == "short"`:**
       - Create a 1–2 sentence summary capturing the central, explicitly stated ideas of the section.
       - Avoid minor details and focus on main claims, methods, or findings as stated.
     - **If `summary_level == "detailed"`:**
       - Create:
         - One concise paragraph summarizing the section.
         - 3–5 bullet points highlighting key elements such as:
           - Main goals or questions.
           - Techniques or methods.
           - Key results or claims.
           - Limitations or future work, if explicitly present.
       - All points must be grounded in the section text.

4. **Validation against hallucination rules**
   - For each sentence and bullet point:
     - Check that the content can be directly traced to the section text.
     - Remove any speculative or inferred claims.
   - Do not:
     - Introduce external context or background.
     - Generalize beyond what the authors themselves state.

5. **Apply strict evidence mode (in collaboration with Guardrails)**
   - If the text is too sparse or vague to support a meaningful summary:
     - Replace the section summary text with:
       > “The source text does not provide enough detail to summarize this section in strict evidence mode.”
   - Attach this message in place of the normal summary content.

6. **Word budget management**
   - After drafting a section summary:
     - Estimate the number of words added.
     - Update `used_words`.
   - If adding the full draft would exceed `max_words = 500`:
     - Shorten the summary (fewer sentences, fewer bullets) while preserving accuracy.
     - If necessary, reduce some section summaries to an ultra-brief form (e.g., a single sentence).
   - If the budget is exhausted:
     - For remaining sections, do not summarize.
     - Mark them with a warning noting that the summary budget was exceeded.

---

## Output rules

For each section, this module produces an internal summary object containing:

- Section name (original and/or normalized).
- Final section summary text (structured according to `summary_level`).
- Any attached warnings, including:
  - “Section skipped: no usable text provided.”
  - “Section very short: summary may be incomplete.”
  - “The source text does not provide enough detail to summarize this section in strict evidence mode.”
  - Any budget-related limitations.
- Word count of the final summary for tracking.

These summaries are later used by Rendering & Refinement to build the Section-by-Section Table, Overall Paper Summary, Expert Summary, and Lay Summary.

---

## Guardrails

- No section may contain:
  - Claims not supported by the text.
  - Invented equations, baselines, or datasets.
  - Sections or subsections that the user did not list.
- If evidence is insufficient, the module must favor explicit strict-evidence messages over guesswork.
- The module must cooperate with the global 500-word budget; no exceptions.
