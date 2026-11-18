# Demo Day Script (5 minutes)

## 0:00 to 0:30: Toolkit story

**Show:** top of the write-up, title and Overview.

**Say:**

> Hi everyone, I'm Ria. My toolkit is four tools from this course: an issue scout, repro-check, ship-gate and a PR monitor. I used them to get a first open source contribution merged in Elmo, 25 tests for two untested helper functions. Claude Code did most of the typing. My part was the judgment calls, like which issue to pick and what to approve.

## 0:30 to 1:45: The friction

**Show:** Tab 2. Point at the first paragraph, then the quote.

**Say:**

> I want to walk through the scout, because its miss taught me the most. On Nov 11 it scanned 300 results, kept 190 after filters and accepted 5. It also got two issues wrong because the work was already done. It accepted kana-dojo 33943 even though it was finished, and it only rejected OpenSearch 7141 because of other checks. The scout's own read-out said it plainly: the rubric had no "already fixed or has an open PR" check, and the fix belonged in the rubric.

## 1:45 to 2:30: The change

**Show:** Scroll to the `not-already-done` code block and point at it.

**Say:**

> So here's the change. Claude Code added this row, not-already-done. It fails an issue if the change is already on the default branch, or a merged PR delivers it. I reviewed it before it went in.

## 2:30 to 3:30: The result

**Show:** The "four of the Elmo issues" sentence, then Tab 3.

**Say:**

> With the patched scout, four of the Elmo issues it checked already had open PRs. Issue 1173 had none and passed all six checks, so I picked it over 996. I claimed it, and PR 1178 was approved and merged about 2.2 hours after I opened it. So the change that mattered wasn't more code. It was one line in a rubric.

## 3:30 to 4:15: One honest limit

**Show:** Tab 4. Point at "My scout fix is untested against the Unit 1 eval."

**Say:**

> One honest limit: I haven't rerun my Unit 1 regression since adding that row, so I don't know yet whether it changes how the scout grades the old eval items.

## 4:15 to 5:00: What's next

**Show:** Same tab. Point at the bullet about maintainers answering outsiders.

**Say:**

> Next, I'd rerun that regression and tighten the row if anything flips. I'd also have the scout check whether maintainers actually answer outsiders. I did that by hand with gh this time. Thanks, happy to take questions.



## Q&A answers (from your page)

- **What almost didn't make it?** The ship-gate rejection. It caught that I missed the issue's "one top-level describe per function" rule.
- **Which week's friction taught you the most?** The scout missing already-done issues, because the fix was one rubric line, not code.
- **Which tool would you feature second?** Ship-gate. Its quote is on the page and it caught something before the maintainer saw it.
- **Would you use it outside the course?** Your honest answer. "I didn't get there" is a complete answer.
- **What did Claude Code do versus you?** It wrote the test file, plan, ship manifest, rubric row and playbook draft. I chose the issue, read each describe block, and approved the plan, the claim text and the PR.
- **Why did the maintainer change `og.ts`?** The issue told me not to touch it, and it told contributors not to add "-Elmo" cases. He decided to change that behavior himself afterward.

