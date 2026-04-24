# The Road Home — phase-2 plan · ship to stakeholder

End-to-end plan covering the remaining work to deliver the final site. Designed to be executed autonomously in one run; all decisions are locked up front so execution does not pause for questions.

**Status at the start of this run (as of 2026-04-24)**
- Brand · personality · tokens system · 20 priority-1 pages · all complete and responsive
- Home page (variant B) critiqued + fixed (typeset, emergency ribbon, services asymmetry, dead-link harden, polish)
- Remaining 19 priority-1 pages have inherited the shared token changes but have **not** been individually critiqued
- Priority-2 (~40 pages) and priority-3 (~24 pages) are not yet scraped or rendered

**Deliverable at the end of this run**
- `/index.html` (project root) still the stakeholder entry point
- `stardust/pages/` populated with every non-holiday real page from theroadhome.org (~64 more HTML files on top of the 20 already there)
- `stardust/pages/sitemap.html` updated to list every page
- `stardust/VALIDATION_REPORT.md` summarizing link, image, and consistency checks + the fixes applied
- `stardust/EXECUTION_LOG.md` appended with honest notes on what was completed, what was stubbed, what needs human review

---

## Decisions locked · no re-asking

1. **Scope.** Every non-holiday, non-test page from theroadhome.org's `page-sitemap.xml` — ~64 pages in addition to the 20 already done. Holiday1–6, sample-page, test pages, and asset URLs stay excluded (per PLAN.md §4).
2. **Imagery.** Reuse TRH photography from the scrape; Unsplash placeholder for slots with no TRH asset. Production must swap in real photography before ship (flagged in the validation report).
3. **Copy.** Verbatim from the live site by default. Targeted rewrite only when (a) the live copy is Lorem ipsum or an obviously-broken placeholder, (b) the live copy repeats itself in-page, or (c) the live page's architecture is incoherent (e.g. `/palmer-court/` = volunteer page — restore to program detail). Every rewrite gets `copy_edited: true` + reason in the briefing frontmatter.
4. **New templates.** Four are required to cover the remaining clusters: **T-08 Person detail**, **T-10 Get-involved program detail**, **T-13 History / timeline**, **T-15 Legal / policy**. Each gets built once as the page that best represents the cluster, then reused for siblings.
5. **Stub policy.** Priority-3 pages that don't materially change the user journey (decades of old fundraisers, personal-page variants, one-off holidays we already excluded) get a **styled stub page**: same nav + footer + breadcrumbs, a single "This page is part of the site rebuild but has not yet been prioritized. Content is available on the live site at theroadhome.org/<slug>." card, and navigation back to the relevant pillar. Stubs count as "delivered but incomplete" in the final log.
6. **Critique pass.** Cover the remaining 19 priority-1 pages via a **sampled critique**: one representative per template type. Findings that apply globally are fixed once in `_tokens.css` or via a page-wide sweep. Per-page findings on representative pages are fixed individually; derivative pages inherit automatically.
7. **Validation.** Every HTML file under `stardust/pages/*.html` runs through (a) a link-health checker (verify every `href` points to either an existing file, an external URL, a `tel:`/`mailto:` scheme, or an in-page anchor that exists), (b) an image-load check (verify every `<img src>` and background-image URL returns an OK response), and (c) a consistency grep sweep (nav structure, footer structure, token usage, and palette discipline). Every issue found is logged; every fix is logged.
8. **Stop conditions.** If total elapsed exceeds **4 hours**, the run writes a partial validation report and stops; whatever is rendered stays rendered. No page that was partially started is left in a half-broken state — each page either ships complete or is replaced by a stub.

---

## Phase 1 · Finish the critique (remaining 19 priority-1 pages)

**Goal.** One consolidated list of cross-page design issues, plus page-specific issues on each template representative. Fix globally where possible; fix locally where needed.

### Step 1 · Representative sample (1 page per template)

For each of the 8 templates represented in priority-1 (other than T-01 Home, which is done), pick one representative page to critique:

| Template | Representative page |
|---|---|
| T-02 Pillar hub | `get-help.html` (the crisis-audience entry; the other three pillar hubs are derivatives) |
| T-03 Service detail | `shelter.html` |
| T-04 Donate flow | `donate.html` |
| T-05 Giving-method detail | `legacy-planned.html` |
| T-06 Transparency | `where-does-it-go.html` |
| T-07 People directory | `meet-the-team.html` |
| T-09 Stories listing | `housing-stories.html` |
| T-11 Form | `contact-us.html` |
| T-12 FAQ | `faq.html` |

### Step 2 · Run two-pass critique per representative (9 pages)

For each, run both passes from the `impeccable:critique` skill:
- **Assessment A** — sub-agent LLM design review against the DON'T guidelines, Nielsen's 10 heuristics, cognitive-load checklist, persona walkthroughs
- **Assessment B** — `npx impeccable --json <page>` detector

Collect all findings into `stardust/_scratch/critique-phase1.json`.

### Step 3 · Classify findings

Walk the findings and bucket each into:
- **Global** — fix once in `_tokens.css` or by sweeping all `pages/*.html`
- **Template-level** — fix in the representative; re-render derivatives from the fixed template
- **Page-specific** — fix inline

### Step 4 · Apply fixes

Apply fixes in this order: global first, then template-level, then page-specific. Re-check a subset by running the detector on 3 random pages after fixes.

**Expected outputs:**
- Updated `_tokens.css` (if global fixes apply)
- Updated representative pages
- Appended log in `EXECUTION_LOG.md` listing each fix and the page(s) it affected

---

## Phase 2 · Migrate remaining pages (~64)

**Goal.** Render every non-holiday, non-test page from the live site. Use existing templates where they fit; build new templates for the four gaps (T-08, T-10, T-13, T-15).

### Step 1 · Crawl the remaining pages (~64 URLs)

Extend `stardust/_scratch/crawl.js` to cover the priority-2 and priority-3 targets. Target list is derived from `page-sitemap.xml` minus the 20 already scraped and minus the exclusions.

**Priority-2 cluster (40 pages — full templates, real copy):**

| Cluster | Target template | Pages |
|---|---|---|
| Service / program detail | T-03 | `/mens-resource-center/`, `/midvale-family-resource-center/`, `/housing-navigation-program/`, `/all-housing-programs/`, `/permanent-housing/`, `/landlord-resources/`, `/community-referral-page/`, `/insurance/` |
| Donate flow | T-04 | `/one-time-donation/`, `/recurring-donation/`, `/donation-form/`, `/housing-champions-2/`, `/housing-champions-donation/`, `/donate-2/`, `/item-donations/`, `/in-honor-and-memory/`, `/double-donation/` |
| Giving-method detail | T-05 | `/give-main/real-property/`, `/give-main/trusts/`, `/give-main/life-insurance/`, `/give-main/donor-advised-fund/`, `/give-main/stock-wire-transfer/`, `/give-main/in-kind-donations/`, `/give-main/donor-bill-of-rights/`, `/give-main/manage-donations/`, `/other-ways-to-give/` |
| Transparency | T-06 | `/data-finances/`, `/data-dashboard/` |
| People directory | T-07 | `/case-management-team/` |
| Person detail | **T-08 (new)** | `/michelle-flynn/` |
| Stories listing | T-09 | `/moments-that-matter/`, `/team-stories/`, `/videos/`, `/blog/`, `/media-center/` |
| Get-involved program | **T-10 (new)** | `/fundraiser/`, `/mediathon/`, `/sponsor-page/`, `/eagle-scouts/`, `/events/`, `/internships/`, `/court-ordered-volunteering/`, `/get-involved/careers/`, `/careers-positions/` |
| Form | T-11 | `/mvpreferral/`, `/get-help/mvpfacility/`, `/stvinnyreferral/`, `/ssvf-referral/`, `/tanf-rapid-rehousing-program-referral-form/`, `/fundraiser-form/`, `/trh-feedback-form/`, `/secure-file-upload/`, `/tell-your-story/` |
| History timeline | **T-13 (new)** | `/history/`, `/centennialcelebration/`, `/decades/`, `/awards/` |
| Legal | **T-15 (new)** | `/privacy-policy/` |
| Singleton | — | `/item/` (likely a catalog page — render as T-09 stub or skip based on content) |

**Priority-3 cluster (~24 pages — stub policy):**
Any remaining URL in `page-sitemap.xml` that does not fall into the above table gets a styled stub per the stub policy in §5 of the locked decisions. Expected stubs: `/holiday*/` already excluded; any one-off fundraiser pages or old landing pages from previous campaigns.

### Step 2 · Build the four new templates

Build each once using the best-fit priority-2 page as the representative:

- **T-08 Person detail** — render as `michelle-flynn.html`. Structure: hero portrait + bio + signed CEO letter + role strip + "read more stories" cross-link.
- **T-10 Get-involved program detail** — render as `fundraiser.html`. Structure: hero + at-a-glance sidebar (age, time commitment, cost) + narrative + how to sign up + related opportunities + volunteer-contact band. Similar to T-03 but tuned for volunteer vs. crisis audience.
- **T-13 History / timeline** — render as `history.html`. Structure: hero + long editorial + decade anchors (1920s → 2020s) with period photography + milestones list + "where we are now" close. Uses Source Serif 4 for editorial specimen.
- **T-15 Legal / policy** — render as `privacy-policy.html`. Structure: hero (no image) + sticky TOC + long-form prose (sections, subsections) + last-updated badge + contact for questions.

Each template gets added to the stardust catalog with a one-line description and reuses `_tokens.css` exclusively (no new tokens unless genuinely missing from the system).

### Step 3 · Build briefings for the ~64 pages

Same format as the existing briefings. Frontmatter + Intent + Audience + Sections + verbatim #Copy + #Imagery. One briefing per real page. Stubs get a minimal stub briefing.

### Step 4 · Render every page

**Template reuse strategy.** For pages that share a template with a completed sibling, render mechanically: read the briefing, read the template's HTML, substitute the page-specific blocks (hero copy, eyebrow, main content, breadcrumbs), write to `stardust/pages/{slug}.html`.

For new-template pages (T-08, T-10, T-13, T-15), render the representative by hand, then batch the siblings with substitution.

**Stub renderer.** For priority-3 pages: read the briefing, render a generic stub-page template (nav + breadcrumbs + stub-card + footer) and write to disk. Same CSS system.

### Step 5 · Update the sitemap

Rewrite `stardust/pages/sitemap.html` to list every rendered page — full set, grouped by template — and to flag stubs separately from full pages.

**Expected outputs:**
- `stardust/briefings/*.md` grows from 19 to ~83 files
- `stardust/pages/*.html` grows from 21 (including sitemap + index) to ~85 files
- Four new template prototypes rendered as their representative pages
- Updated sitemap

---

## Phase 3 · Generate hero imagery (20 images, Gemini 3 Pro Image Preview)

**Goal.** Replace the Unsplash stock placeholders in the highest-visibility image slots with brand-aligned generated imagery. The generated set is **architectural, atmospheric, and contextual** — not photorealistic portraits of fabricated people.

### Ethical constraint (load-bearing)

This is a homeless-services nonprofit with a real 100-year reputation. Fabricating a photorealistic portrait of "a TRH client named Darnell" and shipping it as evidence of impact is a reputational landmine — if the image is later discovered to be synthetic, donor trust evaporates. The generated set therefore **excludes any photorealistic portraits of people who do not exist**.

What IS generated: building exteriors, interior spaces, city context, hands at work (framed so no face is recognizable), atmospheric details (light through windows, winter streetscapes, doorway thresholds, meal trays).

What is NOT generated: client portraits, staff portraits, board portraits, recognizable face-forward shots. Slots currently filled with Unsplash portraits (Darnell, Amanda, Alex & Taylor, Randall, Cory, the Nelsons, Michelle Flynn, Holly Rogers, etc.) keep their existing placeholder + `needs-real-photo: true` frontmatter flag.

### Step 1 · Identify the 20 slots

Candidate list, ranked by page-view-weight × visual-prominence:

**Building exteriors (5):**
1. Palmer Court — `stardust/pages/palmer-court.html` hero (also used on `about-us.html`, `give-main.html`, `housing-programs.html`)
2. Pamela Atkinson Men's Resource Center — `mens-resource-center.html` hero + `shelter.html` location card
3. Midvale Family Resource Center — `midvale-family-resource-center.html` hero + `shelter.html` location card
4. Gail Miller Resource Center — `resource-centers.html` location card
5. Connie Crosby Family Resource Center warehouse — `resource-centers.html` location card

**Interior / operational (5):**
6. Intake desk / reception at a resource center — used on `shelter.html` + `emergency-services.html` heroes
7. Shared dining / meal service at a resource center — `get-involved.html` hero
8. Classroom / kids' literacy-night space at Palmer Court — `palmer-court.html` volunteer band
9. Warehouse / donation sort room at Connie Crosby — `housing-champions.html` / `item-donations.html`
10. Housing navigator's working desk (papers, phone, computer) — `housing-programs.html` / `legacy-planned.html`

**City context (4):**
11. Downtown Salt Lake City at dusk with Wasatch backdrop — `index.html` hero (replaces current Unsplash portrait)
12. Pioneer Park / Rio Grande neighborhood in winter — `emergency-services.html` atmospheric slot
13. Utah wintery street scene, low light, warm window glow — `get-help.html` pillar-hero art slot
14. Doorway threshold into a warmly-lit building, viewed from outside — `shelter.html` "A bed, a meal" illustration

**Civic / institutional (3):**
15. Audited paperwork / a 2025 Annual Report opened on a desk — `where-does-it-go.html` hero / accountability
16. Community-board-room composition with chairs at a long table — `board-of-trustees.html` hero
17. Centennial mark detail (1923 date stamp / century-old masonry / cornerstone) — `history.html` + `about-us.html` mission slot

**Atmospheric / editorial (3):**
18. Keys in hand with a key-ring fob marked "home" — `housing-programs.html` + `housing-stories.html`
19. Warm coat on a hook by a door, soft morning light — `get-involved.html` moments-that-matter strip
20. A meal tray with a hot bowl on an institutional-but-clean table — `donate.html` "$85 buys one night of shelter" moment

Each image slot has a short **creative brief** derived from:
- `brand-profile.json` → `photography.rules` and `photography.style` (muted cool tones, natural light, the teal stays the loudest color, no sepia / no pity / no golden-hour uplift)
- `.impeccable.md` → community/human-first direction, no startup tropes, no stock-nonprofit clichés
- The specific page's briefing → subject matter + tone

### Step 2 · Invoke the image-generator skill

Use the bundled `ai-image-generator` skill (available via `eds-site-builder`, `sumi`, or `testing` plugin — pick whichever is registered and has Gemini 3 Pro Image Preview support).

**Configuration:**
- `imagery_provider: gemini`
- `imagery_model: gemini-3-pro-image-preview`
- `imagery_credential_source: /Users/paolo/excat/vitamix-gensite/.env`
- `output_dir: stardust/assets/generated/`
- `output_naming: {slot-id}-{short-subject}.png`
- `aspect_ratios: per-slot` — hero 16:9, portrait/building 4:5, detail 1:1, strip 3:2
- `cache_by: prompt_hash` — if the run is re-invoked, don't regenerate identical prompts

Never echo the credential value to stdout, logs, or provenance.

**Per-image prompt template:**

```
Photograph of [subject].
Setting: [setting — e.g. Salt Lake City winter, Palmer Court exterior, resource-center interior].
Mood: [muted, calm, dignified, service-first; natural light; cool tones; the teal brand color stays the loudest saturated color in the frame].
Composition: [specific — e.g. "wide shot with negative space on the left for display type overlay", "tight detail with selective focus"].
Avoid: sepia, golden-hour uplift, hands holding hands, outstretched palms, prayer-circle compositions, stock-photo glossiness, sentimental framing, nonprofit clichés.
Style: documentary editorial, restrained. Think Partners In Health's newer photography — dignified, specific, not triumphant.
```

For each of the 20 slots, the prompt is expanded with slot-specific subject, setting, and composition.

### Step 3 · Substitute the generated images into pages

For each slot:
1. Write the generated image to `stardust/assets/generated/{slug}-{section}.png`
2. Update the page's CSS / HTML to point at the new local path instead of the Unsplash URL
3. Update the page's provenance block: `imagery_slot: {section} — generated via Gemini 3 Pro Image Preview on 2026-04-24, cached at {path}, prompt hash {hash}`
4. Keep the alt text from the existing page (already authored for each slot)

### Step 4 · Fallback behavior

Per-slot, if generation fails:
- **First failure** → retry once with a slightly simplified prompt (drop the longest "Avoid:" clause)
- **Second failure** → keep the existing Unsplash placeholder for that slot; stamp `imagery_fallback: unsplash` in the provenance block; log it in `stardust/_scratch/imagery-fallbacks.json`
- Never abort the whole phase over one image

### Step 5 · Log provenance

Write `stardust/assets/generated/_manifest.json` with an entry per image:

```json
{
  "slot_id": "11",
  "slug": "index",
  "section": "hero-photo",
  "path": "stardust/assets/generated/index-hero.png",
  "prompt_hash": "a1b2c3",
  "provider": "gemini",
  "model": "gemini-3-pro-image-preview",
  "generated_at": "2026-04-24T20:30:00Z",
  "replaces_url": "https://images.unsplash.com/photo-1514894780887-121968d00567..."
}
```

**Expected outputs:**
- ~20 PNG files in `stardust/assets/generated/`
- `_manifest.json` as the audit record
- Updated HTML on the 10–14 pages that carry the top-20 image slots
- Entry in `EXECUTION_LOG.md`: how many generated, how many fell back, total token cost (reported by the skill)

---

## Phase 4 · Site validation

**Goal.** A concrete, auditable proof that the site is ready to hand to a stakeholder. Every link works or is flagged; every image loads or has a fallback; every page uses the shared system consistently.

### Step 1 · Link health check

Write `stardust/_scratch/check-links.js`. Walk every `stardust/pages/*.html`. For each `<a href>`:
- `https?://*` external — `HEAD` request; 200–399 = OK; 4xx/5xx/timeout = flagged
- `tel:` and `mailto:` — format-validate only; no network
- `#anchor` — verify the anchor exists in the same page (`id` attribute present)
- `*.html` (relative) — verify the file exists under `stardust/pages/`
- Other — flag as suspicious

Output: `stardust/_scratch/links-report.json` — per-page list of broken / suspicious links.

### Step 2 · Image health check

Same pattern for every `<img src>` and every inline `background-image:url(...)` on every page. Local files (including Phase-3 generated images at `stardust/assets/generated/`) resolved against `stardust/assets/`. External URLs (Unsplash CDN, still present on slots not in the top-20 generated set) pinged with `HEAD`. Any 4xx/5xx gets flagged.

Also verify: every page that was supposed to carry a Phase-3 generated image is in fact pointing at the local `stardust/assets/generated/` path (not still the old Unsplash URL). The `_manifest.json` from Phase 3 is the source of truth for this check.

Output: `stardust/_scratch/images-report.json`.

### Step 3 · Design consistency sweep

Deterministic grep-style checks:
- Every page imports `_tokens.css` OR (for home-b/home-a and index.html) has inline tokens that match the shared system
- Every page has the same top utility bar structure and nav structure (sanity regex)
- Every page's footer uses `<footer class="site">` (or the identical inline equivalent on home) and has the same 5-column layout
- No page uses a fresh `#cd2653` pink or `#f5efe0` cream (pulled into brand-profile as "WP theme defaults, not real brand")
- No page uses a font other than Archivo / Public Sans / Source Serif 4 — grep for `font-family:` and flag deviations
- No page has `border-left: Xpx solid` with X > 1 (the absolute-ban check from `.impeccable.md`)
- No page has `background-clip: text` (gradient text ban)

Output: `stardust/_scratch/consistency-report.json`.

### Step 4 · Critique sweep on random sample

Pick 5 random pages from the full set and run `npx impeccable --json` on each. Any NEW antipattern that wasn't in the Phase-1 findings gets escalated. At least 2 of the 5 must be pages that received a Phase-3 generated image, so the new imagery gets a quality check in context.

### Step 5 · Apply fixes

For each issue found:
- **Broken internal link** → either point to the correct file or convert to a styled "not yet available" link
- **Broken image** → swap to a Unsplash fallback matched to the image's intent (portrait, building, data, etc.) + log it
- **Inconsistency** → fix in `_tokens.css` or sweep all pages
- **New critique finding** → evaluate: if global, fix in tokens; if single-page, fix inline

Re-run steps 1–3 after fixes until the reports are clean OR the run is at its time budget.

### Step 6 · Write the validation report

`stardust/VALIDATION_REPORT.md` — a clean, stakeholder-readable summary:
- Total pages shipped (full vs. stubbed)
- Link health: N/N OK, M/N flagged (with the flagged list)
- Image health: Unsplash placeholder count vs. Phase-3 generated count vs. real-TRH-asset count
- Design consistency: any deviations + the reason they weren't fixed (if any)
- **Imagery manifest** — the 20 Phase-3 generated images with their prompt hashes, cache paths, and the pages they appear on (cross-reference `_manifest.json`)
- Residual known issues (e.g., "Darnell's pull-quote is fictional — must be replaced with a real client story before ship")
- What to do before handoff to a production platform (Phase D from the original PLAN)

---

## Phase 5 · Project journal (`stardust/JOURNAL.md`)

**Goal.** A single, stakeholder-readable narrative of the entire project from the first stardust brand extraction to the final validation report — detailed enough that a second team doing a similar rebuild with the same pipeline can learn from what worked, what didn't, and what to do differently. This is the last artifact written in the run.

### Structure

The journal is written as a chronological story, not a changelog. Each chapter names the session, the decisions made, and the artifacts produced. Each chapter ends with 2–5 **learnings** — what to keep, what to drop, what to do differently next time.

**Required chapters, in order:**

1. **Starting point — "I want to rebuild this old site"**
   - The conversation that produced the goal
   - Why stardust was picked over a one-shot design tool
   - What the site looked like on day one (the live theroadhome.org — WordPress, WPBakery, 2020-era aesthetic, three failure modes: Lorem ipsum on key pages, four stacked DONATE buttons, phone-tree navigation)
   - Learnings on how to frame a redesign brief

2. **Brand extraction with stardust**
   - What the Playwright scrape actually captured vs missed
   - Which root tokens turned out to be WP theme defaults (pink, cream) vs real brand (teal family)
   - Voice examples pulled from live copy — why that beat synthesizing them
   - Logo download path, motif identification
   - Learnings: always drive a real browser, always look at computed styles not just inline tokens, always interrogate root-variable palettes against rendered CSS

3. **Design personality (`.impeccable.md`)**
   - The three inline questions (references, dislikes, one rule to bend)
   - Why "skip" produced a stronger `.impeccable.md` than "synthesize" would have
   - How Partners In Health-style references + "no startup tropes / no stock nonprofit clichés" shaped every downstream decision
   - Learnings: the personality document is a real quality gate, not a formality; spending three minutes on it changes the output profoundly

4. **Home prototype — variants A and B**
   - Why we picked 2 variants not 1 (load-bearing direction axis)
   - Variant A = editorial / portrait-led / airy; Variant B = civic / data-led / dense
   - What drove the choice (civic register fit TRH's 100-year-institution register better than editorial warmth)
   - Learnings: present clearly distinct directions, not A/A'; commit to a register before iterating on details

5. **Responsive by default — rule added mid-session**
   - The prototype shipped desktop-only per stardust default; user pushed back immediately
   - How the fix propagated through `.impeccable.md` ("no prototype is considered complete if it only renders at 1440px") and later `_tokens.css` responsive breakpoints
   - Learnings: the stardust default of desktop-only at 1440 is wrong for any modern deliverable; the personality document should inherit responsive as a hard rule from day one

6. **The first plan — 84 pages, 15 templates**
   - How the sitemap crawl surfaced the real IA (not the nav)
   - Why 84 pages reduced to 15 templates (the architecture was repetitive, the redesign is not)
   - Priority-1 / priority-2 / priority-3 split
   - Learnings: always crawl the sitemap before estimating; page count is not template count; a WordPress site's IA is almost always over-branched and simplifies dramatically

7. **Autonomous execution — Phase 1 of the work**
   - What got built in one autonomous run: shared tokens CSS, 9 template prototypes, 10 derivatives, sitemap, landing, briefings
   - What went well: template-first rendering, token-sharing via `@import`, one-shot briefing authoring from the scrape JSON
   - What surprised us: Lorem ipsum on `/emergency-services/` and `/where-does-it-go/`, `/palmer-court/` being a volunteer page in disguise, repeated disclaimer boilerplate leaking across unrelated pages
   - Learnings: always read what's on the source site before trusting its IA; `copy_edited: true` with a reason is cheap and load-bearing

8. **The first critique (home page only)**
   - Dual-pass — sub-agent design review + `npx impeccable --json` detector
   - Findings summary: 24 / 40 on Nielsen; Inter as display + body was the loudest AI-slop tell; triage buried below the hero; services grid symmetric; `href="#"` everywhere; Give row cold
   - How we fixed: Archivo + Public Sans + Source Serif 4 (none on reflex-reject), emergency ribbon, services asymmetry, Give-row promotion, dead-link sweep
   - Learnings: detector false-positives on contrast inside dark sections are noise but the font + structural findings are gold; critique before migrating the rest is correct sequencing (the fixes propagate)

9. **GitHub publication**
   - Why private first, then public (licensing implications of scraped copy + fictional client pull-quotes)
   - The decision to ship `README.md` that names the caveats loudly
   - Learnings: a README that is honest about what's fake and what's real is more useful than a README that performs finished-ness

10. **Phase 2 — critique rest + migrate rest + generate imagery + validate**
    - Sampled critique on 9 representatives vs per-page critique on 19 — why sampling was correct
    - Building the 4 new templates (T-08 Person, T-10 Get-involved program, T-13 History, T-15 Legal)
    - ~40 priority-2 pages rendered from templates, ~24 priority-3 pages stubbed
    - Phase 3 imagery generation with Gemini 3 Pro Image Preview — 20 slots, architectural/atmospheric not photorealistic portraits
    - Phase 4 validation — link health, image health, consistency sweep, random-sample critique
    - Learnings from each step

11. **Closing learnings — for the next site**

   A distilled playbook. Bullet form. Not essay form. Examples of what should appear:
   - "Invoke stardust only after the sitemap has been crawled — never estimate from a nav menu"
   - "Responsive is a rule, not a Phase-2 polish task"
   - "Pick distinctive display + body fonts on day one; the reflex-reject list is binding"
   - "Dead-link hardening is mechanical and belongs in the renderer, not a post-critique fix"
   - "Don't generate portraits of fabricated people, ever"
   - "Write `copy_edited: true` with a reason; the cheap audit trail saves you the expensive one"
   - "Samples beat full passes for critique on repetitive templates"
   - "Lorem ipsum on a production nonprofit site is more common than expected — assume it"
   - "Ship a JOURNAL.md on day one of the next project, not at the end"

   Target: 15–25 concrete, reusable lessons. Each lesson is one sentence, action-oriented, grounded in a specific thing that happened on this project.

### Writing style

- First-person plural ("we"), past tense
- Each chapter titled with the session's real name or goal, not "Step 4.2.1"
- Timestamps where meaningful ("one autonomous run, roughly 90 minutes")
- Quote specific hexes, file paths, font names, page slugs — not abstractions
- Do not repeat the plan; tell the *story* of executing the plan
- Call out dead ends and bad ideas we tried and abandoned — those are the most valuable pages

**Expected length:** 2,500–4,000 words. If it's shorter than 2,000 the chapters are too thin; if it's longer than 5,000 the writing is not doing its job.

**Output:** `stardust/JOURNAL.md`. Final commit after validation.

---

By the end of the run, the tree should look like:

```
theroadhouse/
├── index.html                                   ← stakeholder landing (unchanged)
├── .impeccable.md
├── stardust/
│   ├── brand-profile.json
│   ├── brand-board.html
│   ├── PLAN.md                                   ← original plan
│   ├── PLAN2.md                                  ← this document
│   ├── EXECUTION_LOG.md                          ← appended with Phase-1/2/3/4 notes
│   ├── VALIDATION_REPORT.md                      ← new · stakeholder-readable
│   ├── JOURNAL.md                                ← new · full project narrative + lessons
│   ├── assets/
│   │   ├── logo.png
│   │   ├── pages/                                ← scraped TRH imagery (existing)
│   │   └── generated/                            ← NEW · 20 Gemini-generated images + _manifest.json
│   ├── briefings/                                ← grows to ~83 files
│   ├── prototypes/
│   │   ├── _tokens.css                           ← may gain fixes from Phase 1
│   │   ├── home-a.html
│   │   └── home-b.html
│   └── pages/                                    ← grows to ~85 files
│       ├── index.html
│       ├── sitemap.html                          ← rewritten to list everything
│       ├── (20 existing priority-1 pages)
│       ├── (~40 priority-2 pages, full templates)
│       ├── (~24 priority-3 pages, styled stubs)
│       └── (new template originals for T-08, T-10, T-13, T-15)
```

---

## Stop conditions (same shape as the Phase-1 plan)

- Elapsed > 4 hours → write partial `VALIDATION_REPORT.md` explaining where the run stopped, and stop.
- A single page blocks the render (asset fetch loop, bad content from the scraper) → skip it with a log entry, keep going.
- Playwright blocked by the source site repeatedly → fall back to serving whatever content the Phase-1 scrape already captured; do not re-attempt per page beyond 3 retries.
- A new antipattern that triggers a `/typeset`-level redesign mid-run → defer that fix to the validation report as a recommendation, do not attempt to re-typeset mid-run.
- Gemini 3 Pro Image Preview API rejects > 5 prompts consecutively (rate-limit, credential expired, etc.) → stop Phase 3 early, keep the successfully generated images, fall back to Unsplash for the rest, log the cutoff point, and continue to Phase 4 validation on what's there.
- The credential at `/Users/paolo/excat/vitamix-gensite/.env` is missing or malformed → skip Phase 3 entirely, proceed to Phase 4 with Unsplash in place, and flag this prominently in `VALIDATION_REPORT.md` so the stakeholder knows imagery generation was deferred.
- If any earlier phase stops short, Phase 5 (journal) still runs — it writes the story of what we did, including the stop point. Shipping without a journal is not an option.

---

## What I will NOT do (explicit scope caps)

- I will not change the brand palette, the typography pair, or the `_tokens.css` architecture beyond what Phase-1 critique demands.
- I will not touch home-a.html (retained as design history).
- I will not migrate blog posts, individual story pages, or individual team-member detail pages beyond what's in `page-sitemap.xml` (the team/story sitemaps contain a separate long tail that's out of scope for this run).
- I will not implement real form submission, payment integration, CMS wiring, platform handoff, or production image optimization. Those remain Phase-D concerns.
- I will not produce marketing copy, campaign pages, or fundraising event pages beyond what the live site already carries.
- **I will not generate photorealistic portraits of fabricated people**, regardless of how much easier it would make the page look. Client, staff, and board portraits stay flagged for human replacement. Phase-3 imagery is architectural, atmospheric, and contextual only.

---

## Open for approval

This is the contract for the autonomous run. Confirm to execute.
