[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/meviusorg5799/fast-site-generator/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/meviusorg5799/fast-site-generator/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

# fast-site-generator

A fast drop-in for makefun-site 1.0.0: for each case's input it returns exactly
the output tree makefun-site's build returns — every page's `index.html`, the
index and tag pages, `build-report.txt` and each static file — byte for byte,
at a fraction of the CPU time.

## How it is fast

- **Single-pass Markdown**: blocks render straight into the output, with no
  token tree in between; inline markup scans once, flushing plain runs in
  bulk instead of one character at a time.
- **Pre-resolved templates**: every path's first name is bound to its
  enclosing `for` at parse time, so rendering walks no scope chain for the
  common case; loop frames and their `loop` records are reused per iteration.
- **Cached slugs and dates**: repeated titles, tags and dates slugify once
  per case (the caches are cleared on every `run()` call, so nothing carries
  from one case to the next).
- **No per-page re-parsing**: templates are parsed once per case; cards are
  built once per page and shared between the page and the lists that show it.

## Layout

- `solve.js` — dependency-free ES module; `run(input)` returns the output tree.
- `main.ts` — the program the harness runs: reads every case from
  `/input/cases.json`, answers each with `run`, writes `/output/answers.json`.

## How to run it

```sh
# Node 22.18 or later (the grader runs Node 22.23.4)
MAKEFUN_INPUT_DIR=<dir with cases.json> MAKEFUN_OUTPUT_DIR=<dir for answers.json> node main.ts
```

No dependencies, no network, no clock: the output is a function of the input
alone, and two runs over the same cases are byte-identical.

## License

MIT — see [LICENSE](LICENSE).
