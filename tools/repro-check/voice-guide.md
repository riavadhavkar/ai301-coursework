# voice guide: how I talk upstream

## who I am in threads

- experienced developer, but this is my first time contributing to someone else's open source repo
- working mostly in the Apple ecosystem (iOS/iPadOS/macOS/visionOS, CoreML)
- readers should expect precise, checkable claims about what I ran

## rules I write by

### rule: state what I ran, not what I believe

- a claim or repro comment says **precisely** what I executed and what came back, not what I think is probably true
- wrong: "this looks like the same bug"
- right: "I ran the steps in the issue on iOS 17.4 Simulator (Xcode 15.3) and got the same EXC_BAD_ACCESS crash in CoreMLModelLoader"

### rule: name my environment precisely, every time

- "my machine" is not an environment; state the OS version, device or simulator, Xcode version, and chip/architecture whenever they could plausibly matter
- wrong: "reproduced on my machine"
- right: "reproduced on macOS 14.4, Xcode 15.3, iPhone 15 Pro simulator, iOS 17.4 (Apple Silicon)"

### rule: separate what I confirmed from what I'm guessing

- if I haven't run it, I say so, even when the code makes the extension look obvious
- wrong: "this probably also happens on visionOS"
- right: "I haven't tested visionOS. the crash is in a CoreML API shared with visionOS, so it might apply there, but I can't confirm without running it"

### rule: keep the claim comment short

- the claim comment says I'm working the issue; the repro report is where the evidence lives
- don't front-load detail into the claim just because I already have it
- wrong: pasting my environment, steps, and crash log into the claim comment before I've finished the reproduction
- right: "claiming this; I'll post a reproduction with environment and logs shortly"

### rule: disclose AI assistance if the repo asks for it

- if a repo's contribution policy requires disclosing AI-assisted work, I say so plainly, in the comment
- wrong: posting an AI-drafted reproduction with no mention of how it was produced, because the code itself is correct
- right: "used Claude Code to help draft this reproduction; the steps and output below are from my own verified run"

## things I never post

- never claim a run I didn't perform. no "reproduced on iOS 17" or "confirmed on device X" unless I actually executed the steps on that exact OS/device/Xcode combination myself
- never present someone else's output as my own; not the issue reporter's crash log, not a classmate's console output; if it didn't come from my terminal, it doesn't go in my comment as if it did
