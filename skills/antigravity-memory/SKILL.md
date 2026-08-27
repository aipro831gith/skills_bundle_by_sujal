---
name: antigravity-memory
description: Deploys Memory Keeper Sub-Agent for context save/load, auto USER_MANUAL.md generation, and Scheduled Tasks (Cron) for nightly context pruning against content rot.
---
# Antigravity Enterprise Ecosystem: Memory & Scheduled Pruning Skill

This skill governs **Agent 15 (Memory Keeper)** and **Agent 16 (Documentarian User-Manual Writer)**.

## 1. Memory Keeper Sub-Agent (`/context-save`, `/context-load`, & Scheduled Pruning)

### Scheduled Task Integration (Anti-Content-Rot Protocol)
Content rot (context drift) occurs when long conversations fill up memory and cause AI agents to forget early rules or hallucinate critical bugs. 

To eliminate content rot, Agent 15 integrates with **Antigravity Scheduled Tasks (Cron)**:
* **Nightly Automated Memory Compression Schedule:** Configured to trigger automatically at `0 0 * * *` (midnight nightly) or via manual `/context-save`.
* **Action:** The Scheduled Task automatically reads active project diaries, compresses the daily code changes, generates a fresh `context_snapshot.md` inside `3_PROJECT_BACKUP_AND_DIARY/`, and clears outdated conversation noise.

### Step 1: Sub-Agent Dispatch for `/context-save`
When the Boss types `/context-save` or when the Scheduled Task fires:

```json
{
  "Subagents": [
    {
      "TypeName": "memory-keeper-save",
      "Role": "Context Snapshot & Scheduled Pruning Sub-Agent",
      "Prompt": "Execute memory compression protocol. 1. Read all files in folder 1, 2, and 3. 2. Summarize key architectural decisions, active task states, and code file inventories. 3. Write context_snapshot.md in 3_PROJECT_BACKUP_AND_DIARY/. 4. Clear stale prompt overhead to eliminate content rot and context drift. Log in diary 1."
    }
  ]
}
```

### Step 2: Sub-Agent Dispatch for `/context-load`
When the Boss types `/context-load`:

```json
{
  "Subagents": [
    {
      "TypeName": "memory-keeper-load",
      "Role": "Context Restore Memory Keeper Sub-Agent",
      "Prompt": "Read context_snapshot.md in folder 3. Silently restore all project context, diaries, active file registries, and task matrices into memory."
    }
  ]
}
```

---

## 2. Documentarian User-Manual Writer Sub-Agent (Auto-Completion)

When the software achieves 100% completion (after `/surgical`), execute `invoke_subagent`:

```json
{
  "Subagents": [
    {
      "TypeName": "user-manual-writer",
      "Role": "End-User Documentation Sub-Agent",
      "Prompt": "Read master_spec.md and 2_MAIN_CODING_FILES/. Generate a plain-English USER_MANUAL.md in the root directory for end-users (customers). Include login steps, core feature walkthroughs, button explanations, and user troubleshooting guides."
    }
  ]
}
```

### Salesman Hand-off
Tell the Boss:
*"Boss, our Memory Keeper Sub-Agent has executed the Scheduled Context Compression protocol to eliminate content rot, and our Documentarian Sub-Agent has generated `USER_MANUAL.md` for your end-users!"*
