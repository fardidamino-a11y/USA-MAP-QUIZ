# Can You Master All 50 States?

A browser geography game for students, families, trivia fans and YouTube audiences. Plain HTML, CSS and JavaScript: no framework, no backend, no accounts, no paid APIs.

## What's in the game

| Mode | What players do |
|---|---|
| **A · Name All 50** | Click a state and type its name, or just type any state name and it fills in. Untimed or 5 min / 2 min / 1 min. Reveal button when stuck; first-letter hint on Beginner. |
| **B · Spot the State** | A state is highlighted; pick its name from four choices. Less familiar states and neighbouring-state distractors appear as difficulty rises. |
| **C · State Capitals** | "Capital of X?", "Y is the capital of which state?" and (Advanced+) click the state on the map. Separate no-score **Capitals drill**. |
| **D · State Flags** | Identify the state from its flag. Look-alike seal flags are used as distractors on hard levels. |
| **E · Physical Geography** | 52 questions on rivers, mountains, lakes, deserts and landforms, clearly labelled as physical geography. Location questions are answered on the map. |
| **F · Ultimate USA Challenge** | Five rounds of five: states, capitals, flags, physical geography, then an Expert final. |
| **Play Along With Me** | Presenter mode for recording: question → countdown → "Time's up" pause → reveal with explanation → outro screen with your channel name and game link. |
| **Practice / Study** | Retry missed questions until correct; explore every state's capital, region, neighbours and flag. |

**Difficulty** (Beginner, Intermediate, Advanced, Expert) changes question selection, hints and timing. It never changes the facts.

**Scoring:** 100 per correct answer (50 if a hint was used), a speed bonus of up to +50 only in timed levels, and a streak bonus of up to +50. Untimed modes never penalise taking your time. Personal bests, preferences and the last set of missed questions are stored in `localStorage` only.

**Accessibility:** every action works from the keyboard (A–D or 1–4 to answer, Enter for next, H for hint, Esc to pause). Map-click questions also offer a state list, and Name All 50 accepts typed names with no clicking. Correct and incorrect use different colours *and* ✓ / ✗ marks. Animations respect `prefers-reduced-motion`. The game auto-pauses timed questions when the tab is hidden.

## Run it locally

No build step is needed to play.

- **Easiest:** open `index.html` in any modern browser (double-click it).
- **Or serve the folder:** `npx serve .` (or `python3 -m http.server 8080`) and open the printed address.

## Deploy

The game is a static site. Upload the folder (at least `index.html`, `css/` and `js/`) to any static host: GitHub Pages, Netlify, Vercel, Cloudflare Pages, or your own web server.

Prefer a single file? Run `npm install` then `npm run build`, and upload `dist/index.html` on its own.

**Direct links:** add a hash to open a mode's setup screen directly, for example `…/index.html#map`, `#highlight`, `#capitals`, `#flags`, `#physical`, `#ultimate`, `#playalong`, `#study`, `#practice`. These work well in a YouTube description or pinned comment.

## Using it for YouTube

1. Open **Play Along With Me**, choose a category, difficulty, number of questions and thinking time.
2. Enter your channel name and the public link to the game; both appear on the outro screen.
3. Turn on presentation mode (the full-screen button) and start your screen recorder.
4. Press **Space** or **→** to reveal and advance, **P** to pause the countdown, **H** to hide the controls.

A regular YouTube video cannot run the game inside the player. Viewers play along by pausing the video, and play for real by following your link to the browser game.

## Project layout

```
index.html              Page shell; loads data first, then game code
css/styles.css          All styles (design tokens at the top)
js/data/states.js       50 states: name, postal code, capital, region, familiarity tiers, distractor cities
js/data/physical.js     Physical geography questions
js/data/map-data.js     GENERATED state shapes, label points and neighbours
js/data/flags-data.js   GENERATED state flag SVGs
js/storage.js           localStorage wrapper (bests, preferences)
js/audio.js             Synthesised sound effects (Web Audio, no files)
js/map.js               Interactive SVG map: selection, zoom/pan, call-outs, tooltips
js/questions.js         Question selection, distractors and difficulty rules
js/app.js               Screens, game engines, results, practice, Play Along
scripts/                Data generators, data checks and the single-file bundler
```

### Adding content

- **New physical geography question:** add an object to `js/data/physical.js` (`kind: 'state'` for a map answer, or `kind: 'choice'` with three wrong `options`). Run `npm run verify`.
- **New category** (nicknames, time zones, national parks…): add a data file in `js/data/`, add a builder in `js/questions.js` that returns question objects (same shape as the existing ones), and add a card to `MODES` in `js/app.js`. Scoring, results, practice and Play Along work with any question that follows the shape.

### Regenerating data

```
npm install
npm run build:data   # rebuild map-data.js and flags-data.js
npm run verify       # 50 states, no D.C./territories, shapes + flags present, capitals cross-checked
npm run build        # single-file dist/index.html
```

## Data sources and licences

- **State boundaries:** [us-atlas](https://github.com/topojson/us-atlas) v3 (`states-albers-10m`), derived from U.S. Census Bureau cartographic boundary files. ISC licence. Albers USA projection with Alaska and Hawaii insets. D.C. is removed at build time.
- **Flags:** SVGs from the [us-state-flags](https://www.npmjs.com/package/us-state-flags) package (ISC licence). State flags are official government designs. The set includes Minnesota's 2024 flag, Utah's 2024 flag and Mississippi's 2021 flag. Massachusetts and Illinois have been studying redesigns; if a state adopts a new flag, replace its SVG and run `npm run build:flags`.
- **Capitals:** cross-checked by `npm run verify` against the reference list shipped in us-state-flags.
- **Physical geography:** elevations and facts follow U.S. Geological Survey and National Park Service figures; each question carries its own explanation.
- **Fonts:** Big Shoulders Display, Overpass and Atkinson Hyperlegible from Google Fonts (SIL Open Font Licence), with system fallbacks so the game works offline.
