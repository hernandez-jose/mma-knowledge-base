# Technique page template

Every submission page in `submissions/` follows this exact structure. Keep the
headings, the order, and the table columns identical across files — the value of
this library is that you can find the same information in the same place on
every page.

Escape pages in `escapes/` follow the variant at the bottom of this file.

---

## Required front matter (immediately under the H1)

```markdown
# Rear Naked Choke

> One sentence: what it is and why it earns its place.

| | |
|---|---|
| **Category** | Strangle (blood choke) |
| **Comes from** | Back control, turtle, technical mount |
| **Difficulty** | 2/5 |
| **How common** | 5/5 |
| **Legal** | All belts, all major rulesets |

**Also known as:** mata leão, sleeper, RNC
```

Difficulty and frequency ratings must match [`jujitsu.md`](../jujitsu.md). If
you think a rating is wrong, say so in the page — don't silently change it.

---

## Required sections, in this order

### `## 1. The mechanism`
What *physically* causes the tap. Be precise: which structure is compressed,
stretched or rotated, and against what. One or two short paragraphs. This
section is what makes every later adjustment make sense — if a reader
understands the mechanism, they can invent their own adjustments.

### `## 2. The standard application`
Numbered steps for a same-size opponent. 5–9 steps. Each step is one action.
Bold the two or three details that people most often get wrong.

### `## 3. Adjusting for their body`
**This is the most important section on the page. Give it the most words.**

Open with this quick-reference table:

```markdown
| Their build | Why it breaks the standard version | Your adjustment |
|---|---|---|
```

Then a `###` subsection for each build that materially changes the technique.
Use only the ones that genuinely apply — do not pad with builds that make no
difference. Typical ones:

- Short limbs / short arms
- Long limbs / long arms
- Thick neck, heavy traps
- Thin neck
- Much heavier than you
- Much lighter than you
- Very flexible
- Very stiff / muscular
- Long torso / short torso

Each subsection must give a **concrete mechanical change** — a different grip,
a different angle, a changed hip position, a different finishing lever. Not
"be more patient" or "use technique." Specifics like *"drop your hips two
inches toward their head so the elbow clears your centreline"* are the point.

### `## 4. Adjusting for your body`
The same idea in reverse — what changes when *you* are the short-armed one, the
lighter one, the one with small hands. Shorter than section 3, but real.

### `## 5. Failure modes`
Table, always these three columns:

```markdown
| What you feel | What's actually wrong | Fix |
|---|---|---|
```

### `## 6. Chains`
What to take when it fails, and what feeds into it. Prefer a small table of
"their defense → your next attack."

### `## 7. Defending it`
Early, middle and late defense. Be explicit that late defense is worse than
early defense.

### `## 8. Drills`
2–4 concrete drills with a rep scheme or a positional-sparring start position.

### `## 9. Safety`
Injury risk, how fast to apply, when to release, ruleset/belt legality.

### `## Related`
Bullet links to sibling pages and back to the atlas. Relative paths only.

---

## Escape page variant (`escapes/`)

Same front matter shape, then:

1. `## 1. Why the position works` — what the top player is controlling.
2. `## 2. The rules of survival here` — frames, breathing, what never to do.
3. `## 3. The escapes` — one `###` per escape, each with numbered steps.
4. `## 4. Adjusting for their body` — **the priority section**, same table format.
5. `## 5. Adjusting for your body`
6. `## 6. Failure modes` — same three-column table.
7. `## 7. Drills`
8. `## 8. Safety`
9. `## Related`

---

## House style

- Second person. "You" do the technique, "they" are the opponent.
- Use they/them for both people throughout — never assume gender.
- Metric and imperial both fine, but be concrete about distance and angle.
- No invented statistics. If you cite a number, it must be one already in
  `jujitsu.md` or clearly hedged as a rule of thumb.
- Don't repeat the whole atlas — link to it.
- Every page must be useful to a blue belt and not wrong to a black belt.
