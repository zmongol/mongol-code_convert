# Mongolian spelling corrections — look-back record

The one place where the converter fix, the wrong-words findings, and every
spelling correction live, so we can always look back. This file is the
canonical copy; the 30-minute Facebook posting job reads a synced copy of it
at `~/workspace/facebook-mongolian-news/spelling-corrections.md` at the start
of every run and applies every row in the corrections table when it drafts.
When a row is added here, sync the copy there too.

## How the pipeline actually works

There is NO Cyrillic step. Each run drafts the story directly in traditional
Mongolian Unicode (MenkLetter), then converts it to Z52 with mongol-convert
(this repository). The converter is faithful to whatever spelling it is
given: a wrong spelling in the draft comes out wrong in the post, and the
converter cannot fix it.

That means wrong words in the posts come from two different places, and only
one of them is fixed in code:

- **Converter bug** — the converter itself maps a correct spelling to the
  wrong Z52. Fixed in this fork, with a regression test.
- **Drafting error** — the draft spells the word the modern Khalkha way
  instead of the classical way. Fixed by spelling it right in the draft, and
  recorded in the table below.

## Fixes log

- **2026-11-17 — England / medial GA after ANG.** Converter wrote
  ᠠᠩᠭᠯᠢ as ᡥᡪᡪᢊᡪᡪᢑᡫ; correct is ᡥᡪᡪᢊᢊᢑᡫ. Cause: `resolve_g` in
  `crates/mongol-convert/src/letter/menk.rs` returned the double-tooth form
  for GA after ANG in back-vowel (masculine) words. Fix: GA immediately
  after ANG now keeps the hook form. Commit `6d48f10` on this fork, with a
  regression test in `crates/mongol-convert/tests/cli.rs`. The skill's
  converter was rebuilt from the fork the same day (mongol-convert 0.8.1).
- **2026-11-17 — Packager source URL.** The skill packager hardcoded the
  upstream repository URL in the manifest. It now derives the source URL
  from the git remote, so a skill packaged from the fork records the fork.
  Commit `5508ed5` on this fork.
- **Rule (user, 2026-11-17): all converter work happens in this fork —
  never upstream.**

## Corrections table

Every row is verified: the "correct spelling" column, run through the
converter, produces exactly the Z52 in the last column. "Wrong spelling" is
what the drafts used to write. Add a row whenever the user corrects a word —
verify the converter output first, then record it here.

| Wrong spelling (draft) | Correct spelling (classical) | Meaning | Correct Z52 (verified) | Date |
| --- | --- | --- | --- | --- |
| ᠪᠤᠤᠷᠠᠪᠠ | ᠪᠠᠭᠣᠷᠠᠪᠠ (bagoraba) | fell / dropped / reduced | ᡳᡪᡱᡱᡭᢞᡪᡳᡪᡨ | 2026-11-17 |
| ᠪᠤᠤᠷᠠᠭᠰᠠᠨ | ᠪᠠᠭᠣᠷᠠᠭᠰᠠᠨ (bagoragsan) | has fallen | ᡳᡪᡱᡱᡭᢞᡪᡪᡪᢔᡪᡧ | 2026-11-17 |
| ᠪᠤᠤᠷᠠᠵᠤ | ᠪᠠᠭᠣᠷᠠᠵᠤ (bagoraju) | falling | ᡳᡪᡱᡱᡭᢞᡪᢛᡳ | 2026-11-17 |
| ᠪᠤᠤᠷᠠᠬᠤ | ᠪᠠᠭᠣᠷᠠᠬᠤ (bagorakhu) | to fall | ᡳᡪᡱᡱᡭᢞᡪᡪᡪᡳ | 2026-11-17 |
| ᠪᠤᠤᠷᠠᠭᠤᠯᠬᠤ | ᠪᠠᠭᠣᠷᠠᠭᠤᠯᠬᠤ (bagoragulhu) | to cause to fall | ᡳᡪᡱᡱᡭᢞᡪᡱᡱᡭᢑᡪᡪᡳ | 2026-11-17 |
| ᠪᠤᠤᠷᠠᠭᠤᠯᠵᠤ | ᠪᠠᠭᠣᠷᠠᠭᠤᠯᠵᠤ (bagoragulju) | causing to fall | ᡳᡪᡱᡱᡭᢞᡪᡱᡱᡭᢑᢛᡳ | 2026-11-17 |
| ᠠᠤᠰᠲᠷᠠᠯᠢ | ᠠᠤᠰᠲᠷᠯᠢᠶ᠎ᠠ (Australia) | Australia | ᡥᡪᡭᢔᢘᢞᢑᡬᡫᡨ | 2026-11-17 |
| ᠵᠠᠬᠤ | ᠵᠠᠬ᠎ᠠ (jaka) | percentage point (in "percent's jaka") | ᡬᡪᢇᡨ | 2026-11-17 |
| ᠦᠨᠡ | ᠦᠨ᠎ᠠ (üne) | price | ᡥᡭᡬᡰᡨ | 2026-11-17 |
| ᠳᠤᠤᠷᠠ | ᠳᠣᠣᠷ᠎ᠠ (doora) | lowest / below (in "lowest price") | ᢘᡭᡭᢝᡨ | 2026-11-17 |

## General rules (from the corrections above)

- Classical spelling keeps the G where Khalkha contracts -agu- / -egü- to a
  long vowel: bagoraba, not booraba. When unsure about a word's classical
  spelling, prefer the form with the G kept.
- Foreign names ending in -ia take -ᠢᠶ᠎ᠠ (Y + FVS4 + A): Australia is
  ᠠᠤᠰᠲᠷᠯᠢᠶ᠎ᠠ.
- A word-final A after Q, N or R takes the free variation selector: ᠵᠠᠬ᠎ᠠ (jaka,
  "percentage point"), ᠦᠨ᠎ᠠ (üne, "price") and ᠳᠣᠣᠷ᠎ᠠ (doora, "lowest /
  below") — not ᠵᠠᠬᠤ / ᠦᠨᠡ / ᠳᠤᠤᠷᠠ and not the plain forms without the
  selector.

## Known open items

- The wrong spellings above are already live in Reels posted on 2026-11-17
  (the ᠪᠤᠤᠷᠠ- family appears 19+ times in that day's drafts). A posted
  Reel cannot be edited — re-posting a corrected version is the user's call.
- Other ᠤᠤ / ᠥᠥ words in the 2026-11-17 drafts (e.g. ᠵᠤᠤᠨ, ᠮᠤᠤ,
  ᠰᠤᠤᠷᠢ, ᠳᠤᠤᠷᠠ, ᠳᠠᠭᠤᠤ) have NOT been checked — some may be genuine long
  vowels, some may be the same Khalkha-contraction mistake. They need the
  user's eye before any row is added for them.
