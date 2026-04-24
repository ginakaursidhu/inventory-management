---
name: redesign-ui
description: Redesigns the App.vue layout from a top navigation bar to a modern SaaS-style sidebar navigation with CSS variables, consistent spacing, and a polished professional look — with subtle Taylor Swift era-inspired aesthetics woven into the palette. Use this skill when converting the app to a sidebar layout.
---

# Redesign UI: Top Nav → Sidebar Layout

This skill transforms the app shell from a horizontal top navigation bar into a modern SaaS-style sidebar navigation (Linear/Notion/Vercel style). The palette draws subtly from Taylor Swift's *Midnights* era (deep purple-navy sidebar, lavender active states) and *Fearless* era (antique gold accent on active items). These touches are tasteful and professional — not novelty. Follow every step exactly — do not skip sections or reorder them.

## 1. Scope & Constraints

### Files to modify
| File | Change |
|------|--------|
| `client/src/App.vue` | Main layout restructure + CSS variables |
| `client/src/components/FilterBar.vue` | Change `top: 70px` → `top: 0` |
| `client/src/components/ProfileMenu.vue` | Flip dropdown upward, restyle for dark sidebar |
| `client/src/components/LanguageSwitcher.vue` | Flip dropdown upward, restyle for dark sidebar |

### Files NOT to touch
- Any file in `client/src/views/`
- Any file in `client/src/composables/`
- `client/src/main.js`
- `client/src/api.js`
- `client/src/router.js`
- Anything in `server/`

### Mandatory delegation rule
Per CLAUDE.md: **ANY modification to a `.vue` file MUST be delegated to the `vue-expert` agent.** Never directly edit `.vue` files yourself. Prepare precise, complete instructions for each `vue-expert` call.

---

## 2. CSS Variables to Add to App.vue

Add this `:root` block at the very top of the `<style>` section in App.vue, before all other rules:

```css
:root {
  /* Sidebar layout — Midnights era palette */
  --sidebar-width: 252px;
  --sidebar-bg: #0d0c1d;              /* Midnights: deep purple-black */
  --sidebar-text: #8b85b8;            /* muted lavender-gray */
  --sidebar-text-hover: #e9e4f0;      /* near-white with lavender warmth */
  --sidebar-active-bg: rgba(167, 139, 250, 0.12); /* lavender tint */
  --sidebar-active-text: #c4b5fd;     /* lavender-300 */
  --sidebar-border: rgba(167, 139, 250, 0.1); /* lavender-tinted dividers */
  --sidebar-header-text: #f8f4ff;     /* barely-lavender white */

  /* Fearless: antique gold accent for active nav indicator */
  --color-accent-gold: #c9a227;

  /* Lucky number — easter egg */
  --ts-lucky: 13; /* 13 */

  /* Theme colors */
  --color-bg-body: #f8fafc;
  --color-bg-card: #ffffff;
  --color-text-primary: #0f172a;
  --color-text-secondary: #64748b;
  --color-text-muted: #94a3b8;
  --color-border: #e2e8f0;
  --color-border-hover: #cbd5e1;
  --color-primary: #7c3aed;           /* violet — Midnights primary */
  --color-primary-light: #f5f3ff;
  --color-primary-dark: #5b21b6;

  /* Shape tokens */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 10px;
  --shadow-sm: 0 1px 3px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.06);
}
```

---

## 3. App.vue Template Replacement

Replace the entire `<template>` block with:

```html
<template>
  <div class="app">
    <aside class="sidebar">
      <div class="sidebar-header">
        <h1>{{ t('nav.companyName') }}</h1>
        <span class="sidebar-subtitle">{{ t('nav.subtitle') }}</span>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" :class="{ active: $route.path === '/' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="2" width="7" height="7" rx="1"/>
            <rect x="11" y="2" width="7" height="7" rx="1"/>
            <rect x="2" y="11" width="7" height="7" rx="1"/>
            <rect x="11" y="11" width="7" height="7" rx="1"/>
          </svg>
          {{ t('nav.overview') }}
        </router-link>

        <router-link to="/inventory" :class="{ active: $route.path === '/inventory' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M10 2L2 6v8l8 4 8-4V6L10 2z"/>
            <path d="M2 6l8 4 8-4"/>
            <path d="M10 10v8"/>
          </svg>
          {{ t('nav.inventory') }}
        </router-link>

        <router-link to="/orders" :class="{ active: $route.path === '/orders' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <rect x="4" y="2" width="12" height="16" rx="1.5"/>
            <path d="M7 7h6M7 10h6M7 13h4"/>
          </svg>
          {{ t('nav.orders') }}
        </router-link>

        <router-link to="/spending" :class="{ active: $route.path === '/spending' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M2 14l4-5 4 3 4-6 4 4"/>
            <path d="M2 17h16"/>
          </svg>
          {{ t('nav.finance') }}
        </router-link>

        <router-link to="/demand" :class="{ active: $route.path === '/demand' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M2 14l5-5 4 4 7-9"/>
            <path d="M14 5h4v4"/>
          </svg>
          {{ t('nav.demandForecast') }}
        </router-link>

        <router-link to="/reports" :class="{ active: $route.path === '/reports' }">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="2" width="14" height="16" rx="1.5"/>
            <path d="M7 6h6M7 9h6M7 12h4"/>
            <path d="M7 15h2"/>
          </svg>
          Reports
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <div class="main-wrapper">
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>
```

The `<script>` section is **unchanged** — do not modify it.

---

## 4. App.vue CSS Changes

### Remove these rules entirely
- `.top-nav { ... }`
- `.nav-container { ... }`
- `.nav-container > .nav-tabs { ... }`
- `.nav-container > .language-switcher { ... }`
- `.logo { ... }`
- `.logo h1 { ... }`
- `.subtitle { ... }`
- `.nav-tabs { ... }`
- `.nav-tabs a { ... }`
- `.nav-tabs a:hover { ... }`
- `.nav-tabs a.active { ... }`
- `.nav-tabs a.active::after { ... }`

### Modify `.app`
```css
.app {
  display: flex;
  flex-direction: row;   /* changed from column */
  min-height: 100vh;
}
```

### Modify `.main-content`
```css
.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
  /* remove: max-width, margin: 0 auto, width: 100% */
}
```

### Add new sidebar rules
```css
.sidebar {
  width: var(--sidebar-width);
  min-width: var(--sidebar-width);
  background: var(--sidebar-bg);
  height: 100vh;
  position: sticky;
  top: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  z-index: 100;
}

.sidebar-header {
  padding: 1.5rem 1.25rem 1rem;
  border-bottom: 1px solid var(--sidebar-border);
}

.sidebar-header h1 {
  font-size: 1rem;
  font-weight: 700;
  color: var(--sidebar-header-text);
  letter-spacing: -0.025em;
  margin-bottom: 0.25rem;
}

.sidebar-subtitle {
  font-size: 0.75rem;
  color: var(--sidebar-text);
  font-weight: 400;
}

.sidebar-nav {
  flex: 1;
  padding: 0.75rem 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  overflow-y: auto;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.875rem;
  border-radius: var(--radius-sm);
  color: var(--sidebar-text);
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.15s ease;
}

.sidebar-nav a svg {
  flex-shrink: 0;
  opacity: 0.7;
  transition: opacity 0.15s ease;
}

.sidebar-nav a:hover {
  color: var(--sidebar-text-hover);
  background: var(--sidebar-active-bg);
}

.sidebar-nav a:hover svg {
  opacity: 1;
}

.sidebar-nav a.active {
  color: var(--sidebar-active-text);
  background: var(--sidebar-active-bg);
  border-left: 2px solid var(--color-accent-gold); /* Fearless gold */
  padding-left: calc(0.875rem - 2px); /* compensate for border width */
}

.sidebar-nav a.active svg {
  opacity: 1;
}

.sidebar-footer {
  padding: 0.75rem;
  border-top: 1px solid var(--sidebar-border);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-top: auto;
}

.main-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}
```

### Migrate global utilities to use CSS variables
Replace all hardcoded hex colors in the remaining global utility classes with CSS variables:

```css
/* .stat-card */
.stat-card {
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
}
.stat-card:hover {
  border-color: var(--color-border-hover);
}

/* .card */
.card {
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
}
.card-header {
  border-bottom: 1px solid var(--color-border);
}
.card-title {
  color: var(--color-text-primary);
}

/* table */
thead {
  background: var(--color-bg-body);
  border-top: 1px solid var(--color-border);
  border-bottom: 1px solid var(--color-border);
}
th {
  color: #475569;
}
td {
  border-top: 1px solid #f1f5f9;
  color: #334155;
}
tbody tr:hover {
  background: var(--color-bg-body);
}

/* body */
body {
  background: var(--color-bg-body);
  color: var(--color-text-primary);
}
```

---

## 5. FilterBar.vue Changes

In `client/src/components/FilterBar.vue`, change the sticky position:

```css
/* Before */
.filters-bar {
  top: 70px;
}

/* After */
.filters-bar {
  top: 0;
}
```

That is the only change needed in FilterBar.vue.

---

## 6. ProfileMenu.vue Changes

### Dropdown position: flip from downward to upward
```css
/* Before */
.dropdown-menu {
  top: calc(100% + 0.5rem);
  right: 0;
}

/* After */
.dropdown-menu {
  bottom: calc(100% + 0.5rem);
  left: 0;
  right: auto;
  top: auto;
}
```

### Button styling: adapt for dark sidebar background
```css
/* Before */
.profile-button {
  background: white;
  border: 1px solid #e2e8f0;
}
.profile-button:hover {
  background: #f8fafc;
  border-color: #cbd5e1;
}
.profile-name {
  color: #0f172a;
}
.chevron {
  color: #64748b;
}

/* After */
.profile-button {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.12);
  width: 100%;
  justify-content: flex-start;
}
.profile-button:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.2);
}
.profile-name {
  color: #e2e8f0;
}
.chevron {
  color: #64748b;
  margin-left: auto;
}
```

---

## 7. LanguageSwitcher.vue Changes

### Dropdown position: flip from downward to upward
```css
/* Before */
.dropdown-menu {
  top: calc(100% + 0.5rem);
  right: 0;
}

/* After */
.dropdown-menu {
  bottom: calc(100% + 0.5rem);
  left: 0;
  right: auto;
  top: auto;
}
```

### Button styling: adapt for dark sidebar background
```css
/* Before */
.language-button {
  background: white;
  border: 1px solid #e2e8f0;
  color: #334155;
}
.language-button:hover {
  background: #f8fafc;
  border-color: #cbd5e1;
}
.globe-icon {
  color: #64748b;
}
.chevron {
  color: #64748b;
}

/* After */
.language-button {
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.12);
  color: #94a3b8;
  width: 100%;
  justify-content: flex-start;
}
.language-button:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.2);
  color: #e2e8f0;
}
.globe-icon {
  color: #64748b;
}
.chevron {
  color: #64748b;
  margin-left: auto;
}
```

---

## 8. Execution Order

Execute these steps in sequence. Each `.vue` file change MUST be delegated to `vue-expert`.

**Step 1 — Read all files**
Read the current content of:
- `client/src/App.vue`
- `client/src/components/FilterBar.vue`
- `client/src/components/ProfileMenu.vue`
- `client/src/components/LanguageSwitcher.vue`

**Step 2 — Delegate App.vue to vue-expert**
Instruct `vue-expert` to:
1. Replace the entire `<template>` block with the template in Section 3
2. Leave `<script>` completely unchanged
3. In `<style>`: add `:root` CSS variables block (Section 2) at top
4. Remove the listed top-nav CSS rules (Section 4)
5. Modify `.app` to `flex-direction: row`
6. Modify `.main-content` (remove max-width centering)
7. Add all new sidebar CSS rules (Section 4)
8. Migrate global utilities to use CSS variables (Section 4)

**Step 3 — Delegate FilterBar.vue to vue-expert**
Instruct `vue-expert` to change `top: 70px` → `top: 0` in the scoped `.filters-bar` style.

**Step 4 — Delegate ProfileMenu.vue to vue-expert**
Instruct `vue-expert` to apply the dropdown position flip and button restyling from Section 6.

**Step 5 — Delegate LanguageSwitcher.vue to vue-expert**
Instruct `vue-expert` to apply the dropdown position flip and button restyling from Section 7.

**Step 6 — Verify**
Run through the verification checklist below.

---

## 9. Verification Checklist

After all changes are applied, confirm:

- [ ] Sidebar renders on the left, full viewport height, deep purple-black background (`#0d0c1d`)
- [ ] Sidebar is exactly 252px wide and sticks while page content scrolls
- [ ] All 6 nav links render with SVG icons and labels
- [ ] Active route link has lavender background tint (`rgba(167, 139, 250, 0.12)`) and lavender text (`#c4b5fd`)
- [ ] Active route link has a 2px antique gold left border (`#c9a227`) — the Fearless accent
- [ ] Inactive nav text has a lavender-gray tone (`#8b85b8`), not flat slate
- [ ] Clicking nav links navigates correctly (no 404s)
- [ ] LanguageSwitcher and ProfileMenu are at the sidebar bottom
- [ ] Both dropdowns open **upward** (above the button, not below)
- [ ] FilterBar is sticky at `top: 0` within the main content area
- [ ] All 6 views (Dashboard, Inventory, Orders, Finance, Demand, Reports) load without errors
- [ ] No console errors in the browser
- [ ] Layout fills full viewport width (no horizontal scroll)
- [ ] Modals (ProfileDetails, Tasks) still open and close correctly
- [ ] Main content area retains clean light background — TS palette is **sidebar-only**, not garish
