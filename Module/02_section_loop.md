# Module 02 — Section Loop  
## Change Log  
- Added `summary_level` variable.  
- Implemented conditional short/detailed summary logic.  
- Added bullet list requirement for detailed mode.  
- Strengthened hallucination checks inside the loop.

---

## Purpose  
Generate section-level summaries using short or detailed modes while ensuring strict evidence and PS2 constraints.

## Inputs  
- Section text  
- summary_level  
- evidence_mode  
- PS2 constraints  
- Flags from Module 01

## Logic  

For each section in the ordered list:

1. **Check for missing text**  
   - If no text →  
     ```
     Section skipped: no usable text provided.
     ```

2. **Check for very short text (<50 words)**  
   - Add warning:  
     ```
     Section very short: summary may be incomplete.
     ```

3. **Apply summary mode**  
   - **If summary_level = "short":**  
     - Produce a 1–2 sentence summary.  
   - **If summary_level = "detailed":**  
     - Produce 1 short paragraph.  
     - Follow with 3–5 bullet points.

4. **Evidence-Based Filtering**  
   - Remove any claims not supported directly by the text.  
   - No invented details, baselines, or equations.

5. **Word-Budget Management**  
   - Track the 500-word PS2 limit.  
   - Shorten subsequent summaries if needed.

## Outputs  
- Summary (short or detailed)  
- Bullet points (detailed only)  
- Warnings for missing or short sections  

