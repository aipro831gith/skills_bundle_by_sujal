# 2_MD_Design_Skill.md: Mobile Flutter/Android Layout System

This modular front-end design skill is automatically selected by **Agent 05 (Front-End Builder)** when building Mobile Applications, Android APKs, iOS builds, or Flutter cross-device code.

---

## 📱 Mobile Layout Blueprint & Touch UI

### 1. Mobile Component Structure:
* **Bottom Navigation Bar:** 4 to 5 core navigation destinations (Home, Feeds, Orders, Profile). Includes subtle active icon elevation and ripple animation on tap.
* **Sliver App Bar:** Collapsible top app bar that shrinks smoothly on scroll down to maximize screen real estate.
* **Card-Based Scroll Feeds:** Infinite scroll list view with rounded card containers (`border-radius: 16px`), soft drop shadows, and pull-to-refresh gesture support.
* **Floating Action Button (FAB):** Prominent bottom-right action button for primary tasks (e.g. `+ Add New Order`).

---

## 🎨 Touch Gesture & Interactive Standards

* **Touch Target Size:** All touchable targets MUST be at least 48x48 dp to ensure accessibility.
* **Haptic & Visual Ripple Feedback:** Tapping buttons or list items triggers a subtle Material/Cupertino ripple effect.
* **Dark Theme Adaptation:** Automatically reads device system theme setting (`MediaQuery.of(context).platformBrightness`) and applies high-contrast dark theme surfaces.
