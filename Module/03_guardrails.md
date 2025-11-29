# /modules/03_guardrails.md

## Change Log

- **v1.0:** Initial creation of Guardrails module with strict evidence mode and standardized warnings.

---

## Purpose

The Guardrails module enforces strict evidence-based behavior and standardized diagnostics across all other modules. It ensures that no hallucinations, invented sections, or invented equations appear in the summaries, and it provides consistent messages for missing or short sections.

---

## Input rules

- Receives intermediate outputs from:
  - Intake & Setup (section flags).
  - Section Loop (proposed summaries).
  - Key Contributions Highlighter (proposed contributions).
  - Equation Explainer (proposed explanations).
- Receives global configuration:
  - `evidence_mode` (default: `"strict"`).
  - PS2 word budget information.
- Guardrails logic is always active and must be applied to all content before rendering.

---

## A. Strict Evidence Mode (`evidence_mode = "strict"`)

**Mode definition:**

- When `evidence_mode` is `"strict"`, the system must use ONLY claims explicitly appearing in the text.
- No inferring, extrapolating, or drawing on external knowledge is allowed.

**Behavior:**

1. **Content validation**
   - For each proposed summary sentence, bullet point, or contribution:
     - Verify that it corresponds closely to a statement or combination of statements in the source text.
   - If any part is not clearly supported:
     - It must be rephrased into a weaker, directly supported statement or removed.

2. **Insufficient evidence handling**
   - If a section or topic has too little information to summarize without speculation:
     - Replace the summary content with the exact message:

       > “The source text does not provide enough detail to summarize this section in strict evidence mode.”

3. **Application scope**
   - Apply strict evidence checks to:
     - Section summaries.
     - Overall Paper Summary.
     - Expert Summary.
     - Lay Summary.
     - Key contributions.
     - Equation explanations.

---

## B. Short/Missing Section Warnings

**Standardized messages:**

- For sections with no usable text:
  - “Section skipped: no usable text provided.”
- For sections under 50 words or very sparse:
  - “Section very short: summary may be incomplete.”

**Behavior:**

1. **Missing sections**
   - When `is_missing` is true:
     - Inject “Section skipped: no usable text provided.” in:
       - The Section-by-Section Table `Notes / Warnings` cell.
       - The Checks & Warnings → Missing sections list.

2. **Very short sections**
   - When `is_very_short` is true:
     - Preserve any minimal summary if possible.
     - Append “Section very short: summary may be incomplete.” to:
       - The Section-by-Section Table `Notes / Warnings` cell.
       - The Checks & Warnings → Empty or very short sections list.

3. **Budget-related limitations**
   - If some sections are not summarized due to the 500-word PS2 limit:
     - Include an explicit warning stating that the word budget was reached, and some sections could not be summarized in detail.

---

## Additional guardrails

- **No invented sections or citations**
  - Ensure that:
    - Only user-supplied section names appear in outputs.
    - No extra sections are added.
    - No references or citations are fabricated.

- **No invented equations**
  - Verify that all equation references and explanations correspond to equations present in the source text.
  - If an equation is referenced but not shown, require explicit note that the equation is missing.

- **PS2 constraint enforcement**
  - Confirm that:
    - Total summary length ≤ 500 words.
    - Summaries avoid hallucinated content.
  - If any violation occurs, the Guardrails module must:
    - Direct the system to shorten or remove content.
    - Inject relevant warnings into Checks & Warnings.

---

## Output rules

- Approve, modify, or veto proposed content before it reaches rendering.
- Provide:
  - A list of approved summaries and explanations.
  - A list of standardized warnings to be surfaced in Checks & Warnings.
  - Status flags indicating whether all PS2 constraints are satisfied.

---

## Guardrails

- This module must never generate new scientific content; it only constrains and annotates.
- Guardrails take precedence over any stylistic or user-preference instructions when in conflict.
- The system must always err on the side of under-specifying rather than over-claiming.
