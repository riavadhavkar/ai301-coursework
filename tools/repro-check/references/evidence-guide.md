# evidence guide: where proof lives in a reproduction package

## environment

- where it lives: in an eval bundle, the repo-facts block and the repro report's stated environment section; in live mode, the draft repro report's environment lines, read against the issue's own stated target (the OS/language-runtime/tool version the reporter says they hit it on, usually near the top of the issue body or in an issue-template field)
- what good looks like: the environment names the OS and the language/runtime and tool versions relevant to that project's stack — for an Apple-platform project: macOS version, Xcode version, the target OS and version (iOS/iPadOS/macOS/visionOS), the device or simulator, and the chip/architecture (Apple Silicon vs. Intel); for other stacks: the interpreter/runtime version (e.g. Node, Python), package-manager or install method, and OS — and either matches what the issue targets, or the difference is explicitly called out ("issue reports iOS 17.0, I tested on iOS 17.4, still reproduces"; "issue filed on Node 20, I ran Node 22, no version-specific behavior involved")

## steps

- where it lives: the repro report's steps section (eval bundle or draft); in live mode this is the numbered list the student will post, not a description of steps taken from memory
- what good looks like: a stated starting state (a clean checkout, a specific commit/tag/branch, or "fresh project from Xcode's template") followed by exact actions — commands run, or Xcode UI actions (scheme, target device selection, build-and-run) — ending in the precise trigger; a stranger with the same repo and the same starting state should be able to follow the list without guessing an unstated step or an assumed prior action

## behavior shown

- where it lives: the repro report's output excerpt, crash log, or screenshot, read directly against the issue's description of the bug (not against the issue's title alone)
- what good looks like: the artifact shows the same error type or symptom the issue describes (same exception/crash class, same failing symbol or code path, same wrong output) AND that behavior was produced by following the issue's own stated trigger steps — not a different path that happens to also fail; "something crashed" is not enough, the crash has to be the same crash, reached the same way; a report showing a different crash, a different subsystem, or the right symptom from an unstated trigger is an adjacent finding, not a match
- an honest, evidenced cannot-reproduce also satisfies this check: if the report made a genuine attempt at the issue's own trigger steps, shows real output from that attempt, and names specifically what differed from the issue's conditions (a different environment, a data shape that may not meet a trigger threshold, and so on), that counts as showing the behavior faithfully even though it did not occur — the failure to reproduce is itself the finding, and it is judged by whether the attempt was genuine and specific, not by whether the bug showed up

## honesty

- where it lives: the claim comment's and repro report's stated conclusion (the sentence that says what happened), read against the artifacts in that same report
- what good looks like: the conclusion claims exactly what the artifact shows, no more; "I could not reproduce this on iOS 17.4, ran the steps three times, app behaved normally each time" is an honest pass, backed by evidence, even though it doesn't confirm the bug; a conclusion that claims reproduction ("confirmed") when the pasted output doesn't actually show the issue's error, or that extends the claim beyond what was tested ("also affects visionOS" with no visionOS run in the artifacts), fails this check regardless of how confident it reads

## comms

- where it lives: the claim comment and repro comment, read against the issue they're replying to and against the repo's CONTRIBUTING file or issue/PR templates (including any AI-use disclosure requirement)
- what good looks like: the comment addresses this issue specifically — its actual error, its actual environment — rather than reading as a template filled in on autopilot; required template fields are present; if the repo's contribution policy requires disclosing AI-assisted work, the comment discloses it plainly; boilerplate ("thanks for the report! I'll take a look.") with no specific content next to it is a fail, a short comment that states a specific fact ("claiming this — reproduced the crash, repro report to follow") is a pass even though it's brief
