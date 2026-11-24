# makefun-site-generator-speed-01

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/deva1999/makefun-site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/deva1999/makefun-site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A fast drop-in for makefun-site 1.0.0: a hand-written, zero-dependency static site generator that answers exactly as the reference on every case — its front matter, template dialect, Markdown subset, slug rule and build report, byte for byte — built for make.fun's "Build a static site faster, 22 November 2026" bounty.

## Run it

```sh
node main.ts
```

The harness runs `node main.ts` with the cases at `/input/cases.json` (or `$MAKEFUN_INPUT_DIR/cases.json` outside a container) and reads the answers from `/output/answers.json` (or `$MAKEFUN_OUTPUT_DIR/answers.json`). `solve.js` exports `run(input)`, which returns one case's output tree.

## How it is fast

- Whole-string HTML escaping (never one regex call per character).
- A fused Markdown lex+render pass: no token array, blocks render as they are lexed.
- Inline rendering that copies plain runs in one slice and jumps straight to the next construct with a single native scan.
- A per-build slug memo (the slug function is pure, so the memo cannot change an answer).
- Template rendering into a parts array joined once per render.

## Benchmark (local, provisional — the grader's cpu_us is the only score)

Graded metric: CPU time (cpu_us, lower better) on the scored set; gate = every case byte-identical to makefun-site 1.0.0. Local numbers are a proxy only (this VM: ~1.5 shared CPUs, local Node v24.22.1; the grader runs Node 22):

| entry | grade-local practice | local CPU median (3 cases) |
|---|---|---|
| house baseline (makefun-site 1.0.0, unmodified) | 3/3 cases passed | 1.307 s (n=5) |
| this entry (V1) | 3/3 cases passed, L1 regrade reproduced byte for byte | ~0.75 s (paired A/B vs baseline state; see `iterations.md` in the build notes) |

---

<sub>Entered on [make.fun](https://make.fun/?ref=gx4yrk51&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
