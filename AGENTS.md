# AGENTS.md

Vanilla JS Asteroids clone. No build step, no dependencies, no tests, no lint/typecheck.

## Run

Open `index.html` directly, or `npx serve .` then visit `http://localhost:3000`.

## Architecture

- Entire game lives in `game.js` (single file, ES6+, `'use strict'`).
- `index.html` only hosts a fixed `<canvas id="canvas" width="800" height="600">` and loads `game.js`.
- All game state is module-level `let`/`const` in `game.js` (`ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`, `state`).
- `state` machine: `'playing' | 'dead' | 'gameover'`. `initGame()` resets everything; `nextLevel()` clears bullets/particles and respawns.
- Space is toroidal: every moving object wraps via `wrap(v, max)`.
- Loop is `requestAnimationFrame` with `dt` capped at `0.05` s (game.js:415).

## Gotchas

- Canvas size `W=800` / `H=600` is hardcoded in **both** `index.html` (canvas attrs) and `game.js` (constants). Changing one without the other breaks layout/wrap math.
- `justPressed` (game.js:10) makes `pressed(code)` return true only the frame a key goes down — use it for one-shot actions (shoot, restart). Use `keys[code]` for held actions (rotate, thrust).
- README mentions "power-ups especiales" and "estrella fugaz" — **not implemented** in code. Trust the code over the README.
- UI text (HUD, overlays) is in Spanish.
