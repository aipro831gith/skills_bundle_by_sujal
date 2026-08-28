# ANTIGRAVITY ENTERPRISE HANDBOOK
## The Complete 10-Module Staff Manual for Google Antigravity 2.0

**Version:** 1.0 | **Date:** August 2026
**For:** Software development teams using the Skills Bundle by Sujal on Google Antigravity

---

> **How to read this handbook:**
> Plain English first. No jargon without explanation. If a word sounds technical,
> we explain it right away using a real-world comparison.
> Technical depth is preserved — we just make it accessible to everyone.

---

# MODULE 01: What Is Antigravity 2.0?

## The Simple Explanation (ELI10 Version)

Imagine you're running a kitchen restaurant. You (the Boss) sit at the front counter and take orders from customers. You don't cook — you have a **Head Chef** who manages the kitchen. The Head Chef has **specialist cooks**: one makes appetizers, one makes the main course, one makes dessert, one checks quality, one handles packaging.

**Google Antigravity 2.0** is the restaurant, but instead of food, it makes software. You tell the AI what product you want to build. The AI team — led by a "Team Leader" — figures out the technical details, writes the code, checks it works, secures it, and prepares it for launch.

You never have to write a single line of code if you don't want to. You just describe what you want, review the progress, and approve each stage.

## The Technical Explanation

**Google Antigravity 2.0** is a desktop Electron application for **multi-agent AI orchestration**. It lets you:

- Run multiple AI sub-agents concurrently on complex, multi-file software projects
- Monitor agent activities through an **auxiliary pane** (Subagents, Background Tasks, Artifacts, Files Changed, Terminals)
- Schedule **recurring Cron jobs** and **one-time timer tasks** for background automation
- Manage **workspace projects** — switch between different repositories with preserved context
- Configure **skills, rules, MCP servers, and hooks** for specialized agent behaviors

### Two Surfaces, One Ecosystem

| Surface | What It Is | Best Use |
|---------|-----------|---------|
| **Antigravity 2.0** | Standalone Electron desktop app | Long-running agent fleets, background task monitoring, project management |
| **Antigravity IDE** | AI-first VS Code fork | Real-time code editing, inline AI assistance, compiler-integrated fixes |

Both surfaces share the same underlying agent capabilities and read the same `.agents/` customization folder. They are fully compatible — you can switch between them freely.

### The Agent Model (How AI Decisions Are Made)

Antigravity uses **Google's Gemini model family**. In Settings, you choose the active model (Gemini Flash, Gemini Pro, or others as they become available). The model reads your skills, rules, and conversation history to take actions.

**Important:** The model does NOT automatically know your codebase. It learns about your project through:
1. Files you `@mention` in chat
2. Skills it loads from `.agent/skills/`
3. Rules from `AGENTS.md` / `GEMINI.md`
4. MCP tools that give it access to databases, GitHub, etc.

---

# MODULE 02: The Antigravity IDE — Your AI-Powered Editor

## The Simple Explanation

The Antigravity IDE is like a very smart text editor that's always watching what you type and offering to help. It's built on top of VS Code (the most popular code editor in the world), so it looks familiar, but it has three extra AI superpowers built in.

## Three AI Modalities (Three Ways to Get Help)

### Mode 1: Tab Autocomplete — "Suggestion Machine"
*Passive: AI suggests, you decide*

As you type code, the AI silently watches and suggests what comes next — similar to Google search's autocomplete, but for code. It considers:
- What you've typed so far
- Other files you have open
- Your terminal output
- Your clipboard (optional)

**To use:** Just type. A grey suggestion appears. Press `Tab` to accept, `Esc` to reject, `Ctrl+→` (Windows) / `⌘+→` (macOS) to accept one word at a time.

**Supercomplete:** For larger changes (like refactoring a whole function), the AI shows a floating window with a proposed "diff" — the red/green before-and-after view.

### Mode 2: Inline Command — "Highlight and Fix"
*Instructive: you tell it exactly what to change*

Highlight any block of code, then press `Ctrl+I` (Windows) / `⌘+I` (macOS). A small text box appears. Type your instruction:
- `"Add input validation to this function"`
- `"Write a docstring for this class"`
- `"Refactor this to use async/await"`

The AI edits only the highlighted block. Everything outside stays untouched.

**No selection:** Press `Ctrl+I` without highlighting anything to generate brand-new code at your cursor position.

### Mode 3: Sidebar Chat + Agent — "Your Pair Programmer"
*Collaborative: multi-step tasks with full workspace access*

The most powerful mode. Open the Sidebar Chat and talk to the AI like a colleague. In **Agent Mode**, it can:
- Read and write multiple files across your project
- Run terminal commands (build, test, install packages)
- Search the web for documentation
- Use MCP tools (database queries, GitHub API, security scans)
- Plan multi-step tasks and execute them autonomously

**Planning Mode:** Before the agent does anything, it shows you a step-by-step plan. You can review, modify, or reject steps before execution begins.

## IDE-Specific Features

### Code Lenses
Small clickable buttons appear directly above your functions and classes:
- "Refactor" — rewrite this function better
- "Write Tests" — generate unit tests for this function
- "Explain" — plain-English explanation of what this does

### Visual Diff Overlays
When the agent proposes edits, red/green highlights appear directly in your code — like "Track Changes" in Word. You review each change and click Accept or Reject per line.

### Diagnostic Auto-Fix
See a red squiggle under an error? Click it → a lightbulb appears → click "Fix with Antigravity". The agent reads the full error context and writes a targeted fix.

### Workspace-Scoped Customizations
The IDE automatically discovers your `.agents/` folder and loads:
- Project-specific rules (`AGENTS.md`)
- Custom skills (`.agent/skills/*.md`)
- MCP server connections (`mcp_config.json`)
- Lifecycle hooks (`hooks.json`)

---

# MODULE 03: MCP Architecture & Real-Time Tooling

## The Simple Explanation

Think of **MCP (Model Context Protocol)** as a plug-and-socket system for your AI agent. Your agent is the lamp — it needs electricity (tools) to work. MCP is the electrical socket standard that lets any tool provider "plug in" to your agent safely.

Instead of the agent only knowing how to read/write files, with MCP it can also:
- Query your database (ask: "how many users signed up today?")
- Talk to GitHub (ask: "create a pull request")
- Run a security scan (ask: "are there any SQL injection vulnerabilities?")
- Search the web (ask: "what does this npm package do?")

## Three Transport Types

MCP servers connect to the agent via three communication methods:

| Transport | How It Works | Best For |
|-----------|-------------|---------|
| **stdio** | The agent spawns a local process and talks through standard input/output | Local tools (filesystem, local DB, CLI tools) |
| **sse** | Server-Sent Events — a server pushes updates to the agent over HTTP | Remote servers that push real-time events |
| **streamableHttp** | Standard HTTP with streaming response support | Cloud-hosted MCP servers, enterprise APIs |

**This repository uses `stdio` for all MCP servers** — they run as local processes managed by the agent. This is the most common and most secure option.

## Tool Execution Policies — The Safety System

Every MCP tool falls into one of three safety categories:

| Policy | What It Means | Example |
|--------|--------------|---------|
| **always-proceed** | Agent runs the tool without asking you first | `SELECT * FROM users` (read-only) |
| **request-review** | Agent shows you what it's about to do and waits for your "OK" | `DROP TABLE payments` (destructive) |
| **proceed-in-sandbox** | Agent runs the command inside an isolated container — if it breaks, it doesn't affect your system | `npm install` (might download malware) |

## MCP Servers in This Repository

```
.agents/mcp_config.json defines these 6 servers:

1. filesystem-workspace    — reads/writes files in your project (always-proceed)
2. postgres-analytics      — queries your analytics database (SELECTs: proceed; writes: review)
3. github-enterprise       — manages GitHub repos, PRs, branches (reads: proceed; writes: review)
4. docker-sandbox          — runs build/test commands in isolated container (proceed-in-sandbox)
5. sast-security-scanner   — semgrep SAST scan for vulnerabilities (always-proceed, read-only)
6. brave-search            — web search for research tasks (always-proceed, read-only)
```

## Setting Up MCP: Step-by-Step

1. Copy `.agents/mcp_config.json` (already in this repository — nothing to copy).
2. Set required environment variables in your `.env` file (see `README.md` → Environment Setup).
3. Open Antigravity 2.0 → **Skills & Customizations** in left sidebar → verify MCP servers appear.
4. In chat, type `@mcp` to see all available MCP tools.

---

# MODULE 04: Skills Taxonomy & Progressive Disclosure

## The Simple Explanation

A **skill** is a recipe card for the AI. Instead of the AI guessing how to do something, you give it a detailed playbook: "Here's exactly how to build a backend API — step 1, step 2, step 3…"

**Progressive Disclosure** is the AI's memory-saving trick. Imagine a library with 1,000 recipe cards. You can't read all 1,000 at once — that would overwhelm anyone. So instead, only the recipe *titles* are always available. When you need a specific recipe, you pull that card out and read it in full.

Antigravity works the same way with skills:
- **Always loaded:** Skill names and one-line descriptions
- **Loaded on demand:** Full skill content (only when you activate it)

## Skill File Structure

Every skill in this repository is a Markdown file with:

```markdown
---
name: skill-name-here          ← Short name (used for discovery)
description: One sentence...   ← ALWAYS INJECTED into context (never evicted)
---

# ROLE: What this agent does
## 1. CORE DIRECTIVE & SINGLE RESPONSIBILITY
## 2. PREREQUISITES & ENTRY GATES
## 3. STEP-BY-STEP EXECUTION PROTOCOL
## 4. STRICT TECHNICAL & SECURITY CONSTRAINTS (HARD RULES)
## 5. MANDATORY MACHINE-READABLE ARTIFACTS / OUTPUTS
## 6. VERIFICATION & EXIT GATES (COMMANDS & CRITERIA)
## 7. ERROR HANDLING & ESCALATION MATRIX
```

The YAML frontmatter (`---` block) is what Antigravity reads first. The `description` field is the only part guaranteed to be in the agent's memory at all times.

## Skill Discovery Process

When you open a project in Antigravity 2.0 or Antigravity IDE, the platform automatically:

1. Walks from your current directory up to the `.git` root
2. Finds `.agents/` (or `.agent/`, `_agents/`, `_agent/`) folders
3. Reads all `SKILL.md` files in `skills/` subfolders
4. Injects only the `name` and `description` from each skill's frontmatter
5. Makes the full skill body available for on-demand activation

## Loading Priority (Highest to Lowest)

When two skills have the same name, this order determines which one wins:

1. **Workspace** `.agents/` folder (your project — highest priority)
2. **Declared** configurations in `skills.json` / `plugins.json`
3. **Global** discovery at `~/.gemini/config/`
4. **Built-in** Antigravity default skills
5. **Global declared** configurations

**Practical implication:** You can override any built-in Antigravity skill by creating a skill with the same name in your project's `.agents/` folder.

## The 25 Canonical Skills in This Repository

| # | Skill Name | Trigger | Group |
|---|-----------|---------|-------|
| 1 | `antigravity-start` | `/start` | Setup |
| 2 | `antigravity-team-leader` | Auto (Tier 2) | All |
| 3 | `antigravity-research` | `/research` | G1 |
| 4 | `antigravity-spec` | `/spec` | G1 |
| 5 | `antigravity-architecture` | `/architecture` | G2 |
| 6 | `antigravity-document` | `/document` | G2 |
| 7 | `antigravity-build` | `/build-all` | G3 |
| 8 | `antigravity-frontend-builder` | Auto (G3 worker) | G3 |
| 9 | `antigravity-backend-builder` | Auto (G3 worker) | G3 |
| 10 | `antigravity-database-builder` | Auto (G3 worker) | G3 |
| 11 | `antigravity-security-guard` | Auto (G3 worker) | G3 |
| 12 | `antigravity-github-saver` | Auto (G3 worker) | G3 |
| 13 | `antigravity-qa` | `/qa-test` | G4 |
| 14 | `antigravity-surgical` | `/surgical` | G4/5/6 |
| 15 | `antigravity-polish` | `/polish` | G5 |
| 16 | `antigravity-sec-ops` | `/sec-ops` | G6 |
| 17 | `antigravity-auditor` | `/audit` | G6 |
| 18 | `antigravity-hardware-compliance` | `/hardware-compliance` | G6 |
| 19 | `antigravity-memory` | `/context-save` + Cron | G7 |
| 20 | `antigravity-user-manual-writer` | Auto (G7) | G7 |
| 21 | `antigravity-deploy` | `/deploy` (manual) | G8 |
| 22 | `antigravity-web-deploy` | Auto (G8 specialist) | G8 |
| 23 | `antigravity-playstore-deploy` | Auto (G8 specialist) | G8 |
| 24 | `antigravity-appstore-deploy` | Auto (G8 specialist) | G8 |
| 25 | `antigravity-github-deploy` | Auto (G8 specialist) | G8 |

---

# MODULE 05: Platform Customization — Rules, Hooks, and Plugins

## Rules: Teaching the Agent Your Standards

Rules are instructions that the agent always follows, regardless of which skill is active. They enforce coding standards, naming conventions, security policies, and any other consistent behavior.

**Where rules live:**

| Location | Scope | Use For |
|----------|-------|---------|
| `AGENTS.md` (root) | Entire repository | Project-wide standards |
| `GEMINI.md` (any folder) | That folder + subfolders | Folder-specific overrides |
| `.agents/rules/*.md` | Entire workspace | Additional rule files |
| `~/.gemini/config/` | All projects on this machine | Machine-global rules |

**Rule triggers:**

- `always_on` — Loaded unconditionally every conversation
- `trigger: model_decision` — Loaded only if the AI decides it's relevant (Progressive Disclosure applies)

**Example rule in `AGENTS.md`:**
```
NEVER use float arithmetic for money. ALL monetary values use integer cents.
```

This single rule, declared in `AGENTS.md`, applies to every single agent in every group, in every conversation in this repository.

## Hooks: Automating Quality at Every Step

Hooks run shell commands at specific agent lifecycle events. Think of them as "tripwires" — whenever something happens (a file is saved, a commit is made), a specific command automatically runs.

**Hook events available:**

| Event | When It Fires |
|-------|--------------|
| `pre_tool` | Before any agent tool runs |
| `post_tool` | After any agent tool completes |
| `pre_commit` | Before a git commit is made |
| `post_build` | After a build command completes |

**Hooks in this repository** (`.agents/hooks.json`):

| Hook | Event | What It Runs |
|------|-------|-------------|
| `pre-tool:typescript-check` | Pre file write | `tsc --noEmit --strict` |
| `post-tool:eslint-fix` | Post TS/JS file write | `eslint --fix` |
| `pre-commit:full-quality-gate` | Pre commit | tsc + eslint + npm audit + jest |
| `pre-commit:secret-scan` | Pre commit | Grep for API keys / private keys |
| `pre-commit:env-not-staged` | Pre commit | Verify `.env` is not staged |
| `post-build:prisma-validate` | Post schema write | `prisma validate` |
| `pre-deploy:full-audit-gate` | Pre deploy command | Full gate check before any deploy |

## Plugins: Bundling Customizations Together

A **plugin** bundles multiple skills, rules, and MCP configs into a single installable package. This is useful when you want to share a complete set of customizations across teams or projects.

Plugin structure:
```
.agents/plugins/my-plugin/
├── plugin.json          ← Plugin manifest (name, version, description)
├── skills/
│   └── my-skill/SKILL.md
├── rules/
│   └── my-rules.md
└── mcp_config.json
```

This repository's skills can be packaged as a plugin for other teams to `install` via Antigravity's plugin manager.

---

# MODULE 06: The 8-Group Pipeline — Deep Dive

## The Simple Explanation

Building software with this AI team is like building a house. You don't start with painting the walls before the foundation is laid. There's a specific order of operations. Skip a step, and the whole thing falls apart.

Our 8-Group pipeline is that ordered process. Each Group is a phase of construction. Each phase has a "gate" — a test you must pass before the next phase begins. This prevents AI agents from building on shaky foundations.

## Group-by-Group Walkthrough

### GROUP 1: Product Discovery (G1)
*"What are we building and is it viable?"*

**Research Agent (Agent 01) does:**
- Web research on the target market and competitors
- Technology landscape survey (what frameworks exist, what's the best choice?)
- Dependency security scan (which npm packages have CVEs?)
- API availability check (if your app needs external APIs, do they exist and work?)

**Spec Writer (Agent 02) does:**
- Writes `master_spec.md` — a 6-section document covering: product vision, UI screen inventory, mathematical formulas, user personas, user workflows, success metrics

**⛔ HARD STOP:** Boss reads the master spec and types "APPROVED" before G2 begins.

### GROUP 2: System Blueprint (G2)
*"How exactly will we build it?"*

**Architecture Agent (Agent 03) generates:**
- `system_architecture.md` — Technology stack decisions (locked, not to be changed mid-project)
- `agents.md` — All coding standards for this specific project
- `tasks.md` — Task assignment matrix for each builder agent

**Documentarian (Agent 03b) generates 7 Compulsory Documents:**
1. `01_product_requirements.md` — Feature list with acceptance criteria
2. `02_api_contracts_and_endpoints.md` — **OpenAPI 3.1 YAML** (exact API contract)
3. `03_database_schema_blueprint.md` — **Prisma schema** with all models and indexes
4. `04_ui_ux_design_system.md` — Color palette, typography, animation spec, WCAG requirements
5. `05_hardware_and_sensor_protocols.md` — Hardware specs (if applicable)
6. `06_security_and_compliance_policy.md` — Argon2id params, RBAC roles, cookie spec
7. `07_testing_and_qa_strategy.md` — Test targets, coverage minimums, QA workflow

**⛔ HARD STOP:** Boss reviews all 7 documents and types "APPROVED" before G3 begins.

### GROUP 3: Full-Stack Construction (G3)
*"Build it — all 5 specialists working simultaneously"*

5 agents run in parallel:
- **Agent 05 (Frontend):** All screens, components, 3-state buttons, dark mode, responsive layout
- **Agent 06 (Backend):** All API routes, controllers, math service (integer cents), retry wrappers
- **Agent 07 (Database):** Prisma schema, indexes, SERIALIZABLE transactions, seed data
- **Agent 08 (Security):** Argon2id, RS256 JWT, rotating refresh tokens, RBAC, rate limiting, Helmet CSP
- **Agent 09 (Version Control):** `.gitignore`, dependency typosquat audit, npm CVE scan, commit log

**No hard stop** — but all 5 must complete before G4 starts. Team Leader monitors diary files.

### GROUP 4: QA Audit (G4)
*"Does it work? Are the calculations right? Can a real person use it?"*

**3-Pillar QA protocol:**
- **Pillar 1:** `tsc --noEmit` + `eslint --max-warnings 0` (zero errors tolerated)
- **Pillar 2:** Every mathematical formula in the spec is verified against the code with real test inputs
- **Pillar 3:** Every user workflow in the spec is simulated (happy path, error path, network failure)

**3-Paths Rule:** When a bug is found, QA presents exactly 3 remediation options to the Boss. No silent fixes.

### GROUP 5: Polish Loop (G5 — max 2 iterations)
*"Make it beautiful and accessible"*

**7-dimension sweep:**
1. Button state completeness (default/hover/active/focus/disabled/loading)
2. Color contrast (WCAG 2.1 AA: ≥4.5:1 for normal text)
3. Animation timing consistency (0.2s ease-in-out standard)
4. Spacing grid compliance (4px grid)
5. Dark mode token coverage
6. Loading state coverage (all API-calling buttons)
7. Form accessibility (all inputs have labels, keyboard navigable)

**Team Leader enforces max 2 iterations.** If issues remain after 2 loops, Boss decides: accept current state, surgical fix, or defer to sprint 2.

### GROUP 6: Audit & Compliance (G6)
*"Is it secure? Is it correctly built?"*

**DevSecOps Scanner (Agent 18) runs 5 automated scans:**
1. SQL injection (grep + semgrep)
2. Hardcoded secrets (pattern matching)
3. PII in logs (`console.log` containing email/password/token)
4. npm CVE audit (zero HIGH/CRITICAL tolerance)
5. Open endpoints without auth middleware

**Recovery Auditor (Agent 17):** Available for pre-built legacy codebases that need a rescue audit before entering the pipeline.

**Hardware Safety (Agent 19):** For IoT/embedded projects — datasheet research, 10-step MISRA C virtual simulation, Blast Radius report, mandatory Boss confirmation before any hardware command.

### GROUP 7: Documentation & Memory (G7)
*"Preserve the work and teach real users"*

**Memory Keeper (Agent 15):**
- Writes `context_snapshot.md` — 8-section machine-readable project state
- Runs nightly Cron to prevent context rot (AI forgetting project details)

**User Manual Writer (Agent 16):**
- Writes `USER_MANUAL.md` — plain English, zero jargon, for actual end-users
- Every button referenced by its exact label from the built UI
- 5 mandatory sections: Getting Started, Account, Features, Dashboard, Troubleshooting

### GROUP 8: Deployment (G8 — MANUAL ONLY)
*"Send it to the world — but only when Boss says go"*

**7 pre-deployment gates** must all pass:
1. DevSecOps audit: 0 CRITICAL/HIGH findings
2. G7 COMPLETED
3. `tsc --noEmit` exits 0
4. `npm audit --audit-level=high` exits 0
5. `.env` not tracked by git
6. `USER_MANUAL.md` exists
7. Manual Boss trigger (not auto-invoked)

**3 deployment options presented to Boss:**
- Option 1 (Recommended): Staging first → verify → promote to production
- Option 2: Direct to production (acknowledge risk)
- Option 3: Blue-green (zero downtime, gradual traffic switch)

**Platform-specific specialists:**
- Web: Vercel / Railway / Cloud Run (`web_deploy_skill.md`)
- Google Play Store: AAB build + keystore signing (`playstore_deploy_skill.md`)
- Apple App Store: IPA + TestFlight + privacy labels (`appstore_deploy_skill.md`)
- GitHub CI/CD: Actions pipeline + semantic versioning (`github_deploy_skill.md`)

---

# MODULE 07: Master Command & Context Mention Dictionary

## Slash Commands — Full Reference

### `/start` — Project Initialization
**What it does:** Activates the Salesman AI (Tier 1) which briefs the Team Leader (Tier 2) and initializes the workspace.
**What to say:** `/start` then describe your product in plain English.
**Example:**
```
/start
I want to build an invoicing SaaS for freelance designers.
Users should be able to create invoices, add line items, apply 3% GST,
send invoices by email, and track payment status (Draft / Sent / Paid).
```

### `/research` — Market & Technical Research
**What it does:** Dispatches the Research Agent (Agent 01) to conduct market analysis, competitor research, API landscape survey, and dependency CVE pre-scan.
**What you get:** `research_report_v1.md` in `1_COMPLETE_DOCUMENTATION/` with structured findings.
**Example:** `/research` (runs automatically based on your /start description)

### `/spec` — Master Specification Writer
**What it does:** Dispatches the Spec Writer (Agent 02) to convert research findings into a formal 6-section master specification.
**What you get:** `master_spec.md` — the contract that all builders follow.
**Example:** `/spec` (runs after /research, or type it to regenerate the spec)

### `/architecture` — System Architecture Generator
**What it does:** Locks the technology stack, generates `agents.md` (coding standards), and `tasks.md` (task assignments).
**What you get:** Three files that define HOW the software will be built.
**Example:** `/architecture` (runs after /spec is Boss-approved)

### `/document` — 7 Compulsory Documents
**What it does:** Generates all 7 blueprint documents including OpenAPI 3.1 YAML (API contract) and Prisma schema (database blueprint).
**What you get:** 7 files in `1_COMPLETE_DOCUMENTATION/`.
**Example:** `/document` (runs after /architecture)

### `/build-all` — Full-Stack Construction
**What it does:** Dispatches 5 concurrent sub-agents (frontend, backend, database, security, version control) to build the complete application.
**What you get:** Complete source code in `2_MAIN_CODING_FILES/`.
**Example:** `/build-all` (runs after 7 documents are Boss-approved)

### `/qa-test` — Quality Assurance Audit
**What it does:** Runs 3-Pillar QA: TypeScript compilation, formula verification, click-flow simulation.
**What you get:** `qa_report_v1.md` with structured bug registry.
**Example:** `/qa-test` (runs after /build-all completes)

### `/polish` — Visual Enhancement Sweep
**What it does:** Sweeps the frontend for 7 UI/UX dimensions and generates an Enhancement Cut Report.
**What you get:** List of approved cuts routed to the Surgeon for targeted fixes.
**Example:** `/polish` (runs after /qa-test passes)

### `/surgical` — Precision Bug Fixer
**What it does:** Applies a line-level code fix with pre-edit backup, impact analysis, and rollback capability.
**What you get:** Fixed file + backup in `3_PROJECT_BACKUP_AND_DIARY/` + diary log entry.
**How to trigger:** Team Leader dispatches automatically based on QA/Polish reports. Or type `/surgical` with an exact file + line description.

### `/sec-ops` — Security Vulnerability Scanner
**What it does:** Runs 5 automated security scans (SQL injection, secrets, PII logs, CVE, open endpoints).
**What you get:** `06_sec_ops_audit_report_v1.md` with SARIF-compatible findings.
**Example:** `/sec-ops` (runs after /polish)

### `/audit` — Pre-Built Codebase Recovery
**What it does:** Recovers and realigns an existing broken codebase through 4-angle audit + rescue rebuild.
**What you get:** `current_state_map.md`, `rescue_rebuild_log.md`, timestamped backup.
**Example:** `/audit` (use on existing legacy codebases, not fresh builds)

### `/hardware-compliance` — Hardware Safety Protocol
**What it does:** Researches hardware datasheet, runs 10-step MISRA C simulation, generates Blast Radius report.
**What you get:** `08_hardware_safety_report_v1.md` + Boss confirmation gate.
**Example:** `/hardware-compliance` (for IoT/embedded/Arduino projects)

### `/context-save` — Context Snapshot
**What it does:** Compresses the entire project state into `context_snapshot.md` (8-section machine-readable format).
**What you get:** A snapshot file that can restore full project context in a new session.
**Example:** `/context-save` (also runs automatically on nightly Cron schedule)

### `/context-load` — Context Restore
**What it does:** Reads `context_snapshot.md` and restores full project context for a new session.
**Example:** `/context-load` (use at the start of a new conversation on an existing project)

### `/deploy` — Production Deployment (MANUAL ONLY)
**What it does:** Runs 7 pre-deployment gates, presents deployment options, dispatches the appropriate deployment specialist.
**Example:** `/deploy` (then choose Option 1, 2, or 3)
**⚠️ IMPORTANT:** This command is NEVER invoked automatically. Always requires your explicit command.

---

## Context Mentions (`@`) — Full Reference

### `@file`
Attach a specific file to your message. The agent reads its full contents.
```
"Why is this function returning undefined? @file:backend/services/invoiceService.ts"
```

### `@folder`
Attach all files in a folder. Use when the agent needs broader context.
```
"Review all my API routes: @folder:backend/routes"
```

### `@git`
Attach current git status, recent diff, or commit history.
```
"What changed in the last 3 commits? @git"
```

### `@terminal`
Attach the output of an active terminal session.
```
"My build is failing. Here's the error: @terminal"
```

### `@mcp`
Access tools from a connected MCP server.
```
"How many invoices were created today? @mcp:postgres-analytics"
```

### `@agent`
Reference a previous or currently running sub-agent conversation.
```
"The QA agent found some bugs. What were they? @agent:qa-session-id"
```

### `@docs`
Reference the loaded skill descriptions (the progressive disclosure index).
```
"What can the surgical agent do? @docs"
```

### `@problems`
Attach the current list of compiler errors, lint warnings, or type errors from the IDE Problems pane.
```
"Fix all of these TypeScript errors: @problems"
```

---

# MODULE 08: Multi-Agent Orchestration — How the Team Works Together

## The Simple Explanation

Think of our agent system like a building construction company:

- **You (Boss)** = Property owner. You describe what you want, approve major decisions, pay for it.
- **Salesman AI** = Sales rep at the front desk. Takes your order in simple language.
- **Team Leader** = General Contractor. Manages all the workers, reads blueprints, knows when each worker should start.
- **Specialized Workers** = Electricians, plumbers, carpenters — each an expert in their domain.

Workers never talk directly to you — they report to the General Contractor, who summarizes for the Sales rep, who tells you.

## The 3-Diary State Machine

All agents synchronize through 3 shared diary files — not through direct memory transfer:

### `diary_1_audit_log.md` — The Event Log
Every agent action is logged here with a timestamp. Format:
```
| 2026-08-28T10:30:00Z | agent_05_frontend | BUILD_STARTED | Building page components | IN_PROGRESS |
| 2026-08-28T11:45:00Z | agent_05_frontend | FRONTEND_BUILD_COMPLETED | All screens done | COMPLETED |
```

### `diary_2_api_registry.md` — The API Contract Mirror
Every API endpoint created by the Backend Builder is registered here so the Frontend Builder can reference it:
```
| GET | /api/invoices | Returns all invoices for authenticated user | requireAuth | 200 Invoice[] |
| POST | /api/invoices | Creates new invoice | requireAuth + Zod validation | 201 Invoice |
```

### `diary_3_task_matrix.md` — The Pipeline Status Board
Shows which Group each phase is in:
```
| G1 | Product Discovery | COMPLETED | 2026-08-28 |
| G2 | System Blueprint  | COMPLETED | 2026-08-28 |
| G3 | Construction      | IN_PROGRESS | — |
```

## How Sub-Agent Spawning Works

When the Team Leader needs a worker, it calls `invoke_subagent`. The worker receives a completely self-contained prompt — it does NOT inherit the conversation history.

This is why every skill prompt is designed to say: *"Read these files first: [list]. Then do: [task]. Then log: [what to log]."*

The worker is like a contractor hired for one specific job. They read the blueprints, do the work, file a report, and go home. They don't need to know everything that happened before they arrived.

## The Polish Loop Ceiling

The Team Leader has a hard-coded rule: **Group 5 (Polish) runs maximum 2 times.** 

Why? To prevent the "infinite polish trap" — where an AI agent keeps suggesting cosmetic improvements forever, never letting the project ship. After 2 rounds, any remaining polish issues are documented and deferred to Sprint 2.

---

# MODULE 09: Context Rot Prevention & Memory Management

## The Simple Explanation

Imagine you're having a very long conversation — so long that by the time you get to minute 90, you've forgotten what was said in minute 1. AI agents have the same problem. The context window (the AI's "working memory") has a limit.

**Context Rot** is when the AI starts making decisions without remembering earlier agreements. It might suggest using `bcrypt` when you agreed earlier to use `Argon2id`. It might rewrite a component that was already marked as "working." It might lose track of which Group the project is in.

## The 2-Part Solution

### Part 1: Context Snapshot (`/context-save`)
The Memory Keeper (Agent 15) reads every important project file and compresses the essential information into a single, structured `context_snapshot.md`. This file has 8 sections:

1. **Project Identity** — What we're building, the tech stack
2. **Phase State Matrix** — Which Group each phase is in
3. **File Inventory** — All documents and their versions
4. **Active Decisions** — Locked architecture choices (tech stack, security standards)
5. **Open Issues** — Bugs and unresolved items from QA reports
6. **Boss Instructions Log** — Everything the Boss explicitly requested
7. **API Registry Snapshot** — The last 10 API endpoints registered
8. **Restore Instructions** — How to reload this context in a new session

### Part 2: Nightly Cron
The Memory Keeper runs automatically every night at midnight (configurable):
```json
{ "CronExpression": "0 0 * * *", "Prompt": "Execute /context-save protocol..." }
```

This means even if you don't manually save, you always have yesterday's snapshot as a safety net.

## How to Resume a Project After a Break

1. Open Antigravity 2.0 / IDE with the project folder
2. Type `/context-load`
3. The Memory Restore agent reads `context_snapshot.md` in priority order
4. Reports: *"Context restored. Current phase: G4. 3 open QA bugs. Tech stack: Next.js + Express + PostgreSQL."*
5. Continue from where you left off

---

# MODULE 10: Zero-Hallucination Verification & Quality Standards

## The Problem with AI Hallucination in Code

AI agents can "hallucinate" — confidently producing code that looks correct but doesn't work, uses wrong API parameters, or invents functions that don't exist. This is catastrophic in production software.

## Our 5-Layer Hallucination Defense System

### Layer 1: Schema-First Gate
The pipeline NEVER starts coding until OpenAPI 3.1 YAML (exact API contract) and Prisma schema (exact database structure) are written and Boss-approved.

This means the AI can't "invent" an API endpoint that doesn't match the contract — because the contract exists in a file that the QA agent will cross-reference against the actual code.

### Layer 2: Parameterized Query Enforcement
The GitHub Saver (Agent 09) and Security Scanner (Agent 18) both scan for string-interpolated SQL:
```bash
grep -rn '`SELECT.*\${' 2_MAIN_CODING_FILES/
```
Any match is a CRITICAL finding that blocks deployment. The agent cannot hallucinate a "safe" SQL injection.

### Layer 3: TypeScript Strict Mode
Every TypeScript file is compiled with `--strict`. If a function returns `undefined` when the type says `Invoice`, the compiler catches it. The pre-commit hook blocks the commit.

### Layer 4: Formula Verification Tests
For every mathematical formula in `master_spec.md`, the QA agent writes a unit test with the EXACT input/output from the spec:
```typescript
expect(applyGST(10000, 3)).toBe(10300); // Spec says: $100 + 3% GST = $103
```
If the AI wrote the formula wrong, the test fails. The test input/output is from the human-written spec — not invented by the AI.

### Layer 5: 3-Paths Bug Resolution (No Silent Fixes)
When QA finds a bug, the agent NEVER silently fixes it. It always:
1. Reports the bug with exact file + line number
2. Presents 3 remediation options (different risk profiles)
3. Waits for Boss to choose

This prevents the AI from "fixing" a symptom while hiding a deeper problem.

## Production Quality Rating System

At the end of each Group, the Team Leader checks these criteria:

| Category | What's Checked | Pass Threshold |
|----------|---------------|---------------|
| Compilation | `tsc --noEmit` exit code | 0 |
| Code Quality | ESLint max warnings | 0 |
| Security | npm audit HIGH/CRITICAL CVEs | 0 |
| Test Coverage | Jest line coverage | ≥ 85% |
| Formula Accuracy | Spec example inputs match code output | 100% |
| SQL Safety | Zero string-interpolated SQL queries | 0 |
| Secret Safety | Zero hardcoded credentials in source | 0 |
| Auth Security | Argon2id used (not MD5/SHA/weak bcrypt) | ✅ Confirmed |
| Money Precision | Integer cents throughout (no float money) | ✅ Confirmed |
| Deployment Gate | All 7 pre-deploy gates pass | All 7 |

**All criteria must pass before the project ships.** The agent cannot override these gates.

---

## Appendix A: Quick-Reference Security Cheat Sheet

| Standard | Required Value | Forbidden Alternative |
|----------|---------------|----------------------|
| Password hashing algorithm | Argon2id | MD5, SHA-*, bcrypt <12 rounds |
| Argon2id memory cost | m=65536 (64 MiB) | Any lower value |
| Argon2id time cost | t=3 | t=1 or t=2 |
| Argon2id parallelism | p=4 | Any lower value |
| JWT algorithm | RS256 | HS256 |
| JWT library | `jose` | `jsonwebtoken` (HS256 default risk) |
| Access token storage | HTTP Authorization header or cookie | localStorage |
| Refresh token storage | HttpOnly + SameSite=Strict + Secure cookie | localStorage, sessionStorage |
| Cookie SameSite | Strict | Lax or None without Secure |
| SQL query style | Parameterized (`$1`, `?`, Prisma ORM) | Template literals, string concatenation |
| Monetary storage | Integer cents (Int column in Prisma) | Float, Decimal |
| CORS origins | Explicit whitelist from env var | `*` in production |
| Secrets source | `process.env.*` only | Hardcoded in source |

---

## Appendix B: Folder Structure Enforcement

```
Project Root/
├── 1_COMPLETE_DOCUMENTATION/     ← Documentation ONLY. No code here.
│   ├── master_spec.md
│   ├── 01_product_requirements.md
│   ├── 02_api_contracts_and_endpoints.md   ← OpenAPI 3.1 YAML
│   ├── 03_database_schema_blueprint.md     ← Prisma schema
│   ├── 04_ui_ux_design_system.md
│   ├── 05_hardware_and_sensor_protocols.md
│   ├── 06_security_and_compliance_policy.md
│   └── 07_testing_and_qa_strategy.md
│
├── 2_MAIN_CODING_FILES/          ← Source code ONLY. No docs here.
│   ├── frontend/
│   ├── backend/
│   └── database/
│
└── 3_PROJECT_BACKUP_AND_DIARY/   ← Logs and backups ONLY. No code or docs.
    ├── diary_1_audit_log.md
    ├── diary_2_api_registry.md
    ├── diary_3_task_matrix.md
    ├── context_snapshot.md
    └── legacy_backup_{timestamp}/
```

**The penalty for violation:** Any source code found in folder 1 or 3 is flagged as a CRITICAL violation by the Auditor and must be relocated before deployment.

---

*End of Enterprise Handbook v1.0*
*For the latest platform documentation: https://antigravity.google/docs*
