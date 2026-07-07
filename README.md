# Spectrum Connect — an AI-executed, human-verified evidence-synthesis platform

> **Case study / portfolio overview.** The production source is private. This repo
> explains *what I built and how* — the architecture, the engineering decisions, and
> the rigor — without the proprietary methodology.

## The problem

Parents of autistic children are pushed into becoming amateur researchers. For any given
intervention — a supplement, a therapy, a diet — the honest answer to *"does this actually
work?"* is buried in conflicting studies, marketing, and jargon. I set out to build a system
that produces **trustworthy, auditable answers at systematic-review quality**, automatically.

## The idea

An **AI-executed pipeline** that runs the same process a team of methodologists would — search,
screen, appraise, synthesize — but with every step **logged, checkable, and gated by a human**
before anything is published. The trust mechanism isn't "the AI is smart"; it's **transparency**:
methodology invisible to the parent, provable to any skeptic who inspects it.

## How it works (architecture)

A **12-stage, 5-gate protocol** built on published evidence standards (PRISMA, AMSTAR 2, GRADE),
implemented as six independent platform layers:

| Layer | What it does |
|---|---|
| **Source Databank** | Deduplicated corpus of every validated source; live Crossref/PubMed resolution; retraction blast-radius; **hash-chained tamper-evident audit** |
| **Orchestrator** | 12-stage / 5-gate state machine; enforces a dual-rating invariant and records full run provenance |
| **Integrations** | Live PubMed search + registry pre-fill (PROSPERO / OSF) |
| **Sign-Off Workbench** | The human gate — independence/COI checks, 21 CFR Part 11-style tamper-evident e-signatures |
| **Operator Console** | FastAPI web app; TOTP 2FA; every action attributed; PRISMA screening funnel visualized |
| **Guideline Auto-Update** | Watches standards bodies for changes that would reopen a completed topic |

### Engineering principles I held to
- **Closed-world citation rule** — every claim must trace to a validated, stored source. No source, no claim. This is the anti-hallucination mechanism.
- **Honest by construction** — anything not yet fully real is *labeled* as provisional in the UI. A check never claims to have run if it didn't.
- **Human holds the final gate** — the AI produces a first pass; a credentialed, independent reviewer signs off. An unmet gate blocks publication by design.
- **Auditable end to end** — a hash-chained event log means the trail can't be silently edited after the fact.

## Selected technical highlights
- **110+ automated tests** across six packages; deterministic, offline-runnable test suites (no live network needed to prove correctness).
- **Deployment-ready**: runs on SQLite for dev and Postgres in production with no code change; Dockerfile + health checks.
- **Security**: password + TOTP two-factor auth, per-tenant data model, per-action attribution.
- **Live data**: real PubMed/Crossref resolution with graceful handling of transient failures (a network blip never corrupts a verdict).

## Stack
`Python` · `FastAPI` · `SQLAlchemy` · `Postgres` / `SQLite` · `Claude / LLM orchestration` · `pytest` · `Docker`

---

*Want a live walkthrough or a look at the real system? Email rkawai1801@gmail.com.*
