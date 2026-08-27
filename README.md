# ANTIGRAVITY 2.0 ENTERPRISE MULTI-AGENT SYSTEM (`skills_bundle_by_sujal`)
# Official GitHub Repository: https://github.com/aipro831gith/skills_bundle_by_sujal

Welcome to the official **Antigravity 2.0 Enterprise Ecosystem** created by Sujal! Every single agent and sub-agent has its own **1-to-1 dedicated skill directory** in `skills/`.

---

## 📂 Complete 1-to-1 Dedicated Skill Directory Inventory (22 Skill Directories)

| Skill Directory Name (`skills/`) | Sub-Agent Full Name | Tier Level | Command Trigger | Primary Responsibility |
| :--- | :--- | :--- | :--- | :--- |
| **`skills/antigravity-start/`** | **Salesman AI** | Tier 1 | `/start`, `/end` | User greeting, slash command receiving, `/start`/`/end` triggers. |
| **`skills/antigravity-team-leader/`** | **Agent 00: Team Leader Sub-Agent** | Tier 2 | `/start` (via Salesman) | Async non-blocking supervision, worker orchestration, loop engineering. |
| **`skills/antigravity-research/`** | **Agent 01: Researcher Sub-Agent** | Tier 3 | `/research` | Compulsory live web search for competitors, APIs, hardware protocols. |
| **`skills/antigravity-spec/`** | **Agent 02: Spec Writer Sub-Agent** | Tier 3 | `/spec` | Skill detection, word breakdown of UI/math (`master_spec.md`), immutable `_v2.md` specs. |
| **`skills/antigravity-city-planner/`** | **Agent 03: City Planner Sub-Agent** | Tier 3 | `/architecture` | 3-Step macro city mapping (`system_architecture.md`). |
| **`skills/antigravity-office-manager/`** | **Agent 04: Office Manager Sub-Agent** | Tier 3 | `/architecture` | Generates strict coding rules (`agents.md`) and master checklist (`tasks.md`). |
| **`skills/antigravity-documentarian-architect/`** | **Agent 03b: Documentarian Architect** | Tier 3 | `/document` | Generates **ALL 7 Compulsory Documents** in folder 1. |
| **`skills/antigravity-frontend-builder/`** | **Agent 05: Front-End Builder Sub-Agent** | Tier 3 | `/build-all` | Reads `frontend_builder.md`, `1_MD`, `2_MD`, `3_MD`, `N_MD`; designs UI/UX layouts & 3-state hover buttons. |
| **`skills/antigravity-backend-builder/`** | **Agent 06: Backend Builder Sub-Agent** | Tier 3 | `/build-all` | Reads `backend_builder.md`; parameterized DB queries & exact math logic. |
| **`skills/antigravity-database-builder/`** | **Agent 07: Database Builder Sub-Agent** | Tier 3 | `/build-all` | ORM models for PostgreSQL, Firebase, MongoDB, Google Sheets with indexing. |
| **`skills/antigravity-security-guard/`** | **Agent 08: Security Guard Sub-Agent** | Tier 3 | `/build-all` | Zero-trust JWT, bcrypt password hashing (salts >= 10), RBAC access controls, `.env`. |
| **`skills/antigravity-github-saver/`** | **Agent 09: GitHub Saver Sub-Agent** | Tier 3 | `/build-all` | Generates `.gitignore`, dependency typosquatting checks, simulated commit logs. |
| **`skills/antigravity-spell-checker/`** | **Agent 10: Spell Checker Sub-Agent** | Tier 3 | `/qa-test` | Syntax check, compiler error verification, unhandled exception checks. |
| **`skills/antigravity-math-checker/`** | **Agent 11: Math Checker Sub-Agent** | Tier 3 | `/qa-test` | Verifies math calculations (\(Total \times 1.03\)) against spec. |
| **`skills/antigravity-human-tester/`** | **Agent 12: Human Tester Sub-Agent** | Tier 3 | `/qa-test` | Simulates human browser clicks; enforces **3 Paths Rule** for bugs. |
| **`skills/antigravity-polish/`** | **Agent 13: Polisher Sub-Agent** | Tier 3 | `/polish` | UX visual sweep for animations, responsive breakpoints, padding tweaks. |
| **`skills/antigravity-surgical/`** | **Agent 14: Surgeon Sub-Agent** | Tier 3 | `/surgical` | Pre-Change Insurance Protocol (backs up file to folder 3 first), impact analysis, and precision code edit. |
| **`skills/antigravity-memory-keeper/`** | **Agent 15: Memory Keeper Sub-Agent** | Tier 3 | `/context-save`, `/context-load`, Cron | Context snapshots + Scheduled Tasks (Cron) nightly context pruning. |
| **`skills/antigravity-user-manual-writer/`** | **Agent 16: User Manual Writer** | Tier 3 | (Auto) | Auto-generates plain-English `USER_MANUAL.md` upon project completion. |
| **`skills/antigravity-auditor/`** | **Agent 17: Auditor Recovery Sub-Agent** | Tier 3 | `/audit` | Multi-angle reverse engineering for pre-built or broken projects. Rebuilds according to Boss's dream vision. |
| **`skills/antigravity-sec-ops/`** | **Agent 18: DevSecOps Sub-Agent** | Tier 3 | `/sec-ops` | Scans for SQL injections, PII leaks, open endpoints, typosquatted packages; outputs `06_sec_ops_audit_report.md`. |
| **`skills/antigravity-hardware-compliance/`** | **Agent 19: Hardware Compliance Sub-Agent** | Tier 3 | `/hardware-compliance` | MISRA rules, C/C++/Rust pointer bounds checking, zero-panic runtime safety; outputs `08_hardware_safety_report.md`. |

---

## 🎨 Extensible Front-End Builder Architecture (`skills/antigravity-frontend-builder/`)
- `1_MD_Design_Skill.md`: Web SaaS Layout (Collapsible sidebar, top header profile/theme switch, CSS grid).
- `2_MD_Design_Skill.md`: Mobile Flutter/Android Layout (Bottom navigation bar, sliver headers, card feeds).
- `3_MD_Design_Skill.md`: Enterprise ERP Layout (Multi-Tab views, dense data tables, pagination, sortable columns).
- `N_MD_Design_Skill.md`: Extensible pattern template for future custom design skills (4_MD, 5_MD, etc.).
- **3-State Interactive Buttons:** Default, hover (`0.2s ease-in-out; scale(1.03)`), and active (`scale(0.98)`), plus automated click loading spinners.

---

## Installation & Quick Start

1. Copy `.gemini` to your user home directory or project root.
2. Place `AGENTS.md` and the `skills/` folder in your workspace root.
3. Open your terminal or AI interface and type `/start`. Welcome to your Enterprise Multi-Agent Software Corporation!
