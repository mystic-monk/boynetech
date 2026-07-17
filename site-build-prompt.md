# Prompt: Build the Boyne Tech website (and do it better)

Use this prompt with any AI coding assistant to reproduce this site from scratch, or to brief a new build for a similar early-stage consultancy. It captures the design system, structure, and — most importantly — the content-honesty rules that this build got wrong the first time and had to fix afterward. Follow the honesty rules from the start; don't repeat the cleanup.

---

## 1. Brief

Build a marketing website for **[Company Name]**, an Irish technology and analytics consultancy. Practice areas: IT Managed Services, Data & Analytics, Software Development, AI & Automation, Digital Transformation. The company is early-stage — assume no published client case studies, no confirmed industry certifications, and no built product accelerators unless the site owner explicitly confirms otherwise. The site must never claim more than what's true today.

Stack: **plain static HTML/CSS/JS. No framework, no build step, no bundler.** Must deploy as-is to any static host (Render, Netlify, GitHub Pages, S3). One `css/styles.css` shared by every page; each page has an inline `<style>` block only for page-specific rules.

## 2. Design system

**Colors** (CSS custom properties):
```css
--navy:    #0E2438   /* primary text / dark backgrounds */
--blue:    #1C537E   /* links, secondary accent */
--emerald: #177A4E   /* primary accent, CTAs */
--mint:    #7CFFB8   /* emerald substitute on dark backgrounds — emerald fails contrast on navy */
--slate:   #4E646A   /* body copy on light backgrounds */
--grey:    #8C9994   /* muted text, borders */
--cloud:   #EEF3EE   /* alternate section background */
--white:   #FFFFFF
```

**Type**: Display font "Syne" (weights 700/800) for headings, "DM Sans" (400/500) for body, both via Google Fonts. h1 `clamp(2.25rem, 5vw, 3.5rem)`, scale down through h6.

**Spacing scale**: xs 0.5rem · sm 1rem · md 1.5rem · lg 2.5rem · xl 4rem · 2xl 6rem. Radius: sm 4px · md 8px · lg 16px. Shadows: three tiers, all `rgba(navy, opacity)`.

**Layout rules**:
- Sections alternate background between white and `--cloud` for visual rhythm as you scroll — track this explicitly when adding/removing sections so two of the same color never end up adjacent.
- `.grid--2/3/4` utility classes, responsive: 4→2→1 columns, 3→1, 2→1 at 1024px/768px/480px breakpoints.
- Sticky nav, mobile hamburger menu, active-page link state in both desktop and mobile nav (keep them in sync — same links, same order, always).
- Standard 4-column footer (brand blurb + registration number, Services links, Company links, Get in Touch) plus a legal bar (copyright, Privacy, Cookies, Terms).

## 3. Sitemap

- `index.html` — hero, 5(+1 custom) service cards, "why us" trust columns, stats strip, insights teaser, CTA
- `services.html` — one detailed block per practice area (alternating image-left/right), "how we work" 4-step process
- `solutions.html` — the kinds of problems the team solves (honest capability scenarios, not claimed past projects) + custom-build pitch + technology stack (grouped by category, not one long undifferentiated tag list) + CTA
- `about.html` — story, values, CTA
- `careers.html` — culture pitch, speculative-application strip (if there are no open roles, say so plainly)
- `insights.html` — articles list + newsletter signup
- `contact.html` — form + contact details + FAQ accordion
- `privacy.html`, `terms.html`, `cookie-policy.html` — real, specific legal copy matching what the site actually does (see §5)
- `404.html`
- `sitemap.xml`, `site.webmanifest`, favicon set
- `_redirects` file (Netlify/Render syntax) for any URL you ever retire — 301, don't just orphan it

Do **not** create a separate "Our Work" / case-studies page that exists only to say "we have no case studies yet." That's two thin, low-credibility pages instead of one substantial one — fold honest capability/example content into the Solutions page instead.

## 4. Functional requirements

- **Every form must actually work on day one.** Wire to a real form-backend service (e.g., Formspree) via `fetch` + `Accept: application/json`, with a honeypot field, a disabled/loading submit state, and a real error path with a mailto fallback — not a fake success message that discards the input. `action="#"` on a form is never acceptable to ship.
- **GDPR/ePrivacy, done correctly, not decoratively**:
  - Contact/enquiry forms: legitimate-interest basis is enough — a one-line "by submitting you agree to our Privacy Policy" note is sufficient, no checkbox required.
  - Newsletter/marketing signup: legally requires **explicit, unticked opt-in consent** — add a required checkbox referencing the Privacy Policy, don't just collect an email.
  - Whatever the Privacy Policy claims about legal basis and data collected must match what the forms on the site actually do. Write the policy after the forms exist, or update both together.
- If you add an AI chat widget, never embed a real LLM API key in client-side JS — it's fully readable via view-source and will get exfiltrated/billed against. Proxy it through a backend endpoint, even a minimal serverless one.
- Fully responsive, keyboard-navigable, ARIA-labelled nav/accordion/forms, `loading="lazy"` on below-the-fold images, meaningful alt text.
- SEO: per-page meta description, OG/Twitter tags, canonical URL, `Organization` JSON-LD, and a maintained `sitemap.xml` — remove entries the moment a page is retired.

## 5. Content-honesty rules (the part that actually matters)

This is the section that got skipped the first time and had to be fixed page by page afterward. Apply it from the first draft:

1. **Never invent accreditations, certifications, partnerships, or industry-body memberships.** Only list ones the site owner has explicitly confirmed are real and current.
2. **Never state a specific, precise-sounding metric (a percentage, a project count, a "years experience" number) without confirming its source.** If it's the founders'/team's *individual* career history predating the company, label it that way explicitly (e.g., "years combined leadership experience across the team") — don't let it read as the company's own delivery track record if the company doesn't have one yet.
3. **Never mark a product/tool "Available" unless it is genuinely built and demoable today.** Ideas that exist only as a concept get labeled "In Development" at most, or left off the site entirely — don't invent a fake capability just to fill a grid to three cards.
4. **Never show blog/insights content with a fake publish date or byline for an article that hasn't been written.** Either the article exists and is dated correctly, or it's clearly marked "coming soon" with no fabricated metadata pretending otherwise.
5. **Don't leave bracketed placeholder content live** — `[Head of Delivery]`, stock team bios, placeholder phone numbers — without either filling it in with real information or removing the section. Placeholder-looking content reads worse than no content.
6. **Match tone to actual company stage.** For an early-stage company, "we're deliberately getting our first engagements underway" reads as more credible than implying an established portfolio — don't oversell out of habit.
7. **Write like a person, not a template.** Avoid em-dash-heavy sentence construction, "it's not just X, it's Y" constructions, and rule-of-three padding. Vary sentence length and rhythm. Short declarative sentences read more human than compound ones stitched together with dashes.
8. **Before adding any specific claim — a stat, a cert, a case study detail, a "we've delivered X" line — ask the site owner to confirm it's real.** Don't fill a content gap with something plausible-sounding. A visibly honest "we haven't done this yet" section beats a fabricated one every time; it was the single most consistent fix needed across this entire site.

## 6. Process notes for whoever builds this

- Build and fully wire one page (real nav, real footer, real working form if it has one) before replicating the pattern to the rest of the site, so link/nav drift doesn't compound across a dozen files.
- Keep the nav link list and footer "Company" link list byte-identical (same items, same order) across every single page — this is the easiest thing to let drift and the easiest to grep-check.
- When you retire or merge a page, don't just delete links to it: add a real redirect (`_redirects` entry + meta-refresh fallback on the old file), update `sitemap.xml`, and re-check every page for stale links via a repo-wide search before calling it done.
