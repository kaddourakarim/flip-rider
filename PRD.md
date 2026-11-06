# Flip Rider — Product Requirements Document

**Owner:** Karim  · **Players:** Karim + Léo (age 5)  · **Date:** 2026-11-06  · **Status:** v1 built

---

## 1. Summary

Flip Rider is a small browser game: a side-view, physics-based motorbike ride across hilly 2D terrain. You hold the gas, lean back and forth, launch off ramps, do backflips and frontflips in the air, and cross the finish line to win a **cup** (bronze, silver or gold). Your score combines **how fast you finished** with **how many flips you landed**.

It belongs to the classic "elastic motorbike" physics-game genre, with an **original** name, art, levels and color palette. The look is deliberately simple: flat colors, bold shapes, no clutter.

## 2. Goals

| # | Goal | How we know it worked |
|---|------|----------------------|
| G1 | A 5-year-old can play without help after a 1-minute demo | Léo finishes Level 1 on his own |
| G2 | Arrow keys only | No other key is needed to play, retry or move between levels |
| G3 | Gamified and rewarding | Every finish gives a cup, flips show a big "+points" popup |
| G4 | Simple and fast to open | One `index.html` file, opens in any browser, nothing to install |
| G5 | Fun for a parent too | Gold cups and double flips are hard enough to be a real challenge |

### Non-goals (v1)
- Online leaderboards, accounts or multiplayer
- Level editor
- Mobile/touch controls
- Recreating any existing game's levels, art or branding

## 3. Players

- **Léo (5):** can't read much yet, presses keys for a long time, mashes, gets frustrated by instant failure. Needs big visuals, sounds, forgiving controls and a reward every time.
- **Karim (adult):** wants a quick, satisfying challenge and to beat Léo's score (or let him win).

## 4. Controls (arrow keys only)

| Key | On the ground | In the air | In menus |
|-----|---------------|------------|----------|
| ↑ Up | Gas (go forward) | — | Start / Play again |
| ↓ Down | Brake | — | — |
| ← Left | Lean back (wheelie) | **Backflip** spin | Previous level / back to menu |
| → Right | Lean forward | **Frontflip** spin | Next level |

Extra (parent-only, not needed to play): `M` mutes sound, `Esc` returns to the menu.

### Kid-friendly sensitivity (key requirement)
- **Soft ramp-up:** leaning and gas ease in over ~¼ second, so a quick tap barely moves the bike; you have to *hold* the arrow.
- **Spin speed cap:** the bike can't spin faster than about one flip per second, so it never spins out of control.
- **Auto-steady:** with no lean key pressed, the bike gently straightens itself on the ground and slows its spin in the air.
- **Soft landings:** wheels don't bounce.
- **Forgiving crashes:** a crash only happens when the rider's **head** touches the ground. After a crash the bike reappears at the last checkpoint after ~1 second. No "game over".
- **Forgiving flips:** a flip counts once you've turned about 310° (not a perfect 360°).

## 5. Gameplay

1. **Menu:** pick a level with ← / →, press ↑ to start.
2. **Ready:** bike waits at the start; "Press ↑ to GO!". The timer starts on the first ↑.
3. **Ride:** hills, kicker ramps with downhill landings (long air time = flips), collectible ★ stars, checkpoint flags.
4. **Finish line:** the timer stops, a score card counts up, and a cup appears with confetti.
5. **After the finish:** ↑ play again · → next level · ← menu (keys are ignored for the first 1.5 s so a held arrow doesn't skip the cup).

### Levels (v1)
| # | Name | Feel |
|---|------|------|
| 1 | Sunny Hills | Gentle bumps and one friendly jump. Made for Léo. |
| 2 | Jump Park | A staircase of kicker ramps. Single flips. |
| 3 | Big Mountain | Starts high, huge jumps. Double flips possible. |

## 6. Scoring and cups

```
Time points  = 1000 × (2 − time ÷ par time)   (minimum 0; finishing at par = 1000)
Flip points  = 300 per flip; landing 2+ flips in one jump earns a +300 combo bonus per extra flip
Star points  = 100 per star
Total score  = Time + Flips + Stars
```

| Cup | Rule |
|-----|------|
| 🥇 Gold | Total ≥ level's gold target |
| 🥈 Silver | Total ≥ level's silver target |
| 🥉 Bronze | Any finish (every finish earns a cup) |

The best score and best cup per level are saved in the browser and shown on the menu.

## 7. Look and feel

- **Simple flat style:** a bright sky gradient, soft parallax hills, green grass edge on brown dirt, a chunky orange bike and a rider in a yellow helmet.
- **Page:** a deep navy page with a big, playful title, the game canvas in a rounded frame, and a row of large key "chips" explaining the controls with pictures, so it works for a child who can't read yet.
- **Feedback:** "BACKFLIP! +300" popups, star sparkles, a "Oops!" on crash, a progress bar to the finish, engine sound that follows your speed, cheerful jingles.
- Original palette and art, not copied from any existing game.

## 8. Technical

- A single self-contained `index.html` (HTML + CSS + JavaScript, canvas 2D). The only external resource is Google Fonts (works offline with fallback fonts).
- **Physics:** a Verlet "soft" bike: 3 masses (rear wheel, front wheel, rider head) joined by stiff links. Wheels roll on a smoothed terrain line. Fixed 120 Hz physics step for consistent feel on any computer.
- **Flip detection:** add up the bike's rotation while both wheels are off the ground; count full turns on landing; cancel if the rider crashes right after landing.
- **Sound:** Web Audio API, generated in code (no audio files).
- **Storage:** `localStorage` for best scores (the game still works if storage is blocked).
- **Target:** latest Chrome/Safari/Firefox on a laptop with arrow keys.

## 9. Success checklist (v1)

- [x] Plays with arrow keys only
- [x] Left = backflip, Right = frontflip
- [x] Gentle, non-twitchy controls
- [x] Score = speed + flips (+ stars)
- [x] Cup screen at the end of every race
- [x] 3 levels from easy to hard
- [ ] Playtested with Léo; adjust sensitivity and cup targets

## 10. Ideas for later

- "Léo mode" toggle: even slower spin, auto-gas
- More levels, night theme, choose bike color
- Two-player "take turns" scoreboard (Papa vs Léo)
- Ghost replay of your best run
