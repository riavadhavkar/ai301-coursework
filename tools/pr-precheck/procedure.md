# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->
1. Live mode only: read `scope.md` first and stop if the `Repo:` line is a placeholder or the package is for another repo. Then read `voice-guide.md`. In eval mode skip both.
2. Read the repo facts next (eval: the repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Note every PR template section, every stated check the repo asks for, and the AI-use policy. This comes before the PR because the description and evidence are graded against it.
3. Read the issue and its maintainer signals (eval: the issue and thread highlights; live: the issue body and thread). Note what the issue asks for and any direction a maintainer gave (an approach, a convention, a scope limit).
4. Read the plan before the diff (eval: the plan-context block; live: `plan.md`). Note the in-scope list, the not-in-scope line, the files it says it will touch, the test plan with its expected before and after, and every entry under Deviations. The plan must come before the diff, because the diff is graded against it, and reading the diff first makes whatever it contains look intended.
5. Read the diff and commit list (eval: the candidate PR's diff and commits; live: `git diff main...HEAD` and `git log main..HEAD --oneline`). List every changed file and, for each, one line on what the hunks change. Note any debug lines, commented-out code, stray files, or whitespace-only hunks as you go.
6. Read the PR title and description last (eval: the candidate PR's description; live: `pr_draft.md`). Note every claim it makes about what changed, every Testing box ticked, and the AI-use disclosure. It comes after the diff because it is graded against the diff.
7. Read the test evidence (eval: the test-evidence section; live: `test_evidence.md`). Note the before output, the after output, and the outcome of each repo check.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->
1. Plan fidelity (diff-within-plan): make two lists. List A is the files and behaviors the plan names as in scope or to touch, plus each deviation note. List B is the files and behaviors the diff actually changes. Record every item in B not in A (extra) and every item in A not in B (missing). For each missing item, record whether a deviation note or the description says it was left out.
2. Plan fidelity (description-matches-diff): copy each change the description's Summary and Changes claim. Match each to a hunk in the diff and record the file. Record every substantive hunk in the diff that no claim covers, and every claim with no hunk.
3. Test evidence (evidence-decisive): copy the plan's expected before and after. Find the before output and the after output in the evidence. Record whether each shows the specific behavior the plan named, and whether the steps match the reproduction's steps. Record whether the after output would look the same without the fix.
4. Test evidence (repo-checks-run): list the checks the repo asks for (template Testing section, CONTRIBUTING). For each, record the outcome shown in the evidence, or "absent". Record every Testing box the description ticks and whether the evidence supports it.
5. Diff quality (no-debris, change-reviewable): copy each debug print, commented-out block, dead code, stray file, or whitespace-only hunk with its file. Then record which hunks are the fix, and which are neither the fix nor named in the plan.
6. Standards and comms (template-honored): list each section of the PR template and copy the first line of what the description wrote under it, or "empty" or "placeholder".
7. Standards and comms (ai-disclosure): copy the repo's AI-use ask and the description's disclosure sentence. Record "none" if there is no disclosure.
8. Standards and comms (maintainer-direction): copy each maintainer direction from the thread and each contributing ask. Next to each, copy the part of the PR (title, description, or hunk) that follows or engages it, or "none".
9. Title (title-clear): copy the title and one line saying what the diff does.
10. In eval mode use only the bundle text. In live mode use only what the drafts, the diff, and the pages named above contain, and quote, never other files in the working directory.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->
1. Run the checks in the order they appear in rubric.md.
2. For each check, use only the evidence gathered for it and apply the pass condition exactly as written.
3. If the evidence for a check is genuinely absent from the package, grade it fail when its absence is the failure (for example no after output, no AI disclosure where one is required, a template section left empty). Grade it unclear only when evidence exists but is too thin to decide, or when the plan's scope cannot be found at all.
4. A shortfall the PR discloses plainly (in the description or a deviation note) is not a failure of the check it touches. Grade the disclosure, not the shortfall.
5. Write one line per check: the fact or quote that decided it.
6. A check may be graded from the gathered notes without re-reading the whole package, except diff-within-plan and description-matches-diff, which must be re-checked against the diff itself before they are graded, because a missed hunk flips them.
7. If a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary.
8. If this procedure is silent on a step, say so in the summary instead of inventing a step.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->
1. Apply the rubric's verdict rule: accept only if every required check passes.
2. Count every unclear grade as fail.
3. Preferred checks are reported but never change the verdict.
4. Name the deciding check in the output. On reject, it is the first required check, in rubric order, that failed or was unclear; quote its one line of evidence. If more than one required check failed, name only that first one in the summary and list the rest by name. On accept, name the check that came closest to failing, with its evidence line.
5. In live mode, add any voice-guide rules the draft breaks to the summary. They do not change the verdict.
6. Emit the JSON block last, with one entry per check in rubric order and the verdict, and nothing after it.