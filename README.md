# site-generator-speed

![make.fun bounty](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)
![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/zammili/site-generator-speed/rank.svg)
![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/zammili/site-generator-speed/reward.svg)

A faster drop-in for makefun-site 1.0.0, written for the make.fun bounty
"Build a static site faster" (site-generator-speed-01-run-20261122).

`solve.js` is a dependency-free ES module whose `run(input)` returns exactly
the output tree makefun-site 1.0.0's `build` returns for the same input —
every page's `index.html`, the index and tag pages, `build-report.txt` and
each static file, byte for byte. `main.ts` is the harness driver: it reads
`/input/cases.json`, answers each case with `run`, and writes
`/output/answers.json`.

The implementation is an independent reimplementation of makefun-site's
rules — its front matter, permalinks and slug rule, its Markdown subset, its
collections and tags, its template dialect and its build report — tuned for
CPU time:

- source lines are classified by their first characters instead of a battery
  of regular expressions;
- inline Markdown is scanned in bulk runs (the next construct character is
  found with `indexOf`) instead of escaping character by character;
- front matter is read by scanning for line ends, so a body is a single
  slice rather than a split-and-join of every line;
- word counts are counted in one pass instead of matched into an array;
- a post's template card is built once and shared by its page, its index
  and tag listings and the recent list, instead of being rebuilt per use;
- slugs are memoised within a build (the same tag names and titles recur
  across hundreds of sources), and dropped with it — nothing is kept
  between cases.

Correctness is the gate: the entry was verified byte for byte against
makefun-site 1.0.0 on the kit's practice set and on 108 further sites drawn
by the kit's own generator, and it runs about 2.5x faster than the house
baseline's `run` on the practice set (about 1.6x end to end through the
driver, whose JSON reading and writing both entries share).

## License

MIT — see [LICENSE](LICENSE).

Entered on [make.fun](https://make.fun/bounty/site-generator-speed-01-run-20261122?ref=nj55s4jw) by @zammili.
