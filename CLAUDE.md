# CLAUDE.md

This file is the standing brief for building Honq. Read it at the start of every session and stay inside it. Honq is a hackathon prototype, due June 18, built solo by someone who is technically literate but new to shipping apps and to modern tooling. Explain unfamiliar steps plainly the first time, then assume they are retained.

## What Honq is

A gamified tracker for the Honors College's beyond-the-classroom formation. A student scans a QR code a professor displays, earns a badge on their own phone, stacks badges into higher tiers, competes on a cohort leaderboard, and presents the result to an advisor as proof of completion. The student owns the record; the institution does not maintain it.

## What we are building right now

A demo prototype for a five-minute pitch, not a production system. The whole job of the build is to make one ninety-second story tangible and believable. A small thing that works and tells a clear story beats an ambitious thing that is half-built. Flag scope creep and fold the flag into the work; do not front-load it.

## Hard constraints (do not violate without asking)

- Single self-contained `index.html`. No backend, no server-side logic, no database.
- Persistence is browser localStorage only.
- No build tooling and no package installs. Load any external library through a CDN script tag.
- Must be served over HTTPS, because the camera requires it.
- Vanilla HTML, CSS, and JavaScript. Do not introduce a framework or a bundler.

## Design invariant (the reason the project exists)

Honq is non-authoritative. It is a demonstration tool the student holds, never the institutional system of record. Do not add anything that would make the institution the maintainer or controller of student data: no institutional login, no sync to an institutional system, no export that ships data to a server we control. If a requested feature would cross that line, stop and raise it before building.

## Build order

Build in this sequence. Do not jump ahead; each step de-risks the next.

1. Scaffold `index.html`, get it serving over HTTPS, confirm it loads on the actual demo iPhone.
2. QR scan first, with a manual code-entry fallback. This is the only piece that can break the demo on stage, so prove it before building anything that depends on it. Pick a current, well-maintained client-side QR-decode library and verify it works in mobile Safari before committing to it.
3. The state model and localStorage load/save.
4. Seed data: a student profile placed mid-journey, the catalog of credentials and their codes, and a seeded cohort for the leaderboard.
5. The badge case with progress indicators, the award-on-scan moment with a small satisfying celebration, and the stacking logic so a completing scan unlocks the higher tier in the same beat.
6. The local, seeded, pseudonymous leaderboard with the student ranked mid-pack.
7. The second score action: log a published paper, which jumps the student to the top.
8. Polish pass: the full ninety-second choreography runs clean end to end, rehearsed on the demo device.

## In scope for the demo

Home view with seeded standing; badge case with earned and in-progress credentials and progress indicators; live QR scan with fallback; stacking; seeded pseudonymous leaderboard; a published-paper action that moves rank; a small award celebration.

## Out of scope (do not build)

Real authentication, any backend, cross-device sync, achievement verification, a professor-side QR generator (two pre-made codes suffice), anti-spoofing on the QR, Banner or LMS integration, more than one fully built formation domain.

## How to work with me

Lead with the change or recommendation, then the reasoning. Show me what you changed and why, the way you would in a pull request. Treat OS and browser behavior as volatile: verify version-specific steps against a current source rather than reciting them, and tell me plainly when something is unverified. When code or a tool is unfamiliar to me, say what each step does and why. No em-dashes, no horizontal rules in anything you write. Do not manufacture disagreement; raise a concern only when it is substantive, and fold it into the work.
