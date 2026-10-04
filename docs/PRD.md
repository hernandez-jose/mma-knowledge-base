# Product Requirements Document — MMA Knowledge Base (web app)

| | |
|---|---|
| **Status** | Draft v1 — for review |
| **Date** | 2026-10-03 |
| **Companion** | [`DRD.md`](DRD.md) — Design Requirements Document |
| **Source content** | [`jujitsu.md`](../jujitsu.md) · [`boxing.md`](../boxing.md) · [`kickboxing.md`](../kickboxing.md) · [`muay-thai.md`](../muay-thai.md) · [`mma.md`](../mma.md) · [`drills/`](../drills/) · [`techniques/`](../techniques/) · [`mma-training-plan.md`](../mma-training-plan.md) |

---

## 1. Summary

A web application that holds the complete, organised knowledge of the four core combat arts of mixed martial arts — **Brazilian Jiu-Jitsu, Boxing, Kickboxing and Muay Thai** — plus an **MMA hub** that connects them. Each art is its own category, and each category is structured the way [`jujitsu.md`](../jujitsu.md) is: *every* position, movement, strike, defense, counter, rule and drill, each rated for **difficulty** and **how common** it is, with the same ratings meaning the same thing across arts.

The content already exists as Markdown (four atlases + a technique library + training plans). The app's job is to make that content **findable, comparable, learnable and trustworthy**, not to re-invent it.

## 2. Problem

- Martial-arts knowledge online is scattered across videos, forum posts and gym lore. Terminology differs between sources (Thai transliterations, boxing numbering systems, BJJ naming).
- "Complete" references rarely exist. Most lists give the exciting 20% of techniques and skip the unglamorous 80% — footwork, defense, grip fighting, checks, posture, rules.
- Nothing helps a learner decide **what to learn first**, nor compares *the same idea across arts* (a teep vs. a jab vs. a frame).
- Rulesets are rarely explained next to the technique they permit or ban, so people learn moves they cannot use in their sport.
- Existing atlases are long documents. A 100 KB Markdown file is a reference, not a tool.

## 3. Goals and non-goals

### Goals
1. **Completeness.** Every art covers its full taxonomy (Section 7), with a visible gap-analysis rather than a hidden one.
2. **One rating language.** Difficulty 1–5 and Commonness 1–5 mean the same in every art.
3. **Findability.** Any technique reachable in ≤ 3 interactions from the home page; search returns the right entry from a misspelling, a Thai/Portuguese/English alias or a gym nickname.
4. **Learnability.** Learning paths drawn from the syllabus tiers already in each atlas.
5. **Cross-art linking.** A user can pivot from a boxing slip to a Muay Thai slip-and-clinch, or from a double-leg to its sprawl, in one click.
6. **Honesty.** Ratings and statistics carry their provenance and uncertainty, as the atlases do today.
7. **Safe by default.** Safety and legality information appears on the page of the technique it applies to.

### Non-goals (v1)
- Not a video platform; no hosting of user-generated video.
- Not a social network, forum or marketplace.
- Not a replacement for a coach — the app says so where it matters (joint locks, chokes, strikes to the head).
- Not a fight-statistics or betting product.
- No stand-alone **Wrestling, Judo, Karate, Taekwondo, Sambo** categories in v1. Wrestling and judo takedowns are already covered *inside* the Jiu-Jitsu atlas (Parts III–IV) and surface in the MMA hub. These arts are the first candidates for v2 (Section 14).

## 4. Users and jobs-to-be-done

| Persona | Needs | Key jobs |
|---|---|---|
| **New student** (0–6 months) | What do I learn first? What does this word mean? Is this safe? | Follow a path; look up a term; check a rule |
| **Active amateur** (1–5 years) | Fill gaps, prepare for a ruleset, learn counters | Find counters to a technique; compare rulesets; track what they know |
| **Competitor / MMA hobbyist** | Cross-art game planning; rules for the exact event | Compare rulesets; see transitions between arts; build a game plan |
| **Coach / instructor** | Build a class or a camp from the syllabus; share a list with students | Assemble a lesson plan; export/print; share links |
| **Curious fan** | Understand what they're watching | Identify a technique by name; see what it counters |
| **Contributor / editor** (internal) | Keep the content correct | Edit, review, cite, version |

## 5. Scope: the content model

### 5.1 Hierarchy

```
App
└── Art (category)            Jiu-Jitsu · Boxing · Kickboxing · Muay Thai · MMA hub
    └── Part                  Map · Stances · Footwork · Strikes · Defense · Counters · Rules · Syllabus · Gaps
        └── Section           e.g. "Dominant top positions", "Jab variations"
            └── Entry         one technique / position / concept / rule
```

### 5.2 Entry schema (every entry, every art)

| Field | Type | Notes |
|---|---|---|
| `id` | slug | stable, e.g. `boxing/jab`, `muay-thai/teep-rear` |
| `art` | enum | `jiu-jitsu`, `boxing`, `kickboxing`, `muay-thai`, `mma` |
| `part`, `section` | string | as in the atlas |
| `name` | string | canonical English name |
| `aliases[]` | strings | Thai, Portuguese, Japanese, gym nicknames, misspellings (e.g. *sok tat*, *sok tad*) |
| `description` | markdown | one to three sentences |
| `difficulty` | 1–5 \| null | shared scale; `null` for illegal/omitted items |
| `commonness` | 1–5 \| null | shared scale |
| `kind` | enum | position, movement, strike, kick, knee, elbow, defense, counter, hold, takedown, sweep, escape, submission, concept, drill, rule |
| `range` | enum | long, mid, short, clinch, ground (striking arts) |
| `limb`/`target` | enums | for strikes: weapon (fist, shin, knee, elbow, foot) and target (head, body, leg…) |
| `stance` | enum | orthodox / southpaw / both / n/a |
| `legality` | map | per ruleset: legal / banned / restricted (e.g. elbows: banned in K-1, legal in Muay Thai) |
| `gi_marker` | enum | Jiu-Jitsu only: gi-only / no-gi-better / both |
| `counters[]` | entry ids | what defeats it |
| `counter_to[]` | entry ids | what it defeats |
| `chains_from[]`, `chains_to[]` | entry ids | setups and follow-ups |
| `cross_art[]` | entry ids | analogues in other arts (see 5.4) |
| `body_adjustments` | markdown | the technique-library "adjust for their body / yours" content (where it exists) |
| `safety` | markdown | injury risk, tap rules, legality notes |
| `media[]` | links | licensed images or external references; never hot-linked without a licence check |
| `sources[]` | citations | URLs / references |
| `status` | enum | draft, reviewed, verified |
| `last_reviewed` | date | and reviewer |

### 5.3 Shared taxonomies

- **Rating scales** (verbatim from [`jujitsu.md`](../jujitsu.md)): Diff 1 *Day one* → 5 *Expert*; Com 5 *Everywhere* → 1 *Rare*. Displayed with the label, never as a bare number.
- **Rulesets:** IBJJF, ADCC, Submission-only/no-gi, Boxing (amateur, professional), K-1/GLORY, Full-contact, Low-kick, Point-fighting, Muay Thai (Thai stadium, amateur/IFMA-style, Western pro), MMA (Unified Rules).
- **Ranges:** long · mid · short · clinch · ground.

### 5.4 Cross-art equivalence (the "Rosetta" layer)

A curated mapping that lets a user pivot between arts. Examples (to be completed by editors; each link carries a one-line *how they differ* note):

| Concept | Jiu-Jitsu | Boxing | Kickboxing | Muay Thai |
|---|---|---|---|---|
| Distance tool | Grip fighting | Jab | Teep / oblique | Teep |
| Space-making | Frame + shrimp | Pivot / clinch | Teep / push-off | Teep / frame in the clinch |
| Defend a long weapon | Posture, guard | Slip / parry | Shin check | Shin check / catch |
| Upper-body control | Collar tie / underhook | Tie-up | Single/double collar | Double collar tie (plum) |
| Off-balance | Sweep | Pivot / shove | Catch and sweep | Sweep / dump |
| Finishing a hurt opponent | Choke | Flurry | Combination | Elbow / knee |

### 5.5 The MMA hub

Not a fifth atlas. It contains: the **ladder of ranges** across arts (striking → clinch → takedown → ground); **ruleset comparison** (Unified Rules vs. each art's rules); **transitions** (strike → clinch → takedown → ground, sprawl-and-brawl, cage wrestling); **learning paths** that mix arts, drawn from [`mma-training-plan.md`](../mma-training-plan.md) and [`training-plan.md`](../training-plan.md); and the **gap-analysis** summaries.

### 5.6 Combos and drills (user- and editor-authored content)

Two new content types sit beside entries. Both reference entries by id; neither duplicates technique content.

**Combo** — an ordered list of steps.

| Field | Type | Notes |
|---|---|---|
| `id`, `art`, `title`, `author`, `visibility` | | `visibility`: private / shared-by-link / public (public requires review) |
| `notation` | string | Canonical hyphenated form, e.g. `1-1-2-SK` ([notation spec](../drills/README.md)) |
| `steps[]` | list | Each: `entry_id`, optional `side` (lead/rear), `target` (head/body/leg), `modifier` (feint, switch, pivot), `note` |
| `ruleset` | enum | The ruleset the combo was validated against |
| `stance` | enum | orthodox / southpaw / both |
| `purpose` | text | Why it works, one or two sentences |
| `counters[]` | combo or entry ids | Optional: what it beats / what beats it |
| `difficulty` | 1–5 | **Computed** (highest step rating + length factor) |
| `commonness` | 1–5 | **Computed** (lowest step rating) |
| `warnings[]` | list | Output of the validator (see 8.9) |
| `status` | enum | draft / reviewed / verified |

**Drill** — a structured partner or solo exercise.

| Field | Type | Notes |
|---|---|---|
| `id`, `art`, `title`, `author`, `visibility`, `status` | | as above |
| `roles[]` | list | feeder, attacker, defender, holder, flow partner, coach, timer |
| `start` | text | Starting range or position (entry id for positions) |
| `rules` | text | What each role may do |
| `intensity` | 1–5 | The shared ladder (README §3.1); **required** |
| `timing` | object | rounds, work seconds, rest seconds, or reps |
| `goal` | text | The one skill trained |
| `combos[]` | combo ids | Combinations the drill uses |
| `progressions[]` | list | Three levels of "make it harder" |
| `safety` | text | **Required**; may be pre-filled from the entries used |
| `equipment[]` | list | Gloves, shin guards, pads, kick shield, mat… |
| `type` | enum | solo / pad-feed / partner-reactive / flow / positional / sparring-theme |

**Seeded library:** the combos and drills in [`drills/`](../drills/) are imported as `verified-seed` content and are the first thing users see.

## 6. Current content inventory (honest status)

Measured from the repository on the date above (rated table rows with both a Diff and a Com value; the atlases also contain unrated reference text, rules and syllabus sections).

| Art | File | Size | Parts | Rated entries | Depth beyond the atlas |
|---|---|---|:-:|:-:|---|
| Jiu-Jitsu | `jujitsu.md` | ~103 KB | 15 | ~276 | 29 deep-dive pages in `techniques/` (22 submissions, 7 escape systems) |
| Boxing | `boxing.md` | ~35 KB | 15 | ~226 | none yet |
| Kickboxing | `kickboxing.md` | ~25 KB | 15 | ~128 | none yet |
| Muay Thai | `muay-thai.md` | ~31 KB | 18 | ~161 | none yet |
| Combos & drills | `drills/` (6 files) | ~48 KB | — | 123 combos/chains and 65 partner drills (seed) | feeds F-31 |
| MMA hub | `mma.md` | ~8 KB | 6 | cross-art tables (not rated) | links into the four atlases |

**What this means:** Jiu-Jitsu is the benchmark. The three striking atlases are *complete as a first-pass taxonomy* but are **not yet at Jiu-Jitsu depth** — they lack the per-technique deep-dive pages (mechanism, standard application, body-size adjustments, failure modes, chains, defense, drills, safety). Producing those pages (≈ 40–60 for boxing, ≈ 40–50 each for kickboxing and Muay Thai) is a v1 content workstream, tracked in Section 12. The atlas rows also have no per-row image/source column yet; the Jiu-Jitsu file has one.

## 7. Content completeness requirements

An art is **"complete" for v1** only when every row of its checklist below exists as an entry, has ratings, has a legality note where rules differ, and has been reviewed by a qualified practitioner of that art. The checklist is the acceptance test.

### 7.1 Jiu-Jitsu (checklist mirrors `jujitsu.md`)
Positions (top, guards, standing/clinch) · takedowns (wrestling, judo, BJJ entries) · takedown defense · guard passing · sweeps · escapes (positional and submission defense) · submissions (chokes, arm/shoulder locks, leg locks, spinal locks/cranks) · transitions and back takes · concepts · solo movements · rulesets and belt legality · syllabus tiers · class templates · gap analysis · technique-library pages.

### 7.2 Boxing
Stances and guards · footwork (movement and angles) · all punches (six fundamentals; jab, cross, hook, uppercut and overhand variations; specialty punches; body punching) · combinations · defense (distance, blocks, parries, slips/rolls/pulls, clinch) · counters per punch · offense systems (feints, pressure, out-boxing, traps) · inside fighting · styles · solo drills and equipment · amateur/pro rules, weight classes, scoring, fouls · syllabus · gap analysis.

### 7.3 Kickboxing
Stances and guards · footwork · hand strikes (shared with boxing) · all kicks (push, round, specialty) · knees (and the elbow rule) · combinations · defense (punch, kick, clinch) · counters · clinch under restricted rules · styles · rulesets (point-fighting, full-contact, low-kick, K-1/GLORY) · syllabus · gap analysis.

### 7.4 Muay Thai
Stance/posture · ritual and culture (wai kru, ram muay, mongkol, pra jiad, music) · footwork · punches · teeps · kicks · knees · elbows · the clinch (holds, strikes, sweeps/dumps, defense) · defense · counters · styles (femur, tae, khao, sok, plam…) and **Thai scoring culture** · training tools · rulesets · syllabus · gap analysis.

### 7.5a Combos and drills completeness
Each art needs, at launch, at least: **15 offensive combinations** spread across Diff 1–4 · **8 counter-combinations** · **10 partner drills** covering distance, defense, offense and one low-intensity application round · a **solo practice** block · a safety note per drill. Jiu-Jitsu uses attack and escape **chains** in place of tokenised combos (15 chains plus 4 escape chains in the seed). The seeded library in `drills/` meets this for all four arts and the MMA hub; counts are a floor, not a ceiling.

### 7.5 Content quality rules (carried over from the technique library)
- Second person; **they/them** for opponents and unnamed people.
- No invented statistics. A number is either already sourced, or hedged as a rule of thumb.
- Every rating on a technique page matches its atlas row; disagreement is stated on the page, never silently changed.
- Safety and legality notes are mandatory on any technique that can injure the practitioner or partner (joint locks, chokes, head strikes, slams, kicks to the legs, elbows).
- Names follow common usage; **alternate spellings are indexed as aliases**.

## 8. Functional requirements

Priority: **P0** = launch blocker · **P1** = launch target · **P2** = after launch.

### 8.1 Browse and navigate
| ID | Requirement | Pri |
|---|---|:-:|
| F-1 | Home page presents the four arts as categories plus the MMA hub, each with a short description and entry count. | P0 |
| F-2 | Each art has a landing page listing its Parts in the same order as the atlas, with section anchors. | P0 |
| F-3 | Section pages render entries as sortable tables with Diff and Com shown as labelled chips, matching the atlas look. | P0 |
| F-4 | Breadcrumbs and a persistent art switcher on every page. | P0 |
| F-5 | Entry detail page (Section 8.5). | P0 |
| F-6 | "Related" rails: counters, chains, cross-art equivalents. | P1 |

### 8.2 Search and filter
| ID | Requirement | Pri |
|---|---|:-:|
| F-7 | Global search across all arts with instant results; matches name, aliases, description. | P0 |
| F-8 | Alias-aware and typo-tolerant (e.g. "sok tat", "sok tad", "tae tad"). | P0 |
| F-9 | Filters: art, kind, Diff range, Com range, range (long/mid/short/clinch/ground), weapon, target, stance, legality for a chosen ruleset, gi/no-gi. | P0 |
| F-10 | "What counters this?" and "What does this counter?" one-click views. | P1 |
| F-11 | Saved filter sets via URL (shareable). | P1 |

### 8.3 Learn
| ID | Requirement | Pri |
|---|---|:-:|
| F-12 | **Learning paths** generated from each atlas's syllabus tiers (Tier 0 → Tier N), including cross-art MMA paths from the training plans. | P0 |
| F-13 | "Start here" for beginners per art: Diff ≤ 2 and Com ≥ 4 entries in syllabus order. | P0 |
| F-14 | Glossary of terms with pronunciation for Thai, Portuguese and Japanese words (text; audio P2). | P1 |
| F-15 | Beginner safety primer (tapping, sparring ladder, concussion awareness, hand wrapping) shown before first access to any contact-heavy path. | P0 |
| F-16 | Concept pages (e.g. posture, frames, distance, the check) linked from every entry that depends on them. | P1 |

### 8.4 Compare
| ID | Requirement | Pri |
|---|---|:-:|
| F-17 | **Ruleset matrix**: pick two or more rulesets, see which techniques are legal/banned/restricted. | P0 |
| F-18 | **Cross-art compare**: side-by-side view of equivalent concepts (Section 5.4). | P1 |
| F-19 | **Style archetypes** (e.g. Muay femur vs. K-1 volume vs. out-boxer) with typical tools and counters. | P2 |

### 8.5 Entry detail page
Required blocks, in order: name + aliases · ratings with labels · one-line description · kind/range/weapon/target/stance badges · **legality by ruleset** · how it works (mechanism/steps; full template where a deep-dive exists) · **adjust for their body / your body** (where it exists) · common mistakes (failure modes) · setups and chains · **defenses and counters** · drills · **safety** · related entries · sources and review status.
Where only an atlas row exists (no deep-dive yet), the page shows the row's content and a visible *"Deep dive coming"* state — never an empty template. | **P0**

### 8.6 Personalisation (account optional)
| ID | Requirement | Pri |
|---|---|:-:|
| F-20 | Anonymous browsing is fully functional; no account needed to read. | P0 |
| F-21 | Bookmarks and "known / learning / want to learn" status per entry, stored locally and (with an account) synced. | P1 |
| F-22 | Progress per learning path. | P1 |
| F-23 | Notes per entry (private). | P2 |
| F-24 | Personal "game plan" board: collect entries into chains and print/export. | P2 |

### 8.7 Coach tools
| ID | Requirement | Pri |
|---|---|:-:|
| F-25 | Build a class plan from entries using the 60/90-minute templates already in the atlases; export to PDF and share by link. | P2 |
| F-26 | Printable one-page cheat sheets per section (table view, print stylesheet). | P1 |

### 8.8 Content operations (internal)
| ID | Requirement | Pri |
|---|---|:-:|
| F-27 | **Markdown is the source of truth.** A build step parses the atlas tables and technique pages into the entry schema; the app never diverges from the repository content. | P0 |
| F-28 | Validation in CI: every rated row has Diff and Com in 1–5 (or an explicit "—" for illegal/omitted items); every link resolves; every entry id is unique; ratings on deep-dive pages match their atlas row. | P0 |
| F-29 | Editorial states (draft → reviewed → verified) shown on the entry; unreviewed content labelled. | P0 |
| F-30 | Change log per entry; "Report an error" link on every page. | P1 |

### 8.9 Combo and Drill Builder, library and runner

Learning an art means repeating sequences with a partner. This feature lets anyone **build, validate, save, share and run** combinations and partner drills in every art, starting from the seeded library.

**Example the builder must handle:** typing `1-1-2-SK` (or tapping jab, jab, cross, switch kick) produces a four-step combo with plain-English text, a computed difficulty, a legality result per ruleset (illegal in boxing because it contains a kick; legal in K-1, Muay Thai and MMA), and suggested counters and drills.

| ID | Requirement | Pri |
|---|---|:-:|
| F-31 | **Seeded library**: all combos and partner drills in `drills/` browsable per art, filterable by Diff, intensity, ruleset, type (solo / pad / partner / flow / positional) and role. | P0 |
| F-32 | **Combo builder**: assemble steps by tapping entries (grouped by kind and filtered to the chosen ruleset and range) *or* by typing notation; both views stay in sync. Supports drag-to-reorder, duplicate, delete and per-step modifiers (side, target, feint, switch, pivot). | P0 |
| F-33 | **Notation parser**: parses and prints the canonical notation (`drills/README.md` §1), tolerates spaces, case and common aliases (`jab`, `cross`, `hook`), rejects unknown tokens with a suggestion. | P0 |
| F-34 | **Validator**: runs the ten rules in `drills/README.md` §2 live — *errors* (illegal in ruleset; broken range continuity) block publishing, *warnings* and *suggestions* are shown inline with a one-line fix. | P0 |
| F-35 | **Computed difficulty and commonness** from the entries used; never hand-entered. | P0 |
| F-36 | **Plain-English renderer**: every combo shows its notation, a sentence, and a step list with entry links. | P0 |
| F-37 | **Drill builder**: choose a template (pad-feed, partner-reactive, flow, positional, sparring-theme), fill in roles, start, rules, timing, intensity, goal, progressions. Intensity and safety are required; safety is pre-filled from the entries used and can be extended, not removed. | P0 |
| F-38 | **Intensity guard**: head-contact, joint-lock, choke and elbow/knee drills default to intensity ≤ 2; selecting a higher level on those shows a confirmation naming the risk. Level 5 is only available for the *sparring-theme* type. | P0 |
| F-39 | **Drill runner**: round timer with work/rest intervals, role swap prompts, current-step display, audio/haptic cues, wake-lock so the screen stays on; works offline. | P1 |
| F-40 | **Combo practice mode**: shows one step at a time (or the whole sequence), optional call-out of the next step on a timer ("pad caller"), repeats N times, then adds a step ("relay"). | P1 |
| F-41 | **Combo generator**: choose art, ruleset, length, Diff ceiling and focus (punch-kick, counters, clinch…); app proposes combos that pass the validator. Deterministic with a seed, so a coach can share "Tuesday's rounds". | P1 |
| F-42 | **Counter and drill suggestions**: for any combo, list the defenses and counters from the atlas and drills that train them. | P1 |
| F-43 | **Save, tag and organise**: private by default; collections ("Tuesday kickboxing class"); duplicate-and-edit of seeded items. | P1 |
| F-44 | **Share**: stable link containing the notation (`/combo?n=1-1-2-SK&rs=k1`); no account needed to open. | P1 |
| F-45 | **Class plan assembly**: combine combos and drills into a 60/90-minute plan using the atlas class templates; print or export to PDF. (Extends F-25.) | P2 |
| F-46 | **Public contributions**: users may submit combos and drills for the public library; submissions go to an editorial queue and must pass the validator, carry a safety block, and be reviewed before appearing. | P2 |
| F-47 | **Coach assignments**: a coach shares a drill set with a group by link; members mark done. | P2 |
| F-48 | **Mixed-art combos** (e.g. strike → level change → takedown) using the grappling tokens, validated against the MMA ruleset. | P2 |

**Validator rules (summary; full text in `drills/README.md` §2).** (1) Legal in the chosen ruleset. (2) Range continuity between steps. (3) No same-limb repeats without a reset unless it is a marked double. (4) Level/side/speed variety suggestion. (5) Safe ending. (6) Length vs the user's level. (7) Finisher without setup. (8) Defense/reset present in drills. (9) Stance consistency after switches. (10) Difficulty computed.

**Moderation and safety.** User content is data, never rendered as HTML. Public items need practitioner review. Joint-lock, choke, head-strike, elbow and slam content cannot be published without a safety block and an intensity ≤ 3. The app does not offer "full power" partner drills; hard rounds exist only as *sparring themes* with the supervision notice.

**Acceptance tests (minimum).**
1. `1-1-2-SK` parses to jab, jab, cross, switch kick; Diff is computed from the entries; it is *illegal* under boxing and *legal* under K-1.
2. `1-2-Eh` is rejected under K-1 and accepted under Muay Thai.
3. A combo with a head kick and no setup in the previous two steps produces a warning, not an error.
4. A drill with a choke and intensity 4 prompts the intensity guard.
5. A shared link renders the same combo and validation result for a signed-out visitor.
6. The seeded combos in `drills/` all validate with zero errors.

## 9. Non-functional requirements

| Area | Requirement |
|---|---|
| **Performance** | Entry and section pages: LCP ≤ 2.5 s on a mid-range phone on 4G; search results within 150 ms after the index loads; initial JS ≤ 200 KB gzipped for content pages. |
| **Offline** | Installable PWA; the full text content is cacheable (it's text, tens of MB at most) so it works in a gym with poor signal. (P1) |
| **Accessibility** | WCAG 2.2 AA; full keyboard operation; tables have proper headers and responsive reflow; meaning never conveyed by colour alone (ratings have labels and numerals). |
| **Responsive** | Phone-first. The primary use-case is a phone propped up in a gym. |
| **Internationalisation** | UI strings externalised from day one. Content is English in v1; Thai, Portuguese and Japanese terms appear as aliases with native script where verified. Spanish/Portuguese UI translation is a P2 candidate. |
| **SEO** | Static, server-rendered entry pages with stable URLs and structured data; canonical alias handling. |
| **Privacy** | No tracking beyond privacy-respecting aggregate analytics; no sale of data; account data minimal (email or social sign-in). |
| **Security** | Standard web hardening; contributor tooling behind authentication; no user-generated HTML. |
| **Availability** | Static hosting on a CDN; target 99.9%. |
| **Maintainability** | The content pipeline is a plain parser over Markdown; adding an art means adding one file that follows the template. |

## 10. Technical approach (recommendation, not a mandate)

- **Static-first site generator** (e.g. Astro or Next.js static export) that compiles the Markdown into JSON entry records at build time.
- **Client-side search index** (e.g. Pagefind or MiniSearch) built from entries and aliases — no search server in v1.
- **Local-first user state** (IndexedDB) with optional account sync later.
- **Content parser** reads atlas tables (`| **Name** | description | Diff | Com |`) into entries; deep-dive pages are parsed against [`techniques/_TEMPLATE.md`](../techniques/_TEMPLATE.md) headings.
- **Known parser risks:** two table shapes exist (Jiu-Jitsu has an *Image / source* column and gi markers; the striking atlases do not yet), some rows use "—" for unrated/illegal items, and some atlas tables have different column sets (rulesets, styles). The parser must be driven by table headers, not column positions, and fail CI on unknown shapes.

## 11. Success metrics

| Metric | Target (6 months after launch) |
|---|---|
| Search success (a result clicked within a session) | ≥ 80% |
| Entry pages with **verified** status | ≥ 60% of rated entries; 100% of safety-critical ones |
| Learning-path starts that reach Tier 2 | ≥ 25% |
| Median time from landing to first entry viewed | ≤ 15 s |
| Returning users (30-day) | ≥ 30% |
| Reported-error resolution time | median ≤ 7 days |
| Accessibility audit | zero WCAG AA blockers |

(Targets are proposals to be agreed with stakeholders, not forecasts.)

## 12. Content workstreams and milestones

| Milestone | Scope | Exit criteria |
|---|---|---|
| **M0 — Content baseline** *(done in this repo)* | Four atlases, MMA hub, technique library for Jiu-Jitsu, training plans, PRD, DRD | Files exist; internal review done (Section 15) |
| **M1 — Parser + CI** | Markdown → JSON pipeline; validation rules F-28 | All four atlases parse with zero errors |
| **M2 — Core app** | F-1…F-9, F-12, F-13, F-15, F-17, F-20, F-27…F-29 | Usable on phone; search and filters work; ruleset matrix live |
| **M2b — Builder** | F-31…F-38 (library, builder, parser, validator, drill builder, intensity guard) | Acceptance tests 1–6 in §8.9 pass |
| **M3 — Striking deep dives (wave 1)** | Boxing: jab, cross, hooks, uppercuts, slip/roll/parry, pivot, check hook, body shots · Kickboxing: low kick, calf kick, body kick, high kick, teep, oblique, shin check, catch · Muay Thai: teep, round kick, check, catch, straight knee, curving knee, elbows ×4, plum, sweeps | Pages follow the template (adapted: mechanism, standard application, adjust for their/your body, failure modes, chains, defense, drills, safety) |
| **M4 — Practitioner review** | Each atlas reviewed by a qualified coach/competitor of that art | Entries move to **verified**; disputed items resolved or annotated |
| **M5 — Launch (P0 + most P1)** | + F-6, F-10, F-11, F-14, F-16, F-18, F-21, F-22, F-26, F-30 | Metrics baselines recorded |
| **M6 — Striking deep dives (wave 2)** | Remaining entries rated Com ≥ 3 | Coverage target met |
| **v2 candidates** | Wrestling, Judo, Karate/Taekwondo (for kickboxing), Sambo; audio pronunciation; Spanish/Portuguese UI; coach tools | Separate PRD |

## 13. Risks and mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|:-:|:-:|---|
| **Content errors** (a wrong technique, rule or term) | High | High | Practitioner review (M4); statuses shown on each entry; "Report an error"; honest provenance notes |
| **Rules change** (IBJJF, commissions, promotions) | High | Med | Every ruleset block carries "as of" date and a source link; quarterly rules review |
| **Injury liability** (readers attempt dangerous techniques unsupervised) | Med | High | Safety blocks mandatory; "train with a qualified coach" notices; no "how to hurt someone" framing; terms of use |
| **Copyright of images and video** | Med | High | Only licensed/own media; link out rather than embed; image rights field per media item |
| **Thai/Portuguese/Japanese transliteration disputes** | High | Low | Alias system; show variants; never claim a single "correct" spelling |
| **Inconsistent depth across arts** | High (today) | Med | Visible coverage meters; deep-dive waves; "Deep dive coming" state |
| **Ratings seen as authoritative** | Med | Med | Keep the atlas honesty note visible; label as editorial judgment |
| **Scope creep into video/social** | Med | Med | Non-goals fixed; separate PRDs |
| **Unsafe or wrong user-made drills** | Med | High | Validator errors, intensity guard, mandatory safety block, review before public, private by default |
| **Parser fragility** | Med | Med | Header-driven parsing; CI fails on unknown table shapes |

## 14. Open questions

1. Which ruleset set is authoritative for each art in v1, and who owns "as of" updates?
2. Is **Wrestling** (and Judo) a launch category given MMA's reliance on them, or a fast-follow? *(Recommendation: fast-follow; the Jiu-Jitsu atlas already carries takedowns and defense.)*
3. Will the app hold any **media** (diagrams, photos), and if so, who commissions or licenses it? (The repository already has 3D figure renders in `assets/figures3d/`; confirm their rights before reuse.)
4. Accounts: is sync worth the privacy and maintenance cost in v1, or ship local-only?
5. Who are the named practitioner reviewers for each art?
6. Monetisation, if any (free / donation / premium paths)? Not addressed in this PRD.
7. Where do **women's and kids'** variants of rules and training live — inside each art's rules part, or as separate paths?

## 15. Review notes on the source content (what the internal review found)

An editorial review of the new atlases against their sources and against `jujitsu.md` produced these findings. Items marked ✔ were fixed in the repository; the rest are tracked for M4.

| # | Finding | Status |
|---|---|:-:|
| 1 | Unverifiable fighter attributions (e.g. specific boxers credited with named punches) were removed from the boxing atlas. | ✔ |
| 2 | A non-technique ("backfist is illegal") sat inside the boxing punch table and was removed. | ✔ |
| 2b | Two invented-sounding guards ("walk-down", "over-the-top") and a vague "cut kick" entry were removed. | ✔ |
| 3 | A speculative "muay tee" style label was replaced with a plain "counter fighter" label. | ✔ |
| 4 | Thai elbow and knee names conflict between sources (e.g. *sok ngat* vs *sok hud*, *sok sab* vs *sok ti*). The atlas now flags this and lists variants. Needs a Thai-speaking kru to settle. | partial |
| 5 | Ruleset details (round counts, glove ounces, knockdown rules, clinch limits, Thai stadium scoring emphasis) were summarised from secondary sources and vary by promotion. Marked "verify per promotion"; need primary-source check against current rulebooks. | open |
| 6 | The striking atlases lack the per-row *Image / source* column that `jujitsu.md` has. | open |
| 7 | The striking atlases lack deep-dive pages (Section 6). | open |
| 8 | Wikipedia pages were not reachable from the authoring environment, so several references rely on secondary articles and general knowledge rather than primary rulebooks. | open |
| 9 | Ratings are editorial judgment; there is no public frequency dataset for striking arts comparable to the BJJ competition data cited in `jujitsu.md`. | accepted |

## 16. Out of scope / future

Video library, live classes, gym finder, fight-card tracking, AI coach, wearable integration, marketplace. Any of these needs its own PRD.

## 17. Glossary

**Atlas** — a full-reference Markdown file for one art. **Entry** — one technique, position, concept or rule. **Deep dive** — a template-driven page about one technique. **Diff / Com** — the shared difficulty and commonness ratings. **Ruleset** — a specific set of competition rules that decides what is legal.
