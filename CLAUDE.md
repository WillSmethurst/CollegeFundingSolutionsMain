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
- `articles.html`, `blogs.html`, `article-*.html`, `blog-*.html`: `--navy-mid: #152540`, `--navy-light: #1E3357` (blue family)
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

### Articles (`articles.html`)

Articles are activated by editing the article-row block inside `<div class="articles-list" id="articles-list">`.

The page currently contains **13 article rows**. The first 3 render visible by default; rows 4 through 13 carry the `data-article-hidden` attribute and are revealed three at a time by the "See More Articles" button. **Rule: the first three `.article-row` elements must NOT have `data-article-hidden`. The fourth and beyond MUST have `data-article-hidden`.**

Note: the visible count label and the JS-collected total use `document.querySelectorAll('.article-row').length` for the total, but the static fallback text on the page reads `Showing 3 of 13 articles` in two places (`#articles-count-label` and `#visible-count` / hardcoded `13`). If the total count changes, update both static fallbacks as well as any article numbering.

Exact structure of an article row:

```html
<article class="article-row" role="listitem" aria-label="Article N" data-article-hidden>
  <div class="article-row__aside">
    <span class="article-row__number">NN</span>
    <span class="article-row__category">Category Name</span>
  </div>
  <div class="article-row__content">
    <h2 class="article-row__title">Article Title</h2>
    <p class="article-row__excerpt">Excerpt copy…</p>
    <div class="article-row__footer">
      <a href="article-slug.html" class="article-row__read-more">Read More</a>
      <span class="article-row__pill">Article</span>   <!-- or "Guide" -->
    </div>
  </div>
</article>
```

Remove the `data-article-hidden` attribute to make an article render in the initial visible set. The `__number` is a two-digit string (`01`, `02`, …). The `__pill` text is either `Article` or `Guide` and should match the pill shown inside the post header on the target page.

Article category values seen in use: `Financial Strategy`, `School Selection`, `Admissions`, `Financial Aid`, `Planning`. The filter tags rendered above the list currently include `All Topics`, `Financial Aid`, `Admissions`, `Planning` (note: the filter UI is presentational only — filtering is not yet wired up).

### Article Page Template (e.g. `article-understanding-the-fafsa.html`)

Each article page is self-contained with its own `<style>` block. Key building blocks inside `<article class="post-body">`:

- `.post-type-badge` — small pill at top of reading column ("Article" or "Guide"), matches the header pill.
- `.post-intro` — opening paragraph, larger type, ends with a divider rule.
- `.post-section` with `.post-section__heading` (H2) and optional `.post-section__subheading` (H3).
- `.post-callout` — gold-bordered key-takeaway box.
- `.post-list` — bulleted list with gold dots.
- `.post-definition` — boxed key-term definition.
- `.post-cta` — closing CTA box (do not remove — it's the conversion point).
- The page header (`<section class="post-header">`) carries the category eyebrow, pill (Article/Guide), title, publish date, and read-time estimate.
- SEO fields at the top: `<title>`, `<meta name="description">`, canonical URL, and OG tags. Update all four when changing the title or slug.

### Blog Posts (`blogs.html`)

Blog posts are activated by editing the `blogPosts` JavaScript data array inside the `<script>` block in `blogs.html`. Cards are rendered into `#blog-grid` by the `renderBlogs()` function. Six cards per page, with prev/next arrow pagination and an in-page filter tag row.

Exact field shape of a `blogPosts` entry:

```js
{
  category: 'Financial Strategy',     // must match a filter tag label exactly
  title:    'Card headline',
  excerpt:  'Short description shown on the card.',
  meta:     'Month DD, YYYY',          // or 'Coming Soon' for placeholders
  link:     'blog-slug.html',          // or null for placeholders
  status:   'live',                    // 'live' or 'placeholder'
  icon:     '<svg-inner-paths-or-fragment>'  // legacy field; not used by the current renderer
}
```

Categories currently in use (must match filter tags exactly): `Financial Strategy`, `Academic Planning`, `School Selection`, `Financial Aid`, `Timelines & Process`.

The renderer maps each category to an SVG icon file in `images/` via the `categoryIcons` object (`financial-strategy-symbol-transparent.svg`, `academic-planning-transparent.svg`, `school-selection-transparent.svg`, `financial-aid-transparent.svg`, `timeline-and-process-transparent.svg`). The legacy inline `icon` SVG string on each entry is preserved but not rendered.

Placeholders (`status: 'placeholder'`) get a "Coming Soon" ribbon and a non-clickable "Read More" — the `link` should be `null`.

### Blog Post Template (e.g. `blog-merit-aid-vs-need-based-aid.html`)

Same shell as article pages (own `<style>`, own nav/footer, own SEO block) but simpler body — typically just `.post-intro`, `.post-section`s, `.post-callout`s, `.post-list`s, and the closing `.post-cta`. No `.post-type-badge` and no `.post-definition` block by default.

### Related-Card Cross-Linking

Both article and blog pages end with a `<section class="related-posts">` block:

- Heading reads `More in {Category}` and is scoped to the current page's category.
- Up to 3 `<a class="related-card">` items, each containing `.related-card__category`, `.related-card__title`, `.related-card__excerpt`, `.related-card__footer` with `.related-card__date` and `.related-card__link`.
- "All Articles" link in the header points to `articles.html` from article pages and to `blogs.html` from blog pages.
- **Articles only link to other articles, blogs only link to other blogs.** Inline body-copy links inside an article may reference other articles by relative URL (e.g. the FAFSA article links inline to `article-appealing-financial-aid-award.html`).
- Never link a page to itself. When fewer than 3 related items exist in a category, delete the extra card blocks rather than leaving empty ones (a placeholder card pointing back to `blogs.html` is used as a temporary fallback in some blog pages — see `blog-merit-aid-vs-need-based-aid.html`).
- When a new article or blog in a given category is published, revisit the related-posts blocks of existing posts in the same category and swap the oldest placeholder for the new live link.

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
Resources index: `articles.html`, `blogs.html`.
Long-form: `article-*.html` (13 files), `blog-*.html` (7 files).
Legal: `privacy.html`, `terms.html`.
Out-of-band onboarding: `swfcpintakeform.html` (not linked from public nav).
