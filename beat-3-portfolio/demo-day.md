# Demo Day Script (5 minutes)

**Show:** top of the write-up, title and Overview.

**Say:**

> Hey everyone, my name is Ria and my toolkit consists of four tools from this course: an issue scout, repro-check, ship-gate and a PR monitor. And I used them to get my first open source contribution merged into Elmo which is an open source AI visibility project. I wrote 25 tests for two untested helper functions with the help of Claude. I would say my part in this project was to make the judgment calls, like which issue to pick and what to approve since Claude handled the rest.

## 0:30 to 1:45: The friction

**Show:** Tab 2. Point at the first paragraph, then the quote.

**Say:**

> Now, I want to take a moment to walk through the scout. So on Nov 11 it scanned around 300 results and kept 190 after filters but ended up accepting only 5. It also got two issues wrong because the work for those was already done. The scout's own read-out said the cause was that the rubric had no "already fixed or has an open PR" check, so the fix belonged in the rubric.

## 1:45 to 2:30: The change

**Show:** Scroll to the `not-already-done` code block and point at it.

**Say:**

> And to fix that Claude Code added this row called not-already-done which fails an issue if the change is already on the default branch, or a merged PR delivers it. And I reviewed this change before it went in.

## 2:30 to 3:30: The result

**Show:** The "four of the Elmo issues" sentence, then Tab 3.

**Say:**

> Now with the patched scout, four of the Elmo issues it checked already had open PRs. Issue 1173 had none and passed all six checks, so I picked that and claimed it, and PR 1178 was approved and merged about 2 hours after I opened it. The takeaway here was that the fix was one line in a rubric and not more code.

## 3:30 to 4:15: One honest limit

**Show:** Tab 4. Point at "My scout fix is untested against the Unit 1 eval."

**Say:**

> One limitation that I encountered was that issue 1173 wasn't listed on the course site, so I couldn't claim it there. Instead I went ahead and added a comment on the GitHub issue itself. 

## 4:15 to 5:00: What's next

**Show:** Same tab. Point at the bullet about maintainers answering outsiders.

**Say:**

> What's next is that, I'd have the scout check whether maintainers answer outsiders because I did that by hand with gh this time.

> One more thing I'd like to highlight is that the maintainer of the repo I contributed to merged my PR super fast since they are very active on this project. I noticed that they also edited my code by adding some more test cases and simplifying my code. Overall, I was super happy with my first ever open source contribution. That wraps up my project, thank you!



## Q&A answers (from your page)

- **What almost didn't make it?** The ship-gate rejection. It caught that I missed the issue's "one top-level describe per function" rule.
- **Which week's friction taught you the most?** The scout missing already-done issues, because the fix was one rubric line, not code.
- **Which tool would you feature second?** Ship-gate. Its quote is on the page and it caught something before the maintainer saw it.
- **Would you use it outside the course?** Your honest answer. "I didn't get there" is a complete answer.
- **What did Claude Code do versus you?** It wrote the test file, plan, ship manifest, rubric row and playbook draft. I chose the issue, read each describe block, and approved the plan, the claim text and the PR.
- **Why did the maintainer change `og.ts`?** The issue told me not to touch it, and it told contributors not to add "-Elmo" cases. He decided to change that behavior himself afterward.

