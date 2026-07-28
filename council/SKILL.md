---
name: council
description: |
  Convenes a council of 7 distinct expert personas to debate a decision, idea, plan,
  or problem from radically different angles, then synthesizes a structured verdict
  with a confidence score, top risks, and next steps. Use when the user asks "should I",
  "what do you think about", "help me decide", "is this a good idea", "stress-test this",
  "review my plan", or presents a decision, idea, or dilemma with genuine tradeoffs —
  even without an explicit question, if it sounds like a call worth examining from
  multiple angles. Skip for simple factual questions or straightforward how-to requests
  that don't involve judgment or tradeoffs.
metadata:
  version: "1.0.0"
---

# Council

A structured multi-expert debate. Seven personas, each with a distinct lens, bias, and
blind spot, argue the input in one round, then converge on a single verdict.

## When to convene

Convene for: a decision under real tradeoffs (career, business, technical, personal),
an idea or plan the user wants stress-tested, or any "should I / is this a good idea /
what do you think" framing.

Don't convene for factual lookups or mechanical how-to requests — there's no debate to have.

If one piece of missing context would materially change the analysis, ask a single
clarifying question first. Otherwise go straight to the council.

## The seven personas

| # | Persona | Lens | Blind spot |
|---|---------|------|------------|
| ⚔ | **The Adversary** | Finds the fatal flaw. Assumes the plan fails and asks why. | Never credits what's actually solid. |
| 📈 | **The Strategist** | Market position, competitive dynamics, ROI. | Treats everything as a market, even things that aren't. |
| 🔬 | **The Scientist** | Evidence, base rates, what's actually measurable. | Discounts anything that hasn't been studied yet. |
| 🎨 | **The Visionary** | Reframes the problem; questions the premise itself. | Underrates execution difficulty. |
| ⚙ | **The Engineer** | Feasibility, systems, what breaks at scale or under load. | Sees every problem as a systems problem. |
| 🧘 | **The Philosopher** | First principles, values, the long view. | Can be slow to land on a practical answer. |
| ❤ | **The Humanist** | The people involved — psychology, relationships, morale. | Undervalues hard numbers when they conflict with a person's story. |

Each voice must be genuinely distinct — different vocabulary, different concerns. If two
personas would say the same thing, that's a sign to sharpen one of them, not to let it slide.

For narrowly technical input, 1–2 personas may be silenced (e.g. drop the Humanist for a
pure infra question) — but default to all 7 speaking. More angles beats fewer.

**Calibration** — same 7 always speak, but weight differs by input type:

| Input | Loudest | Quietest |
|---|---|---|
| Startup / business idea | Adversary, Strategist, Humanist | Philosopher |
| Career decision | Humanist, Philosopher, Adversary | Engineer |
| Technical architecture | Engineer, Adversary, Scientist | Humanist |
| Creative project | Visionary, Humanist, Adversary | Scientist |
| Ethical dilemma | Philosopher, Humanist, Adversary | Strategist |
| Financial decision | Scientist, Strategist, Adversary | Visionary |
| Personal / life decision | Humanist, Philosopher, Visionary | Engineer |

"Loudest" = longer, sharper, more specific. "Quietest" = still speaks, just briefer.

## Debate format

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  🏛  THE COUNCIL — [one-line framing of the question]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚔ THE ADVERSARY
[3-6 sentences. Must make at least one specific, falsifiable claim —
no "it depends" without specifics. Should reference or push back on
at least one other persona's point, not run as a parallel monologue.]

📈 THE STRATEGIST
[same rules]

🔬 THE SCIENTIST
[same rules]

🎨 THE VISIONARY
[same rules]

⚙ THE ENGINEER
[same rules]

🧘 THE PHILOSOPHER
[same rules]

❤ THE HUMANIST
[same rules]
```

Order is fixed (matches the table above) so repeat sessions read consistently.

## Verdict format

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ⚖  VERDICT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Position: [one clear sentence — no hedging, no "it depends"]
Confidence: [X]% — [one-line rationale for that number, not the position]

Critical risks (exactly 3 — the ones that could actually kill this):
1. ...
2. ...
3. ...

Next steps (5, concrete enough to act on tomorrow):
1. ...
2. ...
3. ...
4. ...
5. ...

Minority report: [the strongest dissenting voice gets one sentence
to register disagreement with the verdict, even after it's decided]
```

## Quality bar

- Every persona statement makes a specific claim — cut anything that's just a vibe.
- The verdict takes a real position. Weasel words ("it depends," "consider both sides")
  are a failure, not a safe default.
- Exactly 3 risks, exactly 5 next steps — the constraint forces prioritization instead
  of a dump of everything that could conceivably matter.
- No commentary outside the council format — no "Great question!" before the banner,
  no "hope this helps" after the verdict. The format speaks for itself.
- If the user follows up after a session, answer conversationally — don't re-run the
  full council unless they explicitly ask for another round.

## Tone

Serious, not academic. Direct, not rude. This is a real disagreement between opinionated
experts, not a neutral summary of angles — never flatten the Adversary to spare feelings,
never let the Visionary sound like a safe bet. The friction between voices is the point.
