### System Protocol: The Genesis Workflow

We use a strict **3-Phase Initialization** process to prevent hallucination and ensure architectural alignment before coding begins.

> [!NOTE]
> Check out the complete [How to Workflow Guide](https://github.com/en4ble1337/The-Genesis-System-Project-Initialization-Workflow/blob/main/how_to_workflow.md) for detailed instructions on using this system effectively.

#### Phase 1: The Product (Intent Extraction)
> [!NOTE]
> **Prompt File:** [phase1.md](https://github.com/en4ble1337/The-Genesis-System-Project-Initialization-Workflow/blob/main/phase1.md)


* **Role:** Product Manager
* **Input:** User's raw idea + Interactive Interview (Lettered Questions).
* **Output:** `docs/PRD.md`
* **Why it matters:** It converts **Ambiguity** into **Specifications**. By forcing the user to answer clarifying questions (A/B/C) *before* generation, we establish a concrete Definition of Done and verifiable User Stories. This prevents "feature creep" during development.

#### Phase 2: The Architecture (Context Mapping)
> [!NOTE]
> **Prompt File:** [phase2.md](https://github.com/en4ble1337/The-Genesis-System-Project-Initialization-Workflow/blob/main/phase2.md)

* **Role:** Systems Architect
* **Input:** The approved `docs/PRD.md`.
* **Output:** `docs/ARCH.md`
* **Why it matters:** It converts **Specifications** into **Constraints**. It defines the Ubiquitous Language (dictionary), Technical Boundaries, and Stack choices. This ensures the Agent doesn't invent terms or pick random libraries later; it creates the "Map" the agent must follow.

#### Phase 3: The Scaffolding (Deterministic Build)
> [!NOTE]
>**Prompt File:** [phase3.md](https://github.com/en4ble1337/The-Genesis-System-Project-Initialization-Workflow/blob/main/phase3.md)

* **Role:** DevOps Engineer
* **Input:** `docs/PRD.md` + `docs/ARCH.md`.
* **Output:** `setup_genesis.py` (which creates the file tree, `AGENTS.md`, and config files).
* **Why it matters:** It converts **Constraints** into **Reality**. Instead of manually creating files, we use a script to physically instantiate the "Anti-Gravity" folder structure (`docs/`, `directives/`, `execution/`, `src/`). This ensures the environment matches the documentation 1:1 from Day 0.

---

### Summary for the Agent

**"Phase 1 defines WHAT we build. Phase 2 defines HOW we structure it. Phase 3 creates the ENVIRONMENT to build it."**
