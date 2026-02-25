# FitLife App — Project Memory
> This file is the source of truth for Claude Code (and any new Claude session).
> Always read this first before making changes.

---

## User Profile
| Field | Value |
|-------|-------|
| Age | 56, male |
| Height/Weight | 5'10", 198 lbs |
| Experience | Intermediate (1–3 years consistent) |
| Goal | Fat loss + muscle preservation (moderate deficit) |
| Activity | Lightly active, desk worker — sits all day |
| TDEE | ~2,540 cal/day |
| Target Calories | 2,150 cal/day |
| Macros | 175g protein / 190g carbs / 65g fat |
| Max HR | 164 bpm (220–56) |
| Cardio HR Zone | 139–156 bpm (85–95% max HR) |

## Equipment
- Barbell + bench (**solo training — RPE 7 max, no grinding reps**)
- Dumbbells, kettlebells
- Pull-up bar (working toward pull-ups via dead hangs + band-assisted)
- Elliptical
- Punching bag / heavy bag

## Known Limitations / Injuries
- **Shoulder**: avoid heavy overhead pressing. Cuban press used for shoulder health/prehab
- **Hips**: chronically tight from desk work, can't touch toes — hip mobility is #1 priority
- **Lower back**: sensitivity noted, weak core/hamstrings as likely root cause
- Solo training safety: bench press, skull crushers, clean deadlift all flagged with RPE warnings

---

## Tech Stack
| Layer | Choice |
|-------|--------|
| Framework | React 18 (UMD via CDN) |
| Compilation | esbuild (pre-compiled — NO Babel at runtime) |
| Styling | Vanilla CSS via injected `<style>` tag, CSS custom properties |
| Storage | `localStorage` (key/value JSON) |
| Fonts | Bebas Neue (display), DM Sans (body), DM Mono (numbers) via Google Fonts |
| Hosting | GitHub Pages (`https://[username].github.io/fitlife`) |
| AI Coach | Anthropic Claude API (`claude-sonnet-4-20250514`) |
| File | Single `index.html` — fully self-contained |

## Critical Build Note
The app is written in JSX (`fitness-app.jsx`) and **must be compiled with esbuild** before
deploying. The `index.html` contains pre-compiled vanilla JS — no Babel, no build step at
runtime. This is what makes it fast on iPhone.

**Compile command (from project root):**
```bash
node compile.mjs   # or however esbuild is invoked
```
The compile script reads `fitness-app.jsx`, transforms JSX → `React.createElement()`,
prepends `const { useState, useEffect, useRef } = React;`, and outputs into `index.html`.

---

## App Structure — 6 Tabs

### 1. Home (Dashboard)
- Week strip calendar (trained days highlighted)
- Calorie ring + macro bars for today
- Stat grid: lifts this week, total sessions
- "Next Up" card showing next workout (A or B)
- Deload banner when triggered
- Motivational callout ("2 years vigorous exercise reversed 20 years of heart aging...")

### 2. Lift (Strength)
- **A/B Full Body program** — alternates each session
- **Main lifts** (consistent every session):
  - Workout A: Bench Press, Romanian Deadlift (DB), Goblet Squat (KB), Turkish Get-Up
  - Workout B: Clean Deadlift, Bulgarian Split Squat, KB Swing, Incline Bench Press (DB)
- **Accessories** (rotate automatically from pools each session):
  - Push pool: Diamond/Wide/Offset Push-Ups, Incline DB Fly, Bench Dips, Skull Crusher, Seated Tricep Extension, Cuban Press, Seated DB OHP, Box Jumps
  - Pull pool: Incline Row, Dead Hang/Band Pull-Up, Incline Bicep Curl, Farmer's Carry
  - Core pool: Plank, Flutter Kicks, Russian Twist, Bicycle Crunch, Wall Sit, Split Squat BW, KB Figure 8, Horse Stance, Single-Leg Balance, Jump Rope
- **Per-set logging**: weight + reps inputs, ✓ done / ✕ fail buttons per set
- **Rest timer**: auto-triggers after each set, visual countdown ring, vibrates at 3s
- **Progressive overload engine**:
  - Hit top of rep range 2 sessions in a row → suggest weight increase (2.5 or 5 lb)
  - 2 consecutive sessions with fails → suggest 10% deweight
  - User must confirm before changes apply
- **Deload system** (hybrid):
  - Scheduled: every 12 strength sessions (~4 weeks)
  - Performance-triggered: 3+ sessions with fails in last 4
  - During deload: all weights shown at 60%
- **Exercise demo modal**: every exercise has ▶ button → opens MusclesWiki page + YouTube link + coaching cue + solo safety note where applicable
- Sub-tabs: Log Workout | History | Warm-up | Library

### 3. Cardio
5 protocols, each with interval checklist + session timer:
1. **Norwegian 4×4** — 4 × 4 min at 139–156 bpm, 3 min recovery. Primary VO₂ max protocol.
2. **Heavy Bag HIIT** — 5 × 3 min boxing rounds, 1 min rest
3. **KB Complex Circuit** — 4 rounds: Swing→Clean→Press→Goblet Squat, no rest between exercises
4. **Huberman Assault Bike** — 20-sec sprints every 2 min over 30 min (from user's bookmarks)
5. **Japanese Interval Walk** — 3 min brisk / 3 min easy × 5 (from user's bookmarks)

### 4. Move (Mobility)
6 sub-sections, all with direct YouTube/link buttons:
- **Morning**: 5 Stretches Every Morning, CARs, Angle Toe Touches, Seated Toe Touch
- **Pre-Workout**: Limber 11 (featured), 5-Min Squat Mobility, CARs
- **Hips**: Lying Crossover, Seated Pretzel, Leg Tuck, Cobra, Windshield Wipers, Tight Hips Fix, 6 Exercises If You Sit a Lot
- **Upper**: BJJ Shoulder Stretch, Reach-Up Side Bend, Wrist Flexion, Neck Exercises, Fix Your Posture
- **Back**: Lower Back Exercises, Back/Joint Routine, Calf Wall Stretch, Thoracic Rotation
- **Feet**: Plantar Fasciitis Prevention, Plyometric Tendon Work, Foot & Ankle Strength

### 5. Fuel (Nutrition)
- Daily calorie ring + macro progress bars
- Quick-add food library (14 common foods)
- Custom food entry (name, cals, protein, carbs, fat)
- Targets view with calculated macros
- Today's logged items list

### 6. Coach (AI)
- Claude API (`claude-sonnet-4-20250514`) with full user profile in system prompt
- Knows: exercise library, workout history, current working weights, all 5 cardio protocols, mobility routine
- Suggested prompts pre-loaded
- Full conversation history sent each request

---

## Data Storage (localStorage keys)
| Key | Value |
|-----|-------|
| `history` | Array of session objects `{id, date, type, workout, name, isDeload, sets, exercises}` |
| `weights` | Object `{exerciseId: lastUsedWeight}` |
| `nutrition` | Array of daily logs `{date, cals, protein, fat, carbs, items[]}` |

---

## Exercise Library (full list by category)

### Main Lifts
`bench_press`, `rdl_db`, `goblet_squat`, `turkish_getup`, `clean_deadlift`, `bulgarian_split`, `kb_swing`, `incline_bench_db`

### Push Accessories
`push_diamond`, `push_wide`, `push_offset`, `incline_fly`, `bench_dip`, `skull_crusher`, `seated_tri`, `cuban_press`, `seated_ohp`, `box_jump`

### Pull Accessories
`incline_row`, `dead_hang`, `incline_curl`, `farmers_carry`

### Core & Stability
`plank`, `flutter_kicks`, `russian_twist`, `bike_crunch`, `wall_sit`, `split_squat_bw`, `kb_figure8`, `horse_stance`, `single_leg_bal`, `jump_rope`

---

## Design System
```
Colors:
  --bg:      #090909  (background)
  --s1:      #101012  (card background)
  --s2:      #161619  (secondary surface)
  --s3:      #1e1e23  (input background)
  --s4:      #26262d  (elevated surface)
  --lime:    #c8f135  (primary accent — buttons, active states)
  --blue:    #3fb8f5  (info, YouTube links)
  --red:     #f55a3f  (cardio, fail states, warnings)
  --amber:   #f5a83f  (deload, safety notes, caution)
  --purple:  #b06af5  (accessories badge)
  --teal:    #3ff5c8  (main lifts badge, success)
  --pink:    #f53f8a  (morning mobility)
  --text:    #f0f0f0
  --t2:      #9090a0  (secondary text)
  --t3:      #55555f  (muted text)

Typography:
  Display:  Bebas Neue (page titles, large numbers)
  Body:     DM Sans (all UI text)
  Mono:     DM Mono (weights, reps, timers)

Spacing: --r: 14px (card radius), --rs: 9px (small radius), --nav: 68px
```

---

## Inspiration / Bookmarks (from user's Raindrop.io)
- **@kneeovertoesguy** — ATG training, knee over toe for box jumps and split squats
- **@Asgooch** — Single KB full body training and complexes
- **Peter Attia** — VO₂ max, atherosclerosis, longevity metrics
- **Huberman Lab** — Assault bike protocol, Friday HIIT
- **@5060fit** — Multi-plane training, plyometrics for tendon health
- **Joe DeFranco** — Limber 11 (featured warm-up)
- **Norwegian 4×4** — Primary cardio protocol, deeply researched by user
- **BJJ/martial arts interest** — Influences KB training style and punching bag use

---

## Sprint History

### Sprint 1 — Camera + Barcode Nutrition ✅ DONE
- [x] Native `BarcodeDetector` API (iOS 17+ / Chrome 83+) — no external library, zero added page weight
- [x] Open Food Facts API (free, no key) — EAN-13, UPC-A, UPC-E, Code 128, Code 39, ITF
- [x] Full-screen camera viewfinder with lime targeting frame + corner accents
- [x] Per-100g nutrition display + adjustable gram input with live scaled preview
- [x] One-tap add to daily log, rescan on key change (React remount pattern)
- [x] Graceful fallbacks: camera denied, barcode not in DB, iOS < 17 / unsupported browser
- [x] `BarcodeScanner` + `NutritionTab` override injected as second `<script>` block (lines 779-1206)
- [x] `function NutritionTab(){}` override works because function declarations at top-level scripts replace the global binding — App looks up `NutritionTab` by name at render time
- **Branch workflow**: `dev/sprint-1` → merge to `main` → `git push`

## Next Sprint Backlog

### Sprint 2 — Mindfulness Tab
- [ ] New 7th tab: **Mind** (brain/lotus icon)
- [ ] Breathing exercises: Box breathing (4-4-4-4), 4-7-8, Physiological sigh (Huberman)
  - Animated visual guide (expanding/contracting circle)
  - Configurable rounds
- [ ] Meditation timer: simple countdown with ambient option
- [ ] Consider: guided sessions via text (Claude API) for pre-workout focus or post-workout recovery
- [ ] Log mindfulness sessions to history

### Sprint 3 — Fitbit Integration
- [ ] Fitbit Web API (OAuth 2.0) — requires user to authorize app
- [ ] Pull daily steps, active minutes, sleep stages/score, resting HR
- [ ] Display on Dashboard: sleep score card, steps ring, resting HR trend
- [ ] Use sleep score to influence workout recommendations (poor sleep → suggest lighter session)
- [ ] **Constraint**: Fitbit OAuth requires a registered app at dev.fitbit.com and a redirect URI — GitHub Pages URL works as redirect URI
- [ ] Consider: Fitbit intraday HR data could auto-detect cardio zone during Norwegian 4×4

### Future Ideas
- [ ] Gymnastic rings progression (user has interest per bookmarks)
- [ ] RPE auto-calculator based on reps in reserve
- [ ] Body weight / measurements tracker
- [ ] Progress photos (camera)
- [ ] Export workout data to CSV
- [ ] Share workout summary card (image)
- [ ] 2-set high-frequency option (Dorian Yates style — bookmarked by user)

---

## Files
| File | Purpose |
|------|---------|
| `index.html` | Production app — pre-compiled, deploy this to GitHub Pages |
| `MEMORY.md` | This file — project context for Claude |
| `README.md` | Public-facing description (GitHub Pages) |

> Note: `fitness-app.jsx` source is not tracked in git. New features are injected as additional `<script>` blocks after line 777 (main compiled app) and before line 1208+ (mount script). Each sprint adds a clearly labeled block.

## Git Workflow
- Branch: `dev/sprint-N` for each sprint
- After implementation: `git add -p` → commit on dev branch → merge to `main` → `git push`
- GitHub Pages auto-deploys from `main` within ~60 seconds

---
*Last updated: February 2026 — Sprint 1 complete (barcode scanner)*
