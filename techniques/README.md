# Technique Library

Deep-dive pages for individual techniques. Where [`jujitsu.md`](../jujitsu.md) is
the **map** — every technique that exists, rated for difficulty and frequency —
this library is the **manual**: how each technique actually works, and how it
changes against different bodies.

## Why this library exists

Most instruction teaches a technique once, against an imaginary opponent who is
exactly your size. Real training is not like that. The armbar that finishes your
same-size training partner slides off someone with short, thick arms. The mount
escape that works on a 70 kg opponent does nothing against 110 kg. The triangle
that closes easily on a narrow opponent will not lock on someone with broad
shoulders.

**Every page in this library has a dedicated section on adjusting the technique
for the opponent's body — and for your own.** That section is the longest one on
each page, because it is where most of the real skill lives.

## Contents

| Section | What's in it |
|---|---|
| **[Submissions](submissions/)** | 22 finishing techniques — chokes, arm locks, leg locks. Mechanism, application, body-size adjustments, failure modes, chains, defense, drills, safety. |
| **[Escapes](escapes/)** | 7 positional escape systems. Survival rules, every escape from that position, and how the escape changes against much bigger or much smaller opponents. |

## How to use it

- **Learning a technique for the first time?** Read sections 1 and 2, then drill. Ignore the rest until it works on someone.
- **It works on some people and not others?** That's section 3, and it's why this library exists.
- **It stopped working entirely?** Section 5, failure modes.
- **Just got tapped by it?** Section 7, defending it.

## Structure

```
techniques/
├── README.md              this file
├── _TEMPLATE.md           the page spec — read before adding a page
├── submissions/           22 pages + index
└── escapes/                7 pages + index
```

## Adding a page

Follow [`_TEMPLATE.md`](_TEMPLATE.md) exactly. The value of this library is that
the same information lives in the same place on every page — a page that
invents its own structure is worse than no page. Difficulty and frequency
ratings must match [`jujitsu.md`](../jujitsu.md).

## Related

- [`jujitsu.md`](../jujitsu.md) — the full technique atlas, ratings, rulesets, class syllabus
- [`training-plan.md`](../training-plan.md) — the 8-week MMA-focused program that schedules this material
