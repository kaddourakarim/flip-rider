# MEMORY — Flip Rider (Game 101)

Project log for Karim + Léo's motorbike flip game. Read this together with CLAUDE.md at the start of every session.

## Status (2026-11-06)
- ✅ PRD written: `PRD.md`
- ✅ Game v1 built: `index.html` (single file, open it in a browser to play)
- ✅ Tested with automated runs (bots in the browser) on all 3 levels
- ⏳ GitHub: blocked. git needs the macOS Command Line Tools (`xcode-select --install`), and the GitHub CLI isn't installed
- ⏳ Playtest with Léo, then adjust sensitivity and cup targets

## Key decisions
- **Original game in the same genre**, not a copy of Elastomania: our own name (Flip Rider), art, levels and color palette, with the same simple flat style.
- **Arrow keys only:** ↑ gas, ↓ brake, ← backflip/lean back, → frontflip/lean forward. Menus also use only arrows. (Extras for the parent: M = mute, Esc = menu.)
- **One self-contained `index.html`** (canvas + Web Audio, no build step, no assets).
- **Score** = time points (1000 × (2 − time/par), min 0) + 300 per flip (+300 combo per extra flip in one jump) + 100 per star. **Every finish wins a cup** (bronze minimum).
- **Crash only when the head touches the ground.** Respawn at the last checkpoint after 1.2s, no game over.

## Kid-friendly tuning (in the `T` object in index.html)
- Lean eases in over ~0.25s (`ROT_RAMP: 4`) but **stops fast when released** (rate 12). Bug found: easing out made the bike over-rotate after the key was released.
- Spin speed cap `AIR_MAXW: 6.6` rad/s (about 1 flip per second).
- With no key held in the air, a **landing assist** (`AIR_ASSIST: 10`, `AIR_DAMP: 8`) stops the spin and lines the bike up with the slope below.
- On the ground: auto-straighten (`ASSIST`), wheels don't bounce.
- A flip counts at about 310° (`FLIP_SLACK: 0.9` rad).

## Lessons learned
- **Gas has to push the whole bike, not just the rear wheel.** Pushing only the rear wheel gave ~1/3 of the acceleration, and the bike got stuck forever on a ramp after a checkpoint respawn.
- **Checkpoints need a run-up before a ramp**, or a restart launches too slowly.
- Flips need ~1.3s+ of air; doubles need ~2.3s. Kickers are steep lips (`'k'` sharp points) followed by long downhill landings.
- A headless `window.__flip.sim(level, policy)` hook runs the physics synchronously. That's how we tune levels: gas-only, flip-bot and random "kid masher" policies.
- Browser preview of a `file://` page runs as a `data:` URL: localStorage is blocked there (the game handles it). Add `?v=N` to force a reload.
- This Mac has no Python and no Command Line Tools. Running `python3` or `git` pops up an install dialog.

## Test results (bots, after tuning)
| Level | Gas only | Single flips | Doubles | Random mashing |
|---|---|---|---|---|
| 1 Sunny Hills (par 14) | 8.1s, silver | 1 flip → gold | — | always finishes, silver/gold |
| 2 Jump Park (par 20) | 9.7s, silver | 3 flips → gold | — | always finishes |
| 3 Big Mountain (par 22) | 13.1s, bronze | 3 flips → silver | double on mega-ramp → gold | finishes with 0–6 crashes |

## Next ideas
- Léo mode (auto-gas, slower spin), more levels, two-player scoreboard, ghost replay.
