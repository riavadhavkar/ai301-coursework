# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
maintainer-alive: pass — last push 2026-09-16 (6 days old); 0/8 sampled issues show a maintainer comment, carve-out applies
repo-in-use: pass — not archived; last push 6 days before capture
unclaimed: pass — no assignees, no linked PRs; only claim comment is student skonda29 (NONE), house rule permits
scope-fits: pass — single config-field mismatch (settings.redis_host -> redis_url) with clear repro steps
policy: pass — no AI restriction in CONTRIBUTING.md

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "last push 2026-09-16 (6 days old); 0/8 sampled issues show a maintainer comment, carve-out applies"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "not archived; last push 6 days before capture"},
      {"name": "unclaimed", "grade": "pass", "evidence": "no assignees, no linked PRs; only claim comment is student skonda29 (NONE), house rule permits"},
      {"name": "scope-fits", "grade": "pass", "evidence": "single config-field mismatch (settings.redis_host -> redis_url) with clear repro steps"},
      {"name": "policy", "grade": "pass", "evidence": "no AI restriction in CONTRIBUTING.md"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. i first did a cheap sanity check, `--only issue-01,issue-02`: 1/2 agreement. issue-01 disagreed (gold accept, rubric reject) on `maintainer-alive` and `scope-fits`.
2. then did a full run, `--save-run`: 12/20 agreement. weakest category: clear-accept (2/8), with over-rejects on `maintainer-alive` and `scope-fits`, plus two false accepts on `unclaimed`/`scope-fits` combined.
3. diagnosed the 6 disagreeing clear-accept issues plus 2 false accepts by inspecting the raw bundle values (push dates, first-response samples, linked-PR states) for issue-01, issue-04, issue-09, issue-15, issue-20.
4. revised `maintainer-alive` (stopped hard-failing on a mostly-silent response sample), `unclaimed` (added staleness decay so an old claim tied to a since-closed PR no longer counts as active), and `scope-fits` (stopped treating enumerated sub-items as multi-scope, added detection for "TBD"/"out of scope" hedge language, added abandoned-PR/claim-churn history as a difficulty signal).
5. did a partial re-check, `--only issue-01,issue-04,issue-09,issue-15,issue-20`: all 5 previously-disagreeing issues now matched gold.
6. then did a full run, `--save-run` which revealed two new regressions from the round-4 rewrite — issue-01 and issue-19 newly failed `scope-fits` (a multi-part-but-single-task issue now read as too literally multi-scope), and issue-12 flipped to `accept`, losing the entire `policy` category (0/1) because the rubric had no check reading the repo's contribution policy at all — issue-12's repo (bookwyrm) bans AI-generated contributions outright and nothing in the rubric caught it.
7. added a fifth required check, `policy` (reads the CONTRIBUTING.md contribution-policy line from repo-facts, fails only on an explicit AI-contribution ban), and further tightened `scope-fits`'s first sub-condition so multi-file/multi-bullet issues only fail when they're a genuine tracking issue or bundle unrelated problems, not whenever they touch more than one thing.
8. did a partial re-check on the 3 regressed issues (issue-01, issue-12, issue-19): all matched gold.
9. lastly did a full run, `--save-run`: 20/20 agreement, all categories at full marks. This is the run committed to `eval-run.txt`.

**Issue analysis**

`issue-09`. my rubric's first pass rejected it; gold says accept.

2 checks were responsible. `maintainer-alive` failed because the maintainer first-response sample for this repo had only one numeric value (32.9 days) against a 5-issue sample, and my original threshold hard-required a response within 30 days, so the one real data point I had exceeded the cap, even though the repo's last push was only 1 day old. `unclaimed` also failed: the thread had a 2022 comment where a contributor said they'd take the issue and a maintainer acknowledged it, but the linked PR (#11627) closed unmerged and nothing happened for 3+ years after. my original rubric read that old exchange as a live claim with no staleness decay.

gold treats both differently: it accepts on `maintainer-alive` because a fresh push plus a sparse-but-nonzero response sample reads as "alive, just quiet," not "dead," and it treats the 2022 claim as expired since the linked PR closed and years passed with no follow-up. I revised both checks to match this reasoning — `maintainer-alive` no longer hard-fails on a single over-30-day data point when most of the sample has no response at all, and `unclaimed` now discounts a claim if its linked PR has since closed and no newer claim followed.

**Check rationale**

From `rubric.md`, the `scope-fits` check (final version, after two revision rounds):

> "Pass only if all three hold: (1) the issue describes one cohesive piece of work centered on a single underlying bug, feature, or topic. Multiple sub-items, multiple files to touch, multiple named sub-causes of one bug, or a short list of "additional/optional ideas" for how to fix it still count as ONE bounded task as long as they all serve the same single stated problem/outcome (ex. "add doc page + update 4 related pages that all point to it," "add rule previews for remove-identity, fuse-spiders, remove-self-loops," or "freeze bug with 2 diagnosed causes plus 3 optional follow-up optimizations to consider"). Only fail this sub-condition if the item is actually a broad/tracking/umbrella issue spanning unrelated areas of the codebase, or bundles two or more genuinely separate features/problems that don't share one fix; (2) there is no unresolved product or design question — reject if the body contains hedge language like "TBD," "out of scope for v1," or similar markers showing the actual deliverable isn't settled yet; (3) there is no history of 2 or more abandoned or closed linked PRs, or repeated claim-then-unassign/claim-then-abandon cycles, on this issue. That pattern signals real difficulty even when the current thread reads cleanly."

this check went through 2 rewrites. the first pass judged scope almost entirely from the surface text of the current thread (bullet-point structure, comment tone, apparent argument) which missed two failure modes gold was testing for: an issue that reads clean today but has a multi-year history of abandoned attempts (issue-15), and an issue with tidy structured sections hiding an unresolved product decision behind "TBD" language (issue-20). that first rewrite fixed issue-01/04/15/20 but was still too literal about "one concrete change," and a full-run regression check caught it flipping issue-01 and issue-19 to false rejects — multi-file or multi-cause issues that were still genuinely one task got misread as scope creep. the second rewrite narrowed sub-condition (1) to only fail on an actual tracking issue or genuinely unrelated bundled problems, not on touching more than one thing.

**Trade-offs**

the `maintainer-alive` carve-out I added — passing when fewer than 3 of the sampled issues have any numeric response at all, rather than requiring a response under 30 days — gives up the ability to distinguish "maintainer is slow but present" from "maintainer genuinely never responds to anyone." a repo where the only sampled response was a single 32.9-day outlier and every other sampled issue has zero engagement now passes maintainer-alive on a fresh push alone. that's the correct call for the issues in this eval (confirmed by the live run on #62, where 0/8 sampled issues had any response but the repo's push was 6 days old), but it means my rubric would also accept a repo where the maintainer is present in commits but functionally never engages with issue threads — a real newcomer risk that this check no longer screens for on its own. I'm relying on `scope-fits` and `unclaimed` to catch other symptoms of that pattern (unaddressed claim churn, abandoned PRs) rather than catching maintainer disengagement directly.

separately, my first "passing" rubric draft had no check at all for the repo's contribution policy — I only caught this because a later full run flipped issue-12 to a false accept and zeroed out an entire scored category (policy, 0/1). the gap was a missing check entirely, which is a different class of mistake than the tuning issues above: nothing in my rubric was reading the CONTRIBUTING.md line at all. I added `policy` as a fifth required check afterward, but it's worth naming as a trade-off in how I built this — I derived checks reactively from eval disagreements rather than deriving the full check list from the four (plus policy) families up front, so a category with no disagreeing example in my early runs could have stayed silently uncovered.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I chose issue #62 because it's the smallest, most self-contained change of the three my skill accepted — a single mismatch between a config field the health check references (`settings.redis_host`) and what actually exists on Settings. Given this is my first real open-source contribution and I'm still getting comfortable with the tooling (git, the claim/PR flow, the CI expectations of a repo I didn't write), I wanted low implementation risk on my first pick so I can focus on learning the process itself rather than debugging a hard bug.
2. My rubric's verdict correctly identified that the repo is alive despite a near-silent maintainer response sample (0/8 sampled issues had a maintainer comment) — the fresh push date carried the liveness signal instead. What I weighed beyond the rubric's output: the issue has clear repro steps stated directly in the body, which isn't something any of my checks explicitly score but matters a lot for how fast I can actually get moving in Unit 2.
3. I expect the claim itself to be low-difficulty — no currently open PR or assignee, and the one existing claim comment from another student doesn't block me per the Path Review house rule. The main risk I anticipate is confirming exactly which field name (`redis_host` vs `redis_url`) the maintainer actually wants used, since the issue body only states the mismatch, not the intended fix — that's a question I may need to ask in my claim comment in Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
