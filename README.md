# 🚀 Skills Bundle by Sujal
### Enterprise AI Agent Skills — Built for Google Antigravity 2.0

![Platform](https://img.shields.io/badge/Platform-Google%20Antigravity%202.0-blue)
![IDE](https://img.shields.io/badge/IDE-Antigravity%20IDE%20(VS%20Code%20fork)-blueviolet)
![Skills](https://img.shields.io/badge/Skills-25%20Canonical-green)
![Security](https://img.shields.io/badge/Auth-Argon2id%20%2B%20RS256%20JWT-red)
![Coverage](https://img.shields.io/badge/Test%20Coverage-%E2%89%A585%25-brightgreen)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## What This Repository Is

A complete, enterprise-grade collection of **AI agent skills** for Google Antigravity — rewritten from the ground up to production standards. Each skill is a deterministic, test-gated, zero-ambiguity instruction set that tells Antigravity sub-agents exactly what to do, how to verify it worked, and what to do if it fails.

**Think of it as a recipe book** — but instead of cooking instructions, it's step-by-step playbooks for building full-stack software products using an AI team that you direct in plain English.

---

## Repository Structure

```
skills_bundle_by_sujal/
│
├── AGENTS.md                        ← Master agent swarm registry (YOU ARE HERE)
├── README.md                        ← This file
├── .agents/
│   ├── mcp_config.json              ← MCP server connections (database, GitHub, security)
│   └── hooks.json                   ← Lifecycle automation (pre-commit gates, post-save linting)
│
└── .agent/skills/                   ← 25 canonical skill files
    ├── start_skill.md               ← /start — Project initialization
    ├── team_leader_skill.md         ← Tier 2 orchestrator (8-Group pipeline)
    ├── research_skill.md            ← /research — Market & API research
    ├── spec_skill.md                ← /spec — Master specification writer
    ├── architecture_skill.md        ← /architecture — System architect
    ├── document_skill.md            ← /document — 7 compulsory documents
    ├── build_skill.md               ← /build-all — Parallel 5-worker dispatcher
    ├── frontend_builder_skill.md    ← Agent 05: UI/UX builder
    ├── backend_builder_skill.md     ← Agent 06: API + math engine
    ├── database_builder_skill.md    ← Agent 07: Prisma + migrations
    ├── security_guard_skill.md      ← Agent 08: Argon2id + RS256 + RBAC
    ├── github_saver_skill.md        ← Agent 09: gitignore + dependency audit
    ├── qa_skill.md                  ← /qa-test — 3-Pillar QA
    ├── surgical_skill.md            ← /surgical — Precision bug fixer
    ├── polish_skill.md              ← /polish — WCAG 2.1 AA visual polish
    ├── sec_ops_skill.md             ← /sec-ops — 5-vector security scan
    ├── auditor_skill.md             ← /audit — Pre-built codebase recovery
    ├── hardware_compliance_skill.md ← /hardware-compliance — MISRA C + Blast Radius
    ├── memory_skill.md              ← /context-save — Context snapshot + Cron
    ├── user_manual_writer_skill.md  ← Plain-English user manual generator
    ├── deploy_skill.md              ← /deploy — Master deployment orchestrator
    ├── web_deploy_skill.md          ← Vercel / Railway / Cloud Run
    ├── playstore_deploy_skill.md    ← Google Play Store (AAB + keystore)
    ├── appstore_deploy_skill.md     ← Apple App Store (IPA + TestFlight)
    └── github_deploy_skill.md       ← GitHub Actions CI/CD pipeline
```

---

## Quick Start

### Option A: Antigravity 2.0 (Desktop App)

1. Open **Antigravity 2.0**.
2. Click **Projects** in the left sidebar → **Open Folder** → select this repository root.
3. Start a new conversation in the Chat Canvas.
4. Type `/start` and describe your product idea in plain English.

The agent will guide you through the 8-Group pipeline automatically.

### Option B: Antigravity IDE (VS Code Fork)

1. Open the repository folder in **Antigravity IDE**.
2. The IDE automatically discovers `.agents/` at the project root and loads all customizations.
3. Open the Sidebar Chat panel.
4. Type `/start` to begin.

### Option C: Antigravity CLI (`agy`)

```bash
# Navigate to the repository
cd /path/to/skills_bundle_by_sujal

# Launch the CLI (automatically discovers .agents/ from CWD)
agy

# Inside the CLI:
/start
```

---

## The 8-Group Conveyor Belt

Your product is built in a deterministic 8-phase pipeline:

```
G1: Research & Spec  →  G2: Blueprint & Docs  →  G3: Build (5 workers)
       ↓ HARD STOP                ↓ HARD STOP
   Boss approval              Boss approval

G4: QA Testing  →  G5: Polish (max 2×)  →  G6: Security Audit
G7: User Manual  →  G8: DEPLOY (manual only — always Boss confirmation)
```

**Two Hard Stops** require your explicit approval before the pipeline continues:
1. After G1: Review the master specification
2. After G2: Review all 7 blueprint documents

**Group 8 (Deploy) is always manual** — the AI never auto-deploys.

---

## Master Slash Command Directory

Type `/` in the chat to see all available commands.

| Command | Group | What It Does |
|---------|-------|-------------|
| `/start` | Setup | Initialize project, set up workspace, brief Team Leader |
| `/end` | Setup | Graceful project shutdown with final summary |
| `/research` | G1 | Run market research, competitor analysis, CVE dependency scan |
| `/spec` | G1 | Generate master specification with 6 mandatory sections |
| `/architecture` | G2 | Generate system architecture, agents.md, tasks.md |
| `/document` | G2 | Generate all 7 compulsory blueprint documents |
| `/build-all` | G3 | Dispatch 5 concurrent builders (frontend/backend/DB/security/git) |
| `/qa-test` | G4 | Run 3-Pillar QA (compiler + math verification + click-flow) |
| `/polish` | G5 | 7-dimension WCAG 2.1 AA visual enhancement sweep |
| `/surgical` | G4/5/6 | Precision line-level fix with pre-edit backup |
| `/sec-ops` | G6 | 5-vector security scan (SQL injection, secrets, PII logs, CVE, endpoints) |
| `/audit` | G6 | 4-angle pre-built codebase recovery audit |
| `/hardware-compliance` | G6 | Hardware safety (MISRA C sim, datasheet research, Blast Radius) |
| `/context-save` | G7 | Compress project state to context_snapshot.md |
| `/context-load` | G7 | Restore project context from saved snapshot |
| `/deploy` | G8 | Production deployment orchestrator (manual only) |

---

## Master Context Mentions Directory

Type `@` in the chat to attach context to your message:

| Mention | What It Attaches |
|---------|-----------------|
| `@file` / `@folder` | A specific file or entire folder as context |
| `@git` | Current git status, diff, recent commits |
| `@terminal` | Output from an active terminal session |
| `@mcp` | Tools from a connected MCP server |
| `@agent` | A previous or running sub-agent conversation |
| `@docs` | Loaded project documentation (skill descriptions) |
| `@problems` | Current compiler errors / lint warnings from the Problems pane |

**Production examples:**
```
"Fix the TypeScript error in @file:backend/controllers/invoiceController.ts"
"What was the last command output? @terminal"
"Run the QA suite — errors are at @problems"
"Search for the Prisma docs @mcp:sequential-thinking"
```

---

## Keyboard Shortcut Cheat Sheet

### Antigravity IDE (VS Code Fork)

| Action | Windows / Linux | macOS |
|--------|----------------|-------|
| Open Sidebar Chat | `Ctrl+Shift+A` | `⌘+Shift+A` |
| Inline AI Edit | `Ctrl+I` | `⌘+I` |
| Accept Autocomplete | `Tab` | `Tab` |
| Reject Autocomplete | `Esc` | `Esc` |
| Accept Word-by-Word | `Ctrl+→` | `⌘+→` |
| Jump to Next Suggestion | `Tab` (after accept) | `Tab` (after accept) |
| Trigger Diagnostic Auto-Fix | Click error underline → lightbulb | Click error underline → lightbulb |
| Apply Diff Suggestion | `Ctrl+Enter` in diff view | `⌘+Enter` in diff view |
| Reject Diff Suggestion | `Ctrl+Backspace` in diff view | `⌘+Backspace` in diff view |
| Open Command Palette | `Ctrl+Shift+P` | `⌘+Shift+P` |

### Antigravity 2.0 (Desktop App)

| Action | All Platforms |
|--------|--------------|
| New Conversation | Click **New Conversation** in left sidebar |
| Open Slash Commands | Type `/` in chat input |
| Open Context Mentions | Type `@` in chat input |
| Attach file/image | Drag-and-drop into chat canvas |
| View Running Sub-Agents | Click **Subagents** in auxiliary pane |
| View Background Tasks | Click **Background Tasks** in auxiliary pane |
| View Artifacts | Click **Artifacts** in auxiliary pane |

---

## Environment Variable Setup

Copy `.env.template` to `.env` and fill in real values:

```bash
cp .env.template .env
```

Required variables (names only — never commit real values):

| Variable | Used By | Description |
|----------|---------|-------------|
| `DATABASE_URL` | Database Builder, Backend Builder | PostgreSQL connection string |
| `JWT_PRIVATE_KEY` | Security Guard | RSA-2048 private key (PEM format) |
| `JWT_PUBLIC_KEY` | Security Guard | RSA-2048 public key (PEM format) |
| `CORS_ALLOWED_ORIGINS` | Security Guard | Comma-separated allowed origins |
| `POSTGRES_ANALYTICS_URL` | MCP: postgres-analytics | Analytics DB connection string |
| `GITHUB_PERSONAL_ACCESS_TOKEN` | MCP: github-enterprise | GitHub PAT with repo scope |
| `BRAVE_API_KEY` | MCP: brave-search | Brave Search API key |
| `SEMGREP_APP_TOKEN` | MCP: sast-security-scanner | Semgrep Cloud token (optional) |

---

## Security Standards (Non-Negotiable)

| Requirement | Standard | Forbidden Alternative |
|------------|---------|----------------------|
| Password hashing | Argon2id (m=65536, t=3, p=4) | MD5, SHA-*, bcrypt <12 rounds |
| JWT algorithm | RS256 | HS256 |
| Session storage | HttpOnly + SameSite=Strict cookie | localStorage |
| SQL queries | Parameterized bindings | String interpolation |
| Money storage | Integer cents (1099 = \$10.99) | Float/Decimal |
| CORS | Explicit whitelist | `origin: '*'` in production |

---

## MCP Servers Included

| Server | Transport | Policy | Purpose |
|--------|-----------|--------|---------|
| `filesystem-workspace` | stdio | always-proceed | File read/write |
| `postgres-analytics` | stdio | request-review | Database queries |
| `github-enterprise` | stdio | request-review | Repo management |
| `docker-sandbox` | stdio | proceed-in-sandbox | Isolated builds |
| `sast-security-scanner` | stdio | always-proceed | Semgrep SAST |
| `sequential-thinking` | stdio | always-proceed | Structured reasoning |
| `brave-search` | stdio | always-proceed | Web research |

---

## Contributing

1. All skill files must follow the 7-section template in `architecture_skill.md`.
2. Every skill must include a `NEVER DO:` list and escalation matrix.
3. Run the pre-commit hooks before pushing (they run automatically via `hooks.json`).
4. New skills must be registered in `AGENTS.md` before they can be used.

---

## Confidence & Accuracy Note

This repository uses only verified Antigravity platform features as documented at `https://antigravity.google/docs`. Features marked with `[illustrative]` in skill files are best-effort examples, not officially documented behavior. Always check the official docs for the latest capabilities.
