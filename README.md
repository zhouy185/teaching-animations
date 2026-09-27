# Split-Apply-Combine

An interactive, step-through animation of pandas' `groupby().agg()`, built for lecture. Watch 16 real orders sort into store groups, get narrowed to one column, and get counted, summed, and averaged — one `Next` click at a time.

Live: https://zhouy185.github.io/teaching-animations/ (once GitHub Pages is enabled on this repo)

## Using it in class

- Open `index.html` in any browser, or use the Pages URL above once it's live.
- `Next` / `Prev` buttons, or the Left/Right arrow keys, step through the 6 stages.
- Click a dot below the table to jump straight to any stage.
- `Replay` resets to the start.

It's a single static HTML file — no build step, no dependencies, no server. It works offline once loaded.

## Reusing this as a template for the next animation

Edit only the `CONFIG` block near the top of the `<script>` in `index.html`:

- `GROUP_COL` / `TARGET_COL` — the grouping key and the column being aggregated.
- `COLS` — which columns to display (key, label, alignment, `fmt: "currency" | "bool"`).
- `DATA` — the row objects.
- `STAGES` — the narration for each step. Each `token` must match a `<span class="tok" id="tok-...">` in the code panel so the highlighted line of code tracks the current stage.

The engine — row-grouping animation, column highlight, count-up stat tiles, result-table reveal, and the stage/dot/keyboard controls — is generic and shouldn't need to change for a same-shape demo (group -> select column -> count/sum/mean).

## Data

Sampled from [`grocery_orders.csv`](https://github.com/zhouy185/MMGMG722_Datasets/blob/main/grocery_orders.csv) — 16 real rows, 4 per store. The page itself notes that the resulting complaint rates are illustrative of this sample, not the true store-level rates in the full dataset.
