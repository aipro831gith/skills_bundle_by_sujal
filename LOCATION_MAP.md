# 🗺️ UNIVERSAL LOCATION & ARCHITECTURE MAP
## Skills Bundle by Sujal — Google Antigravity 2.0 Ecosystem

This document provides the complete, authoritative mapping of where all skills, agents, rules, MCP servers, lifecycle hooks, and project templates live across your system.

---

## 1. SYSTEM DIRECTORY HIERARCHY

```text
C:\Users\ASUS\
├── .gemini\                                      ◄── GLOBAL SYSTEM CONFIGURATION ROOT
│   └── config\
│       ├── skills\                               ◄── GLOBAL SKILLS DIRECTORY (Auto-discovered across all workspaces)
│       │   ├── start\SKILL.md
│       │   ├── team-leader\SKILL.md
│       │   ├── research\SKILL.md
│       │   ├── spec\SKILL.md
│       │   ├── architecture\SKILL.md
│       │   ├── document\SKILL.md
│       │   ├── build\SKILL.md
│       │   ├── frontend-builder\SKILL.md
│       │   ├── backend-builder\SKILL.md
│       │   ├── database-builder\SKILL.md
│       │   ├── security-guard\SKILL.md
│       │   ├── github-saver\SKILL.md
│       │   ├── qa\SKILL.md
│       │   ├── surgical\SKILL.md
│       │   ├── polish\SKILL.md
│       │   ├── sec-ops\SKILL.md
│       │   ├── auditor\SKILL.md
│       │   ├── hardware-compliance\SKILL.md
│       │   ├── memory\SKILL.md
│       │   ├── user-manual-writer\SKILL.md
│       │   ├── deploy\SKILL.md
│       │   ├── web-deploy\SKILL.md
│       │   ├── playstore-deploy\SKILL.md
│       │   ├── appstore-deploy\SKILL.md
│       │   ├── github-deploy\SKILL.md
│       │   ├── deterministic-gatekeeper\SKILL.md
│       │   └── ast-diff-reconciler\SKILL.md
│       └── plugins\
│           └── skills_bundle_by_sujal\
│               └── plugin.json                   ◄── REGISTERED ANTIGRAVITY PLUGIN
│
└── .gemini\antigravity\scratch\
    ├── skills_bundle_by_sujal\                  ◄── MASTER SOURCE REPOSITORY (GitHub sync)
    │   ├── .agent\skills\*.md                   ◄── Flat canonical skill definitions
    │   ├── .agents\
    │   │   ├── mcp_config.json                  ◄── 11 Hardened MCP Server integrations
    │   │   ├── hooks.json                       ◄── 15 Pre/Post lifecycle automation hooks
    │   │   └── skills\*\SKILL.md                ◄── Nested skill packages
    │   ├── agents\*.md                          ◄── 27 Sub-agent persona dispatchers
    │   ├── AGENTS.md                            ◄── Master Agent Swarm Governance
    │   ├── GEMINI.md                            ◄── Universal Project Invariants
    │   ├── README.md                            ◄── Full documentation & quickstart
    │   ├── ANTIGRAVITY_ENTERPRISE_HANDBOOK.md   ◄── 10-Module Staff Manual
    │   └── LOCATION_MAP.md                      ◄── This file
    │
    └── starter_project_template\                ◄── READY-TO-USE NEW PROJECT TEMPLATE
        ├── START_HERE.md                        ◄── 1-Minute Project Launch Guide
        ├── AGENTS.md                            ◄── Pre-linked Agent Registry
        ├── GEMINI.md                            ◄── Pre-linked Project Invariants
        ├── .env.template                        ◄── Security environment placeholders
        ├── .gitignore                           ◄── Pre-configured secret & artifact blockers
        ├── .agents\                             ◄── Pre-configured MCP, hooks, and skills
        ├── 1_COMPLETE_DOCUMENTATION\            ◄── Clean specs & blueprints directory
        ├── 2_MAIN_CODING_FILES\                 ◄── Clean source code directory
        └── 3_PROJECT_BACKUP_AND_DIARY\          ◄── Initialized diary_1, diary_2, diary_3
```

---

## 2. CANONICAL SKILL & AGENT MAPPING TABLE

| Group | Phase Name | Slash Command | Canonical Skill File | Sub-Agent Dispatcher |
|---|---|---|---|---|
| **Setup** | Sales Intake & Setup | `/start`, `/end` | `.agent/skills/start_skill.md` | `agents/start_agent.md` |
| **Tier 2** | Conveyor Orchestrator | *Automated* | `.agent/skills/team_leader_skill.md` | `agents/team_leader.md` |
| **G1** | Market & Tech Discovery | `/research` | `.agent/skills/research_skill.md` | `agents/research_agent.md` |
| **G1** | Master Spec & Feature DAG | `/spec` | `.agent/skills/spec_skill.md` | `agents/spec_agent.md` |
| **G2** | System Architecture | `/architecture` | `.agent/skills/architecture_skill.md` | `agents/architecture_agent.md` |
| **G2** | 7 Compulsory Documents | `/document` | `.agent/skills/document_skill.md` | `agents/document_agent.md` |
| **G3** | Build Orchestration | `/build-all` | `.agent/skills/build_skill.md` | `agents/builder_agent.md` |
| **G3** | Frontend Construction | *Automated (G3)* | `.agent/skills/frontend_builder_skill.md` | `agents/frontend_builder.md` |
| **G3** | Backend API Construction | *Automated (G3)* | `.agent/skills/backend_builder_skill.md` | `agents/backend_builder.md` |
| **G3** | Database Architecture | *Automated (G3)* | `.agent/skills/database_builder_skill.md` | `agents/database_builder.md` |
| **G3** | Zero-Trust Security | *Automated (G3)* | `.agent/skills/security_guard_skill.md` | `agents/security_guard.md` |
| **G3** | Version Control & Audits | *Automated (G3)* | `.agent/skills/github_saver_skill.md` | `agents/github_saver.md` |
| **G4** | 3-Pillar QA Test Suite | `/qa-test` | `.agent/skills/qa_skill.md` | `agents/qa_agent.md` |
| **G4-G6**| Precision AST Surgeon | `/surgical` | `.agent/skills/surgical_skill.md` | `agents/surgical_agent.md` |
| **G4-G6**| AST Diff Reconciler | *Automated* | `.agent/skills/ast_diff_reconciler_skill.md` | *Internal Reconciler* |
| **G4-G8**| Deterministic Gatekeeper | *Automated* | `.agent/skills/deterministic_gatekeeper_skill.md` | *Automated Gate* |
| **G5** | Visual & UX Polish Sweep | `/polish` | `.agent/skills/polish_skill.md` | `agents/polish_agent.md` |
| **G6** | DevSecOps SAST Scanner | `/sec-ops` | `.agent/skills/sec_ops_skill.md` | `agents/sec_ops_agent.md` |
| **G6** | Recovery & Rescue Auditor | `/audit` | `.agent/skills/auditor_skill.md` | `agents/auditor_agent.md` |
| **G6** | Hardware Safety Compliance | `/hardware-compliance` | `.agent/skills/hardware_compliance_skill.md` | `agents/hardware_compliance.md` |
| **G7** | Memory & Snapshot Keeper | `/context-save`, `/context-load` | `.agent/skills/memory_skill.md` | `agents/memory_agent.md` |
| **G7** | End-User Manual Writer | *Automated (G7)* | `.agent/skills/user_manual_writer_skill.md` | `agents/user_manual_writer.md` |
| **G8** | Deployment Orchestrator | `/deploy` (**Manual**) | `.agent/skills/deploy_skill.md` | `agents/deploy_agent.md` |
| **G8** | Web & Cloud Deployment | *Automated (G8)* | `.agent/skills/web_deploy_skill.md` | `agents/web_deploy_agent.md` |
| **G8** | Google Play Store Release | *Automated (G8)* | `.agent/skills/playstore_deploy_skill.md` | `agents/playstore_deploy_agent.md` |
| **G8** | Apple App Store Release | *Automated (G8)* | `.agent/skills/appstore_deploy_skill.md` | `agents/appstore_deploy_agent.md` |
| **G8** | GitHub CI/CD Deployment | *Automated (G8)* | `.agent/skills/github_deploy_skill.md` | `agents/github_deploy_agent.md` |

---

## 3. HOW TO START A NEW PROJECT AUTOMATICALLY

### Step 1: Clone or Copy the Starter Template
To create a brand new project anywhere on your computer:
```powershell
# Copy the clean template to your desired new project folder:
Copy-Item -Path "C:\Users\ASUS\.gemini\antigravity\scratch\starter_project_template" -Destination "C:\Users\ASUS\my_new_project" -Recurse
```

### Step 2: Open in Antigravity
Open `C:\Users\ASUS\my_new_project` in **Antigravity 2.0** or **Antigravity IDE**.

### Step 3: Launch with `/start`
In the chat canvas, type:
```text
/start
```
and provide your vision. The entire 8-Group conveyor belt will handle everything automatically!
