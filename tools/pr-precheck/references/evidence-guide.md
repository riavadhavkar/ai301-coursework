# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->
- Where it lives: the plan's scope statement (the in-scope list, the not-in-scope line, and the files-to-touch list) and its Deviations heading. In an eval bundle this is the plan-context block's scope pair; live, it is `plan.md`. What actually changed lives in the candidate PR's diff and commit list (eval) or in `git diff main...HEAD` and `git log main..HEAD --oneline` (live). The description's claims live in its Summary and Changes sections (eval: the candidate PR's description; live: `pr_draft.md`).
- What good looks like: every changed file and every behavior change in the diff falls inside the plan's stated boundary or is named in a deviation note, and every change the plan promised is either in the diff or named as left out. A deviation recorded in the plan (what changed and why) re-ties a mismatch. A description whose claims each match a hunk, and whose hunks are each claimed, is faithful.
- What silent drift looks like: the diff touches a file or behavior the plan and its deviation notes never mention (more than the plan), or leaves a planned change out with no note (less than the plan), or the description claims a fix or a plan fidelity the diff does not contain. An honestly disclosed shortfall is not drift.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->
- Where it lives: the test-evidence section of the candidate PR (eval) or `test_evidence.md` and the description's Testing section (live). Read it against the plan's test plan, which names the expected before and after, and against the reproduction's steps (eval: the repro in the plan-context block; live: the posted repro comment or the plan's Diagnosis). The outcome of the repo's own checks (tests, lint, typecheck, whatever the PR template's Testing section lists) is part of the same section.
- What good looks like: the evidence re-runs the reproduction's steps and shows a before output and an after output, and the after output shows the specific observable behavior the plan named (a status, a field value, a log line), not just "tests pass". The outcome of each check the repo asks for is visible. A check that failed or could not run is still good evidence when the output and the reason are shown. Evidence that would read the same without the fix is not decisive.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->
- Where it lives: the unified diff and the commit list. Eval: the candidate PR's diff and commits. Live: `git diff main...HEAD` (three dots, from the top folder of the working copy, on the branch) and `git log main..HEAD --oneline`. `git diff main...HEAD --stat` gives the list of files changed.
- What good looks like: a reviewer can see the fix in the hunks, and the only other hunks are ones the plan names (for example a test file, or a config entry the plan says to remove). Nothing unrelated rides along.
- Debris tells: added `print` or logging lines used for debugging, commented-out code, dead code or unused imports, stray files (scratch notes, output files, `plan.md`, `pr_draft.md`, `test_evidence.md`, `comment.md` committed by mistake), whitespace-only or reformatting hunks, renames or refactors the plan never mentioned, and drive-by edits to files unrelated to the issue.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->
- Where it lives: the repo's asks live in the repo-facts block (eval) or in the repo itself (live): `.github/PULL_REQUEST_TEMPLATE.md` for the sections, `docs/CONTRIBUTING.md` for contributing instructions and any AI-use policy, and the issue thread for explicit maintainer direction (eval: the thread highlights). The PR's side lives in the PR title and description (eval: the candidate PR; live: `pr_draft.md`), including the disclosure, which in Path Review goes under Notes for Reviewers.
- What good looks like: every template section has real content specific to this change (an N/A with a reason counts), the issue is named (for example `Closes #<number>`), only true Testing boxes are ticked, the AI-use disclosure says in specific terms what the AI did, and any explicit maintainer direction (an approach, a convention, a scope limit) is followed or answered in the PR. The CI box stays unticked in live mode, because CI only runs after the PR opens.
- What a wall looks like: a template section left empty or as placeholder text, boilerplate that could sit on any PR, a required disclosure missing or reduced to a stock line, or a maintainer direction or stated ask the PR visibly ignores.