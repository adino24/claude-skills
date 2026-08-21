---
name: gog-changelog
description: |
  This skill should be used when the user asks to "post a GOG changelog", "write GOG patch notes",
  "format a changelog for GOG", "submit a changelog to GOG", or is preparing release/patch notes for
  Possible One: Lunar Industries to submit through GOG's "Create Changelog" form. Covers GOG-specific
  field formatting, the zero-Steam-reference rule, a humanizer pass for prose sections, fetching and
  cleaning source content from a link, and always asking the user for the version name.
  GOG only — do not use for Steam changelogs; that is a separate skill/rules doc.
metadata:
  game: "Possible One: Lunar Industries"
  platform: GOG
---

# GOG Changelog Posting Rules — Possible One: Lunar Industries

Format and verify a changelog before submitting it through GOG's "Create Changelog" form. Apply these rules only to GOG submissions — a separate rules doc/skill will cover Steam once that workflow starts, and the two are not interchangeable, especially on the Steam-reference rule in §5.

## 1. Sourcing the changelog content
- Input can be a pasted block of text or a link. If given a link, fetch it and work from the fetched page content, not from a guess about what it contains.
- Either way, the source usually mixes real changelog content with surrounding fluff. Strip the fluff and keep only the substance:
  - Cut greetings/salutations (e.g. "Hello Industrialists and Astronauts!"), community thank-you/call-to-action paragraphs (e.g. "keep pressing F8 to send us feedback"), sign-offs, and any hype-framing narrative around the actual entries.
  - Keep: the feature description(s) and every individual fix/change entry.
  - When a feature is explained in prose before the bullet list, keep that explanation (it feeds the humanizer pass in §6) — just drop the marketing frame around it, not the substance.
- Do not carry over section headers or structure from the source as-is; re-derive the section breakdown from §4 once the fluff is stripped.

## 2. Version name field
- **Always ask the user for the version name — the version number and the release date — before drafting.** Never infer either one from the source content (a version number that appears in the source page/text is not necessarily the one to submit) or from the current date. Use exactly what the user gives you.
- Use the format `Patch [version number] ([release date])`, e.g. `Patch 0.7.04.0404 (15 September 2026)`.
- Always include both the version number and the release date — GOG's own recommendation for clarity.
- Stay under GOG's 255-character hard limit for this field.
- Label consistently: "Patch" for regular updates, "Hotfix" for small emergency fixes.

## 3. Visible From field
- Leave optional; timezone is UTC.
- Leave blank to publish immediately, or set a future date/time to schedule the post.

## 4. Changelog body — structure
- Open with one short intro line summarizing the update in plain terms — no hype language.
- Break content into clearly labeled sections using bold text (the editor's B button), not markdown headers — the GOG editor is rich-text, not markdown.
- Use the bullet-list tool for every individual entry. Never type dashes or asterisks as plain-text bullets.
- Reuse these section labels where they fit:
  - **[Feature Name]** (e.g. "New Astronaut System")
  - **[Feature Name] — Fixes & Tweaks**
  - **Missions and Guides**
  - **Fixes**
- Keep section order consistent across entries: new features first, then fixes, so returning players can scan quickly.

## 5. Tone and content rules
- Stay grounded and factual — this is a hard-science lunar sim. Avoid buzzwords like "groundbreaking," "revolutionary," "game-changing."
- Preserve original developer phrasing where possible; edit only for clarity and grammar, not for style.
- **Platform accuracy check** (per GOG's own banner warning): before submitting, confirm every listed feature, game mode, system, and language is actually supported on the GOG build. Remove or flag anything Steam-only, beta-only, or not yet live on GOG.
- **Zero Steam references — hard rule, no exceptions.** A GOG changelog must contain no mention of Steam, Valve, Steamworks, Steam Workshop, Steam Achievements, Steam Cloud, or any other Steam-branded term, in any form — including reworded mentions like "like on Steam" or "same as the Steam version." Use fully platform-neutral phrasing instead: "in-game achievements" not "Steam Achievements," "community mods" not "Steam Workshop," "cloud saves" not "Steam Cloud." This applies to body text, links, and any pasted image/screenshot text.
- **Verify before submitting:** run a literal, case-insensitive text search of the full draft for `steam`, `valve`, and `steamworks`. Zero hits required. Treat any hit as a blocker — rewrite the sentence so it still reads naturally platform-neutral, don't just delete the word.

## 6. Humanizer pass
- After drafting the intro line and any longer feature-explanation paragraphs (not one-line fix bullets — see §7), run that prose through the `/humanizer` skill in **embedded mode** so it returns only the rewritten text, no draft/audit ceremony.
- Keep the register technical and plain, not personality-forward: this is reference/changelog copy, so humanizer's "PERSONALITY AND SOUL" section does not apply — no injected opinion, banter, or first-person color. The only goal is to strip AI-sounding tells (buzzwords, em dashes, rule-of-three, inflated-significance phrasing, filler) while keeping content factual per §5.
- Skip this pass on individual fix bullets — they are already short and mechanical (what broke → what now happens) and don't need it.

## 7. Fix entries
- Describe what was broken and what now happens, in one sentence per fix.
- Merge near-duplicate fixes only when they share the same root cause. If two distinct triggers caused the same symptom, list them as separate bullets for clarity.

## 8. Output file
- Save the finished changelog as a standalone `.md` file, in addition to presenting it in the conversation.
- Name the file after the version name from §2, exactly as it will be submitted — e.g. `Patch 0.7.04.0404 (15 September 2026).md`.
- Save it in the directory Claude Code started in for the session (the initial working directory), not whatever directory the conversation may have navigated to since.

## 9. Before-submitting checklist
- [ ] Source content fetched (if a link) and stripped of greetings, thank-yous, and sign-off fluff
- [ ] User was asked for and provided the version name (number + date) — not inferred
- [ ] Version name includes number + date
- [ ] Intro line present, no hype words
- [ ] Intro line and feature-explanation paragraphs passed through `/humanizer` (embedded mode, technical register kept)
- [ ] Sections bolded, entries bulleted via editor tools (not typed symbols)
- [ ] All features/languages confirmed live on the GOG build
- [ ] Literal text search for "Steam" / "Valve" / "Steamworks" returns zero hits
- [ ] Grammar and punctuation pass
- [ ] Output saved as `[version name].md` in Claude Code's starting directory
