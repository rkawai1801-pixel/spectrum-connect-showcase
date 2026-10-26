# The agents, and why each AI is used where it is

The engine uses **two AI systems** and **plain code**, each for the job it does best. This page
explains every agent, which system runs it, why, and how each choice was tested. Most of the choices
below were made by the founder, Ray Kawai, between September and October 2026, and each one was
measured before it was kept.

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
   named person instead of being guessed. An empty answer ("this paper does not say") is allowed.
7. **Nothing points backwards.** Work flows one way through the steps, like water through pipes.
   Computer scientists call this shape a DAG (a directed acyclic graph).

### How this design came about

- **Before:** early versions used one large AI request to do several jobs at once. Its answers were
  hard to check, and it was not clear which part of an answer came from where.
- **21 September 2026.** Ray asked for the whole engine to be redrawn as "a fixed graph of dedicated
  agents, each with one job, one input, one output, one receipt, and one test that can be run on it
  alone." He set the rules for every system in the family: the model answers questions and the code
  decides; every judgement carries the sentence it came from; runs are repeated and disagreements go
  to a person, never to a vote; agents never talk to each other; nothing points backwards.
- **22 September.** Ray took it further for the quality checklists: "each question becomes
  designated agents that its main role is to answer that question. It's similar to the concept of
  DAG." AMSTAR 2, for example, has 16 questions, so it became 16 small agents, and code combines
  their answers using the checklist's own rules.
- **23 September.** It became a standing rule: "Anything that involves AI needs agentic systems, so
  there is structure in how the AI has to work."

*Why this matters to a reviewer:* a small agent with one question can be tested on its own, with a
fixed example and a known right answer. A big AI request that does everything cannot.

---

## The three workers

| Worker | What it is | What it is good at | What we use it for |
|---|---|---|---|
| **Claude** (by Anthropic) | A large language model that reads and writes | Reading long, messy text; finding the right sentence; writing clear prose | Drafting the question, building the search, reading reviews, writing the "In depth" section |
| **Jev** (by typesafe.ai) | A model that only answers multiple-choice questions, each with a confidence score | Fast, cheap yes/no and pick-one answers; it cannot answer outside the given choices | Screening papers, naming study types, the quality checklists, certainty ratings |
| **Code** | Plain rules written in Python | Doing the same thing every time | Every decision, score, gate and status |

### How Jev works, in plain words

You give Jev three things: a piece of text, one question, and a short list of allowed answers. Jev
gives back one of those answers, and a number from 0 to 1 saying how sure it is.

```
Paper text  +  "Is this a systematic review?"  +  [Meets, Does not meet, Not stated]
                                   |
                                   v
                     "Does not meet", sureness 1.0,
                     based on sentence S001 (the title says "A Narrative Review")
```

Because Jev can only pick from the list, it cannot make up an answer that is not allowed. Because it
gives a sureness number, code can hold back any answer that is not sure enough.

### How Jev came into the engine

- **26 September 2026.** Ray found Jev and asked whether it should replace Claude for the quality
  checklists. He asked for proof first: "If Claude is the right fit, we will use it. Don't lie, show
  me the stats if there is one." There were no published numbers for this kind of task yet, so Claude
  stayed for the moment.
- **29 September.** Ray saw how to make the checklists fit Jev: "breaking down the criteria
  questions in appraisal scales into mini components so that it becomes chaining yes or no. It will
  save money." He also set three limits to work within: where to run the AI, how much text Jev can
  take in at once, and where each checklist's own rules allow stopping early.
- **6 October.** With every checklist rebuilt as small Jev questions, Ray removed Claude from the
  checklists: "We will not use Claude anymore. Way too expensive." Papers Jev is unsure about are
  held for a person, never guessed.

### Why two AI systems instead of one

- **Cost.** Jev charges about $0.042 per million tokens it reads (a token is a short piece of a word)
  and nothing for its answers. For the many small multiple-choice questions in checklists, that is far
  cheaper than a large language model.
- **It can only pick from the choices.** That fits checklists, where every question has fixed
  answers.
- **It reports how sure it is,** so code can hold unsure answers for a person.
- **A different system catches different mistakes.** The engine's own settings file says it plainly:
  a second Claude model "shares blind spots" with the first. A different kind of system is a more
  independent second opinion.

### Two tricks that make Jev safer

1. **Ask twice, in a different order.** An independent test found that swapping the order of the
   answer choices changed about **32.5%** of Jev's answers. So every Jev question is
   asked twice, with the choices reversed. If the two disagree, the AI does not decide. In the
   7 October screening test, the two orders disagreed on only **3 of 195** answers.
2. **The lower sureness counts.** When the two orders agree, the lower of the two sureness numbers is
   the one code uses.

### Why we still use Claude for some steps

Jev is weaker at open reading: finding one exact sentence in a long review. **We tested this
directly, three times** (16 and 17 October 2026), on the same 18 melatonin reviews. Each time Jev
settled **0 of 18**. The three tests cost about **37 cents** in total.

The test also showed *why*. Reviews say the same fact in several places: the title, the abstract and
the methods. When Jev had to pick **one** sentence, its sureness split across all the right ones, for
example 0.5 + 0.3 + 0.2. All three were right, but none reached the 0.9 bar. **67 of the 79** held
answers stopped at exactly that step. The team made three fixes, including asking "is this sentence
a yes or a no?" for each sentence, and tested again: still 0 of 18. A third test gave
Jev an easier job (compare, not locate). Two answers matched Claude for the first time, but Jev still
settled 0 of 18 reviews. So Claude keeps that job, and the question is closed until there is new
evidence.

Ray's reaction shows how these choices get made: "There must be a way to use Jev for this. It costs
too much and [is] more inaccurate when I use Haiku [a smaller Claude model]." The answer was to test
again, not to assume. When the tests failed, the result was recorded and Claude stayed.

The same test also caught a real bug: a number reader turned "−35.6 min" into "−35". It was fixed in
the code that both readers share.

---

## Every agent, step by step

### Step 1. Ask the question: six Claude readers

Six readers each pull out one part of the question a person typed: **who** it is for, **what
treatment**, **compared with what**, **what outcome**, **what dose**, and **how long** people were
followed. Each reader reads the question three times.

- **Code checks** that every answer uses words that are really in the person's question.
- **Code checks** that the three reads agree. There is no vote; if they disagree, a person decides.
- **A person** confirms or rewrites each part, ranks the outcomes, and signs.

*Why:* the question controls everything after it. If it is wrong, the whole review is wrong. So the
AI only drafts, and a person signs.

### Step 3. Search: Claude builds the search, code runs it

Claude helps build the search terms from the locked plan. Code runs the search on PubMed and other
medical sources, and on **ClinicalTrials.gov**, the US register of clinical trials.

- **Every paper has a library label.** PubMed tags each paper with its type. Before any AI reads a
  paper, a simple rule reads that label and removes types that are never evidence (case reports,
  editorials, trial plans, preprints, retracted papers). A paper with no label is never removed by
  this rule; it goes on to be read.
  In the 11 October run this removed 126 of 520 papers for free.
- **Trials with no paper are found and shown.** In the same run, the register search found 28
  melatonin trials. 20 of them had no linked paper, for example a completed 2010 trial called
  "Melatonin for Sleep in Children With Autism". Unpublished trials can hide results that did not go
  well, so the audit lists them.
- **Free full text is fetched before screening.** See step 4.

*Why:* searching must be exactly repeatable, so code runs it and records every result.

### Step 4. Screening: the Jev intake agent

For each paper, **one Jev request** reads the **full text** and answers two things: does this paper
fit the question, and what kind of study is it?

- **Full text, not just the abstract.** On 11 October Ray decided the abstract alone was too
  unreliable: "Just looking through the abstract might not tell everything, which makes it
  inaccurate." So the engine fetches a free full copy first.
- **Open access only, for now.** "We just don't have a reliable way to fetch and extract full text
  from non-open access papers." Paywalled papers are counted, listed and disclosed in every report.
- **Asked twice, in reversed order,** as described above.
- **Jev must be at least 95% sure** before its screening answer counts. Papers below the bar go to a
  **Keep / Exclude page** for a person.
- **Every dropped paper keeps its reason:** the rule it failed and the sentence that shows it.

**How the 95% bar was chosen (7 October 2026).** Ray hand-screened 39 melatonin papers. Jev screened
the same 39, in three improved versions, and the results were compared at eight different bars:

| Version 3, at bar | Agreed with Ray | Ray kept, Jev dropped | Sent to a person |
|---|---|---|---|
| 0.50 | 26 of 39 | 7 | 11 |
| 0.90 | 22 of 39 | 7 | 15 |
| 0.95 | 23 of 39 | 6 | 16 |
| 0.99 | 22 of 39 | 6 | 17 |

- At **every** bar, Jev never kept a paper Ray had dropped.
- Two of the six "Ray kept, Jev dropped" papers were narrative reviews. Jev was right under the
  rules: it quoted the title, which says "A Narrative Review".
- Each full test of 39 papers cost about **1.2 cents**.

*Why Jev:* screening is hundreds of small yes/no questions, exactly what Jev is built for. In the
first signed run it cost **$0.62** for **520** papers, with **0** papers lost.

### Step 5. Naming the study type: Jev, with Claude's guess beside it

Jev decides what kind of study each paper is (for example, a randomised trial or a systematic
review). It must be at least 90% sure. **Claude's guess is shown next to Jev's.**

*Why:* the study type decides which quality checklist a paper gets. Ray: "Not all papers have to go
through all appraisal agents, depending on the type of paper." Jev's accuracy at naming types has not
been measured yet, so both answers are shown side by side until it has.

### Step 5. Quality checklists: three Jev agent groups

| Agent group | Used for | How it works |
|---|---|---|
| **AMSTAR 2** | Systematic reviews | One small agent per question (16 questions, 7 of them "critical") |
| **RoB 2** | Randomised trials | One agent per signalling question, each asked twice in reversed order; the lower confidence counts |
| **ROBINS-I** | Studies that did not randomise | The same careful way |

Code turns the answers into the checklist's official rating, using the checklist's published rules.
Any answer below the 90% bar is **held for a person**. Claude is not used here.

*Why:* checklists are multiple-choice by design, so Jev fits. On 12 October Ray chose to keep the
90% bar on real runs, rather than lower it to get more answers through.

### Steps 5 to 7. Reading the reviews: three Claude readers

Each systematic review is read by three separate Claude readers:

1. **Included studies:** "Which studies does this review say it included?" (used in step 6 to find
   overlap)
2. **Review question:** "What question did this review ask?" (used to check it matches ours)
3. **Findings:** "What did it find for each outcome, with its numbers?" (used in step 7)

Each answer carries its sentence, and code checks that sentence is in the review. Each review is
read more than once.

- **"Match on meaning" (28 September 2026).** Reads often said the same fact in different words, for
  example "1 and 10 mg" and "doses between 1 and 10 mg". The rule now counts those as agreeing, and
  only real clashes go to a person. Lists of included studies must still match exactly.
- **Agreeing is not the same as being right.** In one test, all three reads agreed that a review's
  treatment was "sleep disturbances". That is the problem, not the treatment. This is why every answer
  is checked against fixed rules and a person signs at the end, even when the AI agrees with itself.

*Why Claude:* this is open reading of long documents. Jev scored 0 of 18 here.

### Step 8. Certainty (GRADE): the Jev GRADE agent

Jev answers the GRADE questions for each outcome. The questions were copied from the GRADE handbook,
with page numbers. Code applies the GRADE rules Ray set, for example:

- Results that point in opposite directions lower the rating.
- An outcome measured in only one study is not marked down for inconsistency.
- Publication bias lowers the rating by one level only when the review says it is likely, or the
  funnel plot is lopsided.
- A harm is only reported when the result clearly rules out "no effect".

*Why:* same reason as the checklists: fixed questions with fixed answers.

### Step 10. Plain status: code only

A fixed table in code turns certainty and direction into the words a parent sees, such as "promising
but contested". **No AI is used.** The table checks for harm first (see [methods.md](methods.md)).

### Step 11. Writing "In depth": Claude Opus 5.5

One writer, **Claude Opus 5.5 at high effort**, reads only the papers that passed every check. It
writes six short parts: the numbers, the effects, notable data points, how the authors interpret
their results, how strong a conclusion they drew, and where they say research should go next. The
same model writes the plain-language notes in the audit document.

Every sentence must cite numbered sentences from the papers and quote them. **Code drops** any
sentence that:

- cites a paper that did not pass every check,
- cites a sentence that does not exist,
- quotes words that are not in the paper, or
- states a number that is not in the quoted sentence.

A dropped sentence is never repaired, and every drop is recorded with its reason. The status,
certainty and direction are given to the writer as fixed facts; it cannot change them.

*Why Opus 5.5:* this is the only step where the AI writes words a parent reads. Ray's idea (17
October) was to spend on the strongest model only at the very end, "on the papers that made it
through the super ridiculously hard pipeline." By then only a few papers are left, so the strongest
model costs little. It adds an estimated $0.50 to $2 per topic.

### Final checks: the Verifier

Before sign-off, a verifier runs fixed checks across the whole run: every gate has its evidence,
every paper is accounted for, and nothing was skipped without being labelled "skipped".

---

## Where the money goes

### The first signed run (melatonin, 15 October 2026): $5.16

| Who | Job | Cost |
|---|---|---|
| Jev | Certainty ratings (GRADE) | $2.79, of which $2.14 was a since-fixed bug that fed it all 115 papers |
| Claude | Reading reviews | $1.48, of which $0.69 was paying twice after a restart |
| Jev | Screening 520 papers | $0.62 |
| Jev | Review quality (AMSTAR 2) | $0.20 |
| Other | | $0.08 |

### What it shows

- **The careful work is cheap.** Jev screened 520 papers for 62 cents.
- **The waste was in two bugs, and both are fixed:** $2.83 of the $5.16.
- **Money follows the AI that reads long text.** Claude's reading of reviews is the biggest real
  cost. That is why it was tested against Jev three times before being kept.

---

## The Lab: watching the agents work

The operator console includes a live view called **the Lab**, where each agent is drawn as a small
pixel scientist at a desk: *Pico* (the question), *Scout* (search), *Libby* (finding free full text),
*Sift* (screening), *AMSTAR*, *Rob* and *Robin* (the quality checklists), *Grade* (certainty),
*Verity* (final checks), *Prisma* (paper counts) and *Tracer* (anything that went wrong).

**Where it came from.** On 10 October 2026 Ray asked for an "agent manager", like the
Y Combinator-backed tool Orca, to show how the agents are running. He then asked: "Is it possible to
show things live? Like Twitch live? ... agents as human workers in an office space." To make that
possible, the engine now writes one line to a **call log** every time Jev or Claude answers a
question on a paper. The Lab reads that log. A reviewer can see at a glance which step is working,
which is waiting for a person, and what failed.
