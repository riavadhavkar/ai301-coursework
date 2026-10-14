# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->
- Where it lives: the plan's diagnosis section (the stated cause). The behavior it must explain is in the repro-evidence block (eval) or the student's posted repro comment on the issue (live): the steps, the observed output or error, and any file or line it names.
- What good looks like: the stated cause cites behavior the repro evidence actually shows, and nothing in the repro output contradicts it. A diagnosis that could be pasted onto any bug report, or that explains a different behavior than the repro shows, is not grounded.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->
- Where it lives: the plan's in-scope statement, its not-in-scope line, and the list of files or areas it will touch. Compare with what the issue asks for.
- What good looks like: one bounded change that resolves the issue, with an explicit line about what will not be touched. A drive-by rewrite adds refactors, renames, or extra features, or names files the cause does not require.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->
- Where it lives: the plan's files, approach, and order-of-work sections.
- What good looks like: a stranger could make the first edit without asking the author anything, because the plan names the file or function and what changes in it. "Update the handling as needed" is not executable.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->
- Where it lives: the plan's test plan section, read against the repro evidence's steps and artifacts.
- What good looks like: it re-runs the same repro steps (or a check through the real code) and names the specific output expected before and after the fix. A decisive test plan fails without the fix. A vague one only says "run the tests".

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->
- Where it lives: the plan's risks and unknowns section, and the Deviations heading at the end of the candidate plan (live: plan.md). In live mode, a deviation also needs a follow-up comment if the posted plan is no longer true.
- What good looks like: real uncertainty (an unconfirmed cause, an unread file, an untested assumption) is stated as uncertain. False confidence states a guess as fact, or leaves risks empty while the plan rests on an unverified assumption. An honest mid-build deviation is recorded in the plan with what changed and why.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->
- Where it lives: the plan comment, read against the issue's maintainer signals (thread highlights in eval, the live thread in live mode) and the repo-facts block (PR template, contributing asks, AI-use disclosure policy).
- What good looks like: the comment responds to what maintainers already said, follows the repo's stated asks and any AI-use disclosure rule, and states the same diagnosis, scope, and test plan as the candidate plan. Boilerplate that could be posted on any issue is not thread-aware.