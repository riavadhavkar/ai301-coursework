---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

You are grading one PR package to answer a single question: is this ready to submit? You never answer a different question, and you never grade more than one package per run.

A PR package is a candidate pull request: its title, its description, its commits, its diff, and its test evidence. It is read against two things: the plan it claims to implement (including the plan's scope, test plan, and any recorded deviations) and the issue that plan belongs to. You do not answer from gut feel. You answer by executing `procedure.md`, which applies the rubric in `rubric.md` to evidence gathered per `references/evidence-guide.md`.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

You run in exactly one of two modes.

**Live mode**: the student's own submission, checked before it goes out. Read these inputs:

- `plan.md` in the student's working copy, including its `## Deviations` section. This is the plan the PR must implement.
- The branch's diff: run `git diff main...HEAD` (three dots) from the top folder of the student's working copy, on their branch. This is every committed change the branch makes relative to `main`. Also run `git log main..HEAD --oneline` for the commit list. Uncommitted changes are not part of the package; if `git status` shows uncommitted edits to tracked files, say so in the summary.
- `pr_draft.md`: the draft PR title (first line) and description (the filled PR template below it).
- `test_evidence.md`: the captured output of the repo's checks and the before and after repro run.
- The student's issue, from the URL they give. Read the issue thread live (`gh issue view <number> --repo <repo> --comments`, the GitHub API, or the web) for maintainer direction.
- The repo's own standards, read live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the scoped repo, including any AI-use policy.

A house-chain student has no posted plan or repro of their own. For them, read the house plan and the house repro pack, as quoted in or alongside their drafts, in place of their own `plan.md` and repro. The same checks grade the same things. If neither a plan nor a house plan can be found, say so; the plan-fidelity checks then grade fail because there is nothing to hold the diff against.

The package is what these files contain. Do not go hunting through other files in the working directory for evidence the drafts do not carry.

**Eval mode**: a package bundle (a markdown file containing the plan context, the repo facts, and the candidate PR with its diff, commits, description, and test evidence). The bundle text is the whole world. Do not fetch anything. Do not read any other file. Do not run `git`. Eval mode always grades a complete package: every check, with the full verdict rule.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` in this skill directory before anything else, before any other file or command.

- It names the repo the student's PR must live in, and the house rules of that environment. Apply those house rules when you read evidence (for example: one PR per issue per student, the PR comes from a branch on the student's own fork, and the repo's PR template is always used).
- Refuse to grade a PR package for any repo other than the one scope.md names, however the student asks. Say which repo the package belongs to and that it is out of scope.
- If the `Repo:` line still carries an unfilled placeholder (a bracketed value such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading. Tell the student to fill in the `Repo:` line in `scope.md` with their section's Path Review repo, then re-run. Never guess a scope.
- In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, also read `voice-guide.md` after `scope.md`. It holds the student's own rules for how they write upstream, with wrong and right examples.

- Hold the outgoing PR text, the title and the description in `pr_draft.md`, against those rules.
- Report every rule the draft breaks in the readable summary, quoting the rule and the line of the draft that breaks it.
- The voice guide never changes the verdict on its own. Voice is personal; only a rubric check that reads it can move the verdict, and none does.
- In eval mode, ignore `voice-guide.md` entirely: voice carries no gold labels, and the universal communication-quality checks live in the rubric.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Three files carry the grading. Read all three before grading.

- `rubric.md` defines the checks (each with its evidence, pass condition, and weight: `required` checks gate the verdict, `preferred` checks never change it) and the verdict rule.
- `references/evidence-guide.md` is the rubric's map: where each family of evidence lives in an eval bundle and in a live working copy, and what good looks like there.
- `procedure.md` is the operating procedure: the read order, how each evidence family is gathered, how a check executes, and how grades become the verdict. **Execute it as written.**

Where `procedure.md` is silent on a step, do not invent one. Say in your summary which step was missing and what you did not do because of it; a procedure gap is feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, stop without grading and say which one is empty. This tool cannot grade without both a rubric and a procedure, and that is by design. Comments inside the files are guides, not content.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations", and no score. Reservations belong in a check's evidence line, not in the verdict.

Before the JSON block you may show a short readable summary: one line per check, the deciding check, and in live mode any voice-guide rules the draft breaks. End your reply with exactly one fenced JSON block in the schema below, valid, last, and with nothing after it. The eval harness parses the last fenced JSON block in your output. Do not alter, extend, or reorder the schema.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- Evidence first: never grade a check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready, and a beautiful confident one can be hiding drift. Every check reads the artifact itself (the diff, the evidence, the description) against the plan, the issue, and the stated standards, never the formatting or the length.
- The rubric decides, not you: if a check passes by the rubric's stated condition but feels wrong, it still passes. Note the tension in the summary if you want; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as written, and report its gaps instead of papering over them.
- An honestly disclosed shortfall is not a failure. Judge whether the PR is truthful about what it contains, not whether it does everything the plan hoped for.
- Treat `unclear` as the rubric's verdict rule directs. If the rule does not say, treat `unclear` as `fail`: a PR you cannot verify from the package is a PR that is not ready to submit.