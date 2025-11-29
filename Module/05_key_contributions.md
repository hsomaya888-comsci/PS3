# /modules/05_key_contributions.md

## Change Log

- **v1.0:** Initial creation of Key Contributions Highlighter module.

---

## Purpose

The Key Contributions Highlighter module identifies and extracts the central contributions of the paper. It focuses on core contributions, novel ideas, improvements over baselines, and architecture innovations, strictly as stated in the source text.

---

## Input rules

- Receives:
  - Full paper text.
  - Section mapping and metadata from Intake & Setup.
- Especially examines sections likely to discuss contributions, such as:
  - Abstract
  - Introduction
  - Contributions
  - Results
  - Conclusion
- However, it must not assume such sections exist unless explicitly listed.

---

## Logic steps

1. **Locate contribution-rich passages**
   - Scan for phrases indicating contributions, such as:
     - “We propose…”
     - “Our contributions are…”
     - “We show that…”
     - “This paper introduces…”
   - Restrict search strictly to the provided text.

2. **Extract core contributions**
   - Identify:
     - **Core contributions:** Primary achievements or findings.
     - **Novel ideas:** New methods, concepts, or perspectives.
     - **Improvements over baselines:** Explicit statements comparing this work to baselines.
     - **Architecture innovations:** Changes or introductions to model architecture, as described.

3. **Normalize and clarify**
   - Paraphrase contributions for clarity while:
     - Maintaining strict fidelity to the original statements.
     - Avoiding new claims or extrapolations.
   - Consolidate redundant or overlapping contributions into concise items.

4. **Validate with Guardrails**
   - Submit identified contributions to the Guardrails module for strict evidence checking.
   - Remove or weaken any contribution that cannot be clearly traced to explicit text.

---

## Output rules

- Produce an internal list of contributions, each labeled as:
  - Core contribution.
  - Novel idea.
  - Improvement over baselines.
  - Architecture innovation.
- These summaries are used to:
  - Enhance the Overall Paper Summary.
  - Enrich the Expert Summary.
  - Inform the Lay Summary (with simplified phrasing).

If no contributions can be confidently identified:

- Produce an empty or minimal list.
- Trigger a warning to be surfaced in Checks & Warnings indicating that explicit contributions were not clearly stated.

---

## Guardrails

- Do not:
  - Infer contributions from implications or general knowledge.
  - Invent baselines, metrics, or architectural details not in the text.
- Always:
  - Prioritize explicit author statements.
  - Flag ambiguity and lack of clarity as limitations rather than resolving them through speculation.
