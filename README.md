# Teaching Animations

A growing collection of interactive, step-through animations built for lecture. `index.html` is a table of contents linking to each demo.

Live: https://zhouy185.github.io/teaching-animations/ (once GitHub Pages is enabled on this repo)

## Demos

1. **Split &rarr; Apply &rarr; Combine** ([split-apply-combine.html](split-apply-combine.html)) — pandas' `groupby().agg()`, one step at a time. Watch 16 real orders sort into store groups, get narrowed to one column, and get counted, summed, and averaged.
2. **LEFT JOIN, Step by Step** ([left-join.html](left-join.html)) — a SQL `LEFT JOIN`, one step at a time. A trips table and a zones lookup table start apart, matching rows light up together, an unmatched row is kept with `NULL`s (the key LEFT vs INNER JOIN distinction), then the looked-up columns are attached.
3. **Join &rarr; Group &rarr; Sort** ([inner-join-groupby.html](inner-join-groupby.html)) — the same two tables, but with `INNER JOIN` (the unmatched row is visibly excluded, not NULL-filled) feeding into `GROUP BY`, `COUNT`, `AVG`, and `ORDER BY`. Rows regroup and recolor from per-location to per-borough, stats count up per group, and a ranked result table appears.
4. **Same Table, Twice** ([same-table-twice.html](same-table-twice.html)) — one `zones` lookup table joined twice under two aliases (pickup and dropoff) &mdash; no copy needed, just two aliases and two `ON` conditions. `GROUP BY` on the *pair* of boroughs, `COUNT`, and `ORDER BY` build an origin&ndash;destination result, which gets pivoted into a small grid at the end.

## Using it in class

- Open `index.html` in any browser, or use the Pages URL above once it's live, and click through to a demo.
- Each demo has `Next` / `Prev` buttons, or Left/Right arrow keys, to step through its stages.
- Click a dot below a demo's table to jump straight to any stage.
- `Replay` resets a demo to the start.

Each demo is a single static HTML file — no build step, no dependencies, no server. It works offline once loaded.

## Adding a new demo

Pick whichever existing file matches the *shape* of the concept you're teaching, copy it, and edit its `CONFIG` block near the top of the `<script>`. Then add a card to `index.html`'s TOC and a line to the Demos list above.

- **Group -> select column -> count/sum/mean**, e.g. `groupby().agg()`: copy `split-apply-combine.html`.
  - `GROUP_COL` / `TARGET_COL` — the grouping key and the column being aggregated.
  - `COLS` — which columns to display (key, label, alignment, `fmt: "currency" | "bool"`).
  - `DATA` — the row objects.
  - `STAGES` — the narration for each step. Each `token` must match a `<span class="tok" id="tok-...">` in the code panel so the highlighted line of code tracks the current stage.
  - The engine — row-grouping animation, column highlight, count-up stat tiles, result-table reveal — is generic and shouldn't need to change for a same-shape demo.
- **Two tables matched on a key**, e.g. `JOIN`: copy `left-join.html`.
  - `TRIP_COLS` / `ZONE_COLS` and `TRIPS` / `ZONES` — the two tables' columns and rows (rename freely; the join key just needs to appear as a column on each side).
  - `STAGES` — same narration/token pattern as above.
  - The engine — join-key column highlight, color-linked row matching, the no-match spotlight, and fading in the looked-up columns — is generic for a same-shape "match rows across two tables, then attach columns" demo.
- **Join, then group and aggregate**, e.g. `INNER JOIN ... GROUP BY ... ORDER BY`: copy `inner-join-groupby.html`.
  - `TRIP_COLS` / `ZONE_COLS`, `TRIPS` / `ZONES`, `BOROUGH_ORDER` / `BOROUGH_COLORS` — the two tables plus the grouping key's possible values and colors (rename freely).
  - `STAGES` — same narration/token pattern as above.
  - This engine combines the previous two: it drops (rather than NULL-fills) unmatched rows, then reuses the FLIP-reorder/stat-tile/result-pane mechanic from `split-apply-combine.html` to group, count, average, and rank the survivors. Row-match colors are recomputed rather than duplicated when the coloring scheme changes from per-key-value to per-group, which keeps one CSS mechanic (`is-matched` + `--match-color`) doing both jobs.
- **One lookup table joined twice (a self-join by alias)**, with a composite `GROUP BY`: copy `same-table-twice.html`.
  - `TRIP_COLS` / `ZONE_COLS`, `TRIPS` (now with two foreign keys, e.g. pickup + dropoff) / `ZONES`, `BOROUGHS` / `GROUP_ORDER` / `GROUP_COLORS`.
  - `STAGES` — same pattern, but a stage's `token` can be an array (`tokens: [...]`) when it should highlight more than one code span at once (used here for "either lookup can fail" spanning both `JOIN` lines).
  - Only one lookup-table panel exists in the DOM; it's highlighted and attached from twice in sequence (`highlight-pu`/`highlight-do`, then `pickup_borough`/`dropoff_borough` columns) rather than duplicated, since the whole point is that no copy exists. A row drops if *either* lookup fails. The grouping key is a compound string (`"<pickup>→<dropoff>"`), and the final reveal adds a small pivoted grid (rows/cols = the distinct key values, cells = counts) alongside the flat ordered result table.

All four engines share the same stage/dot/keyboard/Prev/Next/Replay controls and CSS look, so a new demo built from any of them feels consistent with the rest.

## Data

- **Split-Apply-Combine**: sampled from [`grocery_orders.csv`](https://github.com/zhouy185/MMGMG722_Datasets/blob/main/grocery_orders.csv) — 16 real rows, 4 per store. The page itself notes that the resulting complaint rates are illustrative of this sample, not the true store-level rates in the full dataset.
- **LEFT JOIN**, **Join &rarr; Group &rarr; Sort**, and **Same Table, Twice**: the same synthetic HVFHV-style trip rows and small zones lookup, hand-built (not sampled) so the match/no-match story comes out clean, and shared across the three demos so they read as one continuing example — each page notes this itself.
