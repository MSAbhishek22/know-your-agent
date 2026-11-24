# Know Your Agent: a trust score that shows its work

**Perfios × Techfest, IIT Bombay — "Agentic AI in BFSI" Hackathon 2026**
**Round 1 abstract · Problem 2 — Know Your Agent**
**M S Abhishek · Team: Abhi Solo**

---

## The problem

A bank's website now gets two kinds of visitors. One is a person. The other is software acting on its own — an AI agent or a bot. Some of that software is welcome: a customer's own assistant checking a balance. Some of it is not: a fraud bot testing stolen passwords. Most defences answer this once, at the door, with a captcha or a fingerprint check, and the answer comes with no reasons. When it is wrong, a real customer is locked out of their own money, or a bot is waved in. What a risk team needs is not a single yes or no at login. It needs a score that keeps updating through the session, and that can explain itself when someone asks why.

## What I propose

I will build a small scoring service that runs beside a demo banking app and watches each session as it happens. It does not try to identify the person. It watches behaviour and keeps a running Agent Trust Score, from 0 to 100. One rule governs the whole design: no point moves without a receipt. Every change to the score is stored as a short sentence plus the raw evidence behind it, so a reviewer can open the session later and see exactly why the score went up or down.

## How it works

Four groups of signals feed the score.

**Timing.** People pause, re-read, and change pace. Scripts tend to act at near-constant gaps. I measure the spread of gaps between actions, not only the average speed.

**Path shape.** A person often checks a statement, goes back, then pays a bill. An agent tends to take the shortest path to its goal, the same way every time. I record the order of screens visited and compare the shape of the session against both patterns.

**Input cadence.** A pasted password, fields filled in exact tab order, no corrections, no pause before a large transfer — each is a weak sign on its own. Together they say more.

**Session context.** Device, network, time of day, and whether this session behaves like this account's own earlier sessions.

Each signal is a plain rule or a light statistical check, written in Python with the standard library first, the same way I built my earlier work. Each one returns points and a sentence: "Fourteen actions in nine seconds, gaps within three milliseconds of each other: −12." Fixed, published weights add the points up. There is no black box. Anyone can replay a recorded session and get the same score.

The score ends in one of three bands. High trust: the session runs. Low trust: step up the checks. The middle band is the honest one — the system says it is not sure, holds high-risk actions, and hands the session to a human reviewer with the evidence list attached. It never blocks silently on a guess.

## What is new here

The new part is the receipt. I built Verity, a research agent that answers questions only with claims tied to a source and the exact evidence for each claim. It is live at https://verity-m-s-abhisheks-projects.vercel.app, the code is at https://github.com/MSAbhishek22/verity, and its 49 tests all pass. Bot detection usually hands over a bare number from a model nobody can question. Here the score is an argument instead: every point is earned by a named signal, the weights are public, and a wrong signal can be pointed at, fixed, and re-tested. A bank's audit team will ask for that first, so I am building it in from the start, not bolting it on later.

## How I will test it

I have no access to real bank data, and I will not invent results. I will build two sets of session traces on the demo app. Human traces: volunteers using the app normally, plus scripted messy traces with pauses, typos, and backtracking. Agent traces: browser-automation and agent frameworks driving the same app, followed by a red-team round where I tune the agents to copy human timing and see which signals still hold. I will measure four things: how many agent sessions are caught, how many human sessions are wrongly flagged (the number I care about most), how early in the session the score settles, and whether reviewers shown only the evidence list reach the same call as the score. Failures will be reported by name, not hidden inside an average.

## Responsible design

No biometrics: no face, voice, or fingerprint, and no attempt to identify who the person is. The system stores timings and screen paths, not what was typed. A false alarm costs a real person access to their own money, so the design treats a wrong "bot" call as the worst error it can make. Uncertain sessions go to a human, and a flagged customer gets a review and appeal route, not a dead end. The score dies with the session; it is not kept to profile anyone.
