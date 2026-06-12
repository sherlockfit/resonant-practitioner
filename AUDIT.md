# Site Audit — 2026-06-12

Scope: post-PR #1 content fill. Audit performed at commit `3f6e636a3c56686a1113058d5a4c816f92a193db`.

## 1. Summary

- Total pages audited: 22
- Broken internal links: 4 (all were stub-redirect targets; fixed in this PR — see §9)
- Missing routes: 0
- Front-matter issues: 3 (eyebrow inconsistency on appendices; two Appendix B titles missing prefix)
- Cross-link gaps: 1 (Module 00 has no back-link; by design as entry point — flagged for awareness)
- Asset issues: 0
- Build-blocker risks: 2 (missing `url:` and `baseurl:` in `_config.yml`; no GitHub Actions Pages workflow)

---

## 2. Build verification

### 2.1 `_config.yml` review

| Key | Value | Status | Notes |
|---|---|---|---|
| `title` | `The Resonant Practitioner` | ✅ Present | Used in `<title>` tag |
| `description` | `A self-directed graduate program…` | ✅ Present | |
| `permalink` | `pretty` | ✅ Present | Directory-style URLs |
| `markdown` | `kramdown` | ✅ Present | With GFM input |
| `theme` | *(absent)* | ⚠️ Missing | Using custom `_layouts/default.html` — not a gem-based theme. Fine for GitHub Pages but should be documented. |
| `url` | *(absent)* | ⚠️ Missing | Absent. Absolute URLs will not resolve correctly until set to `https://sherlockfit.github.io`. Not a hard build blocker but required for any `{{ site.url }}` references. |
| `baseurl` | *(absent)* | ⚠️ Missing | Absent. For a Project Page at `/resonant-practitioner/` this must be set to `/resonant-practitioner`; for a User/Org Page it should be `""`. Confirm Pages URL and set accordingly. |
| `plugins` | *(absent)* | ℹ️ Note | No plugins declared. `kramdown` + custom layout requires no allowlisted plugins. Fine as-is. |
| `collections` | *(absent)* | ℹ️ Note | Not using Jekyll collections — directory-based `index.md` files used instead. Consistent with current structure. |
| `defaults` | `layout: default` for all paths | ✅ Present | All pages get the default layout. |
| `exclude` | `README.md`, `pdf/`, `Gemfile`, `vendor` | ✅ Present | Build artifacts excluded. |

### 2.2 Route coverage matrix

All routes referenced in `_includes/nav.html` and `index.md` were verified against the file tree.

| Expected route | File | Exists? | Notes |
|---|---|---|---|
| `/` | `index.md` | ✅ | |
| `/handbook/` | `handbook/index.md` | ✅ | |
| `/handbook/00-orientation/` | `handbook/00-orientation/index.md` | ✅ | |
| `/handbook/01-lineage/` | `handbook/01-lineage/index.md` | ✅ | |
| `/handbook/02-anatomy/` | `handbook/02-anatomy/index.md` | ✅ | |
| `/handbook/03-nervous-system/` | `handbook/03-nervous-system/index.md` | ✅ | |
| `/handbook/04-rhythm/` | `handbook/04-rhythm/index.md` | ✅ | |
| `/handbook/05-music/` | `handbook/05-music/index.md` | ✅ | |
| `/handbook/06-synchronization/` | `handbook/06-synchronization/index.md` | ✅ | |
| `/handbook/07-flow/` | `handbook/07-flow/index.md` | ✅ | |
| `/handbook/08-emotional-arc/` | `handbook/08-emotional-arc/index.md` | ✅ | |
| `/handbook/09-teaching/` | `handbook/09-teaching/index.md` | ✅ | |
| `/handbook/10-instructional/` | `handbook/10-instructional/index.md` | ✅ | |
| `/handbook/11-methodology-lab/` | `handbook/11-methodology-lab/index.md` | ✅ | |
| `/handbook/12-public-practice/` | `handbook/12-public-practice/index.md` | ✅ | |
| `/handbook/capstone/` | `handbook/capstone/index.md` | ✅ | |
| `/handbook/appendix-a-audit/` | `handbook/appendix-a-audit/index.md` | ✅ | |
| `/handbook/appendix-b-class-construction/` | `handbook/appendix-b-class-construction/index.md` | ✅ | Main Appendix B content |
| `/handbook/appendix-b-class-builder/` | `handbook/appendix-b-class-builder/index.md` | ✅ | Redirect stub only — not in nav |
| `/handbook/appendix-c-cue-library/` | `handbook/appendix-c-cue-library/index.md` | ✅ | |
| `/handbook/appendix-d-resources/` | `handbook/appendix-d-resources/index.md` | ✅ | |
| `/summaries/` | `summaries/index.md` | ✅ | |
| `/workbook/` | `workbook/index.md` | ✅ | |

**Result:** All 22 routes resolve. No 404s detected.

Extra route not in nav: `/handbook/appendix-b-class-builder/` — stub redirect page, left in place intentionally to preserve old bookmarks.

### 2.3 Build-blocker risks

| Risk | Severity | Detail |
|---|---|---|
| `url:` absent in `_config.yml` | Low | No `{{ site.url }}` references found in content, so no immediate break. Set before enabling sitemaps or canonical tags. |
| `baseurl:` absent in `_config.yml` | Medium | If the site is served as a Project Page at `/resonant-practitioner/`, all `relative_url` filters will produce incorrect paths without `baseurl: /resonant-practitioner`. Confirm the Pages URL scheme. |
| No GitHub Actions Pages workflow | Info | Pages is using the classic branch-build path (deploy from `main`). No `.github/workflows/` directory exists. This works but provides no build preview or failure notification on PR. Consider adding the standard `actions/jekyll-build-pages` workflow. |
| No `Gemfile` present | Info | `Gemfile` is listed in `exclude` but does not exist in the repo. GitHub Pages will use its default Jekyll version. Pinning with a `Gemfile` is recommended for reproducible builds. |
| Duplicate redirect route | Low | `handbook/appendix-b-class-builder/` exists as both a live route (stub page) and an unlisted legacy slug. The stub correctly redirects to `appendix-b-class-construction/`. No Jekyll build conflict, but long-term the stub directory could be removed once all inbound links are updated. |
| Invalid YAML front matter | None detected | All `index.md` files parsed without errors. No smart quotes in YAML keys. No leading BOM characters detected. |
| Unclosed Liquid tags | None detected | `_layouts/default.html` and `_includes/nav.html` use only simple `{% include %}` and `{{ }}` tags; all appear balanced. |
| Duplicate `permalink:` values | None detected | No page-level `permalink:` front-matter keys found; the site relies entirely on the global `permalink: pretty` setting. |
| Unsupported plugins | None | No plugins declared; kramdown is on the GitHub Pages allowlist. |

---

## 3. Broken internal links

All Markdown internal links in all 22 content pages were checked against the file tree.

| Source | Target | Reason |
|---|---|---|
| `handbook/05-music/index.md` | `../appendix-b-class-builder/` | Pointed to redirect stub instead of real Appendix B content at `../appendix-b-class-construction/`. **Fixed in this PR.** |
| `handbook/08-emotional-arc/index.md` | `../appendix-b-class-builder/` | Same as above. **Fixed in this PR.** |
| `handbook/10-instructional/index.md` | `../appendix-b-class-builder/` | Same as above. **Fixed in this PR.** |
| `handbook/appendix-c-cue-library/index.md` | `../appendix-b-class-builder/` | Same as above (also had stale label "Class Builder Toolkit"). **Fixed in this PR.** |

No other broken or unresolvable internal links were found.

---

## 4. Front-matter audit

| Path | title | eyebrow | subtitle | Notes |
|---|---|---|---|---|
| `index.md` | "The Resonant Practitioner" | "v1.0" | ✅ | `eyebrow: "v1.0"` is unconventional — other pages use a category label; this uses a version string. Flag for consistency review. |
| `handbook/index.md` | "The Handbook" | "Table of Contents" | ✅ | |
| `handbook/00-orientation/index.md` | "Module 0 — Orientation & Personal Mandate" | "Module 0" | ✅ | `eyebrow` is module number, unlike Modules 1–12 which use section names. Inconsistent. |
| `handbook/01-lineage/index.md` | "Module 1 — Lineage Study: The Class & Its Cousins" | "Foundations" | ✅ | |
| `handbook/02-anatomy/index.md` | "Module 2 — Functional Anatomy & Biomechanics" | "Foundations" | ✅ | |
| `handbook/03-nervous-system/index.md` | "Module 3 — Nervous System, Interoception & Trauma-Informed Practice" | "Foundations" | ✅ | |
| `handbook/04-rhythm/index.md` | "Module 4 — Neuroscience of Rhythm & Entrainment" | "Science of the Room" | ✅ | |
| `handbook/05-music/index.md` | "Module 5 — Music Psychology & Sonic Design" | "Science of the Room" | ✅ | |
| `handbook/06-synchronization/index.md` | "Module 6 — Group Synchronization & Collective Effervescence" | "Science of the Room" | ✅ | |
| `handbook/07-flow/index.md` | "Module 7 — Flow States & Peak Experience Engineering" | "Science of the Room" | ✅ | |
| `handbook/08-emotional-arc/index.md` | "Module 8 — Emotional Arc Design" | "Craft" | ✅ | |
| `handbook/09-teaching/index.md` | "Module 9 — Teaching Methodology" | "Craft" | ✅ | |
| `handbook/10-instructional/index.md` | "Module 10 — Instructional Design & Class Construction" | "Craft" | ✅ | |
| `handbook/11-methodology-lab/index.md` | "Module 11 — Methodology Design Lab" | "Originality & Influence" | ✅ | |
| `handbook/12-public-practice/index.md` | "Module 12 — Public Practice: The Iconic Voice" | "Originality & Influence" | ✅ | |
| `handbook/capstone/index.md` | "Capstone Project" | "Synthesis" | ✅ | |
| `handbook/appendix-a-audit/index.md` | "Appendix A — Curriculum Audit / Pseudoscience Audit" | "Appendices" | ✅ | Title uses `/` instead of `—` to separate sub-title; minor style drift. |
| `handbook/appendix-b-class-builder/index.md` | "Class Construction Toolkit" | "Appendix B" | ✅ (stub) | Title missing "Appendix B —" prefix. `eyebrow: "Appendix B"` differs from A, C, D which use `"Appendices"`. Stub redirect page only. |
| `handbook/appendix-b-class-construction/index.md` | "Class Construction Toolkit" | "Appendix B" | ✅ | Title missing "Appendix B —" prefix consistent with C and D. `eyebrow: "Appendix B"` differs from A, C, D which use `"Appendices"`. |
| `handbook/appendix-c-cue-library/index.md` | "Appendix C — The Master Cue Library" | "Appendices" | ✅ | |
| `handbook/appendix-d-resources/index.md` | "Appendix D — Recommended Reading & Resource Map" | "Appendices" | ✅ | |
| `workbook/index.md` | "Methodology Design Lab — 60-Page Workbook" | "Design Lab" | ✅ | |
| `summaries/index.md` | "Concept Summaries" | "Library" | ✅ | |

**Front-matter issues summary:**

1. **eyebrow inconsistency on Appendix B** — `appendix-b-class-builder` and `appendix-b-class-construction` use `eyebrow: "Appendix B"` while A, C, and D use `eyebrow: "Appendices"`. Should be standardized.
2. **title missing "Appendix B —" prefix** — Both Appendix B pages have `title: "Class Construction Toolkit"` without the `Appendix B —` prefix used by A, C, and D.
3. **eyebrow on Module 00** — Uses `"Module 0"` (a module number) rather than a section name like `"Orientation"`. All other modules use section-name eyebrows.
4. **eyebrow on root `index.md`** — Uses version string `"v1.0"` rather than a section-name label. Minor; arguably intentional but flag for review.

---

## 5. Cross-link coverage

| Page | Outbound internal links | Notes |
|---|---|---|
| `index.md` | `workbook/`, `summaries/`, all module cards | Rich link coverage on landing page. |
| `handbook/index.md` | All 22 sub-routes | Complete ToC. |
| `handbook/00-orientation/index.md` | `01-lineage/`, `appendix-b-class-construction/` | Entry point — no back-link by design. No forward link to `workbook/` or `summaries/`; consider adding. |
| `handbook/01-lineage/index.md` | `06-synchronization/`, `12-public-practice/`, `appendix-d/` | No back-link to Module 0 or link to workbook. |
| `handbook/02-anatomy/index.md` | `03-nervous-system/`, `09-teaching/`, `appendix-a/` | Good forward + reference links. |
| `handbook/03-nervous-system/index.md` | `08-emotional-arc/`, `appendix-a/`, `summaries/` | Good coverage. |
| `handbook/04-rhythm/index.md` | `05-music/`, `06-synchronization/`, `appendix-d/` | Good forward chain. |
| `handbook/05-music/index.md` | `08-emotional-arc/`, `appendix-b-class-construction/` (fixed), `summaries/` | Good. |
| `handbook/06-synchronization/index.md` | `07-flow/`, `12-public-practice/`, `summaries/` | Good. |
| `handbook/07-flow/index.md` | `08-emotional-arc/`, `summaries/` | Minimal but functional for a short module. |
| `handbook/08-emotional-arc/index.md` | `05-music/`, `09-teaching/`, `appendix-b-class-construction/` (fixed) | Good. |
| `handbook/09-teaching/index.md` | `10-instructional/`, `appendix-c/` | Good forward chain. |
| `handbook/10-instructional/index.md` | `11-methodology-lab/`, `appendix-b-class-construction/` (fixed) | Good. |
| `handbook/11-methodology-lab/index.md` | `workbook/`, `12-public-practice/`, `capstone/` | Good. |
| `handbook/12-public-practice/index.md` | `capstone/`, `appendix-a/`, `summaries/` | Good terminal-module coverage. |
| `handbook/capstone/index.md` | `08-emotional-arc/`, `11-methodology-lab/`, `workbook/` | Good synthesis links. |
| `handbook/appendix-a-audit/index.md` | `03-nervous-system/`, `summaries/` | Minimal. No link to Appendix D (resource map) or `workbook/`. |
| `handbook/appendix-b-class-construction/index.md` | `08-emotional-arc/`, `09-teaching/`, `10-instructional/`, `appendix-c/` | Good companion links. |
| `handbook/appendix-c-cue-library/index.md` | `08-emotional-arc/`, `09-teaching/`, `appendix-b-class-construction/` (fixed) | Good. |
| `handbook/appendix-d-resources/index.md` | *(none)* | **Zero outbound internal links.** Appendix D lists recommended texts but has no hyperlinks to the modules those texts support. |
| `workbook/index.md` | `11-methodology-lab/`, `capstone/` | Functional but sparse given 60-exercise scope. Could link back to relevant modules per section. |
| `summaries/index.md` | All cited modules and appendices | Excellent link density; every summary links to 2–3 modules. |

**Gaps flagged:**
- `handbook/appendix-d-resources/index.md` — **zero outbound internal links**. The reading list references modules by name but contains no hyperlinks.
- `handbook/00-orientation/index.md` — no link to `workbook/` (which students will use throughout).
- `handbook/appendix-a-audit/index.md` — no link to `appendix-d/` (Resource Map) which contains the reading list for audit skills.
- `workbook/index.md` — only 2 outbound links for 60 exercises; each section could link to its primary handbook module.

---

## 6. Assets

### 6.1 Referenced but missing

None found. The only asset reference in layouts is `assets/css/style.css`, which exists.

### 6.2 Present but unused

| File | Notes |
|---|---|
| `assets/css/style.css` | Used by `_layouts/default.html`. |

No images, fonts, scripts, or other asset files are present in `assets/`. The site loads Google Fonts via an external CDN link in `_layouts/default.html`.

**Asset inventory:** 1 file (`assets/css/style.css`). No orphaned assets. No missing referenced assets.

---

## 7. Content health flags

### 7.1 Short / stub pages

Pages with word counts (including front matter) likely below ~200 content words:

| File | Approx. words | Notes |
|---|---|---|
| `handbook/appendix-b-class-builder/index.md` | ~76 | Intentional redirect stub — acceptable. |
| `handbook/capstone/index.md` | ~187 | Contains rubric, practicum, deliverable but no explanatory prose. Consider expanding the introductory section. |
| `handbook/07-flow/index.md` | ~226 | Dense but functional; bullet format compresses word count. |
| `handbook/06-synchronization/index.md` | ~242 | Same as above. |
| `handbook/09-teaching/index.md` | ~226 | Same. |
| `handbook/10-instructional/index.md` | ~227 | Same. |
| `handbook/11-methodology-lab/index.md` | ~200 | Borderline; workbook integration section compensates. |

Modules 6, 7, 9, 10, 11 are terse but structured (they use lists and tables efficiently). The capstone page is the only one that reads as potentially under-developed.

### 7.2 TODO / placeholder markers

No `TODO`, `FIXME`, `TBD`, or `Lorem ipsum` strings found anywhere in the repository content.

### 7.3 Heading hierarchy issues

No `# H1` headings appear in any content page (correct — the layout renders `page.title` as the `<h1>`).

No heading hierarchy jumps (e.g., H2 → H4) were detected. All content pages use only `##` and `###` levels.

One structural note: `handbook/appendix-d-resources/index.md` uses `### H3` as the first child of `## Annotated reading by module`, then jumps back to `## H2` for Podcasts. This is valid hierarchy but slightly unusual.

### 7.4 Accessibility flags

| Issue | Location | Detail |
|---|---|---|
| No images present | All pages | No `<img>` tags or `![]()` Markdown images anywhere in content — alt-text issue is moot. |
| External CDN fonts | `_layouts/default.html:10` | Google Fonts loaded from `fonts.googleapis.com`. Will not load in offline/restricted environments. Not a breaking issue but worth noting. |
| Generic link text | None found | No "here", "click here", or "read more" link text detected. |
| Skip link | `_layouts/default.html:14` | `<a class="skip-link" href="#content">Skip to content</a>` is present. ✅ |
| `lang` attribute | `_layouts/default.html:2` | `<html lang="en">` is present. ✅ |
| Viewport meta | `_layouts/default.html:4` | Present. ✅ |

---

## 8. Recommended next actions (ranked)

1. - [ ] **Set `url` and `baseurl` in `_config.yml`** — Confirm whether the Pages URL is a User Page (`https://sherlockfit.github.io/`) or Project Page (`https://sherlockfit.github.io/resonant-practitioner/`) and set `url` and `baseurl` accordingly. Blocking for any absolute-URL feature (sitemaps, canonical tags, social sharing). [`_config.yml`]

2. - [ ] **Add outbound internal links to `handbook/appendix-d-resources/index.md`** — The recommended reading list references modules by name but has zero hyperlinks. Add links to the relevant handbook modules for each section. [`handbook/appendix-d-resources/index.md`]

3. - [ ] **Add GitHub Actions Pages workflow** — Add `.github/workflows/pages.yml` using the standard `actions/jekyll-build-pages` action so that build failures are surfaced on PRs rather than discovered only after merge. Refer to [GitHub Pages starter workflow](https://github.com/actions/starter-workflows/blob/main/pages/jekyll.yml).

4. - [ ] **Standardize `eyebrow` on Appendix B pages** — `appendix-b-class-builder` and `appendix-b-class-construction` use `eyebrow: "Appendix B"` while A, C, D use `eyebrow: "Appendices"`. Decide on one pattern and apply consistently. [`handbook/appendix-b-class-construction/index.md`]

5. - [ ] **Add "Appendix B —" prefix to Appendix B titles** — Both Appendix B pages have `title: "Class Construction Toolkit"` without the `Appendix B —` prefix used by A, C, D. [`handbook/appendix-b-class-construction/index.md`, `handbook/appendix-b-class-builder/index.md`]

6. - [ ] **Standardize Module 00 `eyebrow`** — Currently `"Module 0"` (a number) while Modules 1–12 use section-name strings (`"Foundations"`, `"Craft"`, etc.). Consider `"Orientation"` or `"Introduction"`. [`handbook/00-orientation/index.md`]

7. - [ ] **Expand `handbook/capstone/index.md`** — The page is functional (~187 words) but has minimal explanatory prose. A short section on what a strong capstone looks like would help students preparing their dossier. [`handbook/capstone/index.md`]

8. - [ ] **Add module cross-links to `workbook/index.md` per section** — The workbook has 8 thematic sections; each could open with a link to its primary handbook module(s). Currently only 2 outbound links for 60 exercises. [`workbook/index.md`]

9. - [ ] **Add a `Gemfile` (or document Jekyll version)** — Without a `Gemfile`, GitHub Pages will use its current default Jekyll version. Pinning the version prevents silent behavior changes. See [GitHub Pages dependency versions](https://pages.github.com/versions/).

10. - [ ] **Decide fate of `handbook/appendix-b-class-builder/` stub** — Now that all inbound links have been corrected to point to `appendix-b-class-construction/`, the stub exists only for legacy bookmark preservation. Either keep it (benign) or remove it and note that in a redirect strategy. [`handbook/appendix-b-class-builder/index.md`]

11. - [ ] **Review `eyebrow: "v1.0"` on root `index.md`** — All other pages use descriptive category labels as eyebrows. The version string on the landing page is a style outlier; consider `"Graduate Program"` or similar. [`index.md`]

12. - [ ] **Add links from `handbook/00-orientation/` to `workbook/`** — Students start at Module 0 but the workbook is only mentioned in Module 11. A forward-pointer at Orientation would help students know the companion resource exists early. [`handbook/00-orientation/index.md`]

---

## 9. Fixes applied in this PR

| File | Fix | Reason |
|---|---|---|
| `handbook/05-music/index.md` | Changed `[Appendix B](../appendix-b-class-builder/)` → `[Appendix B](../appendix-b-class-construction/)` | Link pointed to the redirect stub rather than the main Appendix B content page; target was unambiguous. |
| `handbook/08-emotional-arc/index.md` | Same fix as above | Same reason. |
| `handbook/10-instructional/index.md` | Same fix as above | Same reason. |
| `handbook/appendix-c-cue-library/index.md` | Changed `[Appendix B — Class Builder Toolkit](../appendix-b-class-builder/)` → `[Appendix B — Class Construction Toolkit](../appendix-b-class-construction/)` | Stale link to stub; also corrected the link label from "Class Builder" to "Class Construction" to match the actual page title. |
