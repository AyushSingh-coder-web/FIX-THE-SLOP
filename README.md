# Nexora — Fix the Slop

**A repaired and redesigned static web project submitted for the NIET Coding Cadets — *Fix the Slop* web-development challenge.**

![Status](https://img.shields.io/badge/status-submission-blue)
![Stack](https://img.shields.io/badge/stack-HTML%20%7C%20CSS%20%7C%20JavaScript-orange)
![Theme](https://img.shields.io/badge/theme-light%20%2F%20dark-8b5cf6)
![Accessibility](https://img.shields.io/badge/a11y-improved-22c55e)
![License](https://img.shields.io/badge/license-unspecified-lightgrey)

The project started as an intentionally flawed Nexora website containing broken functionality, accessibility barriers, incorrect calculations, poor responsive behavior, overlay conflicts, unnecessary effects, and UI/UX inconsistencies.

This submission focuses on **repairing real issues** — usability, accessibility, responsiveness, performance, security, and visual quality — while preserving the original Nexora identity and core purpose.

> **Don't just make the website look better — make it actually work better.**

---

## ✨ Highlights

| | |
|---|---|
| ♿ | Improved accessibility and keyboard navigation |
| 📱 | Responsive improvements for 360px, 768px and 1440px |
| 🌙 | Light/dark theme improvements |
| 🎨 | Redesigned glassmorphism UI sections |
| 🐛 | Fixed broken forms and interactions |
| 🧮 | Fixed all seven calculator/tool issues |
| 📰 | Repaired blog pagination, search and interactions |
| 📊 | Improved dashboard calculations, tables and responsiveness |
| 🔐 | Fixed an actual XSS vulnerability in the chat system |
| 🔒 | Fixed an admin authentication bypass |
| ⚡ | Removed unnecessary render loops and fake background timers |
| 🎯 | Fixed multiple overlay and modal conflicts |
| 🧭 | Repaired broken navigation links |
| 🖱️ | Reduced problematic card rotation effects |
| 🧹 | Improved semantic HTML and code structure |

---

## 📖 Table of Contents

- [Project Overview](#-project-overview)
- [Competition Context](#-competition-context)
- [Key Features](#-key-features)
- [Pages](#-pages)
- [Technology](#-technology)
- [Major Fixes](#-major-fixes)
- [Security Fix — XSS](#-security-fix--xss)
- [Admin Authentication Fix](#-admin-authentication-fix)
- [Navigation Fixes](#-navigation-fixes)
- [Accessibility Improvements](#-accessibility-improvements)
- [UI/UX Improvements](#-uiux-improvements)
- [Card Interaction](#️-card-interaction)
- [Responsive Design](#-responsive-design)
- [Blog / Journal Improvements](#-blog--journal-improvements)
- [Blog Light/Dark Theme](#-blog-lightdark-theme)
- [Tools Improvements](#-tools-improvements)
- [Dashboard Improvements](#-dashboard-improvements)
- [Performance Improvements](#-performance-improvements)
- [Code Quality](#️-code-quality)
- [Project Structure](#-project-structure)
- [Getting Started](#️-getting-started)
- [Deployment](#-deployment)
- [Testing Checklist](#-testing-checklist)
- [Known Limitations](#️-known-limitations)
- [AI-Assisted Development](#-ai-assisted-development)
- [Change Summary](#-change-summary)
- [Competition Evaluation Focus](#-competition-evaluation-focus)
- [Final Status](#-final-status)
- [Credits](#-credits)

---

## 🚀 Project Overview

Nexora is a modern digital/AI-oriented web platform concept containing multiple interconnected pages and experiences.

The supplied project was intentionally designed with numerous defects for the *Fix the Slop* challenge. The objective of this submission was to:

1. Audit the existing implementation.
2. Identify genuine functional problems.
3. Repair broken interactions.
4. Improve accessibility.
5. Improve responsiveness.
6. Improve UI/UX.
7. Reduce unnecessary performance overhead.
8. Fix incorrect calculations.
9. Improve security.
10. Preserve the original purpose and Nexora visual identity.

This was approached as a **targeted repair and enhancement pass**, rather than an unnecessary full rewrite.

## 🏆 Competition Context

This project was prepared for the **NIET Coding Cadets — Fix the Slop** web-development challenge.

The challenge evaluates:

- UI/UX
- Accessibility
- Functionality
- Theme
- Performance
- Technology / code quality
- Responsiveness

The project was reviewed from both a visual and functional perspective.

## ✨ Key Features

**Main Website**
Hero section · Nexora Vision · Philosophy section · How It Works · Testimonials · Pricing · Final CTA · Navigation · Footer · Newsletter · Cookie controls · Nova chat widget

**Blog / Journal**
Featured article · Search · Category filtering · Pagination · Like functionality · Expandable articles · Reading progress · Responsive article cards · Light/dark theme support

**Tools** — seven functional tools:
- KM → Miles converter
- BMI calculator
- Tip splitter
- Currency converter
- Password generator
- Age calculator
- WCAG contrast checker

**Dashboard**
Statistics · Revenue · Orders · Charts · Search · Sorting · Export · Responsive table · Navigation · AI/status sections

**Contact**
Contact form · Validation · CAPTCHA · Consent control · Accessible dialog behavior · Clear/submit functionality

## 📄 Pages

| Page | Purpose |
|---|---|
| `index.html` | Main Nexora landing page |
| `blog.html` | Nexora Journal/blog |
| `tools.html` | Utility tools and calculators |
| `contact.html` | Contact and enquiry form |
| `admin.html` | Dashboard/admin interface |

## 🛠 Technology

The project is primarily a static HTML/CSS/JavaScript application.

**Core:** HTML5 · CSS3 · JavaScript · jQuery

**Existing/used libraries** (depending on page): Bootstrap · Tailwind CDN · Font Awesome · Lodash · Underscore · Moment.js · Three.js · GSAP · Chart.js

The dependency set was audited during the repair process. Where dependency removal could introduce regression risk, libraries were intentionally retained rather than removed blindly.

---

## 🔧 Major Fixes

### 1. Newsletter Modal

**Problems:** blocked the page · non-functional close control · poor close-button contrast · could repeatedly appear

**Fixes:**
- Added dialog semantics
- Added `aria-modal`
- Added focus handling
- Added <kbd>Esc</kbd> support
- Repaired close functionality
- Persisted user choice using `localStorage`
- Reduced repeated popup behavior

### 2. Cookie Banner

**Problems:** could repeatedly reappear · overlapped other floating UI · non-functional Manage action

**Fixes:**
- Persisted consent
- Prevented unnecessary repeated prompting
- Repositioned competing floating elements
- Identified the missing preferences center as a remaining limitation

### 3. Product Hunt Badge

The original Product Hunt element was only a visual `<div>`. It was converted into a semantic link with a real destination, `target="_blank"`, `rel="noopener"`, and an accessible label. It's also hidden on smaller screens to prevent layout collisions.

### 4. Nova Chat

- Uses a semantic `<button>`
- Adds `aria-expanded`
- Adds `aria-controls`
- Removed forced automatic opening
- Clears unread state when opened

---

## 🔐 Security Fix — XSS

One of the most important issues discovered was in the chat system. The original `escapeHtml()` and `sanitize()` functions effectively returned input **without** escaping it, and user-controlled content was inserted into the DOM using `innerHTML`. This created a genuine cross-site scripting risk.

**Fix:** implemented actual HTML escaping/sanitization before inserting user-controlled content, preventing chat input from being interpreted as arbitrary HTML.

## 🔐 Admin Authentication Fix

The admin authentication flow contained a bypass — cancelling the password prompt returned `null`, and the existing condition could incorrectly allow access. The authentication logic was corrected; failed authentication no longer leaves the dashboard accessible.

## 🧭 Navigation Fixes

Several navigation destinations contained incorrect filenames, including `Blog.html` and `contact.htm`. These were corrected to the actual project filenames — particularly important on case-sensitive hosting environments.

---

## ♿ Accessibility Improvements

**Language** — added `<html lang="en">` to affected pages.

**Viewport** — replaced restrictive viewport configuration with:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

This restores normal mobile scaling and pinch zoom.

**Keyboard Navigation** — removed the global Tab-navigation blocker. Previously:

```js
if (e.key === "Tab") {
    e.preventDefault();
}
```

prevented normal keyboard navigation. Tab navigation now works.

**Focus Indicators** — the project previously removed all focus indicators. Visible `:focus-visible` states were restored.

**Reduced Motion** — added support for `@media (prefers-reduced-motion: reduce)`, applied to animations, cursor effects, marquee behavior, card effects, and decorative motion.

**Semantic Controls** — visual `<span>`s used as controls were converted to real `<button>`s where appropriate.

**Labels** — improved labels for search, category filters, forms, newsletter, contact fields, and tool inputs.

**Dialog Accessibility** — improved modal/dialog behavior with `role="dialog"`, `aria-modal`, `aria-label`, and keyboard close support.

---

## 🎨 UI/UX Improvements

The visual redesign focused on maintaining the Nexora identity while improving hierarchy and readability.

**Nexora Vision** — converted from a long text section into a structured glassmorphism content block: "The Nexora Vision" label, stronger typography hierarchy, better line-height, reduced excessive uppercase styling, gradient emphasis, divider, supporting text, translucent background, backdrop blur.

**Our Philosophy** — redesigned from a large text block into a structured editorial section: label, paragraph separation, improved typography and line-height, accent border, translucent background, rounded corners, backdrop blur, subtle shadow.

**How It Works** — the three basic cards were redesigned as premium glassmorphism cards with step numbers, step labels, improved spacing/typography, ambient glows, borders, shadows, rounded corners, backdrop blur, supporting description, and gradient-highlighted wording. Responsive layout: 1440px → 3 cards, 768px → 2+1 cards, 360px → 1 card.

**Testimonials** — removed nested card clutter, created individual glassmorphism cards, added responsive two-column (mobile: single-column) layout, reduced horizontal overflow, added ambient glows, improved typography, quote icons, star indicators, initials avatars, author roles, and backdrop blur.

**Pricing** — removed the redundant bottom "Ready" card, redesigned pricing cards with glass surfaces and backdrop blur, strengthened Pro visual hierarchy, added plan descriptions, improved pricing typography and feature spacing, fixed Enterprise email overflow, standardized CTA sizing, added ambient glows and responsive breakpoints.

## 🖱️ Card Interaction

The original cursor system applied perspective rotation directly to cards (`perspective(800px) rotateY(...)`), which could cause content movement, transform conflicts, visual instability, and reduced readability.

Card rotation was removed/reduced while retaining the useful cursor-glow effect — the resulting interaction is intentionally more stable.

---

## 📱 Responsive Design

Responsive behavior was reviewed around the competition's target widths: **360px · 768px · 1440px**.

Focus areas: navigation, cards, pricing, testimonials, dashboard, tables, forms, buttons, blog, typography, overflow, touch targets.

The goal was to prevent unnecessary page-level horizontal scrolling and maintain usable layouts on mobile and tablet screens.

---

## 📰 Blog / Journal Improvements

The Journal page received a dedicated functionality and responsive review.

**Pagination Fix** — original logic incorrectly used `start = page * PER;` for a 1-indexed page system, which skipped the first six posts on page one. Fixed to:

```js
start = (page - 1) * PER;
```

Page values are also clamped to valid ranges.

**Next Button** — original `page + 2;` did not modify the page number. Fixed to `page++;`. Previous/Next controls are real buttons with accessible labels.

**Like Counter** — fixed string concatenation so likes increment numerically.

**Search** — removed the incorrect filter that excluded the first post; search is now case-insensitive.

**Category Filter** — removed duplicate case-sensitive category values; the system now uses a canonical `AI` value.

**Continue Reading** — the original action could expand *all* posts; it now expands only the selected article.

**Broken Images** — posts with empty or invalid image URLs were repaired with valid image sources.

**Alternative Text** — article images use appropriate alternative-text handling. Decorative images use `alt=""` when the nearby heading already provides the relevant information; the featured article image has descriptive alternative text.

**Overflow** — the original blog article body had a fixed width larger than its card. Changed to:

```css
width: 100%;
max-width: 100%;
```

**Reading Progress** — corrected the progress calculation to use `document.body.scrollHeight - window.innerHeight` instead of the entire document height, clamped between 0 and 100%.

## 🌙 Blog Light/Dark Theme

The Journal now uses CSS custom properties for its theme system.

| Token | Light | Dark |
|---|---|---|
| Background | `#f3ede2` | `#11100e` |
| Surface | `#f7f2e9` | `#191714` |
| Secondary | `#ece4d6` | `#211e1a` |
| Border | `#e2d8c7` | `#332e28` |
| Text | `#1c1917` | `#f5efe5` |
| Muted | `#a8a29e` | `#a8a098` |
| Accent | `#d97706` | `#f59e0b` |

**Theme functionality:** light/dark toggle · persistent preference (`localStorage`) · system preference detection · smooth theme transitions · theme-aware surfaces, text, borders, navigation and inputs.

The editorial visual identity remains intact in both modes.

---

## 🧮 Tools Improvements

All seven calculator/tool experiences contained defects.

| Tool | Problem | Fix |
|---|---|---|
| **KM → Miles** | Used the miles-to-km factor in the wrong direction | Implemented correct km-to-mile conversion |
| **BMI Calculator** | Centimetres treated as metres; category logic inverted | Correct height conversion and category thresholds |
| **Tip Splitter** | Numeric inputs were strings and could concatenate during arithmetic | All numeric inputs parsed before calculation |
| **Currency Converter** | USD → BTC lacked a numeric value, could produce `NaN` | Added validation and clearer error handling |
| **Password Generator** | Tiny character set; hardcoded length; requested length ignored; misleading strength text | Uses upper/lowercase, numbers and symbols; respects requested length (4–64 chars) |
| **Age Calculator** | Naive year subtraction; incorrect handling of later birthdays; unreliable date parsing | Native date input with month/day-aware age calculation |
| **WCAG Contrast Checker** | Contrast calculation was not a valid WCAG calculation | Implemented relative luminance and contrast-ratio per WCAG methodology, using the standard 4.5:1 AA threshold |

---

## 📊 Dashboard Improvements

**Responsive Dashboard**
- **1440px** — full sidebar, full dashboard, multi-column statistics, full chart/table layout
- **768px** — compact layout, responsive statistics, adapted charts
- **360px** — single-column layout, mobile-friendly navigation, reduced unnecessary horizontal overflow

**Dashboard UI** — improved borders, shadows, spacing, radius, typography, section hierarchy, glass surfaces, card structure.

**Statistics** — reviewed and corrected total products, orders, revenue, and average order value. Numeric values are handled as numbers rather than relying on JavaScript coercion.

**Charts** — improved responsive containers, spacing, visual hierarchy, and smaller-screen behavior.

**Orders Table** — improved responsive table wrapper, mobile scrolling, search controls, sorting, order indexing, delete behavior, and numeric amount sorting. Horizontal scrolling is contained inside the table instead of affecting the entire page.

**Search and Sorting** — improved search behavior, sorting consistency, numeric sorting, and mobile toolbar layout.

**Export** — improved the dashboard export operation so it performs a meaningful export rather than presenting a misleading success state.

**Semantic Structure** — improved structure using `<aside>`, `<nav>`, `<main>`, `<section>`, `<table>` where appropriate.

**Dashboard Accessibility** — improved buttons, navigation, focus states, contrast, touch targets, table semantics, and responsive text.

---

## ⚡ Performance Improvements

**Removed duplicate 3D rendering** — the project had both `requestAnimationFrame()` and a redundant `setInterval(render, 100)`. The duplicate rendering loop was removed.

**Removed fake console loops** — removed unnecessary timers producing fake self-check logs, WebSocket reconnect messages, and token-stream messages that consumed resources without providing user-facing functionality.

**Reduced unnecessary effects** — reviewed cursor effects, card transforms, automatic popups, floating notifications, and repeated animations, aiming to retain visual personality without compromising usability.

## 🛡️ Code Quality

- **Targeted fixes** — working functionality was not rewritten without a reason
- **Semantic HTML** — prefer semantic elements where appropriate
- **Accessibility-first interactions** — clickable elements should behave like actual interactive controls
- **Avoid unnecessary animation** — decorative effects should not interfere with content
- **Preserve original functionality** — the purpose of the original application was retained
- **Validate behavior** — visual appearance alone was not treated as proof that a feature worked

---

## 📁 Project Structure

```
Nexora/
│
├── .github/
│
├── css/
│   ├── style.css
│   ├── style2.css
│   ├── final.css
│   └── ...
│
├── js/
│   ├── main.js
│   ├── globals.js
│   ├── data.js
│   ├── jquery.min.js
│   └── ...
│
├── index.html
├── blog.html
├── tools.html
├── contact.html
├── admin.html
│
├── AGENTS.md
├── CLAUDE.md
├── GEMINI.md
├── DESIGN.md
├── RULEBOOK.md
├── CHANGES.md
├── CHATS.md
├── README.md
└── llms.txt
```

## ⚙️ Getting Started

This is primarily a static HTML/CSS/JavaScript project — no React/Vite build pipeline is required.

**Option 1 — Python HTTP server**

```bash
py -m http.server 5500
```

Then open [http://localhost:5500](http://localhost:5500).

**Option 2 — VS Code Live Server**

Open the project in VS Code and launch the root folder using a local development server.

> **Important:** opening HTML files directly with `file:///` may cause problems with some browser APIs and local resources. A local HTTP server is recommended.

## 🌐 Deployment

Compatible with static hosting platforms such as Netlify, Vercel, GitHub Pages, Cloudflare Pages, or any static web server. No Node.js build step is required for the current static implementation.

---

## 🧪 Testing Checklist

Before final submission, the following areas should be manually verified.

**Desktop (1440px)**
- [ ] Navigation
- [ ] Hero
- [ ] Vision
- [ ] Philosophy
- [ ] How It Works
- [ ] Testimonials
- [ ] Pricing
- [ ] Final CTA
- [ ] Footer

**Tablet (768px)**
- [ ] Cards
- [ ] Navigation
- [ ] Forms
- [ ] Blog
- [ ] Dashboard

**Mobile (360px)**
- [ ] No unintended horizontal page overflow
- [ ] Touch targets
- [ ] Typography
- [ ] Tables
- [ ] Forms
- [ ] Buttons
- [ ] Navigation

**Accessibility**
- [ ] Tab navigation
- [ ] Visible focus
- [ ] Escape closes dialogs
- [ ] Labels
- [ ] ARIA states
- [ ] Zoom
- [ ] Reduced motion
- [ ] Screen-reader semantics

**Functionality**
- [ ] Newsletter
- [ ] Cookie consent
- [ ] Chat
- [ ] Navigation
- [ ] Contact form
- [ ] Search
- [ ] Filters
- [ ] Pagination
- [ ] Likes
- [ ] Tools
- [ ] Dashboard
- [ ] Export
- [ ] Theme toggle

---

## ⚠️ Known Limitations

Some areas were intentionally not changed because they require larger architectural work or carry unnecessary regression risk.

- **`js/data.js`** — the large dataset was not rewritten; data restructuring is outside the scope of a targeted repair pass.
- **`js/jquery.min.js`** — despite its filename, this file contains site configuration rather than the normal jQuery library (`window.SITE`, navigation destinations, API-related configuration), so it was retained.
- **Legacy dependency duplication** — the project contains overlapping dependencies. These were reviewed but not blindly removed, since existing pages may depend on them; a complete dependency migration would be a separate refactoring project.
- **Legacy fixed-width sections** — some original layouts were built around desktop-first dimensions. Responsive behavior was improved where practical, but a complete redesign of every legacy section was intentionally avoided.
- **Cookie Preferences** — the original Manage button did not contain a complete preference center. This was identified as an incomplete feature rather than being falsely represented as fully implemented.

## 🤖 AI-Assisted Development

AI tools were used as development assistants during the *Fix the Slop* repair process, for: code auditing, bug identification, accessibility review, UI/UX improvements, responsive CSS, functionality fixes, security analysis, refactoring suggestions, testing guidance, and theme implementation.

All AI-generated suggestions were reviewed and tested before being incorporated into the project. The complete AI conversations and change records are maintained separately according to the competition requirements.

**AI documentation:** `CHATS.md` · `CHANGES.md` · `chats/`

**Prompt / Chat IDs** *(replace with the actual ChatGPT/AI prompt IDs used during the competition)*:

```
Chat 01 — Initial audit
Chat 02 — Overlay fixes
Chat 03 — Accessibility fixes
Chat 04 — Blog fixes
Chat 05 — Tools fixes
Chat 06 — Dashboard improvements
Chat 07 — UI/UX redesign
Chat 08 — Final review
```

---

## 📋 Change Summary

| Category | Major Changes |
|---|---|
| Functionality | Fixed forms, pagination, calculators, navigation, chat, modals |
| Accessibility | Keyboard navigation, focus, labels, ARIA, zoom, reduced motion |
| Security | Fixed chat XSS risk and admin authentication bypass |
| UI/UX | Vision, Philosophy, Testimonials, Pricing, Dashboard redesign |
| Responsive | Improved 360px, 768px and 1440px layouts |
| Blog | Pagination, search, likes, filters, images, overflow |
| Tools | Fixed all seven calculator/tool defects |
| Dashboard | Stats, tables, sorting, export, responsive behavior |
| Theme | Light/dark theme system |
| Performance | Removed redundant rendering and fake timers |
| Code Quality | Semantic controls and targeted refactoring |

## 🏁 Competition Evaluation Focus

| Evaluation Area | Improvements |
|---|---|
| UI/UX | Glassmorphism, typography, spacing, hierarchy, CTA improvements |
| Accessibility | Focus, keyboard navigation, ARIA, labels, zoom, reduced motion |
| Functionality | Forms, pagination, calculators, search, navigation, dashboard |
| Theme | Consistent Nexora visual language and light/dark support |
| Performance | Removed redundant loops and unnecessary background work |
| Tech / Code Quality | Semantic HTML, targeted fixes, safer DOM handling |
| Responsiveness | Mobile/tablet/desktop improvements |

## 📌 Final Status

The project has undergone a substantial repair and polish pass covering functionality, accessibility, security, UI/UX, responsiveness, performance, and code quality.

The original Nexora purpose and visual identity were preserved while broken or misleading behavior was repaired.

### Development Philosophy

> Don't just make the website look better — make it actually work better.

```
Audit
  ↓
Identify real problems
  ↓
Prioritize functionality & accessibility
  ↓
Fix safely
  ↓
Improve UI/UX
  ↓
Test responsive behavior
  ↓
Review performance
  ↓
Verify before submission
```

---

## 📜 Credits

**Project:** Nexora
**Competition:** NIET Coding Cadets — Fix the Slop
**Development:** Built and repaired as part of the competition submission.
**AI Assistance:** AI tools were used as development assistants for auditing, debugging, accessibility review, UI/UX improvements, responsive implementation and code review.

---

<div align="center">

**⭐ Nexora**
*Build faster. Think bigger.*

</div>
