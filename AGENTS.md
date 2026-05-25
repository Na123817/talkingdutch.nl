# AGENTS.md

## Project Handoff: `talkingdutch`

This document is a practical handoff for any AI agent continuing work on this repository.

## 1) What this project is

Single-page marketing/landing site for **Talking Dutch** (online Dutch lessons).

- Stack: plain HTML/CSS/JS (no build system, no framework).
- Main file: `index.html`.
- Local image asset: `face.png`.
- Other files: `CNAME`, `file.svg`, `favicon.svg`.

## 2) Current repo shape

- `index.html` contains:
  - Full page structure for multiple sections/tabs (Home, About, Courseware, Packages, Syllabus, FAQ, Blog, Contact).
  - All CSS in `<style>` (same file).
  - All JS in `<script>` (same file).
- There is no `images/` folder currently. `face.png` is in repo root.
- Favicon is now served from root as `favicon.svg` (orange radiant dot with rounded orange frame).

## 3) Recent changes already applied

### Hero + portrait
- Home hero portrait was removed.
- Portrait is currently used in the About page only.
- A duplicated "Your Teacher" section with a second portrait block was removed.
- Portrait sources were fixed from `images/face.png` to `face.png`.

### Scroll animations
- Added scroll-reveal animations using `IntersectionObserver`.
- Added specific reveal behavior for cards (`.price-card`, `.hs-card`, `.testimonial-card`).
- Extended reveal coverage to previously static blocks in Home/Services/Courseware/Contact/Footer (while keeping the same animation style and observer-based performance approach).
- Added refresh hooks when switching tabs/opening blog article/switching syllabus tabs.

### Responsive (in progress)
- Home responsive received a first hardening pass:
  - pricing grid now degrades `3 -> 2 -> 1` columns (`<=960`, `<=860`),
  - "How it works" grid now degrades `3 -> 2 -> 1` columns (`<=960`, `<=860`),
  - mobile spacing/typography reduced for pricing and "How it works" cards (`<=600`).
- Fixed a reveal CSS precedence bug that kept `reveal-card` transform offsets after visibility, which caused a persistent "stair-step" alignment in narrow responsive views.
- About page "The language" cards were normalized with dedicated classes and now collapse to a centered single-column stack on narrow view (`<=960`) to avoid a visually awkward `2 + 1` layout.
- Packages page responsive hardening:
  - at narrow widths (`<=1080`) the Services layout stacks vertically,
  - package cards collapse to a single column,
  - flip interaction is disabled on narrow view (front face only) to avoid fixed-height clipping/overflow.
  - when flip is disabled in narrow mode, cards now use the same frame-hover treatment as the rest of the site (neutral top border -> orange top border on hover/focus).
- About page cards animation parity:
  - language cards and approach cards now share Home-like hover motion/transition behavior,
  - both sets are registered in the scroll-reveal card pipeline for consistent fade/slide entrance.
- About hover behavior was further normalized to match Home cards exactly:
  - neutral top border at rest,
  - orange top accent appears only on hover,
  - removed lift/translate effect that made hover feel inverted.
- Syllabus visual behavior was aligned to FAQ style:
  - module rows now use clean line-based separators (instead of boxed card blocks),
  - question/title hover and open-state accent mirrors FAQ interaction language,
  - lessons panel now opens/closes with smooth transition (max-height/opacity) instead of abrupt show/hide.
- Blog cards were normalized to the same frame-animation language used across the site:
  - dark neutral card base,
  - neutral top border at rest,
  - orange top border accent and subtle surface shift on hover,
  - removed old orange gradient/lift behavior for consistency.
- FAQ + Syllabus state reset:
  - when entering FAQ, any previously open question is closed,
  - when entering Syllabus, expanded modules/arrows are reset (collapsed),
  - prevents sticky expanded state when navigating away and back.
- Additional Syllabus typography normalization:
  - expanded lesson titles now match FAQ question typography style (Orbitron, uppercase, same sizing rhythm),
  - expanded lesson body text now uses the same readability baseline as FAQ answers (muted tone, larger line-height/size).
- Home readability pass:
  - increased contrast for `How it works` card descriptions and Booking section body copy (step descriptions + CTA helper text) to improve readability on dark background.

### Background effects
- Added subtle global animated background layers via `body::before` and `body::after`:
  - moving grid
  - orange glow drift
- Includes reduced-motion fallback.

### Local console fixes
- Removed Cloudflare email decode script include that failed in local `file://` mode.
- Fixed testimonial stars using HTML entities (`&#9733;`) to avoid encoding breakage.
- Updated testimonial ratings for credibility: Antzela `4.0`, Rory `4.5`, Matt `5.0`.
- Performed a full UTF-8 re-decode cleanup in `index.html` to fix mojibake across visible UI text and symbols.

## 4) Technical debt

- No technical debts pending at the moment.
- Responsive/layout, hover states, and cross-section visual consistency were addressed in the latest pass.

## 5) Known issues (important)

### `file://` security warning
If opened directly from filesystem, browser may show warnings for `file://` origin restrictions.

**Recommendation:** run via local server for testing.

## 6) How to run locally (recommended)

From project root:

- Python:
  - `python -m http.server 5500`
- Then open:
  - `http://localhost:5500/index.html`

This avoids most `file://` origin warnings.

## 7) Design/UX direction currently in code

- Dark theme with orange accent (`--accent: #e87722`).
- Futuristic/tech typography (`Orbitron` + `Barlow`).
- Subtle animated atmosphere (grid + glow).
- Scroll reveal effects on key cards and content blocks.

When extending UI, preserve this direction unless explicitly asked to re-theme.

## 8) Implementation notes

### JS architecture
- Everything is global functions in one script block.
- Tab routing is internal (`show('home')`, etc.).
- No module system.

### Scroll reveal
- Key functions:
  - `getRevealTargets()`
  - `initScrollReveal()`
  - `refreshScrollReveal()`
- Uses `IntersectionObserver` and adds `.reveal-scroll` / `.is-visible`.

## 9) Safe next tasks for a new AI agent

1. **Polish animation timing**
   - Tune reveal delays and distances.
   - Verify no jank on low-end devices.

2. **Responsive QA pass**
   - Validate layout continuity across Home, Syllabus, FAQ, Blog, and footer.
   - Confirm no bottom-gap artifacts remain on short-content tabs.

3. **QA pass**
   - Test all tabs/routes and blog article open/close behavior.
   - Verify no regressions in FAQ interactions and tab/article navigation behaviors.

## 10) Guardrails

- Do not introduce frameworks unless requested.
- Keep edits focused in `index.html` unless explicitly splitting files.
- Avoid destructive git operations.
- Preserve existing content/SEO metadata unless task asks otherwise.

## 11) Quick file references

- Main file: `index.html`
- Portrait asset: `face.png`
- Optional vector: `file.svg`

---

If you are a new AI agent: start by running a full-text search for `â` in `index.html` and clean visible UI strings first.
