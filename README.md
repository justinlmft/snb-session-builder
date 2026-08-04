# SNB Session Builder — one tool, every live practice

Live: **https://justinlmft.github.io/snb-session-builder/**
(`?p=mm` · `?p=cb` · `?p=srs` · `?p=ss` deep-link straight to a practice)

Hosts Mindful Moment, Capacity Builder, Self-Regulation Simplified and Safety
Simplified in a single page with one presenter console, one student window, one live check-in and one
ambient-scene engine. **Safety Simplified was added 2026-08-04 as the first test of that claim** — it took
one content module (`ss_content.js`) plus a dial block, no shell changes, and one
line on the backend type whitelist. It is a 4-week cohort: Passive Safety
Environment → Anchored Awareness → Witnessing → Experiencing.

Its practices run off the **canonical ladder, one stage deeper each week** —
which is literally what the cohort teaches:

| week | practice | ladder depth |
|---|---|---|
| 1 | three items, like/dislike (each following one of Jenny's) | none yet |
| 2 | Anchored Awareness · Validate · Normalize | `anchor` |
| 3 | Witnessing ("acting as a researcher") ×2 | `anchor + witness` |
| 4 | Experiencing (describing safety) ×2 | `anchor + witness + experience` |

`LADDER.segments()` splits the canonical ladder at its own headline boundaries,
so if CB ever changes, SS follows automatically. The source deck is worksheet-
shaped (several questions per slide) but presents one prompt at a time like every
other cohort — Justin, 2026-08-04.

## Two standards, both set by Justin on 2026-08-04

**Capacity Builder is the content standard.** The anchor ladder — the whole
cue → anchor → witness → experience spine — is defined **once**, extracted
verbatim from the CB source into `LADDER`. Self-Regulation Simplified calls it
instead of keeping its own copy. Where the two used to word a prompt differently,
CB's wording wins; where CB has no presenter note, SRS no longer adds one.
That collapsed 21 divergent prompts down to 1 (see Parity).

**Self-Regulation Simplified is the design standard.** One slide design language
for every practice, in `design.css` + `render.js`: everything left-aligned on one
Swiss grid, no colour rails, eyebrows left, a soft veil so text stays legible over
the ambient field, and the rise-in motion on content only (the logo and byline
never animate). MM and CB moved to match.

Each practice still owns its own **curriculum** — its beats — lifted verbatim from
its source. It no longer owns its own renderer or stylesheet.

## What the shell owns vs what each practice owns

| Shell (one copy) | Practice module (verbatim from source) |
|---|---|
| practice picker, presenter console, student window | its beats (`buildBeats`) — verbatim |
| **the slide design language + renderer** | its `map(beat)` → canonical slide kind |
| **the canonical anchor ladder** | its dial markup + dial rendering |
| ambient scenes (7) + scene picker | its agenda + session name |
| live check-in: publish, join code, QR, seams | its live config (type, ref, seams) |
| break clock · QR library · site-data fill | |

There is one frame class (`.snbframe`) and one stylesheet. A practice declares
`map(beat)` translating its native beat into a canonical slide kind — `cover`,
`ptitle`, `prompt`, `info`, `story`, `checkin`, `events` — and the shared
renderer draws it. Adding a slide type means adding it once.

The verbatim dial markup uses `st` and `render()` as globals. `setPractice()`
points `window.st` at the active module's own state object (same reference, so
mutations land where the module's `buildBeats` reads them) and publishes that
module's inline handlers. That is why no dial code had to be rewritten.

## What Capacity Builder gains

CB had no live check-in and no join overlay. Hosting it in the shell gives it
both for free. It still has **no authored check-in slides**, so no seams are
stamped yet — adding those is content work in the CB source (the plan in
`App Designer/Live-Checkin-Plan.md` specs four readings: open · between 1–2 ·
between 2–3 · close).

## Build

```bash
python3 build.py        # → snb-session-builder.html
node parity.mjs         # must print PARITY PASS
node smoke.mjs          # dials, tabs, console walk, dial wiring
```

`build.py` composes from the extracted source blocks in this folder. To pick up a
change made in a source tool, re-extract that practice's blocks and re-run.

## Parity

`parity.mjs` loads each source tool and the merged builder side by side, applies
the same dial sweep to both, and compares the **full beat list** — style, headline,
body, presenter note, section, kind, tagline, phase, break minutes — not just
counts. Randomised content (CB's obstacle statements) is seeded out.

The contract has two halves:

1. **MM and CB must stay byte-identical to their sources.** Nothing about those
   practices changed. Last run: **9/9 sweeps identical** (MM ×4, CB ×5).
2. **SRS is deliberately no longer identical** — it adopted the CB ladder. So
   instead it asserts what that decision was *for*: every prompt shared between CB
   and SRS must carry an identical presenter note. Last run: **82 shared prompts,
   0 drifting**, with a 1-entry allowlist (`Has non-safety changed?`, where CB's
   note is its pendulation timing dial and SRS has no such dial to inherit).

## Sources

| module | source of record |
|---|---|
| `mm` | `github.com/justinlmft/mindful-moment` → `index.html` |
| `cb` | `github.com/justinlmft/capacity-builder` → `index.html` |
| `srs` | `snb-business` → `Curriculum Advisor/Self-Regulation Simplified - Session Builder.html` |
| `ss`  | `snb-business` → `Curriculum Advisor/Safety Simplified.pdf` (203-slide Keynote export) |

**This tool does not replace the three standalone tools.** They stay live and
unchanged. Move over when you're ready, practice by practice.
