# Honq

Honq is a web app for students in the Honors College at Regent University that tracks the co-curricular and undergraduate research requirements: colloquia, lectures, mentoring, and publications that make up the beyond-the-classroom side of an Honors education.

## The problem

Tracking these requirements is a mess in practice. At one end, programs run them out of spreadsheets, brittle rows that someone maintains by hand. At the other, they over-build: a compliance layer stacked on institutional Microsoft tools, or an elaborate Airtable base that staff can see and students cannot. Either way the student, the person the requirement actually belongs to, has no hold on it.

Honq makes the requirement the student's own: they carry it, and they are responsible for meeting it. The game layer that motivates them is deliberately simple, closer to a problem set than a slot machine, which fits the people it is built for.

## How it works

A student opens Honq on their phone and scans a QR code when they complete a qualifying co-curricular event. That earns a badge worth points, or "honks." Badges accumulate into credentials that mark progress through the requirements, and a leaderboard shows where a student stands relative to their cohort.

Progress is reported in person: the student pulls up Honq at each required meeting with their honors advising specialist, who confirms the items against the program's requirements.

## Where AI comes in

AI is in Honq in two places. The first is in how it was built. I made Honq with AI coding assistance rather than a development team, which is how one person took it from idea to working prototype inside a two-week challenge.

The second is in how it runs. When a student meets their honors advising specialist, the advisor view shows a short AI summary of the student's logged activity, so the specialist can see where things stand at a glance and keep the meeting on the student rather than the record. The summary does the compressing; the specialist still does the verifying.

## Why embodied?

In the Regent University Honors College, we think the ultimate higher education experience is embodied. It happens among people in the same place at the same time, and that presence is the point. Honq serves that and then gets out of the way.

The QR code is only shown by a trusted faculty member in the same space, everything the app rewards is something that happened in person, and institutional verification only takes place in person with the students' honors advising specialist.

## Why private?

Honq's central design is that **it never becomes an institutional education record**. The student holds it. The institution does not own its data or keep records through it, and a human advisor, not the app, confirms that a requirement was actually met.

Most student software treats privacy as something to manage after the fact, with consent screens and policy layered onto a system that is already a record. Honq removes the premise: there is no institutional record here for student-privacy law, FERPA included, to govern.

## Why the geese?

Canada geese are the unofficial spirit animals of the Honors College. They're everywhere on campus, everyone has an opinion about them, and they've been a source of Honors in-jokes for years.

## Running it

Honq is hosted on GitHub Pages at [https://simontarr.github.io/honq/](https://simontarr.github.io/honq/). Open it in a browser, ideally on a phone since that's where the scanning happens, and add it to your phone's home screen for the full app experience. The source is in this repository, readable without downloading anything, and the demo is self-contained, with seeded data, so it runs without an account or a backend. A short video walkthrough is linked from the Devpost submission.

If you'd rather see a walkthrough, the demo video is on [YouTube](https://youtu.be/4p3fQ7krgGc).

## Status

This is a working prototype. The demo keeps everything in the browser, which is all it needs to show the flow. A production version will hold the same posture: a pseudonymous, student-held credential with thin records and no link back to institutional identity, the advisor still in the loop.

## About

Honq is a submission to the AI for Learning & Development Build Challenge, hosted by the Regent University Institute for Technology Innovation and the AI Collective Hampton Roads Chapter. It was created by Dr. Simon Tarr, an artist, designer, and academic who serves as the Dean of the Honors College at Regent University. He'd like to use this project (and what he learns about co-programming with AI) to help him directly solve problems that drive him up a wall.
