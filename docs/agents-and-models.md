# The agents, and why each AI is used where it is

The engine uses **two AI systems** and **plain code**, each for the job it does best. This page
explains every agent, which system runs it, and why.

## First, what we mean by "agent"

In many AI products, an "agent" is an AI that decides for itself what to do next. **Ours do not.**

Every agent in Spectrum Connect follows the same strict pattern:

1. **One fixed question.** Each agent is asked exactly one thing, such as "Who were the children in
   this study?" It does not plan or choose what to do next.
2. **It must show its work.** Every answer comes with the exact words from the paper it is based on.
3. **Code checks the work.** Code confirms those words are really in the paper. The AI is never
   asked whether its own quote is real.
4. **Code makes the decision.** The agent supplies facts. A fixed rule turns the facts into a
   decision.
5. **Agents work alone.** No agent sees another agent's answer, so one mistake cannot spread.
6. **"I'm not sure" goes to a person.** If an answer is not confident enough, it is held for a
   named person instead of being guessed.

**Why we built it this way.** Early versions of the engine used one big AI call to do several jobs
at once. Its answers were hard to check, and it was not clear which part of an answer came from
where. Splitting the work into small
agents, each with one question and a quote, makes every step checkable and repeatable. (Ray's rule,
23 September 2026: "Anything that involves AI needs agentic systems, so there is structure in how
the AI has to work.")

---

## The three workers

| Worker | What it is | What it is good at | What we use it for |
|---|---|---|---|
| **Claude** (by Anthropic) | A large language model that reads and writes | Reading long, messy text; finding the right sentence; writing clear prose | Reading reviews, drafting the question, writing the "In depth" section |
| **Jev** (by typesafe.ai) | A model that only answers multiple-choice questions, each with a confidence score | Fast, cheap yes/no and pick-one answers; it cannot answer outside the given choices | Screening papers, naming study types, the quality checklists, certainty ratings |
| **Code** | Plain rules written in Python | Doing the same thing every time | Every decision, score, gate and status |

### Why two AI systems instead of one

- **Cost.** Jev charges about $0.042 per million tokens (word pieces) it reads and nothing for its answers. For
  the many small multiple-choice questions in quality checklists, that is far cheaper than a large
  language model. One independent study (7,977 test items, 19 models) found it about 44 times
  cheaper than the best model it was compared with.
- **It can only pick from the choices.** Jev cannot invent an answer that is not one of the options.
  That fits checklists, where every question has fixed answers.
- **It reports how sure it is.** Every Jev answer comes with a confidence number, so code can hold
  unsure answers for a person.
- **A second, different system catches different mistakes.** Two models from the same company tend
  to share blind spots. A different kind of system gives a more independent second opinion.

### Why we still use Claude for some steps

Jev is weaker at open reading: finding one exact sentence in a long review, or understanding how a
review words its question. **We tested this directly.** We built Jev agents to read review papers and
tested them three times on the same 18 melatonin reviews. Each time Jev settled **0 of 18**. The
three tests cost about **37 cents** in total. So Claude keeps that job, even though it costs more.

---

## Every agent, step by step

### Step 1. Ask the question: six Claude readers

Six readers each pull out one part of the question a person typed: **who** it is for, **what
treatment**, **compared with what**, **what outcome**, **what dose**, and **how long** people were
followed. Each reader runs more than once.

- **Code checks** that every answer uses words that are really in the person's question.
- **Code checks** that the repeated runs agree. There is no vote; if they disagree, a person decides.
- **A person** confirms or rewrites each part, ranks the outcomes, and signs.

*Why:* the question controls everything after it. If it is wrong, the whole review is wrong. So the
AI only drafts, and a person signs.

### Step 3. Search: Claude builds the search, code runs it

Claude helps build the search terms from the locked plan. Code runs the search on PubMed, other
medical sources and ClinicalTrials.gov, and finds free full-text copies.

*Why:* searching must be exactly repeatable, so code runs it and records every result.

### Step 4. Screening: the Jev intake agent

For each paper, **one Jev request** reads the full text and answers two things: does this paper fit
the question, and what kind of study is it?

- **Every question is asked twice, with the answer choices in a different order.** An independent
  test found that swapping the order of choices changed about **32.5%** of Jev's answers. If the two
  orders disagree, the paper is not decided by the AI.
- **Jev must be at least 95% sure** before its screening answer counts. (Ray set this on 7 October
  2026 from a measured test.)
- Papers Jev is unsure about go to a **Keep / Exclude page** for a person.
- Every dropped paper keeps the rule it failed.

*Why:* screening is hundreds of small yes/no questions. That is exactly what Jev is built for, and
it cost **$0.62** for **520** papers in the melatonin run.

### Step 5. Naming the study type: Jev, with Claude's guess beside it

Jev decides what kind of study each paper is (for example, a randomised trial or a systematic
review). It must be at least 90% sure. **Claude's guess is shown next to Jev's** on early runs.

*Why:* the study type decides which quality checklist a paper gets. We have not yet measured how
accurate Jev is at this, so we show both answers side by side until we have.

### Step 5. Quality checklists: three Jev agents

| Agent | Used for | How it works |
|---|---|---|
| **AMSTAR 2 agent** | Systematic reviews | Answers each AMSTAR 2 item, box by box |
| **RoB 2 agent** | Randomised trials | Answers each signalling question; each one is asked twice in reversed order, and the lower confidence counts |
| **ROBINS-I agent** | Studies that did not randomise | Answers each signalling question the same careful way |

Code turns the answers into the checklist's official rating. Any answer below the confidence bar is
**held for a person**. Claude is not used for these checklists.

*Why:* checklists are multiple-choice by design, so Jev fits. Ray decided on 6 October 2026 to stop
using Claude here because it was too expensive for the number of questions involved.

### Step 5-7. Reading the reviews: three Claude readers

Each systematic review is read by three separate Claude readers:

1. **Included studies:** "Which studies does this review say it included?" (used in step 6 to find
   overlap)
2. **Review question:** "What question did this review ask?" (used to check it matches ours)
3. **Findings:** "What did it find for each outcome, with its numbers?" (used in step 7)

Each answer carries its sentence, and code checks that sentence is in the review. An empty answer is
allowed: a review with no list of studies is recorded as having none, not guessed at.

*Why Claude:* this is open reading of long documents. Jev scored 0 of 18 here.

### Step 8. Certainty (GRADE): the Jev GRADE agent

Jev answers the GRADE questions for each outcome. Code applies the GRADE rules Ray set, for example:

- Results that point in opposite directions lower the rating.
- An outcome measured in only one study is not marked down for inconsistency.
- Publication bias lowers the rating by one level only when the review says it is likely, or the
  funnel plot is lopsided.
- A harm is only reported when the result clearly rules out "no effect".

*Why:* same reason as the checklists: fixed questions with fixed answers.

### Step 11. Writing "In depth": Claude Opus 5.5

One writer, **Claude Opus 5.5 at high effort**, reads only the papers that passed every check. It
writes six short parts: the numbers, the effects, notable data points, how the authors interpret
their results, how strong a conclusion they drew, and where they say research should go next.

Every sentence must cite numbered sentences from the papers and quote them. **Code drops** any
sentence that:

- cites a paper that did not pass every check,
- cites a sentence that does not exist,
- quotes words that are not in the paper, or
- states a number that is not in the quoted sentence.

A dropped sentence is never repaired, and every drop is recorded with its reason. The status,
certainty and direction are given to the writer as fixed facts; it cannot change them.

*Why Opus 5.5:* this is the only step where the AI writes words a parent reads. Ray chose the
strongest model for it on 15 October 2026. It adds an estimated $0.50 to $2 per topic.

### Final checks: the Verifier

Before sign-off, a verifier runs fixed checks across the whole run: every gate has its evidence,
every paper is accounted for, and nothing was skipped without being labelled "skipped".

---

## Where the money goes (first melatonin run, $5.16 total)

| Who | Job | Cost |
|---|---|---|
| Jev | Certainty ratings (GRADE) | $2.79, of which $2.14 was a since-fixed bug that fed it all 115 papers |
| Claude | Reading reviews | $1.48, of which $0.69 was paying twice after a restart |
| Jev | Screening 520 papers | $0.62 |
| Jev | Review quality (AMSTAR 2) | $0.20 |
| Other | | $0.08 |

## The Lab: watching the agents work

The operator console includes a live view called **the Lab**, where each agent is drawn as a small
scientist at a desk: *Pico* (the question), *Scout* (search), *Libby* (finding free full text),
*Sift* (screening), *AMSTAR*, *Rob* and *Robin* (the quality checklists), *Grade* (certainty),
*Verity* (final checks), *Prisma* (paper counts) and *Tracer* (anything that went wrong). It lets a
reviewer see at a glance which step is working, which is waiting for a person, and what failed.
