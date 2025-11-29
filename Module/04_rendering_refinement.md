# /modules/04_rendering_refinement.md

## Change Log

- **v1.0:** Initial creation of Rendering & Refinement module for final Markdown output.

---

## Purpose

The Rendering & Refinement module assembles the final user-facing Markdown output. It integrates section summaries, expert and lay interpretations, glossary entries, and diagnostics, while ensuring consistency with PS2 constraints and all guardrails.

---

## Input rules

- Receives:
  - Section summaries and per-section warnings from Section Loop.
  - Guardrail-validated content and warnings from Guardrails.
  - Key contribution list from Key Contributions Highlighter.
  - Equation explanations from Equation Explainer.
  - PS2 constraint status, including word budget usage.

---

## Logic steps

1. **Overall Paper Summary**
   - Synthesize a concise overview from:
     - Section summaries.
     - Key contributions.
   - Avoid introducing any new claims or interpretations.
   - Produce 1–3 short paragraphs.

2. **Section-by-Section Table**
   - Create a Markdown table with columns:
     - `Section`
     - `Summary`
     - `Notes / Warnings`
   - For each section:
     - Use its final summary (short or detailed).
     - Insert standardized warnings (e.g., missing, very short, strict evidence messages).
   - Ensure order matches the user’s original section list.

3. **Expert Summary generation**
   - Use content validated by Guardrails and enriched by:
     - Key contributions.
     - Equation explanations (technical details).
   - Write 1–3 paragraphs targeting an expert audience:
     - Emphasize methodology, architecture, and explicit comparisons.
   - Do not exceed global word budget; if necessary, prioritize precision over length.

4. **Lay Summary generation**
   - Re-express key ideas in accessible language:
     - Focus on the problem, main idea, and significance.
   - Simplify or omit technical details that are not essential for understanding.
   - Ensure no dramatization or external claims.

5. **Mini-Glossary generation**
   - Identify key terms from:
     - Section summaries.
     - Equation explanations.
     - Contribution highlights.
   - For each term:
     - Provide a concise, evidence-based explanation.
   - Present as a Markdown list.

6. **Checks & Warnings assembly**
   - Organize warnings into subsections:
     - Missing sections.
     - Empty or very short sections.
     - PS2 constraint checks.
     - Other limitations (e.g., incomplete equations, undefined terms).
   - Include any strict-evidence messages and budget-related warnings.

7. **Final consistency and constraint checks**
   - Verify:
     - All required top-level sections are present, in this exact order:
       1. Overall Paper Summary  
       2. Section-by-Section Table  
       3. Expert Summary  
       4. Lay Summary  
       5. Mini-Glossary  
       6. Checks & Warnings  
     - Markdown syntax is valid.
     - Total summary length is within 500 words.
   - If length exceeds 500 words:
     - Shorten sections (especially the Expert and Lay summaries) while maintaining correctness.
   - Confirm that:
     - No new sections or equations have been introduced.
     - All standardized warnings appear where appropriate.

---

## Output rules

- Produce a single Markdown document containing:

  1. **Overall Paper Summary**
  2. **Section-by-Section Table**
  3. **Expert Summary**
  4. **Lay Summary**
  5. **Mini-Glossary**
  6. **Checks & Warnings**

- Ensure consistent heading styles and list formatting.
- Ensure that warnings and constraints are clearly visible and understandable to the user.

---

## Guardrails

- The module must not:
  - Introduce new factual content beyond validated summaries and analyses.
  - Re-order or omit required top-level sections.
- The module must:
  - Respect PS2 constraints and strict evidence rules.
  - Explicitly surface any known limitations or constraint violations in Checks & Warnings.
