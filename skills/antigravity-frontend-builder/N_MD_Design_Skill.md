# N_MD_Design_Skill.md: Extensible Design Skill Template

This file defines the extensible architectural pattern for adding future front-end design skills (`4_MD_Design_Skill.md`, `5_MD_Design_Skill.md`, etc.) to **Agent 05 (Front-End Builder)**.

---

## 🧩 How to Add a New Design Skill (`N_MD_Design_Skill.md`)

To add a new custom design skill (e.g. 3D WebGL Canvas, E-Commerce Storefront, Chat Interface):

1. **File Naming Convention:** Name your file sequentially inside `skills/antigravity-frontend-builder/`:
   `4_MD_Design_Skill.md`, `5_MD_Design_Skill.md`, `6_MD_Design_Skill.md`...

2. **Required Structural Sections:**
   * **Target Domain:** Specify when Agent 05 should select this skill (e.g., E-Commerce, 3D WebGL).
   * **Macro Layout Blueprint:** Grid/Flexbox container rules.
   * **Interactive Micro-Animations:** Hover, active, focus, and loading states.
   * **Color Palette & Theme Variables:** CSS custom property mapping.

3. **Automatic System Selection:** Agent 05 dynamically scans `skills/antigravity-frontend-builder/` during `/build-all`, matches `04_ui_ux_design_system.md`, and applies the appropriate `N_MD_Design_Skill.md` automatically!
