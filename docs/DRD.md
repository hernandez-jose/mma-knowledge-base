# Design Requirements Document — MMA Knowledge Base (web app)

| | |
|---|---|
| **Status** | Draft v1 — for review |
| **Date** | 2026-10-03 |
| **Companion** | [`PRD.md`](PRD.md) — Product Requirements Document (requirement IDs such as F-9 refer to it) |
| **Scope** | Information architecture, visual system, components, page templates, interaction, accessibility, content design. Not a visual mock-up set: it specifies what the mock-ups must satisfy. |

---

## 1. Design principles

1. **Reference first, not feed.** This is a dictionary people consult mid-class. Dense, scannable, fast. No carousels, no autoplay, no infinite scroll.
2. **One rating language.** Difficulty and Commonness look and read identically in every art. A "3" in Boxing is a "3" in Muay Thai.
3. **Honesty is visible.** Review status, ruleset "as of" dates and "informed judgment" notes are part of the UI, not a footer.
4. **Safety where it matters.** Safety and legality sit on the entry, above the fold on the technique's own page, never behind a link.
5. **Phone in a gym.** One-handed, sweaty, bad light, poor signal. Large targets, high contrast, offline.
6. **Each art has a personality, the system has one grammar.** Colour and a small motif identify the art; layout, components and behaviour are shared.
7. **Show the mechanism.** Diagrams of range, stance and position carry meaning (the Jiu-Jitsu position ladder, the striking range ladder). Decoration is not added for its own sake.

## 2. Information architecture

```
Home
├── Jiu-Jitsu   (15 parts, mirrors jujitsu.md)
├── Boxing      (15 parts, mirrors boxing.md)
├── Kickboxing  (15 parts, mirrors kickboxing.md)
├── Muay Thai   (18 parts, mirrors muay-thai.md)
├── MMA hub     (ranges across arts · transitions · ruleset compare · mixed learning paths)
├── Learn       (paths · beginner primer · glossary · concepts)
├── Compare     (rulesets · cross-art equivalents · styles)
├── Search      (global, filters)
└── My          (bookmarks · progress · notes — local first)
```

**Rule:** the Parts of each art are shown in the atlas's own order and with its own names, so someone holding the Markdown and someone using the app see the same structure.

**Depth budget:** Home → Art → Part/Section → Entry is three interactions. Search and "Start here" are shortcuts that skip levels.

### 2.1 URL scheme (stable, readable, shareable)

| Page | Pattern | Example |
|---|---|---|
| Art | `/{art}` | `/muay-thai` |
| Part | `/{art}/{part-slug}` | `/muay-thai/the-clinch` |
| Entry | `/{art}/{entry-slug}` | `/boxing/jab`, `/muay-thai/teep-rear` |
| Compare | `/compare/rulesets?set=k1,muay-thai-stadium` | |
| Filters | query string (F-11) | `/search?art=boxing&kind=defense&diff=1-3` |
| Path | `/learn/{art}/{path-slug}` | `/learn/boxing/foundations` |

Entry IDs never change; renames create a redirect from the old slug.

### 2.2 Navigation

- **Top bar (all breakpoints):** logo · art switcher (four art chips + MMA) · search · menu.
- **Phone:** bottom tab bar — *Arts · Search · Learn · Compare · My*. Search is reachable with the thumb.
- **Desktop:** left rail with the current art's Parts (sticky, collapsible), content in the centre, an "On this page" rail on the right for long sections.
- **Breadcrumb** on every page below Home (F-4). The art colour appears as a thin bar, not as the page background.

## 3. Visual system

### 3.1 Colour — tokens

All colours are defined as tokens; components use tokens only. Light and dark themes ship together; the default follows the OS, with a manual toggle.

**Neutrals**

| Token | Light | Dark |
|---|---|---|
| `--bg` | `#ffffff` | `#0f1115` |
| `--surface` | `#f5f6f8` | `#171a21` |
| `--border` | `#d9dce3` | `#2a2f3a` |
| `--text` | `#111827` | `#f3f4f6` |
| `--text-muted` | `#4b5563` | `#9ca3af` |

**Art accents** (used for the art bar, the active chip and links within an art — never as the only carrier of meaning)

| Art | Token | Light | Dark | Contrast vs `--bg` (light / dark) |
|---|---|---|---|---|
| Jiu-Jitsu | `--art-bjj` | `#1d4ed8` (blue) | `#93c5fd` | 6.7 : 1 / 10.5 : 1 |
| Boxing | `--art-boxing` | `#b91c1c` (red) | `#fca5a5` | 6.5 : 1 / 10.0 : 1 |
| Kickboxing | `--art-kick` | `#a16207` (amber) | `#fcd34d` | 4.9 : 1 / 13.1 : 1 |
| Muay Thai | `--art-mt` | `#047857` (green) | `#6ee7b7` | 5.5 : 1 / 12.4 : 1 |
| MMA hub | `--art-mma` | uses `--text` | uses `--text` | n/a (neutral) |

Contrast ratios were calculated from the hex values against the background tokens (WCAG relative-luminance formula) and all pass AA (≥ 4.5 : 1) for text. Recheck whenever a token changes — CI should run a contrast check.

**Rating colours — a sequential scale, plus numerals and labels.** Colour is secondary: every rating also shows its number and word. Use one hue stepped in lightness (light steps for 1, dark for 5) so the scale reads for colour-blind users; do **not** use red/green.

### 3.2 Typography

| Role | Spec |
|---|---|
| UI and body | A humanist sans-serif system stack, with a web font only if it can be subset and self-hosted (target ≤ 60 KB). |
| Headings | Same family, heavier weight. |
| Technique names | Semi-bold, never all-caps (Thai and Portuguese terms need legible diacritics). |
| Native scripts (Thai, Japanese) | A font stack that explicitly includes Noto Sans Thai / Noto Sans JP fallbacks; shown beside the Latin transliteration, not instead of it. |
| Numerals | Tabular for ratings and tables. |
| Size | Body 16 px minimum (17–18 px on content pages); table text 15 px minimum; never below 14 px for any text. |
| Line length | 60–75 characters for prose. |

### 3.3 Spacing, shape and elevation

4 px base unit; spacing scale 4 / 8 / 12 / 16 / 24 / 32 / 48. Radius 8 px on cards and chips, 4 px on inputs and table cells. No shadows in the table views; a single subtle shadow for floating elements (menus, filter sheet). Tap targets ≥ 44 × 44 px.

### 3.4 Iconography and illustration

- **Icon set:** one consistent line-icon family, 1.5 px stroke. Each *kind* has an icon (strike, kick, knee, elbow, defense, counter, hold, takedown, sweep, escape, submission, concept, drill, rule).
- **Art marks:** one simple geometric mark per art (for example: a glove, a shin, a belt-knot, a lattice for the clinch) used in the art switcher and as a decorative header glyph. Placeholders until commissioned.
- **Diagrams:** vector (SVG), theme-aware. Required at launch: (a) striking **range ladder** (long / mid / short / clinch), (b) Jiu-Jitsu **position ladder** (the +4 … −4 table from `jujitsu.md`), (c) stance diagrams (orthodox / southpaw foot and hand position). Figure renders already in `assets/figures3d/` may be reused for Jiu-Jitsu positions subject to the rights check in PRD Q3.
- **Photography/video:** none hosted in v1. External references open in a new tab and are labelled with their source.

## 4. Core components

### 4.1 Rating chips (`Diff`, `Com`)

```
Difficulty  ●●●○○  3 · Moderate        How common  ●●●●○  4 · Very common
```

- Always shows **dots + number + word**. The word comes from the shared scale (*Day one · Easy · Moderate · Hard · Expert*; *Rare · Occasional · Common · Very common · Everywhere*).
- Compact variant for table rows: `D3 · C4` with a tooltip/long-press showing the words. A screen reader hears "Difficulty 3 of 5, moderate. How common 4 of 5, very common."
- Unrated/illegal items show "—" and a text reason ("illegal in competition"), not a blank.

### 4.2 Entry row (table)

Columns on desktop: **Name (+ alias) · What it is · D · C · Legality dots · ★ bookmark**. On phones the row becomes a card: name and ratings on the first line, one-line description below, expandable. Rows are links; the entire row is a target.

### 4.3 Legality strip

A compact strip of ruleset badges on an entry, e.g.:

```
K-1 ✕ banned    Muay Thai (stadium) ✓ legal    Muay Thai (amateur) ⚠ restricted    Unified MMA ✓ legal
```

Each state uses an icon **and** a word (✓ legal / ⚠ restricted / ✕ banned / ? unverified). Hovering/tapping shows the "as of" date and source.

### 4.4 Review-status badge

*Draft · Reviewed · Verified* with the reviewer's role and date on tap. Draft items carry a visible, non-alarming "not yet reviewed by a practitioner" label. (PRD F-29)

### 4.5 Safety block

A bordered block with an icon, shown on entries with injury risk. Contains: risk, tap/stop rule, "train with a coach" line. It is **not** collapsible and not styled as a warning-banner clone — it should be noticeable without being cry-wolf. Joint locks, chokes, head strikes, slams and leg kicks require it.

### 4.6 Filter bar and sheet

- Desktop: horizontal chip bar (Art · Kind · Diff · Com · Range · Weapon · Target · Stance · Ruleset · Gi) with an "All filters" side panel.
- Phone: a "Filters" button opens a bottom sheet with the same controls; applied filters appear as removable chips under the search box; result count updates live.
- Ruleset filter changes **legality** from informative to filtering: "only techniques legal under: ___".
- Filter state lives in the URL (F-11).

### 4.7 Range ladder diagram (striking)

An interactive vertical or horizontal diagram of **long → mid → short → clinch**; selecting a range filters the entries to that range. Each range shows two or three representative entries.

### 4.8 Relationship rails

On an entry: **Counters** (what beats it), **Counter to** (what it beats), **Chains** (what comes before and after), **In other arts** (cross-art equivalents with a one-line "how they differ"). Each rail shows at most six items, then "See all".

### 4.9 Learning path stepper

A vertical stepper of tiers (Tier 0 → N) with entries, estimated order, and a progress bar. Completed items can be self-marked. Beginners see "Start here" first (F-13).

### 4.10 Compare tables

Ruleset matrix: rows = techniques or categories, columns = chosen rulesets; sticky first column and header; cells carry the legality icon + word; horizontally scrollable on phones with a visible scroll hint. Cross-art compare: columns = arts, rows = concept.

### 4.11 Glossary tooltip

Technique-specific terms (e.g. *plum*, *teep*, *shrimp*) underlined with a dotted line; hover/tap shows a definition and a link. Pronunciation shown as text in v1.

### 4.12 Callouts

Three types only: **Note** (neutral), **Honesty note** (informed judgment, provenance), **Safety** (Section 4.5). No decorative callouts.

## 5. Page templates

### 5.1 Home

Purpose: orient in five seconds and get to a technique.

```
┌──────────────────────────────────────────────┐
│  [logo]    Jiu-Jitsu  Boxing  Kick  Thai  MMA   🔍  ☰ │
├──────────────────────────────────────────────┤
│  The complete knowledge of the fighting arts          │
│  [ Search any technique, term or rule …        ]      │
├──────────────────────────────────────────────┤
│  ┌ Jiu-Jitsu ┐ ┌ Boxing ┐ ┌ Kickboxing ┐ ┌ Muay Thai ┐│
│  │ N entries │ │ N entries│ │ N entries  │ │ N entries ││
│  │ Start here│ │Start here│ │ Start here │ │Start here ││
│  └───────────┘ └──────────┘ └────────────┘ └───────────┘│
├──────────────────────────────────────────────┤
│  New to this?  [Beginner safety primer] [Pick a path]  │
│  Competing?    [Compare rulesets]                      │
│  MMA hub: ranges · transitions · mixed paths           │
└──────────────────────────────────────────────┘
```

Entry counts come from the data, not hard-coded. No auto-rotating hero.

### 5.2 Art landing

Header with art mark and colour bar · one-paragraph definition · **"The Map"** (the art's Part I, rendered as its ladder/range diagram) · Parts as a grid of cards each with an entry count and a coverage indicator (e.g. "27 entries · 3 deep dives") · "Start here" · "Gap analysis" link · rules summary with an "as of" date.

### 5.3 Part / Section

Section heading and intro · table of entries (4.2) · sticky mini-filter (Diff, Com, Kind) · "On this page" rail · print-friendly (F-26). Section intros reuse the atlas's blockquote notes (e.g. the "chokes are the best submissions" note), styled as notes.

### 5.4 Entry detail

Order is fixed (PRD 8.5). Above the fold on a phone: name, aliases, rating chips, one-line description, legality strip, safety block (if applicable). Then tabs or anchored sections: *How it works · Adjust for body · Mistakes · Chains · Defenses/counters · Drills · Related*. On entries without a deep dive: description, ratings, legality, relationships and a "Deep dive coming" panel with a link to report/request it.

### 5.5 Search

Search box focused on load; recent searches; results grouped by art with a count; each result shows name, matched alias (highlighted), kind, ratings, art colour bar. Empty state suggests spelling variants and links to the glossary. Zero-result searches are logged (privacy-respecting) to guide the content roadmap.

### 5.6 Learn

Path index per art and for MMA · beginner primer (safety first) · glossary (alphabetical and by language) · concepts. Primer is a short, scannable page, not a wall of text.

### 5.7 Compare

Choose rulesets (up to 4) or arts (up to 4) → matrix (4.10). Share link preserves the selection.

### 5.8 My

Bookmarks, "known / learning / want to learn" lists, path progress, notes. Clear statement that data is stored on this device unless signed in. Export and delete buttons.

## 6. Interaction and behaviour

| Topic | Requirement |
|---|---|
| **Search** | Opens with `/` on desktop; results as you type; keyboard navigable; alias and typo tolerant (F-8). |
| **Filters** | Apply immediately; result count announced to assistive tech; clear-all always visible. |
| **Tables** | Sortable by D, C, name; sort state announced; default order follows the atlas. |
| **Bookmarks** | One tap; optimistic update; works offline; no sign-in prompt on first use. |
| **Navigation** | Back button always returns to the same scroll position and filter state. |
| **Motion** | Short (≤ 200 ms) transitions for sheets and chips only; respects `prefers-reduced-motion`; nothing animates by itself. |
| **Loading** | Static content renders without JS; search and personalisation load progressively. Skeletons only for deferred components. |
| **Errors** | Plain language and a way forward; offline pages show cached content with a "saved copy" banner. |
| **First-run primer** | The safety primer is offered, not forced, on first open; it is required once before opening a learning path flagged as contact-heavy. |

## 7. Responsive behaviour

| Breakpoint | Layout |
|---|---|
| **≤ 480 px (phone)** | Single column; bottom tab bar; entries as cards; filters as bottom sheet; compare tables scroll horizontally with sticky first column. |
| **481–900 px (tablet)** | Two-column content; the left rail collapses to a drawer. |
| **≥ 901 px (desktop)** | Left rail + content + optional "On this page" rail; tables with full columns. |

Content is designed phone-first. No horizontal page scroll at any width; 16 px side gutters on phones. Wide tables scroll inside their own container.

## 8. Accessibility (WCAG 2.2 AA minimum)

- **Colour:** text contrast ≥ 4.5 : 1, large text and UI components ≥ 3 : 1; art accents verified (Section 3.1). Meaning is never colour-only: ratings = dots + number + word; legality = icon + word; art = colour bar + label.
- **Keyboard:** all functionality operable by keyboard; visible focus indicator (2 px, ≥ 3 : 1 against adjacent colours); skip-to-content link; logical tab order; no keyboard traps in sheets and menus.
- **Screen readers:** semantic landmarks; tables use real `<table>`, headers, and `scope`; cards that replace tables on phones keep the same reading order; live regions announce filter result counts.
- **Targets:** ≥ 44 × 44 px (exceeds the 24 px AA minimum).
- **Text:** supports 200% zoom and text-spacing overrides without loss of content; no text in images.
- **Motion and flashing:** respect reduced-motion; no flashing content.
- **Language:** `lang` attributes on Thai, Portuguese and Japanese terms so screen readers pronounce them correctly.
- **Forms:** labels, error messages tied to fields, no placeholder-only labels.
- **Testing:** automated (axe) in CI plus manual keyboard and screen-reader passes (VoiceOver, TalkBack, NVDA) before launch; audit by a person who uses assistive technology regularly where possible.

## 9. Content design

### 9.1 Voice

Second person, plain and direct. **They/them** for opponents and any unnamed person. Short sentences. Specifics over adjectives (a distance, an angle, a grip) — the same rule the technique library already sets. No hype, no tough-guy copy, no promise of self-defense results.

### 9.2 Standard strings

- Honesty note (shown once per art landing and on the ratings legend): *"Difficulty and commonness are informed coaching judgment, not a peer-reviewed dataset."*
- Safety line (on contact-heavy pages): *"Learn this with a qualified coach and a cooperative partner. Tap early; release immediately."*
- Rules line: *"Rules vary by organiser and change over time. Confirm with the current rulebook before competing. As of {date}."*
- Unreviewed line: *"This entry hasn't been reviewed by a practitioner yet."*

### 9.3 Naming and aliases

Display the canonical English name first, then aliases in lighter text (e.g. *Horizontal elbow — sok tat / sok tad*). Never state that one transliteration is the only correct one. Native script (e.g. Thai) is shown only where verified.

### 9.4 Ratings legend

Present on every table-heavy page via an info button and as a fixed page in Learn: the two scales, what each number means, and the honesty note.

## 10. Data visualisation

- Rating dots and legality icons follow Section 4.
- Range ladder and position ladder (Section 3.4) are the only "hero" diagrams in v1.
- Any chart (e.g. entry counts per art, coverage meters) uses a single sequential palette, direct labels rather than legends where possible, and an accessible table alternative.
- Coverage meters on art landings show *deep dives completed / entries rated*, and are intentionally honest when low.

## 11. Performance and technical design constraints

| Constraint | Requirement |
|---|---|
| Rendering | Static HTML for every content page; hydrate only interactive islands (search, filters, bookmarks). |
| Weight | ≤ 200 KB gzipped JS on content pages; fonts subset and self-hosted; images lazy-loaded SVG/WebP. |
| Search index | Built at build time, split by art, lazy-loaded on first focus of the search box. |
| Caching | Service worker precaches the shell and content; stale-while-revalidate for content; versioned cache keys per content release. |
| Printing | A print stylesheet for tables and entry pages (F-26): black on white, no navigation, URLs in footnotes. |
| Theming | CSS custom properties; `prefers-color-scheme` with a manual override stored locally. |
| Browser support | Last two major versions of Chrome, Safari, Firefox, Edge; graceful degradation without JS for reading content. |

## 12. Design acceptance criteria

A release is design-complete when:

1. Every page template in Section 5 exists at phone, tablet and desktop widths in light and dark.
2. All components in Section 4 have documented states (default, hover, focus, active, disabled, loading, empty, error).
3. Contrast check passes for every token pair in CI.
4. An automated accessibility scan reports no serious or critical issues, and manual keyboard and screen-reader walkthroughs of Home → Art → Entry, Search → Entry, Filter → Compare pass.
5. A rating chip, legality strip, review badge and safety block appear on at least one entry in each art, using real content.
6. A user who has never seen the app can, in testing, find a named technique and its legality under a chosen ruleset within 30 seconds on a phone (target to validate in usability testing with at least five participants per persona group).
7. No page requires horizontal page scroll; wide tables scroll within their container.

## 13. Open design questions

1. **Brand:** name, logo and art marks are placeholders. Who owns them?
2. **Illustration:** are commissioned technique illustrations in scope for v1, or are diagrams (ranges, ladders, stances) enough?
3. **Dark theme default** for the gym context, or follow the OS?
4. **Thai script display** — which entries have verified Thai spellings, and who verifies them?
5. **Rating visuals:** dots versus a numeric badge — to be settled in usability testing.
6. **Density toggle:** do power users need a compact table mode beyond the default?

## 14. Appendix — component inventory (checklist)

Top bar · art switcher · bottom tab bar · left rail · "On this page" rail · breadcrumb · search box and results · filter chip bar · filter bottom sheet · entry row/card · rating chips · legality strip · review badge · safety block · callouts · relationship rails · range ladder · position ladder · stance diagram · learning-path stepper · compare table · glossary tooltip · bookmark toggle · progress bar · coverage meter · empty state · error state · offline banner · cookie/consent (only if required by analytics) · print layout.
