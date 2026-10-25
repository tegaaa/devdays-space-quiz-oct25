# Copilot instructions

## Project

French-language quiz on space exploration, delivered as a single self-contained `index.html` (HTML, CSS and JS inline). The folder is not a git repository.

## Build, test, lint

None. There is no build step, package manager, test runner or linter. Open `index.html` directly (`file://`) in a browser. When the preview looks stale after an edit, reload with a cache-busting query such as `index.html?v=2`.

## Architecture

- **Data:** the `QUESTIONS` array (`q` text, `a` four answers, `c` index of the correct answer) is in a fixed order and drives everything. Keep exactly 5 questions with 4 answers each (the progress bar `max="5"` is hardcoded in the HTML).
- **State:** module-level `i` (question index), `score`, `answered`. `show()` renders a question, `choose(k)` locks it and scores it, the `#next` click handler advances or reveals the `#results` section.
- **Single scoring guard:** `answered` makes `choose()` ignore repeat clicks, so a question can add to the score only once. Preserve this when changing the answer flow.
- **Focus management:** `show()` focuses the `h2#question`, `choose()` focuses `#next`, and the results screen focuses `#final-score`. Keep this so keyboard-only use works.

## Conventions

- No external dependencies, network requests, servers or build tooling.
- All UI text is in French, with punctuation directly after the word, as in `planète rouge?`.
- Correct and wrong answers must never be shown by color alone: each uses a ✔/✘ text mark plus a different border style (solid vs dashed) and a feedback sentence.
- Presentation targets a projector: base font about 20px, high contrast, visible `:focus-visible` outline. Colors come from CSS custom properties in `:root`, overridden under `prefers-color-scheme: dark`; add new colors to both themes.
- Animations and transitions live only inside `@media (prefers-reduced-motion: no-preference)`.
- The `main` panel has a "mission control" look (grid background, header label via `main::before`); the quiz logic is independent from it.
- Out of scope for this version: timer, leaderboard, score persistence.
- The results screen has a `#replay` button; `replay` handler resets `i`, `score`, `answered`, swaps sections and calls `show()` (focus lands on `h2#question`).
