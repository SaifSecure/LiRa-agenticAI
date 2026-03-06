# LiRa agenticAI

Agentic application for systematic literature review built based on the LiRA (Literature Review Agent) documentation.

## The 6-Phase Agentic Workflow

### Phase 1: Research Question & Scope Agent
**Goal:** Assist researchers in formulating a defined and original research question.
- **Framework Integration:** Incorporate structured frameworks like PICO, PICOC, SPIDER, SPICE, and PEO into the agent’s prompting strategy.
- **Constraint Checks:** Build sub-agents to automatically estimate feasibility (too broad/too narrow) and verify originality by summarizing existing review papers.
- **Output:** A well-structured, novel, and highly targeted research question.

### Phase 2: Search Strategy & Query Agent
**Goal:** Translate the research question into highly optimized, database-specific searches.
- **Query Translation Expansion:** An agent that generates synonyms, logical expressions (AND/OR), and syntax explicitly formatted for IEEE, Scopus, ACM, and Web of Science.
- **Criteria Generation:** Automatically draft inclusion and exclusion criteria based on the selected framework guidelines (e.g., PRISMA).
- **Automation Execution:** Output PRISMA-compliant search logs so the system can ingest raw database exports.

### Phase 3: Multi-Agent Screening & Deduplication
**Goal:** Filter down the raw dataset rigorously to find the final "Included Studies".
- **Deduplication:** A traditional data-processing pipeline (`clean_duplicate.py`) to systematically remove exact and fuzzy duplicates.
- **Agentic Pre-screen:** An LLM Agent that evaluates Titles & Abstracts against the inclusion/exclusion criteria to append an `included_label` (0 or 1).
- **Multi-Agent Verification Loop (Improvement):** Implement a "Reviewer", "Critic", and "Verifier" agent trio to debate the inclusion of edge-case papers before escalating to human review (ASReview integration).

### Phase 4: Data Augmentation & Automated Extraction
**Goal:** Pull both structured metadata and deep thematic concepts from the screened papers.
- **Metadata Scripting:** Extract traditional metrics (top authors, citations, institutions, keywords).
- **LLM Thematic Augmentation:** Agents dynamically read abstracts/methods to extract complex dimensions (e.g., algorithm type, network topologies) turning unstructured text into a highly structured `augmented_dataset.csv`.
- **Knowledge Graph Integration:** Build relationships between extracted entities (Authors → Methods → Limitations).

### Phase 5 & 6: Synthesis, Drafting, and Gap Identification
**Goal:** Read the full text and synthesize the findings into a cohesive Literature Review draft.
- **Hierarchical Outline Generation:** Generate a structured table of contents tailored to the thematic outcomes of Phase 4.
- **Agentic RAG (Retrieval-Augmented Generation):** An agent specifically reads the curated PDFs and drafts the review section by section.
- **Gap Identification:** Compare current agent findings with known limitations in the field to outline future research trajectories.

---

## Architectural & System Enhancements to Build

Beyond the 6 steps, the following system-level features will be implemented using free and open-source resources:

1. **Agentic Feedback loops:** We will implement the proposed multi-agent system (`Reviewer`, `Critic`, `Verifier`) to ensure high-quality accuracy without sole reliance on one LLM call.
2. **Prompt Engineering Repository:** A modular catalog of optimized system prompts for each distinct task.
3. **Workflow Orchestration:** Utilize an agentic framework (e.g., LangChain/Smolagents) or a pipeline orchestrator to allow these 6 steps to run as a continuous, state-aware flow.
4. **Adapter Pattern for Data formats:** Ensure inputs from IEEE, Scopus, and WoS can all map into the standard `raw_dataset` schema gracefully.
