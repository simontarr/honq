# Honq, Build Plan

This is the order of operations for building the demo in Claude Code on your personal Pro account. It expands the build order in CLAUDE.md into what each step produces and what to expect back. It covers the demo only. The production cloud version is described in the Project Brief and is not built here.

## Before the first session

1. Install Claude Code on your Mac. Do this when you sit down to build, and verify the current install command live in that session rather than from a stale instruction, since the install path changes. The native installer is currently the recommended route and needs no separate Node setup. Your personal Pro plan works; remember its usage pool is shared with Claude chat, which is why the planning happened on the work account.
2. Create a project folder and put it under Git. Git snapshots the project so you can roll back instantly when a change goes wrong, which is your safety net for letting the agent move fast.
3. Drop `CLAUDE.md` into the root of that folder. Claude Code reads it automatically at the start of every session.
4. Decide where you will host for testing. You need HTTPS for the camera, so plan to push to a static host that serves over HTTPS rather than opening the file directly. Pick and verify the host when you get there.

## The sequence

### Step 1: Scaffold and serve

Get a single `index.html` rendering a placeholder, served over HTTPS, and loading on your actual demo iPhone. Doing this first surfaces the camera-and-HTTPS reality immediately instead of at the end. Expect a near-empty page that proves the pipeline from your machine to your phone works.

### Step 2: QR scan and fallback (the risk, done early)

Get the camera reading a QR code and printing the raw decoded value on screen, plus a manual text field that accepts the same value typed by hand. Build nothing that depends on the scan until this works on the phone. Expect to choose a QR-decode library here; have Claude Code verify it works in mobile Safari before committing. When this step is done and the fallback exists, the demo can no longer hard-fail on stage.

### Step 3: State and storage

Establish the single state object and the load-from-localStorage-on-open, save-on-change cycle. Expect the app to remember a trivial change across a reload. This is the spine everything else writes to.

### Step 4: Seed data

Author the seeded content: a student profile placed mid-journey, the catalog of credentials with the code each QR carries, and a believable cohort of other students with handles and scores. Expect the app to open already looking like a real account in use, not a blank slate.

### Step 5: Badges, the award moment, and stacking

Build the badge case showing earned and in-progress credentials with progress indicators, wire the scan from Step 2 to award the matching badge with a small satisfying celebration, and add the stacking logic so the scan that completes a set unlocks the higher tier in the same beat. Engineer the seed state so the student is one scan away from a completion, so a single live scan pays off twice. Expect this to be the emotional center of the demo.

### Step 6: Leaderboard

Add the local, seeded, pseudonymous leaderboard with the student ranked mid-pack among the seeded cohort. Expect to see the student climbing relative to fixed competitors.

### Step 7: The escalation

Add the published-paper action that moves the student's score enough to jump to the top of the leaderboard when performed live. Expect the second of your two on-stage payoff moments.

### Step 8: Polish and rehearse

Run the full ninety-second choreography end to end on the demo device until it is clean: open mid-journey, scan to land a badge and unlock a tier, cut to the leaderboard, log the paper, jump to the top. Tighten only what serves those beats.

## Kickoff prompt for your first build session

Paste this once Claude Code is running in the project folder with CLAUDE.md in place. It points the agent at the brief and scopes the first step only, so it does not run ahead.

```
Read CLAUDE.md in this repo. That is the standing brief for this project; follow its constraints exactly, especially the hard constraints and the design invariant.

We are starting at Step 1 of the build order only. Do not build ahead of it. Create a single self-contained index.html with a placeholder Honq home screen, no real features yet, using vanilla HTML, CSS, and JavaScript and no build tooling. Then tell me, step by step and assuming I am new to this tooling, exactly how to serve it over HTTPS and load it on my iPhone so I can confirm the camera will be able to run later. Treat anything version-specific as volatile and verify it rather than reciting from memory.

When you are done, show me what you created and what to expect, and stop. Do not move to Step 2 until I confirm Step 1 works on my phone.
```

For each later step, you will write a short prompt of the same shape: name the step, restate that it should not run ahead, and ask it to show the change and stop. We can draft those together as you reach them, since each one depends on what the previous step actually produced.
