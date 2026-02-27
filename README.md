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

### 🥗 Nutrition — Fuel Tab
4 sub-tabs: **Today · Log Food · Supps · Targets**

**Today tab**
- Calorie ring + macro progress bars (protein / carbs / fat) with daily totals

**Log Food tab**
- **Barcode Scanner** — live camera viewfinder scans EAN-13, UPC-A, UPC-E, Code 128 and more; auto-looks up Open Food Facts → USDA FoodData Central fallback; scanned foods save to *My Foods* for one-tap re-add
- **Scan Label** — Claude Vision reads any Nutrition Facts or Supplement Facts label (photo) and extracts macros automatically
- **Smart food search** — type any food name; queries USDA FoodData Central (Foundation → SR Legacy → Branded, sorted by data quality); select a result → serving size dropdown with named portions ("1 medium apple", "1 cup sliced") + quantity multiplier; macros scale automatically
- **Quick Add** — alphabetical scrollable list combining your saved *My Foods* (with × remove) and 14 preset foods; calorie count shown on each item

**Supps tab**
- **My Stack** — daily supplement checklist; checking off records the dose; resets at midnight
  - *FUEL badge*: macro supplements (protein powder, etc.) auto-log calories + macros to daily totals
  - Timing color codes: amber=morning, lime=pre-workout, blue=post-workout, purple=bedtime
- Add supplements by barcode scan, label photo (Claude Vision), or manual entry
- **Recommended for 56yo male**: Creatine, D3+K2, Omega-3, Magnesium, Collagen Peptides, NAD+, Fiber, Whey Protein — hides items already in your stack
- **Micronutrient tracker** — 10 color-coded progress bars vs age-appropriate RDA targets, fed by both supplement check-ins and food barcode scans:
  Fiber · Vitamin D · Vitamin K · Omega-3 · Magnesium · Calcium · Zinc · B12 · Vitamin C · Potassium

**Targets tab**
- TDEE, daily calorie target, and macro breakdown calculated for your profile

### 🤖 AI Coach
Powered by Claude (Anthropic). The coach knows your full profile, exercise library, workout history, working weights, and all 5 cardio protocols. Requires an Anthropic API key stored locally — never committed to the repo.

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
├── index.html   # Single-file production app — deploy this
└── README.md    # This file
```

New features are implemented directly in `index.html` as additional `<script>` blocks. Push to `main` — GitHub Pages deploys in ~60 seconds.

---

## Roadmap

- [x] **Barcode scanner** — OFN + USDA dual-lookup, iOS Quagga2 fallback *(Sprint 1)*
- [x] **My Foods** — scanned foods saved for instant re-add *(Sprint 1b)*
- [x] **Smart food search** — USDA FoodData Central name search with serving size dropdown *(Sprint 1b / 4b)*
- [x] **Supplements tab** — My Stack checklist, macro FUEL tracking, Recommended stack *(Sprint 2)*
- [x] **Claude Vision label scanning** — photo any nutrition/supplement label to extract macros *(Sprint 3)*
- [x] **Micronutrient tracking** — 10 nutrients with age-appropriate RDAs, fed by supps + food scans *(Sprint 4)*
- [ ] **Cross-device sync** — Supabase (or GitHub Gist) so iPhone and iPad share the same data *(Sprint 3 in backlog)*
- [ ] **Mindfulness tab** — box breathing, 4-7-8, physiological sigh (animated guides), meditation timer *(Sprint 5)*
- [ ] **Fitbit integration** — steps, sleep score, resting HR via Fitbit Web API OAuth *(Sprint 6)*
- [ ] **Body metrics tracker** — weight trend graph, measurements, progress photos *(Sprint 7)*
- [ ] **Export** — workout history and nutrition log to CSV *(Sprint 8)*
- [ ] Gymnastic rings progression module

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
