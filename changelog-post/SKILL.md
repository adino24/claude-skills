---
name: changelog-post
description: |
  This skill should be used when the user asks to prepare, write, format, shorten or post
  release notes, patch notes or a changelog for Possible One: Lunar Industries. Trigger on
  "post a changelog", "write patch notes", "prepare the Steam post", "post a GOG changelog",
  "format a changelog for GOG", "make the changelog files", or when handed a raw changelog
  draft file, pasted changelog text, or a link to one. Produces BOTH platform files in one
  run: a Steam post and a GOG changelog, each as markdown. Covers splitting prose from fix
  entries, shortening over-long prose, the humanizer pass, Steam field structure, GOG's
  "Create Changelog" form fields, the zero-Steam-reference rule for GOG, and the required
  pre-submit checks. Supersedes the retired gog-changelog skill.
metadata:
  game: "Possible One: Lunar Industries"
  platform: Steam + GOG
---

# Changelog Posting Rules — Possible One: Lunar Industries

One run, two output files: a Steam post and a GOG changelog. Both are markdown. Always
produce both, even if the user only mentions one platform. Do not ask which platforms to
target. This skill assumes the Steam post has not been published yet.

The GOG half of this skill replaces the retired `gog-changelog` skill. If any older doc,
note or habit says to write `.txt`, or to save GOG output to a `GOG-Changelogs` folder,
that is stale. This file wins.

## 1. Sourcing the content

- Input can be a raw draft file in the working folder, pasted text, or a link. If given a
  link, fetch it and work from the fetched content, not from a guess about what it contains.
- Split the source at the mechanical changelog heading (the line reading
  `Version [number] Changelog` or equivalent): **prose above, fix entries below.** Every
  later step treats these two halves differently.
- The source may also carry an internal metadata header (draft revision, build paths, where
  the version was read from). That is not part of either post. Drop it.
- Never carry a local build path, drive letter, or internal file path into an output file.

## 2. Version name and date

- **Always ask the user for the version number and the release date before drafting.** Never
  infer either one. A version number appearing in the source is not necessarily the one being
  submitted, and the release date is not necessarily today.
- Ask whether it is a **Patch** (regular update) or a **Hotfix** (small emergency fix), and
  label consistently across both files.
- GOG's version name field format: `Patch [version] ([release date])`, e.g.
  `Patch 0.7.04.0404 (15 September 2026)`. Under GOG's 255-character hard limit.

## 3. Shortening the prose

- Shorten the prose half if it is running long. Cut hype framing, repetition, and any
  paragraph that restates what a later section already says.
- Keep every factual claim. Shortening removes words, never facts, numbers or named systems.
- **Never reword a fix entry.** The bullet section's wording is not yours to edit at any
  stage, in either file. Converting its markup (see section 5) is expected; rewriting its
  sentences is not.

## 4. Humanizer pass

- Run the prose half through the `humanizer` skill in **embedded mode** (returns only the
  rewritten text, no draft/audit ceremony).
- Keep the register plain and technical. This is changelog copy, so humanizer's personality
  guidance does not apply: no injected opinion, banter, or first-person colour.
- Skip the pass on individual fix entries. They are already short and mechanical (what broke,
  what now happens).
- Watch for the tells this source tends to carry: rows of dramatic sentence fragments,
  formulaic sayings ("a freeze is only a freeze until..."), "not X but Y" constructions,
  closing punchlines, decorative bolding, and decorative emoji in headings.
- If removing something that may be established studio voice (a signature emoji, a running
  joke, a sign-off), flag the removal to the user instead of silently keeping or cutting it.

## 5. Steam file

- Standard markdown throughout. **No BBCode.** Steam accepts pasted markdown formatting.
- If the source draft is written in BBCode, convert it: `[h2]` to `##`, `[h3]` to `###`,
  `[b]` to bold, `[i]` to italic, `[list]` and `[*]` to `-` bullets, `[hr]` to `---`, and
  `[p]...[/p]` to plain paragraphs. Drop the empty `[p][/p]` spacers.
- Structure, in order: `TITLE`, `SUBTITLE`, `SUMMARY`, `BODY`, as markdown headings.
- Append the fix-entry section after the prose, wording untouched, markup converted.
- Keep any image placeholder note from the source. It tells whoever posts where the
  screenshot goes.
- Save as `Steam-Post_[YYYY-MM-DD]_[version].md`.

## 6. GOG file

Derive it from the finished Steam prose, not from the raw source.

- Strip: greetings and salutations ("Hello Industrialists and Astronauts!"), community
  thank-you and call-to-action paragraphs (the F8 feedback ask), sign-offs, image
  placeholders, and hype framing.
- Keep: the feature explanation prose, and every individual fix entry.
- Re-derive the section breakdown. Do not inherit the source's headers as-is.
- Open with one short intro line summarising the update in plain terms, no hype.
- Order: new features and balance changes first, then fixes. Returning players scan for what
  is new.
- Section labels as bold, entries as real `-` bullets. In GOG's editor these go in via the B
  button and the bullet-list tool, never as typed dashes. Note that in the file.
- Include the form fields at the top: the version name from section 2, and the Visible From
  field (optional, UTC, blank publishes immediately).
- Save as `GOG-Post_[YYYY-MM-DD]_[version].md`.

### Zero Steam references, hard rule, no exceptions

A GOG changelog contains no mention of Steam, Valve, Steamworks, Steam Workshop, Steam
Achievements, Steam Cloud, or any Steam-branded term, in any form. That includes reworded
mentions like "same as the Steam version", link targets, pasted screenshot text, and any
provenance or "prepared from" line in the file header.

Use platform-neutral phrasing: "in-game achievements", not "Steam Achievements"; "community
mods", not "Steam Workshop"; "cloud saves", not "Steam Cloud". Rewrite the sentence so it
reads naturally; do not just delete the word.

## 7. Fix entries

- One sentence per fix: what was broken, what now happens.
- Merge near-duplicate entries only when they share the same root cause. Two distinct
  triggers producing the same symptom stay as two bullets.

## 8. Tone

- Grounded and factual. This is a hard-science lunar sim. No "groundbreaking",
  "revolutionary", "game-changing".
- Preserve developer phrasing. Edit for clarity and grammar, not for style.

## 9. Output location

Both files go in the same folder as the source draft they were built from, which is the
dated folder for that release. Not a separate per-platform folder, and not the session's
starting directory if that differs.

## 10. Pre-submit checks

Run these and report the results. Do not report the work as done without them.

- [ ] Version number, release date, and Patch/Hotfix label came from the user, not inferred
- [ ] Prose shortened where needed; no fix entry reworded
- [ ] Prose passed through `humanizer` (embedded mode, technical register)
- [ ] Steam file is markdown, no BBCode left: search for `[h2`, `[b]`, `[list]`, `[/p]`
- [ ] Literal case-insensitive search of the **whole GOG file** for `steam`, `valve`,
      `steamworks` returns zero hits. Any hit is a blocker, including in a header line.
- [ ] Zero em dashes and en dashes in either file
- [ ] No internal build paths or drive letters in either file
- [ ] Both files saved to the dated source folder with the section 5 and section 6 names
- [ ] Grammar and punctuation pass

## 11. What to surface to the user

Things this skill cannot verify alone. Raise them as questions or flags, never as guesses:

- **Platform accuracy.** GOG's own form warns about this. Confirm every listed feature, mode,
  system and language count is actually live on the GOG build. Locale counts are the usual
  offender ("added in six languages"). Flag anything Steam-only, beta-only, or not yet
  shipped on GOG.
- Any voice-level edit that might have cut something deliberate (section 4).
- Any fix entry whose wording is unclear enough that shortening it would risk the meaning.
