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
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
