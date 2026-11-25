# Know Your Agent — a trust score where every point comes with a reason you can read

**Perfios × Techfest, IIT Bombay — "Agentic AI in BFSI" Hackathon 2026**
**Round 1 abstract · Problem 2 — Know Your Agent**
**M S Abhishek · Team: Abhi Solo**

---

## Who this is for

One person: the fraud-ops reviewer at a bank or NBFC who gets the sessions the automatic checks could not settle.

Today that reviewer is handed a bare number, or a login that was already blocked, and has to defend the call — to the customer who was locked out of their own money, and later to an audit team — with nothing behind it.

Know Your Agent is built for that reviewer's one decision: let this session run, step up the checks, or hold it — with the reasons in hand.

## The problem

A bank's website now gets two kinds of visitors. One is a person. The other is software acting on its own — an AI agent or a bot. Some of that software is welcome: a customer's own assistant checking a balance. Some of it is not: a fraud bot testing stolen passwords. Most defences answer this once, at the door, with a captcha or a fingerprint check, and the answer comes with no reasons. When it is wrong, a real customer is locked out of their own money, or a bot is waved in.

A single yes or no at login is not enough. What the reviewer needs is a score that keeps updating through the session, and that can show its reasons when someone asks why.

## The one thing it does

Score one live session, and show the work for every point.

A small scoring service runs beside a demo banking app and watches each session as it happens. It keeps a running Agent Trust Score, from 0 to 100. One rule governs the whole design: no point moves without a receipt. Every change to the score is stored as a short sentence plus the raw evidence behind it, so the reviewer can open the session later and see exactly why the score went up or down.

## How it works

Four groups of signals feed the score. They watch behaviour only. Nothing typed is stored, and nobody is identified.

**Timing.** People pause, re-read, and change pace. Scripts tend to act at near-constant gaps. I measure the spread of gaps between actions, not only the average speed.

**Path shape.** A person often checks a statement, goes back, then pays a bill. An agent tends to take the shortest path to its goal, the same way every time. I record the order of screens visited and compare the shape of the session against both patterns.

**Input cadence.** A pasted password, fields filled in exact tab order, no corrections, no pause before a large transfer — each is a weak sign on its own. Together they say more.

**Session context.** Device, network, time of day, and whether this session behaves like this account's own earlier sessions.

Each signal is a plain rule or a light statistical check. Each one returns points and a sentence: "Fourteen actions in nine seconds, gaps within three milliseconds of each other: −12." Fixed, published weights add the points up. There is no black box. Anyone can replay a recorded session and get the same score.

The score ends in one of three bands. High trust: the session runs. Low trust: step up the checks. The middle band is the honest one — the system says it is not sure, holds high-risk actions, and hands the session to a human reviewer with the evidence list attached. It never blocks silently on a guess.

## What is different here

Bot detection usually hands over a bare number from a model nobody can question. Here the score is an argument instead. Every point is earned by a named signal. The weights are public. A wrong signal can be pointed at, fixed, and re-tested. A bank's audit team will ask for the reasons first, so the receipt is not an extra feature — it is the product, and it is what the reviewer actually reads.

## How it will be tested — KYA TraceSet

The test is a fixed, named set of recorded session traces, all made on this project's own demo banking app. It has three parts:

- **Human traces.** Volunteers using the app normally, plus scripted messy traces with pauses, typos, and backtracking.
- **Agent traces.** Browser-automation tools and agent frameworks driving the same app.
- **A red-team round.** The agents are tuned to copy human timing, to see which signals still hold.

Four numbers will be reported, as measured, with failures named rather than averaged away:

1. How many human sessions are wrongly flagged. This is the number that matters most, so it is reported first.
2. How many agent sessions are caught.
3. How early in the session the score settles.
4. Whether reviewers shown only the evidence list reach the same call as the score.

Honest status: the trace set does not exist yet. No trace has been recorded and no session has been scored, so no number from this list is claimed here. Whatever the first real run gives is what this README and the final pitch will report. Once the trace set exists, this README will also carry one full worked example — a real recorded session, its score, and every receipt sentence. Until then, that example is not written.

## Data, stated plainly

I have no access to real bank data, and I will not invent results. Every trace used in this project is generated on its own demo banking app and labelled as demo data. Nothing here is presented as a real bank's sessions. No Perfios product, API, or dataset is used or claimed. If the finals provide one, it will be integrated for real and shown working, or not mentioned at all.

## Responsible design

No biometrics: no face, voice, or fingerprint, and no attempt to identify who the person is. The system stores timings and screen paths, not what was typed. A false alarm costs a real person access to their own money, so the design treats a wrong "bot" call as the worst error it can make. Uncertain sessions go to a human, and a flagged customer gets a review and appeal route, not a dead end. The score dies with the session; it is not kept to profile anyone.

## Status and what happens next

This README is the Round 1 submission. The plan from here: if this entry is shortlisted (results on 4 Dec 2026), I build the demo banking app and the KYA TraceSet, so the final pitch at IIT Bombay (15–18 Dec 2026) quotes measured numbers instead of promises. At the final, the score has to be live, not narrated: the judges' own session on the demo app gets scored in front of them, with the receipt sentences appearing as they act.

The 30-second version of that demo: a scripted agent session and a human session run side by side on the demo app. The agent's score falls point by point, each drop carrying its receipt sentence. The human's messy, pausing session stays trusted. Then the reviewer opens the middle-band queue and reads exactly why.
