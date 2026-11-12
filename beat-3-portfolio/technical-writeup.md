# Adding 25 Tests to Two Untested Helpers in Elmo, and the Checks That Caught My Misses

## Overview

Elmo is an open source AI visibility project. On Nov 10 a maintainer filed issue #1173 pointing out that two helpers in `og.ts`, `getMarketingOgImage` and `getPageImage`, had no tests. I claimed the issue and opened [PR #1178](https://github.com/elmohq/elmo/pull/1178) with 25 tests that cover every statement, branch and function in that file. The maintainer approved it and merged it about 2.2 hours later on Nov 11 (Nov 12, UTC). Four tools from the course were part of the work: an issue scout, repro-check, ship-gate and a PR monitor. Claude Code wrote the test file, the plan and the ship manifest, added the `not-already-done` row to my scout's rubric after the scout missed already-fixed issues, and drafted the monitor playbook. My part was the judgment calls. I chose the issue, read each describe block in the tests, and approved the plan, the claim text and the PR. I also reviewed the rubric row and the playbook's rule to nudge once after 5 quiet days.

## What happened

### Nov 11: picking an issue and fixing my scout

So how did I end up with #1173? I started with my scout. It scanned 300 results, kept 190 after filters and accepted 5, and four of those 5 were one batch filed within 30 seconds. It also got two issues wrong because they were already done. OpenSearch #7141 was rejected, but only because of other checks, and kana-dojo #33943 was accepted even though the work was finished. The scout's own read-out named the cause: "The rubric has no 'already fixed or has an open PR' check... the fix belongs in the rubric."

Thus Claude Code added this row to my rubric, and I reviewed it:

```
not-already-done | fail if the change is already on the default branch, or a merged PR delivers it | required
```

With the patched scout, four of the Elmo issues it checked already had open PRs. #1173 had none, and the scout accepted it on all six checks, so I picked it over #996.

### Nov 11, 16:17: claim and reproduction

I posted the claim comment, which was the first and only comment on the issue. Next, repro-check rejected the first version of my reproduction report:

> steps followable: fail. "Step 3 says 'a throwaway tsx script kept outside the repo' but never shows the script or how it was run."

It was right, since a reader could not have followed that step. Claude Code revised the report to show the script and how it was run, and the second version was accepted.

### Nov 11, 16:33: the tests

Claude Code wrote the 25 tests (`87045a0`) and added my username to the CLA file (`cf4e614`). The PR description shows vitest before and after, which is the gap the issue described:

- Before: `No test files found, exiting with code 1`
- After: `Tests 25 passed (25)`, with `og.ts` coverage at statements 10/10, branches 4/4, functions 2/2 and lines 8/8

Full coverage only tells me the lines ran, not that the tests can fail. So I had Claude Code break the code on purpose. It removed the `i` (case-insensitive) flag in `og.ts`, ran the suite, then reverted the change. The suite caught it:

```
FAIL ... removes the brand from the start of "elmo · Pricing"
Tests 1 failed | 24 passed (25)
```

The final version of the test file starts like this (commit `f8fea68`):

```ts
const titleOf = (title: string) =>
	new URL(getMarketingOgImage({ title }), "http://localhost").searchParams.get("title");

describe("getMarketingOgImage", () => {
	describe("title", () => {
		it.each([
			["Elmo · Open Source AI Visibility", "Open Source AI Visibility"],
			["Elmo - Docs", "Docs"],
```

### Nov 11, ~16:4x: ship-gate rejects my branch

Ship-gate checks the branch against what the maintainer asked for before I open a PR, and it failed:

> maintainer-direction [fail]: Issue says 'one top-level describe per function' but the file has two top-level describes for getMarketingOgImage ('... title' and '... URL') plus getPageImage.

This was my miss. The issue says "one top-level `describe` per function," and I read the test file without catching that Claude Code had split `getMarketingOgImage` across two. At 16:43 Claude Code nested them under a single `describe("getMarketingOgImage")` in commit `f8fea68`, and the next run said `verdict: PASS`. I opened PR #1178 at 16:47 (21:47Z).

### Nov 11, 16:54: waiting

The PR monitor's first run said to wait. Its playbook is built from the last five outside PRs that were merged on this project, whose first maintainer response took 69.69, 1.21, 0.25, 0.39 and 0.33 days (median 0.39). The rule I reviewed was to nudge once after 5 quiet days, and it never came into play.

### Nov 11, 19:07: approval and merge

The maintainer, jrhizor, approved at 00:07:58Z (Nov 12, UTC; 19:07 local on Nov 11) with "thanks!" and merged at 00:09:28Z, about 2.2 hours after I opened the PR. The merge commit is `a9a54361` and the issue closed. Before the merge, jrhizor added commit `aa07a0b`. It added test cases, simplified my URL helper and changed 3 lines in `og.ts` so that words like "Elmo-Powered" are no longer stripped.

## Limitations and what's next

- **The merged result isn't only my PR as opened.** The issue told me not to change `og.ts`, so my PR left it alone. The maintainer then changed three lines in it and edited the tests himself. The same issue had told contributors not to add cases for words ending in "-Elmo", because the current output wasn't meant to be locked in as intended behavior. So my tests followed the issue as written, and the maintainer decided to change that behavior afterwards. That means the merged code doesn't show only what I submitted. If I find a case like that again, I would raise it in the PR description, as the issue says to, instead of adding a test for it.
- **I haven't compared my tests with his edits.** The next step is to read `aa07a0b` line by line to see how he rewrote the tests, so I can match the project's style from the start next time.
- **My scout fix is untested against the Unit 1 eval.** I haven't rerun the Unit 1 regression since the `not-already-done` row was added, so it may grade "unclear" on those bundles. The next step is to rerun it and tighten the row's wording if any items flip.
- **The scout can't tell whether maintainers answer outsiders.** I checked that by hand with `gh`, which is where the response times above came from. The next step is to have the scout pull those numbers itself.
- **I couldn't claim #1173 on the course site.** It wasn't listed there, so I claimed it with a comment on the GitHub issue.
