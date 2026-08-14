LEVEL 2: PROJECT MASTER GUARDRAILS
File Configuration: Save verbatim as AGENTS.md in your Project Root Folder

Purpose: Enforces code quality, structural layout, framework scalability, directory patterns, and hardware/API portability options for this specific workspace.

1. Strict 3-Folder Architecture Rule
The agent team must strictly build, organize, and read the project within the following three root directories. Never scatter files outside this layout:
• Folder 1: `1_COMPLETE_DOCUMENTATION/` - Houses the PRD, system design blueprints, interface specifications, and API data menus.
• Folder 2: `2_MAIN_CODING_FILES/` - Houses the lightweight application engine, UI source components, modules, and cross-device compilation files.
• Folder 3: `3_PROJECT_BACKUP_AND_DIARY/` - Houses old critical code block backups, records of added/deleted assets, changelogs, and rolling laboratory diary entries.

2. Diary Records & Permanent Code Tracking
• The Running Dictionary: Maintain a rolling master progress log inside `3_PROJECT_BACKUP_AND_DIARY/`. For every modification or terminal execution step, record a clear description of assets added, blocks deleted, files affected, and the engineering rationale behind the trajectory.
• Diary 1 (Master Audit Log): Logs completed actions and handover states.
• Diary 2 (Unconnected Pipeline & API Registry): Tracks API endpoints and flows awaiting integration.
• Diary 3 (Task Matrix): Real-time status for all tasks (PENDING | IN_PROGRESS | COMPLETED | BLOCKED).

3. Ultra-Lightweight Code & Global DRY Enforcement
• DRY (Don't Repeat Yourself) Shared Boxes: Never duplicate system logic across different codebase zones. If a feature (e.g., PDF compilation, data encoding, form validation) is needed across multiple app locations, extract it into a single, isolated Shared Service Module. Pass connection pathways to that module using parameter changes.
• Performance Target: Code must be engineered for fast rendering within micro-milliseconds, optimized layouts, lightweight dependencies, smooth view navigation, and snappy interface control profiles.

4. Reverse Engineering & Future-Proof Integration
• Hardware & Third-Party Portability: Build the architecture to remain entirely decoupled from external frameworks. Every logic block must be written using clear abstractions so that the application can seamlessly hook into physical hardware layers (e.g., RFID scanners, biometric machines) or e-commerce pipelines (Shopify, Instagram, GST tax calculation matrices) in future updates.
• In-Line Visual Connection Maps: At the start of every core functional block or API pipeline, inject a clear structural comment block specifying its usage areas and active links. This satisfies vulnerability protection against hidden data leak dependencies. Follow this comment block format:
  // START LOGIC BLOCK: [Feature Name]
  // ACTIVE CONNECTIONS SUMMARY: This module is currently linked to [X] operational areas:
  // 1. Endpoint A: [Details]
  // 2. Endpoint B: [Details]
  // [List all active connections so moving or breaking lines doesn't orphan hidden dependencies]

5. Resilient Failures (Universal Graceful Fallbacks)
• Fault-Tolerant Isolation: All external third-party tools, external APIs, and local state engines must be wrapped in catch statements with strict fallbacks. If an integration (e.g., Shopify API or a local device scanner) drops line or fails internally, the main application loop must never crash. Safely catch the anomaly, degrade performance gracefully, log the event to the project diary, and display a helpful manual entry message.

6. Senior Enterprise Gating & Security Guardrails
• Zero-Trust Environment Secret Management: Never write sensitive deployment passwords, cloud credentials, database strings, or authorization tokens directly into repository code strings. Always map secrets out into a root `.env` environment file and ensure it is safely listed in the `.gitignore` directory to avoid server leakage during public pushes.
• Automated GitHub Continuity Pipeline: Maintain real-time safety tracking. Every time an individual application feature achieves 100% code validation and compiler health, compile the terminal staging sequences to execute a clean commit and push the stable build up to the GitHub repository branch. Ensure the descriptive commit logs mirror our Diary Notes.
• Isolated Multi-Agent Gating: Code implementation tasks must utilize separate agent execution structures. The coding agent writes lines under plan mode limits, while a completely isolated Reviewer and Tester sub-agent triggers browser simulation testing scripts and code-quality checks before allowing code integration into the stable main branch.

7. Task Breakdown and Manager Commands
• When using `/architecture`, the Office Manager (Agent 04) MUST generate `tasks.md`. It must break down every task for all coding agents in a step-by-step format, showing how they communicate and connect.
• The Main Agent (Company Manager) never writes code, only orchestrates the 17 Sub-Agents and commands "Fix this, fix that".
