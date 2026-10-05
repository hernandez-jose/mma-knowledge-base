# Project State — read this first

> **Purpose:** a handoff file so any new Claude Code session (or person) can pick up exactly where the last one stopped, without re-deriving anything. **Update it at the end of every working session** (rules in §9).
>
> **Last updated:** 2026-10-05 · **Updated by:** Claude Code session `01QM7YG29NcuAtLaWXj4WwQe`

---

## 1. What this project is

A knowledge base for **Brazilian Jiu-Jitsu, Boxing, Kickboxing and Muay Thai** (plus an MMA hub), written as Markdown "atlases", and the specs for a **web app** that will present it. One category per art; every entry rated for difficulty (1–5) and how common (1–5). The Jiu-Jitsu atlas (`jujitsu.md`) is the benchmark the other arts are built to match.

Repo: `hernandez-jose/mma-knowledge-base`. User: Jose (they/them where unstated).

## 2. Where things stand (one-paragraph summary)

Four atlases, an MMA hub, a PRD, a DRD and a combos/partner-drills library exist. **The web app itself is not built** — there is no code, only specs. Jiu-Jitsu has the deepest content (atlas + 29 technique deep-dives). The three striking atlases are a complete *first-pass taxonomy* but **have no deep-dive pages, no per-row source column, and no practitioner review**. All rulesets and Thai terminology are from secondary sources and need verifying.

## 3. Git state (check with `git branch -a` — this may be stale)

| Branch | Contents | On `main`? |
|---|---|:-:|
| `main` @ `0c7e3d0` | Everything: atlases, `mma.md`, PRD, DRD, `drills/`, builder spec, `PROJECT-STATE.md`, `CLAUDE.md` | ✔ |
| `claude/mma-knowledge-base-docs`, `claude/combos-and-drills` | fully merged into `main` (fast-forward); safe to delete | ✔ |

The user has approved pushing to `main` twice (2026-10-03, 2026-10-05), each time for that specific work only. **Ask before pushing to `main` again.** Never open a PR unless asked.

## 4. File map

| Path | What it is | Size | Status |
|---|---|---|---|
| `jujitsu.md` | BJJ atlas, 15 parts, ~276 rated rows, image/source column | ~104 KB | Pre-existing; benchmark |
| `boxing.md` | Boxing atlas, 15 parts, ~224 rated rows | ~36 KB | First pass; unreviewed |
| `kickboxing.md` | Kickboxing atlas (K-1/GLORY focus), 15 parts, ~128 rows | ~26 KB | First pass; **thinnest** |
| `muay-thai.md` | Muay Thai atlas, 18 parts, ~160 rows; ritual, clinch, scoring culture | ~32 KB | First pass; unreviewed |
| `mma.md` | MMA hub: ranges, cross-art equivalents, transitions, ruleset summary, mixed paths | ~8 KB | First pass |
| `docs/PRD.md` | Product requirements (F-1…F-48), content model, milestones, risks, review findings | ~34 KB | Draft v1 |
| `docs/DRD.md` | Design requirements: IA, tokens, components (4.1–4.13), templates, a11y | ~30 KB | Draft v1 |
| `docs/PROJECT-STATE.md` | **This file** | | Living |
| `drills/README.md` | Notation, build rules, intensity ladder, roles, safety | ~9 KB | merged |
| `drills/{boxing,kickboxing,muay-thai,jiu-jitsu,mma}.md` | 123 combos/chains, 65 partner drills | ~32 KB | merged |
| `techniques/` | 22 submission + 7 escape deep-dive pages, `_TEMPLATE.md` | — | Pre-existing; **BJJ only** |
| `mma-training-plan.md`, `training-plan.md` | 24-week MMA curriculum; 8-week plan | — | Pre-existing |
| `assets/figures3d/` | 3D figure renders of BJJ positions | — | Pre-existing; rights unconfirmed |

## 5. Conventions (do not break these)

- **Rating scales** are copied from `jujitsu.md`: Diff 1 *Day one* → 5 *Expert*; Com 5 *Everywhere* → 1 *Rare*. Illegal/omitted items use "—". Atlas honesty note stays: ratings are editorial judgment.
- **Atlas shape:** `# Title — The Complete Move Atlas` → how-to-read → Part I Map → … → Rules → Syllabus → Class templates → Gap analysis → Sources.
- **Voice:** second person; **they/them** for opponents and unnamed people; no invented statistics; no unverifiable fighter attributions.
- **Technique pages** follow `techniques/_TEMPLATE.md` (Mechanism → Standard application → Adjust for their body → Adjust for your body → Failure modes → Chains → Defending → Drills → Safety → Related). Ratings must match the atlas row.
- **Combo notation** (canonical in `drills/README.md`): hyphenated tokens — `1` jab, `2` cross, `3`/`4` lead/rear hook, `5`/`6` uppercuts, `T` teep, `RK/LK` low, `RB/LB` body, `RH/LH` head, `SK` switch kick, `KN` knee, `Eh/Eu/Ed` elbows, `sl pa ro bl ck ct pv sb` defenses. Example `1-1-2-SK`.
- **Intensity ladder:** 1 Air · 2 Touch · 3 Light · 4 Technical · 5 Hard (supervised sparring only).
- **Relative links only**; atlas files live at repo root, drills in `drills/`, specs in `docs/`.
- **Ruleset claims** carry "verify per promotion". Don't state a rule as fact without a source.
- **Wikipedia is blocked** from this environment (egress proxy). `WebSearch` works but returns summaries; prefer primary rulebooks when reachable.

## 6. Decisions already made (don't re-litigate)

1. Four arts + an MMA hub; **no standalone Wrestling/Judo/Karate/Sambo in v1** (takedowns live in the Jiu-Jitsu atlas); they are v2 candidates.
2. **Markdown is the source of truth**; the app compiles it at build time.
3. Kickboxing atlas is written for **K-1/GLORY rules**; other kickboxing rulesets are summarised in its Part XII.
4. Combo **difficulty/commonness are computed** from steps, never typed by users.
5. Safety block and intensity are **mandatory** on drills; user content is private by default; public items need review.
6. DRD art accent colours: BJJ `#1d4ed8`, Boxing `#b91c1c`, Kickboxing `#a16207`, Muay Thai `#047857` (dark variants in DRD §3.1); all pass AA contrast.
7. Static-first site + client-side search + local-first user state (recommendation, not mandate).

## 7. Known issues and open review findings

| # | Issue | Where | Status |
|:-:|---|---|---|
| 1 | Striking atlases lack the per-row *Image / source* column | boxing, kickboxing, muay-thai | Open |
| 2 | No deep-dive technique pages for striking arts | `techniques/` | Open (planned: PRD M3/M6) |
| 3 | Thai elbow/knee names conflict between sources (*sok ngat* vs *sok hud*, *sok sab* vs *sok ti*) | `muay-thai.md` Part IX | Flagged; needs a Thai-speaking kru |
| 4 | Ruleset details (rounds, gloves, knockdown rules, clinch limits, Thai scoring emphasis, Unified Rules) unverified against primary rulebooks | all striking atlases, `mma.md` | Open |
| 5 | Ratings are judgment; no public frequency data for striking arts | all | Accepted |
| 6 | Nothing has had practitioner review; **no entry is "verified"** | all | Open |
| 7 | Kickboxing atlas is the thinnest (~128 rated rows vs 224 boxing) | `kickboxing.md` | Open |
| 8 | `drills/` combos/drills are starting points; elbow, clinch and leg-lock drill safety notes especially need coach review | `drills/` | Open |
| 9 | Image rights for `assets/figures3d/` unconfirmed | `assets/` | Open (PRD Q3) |
| 10 | `drills/` not yet merged to `main` | git | **Fixed 2026-10-05** |

Already fixed (don't redo): removed invented fighter attributions and non-techniques from `boxing.md`; removed two invented guards and a vague "cut kick"; replaced "muay tee" with "counter fighter"; added kickboxing/Muay Thai counter combos to reach the PRD floor of 8.

## 8. Next steps (priority order)

1. ~~Merge drills branch~~ — done 2026-10-05.
2. Add the **Image/source column** and source links to the striking atlases (match `jujitsu.md`).
3. **Deep-dive pages, wave 1** (follow `_TEMPLATE.md`, adapted for strikes):
   - Boxing: jab, cross, lead hook, uppercut, slip, roll, parry, pivot, check hook, body shots → `techniques/boxing/`
   - Kickboxing: low kick, calf kick, body kick, head kick, teep, oblique, shin check, catch → `techniques/kickboxing/`
   - Muay Thai: teep, round kick, check, catch, straight knee, curving knee, 4 elbows, plum, sweeps → `techniques/muay-thai/`
   - Create `techniques/boxing/README.md` etc. and update `techniques/README.md` (currently says only submissions + escapes).
4. **Deepen kickboxing** (more kick variations, clinch, rules by organisation, Karate/Taekwondo-style entries).
5. **Primary-source rules pass**: IBJJF, ADCC, commission boxing rules, K-1/GLORY, Lumpinee/Rajadamnern, IFMA, Unified Rules — add "as of" dates and links.
6. **Practitioner review** per art; flip entry status to reviewed/verified.
7. **Parser + CI validation** (PRD F-27/F-28): Markdown → JSON; fail on bad ratings, broken links, duplicate ids, unknown table shapes; run the combo validator over `drills/` (acceptance tests in PRD §8.9).
8. **Build the app** (PRD M2, M2b) — static site, search, filters, ruleset matrix, Train area.
9. Standalone **Wrestling** (and Judo) atlas — v2.

## 9. How to update this file (rules for every session)

At the **end** of each session — or before the context gets long — do this:
1. Update **Last updated** and the git-state table (§3) from `git branch -a` and `git log --oneline -5 --all`.
2. Update the file map (§4) for anything added, renamed or merged.
3. Move finished items out of §8 into §10 (changelog) and add anything new you discovered to §7.
4. Record any **decision** the user made in §6 so it is never re-asked.
5. Commit this file with the work. Never leave it describing a state that no longer exists.

At the **start** of each session: read this file, run the checks in §11, then continue from §8.

## 10. Changelog

| Date | What happened |
|---|---|
| 2026-10-03 | Read `jujitsu.md` and the technique library; researched striking arts via web search; wrote `boxing.md`, `kickboxing.md`, `muay-thai.md`; wrote `docs/PRD.md` and `docs/DRD.md`; internal review (fixed invented entries, added honesty notes); created `mma.md`; pushed to branch, then fast-forwarded `main` at `043d965` on user request. |
| 2026-10-04 | Added `drills/` (notation, 123 combos/chains, 65 partner drills); added Combo/Drill Builder spec to PRD (§5.6, §7.5a, §8.9, M2b) and DRD (§4.13, §5.9); linked drills from each atlas; pushed branch `claude/combos-and-drills` (**not on main**). Created this file and `CLAUDE.md`. |
| 2026-10-05 | Merged `claude/combos-and-drills` into `main` (fast-forward, `0c7e3d0`) on user request; updated this file. |

## 11. Quick checks (run before committing)

```bash
# broken relative links in the new files
python3 - <<'PY'
import re,os
for f in ['boxing.md','kickboxing.md','muay-thai.md','mma.md','jujitsu.md','docs/PRD.md','docs/DRD.md','docs/PROJECT-STATE.md']+['drills/'+x for x in os.listdir('drills')]:
    if not os.path.exists(f): continue
    d=os.path.dirname(f)
    for m in re.findall(r'\]\((?!http|#)([^)#]+)',open(f).read()):
        if not os.path.exists(os.path.join(d,m)): print('BROKEN',f,m)
PY
# rated-row counts per atlas (should not shrink)
for f in jujitsu boxing kickboxing muay-thai; do echo $f $(grep -cE '^\|.*\| *[1-5] *\| *[1-5] *\|' $f.md); done
# combo and drill counts in the seed library (123 combos, 65 drills at last count)
grep -hcE '^\| (B|K|M|J|X)[0-9]+ ' drills/*.md; grep -hcE '^\| (BD|KD|MD|JD|XD)[0-9]+ ' drills/*.md
```
