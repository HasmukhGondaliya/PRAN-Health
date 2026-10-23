# PRAN Health

Home fitness and pure-vegetarian nutrition for Gujarat, in Gujarati, Hindi and English. It's a single self-contained `index.html` with no build step, no backend, no account and no tracking.

## Deploy to GitHub Pages (existing repo `PRAN_health`)

1. Keep a copy of the current app first, for example by renaming the existing `index.html` to `legacy.html`.
2. Put this `index.html` (and this README) in the repository root and commit.
3. In the repository, go to **Settings → Pages** and set **Source: Deploy from a branch**, **Branch: `main` / root**.
4. Open `https://hasmukhgondaliya.github.io/PRAN_health/`.

You don't need to configure a base path. All navigation uses hash routes (`#/diet`, `#/exercise/squat`), and every illustration is drawn in code, so there are no asset paths to break under `/PRAN_health/`.

The only external request is Google Fonts (Baloo Bhai 2, Baloo 2, Mukta Vaani, Mukta). If it's blocked or the connection is slow, system fonts are used instead.

## Run locally

Open `index.html` directly, or serve it:

```
python3 -m http.server 8000   # then visit http://localhost:8000
```

## What's included

- **Home**:
  - Hero section with a start button.
  - Four quick-start workouts.
  - Today's suggestion, which follows any active program and alternates recovery after strength days.
  - Daily movement ring.
  - Eight goal cards.
  - A Gujarati thali spotlight.
  - Honest progress figures, which stay empty until you work out.
- **Exercise library**:
  - 49 exercises, each with original animated illustrations and numbered start/movement frames.
  - Search in all three languages, plus filters.
  - Detail pages covering steps, breathing, muscles (with a body map), easier and harder variations, mistakes, safety, suitability, alternatives, and benefits.
  - Each benefit is labelled by type (direct effect, general benefit, or depends on consistent training) and by evidence level, with linked references.
- **Workouts**:
  - Quick start.
  - A builder that takes age group, experience, goal, 5–45 minutes, intensity, equipment, space and limitations.
  - Eight programs with built-in rest days and a shorter-session option.
  - Saved workouts.
  - A "choose exercises" list builder.
- **Workout player**:
  - Nothing starts until you press Start, and sound stays off until you opt in.
  - Get-ready countdown, then work and rest timers with an automatic switch between them.
  - Pause/resume, skip, a Done button for repetition sets, and end.
  - Next-exercise preview and progress segments.
  - Full screen, and keeps the screen awake where the browser supports it.
  - Pauses automatically if you leave the tab.
  - Keyboard shortcuts (Space, N, M, F, Esc) and TV-remote arrow navigation.
  - "Continue later" resumes where you stopped.
  - The completion summary shows measured active time, planned versus actual time, skipped exercises and optional effort rating.
  - No calorie claims.
- **Vegetarian diet**:
  - Eight plan types.
  - Settings for dairy, onion/garlic and Jain-style root vegetables, plus five allergies, spice level, season and budget.
  - Five editable meal times.
  - Meal swaps chosen for similar protein and energy, and ingredient swaps (paneer↔tofu, dal↔dal, fruit by season).
  - Portion control.
  - Rounded nutrition estimates.
  - A protein guide and an optional calculator (adults only; your weight is not saved).
  - Notes on when to see a doctor or dietitian.
- **Progress**:
  - Weekly-minutes chart and month calendar.
  - Milestones and everyday-ability milestones.
  - Habits.
  - Optional private body measurements, off by default.
  - History, and a printable paper tracker.
- **Profile**:
  - Several people on one device (this keeps the old site's "couple mode").
  - Optional seven-step onboarding.
  - Accessibility settings: text size, theme, high contrast, reduced motion, TV mode.
  - Export, restore and delete your data.
- **Evidence and safety**: urgent-symptom guidance, stop rules, home safety, special groups, how claims are labelled, what the app never claims, the full reference list, and known limitations.

## Updating content

Everything is plain JavaScript inside `index.html`. Search for these names:

| What | Search for |
|---|---|
| Exercise text (steps, safety, benefits, cues) | `const EXINFO` |
| Exercise illustrations (pose angles, props) | `const POSES` |
| References | `const REFS` |
| Safety rules | `function unsuitable` |
| Workout generator, quick starts, programs | `function generate`, `const QUICK`, `const PROGRAMS` |
| Foods (values per 100 g raw), meals, swaps | `const FOODS`, `const MEALS`, `const SUBS`, `const PREFSWAP` |
| Diet plan types | `const PLANS` |
| Interface text | `I18N.en`, `I18N.gu`, `I18N.hi` (the same keys in each) |

## Privacy and storage

Data is stored in `localStorage` under keys starting `pran.v1.`, with a separate namespace for each person. If storage is unavailable (for example in private mode), the app works in memory and says so on the Profile page.

Nothing is sent anywhere. Data from the old "Prana Protocol" app is not migrated, because it used different storage.

## Testing done

Tested in headless Chromium at 390 px (phone) and at 1366 px desktop width:

- **Routes and errors:** every route loads in all three languages and in light and dark themes, with no script errors.
- **Workout player:** pause holds the timer, skip works in both work and rest phases, and "continue later" resumes correctly. Completed workouts are recorded and persist after a reload.
- **Durations:** the quick starts last 5.0, 10.0, 20.3 and 10.3 minutes.
- **Diet safety:** an automated check of 440 meal slots across 11 combinations of preferences and allergies found no forbidden ingredient.
- **Exercise safety:** onboarding saves correctly, and an older adult with knee and balance limitations gets no unsuitable exercises.
- **Navigation:** the TV arrow navigation and the Esc/back handling work.

## Known limitations

- Detailed exercise steps are in English. Names, key cues, safety messages and the whole interface are in Gujarati and Hindi. The translations should be reviewed by native-speaking health professionals.
- Nutrition values are rounded estimates of the kind published in the Indian Food Composition Tables 2017, not verified table entries. Check them against IFCT before any clinical use.
- Costs are shown as ₹ / ₹₹ / ₹₹₹ rather than prices, because prices vary by place and season.
- Full offline installation (a PWA) needs a separate `manifest.json` and service-worker file, which a single HTML page can't include. The page does work without a connection once it's open.
- The safety rules rely on what users tell the app. They are not a clinical assessment.
