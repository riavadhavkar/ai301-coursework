# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-diagnosis | The plan's stated cause, read against the repro-evidence block (live: the student's posted repro comment). Note the exact behavior, error, or output the repro shows. | Pass if the stated cause explains the specific behavior the repro evidence shows and does not contradict or ignore any of it. Fail if the cause is generic, cites behavior the repro never showed, or conflicts with the repro output. Unclear if the repro evidence is too thin to tell. | required |
| cause-not-symptom | The plan's approach and files to touch, read against the stated cause and the repro evidence. | Pass if the planned change fixes the underlying cause the evidence points at. Fail if it only hides, suppresses, or works around the visible symptom (for example catching the error, special-casing the one repro input) while the cause stays in the code. | required |
| bounded-scope | The plan's in-scope statement, its not-in-scope line, and the files or areas it names, read against what the issue asks for. | Pass if the plan describes one bounded change that resolves the issue and explicitly leaves other things alone. Fail if it adds refactors, cleanups, extra features, or files the issue does not need, or if it states no limit at all. | required |
| executable | The plan's files, approach, and order of work, read as if by someone who has never seen the repo. | Pass if a stranger could start the first edit without asking the author anything: the plan names the files, modules, or functions to change and what changes in each. Naming the code area and the specific change is enough when the plan also gives a concrete way to pin the exact function (for example a named debug trace or a call path to follow). Fail if it only says what the outcome should be, if it leaves the layer or component to change undecided, or if it leaves the key step as "figure out" or "update as needed". | required |
| observable-test-plan | The plan's test plan, read against the repro evidence's steps and output. | Pass if it re-runs the repro steps (or a check through the real code) and names the specific before and after output that will show the fix worked. Fail if it only says "run tests" or "verify it works", if its expected result cannot be observed, or if it would pass without the fix. | required |
| honest-unknowns | The plan's risks and unknowns, and its Deviations heading, read against how sure the plan sounds. | Pass if real uncertainty (an unconfirmed cause, an untested assumption, a file not yet read) is stated as uncertain, and claims the evidence does not back are not presented as fact. Fail if unknowns are dressed up as certainty, or if the plan states no unknowns while resting on a guess the evidence doesn't confirm. | required |
| thread-and-conventions-aware | The plan comment, read against the issue's maintainer signals (thread highlights, or live thread) and the repo-facts block (contributing asks, templates, AI-use policy). | Pass if the comment follows the repo-facts block's stated asks (template, contributing rules, AI-use disclosure) and, where the thread has a maintainer signal (a suggested direction, a claimed approach, a request), responds to it. If the thread has no maintainer signal, judge on the repo's stated asks alone. Fail if the comment ignores a maintainer signal, breaks a stated ask or policy, or is boilerplate that could be posted on any issue. | required |
| comment-matches-plan | The plan comment read against the candidate plan (live: plan.md). | Pass if the comment states the same diagnosis, scope, and test plan as the plan, in the author's own words. Fail if it promises something the plan does not contain or leaves out the plan's main limits. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
- accept only if every required check passes.
- reject if any required check fails.
- unclear counts as fail: a plan that cannot be verified from the package is not ready to build from.
- preferred checks never change the verdict, but are reported in the summary.