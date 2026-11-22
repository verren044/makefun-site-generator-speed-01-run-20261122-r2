# BUILD_LOG — site-generator-speed-01-run-20261122-r2

Contest: site-generator-speed-01-run-20261122 — "Build a static site faster, 22 November 2026"
Prize: $100 (10 × $10) · Deadline: 2026-11-24T20:00:00Z (03:00 WIB 25 Nov 2026)
Entry: a faster drop-in for makefun-site 1.0.0 (`solve.js` + `main.ts`, kit driver).

## What was built (fresh, in the post-opening session)

`solve.js` is my own dependency-free reimplementation of the
makefun-site 1.0.0 build, written from the kit spec only (README, RULES.txt,
competition.yaml, reference behaviour): front matter, Markdown subset,
template dialect, slug rule, collections/tags, layouts, build report.
Speed work vs the reference: bulk HTML escaping in inline rendering
(the reference escapes one character at a time), single-pass Markdown
rendering with no token objects, ASCII fast path in slugify, memoised
slugify, cached page/card objects, single-scan word counting.

## Practice results (official scorer, kit practice set, 3 cases)

- Scorer: `{"passed":1,"cases_passed":3,"cases_total":3}` — 3/3 identical.
- Two full runs byte-identical: answers.json sha256
  `25980928b145f54712027939364a64c277098bc4528a2d524e0bc27a0275fb03` (both runs).
- In-process timing over the 3 practice cases (5 runs, solve only, no startup):
  - house baseline (makefun-site 1.0.0): 528, 542, 553, 587, 647 ms — median 553 ms
  - this entry: 189, 194, 196, 227, 398 ms — median 196 ms
  - ratio: ~2.8× faster than the baseline (median 196 ms vs 553 ms).

## Bugs found in practice (and fixed in-session)

1. Pass 1 scored 0/3: my heading detector accepted `#text` (no space after
   `#`) as a heading; the spec requires at least one space/tab after the
   `#'s`. Fixed in the pass-2 rewrite.
2. After that fix, tag pages rendered empty names and every tag's page held
   all posts: `tagOf` was defined with one parameter but called with two
   `(b, t)`, so it read the build object as the tag. Fixed (signature
   `(b, t)`); scorer then 3/3.

## Files

- `solve.js` — the fast reimplementation (pass 1 + pass 2 full rewrites in this session, plus the tagOf fix)
- `main.ts` — the kit starter driver (unchanged copy of the baseline driver)
- `README.md` — with the three live make.fun badge lines (owner verren044, repo makefun-site-generator-speed-01-run-20261122-r2, referral k7cmczfx)
- `LICENSE` — MIT, `SPDX-License-Identifier: MIT` first line
- `kit/` — the kit bundle (not committed, gitignored)
