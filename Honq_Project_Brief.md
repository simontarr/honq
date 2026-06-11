# Honq, Project Brief

Owner: Dean, Honors College
Status: concept and demo stack locked; production architecture directionally set, not built
Last updated: June 11, 2026

## What Honq is

Honq is a gamified tracker for the beyond-the-classroom formation that defines the Honors College: colloquia, published work, and the other co-curricular requirements students complete toward their HC obligations. A student attends a colloquium, scans a QR code the professor displays, and earns a badge on their own device. Completing a set of requirements stacks into a higher badge. A cohort leaderboard lets students see who is doing more, and pushes them to do more themselves. At semester's end the student presents their record to their advisor as proof of completion.

The design choice underneath every feature is that the student owns the record and the institution does not maintain it. That single decision is what makes Honq inexpensive to run, free of heavy IT involvement, and structurally outside the FERPA custody problem that has made microcredentialing expensive and cautious elsewhere.

### Value statement (one sentence)

Honq gamifies the beyond-the-classroom formation that defines the Honors College, in a mobile experience the student owns and presents to an advisor rather than one the institution maintains.

### Positioning hook (for the pitch)

It delivers real microcredential mechanics without the Canvas price tag and without the FERPA custody burden, because the student holds the record and the physical campus is the trust layer.

## Role definition and FERPA posture

This section is written to do double duty: it anchors the decisions log, and it is the framing to put to General Counsel. It is not legal advice. It is the structure counsel would evaluate, stated in the terms that matter to that evaluation.

### Honq's role

Honq is a required, student-held demonstration tool. Honors students are required to track their formation in Honq and to present it to their advisor to demonstrate that a degree-required process has been completed. The requirement governs what the student must do and must show. It does not make the institution the operator of a record system.

### What Honq is not

Honq is not the system of record for completion. The authoritative record of "requirement met" remains in the institution's existing system, created by the advisor the way it always has been. Honq is the motivational and demonstration layer that sits on top of that existing record, not a replacement for it.

### How verification works

Completion is verified in person. The student opens Honq, logged into their own account on their own device, and the advisor, who knows the student, inspects it. Physical presence supplies the identity binding. The advisor then records the conclusion in the institution's existing system. Honq's data is never ingested or retained by the institution.

### Data posture

The student holds their data. If a production version syncs across devices through a hosted account, that account is pseudonymous: the server holds a self-chosen handle and the student's badges, and never holds the map from handle to a named student. That map lives only with the student and is supplied in person at the advisor meeting. Stored data is kept deliberately thin (handle, badge identifiers, scores, timestamps) so that a small cohort plus rich detail cannot be used to re-identify a student.

### The three lines that must hold

The posture above keeps Honq outside FERPA's definition of an education record (records directly related to a student and maintained by the institution or a party under its control; 20 U.S.C. § 1232g, 34 CFR Part 99). It holds only if all three of these stay true:

1. Honq does not maintain records for the institution. The records stay with the student; the institution's conclusion stays in the institution's existing system.
2. The institution does not control Honq's data. If Regent operates Honq as its official, controlled record system, custody returns and the posture collapses.
3. The human advisor is the actual verifier, not a rubber stamp. If in practice no one inspects and the badge alone is the decision, Honq has functionally become the record-maker regardless of policy language.

### Why mandating is still safe

Requiring a tool is not what brings it under FERPA; maintaining or controlling its records is. A third party becomes covered only when the institution outsources a function to it and the party operates under the institution's direct control with respect to the use and maintenance of education records (34 CFR § 99.31(a)(1)(i)(B)). A mandated, student-held tool that the institution neither controls nor retains records from does not meet that test. The closest familiar analogue is a required portfolio: a program can require a student to assemble and present one without becoming the custodian of it.

This is the genuinely contestable zone, where reasonable counsel could weigh risk differently. The Dean authoring the policy is a real advantage here, because FERPA analysis looks at the actual relationship and control, and the policy language is part of that relationship. Policy that explicitly frames Honq as a student-held demonstration tool, names the existing system as the record of completion, and assigns verification to the advisor as a human act is itself load-bearing, not just paperwork.

## Architecture

### Demo build (what you build for June 18)

A single self-contained web app. It runs entirely in the browser on the student's phone (client-side only), with no server of ours running any logic and no database. Persistence is the browser's localStorage, which keeps data on the device. The app is hosted as static files on a host that serves over HTTPS, which the camera requires. There is no build tooling: one `index.html`, with any external library loaded through a CDN script tag.

The data model is one structured object the app reads from localStorage on load and writes back on change: the student profile and chosen handle, earned badges, progress toward in-progress badges, the catalog of credentials with the code each QR carries, and a seeded roster of other students with handles and scores for the leaderboard.

The credentialing maps to the issuer, holder, verifier model. The professor displaying the QR is the issuer attesting an event happened. The phone holding the badge is the holder. The advisor inspecting later is the verifier. For the prototype the QR carries a known string with no cryptographic signature, which is fine because the stakes are low and human trust does the work. The real version would have the issuer sign a token the holder cannot forge, which is what the verifiable-credentials standards add.

### Production direction (not built for the demo)

The durable, undergrad-proof, FERPA-clean version replaces device-only storage with a pseudonymous hosted account that syncs across devices, so a lost or wiped phone does not lose a student's progress. The hosting is almost certainly a managed backend service, where a provider runs the database, sync, and account plumbing and you configure rather than operate it, which keeps Regent IT out of it and the institutional argument intact. Whose budget pays for the host and where the server physically sits are both irrelevant to FERPA; what matters is that the data is genuinely de-identified and that the institution neither controls nor relies on an identifiable record. Specific service and current free or low-cost tiers should be selected and verified at build time, since those offerings shift.

### The one real risk: the QR scan

Everything else is low-risk assembly. The scan is the only piece that can fail in a way that breaks the demo on stage. It needs HTTPS, a camera-permission grant, and a client-side decoding library, and mobile-browser behavior here is volatile enough to verify live rather than trust to memory. Two consequences for the build: build the scan first, and the moment it works, add a manual code-entry fallback so a misbehaving camera in the room can never hard-fail the demo.

## Demo scope

Sized to one ninety-second story performed live: open on the student mid-journey, scan a colloquium QR and watch a badge land and a higher tier unlock in the same beat, cut to the leaderboard where she has risen but is not first, log a published paper and watch her jump to the top.

### Must have

1. A home view showing current standing, pre-seeded so the student arrives mid-journey rather than at zero.
2. A badge case showing earned and in-progress credentials, with a visible progress indicator ("3 of 4 colloquia toward your Semester Formation badge").
3. The live QR scan that awards a badge, working on the actual demo device, with a manual-entry fallback.
4. Stacking logic, so the scan that completes a set unlocks the higher badge in the same moment.
5. A seeded, pseudonymous leaderboard, entirely local, with the student ranked mid-pack.
6. A second score-moving action (log a published paper) that visibly jumps the student to the top.

The award moment needs a small, satisfying celebration. That micro-moment is the gamification the whole pitch rests on, so do not ship a silent state change there.

### Nice to have

A signup flow worth ten seconds of "notice there is no institutional login." A richer award animation. A few greyed, locked tiles implying other formation domains without building them. An advisor or dean inspection view, though that beat can be narrated for free.

### Cut

Real authentication, any backend, cross-device sync. Achievement verification. A professor-side QR generator (two pre-made codes suffice). Anti-spoofing on the QR (the answer to a judge is the human-trust line, not engineering). Banner or LMS integration, whose absence is the point. Multiple fully built domains (one track told completely beats five shown halfway, and the demo time carries only one).

## Decisions log

Append new decisions here with a date. Do not relitigate settled items without a reason that changes the underlying facts.

- 2026-06-11 Name is Honq.
- 2026-06-11 Concept locked: a self-sovereign, embodied, motivational co-curricular formation tracker for the Honors College.
- 2026-06-11 Honq is non-authoritative by design. It is a demonstration tool, not the system of record. This is the load-bearing decision; data loss and FERPA exposure both stay contained only while it holds.
- 2026-06-11 FERPA approach: avoid becoming an education record through student custody, institutional non-control, and human verification. Mandate the process, not the record.
- 2026-06-11 Identity: pseudonymous handle for the leaderboard; identity binding for advisor verification supplied by physical presence and never stored.
- 2026-06-11 Demo stack: single self-contained static web app with localStorage, no backend, served over HTTPS. No build tooling; external libraries via CDN.
- 2026-06-11 QR mechanic for the prototype: known-string codes, no cryptographic signing; human trust over anti-spoofing.
- 2026-06-11 Banner and LMS integration excluded, by the Dean's authority over HC policy.
- 2026-06-11 Production direction set (not built): pseudonymous account with cross-device sync on a managed backend service. Hosting payer and server location are irrelevant to FERPA; de-identification and non-control are what matter.
- 2026-06-11 Build approach: planning and specs on the work account; build with Claude Code on the personal Pro account; build the scan and its fallback first.
