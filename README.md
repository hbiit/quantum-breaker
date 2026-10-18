# Quantum Breaker 3D (start.ng Arcade)

A futuristic 3D cyberpunk brick breaker powered by **Three.js** and procedural **Web Audio API** sound synthesis. It delivers fluid 60 FPS gameplay, swept sub-stepped physics collision handling, 12 quantum sectors, 10 classic power-ups, native **start.ng** player identity cards, and global leaderboard synchronization.

- **Play Live on start.ng:** `https://quantum-breaker.start.ng`
- **GitHub Repository:** `https://github.com/hbiit/quantum-breaker`

---

## 🚀 How to Run the Game

### Option 1: Direct File Launch (Zero Install)
You can directly open `index.html` in any modern web browser (Google Chrome, Microsoft Edge, Firefox, Brave, Safari).
```
Double-click index.html or open it with your browser
```

### Option 2: Run via Dev Server
```bash
# Inside the directory:
npm run dev
# or
npx serve -l 3000 .
```
Then open `http://localhost:3000` in your browser.

---

## 🎮 Controls

| Action | Keyboard / Mouse | Touch / Mobile |
| :--- | :--- | :--- |
| **Move Paddle** | Mouse pointer OR `A` / `D` OR `Left` / `Right` Arrow keys | Touch and drag paddle horizontally |
| **Launch Ball** | `Space` OR Left Click | Tap screen |
| **Release Sticky Ball** | `Space` OR Left Click | Tap screen |
| **Fire Lasers** | `Space` OR Left Click | Tap screen |
| **Pause / Resume** | `P` OR `Escape` | Tap ⏸ button in the HUD |
| **Mute / Unmute** | `M` key | Tap 🔊 button in the HUD |

---

## 💎 Power-Up System (10 Classic Abilities)

Power-up capsules drop from destroyed bricks (22% base chance, 100% on `P` gold bricks):

1. **Expand Paddle (`↔` / Cyan):** Expands paddle width by 45% (12s).
2. **Shrink Paddle (`⇥⇤` / Orange):** Shrinks paddle width to 68% (10s negative challenge).
3. **Multi-ball (`×3` / Gold):** Triples every active ball in play at divergent angles (until balls drop).
4. **Slow Motion (`🐢` / Green):** Slows ball speed to 72% for precision targeting (10s).
5. **Hyper Speed (`⚡` / Magenta):** Accelerates ball speed by 135% (10s challenge).
6. **Mega Fireball (`🔥` / Fiery Red):** Smashes continuously through all bricks without deflecting (8.5s).
7. **Magnetic Catch (`🧲` / Lime):** Ball sticks to paddle on impact; launch at your chosen angle (12s).
8. **Plasma Cannons (`🔫` / Bright Pink):** Paddle mounts twin blasters firing laser bolts on click/Space (10s).
9. **Extra Life (`+1` / Ruby Red):** Restores +1 heart (capped at 5).
10. **Safety Barrier (`🛡️` / Electric Blue):** Laser net at the bottom that saves a dropped ball once.

---

## 🌐 start.ng Integration

- **Player Identity:** Automatically greets signed-in start.ng players, displays their custom character avatar, name, and `@account` in the top profile card.
- **Global Leaderboard:** Live 3-column leaderboard (`score`, `sector`, `combo`) defined in `startng.json` synchronized via the start.ng connect iframe API.
- **Guest Support:** Unregistered guests can immediately play or click "Sign In" to preserve scores.

---

## 📐 Design Decisions & Architecture

1. **Tilted Perspective Camera (`fov: 52`, ~62° angle):**
   - Delivers genuine 3D beveled edges, specular highlights, and glowing neon depth, while strictly keeping ball and paddle movement along a 2D plane for clean, intuitive arcade accuracy.
2. **Sub-Stepped Physics Loop (`subSteps: 6`):**
   - Each frame's delta time is broken into 6 sub-steps to completely eliminate high-speed ball tunneling through thin paddle boundaries, corners, or bricks.
3. **Continuous Minimum Vertical Velocity (`|vy| >= 5.5`):**
   - Eliminates tedious horizontal bounce loops between side walls.
4. **Procedural Web Audio API:**
   - 100% self-contained sound effects with dynamic pitch scaling that increases as combos build up. No external audio files needed.
