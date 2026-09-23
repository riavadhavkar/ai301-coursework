# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | repo-facts block: "last push to any branch" date vs. the bundle's `captured` date, and the "maintainer first-response sample" list (days to first owner/member/collaborator comment, out of up to 5 sampled issues). | pass if last push to any branch is within 60 days of the captured date, AND at least one entry in the maintainer first response sample shows a numeric response time (any value, no cap), OR fewer than 3 of the up to 5 sampled entries have a numeric value at all (meaning most sampled issues simply didn't need a response, not that the maintainer is unresponsive). fail if last push is more than 60 days stale, OR every single sampled entry shows "no maintainer comment in thread." | required |
| repo-in-use | repo-facts block: "latest release" date, "last push to any branch" date, "archived:" flag, and star count on the repo line. | fail immediately if `archived:` is true. otherwise pass if latest release is within 180 days of the captured date, OR last push to any branch is within 30 days of the captured date. fail if neither holds (stale release and stale push = likely dead even if maintainer technically alive). | required |
| unclaimed | repo-facts line "this issue: assignees: ...; linked PRs: ..." (with state per PR), AND every comment in the thread, AND the label-freshness event lines. | pass if the assignees list is empty AND no linked PR is currently open AND no claim signal in the thread is both recent (within 180 days of the captured date) and unresolved. a claim comment or assignment from more than 180 days ago is stale and does not count against this check if any linked PR tied to that claim has since closed unmerged (abandoned attempt) with no newer claim after the closure -> treat the issue as unclaimed in that case. fail if a linked PR is currently open, OR a claim within the last 180 days has no subsequent abandonment or closure. | required |
| scope-fits | issue body/title, full comment thread, AND linked-PR states + count of past claim/unassign or claim/abandon cycles (from the "linked PRs" evidence and label-freshness event lines). | pass only if all three hold: (1) the issue describes one cohesive piece of work centered on a single underlying bug, feature, or topic. multiple sub-items, multiple files to touch, multiple named sub-causes of one bug, or a short list of "additional/optional ideas" for how to fix it still count as ONE bounded task as long as they all serve the same single stated problem/outcome (ex. "add doc page + update 4 related pages that all point to it," "add rule previews for remove-identity, fuse-spiders, remove-self-loops," or "freeze bug with 2 diagnosed causes plus 3 optional follow-up optimizations to consider"). Only fail this sub-condition if the item is actually a broad/tracking/umbrella issue spanning unrelated areas of the codebase, or bundles two or more genuinely separate features/problems that don't share one fix; (2) there is no unresolved product or design question. reject if the body contains hedge language like "TBD," "out of scope for v1," or similar markers showing the actual deliverable isn't settled yet; (3) there is no history of 2 or more abandoned or closed linked PRs, or repeated claim-then-unassign/claim-then-abandon cycles, on this issue. that pattern signals real difficulty even when the current thread reads cleanly. fail if any of the three sub-conditions fails, or if the thread shows live unresolved maintainer disagreement about the right fix. | required |
| policy | repo-facts "contribution policy (CONTRIBUTING.md...)" line. | pass unless the policy text states an outright ban on AI-assisted/AI-generated contributions (e.g. "we do not accept AI-generated code or documentation"). a policy that merely requires disclosure, review, testing, or understanding of AI-assisted work (e.g. "AI tools welcome, you must review and understand contributions" or "must personally test and explain every change") still passes. fail only on an explicit ban. | required |

## Verdict rule

Accept only if all five required checks (`maintainer-alive`, `repo-in-use`,
`unclaimed`, `scope-fits`, `policy`) pass. If any required check is `unclear`, treat
it as `fail` for that check — a first issue whose liveness, activity,
claim status, or scope cannot be verified from the bundle is not a first
issue to take. There are no preferred checks in this rubric, so nothing
ranks accepted issues beyond the fit profile in `scope.md` (live mode
only).