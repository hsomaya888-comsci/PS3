# Research Paper Summarizer — Group 130

This repository contains our full summarizer system generated from the Week 12 meta-prompt for CISC 101.

## 📁 Directory Structure

system_prompt.md
modules/
01_intake_setup.md
02_section_loop.md
03_guardrails.md
04_rendering_refinement.md
05_key_contributions.md
06_equation_explainer.md

## 📘 Description

### system_prompt.md
Contains the complete system-level instructions for the Research Paper Summarizer, including:
- Required user inputs  
- Guardrails against hallucinations  
- Summary-level logic  
- Output structure  
- All PS2 constraints  
- High-level modular architecture  

### modules/
Each file in this folder implements a specific stage of the summarization pipeline:

- **01_intake_setup.md** — Normalizes inputs, maps text to sections, applies PS2 constraints  
- **02_section_loop.md** — Iterates through sections and applies short/detailed summary logic  
- **03_guardrails.md** — Enforces strict evidence mode and warning rules  
- **04_rendering_refinement.md** — Assembles the final Markdown output structure  
- **05_key_contributions.md** — Extracts text-supported contributions  
- **06_equation_explainer.md** — Identifies and explains equations in plain language  

## 👥 Group Members
- Elisabeth Feng  
- Samantha Zhou  
- Somaya Hassan  

## 📄 Purpose of the Project
This repository implements a complete prompt-engineered Research Paper Summarizer system that integrates:
- Specification design  
- Modular prompting  
- Hallucination mitigation  
- Conditionals  
- Summary-level modes  
- Multi-audience rendering  

This submission fulfills the full requirements for PS3 Task 1, Task 2, and Task 3.
