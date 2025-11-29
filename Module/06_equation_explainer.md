# /modules/06_equation_explainer.md

## Change Log

- **v1.0:** Initial creation of Equation Explainer module.

---

## Purpose

The Equation Explainer module detects mathematical equations in the provided paper text and provides plain-language interpretations, clarifying the roles and relationships of the variables and operations. It helps bridge the gap between formal math and conceptual understanding, for both expert and lay summaries.

---

## Input rules

- Receives:
  - Full paper text.
  - Section mapping from Intake & Setup.
- Focuses on sections that contain:
  - Displayed equations.
  - Inline mathematical expressions.
  - Textual descriptions referring to equations or formulae.

---

## Logic steps

1. **Detect equations**
   - Identify blocks or segments that correspond to mathematical expressions, such as:
     - Display equations (e.g., centered, with equation numbers).
     - Inline expressions containing operators, summations, matrices, etc.
   - Only treat something as an equation if it appears explicitly in the text.

2. **Group equations with context**
   - For each equation:
     - Collect surrounding explanatory sentences.
     - Identify where the text describes the role or meaning of the equation.

3. **Interpret in plain language**
   - Based on the nearby context:
     - Explain what the equation is computing.
     - Clarify how inputs, parameters, and outputs relate.
   - Use language suitable for:
     - Expert summaries (more technical).
     - Lay summaries (high-level and intuitive).
   - Do not add meaning that is not supported by the context.

4. **Clarify mathematical relationships**
   - Emphasize:
     - How variables interact (e.g., weighted sums, normalization, transformations).
     - How the equation fits into the overall method or architecture, if explicitly described.
   - If an equation is referenced but its full form is missing:
     - State that the equation is incomplete or not fully available.

5. **Validate with Guardrails**
   - Submit explanations to the Guardrails module.
   - Ensure that:
     - No new equations are introduced.
     - No speculative interpretations are added.

---

## Output rules

- Produce an internal collection of equation explanations, each including:
  - The equation (or a reference to it).
  - A brief expert-level explanation.
  - A brief lay-level explanation, where possible.
- These explanations are used to:
  - Enhance the Expert Summary with technical detail.
  - Optionally support the Lay Summary with simplified analogies or descriptions.
  - Provide glossary entries for key mathematical terms.

If no equations are detected:

- Produce an empty list.
- Trigger a warning in Checks & Warnings noting that no equations were found or available.

---

## Guardrails

- Do not:
  - Invent equations or fill in missing parts from prior knowledge.
  - Attribute meanings to symbols that are not supported by the surrounding text.
- Always:
  - Prefer incomplete but accurate explanations over complete-sounding but speculative ones.
  - Flag missing or incomplete equations as limitations in the final diagnostics.
