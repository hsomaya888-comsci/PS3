# /modules/01_intake_setup.md

## Change Log

- **v1.0:** Initial creation of Intake & Setup module specification.

---

## Purpose

The Intake & Setup module standardizes and validates user inputs before any summarization occurs. It normalizes section names, maps text to sections, detects missing and short sections, and initializes PS2 constraints for downstream modules. It ensures that all subsequent processing is grounded in a clean, well-structured representation of the paper.

---

## Input rules

- **Required inputs:**
  - **Full paper text:** The complete text (or all available sections) of the paper.
  - **Ordered list of section names:** Provided explicitly by the user.
  - **Audience type:** Must be either `expert` or `lay`.
  - **Summary level:** Must be either `"short"` or `"detailed"`.

- **Validation:**
  - If any required input is missing or ambiguous, the module must request clarification before proceeding.
  - The module must not assume default values for section names, audience type, or summary level.

---

## Logic steps

1. **Normalize section names**
   - **Trim and clean:**
     - Remove leading/trailing whitespace.
     - Collapse repeated spaces.
   - **Standardize casing:**
     - Optionally convert to a consistent casing (e.g., title case) for internal use while preserving the original for display.
   - **Deduplicate:**
     - If duplicate section names appear, preserve order but internally disambiguate (e.g., track positional index).

2. **Map text to sections**
   - Use the user’s ordered section list as the canonical structure.
   - Associate each section with the corresponding segment of text.
   - If the user has already segmented the text by section, adopt that mapping directly.
   - If mapping is unclear, flag sections as “mapping ambiguous” for potential warning.

3. **Detect missing sections**
   - For each section in the ordered list:
     - If no text is associated, mark the section as **missing**.
     - Record this status for use in:
       - Section-by-Section Table.
       - Checks & Warnings.

4. **Detect empty or very short sections**
   - For each section with text:
     - Estimate the word count.
     - If the word count is 0 → classify as **missing**.
     - If the word count is >0 but <50 → classify as **very short**.
   - Record:
     - `is_missing` (boolean).
     - `is_very_short` (boolean).
     - Approximate word count.

5. **Initialize PS2 constraints**
   - Set an internal maximum of 500 words for all summaries combined, per PS2.
   - Initialize a word usage tracker with:
     - `used_words = 0`
     - `max_words = 500`
   - Ensure all downstream modules are aware of this constraint.

6. **Prepare internal data structure**
   - For each section, create a structure including:
     - Original section name.
     - Normalized name.
     - Associated text.
     - Flags: `is_missing`, `is_very_short`, `mapping_ambiguous`.
     - Approximate word count.

---

## Output rules

The module outputs an internal representation (not shown verbatim to the user) that includes:

- **Global metadata:**
  - Audience type.
  - Summary level.
  - PS2 word budget information (`max_words = 500`).

- **Section list:**
  - Ordered list of section objects with:
    - Original and normalized names.
    - Full text for that section.
    - Flags indicating missing or very short sections.
    - Word counts and mapping status.

This structure is consumed by downstream modules (Section Loop, Guardrails, Rendering & Refinement, etc.).

---

## Guardrails

- The module must not:
  - Attempt to summarize any content.
  - Infer missing text or invent sections.
  - Alter or correct the textual content of the paper.
- The module must:
  - Maintain a clear separation between user-provided text and derived metadata.
  - Preserve the ordered list of sections exactly as provided.
  - Ensure that PS2 constraints (especially the 500-word limit and no hallucinations) are initialized for use by all other modules.
