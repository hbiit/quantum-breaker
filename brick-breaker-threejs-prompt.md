# Prompt: Build a Complete Brick Breaker Game with Three.js

## Role

You are a senior game developer experienced with Three.js and browser game architecture. Build a complete, polished, playable Brick Breaker (Breakout / Arkanoid-style) game. Deliver working code, not a sketch or a partial demo.

## Goal

Create a full 3D-styled Brick Breaker game that runs in the browser using **Three.js**, with **multiple playable levels** and the **classic power-ups** players expect from the genre.

## Tech Requirements

- **Three.js** for rendering (use an orthographic camera for a flat arcade feel, or a slightly tilted perspective camera for depth; pick one and justify it in a comment).
- Plain **JavaScript (ES modules)**, no heavy frameworks.
- Delivery format (choose one and state which):
  - **Option A (preferred for simplicity):** a single `index.html` that loads Three.js from a CDN with a pinned version (for example `https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js` through an import map), with CSS and JS inline or in separate files.
  - **Option B:** a Vite project with `npm install three`, split into modules.
- The game must run by opening the file or running a single command (`npm run dev`). Provide exact run instructions.
- Runs at a stable 60 FPS on a mid-range laptop. Use `requestAnimationFrame` with a **delta-time** based game loop (never tie physics to frame rate).
- No external image or audio files required. Generate visuals with Three.js geometry/materials and sounds with the Web Audio API (simple synthesized beeps), so the project is self-contained.

## Core Gameplay

1. **Paddle:** moves horizontally. Controls: mouse movement, left/right arrow keys, `A`/`D`, and touch drag on mobile.
2. **Ball:** launches from the paddle on click, tap, or `Space`. The launch angle and the bounce angle off the paddle depend on where the ball hits the paddle (edges give sharper angles; center goes straight up).
3. **Bricks:** arranged in a grid. Hitting a brick damages it; when its health reaches 0 it is destroyed and may drop a power-up.
4. **Walls:** left, right, and top boundaries reflect the ball. The bottom is open.
5. **Lives:** the player starts with 3 lives. The ball falling below the paddle costs one life and resets the ball to the paddle.
6. **Score:** points per brick (more for tougher bricks), with a **combo multiplier** for consecutive brick hits without touching the paddle.
7. **Win/lose:** clear all destructible bricks to complete the level; lose all lives for Game Over.
8. **States:** Main Menu, Level Select, Playing, Paused (`P` or `Esc`), Level Complete, Game Over, Game Complete (all levels beaten).

## Physics and Collision Requirements

- Use **swept or sub-stepped collision detection** so the ball never tunnels through bricks or the paddle at high speed.
- Resolve ball-vs-brick collisions by the **side of impact** (reflect X or Y correctly), including corner cases.
- Keep ball speed within a min/max range; speed increases slightly as bricks are destroyed or as levels progress.
- Prevent the ball from getting stuck in a perfectly horizontal bounce loop (enforce a minimum vertical velocity component).
- Handle multiple balls (needed for the Multi-ball power-up) with a clean array-based system.

## Levels

Implement **at least 10 levels** with increasing difficulty, defined as **data** (not hard-coded in logic), for example:

```js
const LEVELS = [
  {
    name: "First Steps",
    ballSpeed: 6,
    layout: [
      "..........",
      ".11111111.",
      ".12222221.",
      ".11111111.",
    ],
  },
  // ...
];
```

Brick type legend (extend as needed):

| Symbol | Brick type | Behavior |
|--------|-----------|----------|
| `.` | Empty | No brick |
| `1` | Normal | 1 hit |
| `2` | Strong | 2 hits, changes color when damaged |
| `3` | Tough | 3 hits |
| `#` | Indestructible | Cannot be destroyed (not required to clear the level) |
| `E` | Explosive | Destroys neighboring bricks when broken |
| `P` | Power-up brick | Always drops a power-up |

Level design requirements:
- Layouts must vary in shape (pyramid, checkerboard, walls with gaps, rings, letters, invaders-style patterns, and so on), not just rectangles.
- Difficulty ramps through: more rows, tougher bricks, indestructible blockers, faster ball, smaller paddle baseline in later levels.
- Include a **Level Select screen** where the player can choose any unlocked level. Unlock the next level on completion.
- Persist progress (highest unlocked level, high score per level) with `localStorage`, wrapped in try/catch.
- Show a level name banner at the start of each level.

## Power-Ups

Power-ups drop from destroyed bricks as falling capsules (colored, labeled with an icon or letter) that the paddle must catch. Use a drop chance of about 15 to 20% (configurable), and always drop from `P` bricks.

Implement all of the following:

| Power-up | Effect | Duration |
|----------|--------|----------|
| **Expand Paddle** | Paddle becomes wider | ~12 s |
| **Shrink Paddle** (negative) | Paddle becomes narrower | ~10 s |
| **Multi-ball** | Splits each ball into 3 | Until balls are lost |
| **Slow Ball** | Reduces ball speed | ~10 s |
| **Fast Ball** (negative) | Increases ball speed | ~10 s |
| **Fireball / Mega Ball** | Ball smashes through bricks without bouncing | ~8 s |
| **Sticky Paddle** | Ball sticks to the paddle on contact; click or `Space` to release | ~12 s |
| **Laser Paddle** | Paddle fires lasers (`Space` / click) that damage bricks | ~10 s |
| **Extra Life** | +1 life (capped at 5) | Instant |
| **Shield / Safety Net** | A barrier at the bottom saves the ball once | One use |

Power-up rules:
- Timed effects show a **HUD indicator with a countdown bar**.
- Collecting the same timed power-up again **refreshes** the timer rather than stacking incorrectly.
- Conflicting effects resolve sensibly (Expand and Shrink cancel each other; Slow and Fast cancel each other).
- All effects are cleanly removed when they expire, on life loss, and on level change (no leaked state).
- Define power-ups in a **data-driven registry** (type, color, icon, duration, `apply()` and `remove()` functions) so new ones are easy to add.

## Visuals and Game Feel

- Neon or clean modern 3D look: beveled box bricks, glowing emissive materials, a rounded paddle (capsule or box with bevel), a glowing sphere ball with a short **trail**.
- Lighting: ambient plus directional or point light; soft shadows are optional.
- A subtle animated background (starfield, grid, or gradient particles).
- **Particle bursts** when bricks break, in the brick's color.
- **Screen shake** on life loss and explosions (small and brief).
- Brick hit flash and smooth scale/fade on destruction.
- Smooth paddle movement (slight easing for keyboard control).
- Responsive canvas that resizes with the window and keeps the correct aspect ratio.

## Audio

Use the Web Audio API to synthesize sounds (no files needed):
- Paddle hit, brick hit, brick destroyed, wall bounce, power-up collected, life lost, level complete, game over.
- Provide a **mute toggle** (`M`), and respect the browser rule that audio starts only after a user gesture.

## UI / HUD

- Always visible: score, lives, current level, high score, active power-up timers.
- Menu screens built with HTML/CSS overlays (clean, centered, keyboard and mouse friendly).
- On-screen instructions for controls on the main menu.
- Pause overlay with Resume, Restart Level, and Quit to Menu.

## Code Quality and Architecture

Organize the code into clear modules or well-separated sections:

- `Game` (state machine and main loop)
- `Paddle`, `Ball`, `Brick`, `PowerUp` entities
- `LevelManager` (loads level data, handles unlocking and progress)
- `PowerUpManager` (spawning, collecting, timers)
- `Physics` / collision helpers
- `AudioManager`, `Particles`, `UI`
- A central `config` object for tunable values (speeds, sizes, drop chance, durations).

Requirements:
- Readable, commented code. Explain non-obvious logic (collision resolution, tunneling prevention).
- **Dispose** of Three.js geometries, materials, and textures when removing objects (no memory leaks across levels).
- Reuse geometries and materials between bricks where possible, or use `InstancedMesh` for performance.
- No global-variable chaos; no magic numbers outside `config`.

## Testing Checklist (verify before finishing)

Run through these and fix any failure before delivering:

- [ ] The game loads with no console errors or warnings
- [ ] Ball never passes through bricks, the paddle, or walls, even at max speed
- [ ] Every power-up works, expires correctly, and resets on level change
- [ ] Multi-ball works, and the life is lost only when the last ball falls
- [ ] All 10+ levels are completable, and the Level Select unlock logic works
- [ ] Indestructible bricks do not block level completion
- [ ] Pause, resume, and restart work from every relevant state
- [ ] Works with mouse, keyboard, and touch
- [ ] Resizing the window does not break gameplay
- [ ] `localStorage` failures (blocked storage) do not crash the game
- [ ] No memory growth after playing several levels in a row

## Deliverables

1. The complete, runnable source code (all files, in full, with no `// ...rest of code` placeholders).
2. A short **README** section covering: how to run it, controls, how to add a new level, and how to add a new power-up.
3. A brief list of any design decisions or assumptions you made.

## Stretch Goals (only after everything above works)

- Boss level with a large multi-hit brick structure
- Combo/score popups floating above bricks
- Gamepad support
- Level editor mode (click to place bricks, export as a layout array)
- Daily challenge or endless mode with procedurally generated levels
- Settings menu (volume, particle quality, screen shake toggle)

## Output Rules

- Do not ask clarifying questions unless something is truly blocking. Make reasonable choices and note them.
- Provide complete working code, not snippets or pseudo-code.
- Prioritize a **working, bug-free core** first, then polish.
