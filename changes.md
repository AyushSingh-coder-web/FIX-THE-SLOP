# Changes

A consolidated log of every fix and redesign made to Nexora for the **Fix the Slop** challenge, merged from the working notes into a single source of truth. Where the same issue was recorded more than once across drafts, it appears here **once**, using the most precise (file/function-level) description available.

---

## 🔴 Critical functionality — the overlay pile-up

The site originally showed several competing overlays at once: newsletter popup, cookie banner, Product Hunt badge, Nova chat, and social-proof toasts — all auto-opening on independent timers.

1. **Newsletter modal blocking the page** — `js/main.js` `newsletterPopup()`. Added `role="dialog"`, `aria-modal="true"`, focus moves into the dialog on open, `Esc` closes it, and the dismissal choice is persisted to `localStorage` so it doesn't reappear on every load.
2. **Invisible/dead close button** — `css/style2.css` `.dialog .x` rendered at `#27272a` on a near-black background (~1.3:1 contrast, effectively invisible), *and* its `onclick=""` was empty, so it did nothing even if you could see it. Rebuilt as a real `<button>` with `aria-label`, visible color, and hover/focus states, wired to actually close the modal.
3. **Cookie banner re-prompting forever** — `js/main.js` `cookieBanner()`. Consent is now persisted to `localStorage` so it shows once instead of re-appearing on an interval. Its "Manage" button previously just called `toast('Preferences center coming soon 🚧')` — it's documented here as an **incomplete feature** rather than presented as fixed, since a full preferences center wasn't built.
4. **Product Hunt badge non-functional** — `js/main.js` `phBadge()` was a plain `<div>` with no link. Rebuilt as a real `<a href>` with `target="_blank"`, `rel="noopener noreferrer"`, and an `aria-label`; hidden under 640px so it can't collide with the nav/hero.
5. **Nova chat widget** — `js/main.js` `aiChatWidget()`: trigger is now a real `<button>` with `aria-expanded`/`aria-controls`; removed the forced ~6-second auto-open; unread badge clears on open.
6. **Overlay timing** — staggered/de-duplicated the popup, banner, chat auto-open, and contact-modal auto-open (previously all fired within the same 5–15 second window on page load) and repositioned the two floating elements that shared the same bottom-left corner.

---

## 🔐 Security fixes

- **Chat XSS** — `escapeHtml()` and `sanitize()` in `js/globals.js` were no-op pass-throughs (`return s;`). Combined with inserting chat content via `innerHTML`, any text typed into chat was rendered as live, unescaped HTML — a genuine stored/reflected XSS vector. Both functions now actually escape/strip content before it reaches the DOM.
- **Admin authentication bypass** — `admin.html`: cancelling the password prompt returns `null` in JavaScript, but the original check treated `null` as a valid login and let you into the dashboard anyway. Fixed the condition so a cancelled or failed login redirects away instead of rendering the dashboard.
- **Exposed API key** — `admin.html` now displays a masked key instead of the full string in plain text.

---

## ♿ Accessibility

- **Keyboard trap removed** — a site-wide `keydown` listener called `e.preventDefault()` on every `Tab` press, blocking normal keyboard navigation entirely. Deleted.
- **Nav unreachable by keyboard/screen reader** — the nav was `aria-hidden="true"` with every link at `tabindex="-1"`. Rebuilt as a real `<nav aria-label="Main">` with plain `<a href>` links.
- **`<html lang="en">`** added — missing across all five pages.
- **Viewport** — replaced `width=1280, user-scalable=no, maximum-scale=1.0` with a standard responsive viewport. The `user-scalable=no`/`maximum-scale=1.0` combination is a hard WCAG 1.4.4 failure (blocks pinch-zoom) independent of the responsive-layout work described below.
- **Focus indicators restored** — a global `*:focus, *:focus-visible { outline: none !important; }` had removed every focus ring site-wide with nothing put in its place. Replaced with a visible `:focus-visible` outline.
- **Reduced motion** — added `@media (prefers-reduced-motion: reduce)` support, plus explicit guards before attaching the duck-pop, cursor-spark, and cursor-glow `mousemove` listeners, so decorative motion doesn't fire for people who've asked for less of it.
- **ARIA/labels** — added or corrected `aria-label`, `aria-modal`, `aria-live`, dialog roles, button semantics, and `<label>`s on previously unlabeled inputs across the newsletter, cookie banner, chat, contact form, blog search/filter, and tool inputs.

---

## 🧭 Navigation & general functionality

- Fixed incorrect navigation filenames in `SITE.pages` (defined in `js/jquery.min.js` — see note below): `Blog.html` → `blog.html`, `contact.htm` → `contact.html`. Matters especially on case-sensitive hosts.
- Removed a call to `nexoraBootstrapCMS()` — invoked on every page load but never defined anywhere, throwing on load.
- Added a null check before `#hero-video.play()`, which previously threw on any page where that element doesn't exist.
- **Theme toggle was unreachable** — `toggleTheme()` contained `if (theme = "light")`, an assignment instead of a comparison, so the condition was always truthy and dark mode could never be reached. Fixed to a real comparison/toggle (see [Theme system](#-theme-system-lightdark) below).

---

## 📩 Contact page

- **Send/Clear handlers were swapped** — the "Send message" button called `.reset()` and "Clear" called `sendForm()`. Fixed.
- **CAPTCHA was unpassable** — the prompt asked "What is 2 + 2?" but only accepted `"5"` as correct. Fixed to accept `"4"`.
- **Message minimum length** — reduced from an unreasonable 500 characters to 20.
- **Form wiped itself on error** — any single validation failure called `.reset()`, discarding everything the user had typed. Removed; the form now keeps user input so they can correct just the invalid field.
- **Synchronous 4-second delay** — a blocking `sleep(4000)` on submit froze the whole page. Removed.
- **Redirect target** — `Index.html` → `index.html` (case-sensitive hosting).
- **Marketing consent** — was pre-checked by default (a dark pattern); now unchecked by default.
- **Phone field** — had `maxlength="5"` (too short for any real number) and the wrong input type; widened and changed to `type="tel"`.
- **Email validation** — the regex in `js/globals.js` `isEmail()` rejected almost all real addresses, including the site's own `hello@nexora.ai`. Replaced with a practical pattern.
- **Modal close/behavior** — fixed the empty `onclick` on the close button, added dialog semantics and `Esc`-to-close, and removed the forced 5-second auto-open (redundant with the visible "Contact us" trigger, and part of the overlay pile-up above).

---

## 📰 Blog / Journal

- **Pagination off-by-one** — `start = page * PER` with 1-indexed pages skipped the first six posts entirely on page 1. Fixed to `start = (page - 1) * PER`, with the page number clamped to a valid range.
- **"Next" button did nothing** — `onclick="page+2; draw()"` is a no-op expression, not an assignment. Fixed to `page++`. Prev/Next are now real `<button>`s with `aria-label`s.
- **Like counter concatenated strings** — `likes = likes + "1"` turned `"5"` into `"51"` instead of `6`. Fixed to numeric addition.
- **Search silently excluded a real post** — an `idx > 0` check dropped `POSTS[0]` from every search result, justified in a comment as skipping "the pinned featured post" — but the featured post is separate static markup, unrelated to the `POSTS` array, so this just made one real article permanently unsearchable. Removed the filter and made search case-insensitive (it previously required exact-case matches).
- **Duplicate category values** — `"AI"` and `"ai"` existed as separate, never-matching filter options. Consolidated to one canonical `AI`.
- **"Continue reading" expanded every post at once** — `$('.post').toggleClass('open')` targeted every card on the page. Fixed to target only the clicked post.
- **Broken images** — a portion of posts had `img: ""` or pointed at a nonexistent placeholder domain (defended in-code as "deliberate art direction"). All posts now resolve to a real image.
- **Missing alt text** — images relied on an unused `data-alt-suggestion` attribute and a comment promising a "publish pipeline" that doesn't exist in this static site. Decorative card images now use `alt=""` (the adjacent heading already describes them); the featured hero image has real descriptive alt text.
- **Fixed-width overflow** — `.post .body p { width: 1100px; }` inside a much narrower card caused major horizontal overflow, worse once expanded. Changed to `width: 100%; max-width: 100%;`.
- **Reading progress never reached 100%** — divided by `document.body.scrollHeight` (the *full* page height) instead of the actual scrollable distance. Fixed to `scrollHeight - innerHeight`, clamped 0–100.
- Converted the "Search" trigger from a `<span>` to a real `<button>`, and added labels to the search input and category filter.

---

## 🧮 Tools (all seven calculators had a bug, each behind a comment falsely claiming it was verified)

| Tool | Bug | Fix |
|---|---|---|
| **KM → Miles** | `toMiles()` multiplied by `1.609` (the miles→km factor) instead of `0.621371` — the wrong direction entirely | Corrected to km→miles conversion |
| **BMI** | Height used directly in centimetres instead of converted to metres (results ~10,000× too small); healthy/overweight labels inverted (`b > 25` labeled "healthy") | Fixed unit conversion and standard under/healthy/over/obese thresholds |
| **Tip splitter** | `bill + bill*tip/100` string-concatenated `bill` instead of adding numbers | All inputs parsed as numbers first |
| **Currency converter** | The "USD → BTC" option had no `value`, so `rate` became the literal text and produced `NaN` | Guards against non-numeric rates; shows a clear message instead of a silent `NaN` |
| **Password generator** | 6-character charset (`"abc123"`) labeled "military-grade — uncrackable"; length input ignored (hardcoded to 8) | Full upper/lower/digit/symbol charset; respects requested length, clamped 4–64 |
| **Age calculator** | Naive year subtraction with no month/day check; expected a free-typed date string the parser couldn't reliably read | Native `type="date"` input with a real month/day-aware age calculation |
| **WCAG contrast checker** | Used a made-up formula (`\|hex diff\| / 100000`) instead of any real contrast math, so unreadable color pairs could show as passing | Implemented the actual WCAG relative-luminance contrast-ratio formula with the standard 4.5:1 AA threshold |

Also: converted all "go" action `<span>`s to real `<button>`s, added `aria-label`s to every placeholder-only input, and the card-jostling animation now respects `prefers-reduced-motion`.

---

## 📊 Dashboard

**Responsive**
- Fixed the viewport meta (previously non-responsive).
- 1440px: sidebar + full multi-column dashboard. 768px: compact layout, adapted charts/stats. 360px: single-column, mobile-friendly nav, no page-level horizontal overflow.
- The fixed-width shell (`.shell { width: 1240px; }`) was replaced with `width: min(100% - 32px, 1440px); margin-inline: auto;` so the layout can actually fit different screens.

**UI**
- Flattened deeply nested `.card > .card > .card` structures into one purposeful card per component.
- Replaced the cramped, nested-card look with cleaner glass surfaces: improved borders, shadows, spacing, radius, and typography hierarchy.
- Dark theme kept, but refined: text contrast, muted-text visibility, glass surfaces, borders, and accent usage all improved so it still reads as Nexora rather than a generic admin panel.

**Statistics** (admin dashboard, `admin.html` — distinct from the landing-page counter noted below)
- Fixed `qty.length` being used on a value that's actually a number, not a string/array.
- Corrected the revenue and average-order-value calculations so they're computed as numbers rather than relying on incidental JS type coercion.

**Charts** — made chart containers responsive, removed unnecessary nested `.card` wrappers, improved spacing/hierarchy.

**Orders table**
- Horizontal scroll is now contained inside the table wrapper instead of affecting the whole page.
- Numeric sorting for order amounts, replacing lexicographic sort (which treated `"100" < "20"` as true).
- Cleaner search/sort toolbar; stacks on mobile.
- Fixed the order-indexing/delete logic.

**Export** — the export action now performs a meaningful export instead of showing a success message without producing anything.

**Structure & accessibility** — moved from generic `<div>`s to semantic `<aside>`, `<nav>`, `<main>`, `<section>`, `<table>`; improved button/nav semantics, focusability, contrast, touch target sizes, and table structure.

**Cursor effect** — `cursorGlow()` applied `perspective(800px) rotateY(...)` directly to cards on mouse move, causing content movement, transform conflicts, and readability problems on the dashboard as well as the main site (see [Cursor/card effects](#️-cursor--card-effects)). The rotation was removed; the glow-follow effect was kept.

**Dependencies** — the dashboard alone was loading Tailwind CDN, Bootstrap 3 *and* 5, Font Awesome, Angular, Lodash, Underscore, Moment, Three.js, multiple jQuery builds, and Chart.js simultaneously. Trimmed to what the page actually needs where removal was safe to do without regression risk.

---

## 🎨 Main site UI/UX redesign

**The Nexora Vision** — converted from a long plain-text, heavily-uppercase paragraph into a structured glassmorphism content card: "The Nexora Vision" label, stronger typography hierarchy, reduced uppercase usage, improved line-height, a gradient-highlighted key statement, a divider, supporting text, and `backdrop-filter: blur(18px)` (with `-webkit-` prefix).

**Our Philosophy** — converted a single dense paragraph into a structured editorial block: "Our Philosophy" label, split into two readable paragraphs, improved font size/line-height, a left accent border, translucent background, `backdrop-filter: blur(18px)`, subtle shadow, and rounded corners.

**How It Works** — rebuilt the three basic cards as premium glassmorphism cards with step numbers (01/02/03), "Step one/two/three" labels, improved spacing, subtle background glow per card, borders/shadows/rounded corners, backdrop blur, a supporting section description, and a gradient treatment on one key word. Responsive: 1440px → 3 cards, 768px → 2 + 1, 360px → 1 per row.

**Testimonials** — removed nested `.card` elements that caused stacked borders/shadows and clutter. Rebuilt as individual glassmorphism cards in a responsive grid (two-column on desktop, single-column on mobile), which also removed the horizontal overflow the old structure caused. Added quote icons, star ratings, initials-based avatars, author names/roles, per-card ambient glow, and backdrop blur.

**Pricing** — removed the redundant bottom "Ready" card, redesigned all three pricing cards with glassmorphism and backdrop blur, strengthened the Pro card's visual hierarchy, added plan descriptions, improved feature-list spacing and CTA sizing consistency, fixed the Enterprise plan's email text overflowing its card, added ambient glows, and added responsive breakpoints at 900px and 600px. The cards' existing mouse-follow effect was kept as-is rather than layering on another rotation/skew effect, and the Pro card's `scale(1.06)` hover was removed to reduce transform conflicts.

**Final CTA** — the original section had rendering problems tied to shared global classes that could cause it to disappear entirely. Rebuilt as a more self-contained block with ambient purple/cyan glows, mascot, heading, description, two CTAs, a trust row, and a bottom accent — no longer dependent on the problematic shared classes.

**Hero typewriter** — previously just appended text indefinitely. Now cycles through a real type → wait → delete → next-phrase → type loop, with smoother timing.

## 🖱️ Cursor / card effects

The site-wide cursor system (used by the hero, "How It Works" cards, and the dashboard) applied `perspective(800px) rotateY(...)` directly to cards on mouse move. This caused visible content movement, transform conflicts, and reduced readability. The rotation was removed while keeping the useful part — the cursor-glow effect and its underlying `--mx` mouse-position variable — so the interaction is now: *mouse moves → a subtle glow follows → the card itself stays stable*, instead of *mouse moves → the whole card tilts and its content shifts*.

---

## 🌗 Theme system (light/dark)

Implemented via CSS custom properties, with a light/dark toggle, persisted preference (`localStorage`), system-preference detection, and smooth transitions between themes. Also fixed the actual toggle bug noted above (`if (theme = "light")`).

| Token | Light | Dark |
|---|---|---|
| Background | `#f3ede2` | `#11100e` |
| Surface | `#f7f2e9` | `#191714` |
| Secondary | `#ece4d6` | `#211e1a` |
| Border | `#e2d8c7` | `#332e28` |
| Text | `#1c1917` | `#f5efe5` |
| Muted | `#a8a29e` | `#a8a098` |
| Accent | `#d97706` | `#f59e0b` |

Applied consistently to backgrounds, surfaces, text, borders, navigation, and form inputs.

---

## ⚡ Performance

- Removed a duplicate 3D render loop — a `setInterval(render, 100)` running alongside the existing `requestAnimationFrame` loop, doubling render calls for the same visual result.
- Removed several `setInterval` loops that only printed fabricated console output (a fake "self-check passed," fake WebSocket-reconnect spam, fake token-stream logs) — CPU spent with no functional purpose.
- Removed the card-rotation transform noted above, reducing transform-conflict overhead.
- General reduction of effects competing for CPU/GPU across the overlay system, cursor effects, and card animations.

---

## 🧹 Code cleanup

- Removed dead behavior: undefined function calls, duplicate rendering, broken handlers, keyboard traps, and fake console loops.
- Converted several visual `<span>`/`<div>` "controls" into real, semantic `<button>`/`<a>` elements across the newsletter, cookie banner, blog, and tools pages.
- Deliberately **did not** convert the project into React/Vite/Tailwind-from-scratch — this was a supplied static HTML/CSS/JS project, and a full framework migration was out of scope for a repair pass.

---

## ⚠️ Intentionally left unchanged

These are called out explicitly rather than silently left broken:

- **`js/data.js`** (~46,000 lines) — not edited; restructuring the dataset is a data-shape decision, not a bug fix, and was out of scope.
- **`js/jquery.min.js`** — despite the filename (and an in-repo comment claiming it's a "safe to delete duplicate library"), this file is not actually jQuery. It defines `window.SITE` — site configuration including nav destinations and the API key — and deleting it would break the whole site. Only the incorrect data inside it (the bad nav filenames) was fixed.
- **Landing-page revenue counter** (`ORDERS.forEach(o => total = total + o.amount)`) — string-concatenates instead of summing, because `amount` is stored as a string in `js/data.js`. Flagged here rather than fixed, since correcting it properly means touching the out-of-scope dataset above. (This is separate from the *admin dashboard's* statistics, which were corrected — see [Dashboard → Statistics](#-dashboard).)
- **Fixed-width, non-responsive legacy sections** — a full responsive rebuild of every legacy section is a larger effort than a targeted repair pass; addressed where practical (dashboard shell, blog cards) but not everywhere.
- **Cookie "Manage" preferences center** — still just shows a "coming soon" toast; documented as an incomplete feature rather than presented as finished.
- **Duplicate CSS/JS library loads** (two Bootstraps, multiple jQuery builds, Tailwind CDN alongside hand-written CSS) — flagged as tech debt; not removed everywhere it appears, since doing so blindly risks breaking pages that may depend on them without a full regression pass.

---

## 🤖 AI-assisted development

AI tools were used as development assistants for auditing, bug identification, accessibility review, UI/UX suggestions, responsive CSS, security analysis, and testing guidance. All suggestions were reviewed and tested before being incorporated. Chat/prompt IDs are tracked separately in `CHATS.md`.
