# Contribution: Add unit tests for the Open Graph image URL helpers

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Ria Vadhavkar
**GitHub Username:** riavadhavkar
**Issue:** https://github.com/elmohq/elmo/issues/1173
**Pull Request:** https://github.com/elmohq/elmo/pull/1178

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the scout and found its rubric misses issues that are already fixed**

I installed issue-scout over my Unit 1 skill, restored rubric.md and the evidence guide from my course repo because the old skill folder was missing, and wrote a search strategy with three gh search issues queries (TypeScript, JavaScript, Java upgrades) ranked by fewest comments. The first scan returned 300 results and accepted all of its top five, but four were from one repo where a member had filed a batch within 30 seconds, so I added a one-result-per-repository filter. Grading my picks showed a bigger gap: the rubric had no "already fixed or open PR" check, so OpenSearch-Dashboards#7141 was rejected only by other checks and kana-dojo#33943 was accepted though it was already done, and I added that check to the rubric in Unit 6. Running the scout from inside Claude Code was blocked by permission prompts, so I ran the first scans in my own terminal and added a gh search issues allowlist rule.

**Unit 6: Installed the voice guard and a repo-memory file, and the first reproduction report got rejected**

I merged my three voice-guide copies into one guide at ~/.claude/voice-guard/voice-guide.md, added an upstream section with rules for claims, questions and pings, and hooked check_voice.py into Claude Code. The wiring test passed: `gh issue comment --help` was stopped with "no body the guard can read", and my real claim passed when posted with --body-file. repro-check rejected my first reproduction report because step 3 referred to a script without showing it, and it also caught my local username in a file path, so I inlined the script and swapped the path for a placeholder. Two things were missing on my machine and I restored them: the repro-check and plan-check skills, and Node 24 plus pnpm 12.16.1 for elmo. CLAUDE.local.md came to 35 lines and is excluded from commits with .git/info/exclude.

**Unit 7: The ship-gate caught a miss in my tests that I had read past, and the PR is open**

I wrote a ship manifest with three rows from elmo's CI (pnpm lint, the apps/www tests, pnpm turbo check-types) and tested each in a clean shell before changing code; every row needed a PATH prefix because a clean shell has Node 26 and no pnpm. The ship-gate ran four times on my branch. The first run passed all three repo checks but my pr-precheck rejected the branch: the issue says to use one top-level describe per function and my test file had three, so I nested two of them under one and added a third commit. The next runs asked me to word the regression-check paragraph to match who ran it and to name my OS, and I also corrected a line that credited me with setting up Node and pnpm, which Claude Code did. The fourth run passed and the PR opened through Claude Code as elmohq/elmo#1178.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I chose elmohq/elmo#1173, adding unit tests for the Open Graph image URL helpers in apps/www/src/lib/og.ts. I have never contributed to open source before, and what I want to learn is how to interact with the open source community. This issue fits because it is TypeScript with vitest and I have experience building front-end web apps, and it spells out every test case, so I could finish it in the time I have.

I found it after a longer route than planned. My scout's first scan (three gh search queries for TypeScript, JavaScript and Java upgrades) accepted all of its top five, but four were one batch of issues filed within 30 seconds by a maintainer. Both issues I picked from the course site were rejected by the scout: OpenSearch-Dashboards#7141 was already fixed by a merged PR, and swift#53247 had no stated deliverable. I then filtered the site's export to TypeScript and JavaScript repos and checked merged and linked PRs with gh. Four of the five elmo issues listed on the site already had open PRs. #1173 had none, and the patched scout accepted it on all six checks with a difficulty note of straightforward. The elmo repo is on the site, but #1173 is not listed there, and I chose it anyway.

My honest read of the scout: its five checks held up on real issues, but the rubric missed issues that were already fixed (it accepted kana-dojo#33943, where the entry already existed), and the scan could not tell me whether maintainers merge outside PRs, which I checked by hand with gh. I added a not-already-done check to the rubric; I have not rerun the Unit 1 regression with it.

Claim comment: https://github.com/elmohq/elmo/issues/1173#issuecomment-6807239352 (posted 2026-11-11)

**Toolkit**

- Tool: issue-scout, with my three-query search strategy and my rubric, plus gh for free checks on merged and linked PRs
- What it produced: a first scan of 300 results with five accepts (four from one batch), rejections of OpenSearch-Dashboards#7141 and swift#53247, an accept with a hollow result on kana-dojo#33943, rejections of elmo#1176 and #1112 for open PRs, and an accept of elmo#1173 on all six checks after the patch
- Difficulties: no already-fixed check, no merge-speed signal, and running the scout from inside Claude Code was blocked by permission prompts until I granted read-only gh access for the call
- What changed: a one-result-per-repository filter in the strategy; scope-fits replaced by scope-feasible plus a difficulty note (the unit patch); a new not-already-done check

---

## Understanding the Issue

**Problem Description**

`apps/www/src/lib/og.ts` exports two small helpers, `getMarketingOgImage` and `getPageImage`, and neither has a test, while most files next to it in `apps/www/src/lib/__tests__/` do.

**Expected Behavior**

A new `og.test.ts` covers the cases the issue lists, passes under vitest, and `pnpm lint` still passes. The PR touches only that file plus my line in `.github/contributors.txt`.

**Current Behavior**

There is no `og.test.ts`: `vitest run src/lib/__tests__/og.test.ts` prints "No test files found". The helpers already return every value the issue lists. I checked all 18 title cases plus the URL, empty-parameter and no-mutation cases.

**Affected Components**

`apps/www/src/lib/og.ts` (read only), `apps/www/src/lib/__tests__/og.test.ts` (new), `.github/contributors.txt` (one line for the CLA), and `apps/www/vitest.config.ts` (read only; its include pattern is `src/**/*.test.ts`).

**Toolkit**

- Tool: Claude Code reading og.ts, the vitest config and the neighboring tests, plus my own probe script run through pnpm exec tsx
- What it produced: the four answers above, backed by the 18-case probe output
- Difficulties: none met; the issue spells out every case and the probe confirmed each value
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

macOS 26.5.2, Apple Silicon (arm64). Node v24.22.3 and pnpm 12.16.1, the repo's `engines` and `packageManager` values. elmo at commit 31d9f9d5 (main, committed 2026-11-10), with vitest v5.0.5 from `apps/www`. Setup: I forked and cloned the repo, then ran `pnpm install --frozen-lockfile` (27.8 s, no errors). Challenge: my default Node was v26.13.1 with no pnpm or corepack, so I downloaded Node 24 (its checksum matched the published SHASUMS file) and pnpm 12.16.1 into ~/.local/opt and put them first on PATH for these commands only.

**Steps to Reproduce**

1. `cd apps/www && pnpm exec vitest run src/lib/__tests__/og.test.ts` printed `No test files found, exiting with code 1`.
2. `ls apps/www/src/lib/__tests__/` lists agent-discovery, api-catalog, auth-md, external-link, format, html-to-markdown, request-handler and seo (all `.test.ts`), none for `og.ts`.
3. I ran the issue's listed cases through `getMarketingOgImage` and `getPageImage` with a throwaway script kept outside the repo, `probe.ts`, from the repo root with `pnpm exec tsx /path/to/probe.ts`. It reads the `title` back out of each returned URL and prints `ok` or `DIFF` per case:

```ts
import { getMarketingOgImage, getPageImage } from "<path-to-clone>/apps/www/src/lib/og.ts";
const t = (s: string) => new URL(getMarketingOgImage({ title: s }), "http://x").searchParams.get("title");
const cases: [string, string][] = [["Pricing · Elmo","Pricing"],["Blog | Elmo","Blog"],["Pricing  ·  Elmo","Pricing"],["Pricing · ELMO","Pricing"],["Elmo · Open Source AI Visibility","Open Source AI Visibility"],["Elmo - Docs","Docs"],["Elmo: Pricing","Pricing"],["elmo · Pricing","Pricing"],["Elmo·Pricing","Pricing"],["Elmo · Pricing · Elmo","Pricing"],["Pricing","Pricing"],["Elmo","Elmo"],["Elmo Cloud","Elmo Cloud"],["Why Elmo · Elmo","Why Elmo"],["Elmo · Why Elmo","Why Elmo"],["Elmohq · Pricing","Elmohq · Pricing"],["Pricing · Elmohq","Pricing · Elmohq"],["A/B? #1 & more","A/B? #1 & more"]];
for (const [i, want] of cases) { const got = t(i); console.log(got === want ? "ok  " : "DIFF", JSON.stringify(i), "->", JSON.stringify(got), got === want ? "" : "expected " + JSON.stringify(want)); }
console.log(getMarketingOgImage({ title: "Pricing", description: "Plans & pricing for teams", label: "Pricing" }));
console.log(getMarketingOgImage({ title: "Pricing" }), getMarketingOgImage({ title: "Pricing", description: "", label: "" }));
console.log(JSON.stringify(getPageImage([])), JSON.stringify(getPageImage(["guides","setup"])));
const a = ["guides","setup"]; getPageImage(a); console.log(JSON.stringify(a));
```

   Result: `ok` for all 18 title cases, then `/og.png?title=Pricing&description=Plans+%26+pricing+for+teams&label=Pricing`, `/og.png?title=Pricing` twice (no `description` or `label`, and empty ones), `{"segments":["image.png"],"url":"/og/docs/image.png"}`, `{"segments":["guides","setup","image.png"],"url":"/og/docs/guides/setup/image.png"}`, and `["guides","setup"]` after the call.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/elmohq/elmo/commit/31d9f9d58fe81eb2be41c8b3b8beaf09e399a54b (upstream main, where the gap exists; this issue asks for missing tests, so there is no failing commit to link)
- **Screenshots/logs:** the vitest "No test files found" output in step 1 and the 18 `ok` lines in step 3
- **My findings:** The gap is real: `og.ts` has no tests. The current behavior already matches every expected value I checked, so the tests can be written without changing `og.ts`. I haven't run `pnpm lint` yet because there is no test file to check.

**Toolkit**

- Tool: repro-check, the voice guard, and a throwaway tsx probe script
- What it produced: a reject on the first report and an accept on the second, and a voice-guard pass on both drafts
- Difficulties: the first report named a script it did not show, and the script's import path held my local username
- What changed: I inlined the script, replaced the path with a placeholder, and listed the test folder's contents in the report

---

## Solution Approach

**Analysis**

There is no defect to trace. `getMarketingOgImage` builds `cleanTitle` with two regexes, `/^Elmo\s*[·\-|:]\s*/i` for the start and `/\s*[·\-|:]\s*Elmo$/i` for the end. It sets `title` on a `URLSearchParams`, adds `description` and `label` only when they are non-empty, and returns `/og.png?` plus the query. `getPageImage` copies the slugs with a spread, appends `"image.png"`, and returns `{ segments, url }` with `url` as `/og/docs/` plus the segments joined by `/`. The spread is why the input array is not mutated.

**Proposed Solution**

Add `apps/www/src/lib/__tests__/og.test.ts`, written in the style of its neighbors (vitest `describe` and `it`, `@/lib/og` import, tabs, double quotes). It reads the `title` back out of the returned URL with `new URL(result, "http://x").searchParams.get("title")`, as the issue suggests, so the tests do not depend on how spaces are encoded. It covers the brand removed from the end, the start and both ends, the left-unchanged cases, special characters, the `/og.png` path, `description` and `label` passed through only when non-empty, and `getPageImage` for no slugs, nested slugs and non-mutation. I will not change `og.ts`, the vitest config or any dependency, add no changeset (the repo says never for `apps/www`), and add no test beyond the cases the issue lists.

**Implementation Plan**

1. Create `apps/www/src/lib/__tests__/og.test.ts` with the cases above.
2. Add `riavadhavkar` on its own line to `.github/contributors.txt`, which is the CLA signature.
3. Run the new test, then every row in my ship manifest (`pnpm lint`, the apps/www tests, `pnpm turbo check-types`), and keep the output in a working note that stays out of the commits.
4. To show the tests catch a regression, temporarily break one regex in `og.ts`, confirm the new test fails, then revert it and confirm `git diff` shows no change to `og.ts`. This is never committed.
5. Commit in two commits with plain imperative subjects and no attribution lines, on the branch `fix/1173-og-helper-tests`.

I have not run `pnpm lint` or `pnpm turbo check-types` with the new file in place, because it does not exist yet; both pass on unmodified main.

**Deviation (2026-11-11):** my first version had three top-level describes (title, URL and `getPageImage`), but the issue asks for one top-level describe per function. The ship-gate's pr-precheck caught it, and I nested the title and URL groups under one `describe("getMarketingOgImage")` in a third commit. The tests and their count (25) did not change.

**Toolkit**

- Tool: plan-check, in live mode, on my plan and a plan comment I did not post
- What it produced: an accept on all eight checks on the first run
- Difficulties: it flagged that the plan had no unknowns note and no Deviations heading, and that the comment asked a question the issue already answers
- What changed: I added an Unknowns section and a Deviations heading to the plan and removed the question from the comment

---

## Testing Strategy

The observable checks are the ones from my reproduction, run again against the built change.

- **Before:** on main at 31d9f9d5, `cd apps/www && pnpm exec vitest run src/lib/__tests__/og.test.ts` prints `No test files found, exiting with code 1`.
- **After:** the same command prints `Test Files  1 passed (1)` and `Tests  25 passed (25)`.
- **Repo checks, each run in a clean shell:** `pnpm lint` exit 0 (1061 files), `cd apps/www && pnpm test` 9 files and 87 tests passed, and `pnpm turbo check-types` 13 tasks successful.
- **Regression check:** I temporarily removed the `i` flag from the start-of-title regex in `og.ts`. One test failed (`removes the brand from the start of "elmo · Pricing"`) and 24 passed. I reverted it, and `cmp` showed `og.ts` identical to the original.
- **Coverage:** `vitest run --coverage` on `og.ts` reports 100% of statements (10/10), branches (4/4), functions (2/2) and lines (8/8). I did not add edge cases beyond the issue's list, because the issue says to cover only the listed cases and not to lock in the behavior of "-Elmo" words or separator-only titles.
- **Not run:** the e2e tests, `pnpm lint:spec`, and CI's other jobs.

**Toolkit**

- Tool: the ship-gate manifest (three rows) plus vitest, and a clean-shell test of every row before I changed any code
- What it produced: three passing checks on main before the change and three passing checks after, plus the regression check
- Difficulties: a truly clean shell has Node 26 and no pnpm, so every manifest row needs a PATH prefix; biome flagged one long line in my first version of the test file
- What changed: I put the PATH prefix in every manifest row and ran biome format on the test file

---

## Implementation Notes

**2026-11-11**

Created the branch `fix/1173-og-helper-tests` and wrote `og.test.ts` with three `describe` blocks (title, URL, `getPageImage`) and `it.each` tables for the title cases. It passed on the first run, 25 of 25, which fits the issue's statement that the expected values were already checked against the current code.

**2026-11-11**

`pnpm lint` flagged one long line in my test file (the `titleOf` helper). I ran `biome format --write` on that file only and it wrapped the line. I added `riavadhavkar` to `.github/contributors.txt` for the CLA and made two commits with plain imperative subjects. Before committing, my clone had no git identity, so I set a repo-local name and my GitHub noreply address, not my real email. No deviations from the plan.

**Toolkit**

- Tool: Claude Code writing the test file and running the checks, with biome as the formatter
- What it produced: `og.test.ts` (25 tests), the CLA line, two commits, and `test_evidence.md` with every output
- Difficulties: the formatter complaint above and the missing git identity; nothing else broke
- What changed: formatted the one file, set a repo-local git identity

---

## Pull Request

**PR Link:** https://github.com/elmohq/elmo/pull/1178

**PR Description:** Adds apps/www/src/lib/__tests__/og.test.ts with 25 tests for getMarketingOgImage and getPageImage (the cases listed in the issue, with the title read back out of the URL), and my username in .github/contributors.txt for the CLA. It says "Fixes #1173", shows the before ("No test files found") and after (25 passed) output, the three clean-shell checks, 100% coverage of og.ts, a regression check, what I did not run, and a notes-for-reviewers section on the AI help.

**Maintainer Feedback:**

- 2026-11-11: none yet. The PR was opened today. The only status on it is Vercel's "Authorization required to deploy", which is the standard fork-PR authorization a maintainer has to give; it is not a test failure.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (my manifest plus my pr-precheck) and the PR-creation command run through Claude Code, with the voice guard on the PR text
- What it produced: four gate runs (one reject, three passes), a PR title and body that passed the voice guard, and PR #1178 targeting elmohq/elmo main from my fork
- Difficulties: my first test file broke an instruction in the issue that I had read past; the install script was first blocked by Claude Code's auto-mode classifier, so I ran it myself; hooks only load at session start, so I tested them with a fresh claude -p session
- What changed: I nested the describes, reworded two paragraphs in the PR text, and corrected the AI-help paragraph so it credits Claude Code for the setup work

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
