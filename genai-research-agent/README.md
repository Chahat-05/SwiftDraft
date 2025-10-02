# SwiftDraft

SwiftDraft is a Python-based multi-agent academic writing pipeline, leveraging Google ADK to orchestrate the drafting, enrichment, formatting, and compilation of scholarly articles. The system is designed for robust automation and modularity, enabling users to transform rough drafts into publication-ready PDFs in a variety of academic formats.

---

##  High-Level Architecture

### 1. Root Agent (`root_agent`)
- **Type:** Agent  
- **Model:** gemini-2.5-flash  
- **Role:** Orchestrator

**Responsibilities:**
- Accepts user input (draft + optional format request)
- Splits input:
  - Draft → `content_struct_agent`
  - Format request → `format_interpreter_agent`
- Passes outputs into the sequential pipeline (`seq_agent`)

---

### 2. Sequential Pipeline (`seq_agent`)
- **Type:** SequentialAgent  
- **Name:** ResearchPipelineAgent

**Flow:**
- **content_struct_agent**  
  Cleans and structures raw draft into Markdown + YAML metadata; preserves medical terms and inserts placeholders if missing.

- **content_enricher_agent**  
  Expands thin or incomplete content using authoritative sources (PubMed, WHO, guidelines), adds context, and inserts citation placeholders (`\cite{}`).

- **format_interpreter_agent**  
  Reads the format request (e.g., IEEE, APA, Vancouver, journal name), and produces a format schema (YAML) specifying sections, citation style, bibliography tool, and formatting constraints.

- **content_integrator_agent**  
  Aligns enriched Markdown with the format schema, ensures required sections appear in the correct order, adds placeholders for missing sections, and outputs metadata on the final structure.

- **latex_agent**  
  Converts integrated Markdown and schema into compile-ready LaTeX, adds the correct document class, citation packages, figures/tables, and calls the `latex_to_pdf` tool to compile the PDF.

---

### 3. Tools

- **latex_to_pdf**  
  Converts LaTeX string → PDF, returning the file path or an error message.

---



##  Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Chahat-05/SwiftDraft.git
   cd SwiftDraft
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
  

3. **Run:**
   ```bash
   adk web
   ```
  

