# Spectrum Connect

**Clear answers for parents about autism treatments, checked the way scientists check them.**

> This is a public overview of the project. The engine's source code is private.
> This page explains what I built, how it works, and what it has done so far.

## The problem

Parents of autistic children hear about a new treatment almost every week: a supplement, a
therapy, a diet. Some help. Some do nothing. Some cause harm. The honest answer to
*"does this actually work?"* is buried in hundreds of research papers that disagree with each
other. Most families do not have time to read them, and many doctors do not either.

## What I built

An engine that does the work a team of research reviewers would do. It finds every paper on one
question, checks how good each paper is, and writes a short report a parent can read. A person
must sign off before any report goes out.

The trust does not come from "the AI is smart." It comes from being able to check every step.
The parent sees a simple answer. Anyone who doubts it can follow every claim back to the paper
it came from.

## How it works

![How one question becomes a report: 12 steps with yes/no gates](docs/how-it-works.svg)

One question walks through **12 steps**. At each **gate** (the dark diamonds) a fixed yes/no
rule decides whether the work goes on. A "no" stops the topic or hands it to a person.

**Why gates?** A weak step should never quietly pass bad work to the next one. A gate makes each
step prove it finished properly before anything builds on it.

- **Every step, what it does and why:** [docs/methods.md](docs/methods.md)
- **The 12 big design choices, the reasons, and what they cost:** [docs/design-decisions.md](docs/design-decisions.md)

## Three rules the engine never breaks

1. **The AI states facts. Code makes the decisions.** The AI reads a paper and reports what it
   says, with a quote. Plain rules, written in code, turn those facts into scores.
   *Why:* an AI can answer the same question two ways on two days. A rule in code cannot. Scores
   must be repeatable, or nobody can check them.
2. **Every claim has a source.** Each fact in a report has a footnote that points to the paper
   it came from, with the exact number. If a fact cannot be traced, it is dropped.
   *Why:* AI can make things up. Refusing anything without a source is the safest guard, and a
   doubter can check every line.
3. **A person signs before anything goes out.** The engine cannot publish without a human
   sign-off. It also checks that the page it publishes is the exact page the person signed.
   *Why:* in medicine, a person takes responsibility for a review. The second check stops a quiet
   change slipping in after the signature.

## What it has done so far

The first full run, on **melatonin for sleep in autistic children**, went from search to signed
report on **15 October 2026**.

| | |
|---|---|
| Papers found and tracked | **520** |
| Papers lost along the way | **0** |
| Outcomes rated | **4**, all rated **low certainty** |
| Overall finding | **"Promising but contested"** |
| AI cost for the whole run | **$5.16** |

Our cost target is **$1 to $3** per question, so the engine can cover hundreds of questions
families ask. About $2.83 of that $5.16 came from two kinds of waste that have since been fixed:
$2.14 from one step reading all 115 papers when it needed only a few, and $0.69 from paying twice
after a restart. The lower cost is not proven until the next full run finishes.

**What "low certainty" means here.** It does not mean melatonin fails. In GRADE, "low" means the
true effect may be quite different from what the studies show, so new research could change the
answer.

A second version of the melatonin report, fixing an error in one section, is being prepared for
a second signature.

## What is still missing, said plainly

- **A second independent reviewer.** Today one person signs off, and every report says so on the
  front page. We chose to run real topics now and say so, rather than wait or hide it.
- **Only free, open-access papers are read in full.** The search still covers everything.
  Paywalled papers are counted, listed and named, so the gap stays visible instead of hidden.
- **Reports are not public yet.** They are signed and locked, but not on a website yet.
- **Watching for new evidence after publishing** (Gate E) is designed but not built.

The engine keeps a written list of every known flaw. Showing the flaws is part of what makes the
reports worth trusting.

## Built on standard methods

- **PRISMA**: how to report a review and show where every paper went
- **AMSTAR 2**: how good is a review of studies
- **RoB 2** and **ROBINS-I**: how good is a single study
- **GRADE**: how sure we can be about an answer

We follow these because they are what medical researchers already trust. We did not invent our
own scoring.

## How it is built

The engine has five parts:

| Part | What it does |
|---|---|
| **Source Databank** | Stores every paper once, with a tamper-evident log of every change |
| **Orchestrator** | Walks one question through the 12 steps and records every result |
| **Integrations** | Searches PubMed and ClinicalTrials.gov; pre-fills the study registration (PROSPERO / OSF) |
| **Sign-Off Workbench** | The human gate: checks the reviewer, records a tamper-evident signature |
| **Operator Console** | The web app a reviewer uses, with password and two-factor sign-in |

**Testing:** 3,281 automated tests passed, none failed (measured 17 October 2026).

**Stack:** Python · FastAPI · SQLAlchemy · Postgres / SQLite · Claude (Anthropic) · pytest · Docker

---

*Not medical advice. Want a live walkthrough? Email rkawai1801@gmail.com.*
