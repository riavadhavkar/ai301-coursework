# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->
1. Read the issue context first (eval: the issue section of the bundle; live: the issue body and thread). Note what the issue asks for and what "done" means.
2. Read the maintainer signals next (thread highlights, or the live thread). Note any suggested direction, any claimed approach, and any request made of contributors. This must come before the plan comment, because the comment is graded against it.
3. Read the repro-evidence block (live: the student's posted repro comment). Note the exact steps, the exact observed behavior, and the output or error. This must come before the plan, because the diagnosis and test plan are graded against it.
4. Read the repo-facts block (live: CONTRIBUTING, PR template, AI-use policy in the repo). Note every stated template, ask, and policy.
5. Read the candidate plan last, then the candidate plan comment. Note the stated cause, scope, files, approach, test plan, risks, and Deviations.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->
1. Diagnosis and grounding: copy the plan's stated cause in one line, and copy the repro behavior it must explain in one line.
2. Scope: copy the in-scope statement, the not-in-scope line, and the list of files or areas. If any are missing, record "absent".
3. Executability: list each file or function the plan names and the change planned in each. Record the order of work if given.
4. Test plan: copy the commands or steps and the expected before and after output. Record whether they map onto the repro steps.
5. Honesty: copy the risks, unknowns, and the Deviations section. Record each claim stated as certain and whether the repro evidence supports it.
6. Comms: copy the sentences in the comment that answer a maintainer signal or follow a repo convention. Record "none" if there are none.
7. In eval mode use only the bundle text. In live mode use only what the drafts contain and quote, never other files in the working directory.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->
1. Run the checks in the order they appear in rubric.md.
2. For each check, use only the evidence gathered for it and apply the pass condition exactly as written.
3. If the evidence for a check is genuinely absent from the package, grade it fail when its absence is the failure (for example no test plan). Grade it unclear only when evidence exists but is too thin to decide.
4. Write one line per check: the fact or quote that decided it.
5. A check may be graded from the gathered notes without re-reading the whole package, except cause-not-symptom, which must be re-read against the repro output.
6. If a check passes by its stated condition but feels wrong, it still passes. Note the tension in the summary.
7. If this procedure is silent on a step, say so in the summary instead of inventing a step.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->
1. Apply the rubric's verdict rule: accept only if every required check passes.
2. Count every unclear grade as fail.
3. Preferred checks are reported but never change the verdict.
4. Name the deciding check in the output: on reject, the first required check that failed or was unclear, with one quoted line of evidence; on accept, the check that came closest to failing.
5. Emit the JSON block last, with one entry per check and the verdict.