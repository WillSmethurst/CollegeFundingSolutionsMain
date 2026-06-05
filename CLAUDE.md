# Strategy West College Planning — Working Notes for Claude

## Project

Public marketing site for **Strategy West College Planning**, a premium, advisor-led college planning advisory firm serving parents of students in grades 9–12.

- Static HTML, no build step. Each `.html` file at the repo root is a standalone page (full inline `<style>` and `<script>` per file — no shared CSS or JS files).
- Hosted on GitHub, auto-deployed by Netlify. **Any push to `origin/main` goes live.**

## Workflow (read this first)

- Make edits locally and create local commits with a clear, descriptive message.
- **Never push to `origin` unless explicitly told to push.** The push is the go-live trigger; every change is reviewed locally and approved before publishing.
- With each change, provide the commit message used or proposed for review.

## Editing Discipline

- **Surgical changes only.** Every edit must leave all other HTML, CSS, and JavaScript completely untouched. State the scope boundaries of each change before making it.
- For any non-trivial change, analyze the relevant file and explain the approach before writing code.
- Prefer targeted find-and-replace edits for single-element surgical changes. Only do a full file rebuild when several layers of a file genuinely change at once.
- **For critical colors (e.g. footer background) hardcode the hex value rather than relying on a CSS variable.** CSS variable definitions are not consistent across files — see "Color Variable Inconsistencies" below.

## Brand

- **Firm name (full):** Strategy West College Planning
- **Footer brand mark:** `Strategy West` (display font) with eyebrow tagline `College Planning` underneath.
- **Brand descriptor (footer / meta):** "Expert, advisor-led guidance for families navigating the college planning process — from freshman year through enrollment."
- **Home hero eyebrow:** "Trusted College Planning Guidance"
- **Home hero headline:** "The College Process Deserves More Than *a Guess.*"
- **Nav logo:** `images/logo.png` (the visible site logo is an image, not text).

There is no single short tagline written on the site. The closest standing phrase is the footer tagline `College Planning` sitting beneath `Strategy West`. The home hero positions the firm with the headline above and subheadline: "Strategy West College Planning guides families through every dimension of the college planning process. Academic positioning, school selection, financial strategy, and everything in between."

## Voice & Tone

- Calm, authoritative, advisor-led, premium. Parent-facing copy, readable on mobile.
- Emphasizes guidance, structure, timing, and informed decision-making.
- Representative on-site lines that capture the voice:
  - "We will not tell you what you want to hear. We will tell you what you need to know, early enough to act on it."
  - "Calm, honest, and proactive guide through one of the most complex processes your family will face."
  - "Reduce the uncertainty and stress of the college planning process and replace it with a clear, step-by-step strategy."

**Never use:** "hack," "loophole," "secret," or any gimmicky language. Never promise guaranteed scholarships, admissions results, or financial-aid outcomes. Avoid fear-based or sensational copy.

## Typography

- **Display:** `Cormorant Garamond` (weights 400–700, italic available), fallback `Georgia, serif`. Used for headings, brand mark, italicized accent words inside headlines, mission row labels.
- **Body:** `DM Sans` (weights 300–600, italic available), fallback `system-ui, -apple-system, sans-serif`. Used for all body copy, nav, buttons, eyebrows, footer.
- Both loaded from Google Fonts via the same `<link>` tag in the `<head>` of every page.

## Brand Colors (Hex)

These are the hex values used in `:root` CSS custom properties across pages:

| Token            | Hex       | Notes |
|------------------|-----------|-------|
| `--navy`         | `#064E3B` | Primary brand — a deep forest green (not a literal navy blue). |
| `--gold`         | `#C4963A` | Primary accent. |
| `--gold-light`   | `#D4A84B` | Hover state of gold. |
| `--gold-pale`    | `#F0E2C4` | Italic accent text on dark backgrounds; nav tagline. |
| `--ivory`        | `#F7F4EE` | Section backgrounds. |
| `--ivory-warm`   | `#EDE9DF` | Closing CTA / card image areas. |
| `--white`        | `#FFFFFF` | |
| `--text-dark`    | `#1A1A2E` | Headings on light backgrounds. |
| `--text-body`    | `#3A3A4A` | Body copy. |
| `--text-muted`   | `#6B6B7E` | |
| `--text-light`   | `#9A9AAA` | |
| `--border`       | `#E2DDD4` | |
| Footer background| `#0a3528` | **Hardcoded in every page**, not via a variable. |

### Color Variable Inconsistencies (important)

`--navy-mid` and `--navy-light` are **defined differently in different files**:

- `about.html`: `--navy-mid: #0a3528`, `--navy-light: #0f4535` (green family)
- `insights.html`, `article-*.html`, `blog-*.html`: `--navy-mid: #152540`, `--navy-light: #1E3357` (blue family)
- `packages.html`: `--navy-mid: #0A3D2E`, `--navy-light: #0D5C45` (green family)

This is why the footer background is hardcoded `#0a3528` rather than referencing `--navy-mid`. **When touching any color that needs to look identical across pages, hardcode the hex.**

## Contact, CTAs & Flow

- **Contact email:** `info@swfcollegeplanning.com`
- **Primary consultation / booking CTA:** `https://meetings-na2.hubspot.com/will-smethurst/cfs-website-meeting` (HubSpot Meetings — opens in a new tab via `target="_blank" rel="noopener"`). Used everywhere the site says "Schedule a Consultation."
- **Full Ensemble Strategy CTA (packages.html, card 3 only):** `https://meetings-na2.hubspot.com/will-smethurst/swfcp-website-full-ensemble-strategy` — a separate HubSpot link reserved for the top-tier package conversation.

### Payment / Intake Flow

- **Packages 1 and 2 are direct Stripe checkout** from the package cards on `packages.html`:
  - Financial Analysis — **$249**: `https://buy.stripe.com/7sYdR26zA1g16Ykf5nbo400`
  - The Essential 3-Pillar Mix — **$649**: `https://buy.stripe.com/28E8wI0bc4sdgyU0atbo401`
  - A Stripe modal popup helper (`openStripeModal`) is defined in `packages.html` but the live CTAs use direct `<a href>` Stripe links.
- **Package 3 (The Full Ensemble Strategy)** has no Stripe checkout. It instead routes to its dedicated HubSpot booking link (`https://meetings-na2.hubspot.com/will-smethurst/swfcp-website-full-ensemble-strategy`) for a guided conversation before any payment.
- **Post-purchase flow for Packages 1 and 2:** After a successful Stripe payment, Stripe redirects the buyer to `swfcpintakeform.html`. The family completes the intake (React + Uppy uploads + Google Apps Script backend, with resume-by-email), then schedules their consultation with Will via the standard HubSpot booking link (`https://meetings-na2.hubspot.com/will-smethurst/cfs-website-meeting`).
- **`swfcpintakeform.html` is the post-purchase onboarding step.** It is NOT linked from the nav, footer, or any marketing page, so it will not be discovered by crawling the site's HTML — but it is reached automatically via Stripe's post-payment redirect after Package 1 or Package 2 is purchased. Treat it as part of the post-purchase flow, not a disconnected page.

## Publishing Mechanics

### Unified Insights Index (`insights.html`)

The site has **one** content index page: `insights.html`. The old `articles.html` and `blogs.html` were retired and **deleted**; a `_redirects` file at the repo root issues 301 redirects from `/articles.html` and `/blogs.html` to `/insights.html`. Do not recreate either old file.

`insights.html` uses the editorial-list look (`.article-row` and friends — class names retained from the original articles template) but the list is **driven by a JavaScript data array** at the bottom of the page, rendered into `<div class="articles-list" id="articles-list">` by `renderInsights()`. Each entry is one insight. The page provides a working **category filter** (chips) and a **See More** progressive-reveal control that share state — clicking a chip filters the array and resets the visible count to 6; clicking See More reveals 3 more rows within the active filter.

Exact field shape of an `insights` entry:

```js
{
  number:   '01',                    // two-digit string; sequential by display order (newest first)
  category: 'Financial Strategy',    // must match a filter chip exactly
  title:    'Insight headline',
  excerpt:  'Short description shown on the row.',
  meta:     'Month DD, YYYY',        // publish date
  link:     'article-slug.html'      // or 'blog-slug.html' or 'insight-slug.html' — see filename rule below
}
```

The unified filter chips are: `All Topics`, `Financial Strategy`, `Admissions`, `School Selection`, `Planning`. These collapsed the legacy article categories (Financial Strategy, School Selection, Admissions, Financial Aid, Planning) and the legacy blog categories (Financial Strategy, Academic Planning, School Selection, Financial Aid, Timelines & Process) into 4 working categories: the legacy `Financial Aid` tag was absorbed into `Financial Strategy`, and `Academic Planning` + `Timelines & Process` both fold into `Planning`.

Insights are displayed in **newest-first order** with their `number` field incrementing sequentially down the visible list (`01`, `02`, `03`, …).

### Detail-Page Format — The "Insight" Template

All 20 existing detail pages (13 `article-*.html` + 7 `blog-*.html`) were migrated in place to a single uniform **no-hero Insight template**. The old article-vs-blog template differences are gone:

- **No navy hero, no gold radial glow.** `.post-header` sits on the normal page background with `padding: 64px 0 0` (desktop) / `padding: 40px 0 0` (mobile, `<600px`). No background color, no `::before` glow.
- **No back link.** The `.post-header__back` markup is removed (the surrounding dead CSS rules are left in place — harmless).
- **Plain title block** at the top of `.post-header__inner`: a single `.post-header__category` (gold-on-light eyebrow), then `<h1 class="post-header__title">` (Cormorant Garamond, `color: var(--navy)`), then `.post-header__meta` with date · 3px dot · read-time (`color: var(--text-muted)`, dot `var(--text-light)`).
- **No `.post-header__pill`** and **no `.post-type-badge`** anywhere. The article/guide pill system has been retired; the category eyebrow is the sole taxonomy badge.
- **Body blocks** stay as they were: `.post-intro`, `.post-section` (with `.post-section__heading` / `__subheading`), `.post-callout`, `.post-list`, and `.post-definition` (the key-term boxes the article template used — preserved where present on the relevant detail pages).
- **Closing CTA** (`.post-cta`) is unchanged. Do not remove — this is the page's conversion point. Standard buttons: "Schedule a Consultation" (HubSpot) + "Watch the Free Workshop" (→ `webinar-library.html`).
- **Related-posts** (`.related-posts`) is unchanged structurally; the "All" link reads **"All Insights"** and points to **`insights.html`** on every detail page.

### Publishing a New Insight

1. **Create the detail page** in the no-hero Insight template — clone any existing detail page, then replace its content. The recommended filename for new pieces is **`insight-<slug>.html`**. The 20 existing pages keep their original `article-*.html` / `blog-*.html` filenames **purely to preserve their SEO**; those prefixes are legacy markers, not type indicators, and should not be carried into new files.
2. **Add one entry to the `insights` data array** at the bottom of `insights.html`. Place it at the top of the array if it's the newest, then renumber subsequent entries (or simply bump downward). The filter and pagination handle the rest automatically.

That's the whole flow. There is no separate articles-list HTML to update, no `data-article-hidden` attribute to manage, no per-page count labels to keep in sync. Retire any older instructions referencing those mechanics.

### Related-Card Cross-Linking

Each detail page ends with a `<section class="related-posts">` containing up to 3 `<a class="related-card">` items pointing to other detail pages, plus an `.related-posts__all` link reading **"All Insights"** that points to `insights.html`.

**Known follow-up — not yet done.** The related-card cross-links currently still respect the **original article-vs-blog groupings**: `article-*.html` pages link only to other `article-*.html` pages, `blog-*.html` pages link only to other `blog-*.html` pages, and the heading still reads `More in {Category}` scoped to that single legacy bucket. Broadening these cards to draw freely across the full Insights pool — so any insight can link to any other regardless of legacy prefix — is a future enhancement. When making other changes to a detail page, leave its related-card scoping as-is until that broader pass happens.

When a new insight is published in an existing category, revisit the related-posts blocks of other insights in the same category and swap any placeholder card for the new live link.

### Nav + Footer Item

A single **Insights** item is wired in three places on every surviving page:

- **Desktop dropdown** (`.nav__dropdown-panel`): `<a href="insights.html" class="nav__dropdown-link" role="menuitem">Insights</a>` — sits above the first divider, ahead of `Timeline`, `Packages`, divider, `Free Workshop`.
- **Mobile accordion** (`.nav__accordion-links`): `<a href="insights.html" class="nav__accordion-link …">Insights</a>` — top of the accordion list.
- **Footer Resources column**: `<a href="insights.html" class="footer__link">Insights</a>` — top of the column, above `Timeline`, `Free Workshop`, `Packages`.

On `insights.html` only, the dropdown item carries the additional `nav__dropdown-link--active` class. No other page marks Insights active.

### Webinar Labeling Convention

The workshop entry reads **"Free Workshop"** consistently in all four places — keep this uniform in future edits:

- **Nav dropdown** (`.nav__dropdown-panel`) Resources item: **"Free Workshop"** (every page).
- **Mobile accordion** (`.nav__accordion-links`) Resources item: **"Free Workshop"** (every page).
- **Footer Resources column** link: **"Free Workshop"** (every page).
- **Nav CTA button** (`.nav__cta-webinar` and the equivalent mobile-drawer button): **"Free Workshop"** (every page).

All four point to `webinar-library.html`.

## Assets

- All images live in `images/` at the repo root.
- Complex illustrated background designs (page heroes, textured section backgrounds) use image files in `images/`, not pure CSS/SVG. Examples currently in use:
  - Page heroes: `about-hero-bg.jpg`, `articles-hero-bg.jpg`, `blogs-hero-bg.jpg`, `college-hero-bg.jpg`, `How-it-works-hero-bg.jpg`, `our-process-hero-bg.jpg`, `packages-hero-bg.jpg`, `pillars-hero-bg.jpg`, `timeline-hero-bg.jpg`, `webinars-hero-bg.jpg`, `graduation-webinar-bg.jpg`.
  - Textured section backgrounds: `three-pillars-texture-bg.png`, `integration-strategy-bg.png`.
  - Brand: `logo.png` is Strategy West College Planning's own brand mark and is used in every page's nav. **Do not substitute any other file for it.**
  - Partner-organization logos (displayed on the homepage only — these are NOT the firm's own logo): `cfs-logo.png` is College Funding Solutions, `acf-logo.png` is American College Foundation. Never use these in place of `logo.png`.
  - Blog-card category icons (SVG, transparent): `financial-strategy-symbol-transparent.svg`, `academic-planning-transparent.svg`, `school-selection-transparent.svg`, `financial-aid-transparent.svg`, `timeline-and-process-transparent.svg`.

## Page Inventory (repo root)

Marketing: `index.html`, `about.html`, `how-it-works.html`, `our-process.html`, `pillars.html`, `packages.html`, `timeline.html`, `webinar-library.html`.
Insights index: `insights.html`.
Insight detail pages: `article-*.html` (13 files) + `blog-*.html` (7 files), all using the unified no-hero Insight template. New insight detail pages should use `insight-*.html`; the legacy `article-*` / `blog-*` prefixes on the existing 20 are retained only for URL/SEO preservation and carry no type meaning anymore.
Legal: `privacy.html`, `terms.html`.
Out-of-band onboarding: `swfcpintakeform.html` (not linked from public nav).
Redirects: `_redirects` at the repo root issues 301 forwards from `/articles.html` and `/blogs.html` to `/insights.html`. Do not recreate the old index files.
