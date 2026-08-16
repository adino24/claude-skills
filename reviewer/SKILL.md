---
name: reviewer
description: |
  This skill should be used when the user wants to draft a reply to a Steam (or similar platform)
  player review for Possible One: Lunar Industries, especially a negative/critical one. Trigger on
  "respond to this review", "draft a reply to this Steam review", "write a response to this negative
  review", pasted review text, or a Steam review URL like
  steamcommunity.com/id/<user>/recommended/<appid>/. Covers extracting numbered complaints from a
  review, collecting the developer's stance on each, drafting a response in the studio's established
  voice, a mandatory humanizer pass, and logging the review + response pair to review-log.md.
metadata:
  game: "Possible One: Lunar Industries"
  platform: "Steam (and similar review platforms)"
---

# Steam Review Response Skill — Possible One: Lunar Industries

Draft developer replies to player reviews, in the voice the studio has already established in its real
responses. This skill never writes a final response in one shot — it extracts complaints, waits for the
developer's actual stance on each, then drafts.

## 1. Getting the review

Accept either:
- **Pasted review text** — use as-is.
- **A Steam review URL** (e.g. `steamcommunity.com/id/<user>/recommended/<appid>/`) — fetch it and pull:
  username, recommendation status (Recommended/Not Recommended), hours played, posted date, and the
  full review text.
  - Fetched text comes back paraphrased by a summarizing model, not guaranteed verbatim. If the
    extraction reads like it's been compressed or reworded, say so and let the user confirm before
    building a complaint list off it.

## 2. Extract complaints — numbered, don't draft yet

List every distinct complaint in the review as **numbered** points. Numbering lets the developer reply
point-by-point instead of writing prose back.

- Split genuinely separate issues into separate numbers, even if the reviewer ran them together in one
  sentence.
- Include positives the reviewer mentioned as their own numbered point too (e.g. "liked the cutscenes") —
  these get acknowledged in the response, not skipped.
- Do not draft a response at this stage. Stop and wait for developer input.

## 3. Collect developer input

Wait for the user to give their stance on the numbered complaints — status, fix, decision, or "ignore."
Rules:
- The user can answer a subset and explicitly tell you to ignore the rest for this response. If so, only
  draft against what they answered, but still log which points were skipped rather than silently dropping
  them (see §6).
- Never invent a fix, roadmap item, date, or feature name that the user didn't give you. If a complaint
  has no developer input, either leave it out of the draft or ask — don't guess at what the studio's
  position is.

## 4. Drafting rules

**Structure, in order:**
1. **Greeting + thanks.** Address the reviewer by name if known. Thank them for the time invested — cite
   hours played or level of detail if notable ("that's no small feat," "such detailed feedback").
2. **Empathy, not defensiveness.** Name the actual issues they hit specifically — don't soften or
   generalize them away.
3. **Status per issue**, for each complaint being addressed:
   - *Actively working on / fixing* — confirmed bugs.
   - *Already planned / in development* — known systemic issues.
   - *Under consideration / on our radar / nothing locked in yet* — feature requests or bigger asks. Never
     promise a date or commit to something not decided.
4. **F8 bug report ask** — for clearly reproducible bugs/crashes/UI glitches, you may add this
   automatically even without explicit developer input: ask the player to press F8 in-game to send logs
   and save data. Skip it for pure design/balance/opinion feedback where there's nothing to reproduce.
5. **Acknowledge positives** the reviewer mentioned, if addressing them per §3's guidance.
6. **Answer design/philosophy questions honestly** (e.g. "who is this game for?") instead of deflecting.
7. **Close with invitation + appreciation** — invite them to check back after patches. Warm, not salesy.

**Tone:**
- Casual-professional. Contractions are fine. Light warmth (":)", "seriously") suits constructive or
  positive-leaning reviews — dial it back for angry or harsh ones.
- No corporate filler ("we take this very seriously," "your feedback is important to us"). Be concrete.
- Non-defensive, even when a complaint stems from player misunderstanding — clarify gently, don't correct
  bluntly.
- No formal sign-off block (no "— The Possible One Team" seen in the studio's real responses) — end on
  the appreciation/invitation line itself.

**Hard rules:**
- Every complaint being addressed in a given draft gets at least a one-line acknowledgment — don't
  cherry-pick only the easy ones unless the user explicitly said to limit scope (see §3).
- Don't fabricate specifics: patch numbers, dates, feature names, roadmap commitments must come from the
  user's input only.
- Keep length roughly proportional to the review — short terse review, short reply; long detailed review,
  longer itemized reply.

## 5. Humanizer pass

Run every draft through the `/humanizer` skill before presenting it. Use embedded mode (return only the
rewritten text) once this skill is being run non-interactively; in normal use, the draft/audit/final loop
is fine to show if useful, but the version handed to the user for approval must be the humanized final.

## 6. Logging

Append every review + response pair to `review-log.md` in the current working directory (create the file
if it doesn't exist — see the template below). Include:
- Username, source URL (if any), recommendation status, hours, posted date.
- Full review text.
- Numbered complaints extracted.
- Developer's input per point, including any points the user said to skip/ignore — note the skip
  explicitly rather than omitting it silently.
- The final (humanized) draft.
- Status (`Draft — awaiting approval`, `Sent`, etc. — ask the user if it's unclear).

**Log entry template:**

```markdown
## Review: [username]

**Source:** [URL or "pasted"]
**Recommendation:** [Recommended/Not Recommended] | **Hours:** [x hrs] | **Posted:** [date]

**Review text:**
> [full text]

**Complaints extracted:**
1. ...
2. ...

**Developer input:**
1. ...
2. [skipped per user instruction, if applicable]

**Draft response (post-humanizer):**
> [final text]

**Status:** Draft — awaiting approval
```

## 7. Before-sending checklist

- [ ] Every reviewer name/detail used is accurate (no invented names, hours, or dates)
- [ ] Every complaint addressed has explicit developer input behind it, or is a standard F8 ask
- [ ] No promised dates/features beyond what the user actually confirmed
- [ ] Passed through `/humanizer`
- [ ] Logged in `review-log.md`
