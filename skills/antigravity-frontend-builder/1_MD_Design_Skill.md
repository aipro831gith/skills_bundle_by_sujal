# 1_MD_Design_Skill.md: Web SaaS Layout System

This modular front-end design skill is automatically selected by **Agent 05 (Front-End Builder)** when building Web Applications, SaaS Portals, or Cloud Dashboards.

---

## 📐 Layout Blueprint & Components

### 1. Macro Layout Structure:
* **Collapsible Left Sidebar:** Navigation links (Home, Analytics, Orders, Settings, Help). Includes toggle button to collapse to icon-only view on 768px screens.
* **Top Header Bar:** 
  * App Branding / Logo (left aligned).
  * Global Search Bar with keyboard shortcut `Cmd+K` / `Ctrl+K`.
  * Dark/Light Theme Toggle button (`data-theme="dark"` / `data-theme="light"` custom CSS properties).
  * User Account Profile Menu (Avatar, Name, Role Badge, Logout dropdown).
* **Main Content Area (CSS Grid):** Fluid responsive grid layout (`grid-template-columns: repeat(auto-fit, minmax(300px, 1fr))`) housing data cards, charts, and interactive widgets.

---

## 🎨 Interactive Button & UI Standards

### 3-State Interactive Buttons:
1. `default`: Clean elevation (`box-shadow: 0 2px 4px rgba(0,0,0,0.1)`), crisp typography, accessible HSL color contrast.
2. `hover`: `transition: all 0.2s ease-in-out; transform: scale(1.03); box-shadow: 0 4px 15px rgba(0,0,0,0.2);`
3. `active`: `transform: scale(0.98); box-shadow: 0 1px 2px rgba(0,0,0,0.1);`

### Loading & Feedback States:
* Buttons that trigger API calls MUST display an embedded SVG loading spinner / pulse animation upon click and temporarily disable further pointer events to prevent double-submits.
