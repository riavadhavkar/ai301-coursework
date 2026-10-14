# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2; paste your filled week-2 voice-guide.md here, whole. It is not
re-authored and it is not graded as new work this week.

Then reread it with the plan comment in mind. Your claim and repro
comments promised and reported; a plan comment commits you to an
approach in front of the people who maintain the code. If your rules
do not cover that register (for example: how you state an approach you
are not certain of, or how you respond when a maintainer already
suggested a direction), extend the guide with what it needs. Extending
is allowed and encouraged; starting over is not required.

Live mode reads this file before your plan comment goes out and
reports any rule your draft breaks. Eval mode ignores it entirely,
because your voice is yours and carries no gold labels.
-->

## Who I am in threads

<!-- Paste your week-2 section here. -->
- full-stack platform engineer (Java/Spring Boot, AWS, Docker), working in a Python backend here
- first time contributing to someone else's open source repo
- readers should expect precise, checkable claims about what I ran

## Rules I write by

<!-- Paste your week-2 rules here, wrong/right pairs and all. Add any
rule the plan-comment register needs that your week-2 comments did
not. -->
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

## Things I never post

<!-- Paste your week-2 list here; extend it if planning tempts you
toward new ones (overpromised timelines are the classic). -->
- never claim a run I didn't perform. no "reproduced on Python 3.11" or "confirmed on Linux" unless I actually executed the steps on that exact OS/Python/container combination myself
- never present someone else's output as my own; not the issue reporter's crash log, not a classmate's console output; if it didn't come from my terminal, it doesn't go in my comment as if it did
- timelines i can't back up. no "will have a pr by friday" unless i actually control that.