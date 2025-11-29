# Module 03 — Guardrails  
## Change Log  
- Added strict evidence mode logic.  
- Added standardized missing/short warnings.  
- Strengthened hallucination prevention.  

---

## Purpose  
Ensure all summaries strictly follow evidence-based rules and no unsupported claims appear.

## Inputs  
- Section text  
- Proposed summary from Module 02  
- evidence_mode  
- PS2 constraints  
- Flags (missing, short)

## Logic  

### 1. Strict Evidence Mode  
If `evidence_mode = "strict"`:  
- Allow only claims explicitly found in the text.  
- Remove speculative or inferred content.  
- If insufficient data, output: "The source text does not provide enough detail to summarize this section in strict evidence mode."
- 
### 2. Missing Section Warning  
If the section has **no text**, output:"Section skipped: no usable text provided."

### 3. Short Section Warning (<50 words)  
If too short, append:"Section very short: summary may be incomplete."

### 4. Hallucination Checks  
Reject or modify content if it includes:
- Invented results  
- Invented citations  
- Invented equations  
- External knowledge  

## Outputs  
- Cleaned, validated summary  
- Standardized warnings  
- Flags for Rendering module  




