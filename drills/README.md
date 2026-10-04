# Combos and Partner Drills Library

How to learn every art by *doing* it: written combinations, chains, and partner drills for **[Boxing](boxing.md)**, **[Kickboxing](kickboxing.md)**, **[Muay Thai](muay-thai.md)**, **[Jiu-Jitsu](jiu-jitsu.md)** and the **[MMA hub](mma.md)**. The same notation and rules also define what the web app's **Combo and Drill Builder** must accept (see [PRD §8.9](../docs/PRD.md) and [DRD §4.13](../docs/DRD.md)).

> Technique definitions and ratings stay in the atlases ([`boxing.md`](../boxing.md), [`kickboxing.md`](../kickboxing.md), [`muay-thai.md`](../muay-thai.md), [`jujitsu.md`](../jujitsu.md)). This library only *sequences* them.

---

## 1. Notation (one language for all striking arts)

A combination is a list of **tokens separated by hyphens**. Read left to right. Example: **`1-1-2-SK`** = jab, jab, cross, switch kick.

### 1.1 Hand strikes
| Token | Meaning | Token | Meaning |
|:-:|---|:-:|---|
| **1** | Jab | **4** | Rear hook |
| **2** | Cross | **5** | Lead uppercut |
| **3** | Lead hook | **6** | Rear uppercut |

Add **b** for the body: `3b` = lead hook to the body. Add **o** for overhand: `2o` = overhand right (orthodox).

### 1.2 Kicks
| Token | Meaning | Token | Meaning |
|:-:|---|:-:|---|
| **T** | Rear teep (push kick) | **LT** | Lead teep |
| **RK / LK** | Rear / lead **low** kick (thigh) | **CK** | Calf kick (either leg; say which if it matters) |
| **RB / LB** | Rear / lead **body** kick | **RH / LH** | Rear / lead **head** kick |
| **SK** | Switch kick (body by default); suffix **-L** low, **-H** head | **OB** | Oblique kick (lead leg, to the knee) |
| **SB** | Spinning back kick (body) | | |

> `kickboxing.md` Part VII uses the shorthand **BK** for "body kick" and **HK** for "head kick". They mean the same as `RB` / `RH` there; the library uses the side-specific forms above.

### 1.3 Knees and elbows (where the ruleset allows)
| Token | Meaning |
|:-:|---|
| **KN** | Straight knee (rear leg unless stated). `KN-c` curving knee, `KN-f` flying knee. |
| **Eh** | Horizontal elbow · **Eu** upward · **Ed** downward · **Ec** diagonal · **Es** spinning |

Prefix **L** or **R** for the limb if it matters (`LEh`).

### 1.4 Movement and defense (these count as steps)
| Token | Meaning | Token | Meaning |
|:-:|---|:-:|---|
| **sl** | Slip | **pa** | Parry |
| **ro** | Roll under | **bl** | Block / cover |
| **pu** | Pull back | **ck** | Shin check |
| **ct** | Catch (kick or teep) | **pv** | Pivot |
| **sb** | Step back | **sd** | Side step |
| **fi** | Feint (say what: `fi-1`) | **sw** | Sweep (clinch) |
| **PL** | Plum / double collar tie | **cl** | Clinch / tie-up |

### 1.4b Grappling tokens (used in the MMA and Jiu-Jitsu files)
`lvl` level change · `dl` double leg · `sl-leg` single leg · `spr` sprawl · `fhl` front headlock · `gil` guillotine · `gnp` ground strike (controlled) · `gu` get-up. Jiu-Jitsu chains use technique names and arrows instead of tokens because the move vocabulary is far larger.

### 1.4a Reading counters
Use a plus or an arrow between the *attack* and the *answer*: `RB > ct-sw` = "against a rear body kick: catch and sweep".

### 1.5 Examples
| Notation | In words |
|---|---|
| `1-2` | jab, cross |
| `1-1-2-SK` | double jab, cross, switch kick to the body |
| `1-2-3-RK` | jab, cross, lead hook, rear low kick |
| `T-2-RB` | rear teep, cross, rear body kick |
| `PL-KN-KN-sw` | plum, two knees, sweep |
| `sl-2-3` | slip, then cross–lead hook (a counter) |

## 2. Rules for building a good combination

These are coaching rules, and the web app turns each one into a **check** (an error or a warning) in the builder.

| # | Rule | App behaviour |
|:-:|---|---|
| 1 | **Legal in your ruleset.** An elbow in a K-1 combo, a kick in a boxing combo, a knee to a downed opponent are all illegal. | **Error** |
| 2 | **Range continuity.** Each step ends at a range the next step can start from (e.g. a long teep → a mid-range jab is fine; a long teep → a clinch knee needs an entry). | **Error** if no entry step, **warning** otherwise |
| 3 | **Same-limb recovery.** The same limb is not thrown twice in a row unless it is a deliberate double (`1-1`). Kicks off the same leg need a reset or a different level. | **Warning** |
| 4 | **Change level, side or speed.** Three punches at one level in a straight line are easy to read. | **Suggestion** |
| 5 | **End safe.** Finish in guard, on both feet, or with an exit step; a combination that ends on one leg or with the chin up is flagged. | **Warning** |
| 6 | **Beginner length.** Up to 4 steps for beginners, 6 for intermediate, no limit for advanced (flows excepted). | **Warning** above the user's level |
| 7 | **Setup before the finisher.** A head kick or flying knee with no setup in the previous two steps is flagged. | **Warning** |
| 8 | **Include defense.** Every drill (not every combo) has a defensive response or a reset. | **Suggestion** |
| 9 | **Stance consistency.** After a switch step or switch kick, later steps use the new lead/rear. | **Warning** if ambiguous |
| 10 | **Difficulty is computed**, not typed: *highest-rated step + length factor*; commonness is the *lowest* step rating. | **Automatic** |

## 3. Partner drill anatomy

Every drill in this library has the same fields so it can be sorted, filtered and run on a timer.

| Field | Meaning |
|---|---|
| **Name** | Short and memorable |
| **Art / tier** | Where it sits in the syllabus |
| **Roles** | Who does what: *feeder / attacker / defender / holder / flow partner* |
| **Start** | The starting distance or position |
| **Rules** | What each partner may do, and the speed |
| **Intensity** | The 1–5 ladder below |
| **Timing** | Round length, rest, number of rounds or reps |
| **Goal** | The one skill the drill trains |
| **Progression** | How to make it harder in three steps |
| **Safety** | The thing most likely to hurt someone, and the guard against it |

### 3.1 Intensity ladder (the same in every art)

| Level | Name | What it means (rule of thumb) |
|:-:|---|---|
| **1** | Air / walk-through | No contact. Slow enough to read every detail. |
| **2** | Touch | Contact with a light touch — "tap, don't hit" (roughly a fifth of power). |
| **3** | Light | Controlled power (roughly a third to a half). Feeder can be hit and keep working. |
| **4** | Technical | Realistic speed with control; stops on a clean hit or a good defense. |
| **5** | Hard | Full-effort rounds. Not for drills — only supervised sparring and competition prep. |

Percentages are coaching rules of thumb, not measurements. **Never skip a rung.** Start every new drill at level 1 or 2, even if you know the technique.

## 4. Roles and equipment

| Role | Job | Equipment |
|---|---|---|
| **Feeder** | Gives a predictable, repeatable attack at a stated speed | Gloves, shin guards as the drill requires |
| **Holder** | Holds pads and calls combinations | Focus mitts, Thai pads, kick shield |
| **Defender / counter-puncher** | Answers a stated attack with a stated response | Mouthguard, cup, gloves |
| **Flow partner** | Trades positions at low resistance | Gi or no-gi clothing, mat, nails trimmed |
| **Coach / timer** | Starts and stops rounds, watches intensity | Timer, bell |

Always: mouthguard (and a cup) for contact drills, wrapped hands, mats for anything that can end on the ground, **tap and stop means stop**.

## 5. Safety rules for every drill

1. Agree the intensity level **before** the first rep. Say it aloud.
2. The feeder sets the pace; the defender can ask to slow down at any time.
3. Head contact is level 2 or lower unless you are in supervised sparring.
4. Joint locks: **slow in, slow out.** Never speed up when someone resists.
5. Choke or neck drills: release immediately on a tap, and on any verbal signal or a loss of control.
6. Stop when technique falls apart — tired reps are bad reps and injury reps.
7. Concussion symptoms, a sharp joint pain or a failed check means the session is over for that person.
8. Kids and beginners: levels 1–2 only, supervised by a qualified coach.

## 6. The library

| File | What's inside |
|---|---|
| [`boxing.md`](boxing.md) | Boxing combinations, counter-combinations, partner drills |
| [`kickboxing.md`](kickboxing.md) | Punch-kick combinations, kick-defense and counter drills |
| [`muay-thai.md`](muay-thai.md) | Teep/kick/knee/elbow combinations, clinch drills, check-and-catch drills |
| [`jiu-jitsu.md`](jiu-jitsu.md) | Attack chains, escape-and-reversal chains, positional and flow drills |
| [`mma.md`](mma.md) | Strike-to-takedown combinations and cross-art drills |

Every combination has a **Diff** (1–5, same scale as the atlases) and a **purpose**. Every drill has an **intensity**. All are *starting points* — a coach adapts them to the room, the people and the ruleset.
