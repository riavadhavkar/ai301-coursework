# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass Condition | Weight |
|---|---|---|---|
| environment recorded | the repro report's environment section (or claim comment, in claim only mode), read against the OS/language-runtime/tool versions the issue states it was hit on | environment names the OS and the language/runtime and tool versions relevant to that project's stack (e.g., Xcode version, device-or-simulator, and chip/architecture for Apple-platform projects; interpreter/runtime and package-manager versions otherwise), and either matches what the issue targets or the difference is explicitly called out | required |
| steps followable | the repro report's steps section | a stranger with the same repo could follow the steps from a stated starting state to the trigger action without guessing an unstated step | required |
| behavior matches | the repro report's output excerpt/crash log/screenshot, read against the issue's description of the bug | the artifact shows the same error type or symptom the issue describes, produced by following the issue's own stated trigger steps, not an adjacent path — OR, when the report honestly states it could not reproduce, it shows a genuine attempt using the issue's own trigger steps and names what differed from the issue's conditions (an evidenced cannot-reproduce is a pass here, not a fail) | required |
| honest outcome | the repro report's stated conclusion, read against its own artifacts | the conclusion claims exactly what the artifact shows, no more; an evidenced "could not reproduce" is a pass, a claim of reproduction (or of broader scope) unsupported by the artifact is a fail | required |
| comms match repo conventions | the claim comment and repro comment, read against the issue and the repo's CONTRIBUTING file / issue templates, including any AI-use disclosure requirement | comments are specific to this issue and this run (not boilerplate), include required template fields, and disclose AI assistance if the repo's policy requires it | required |
| claim comment stays short | the claim comment | the claim comment states intent to work the issue without dumping full repro detail (environment, steps, logs) into it | preferred |

## Verdict rule

- accept (ready to post) only if every `required` check passes
- `preferred` checks are reported but never change the verdict
- any `required` check graded `unclear` is treated as `fail`; proof that cannot be verified is proof that is not ready to post, so a package with an `unclear` required check gets `reject`, the same as a `fail`