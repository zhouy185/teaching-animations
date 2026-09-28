# Teaching Animations

A growing collection of interactive, step-through animations built for lecture. `index.html` is a table of contents linking to each demo.

Live: https://zhouy185.github.io/teaching-animations/ (once GitHub Pages is enabled on this repo)

## Demos

1. **Split &rarr; Apply &rarr; Combine** ([split-apply-combine.html](split-apply-combine.html)) — pandas' `groupby().agg()`, one step at a time. Watch 16 real orders sort into store groups, get narrowed to one column, and get counted, summed, and averaged.

## Using it in class

- Open `index.html` in any browser, or use the Pages URL above once it's live, and click through to a demo.
- Each demo has `Next` / `Prev` buttons, or Left/Right arrow keys, to step through its stages.
- Click a dot below a demo's table to jump straight to any stage.
- `Replay` resets a demo to the start.

Each demo is a single static HTML file — no build step, no dependencies, no server. It works offline once loaded.

## Adding a new demo

Copy `split-apply-combine.html` to a new file and edit its `CONFIG` block near the top of the `<script>`:

- `GROUP_COL` / `TARGET_COL` — the grouping key and the column being aggregated.
- `COLS` — which columns to display (key, label, alignment, `fmt: "currency" | "bool"`).
- `DATA` — the row objects.
- `STAGES` — the narration for each step. Each `token` must match a `<span class="tok" id="tok-...">` in the code panel so the highlighted line of code tracks the current stage.

The engine — row-grouping animation, column highlight, count-up stat tiles, result-table reveal, and the stage/dot/keyboard controls — is generic and shouldn't need to change for a same-shape demo (group -> select column -> count/sum/mean).

## Data

Sampled from [`grocery_orders.csv`](https://github.com/zhouy185/MMGMG722_Datasets/blob/main/grocery_orders.csv) — 16 real rows, 4 per store. The page itself notes that the resulting complaint rates are illustrative of this sample, not the true store-level rates in the full dataset.
