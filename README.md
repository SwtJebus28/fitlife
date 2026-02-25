# 💪 FitLife

A personal fitness PWA (Progressive Web App) built for iPhone home screen installation. Covers strength training, cardio, mobility, nutrition tracking, and an AI coaching tab — all in a single self-contained HTML file.

> Built for a 56-year-old intermediate lifter training solo at home with barbells, dumbbells, kettlebells, pull-up bar, elliptical, and a punching bag.

---

## Features

### 🏋️ Strength Training
- **A/B Full Body Program** — alternating workouts with consistent main lifts and auto-rotating accessories so sessions never feel stale
- **Per-set logging** — individual weight and reps per set, with ✓ done and ✕ fail tracking
- **Rest timer** — auto-triggers after each set with a visual countdown ring and vibration alert
- **Progressive overload engine** — analyzes your last 2 sessions and suggests weight increases or deweights, requiring your confirmation before applying
- **Deload scheduling** — hybrid system: scheduled every ~4 weeks or performance-triggered when fails accumulate
- **Exercise demo modal** — every exercise has a ▶ button showing a MusclesWiki animated demo, YouTube tutorial link, coaching cue, and solo training safety notes where relevant
- **Full exercise library** — 30+ exercises across main lifts, push, pull, and core categories

### ❤️ Cardio — 5 Protocols
| Protocol | Description |
|----------|-------------|
| Norwegian 4×4 | 4 × 4 min at 85–95% max HR — best-researched VO₂ max protocol |
| Heavy Bag HIIT | 5 × 3 min boxing rounds |
| KB Complex Circuit | Single kettlebell back-to-back movements |
| Huberman Assault Bike | 20-sec sprints every 2 min |
| Japanese Interval Walk | 3 min brisk / 3 min easy × 5 |

Each protocol includes an interval checklist and session timer.

### 🧘 Mobility — Hip Priority
A dedicated Mobility tab with 6 sections built specifically around desk-worker hip tightness:
- **Morning routine** (daily, even on rest days)
- **Pre-workout** — Joe DeFranco's Limber 11 featured
- **Hip & desk-worker** stretches
- **Upper body & shoulder** (BJJ shoulder stretch, posture correction)
- **Lower back & spine**
- **Foot care & tendon health**

All entries link directly to YouTube demos or source content.

### 🥗 Nutrition
- Daily calorie ring and macro progress bars (protein / carbs / fat)
- Quick-add library of 14 common foods
- Custom food entry
- Calculated targets: 2,150 cal · 175g protein · 190g carbs · 65g fat

### 🤖 AI Coach
Powered by Claude (Anthropic). The coach knows your full profile, exercise library, workout history, working weights, and all 5 cardio protocols. Ask it anything about programming, progression, mobility, or nutrition.

### 📷 Barcode Scanner *(Sprint 1)*
Camera-based food barcode scanning built into the Fuel tab:
- Tap **Scan Food Barcode** in Log Food to open the live camera viewfinder
- Point at any packaged food barcode — auto-detects EAN-13, UPC-A, UPC-E, Code 128, and more
- Nutrition data (calories, protein, carbs, fat) auto-fills from [Open Food Facts](https://world.openfoodfacts.org/) — free, no API key required
- Adjust serving size in grams — scaled nutrition updates in real time
- One tap to add to your daily log
- Falls back gracefully if barcode is not in the database or camera is unavailable
- Uses native `BarcodeDetector` API (iOS 17+ / Chrome 83+) — no external library, no extra load time

---

## Installation on iPhone

1. Open `https://[username].github.io/fitlife` in **Safari**
2. Tap the **Share button** → **Add to Home Screen**
3. Tap **Add**

The app opens full-screen with no browser chrome, just like a native app. All workout data, weights, and nutrition logs persist in your device's local storage.

> The AI Coach tab requires an internet connection. Everything else works offline once loaded.

---

## Tech Stack

- **React 18** (UMD build via CDN)
- **Pre-compiled JSX** via esbuild — no Babel runtime, loads instantly on mobile
- **localStorage** for all data persistence
- **Anthropic Claude API** for the AI Coach
- **Google Fonts** — Bebas Neue, DM Sans, DM Mono
- **MusclesWiki** — exercise demo GIFs
- Deployed via **GitHub Pages** as a single `index.html`

---

## Project Structure

```
fitlife/
├── index.html          # Production app (pre-compiled, deploy this)
├── fitness-app.jsx     # Source JSX (edit this, then compile)
├── MEMORY.md           # Project context and sprint backlog for Claude
└── README.md           # This file
```

### Editing & Deploying

The app is written in JSX (`fitness-app.jsx`) and compiled to vanilla JS before deployment. To make changes:

1. Edit `fitness-app.jsx`
2. Compile JSX → JS using esbuild
3. The output replaces the `<script>` block in `index.html`
4. Push to GitHub — Pages auto-deploys in ~60 seconds

---

## Roadmap

- [x] **Barcode scanner** — camera-based food barcode scanning with Open Food Facts API lookup *(Sprint 1)*
- [ ] **Mindfulness tab** — box breathing, 4-7-8, physiological sigh (animated guides), meditation timer
- [ ] **Fitbit integration** — pull steps, sleep score, and resting HR via Fitbit Web API (OAuth)
- [ ] Body weight / measurements tracker
- [ ] Gymnastic rings progression module
- [ ] Export workout data to CSV

---

## Acknowledgements

- [Joe DeFranco's Limber 11](https://www.youtube.com/watch?v=FSSDLDhbacc) — featured warm-up routine
- [Norwegian 4×4 Protocol](https://norwegian4x4.com) — primary cardio methodology
- [MusclesWiki](https://muscleswiki.com) — exercise demonstrations
- [Knee Over Toe Guy](https://x.com/kneeovertoesguy) — ATG training principles
- [Peter Attia](https://peterattiamd.com) — longevity and VO₂ max research
- [Huberman Lab](https://hubermanlab.com) — training and recovery protocols

---

*Personal project — built with Claude (Anthropic)*
