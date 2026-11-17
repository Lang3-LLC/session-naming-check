# Session Naming Check

A free browser tool that reads a Pro Tools **Session Info as Text** export and reports the naming inconsistencies inside it — the same mic under two names, the duplicated take, the spelling that differs by one dot.

Built for 100+ track sessions handed between engineers.

**No account. No upload. No backend.** The file is read in the browser and never leaves the tab.

**Run it online:** [lang3.co/tools/session-check](https://lang3.co/tools/session-check/) — same code, same behaviour. Prefer that link if you just want to check a session; clone this repo only if you want to read the code, host your own copy, or send a patch.

---

## Why it exists

A name being ugly is not a problem. Two names disagreeing with each other is.

In a 110-track session the failures are not things like `79 tracks ending in cm_01` — that is verbose but consistent. The failures are `K` next to `KICK`, four tracks named `DBL A.end of ch1 from ch2` split into two L/R pairs, `Verse` next to `VERSE`.

None of it breaks the mix. All of it breaks the handoff — the moment you open someone else's session, or reopen your own six months later and can't tell which `K` is the kick.

---

## Use

1. In Pro Tools: **File → Export → Session Info as Text**. Tick *Include Track EDL's*.
2. Drag the `.txt` into the tool, or paste its text.
3. Read the report. Copy it as plain text, or print / save as PDF.

Everything runs in one tab. There is no request to any server — you can verify that by opening devtools and watching the network tab stay empty.

---

## What it reads — and what it can't

**Audio tracks only.**

That export does not contain aux, bus, folder, VCA, MIDI or instrument tracks, at any export setting. This was verified against three exports of the same 110-track session, one of which had two aux tracks, a master, a routing folder, a basic folder, a MIDI track and an instrument track newly added — the exported file was **byte-for-byte identical** (88,282 bytes, 1,835 lines, 110 tracks).

So this tool cannot judge routing, bus structure, plugin load or signal flow. A tool that inferred routing from this file would be guessing, and would look confident while doing it. That boundary is stated in the UI as well, not just here.

It also never writes to your session. Every suggestion is text you copy yourself.

---

## What it flags

| # | Check | Example |
|---|---|---|
| 1 | Abbreviation drift — same source under two names | `K` / `KICK`, `SN` / `SNARE`, `BASS` / `BASSG` |
| 2 | Near-duplicates — same name on separate tracks | `DBL A.end of ch1 from ch2.L` / `.R` |
| 3 | Duplicate take markers | `dup`, `dup1`, `dup2` mixed with no consistent position |
| 4 | Case inconsistency, token level | `Verse` / `VERSE`, `Low` / `LOW` |
| 5 | Doubled tokens | `cm.cm` — usually a name that grew by copy-paste |
| 6 | Excessive spaces | three or more spaces inside one name |
| 7 | Inconsistent suffixes | `clean` / `CLEAN`, `.L` / `.R` used inconsistently |
| 8 | Numbering | gaps in the sequence, two-digit and three-digit mixed |

Severity is split: issues that will bite during a session are marked **Fix first**, the rest **Notice**.

---

## What it deliberately does not flag

Most of what looks like chaos is consistent chaos. Early versions of this tool flagged all of the following. All of it was wrong:

- `Verse A` / `Verse B`, `Chords A` / `Chords B` — deliberate pairs, not duplicates
- `GTR CH A` / `GTR PCH A` — one token is a substring of the other, two different instruments
- 79 tracks ending in `cm_01` — verbose but internally consistent

The rule that came out of it: compare names **against each other**, not against an idea of what a good name looks like.

---

## Run it locally

Single file, no build step, no dependencies:

```sh
open index.html          # macOS
xdg-open index.html      # Linux
```

Or drag `index.html` into any browser. It works offline.

---

## Deploy your own

Any static host will do. On GitHub Pages: put `index.html` at the repo root, then Settings → Pages → deploy from branch.

If you host it publicly, keep the attribution line in the footer and point it at your own copy, not at `lang3.co`.

---

## Test data

The numbers used to develop this come from a real 110-track session (48 kHz, 32-bit float, 81 stereo / 29 mono, 382 clips, 7 issues). That session is a client-adjacent project and is **not** included in this repository.

If you want to contribute a fixture, strip the audio file paths and clip names first.

---

## Contributing

Issues and pull requests are welcome, particularly **false positives**. Knowing what the tool wrongly flags is more useful than another confirmation that it works.

When reporting a false positive, include the two (or more) track names as they appear in the export, and what you expected instead. Do not attach real session exports containing client material.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Trademarks

*Pro Tools* and *Avid* are trademarks of Avid Technology, Inc. This project is not affiliated with, endorsed by, or sponsored by Avid Technology, Inc.

*Lang3™*, *L3EQ™* and *L3DY™* are trademarks of Lang3 LLC. The MIT license covers the code, not the marks.
