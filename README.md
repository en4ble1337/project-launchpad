# The Genesis System: Project Initialization Workflow

**Goal:** Transform a vague idea into a "Hallucination-Proof" codebase.

**Method:** A 3-Stage Pipeline (Product → Architecture → Scaffold).

**How to use:** Open a fresh AI chat (Claude/ChatGPT) and run these 3 prompts in order.

---

## Phase 1: The Product Owner (PRD Generator)

**What we are accomplishing:** We are forcing the "Concept" out of your head and into a rigorous specification. This uses your Lettered Q&A method to ensure the AI captures your intent before a single line of code is written.

### 👉 Copy/Paste Prompt 1:

https://github.com/en4ble1337/The-Genesis-System-Project-Initialization-Workflow/blob/main/prd_skill.md

## Phase 2: The Systems Architect (Context Generator)

**What we are accomplishing:** We are creating the "Map" for the project. This defines the Ubiquitous Language (so the AI doesn't invent terms) and the Technical Boundaries (so code goes in the right folder).

### 👉 Copy/Paste Prompt 2 (After PRD is generated):

```markdown
# Role: Principal Systems Architect

We have the approved PRD (above). Now we need to engineer the context for the development agents.
Your goal is to generate the content for `docs/ARCH.md`.

## The Output: `docs/ARCH.md`
Please write a technical document that covers:

1. **The Dictionary (Ubiquitous Language):**
   * Define specific domain terms (e.g., "What is a 'Session' vs a 'Connection'?").
   * This prevents "Conceptual Hallucinations."

2. **The Stack & Boundaries:**
   * **Frontend:** [Define Framework/Tools]
   * **Backend:** [Define Framework/Tools]
   * **Data:** [Define Storage]
   * **Infrastructure:** [Define Environment, e.g., Proxmox, Docker]

3. **Directory Map (Anti-Gravity Alignment):**
   * Confirm that `execution/` is for deterministic scripts.
   * Confirm that `directives/` is for active task instructions.
   * Confirm that `src/` is for application logic.

## Input Data
Use the `docs/PRD.md` content generated above as your source of truth.
```

---

## Phase 3: The DevOps Engineer (The Scaffolder)

**What we are accomplishing:** We are moving from "Text" to "Reality." This prompt writes a Python script that creates every folder and file defined in the previous steps, plus the master AGENTS.md file.

### 👉 Copy/Paste Prompt 3:

```markdown
# Role: DevOps Engineer (Scaffolder)

We are initializing the project. I need a Python script called `setup_genesis.py`.

## Requirements
The script must:
1. **Create Folders:**
   * `docs/`
   * `directives/`
   * `execution/`
   * `src/`
   * `tests/`
   * `.tmp/`

2. **Write Files:**
   * `docs/PRD.md`: Write the content we generated in Phase 1.
   * `docs/ARCH.md`: Write the content we generated in Phase 2.
   * `directives/001_initial_setup.md`: A starter file instructing the agent to install dependencies defined in ARCH.md.

3. **Write the Master Instruction File (`AGENTS.md`):**
   * Create `AGENTS.md` in the root.
   * **Content:**
       ```markdown
       # AGENTS.md - System Kernel

       ## 1. The Prime Directive
       You are an Anti-Gravity Agent.
       * **Read First:** Always read `docs/PRD.md` (What) and `docs/ARCH.md` (How) before coding.
       * **Test First:** No implementation without a failing test in `tests/`.

       ## 2. The 3-Layer Workflow
       * **Layer 1 (Directives):** Read `directives/` for orders.
       * **Layer 2 (Orchestration):** Plan your steps. Use `.tmp/` for scratchpads.
       * **Layer 3 (Execution):** Use Python scripts in `execution/` for repetitive tasks.

       ## 3. Definition of Done
       * [ ] Code in `src/`
       * [ ] Tests pass in `tests/`
       * [ ] PRD/Directive marked as Complete
       ```

4. **Write IDE Config:**
   * Create `.cursorrules` (or `.windsurfrules`) containing: "ALWAYS read AGENTS.md at the start of every session."

## Output
Provide ONLY the Python code block for `setup_genesis.py`.
```

---

## 🏁 Final Step: Execution

Copy the python code from Phase 3.

Run it locally:

```bash
mkdir my-new-project
cd my-new-project
python3 -c "[PASTE THE PYTHON CODE HERE]"
# OR save to file and run
# python3 setup_genesis.py
```

Open the project in your IDE. The system is now live, documented, and ready for development.
