# Front-End Builder Sub-Skill (`frontend_builder.md`)

This file is automatically loaded and executed by the **Front-End Builder Sub-Agent** during `/build-all`.

## 1. Automated Design & Layout Selection (Human Taste System)
The system automatically analyzes the product spec (`04_ui_ux_design_system.md` and `master_spec.md`) to determine the target application type and select the optimal human-taste layout design system:

### Layout Pattern Selection:
*   **Web Applications (SaaS / Portals):** Modern Dashboard layout with collapsible sidebar menu on the left, top header bar (Account profile, light/dark theme switch, notifications), and a fluid CSS Grid main content area.
*   **Mobile Apps (Android / iOS Flutter):** Bottom Navigation Bar (Home, Analytics, Orders, Profile), responsive sliver app bar, card-based feeds, and smooth touch gestures.
*   **Enterprise ERP / Financial Dashboards:** Multi-tab layout, dense data tables with pagination, sortable columns, high-contrast dark mode, and quick-action toolbar.

---

## 2. Interactive UI & Micro-Animations Mandates
*   **3-State Button Rule:** Every interactable element MUST implement 3 clear visual states:
    1. `default`: Clean elevation, crisp typography, accessible color contrast.
    2. `hover`: 0.2s ease-in-out transition, 1.03x scale transform, subtle shadow expansion, brightness boost.
    3. `active`: 0.98x scale compress, shadow reduction.
*   **Theme Switch Mechanics:** Built-in CSS custom properties system mapped to `data-theme="dark"` / `data-theme="light"`. Inverts background, surface, text, and accent colors without page reloads.
*   **Feedback & Spinner Mechanics:** Any button that triggers an API call MUST automatically display a loading spinner / progress pulse upon click and prevent double-clicks.
*   **Zero-Hardcoding Boundary:** Client-side JavaScript/Dart MUST NOT contain hardcoded API keys or secret tokens. All endpoint URLs must reference configurable environment variables.
