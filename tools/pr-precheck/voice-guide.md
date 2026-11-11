# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2 and carried it through week 3; paste your filled voice-guide.md
here, whole. It is not re-authored and it is not graded as new work
this week.

Then reread it with the PR in mind. Your comments promised, reported,
and committed to an approach; a PR title and description ask a
maintainer to spend review time on your work. If your rules do not
cover that register (for example: how a title earns its thirty
seconds, how a description promises exactly what the diff contains,
how a disclosed shortfall is worded so it reads as honesty rather
than apology), extend the guide with what it needs. Extending is
allowed and encouraged; starting over is not required.

Live mode reads this file and holds your draft PR title and
description against it, reporting any rule your draft breaks. Eval
mode ignores it entirely, because your voice is yours and carries no
gold labels.
-->

## Who I am in threads

<!-- Paste your section here. -->
- full-stack platform engineer (Java/Spring Boot, AWS, Docker), working in a Python backend here
- first time contributing to someone else's open source repo
- readers should expect precise, checkable claims about what I ran

## Rules I write by

<!-- Paste your rules here, wrong/right pairs and all. Add any rule
the PR register needs that your comments did not. -->
### rule: state what I ran, not what I believe

- a claim or repro comment says **precisely** what I executed and what came back, not what I think is probably true
- wrong: "this looks like the same bug"
- right: "I ran the steps in the issue with uvicorn and redis:7-alpine and got the same ConnectionError from the Redis client in /health"

### rule: name my environment precisely, every time

- "my machine" is not an environment; state the OS, chip/architecture, Python version, server, and container image versions whenever they could plausibly matter
- wrong: "reproduced on my machine"
- right: "reproduced on macOS, Apple Silicon, Python 3.12, uvicorn, redis:7-alpine via docker compose"

### rule: separate what I confirmed from what I'm guessing

- if I haven't run it, I say so, even when the code makes the extension look obvious
- wrong: "this probably also breaks with a Redis cluster"
- right: "I haven't tested a Redis cluster. I only ran against redis:7-alpine from the compose file, so I can't say whether the same code path is hit there"

### rule: keep the claim comment short

- the claim comment says I'm working the issue; the repro report is where the evidence lives
- don't front-load detail into the claim just because I already have it
- wrong: pasting my environment, steps, and crash log into the claim comment before I've finished the reproduction
- right: "claiming this; I'll post a reproduction with environment and logs shortly"

### rule: disclose AI assistance if the repo asks for it

- if a repo's contribution policy requires disclosing AI-assisted work, I say so plainly, in the comment
- wrong: posting an AI-drafted reproduction with no mention of how it was produced, because the code itself is correct
- right: "used Claude Code to help draft this reproduction; the steps and output below are from my own verified run"

### rule: state what i'm unsure about plainly.

- wrong: "i think maybe the issue could possibly be in parser.py"
- right: "the repro points at parser.py line 42. i haven't confirmed whether the same path is hit by X, so i'll check that first."

### rule: acknowledge any direction a maintainer already suggested.

- wrong: "here's my plan." (ignores the earlier suggestion)
- right: "@maintainer suggested fixing this in the validator. my repro agrees, so i'm planning the change there."

### rule: a PR title says what changes and where, in one line

- the title earns a maintainer's first thirty seconds: name the behavior fixed and the area, not the process I followed
- wrong: "Fix issue" or "Updates to health check"
- right: "Fix /health Redis probe to read settings.redis_url (#62)"

### rule: the description promises exactly what the diff contains

- every bullet under Changes matches a hunk in the diff, and every hunk is covered by a bullet
- wrong: "also cleans up related health check code" (when the diff does not show that, or when it does and the plan never said so)
- right: "changes the Redis probe in api/routes/health.py to build its client from settings.redis_url, removes attr-defined from the api.routes.health mypy override in pyproject.toml, and adds tests/unit/test_health.py"

### rule: a disclosed shortfall is worded as a fact, not an apology

- say what is left out or limited, what I did about it, and stop
- wrong: "sorry, I wasn't able to fully test this, hopefully it's okay"
- right: "make typecheck fails on unmodified main in my Python 3.12 setup, so I ran mypy with --python-version 3.12 locally and have not seen CI's 3.11 run"

### rule: tick only the boxes I can back with pasted output

- a Testing box is ticked only when the output for it is pasted in the description or test_evidence.md
- wrong: ticking "CI passes" before the PR is open
- right: leave the CI box unticked and say CI runs after opening

### rule: describe AI use in the Notes for Reviewers in my own words

- say what I asked the AI to do, what I wrote or decided myself, and how I verified the result
- wrong: "AI was used in this PR."
- right: "used Claude to help read health.py and draft the unit test scaffolding; I chose redis_url over adding two Settings fields, wrote the plan, and checked both tests by running them with and without the fix."

## Things I never post

<!-- Paste your week-2 list here; extend it if PR descriptions tempt you
toward new ones (claiming coverage the evidence does not show is the
classic). -->
- never claim a run I didn't perform. no "reproduced on Python 3.11" or "confirmed on Linux" unless I actually executed the steps on that exact OS/Python/container combination myself
- never present someone else's output as my own; not the issue reporter's crash log, not a classmate's console output; if it didn't come from my terminal, it doesn't go in my comment as if it did
- timelines i can't back up. no "will have a pr by friday" unless i actually control that.
- never claim test coverage the evidence doesn't show. no "fully tested" or "all checks pass" unless the pasted output says so, and a failing or unrunnable check gets named with its reason
- never claim the PR fixes more than the diff changes. a related problem I noticed but did not fix is named as out of scope, not folded into the claim
- never leave a change in the diff that the description and plan don't account for