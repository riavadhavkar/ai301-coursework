# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | The diff's changed files and the behavior each hunk changes, read against the plan's stated scope (in-scope list and files-to-touch) plus any entry under its Deviations heading. | Pass if every changed file and every behavior change in the diff falls inside the plan's stated boundary or is named in a deviation note, and nothing the plan promised is missing unless a deviation note or the description says it was left out. Fail if the diff changes a file or behavior the plan and its deviation notes never mention, or silently omits a planned change. Unclear if the plan's scope cannot be found. | required |
| description-matches-diff | The description's Summary and Changes claims, read against the diff's actual contents (live: git diff main...HEAD). | Pass if every claimed change appears in the diff and every substantive change in the diff is claimed, and any stated shortfall (something left out or changed from the plan) is named plainly. Fail if the description claims a change, a fix, or a fidelity to the plan that the diff does not deliver, or leaves a substantive change unmentioned. An honestly disclosed shortfall is not a fail. | required |
| evidence-decisive | The test-evidence section (live: test_evidence.md and the description's Testing section), read against the plan's test plan and the reproduction's own steps. | Pass if it re-runs the reproduction's steps, shows the before output and the after output, and the after output shows the specific behavior the plan said would change (an observable result, not just "tests pass"). Fail if it only says tests pass or the fix works, if the before or after is missing, or if the shown evidence would look the same without the fix. | required |
| repo-checks-run | The test-evidence section's output from the repo's own checks (the commands the PR template or CONTRIBUTING name, for example unit tests, lint, typecheck), read against the checks the repo asks for. | Pass if the outcome of each check the repo asks for is visible in the evidence. A check that fails or cannot run is still a pass when the evidence shows the failure and the reason. Fail if a check the repo asks for was never run, its outcome is not shown, or the description ticks a Testing box the evidence does not support. | required |
| no-debris | The diff's hunks and the commit list: debug prints, commented-out code, dead code, stray files, leftover markers. | Pass if no added line is debug output, commented-out code, dead code, or a stray file unrelated to the change. Fail if any such leftover rides along in the diff. | required |
| change-reviewable | The diff read as a reviewer would: the lines that fix the issue against everything else in the diff. | Pass if the fix is visible in the diff and nothing unrelated (formatting churn, drive-by edits, unrelated refactors, whitespace-only hunks) buries it. Fail if unrelated hunks or churn make it hard to see what the change does, even when explained. | required |
| template-honored | The description read against the repo-facts block's PR template (live: .github/PULL_REQUEST_TEMPLATE.md), section by section. | Pass if every section the template asks for has real, specific content for this change (an N/A with a reason counts as real) and the issue is named. Fail if a section is empty, left as the template placeholder, or filled with boilerplate that could sit on any PR. | required |
| ai-disclosure | The description read against the repo-facts block's stated AI-use policy (live: CONTRIBUTING.md and the template's disclosure ask). | Pass if the description discloses how AI was used, in specific terms, wherever the repo or course asks for it. Also pass if the repo states no AI-use ask and the description makes no claim about AI. Fail if a disclosure is required and missing, or if it is a stock line with no detail about what the AI did. | required |
| maintainer-direction | The issue's maintainer signals (thread highlights in eval, the live thread in live mode) and the repo-facts block's contributing asks, read against the PR title, description, and diff. | Pass if every explicit maintainer direction in the thread or contributing asks (an approach, a convention, a scope limit, a branch or title format) is followed or engaged by the PR. Also pass if the thread holds no such direction and the repo's stated asks are met. Fail if an explicit direction or stated ask is visibly ignored. | required |
| title-clear | The PR title, read against the diff. | Pass if a reviewer could tell from the title alone what the change does and where. Fail if it is vague, generic, or describes something the diff does not do. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
- accept only if every required check passes.
- reject if any required check fails.
- unclear counts as fail: a PR that cannot be verified from the package is not ready to submit.
- preferred checks never change the verdict, but are reported in the summary.
- an honestly disclosed shortfall (something left out, a change from the plan, a check that failed with its reason shown) never fails a check by itself: the checks above grade whether the PR is truthful and verifiable, not whether it did everything the plan hoped for.