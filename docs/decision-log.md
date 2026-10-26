# Decision log

A dated record of the main decisions behind Spectrum Connect, made by the founder, Ray Kawai, during
the build. Each entry says what was decided and why. Quotes are Ray's own words from the team's
project notes. Smaller engineering choices are not listed.

For the reasoning behind the biggest choices in more depth, see
[design-decisions.md](design-decisions.md). For the agents, see
[agents-and-models.md](agents-and-models.md).

---

### July and August 2026: the start

**May 2026.** Spectrum Connect is founded by Doug and Ray (see [why.md](why.md)).

**3 July.** The engine's code is started.

**10 August.** The first reviewed change (pull request) is opened. From here on, every change to
the engine goes through a pull request that can be reviewed.

### September 2026: how the engine should think

**10 September. High quality and low cost are both goals, not a trade.** "We need to be able to
produce ultra high quality research grade reports with minimal expenses." Long jobs start running
overnight on the team's own computer, and every stage counts its own spend.

**10 September. Use the library's own labels before paying for AI.** PubMed already labels each
paper's type, and nothing in the engine was reading it. In one test, those labels settled or flagged
71 of 142 papers at zero cost. The rule that came from this removes only the types that are never
evidence, and sends everything unclear on to be read. *Why:* a fixed rule is free, exact and
repeatable.

**12 September. Four words for success: reliable, consistent, scalable, affordable.** Every weekly
progress report is written at a fifth-grade reading level and judged against those four words.

**12 September. Screening follows the standard for human reviews.** A blind sample of 60 papers is
rated separately by the AI and a person, and every disagreement is settled by a named person.
*Why:* this is what PRISMA and Cochrane ask of human reviewers. The old pass mark in the code had no
written justification.

**13 September. Free, open full text.** Ray chose two open-access services (CORE and Unpaywall) to
find free full copies of papers. Both were connected to the engine by 12 October. "Full open access (no limitation to access) is very important."

**14 September. The agreement score never blocks the work.** It is reported with its margin of
error. *Why:* the common cut-off (0.60) is a convention its own authors called arbitrary, and with 60
papers the margin of error crosses it.

**15 September. A weak review is weak evidence, not missing evidence.** A review rated low quality
stays in, and its low rating travels with it. *Why:* throwing it away hides it; keeping it with its
rating shows the reader how much to trust it.

**18 to 20 September. A bigger idea is kept separate.** Ray proposed a library where "each paper
becomes its own brain", so anyone can ask questions of checked papers, and a tool that fact-checks
claims on websites and videos. "I believe this is revolutionary for the autism misinformation
crisis." He decided it is a separate project: "This is not for the research engine." *Why:* the
engine has one job, and it is not finished yet.

**19 September. Rules for everything a reader sees.**
- Facts only, never advice. *Why:* that choice belongs to the family and their care team.
- Every fact about the evidence has a footnote and the exact number from the paper. *Why:* a rounded
  or uncited number cannot be checked.
- One named person settles disagreements, and "include" and "exclude" buttons look exactly the
  same. *Why:* the page should never quietly nudge the person toward one answer.
- The agreement figure published is the one from *before* discussion. *Why:* after discussion people
  agree almost by definition, so that number means nothing.

**21 September. Redraw the engine as a graph of small agents.** "A fixed graph of dedicated agents,
each with one job, one input, one output, one receipt, and one test that can be run on it alone."
Rules for every system in the family: the model answers and the code decides; every judgement
carries its sentence; disagreements go to a person, never to a vote; agents never talk to each other;
nothing points backwards. And: "I do not want a rewrite for its own sake." *Why:* small parts can be
tested one at a time; a big AI request cannot.

**22 September. One agent per checklist question.** "For the AMSTAR scale ... each question becomes
designated agents that its main role is to answer that question. It's similar to the concept of
DAG."

**23 September. Every AI step becomes a structured agent.** "Anything that involves AI needs agentic
systems, so there is structure in how the AI has to work."

**26 September. A second AI system is considered, with proof required.** Ray found Jev, a
multiple-choice model, and asked for evidence before switching: "If Claude is the right fit, we will
use it. Don't lie, show me the stats." *Why:* a cheaper tool is only worth it if it is as good.

**27 September. The company will own the engine.** *Why:* the engine is the company's core asset.
The two co-founders also planned a shared company account for their code, so each can see and
review the other's work.

**28 September. "Match on meaning."** When the AI reads a paper several times, reads that say the
same fact in different words count as agreeing. Real clashes still go to a person. *Why:* in the
test, exact matching sent the questions for two of the three reviews to a person, often over the same
fact worded two ways.

**29 September. Turn every checklist question into a chain of yes/no questions.** "It's about
breaking down the criteria questions in appraisal scales into mini components so that it becomes
chaining yes or no. It will save money." *Why:* yes/no questions are what Jev does best, and the
quality checklists were the most expensive step.

**30 September. The "three walls".** Ray wrote down why the work matters: research is locked, the
unlocked part is hard to read, and checking it means risking hope. "The checking has to happen before
the answer reaches the parent." (See [why.md](why.md).)

### October 2026: models, cost, and the first full run

**6 October. No Claude in the quality checklists.** AMSTAR 2, RoB 2, ROBINS-I and GRADE run on Jev.
"We will not use Claude anymore. Way too expensive." Papers Jev is unsure about are held for a
person, never guessed. Claude keeps the search, the synthesis and the writing.

**6 October. Open-access papers only, starting now.** "Until we get access to funding that allows us
to pay for expensive access to publication journals, we are only going to use open access papers."
*Why:* a paper the engine cannot read in full cannot be checked properly.

**7 October. Jev must be 95% sure to screen a paper.** Set after Ray hand-screened 39 papers and Jev
screened the same 39 at eight different bars, in three versions. At every bar, Jev never kept a
paper Ray had dropped. Each test of 39 papers cost about 1.2 cents.

**7 October. No case reports.** "There's a danger of misinterpretation of saying 'oh if it worked
for this 1 patient, it would work for my son'. I think that's a dangerous mindset." A
parent-friendly page explains every kind of paper that is left out (see
[evidence-we-use.md](evidence-we-use.md)).

**7 October. Who the product is for.** A website first, designed for the web rather than phones,
with a look Ray described as "clinical but also warm", later "powerful and authoritative, but still
clinical". Other teams could later be given instructions to run the engine themselves.

**10 October. Show the agents working, live.** Ray asked for an agent manager "like Y Combinator-backed
Orca", and then: "Is it possible to show things live? Like Twitch live?" The engine now writes one
line per AI answer to a call log, and the console's Lab view draws each agent as a pixel scientist at
work. *Why:* if you cannot see what each agent is doing, you cannot trust or fix it.

**11 October. Run real topics now, and say plainly what is missing.**
- Until a second human reviewer joins, the checks that need one are marked "skipped" everywhere,
  never "passed". *Why:* running real topics teaches more than waiting, and honesty about the gap
  keeps the reports trustworthy.
- Full text before screening. "Just looking through the abstract might not tell everything, which
  makes it inaccurate." "Abstract only is unreliable."
- New search order: search everything, fetch free full text first, then one Jev request per paper,
  then code decides. The separate Jev steps are combined into one intake agent.
- The path for papers a person keeps after an "unsure" answer is built now, even though it waits for
  a reviewer to exist.
- Claude's guess at the study type is shown beside Jev's. *Why:* Jev's accuracy at this has not yet
  been measured.
- The test branch is merged into the main code only after a complete run succeeds. *Why:* the main
  code should only hold what has been proven end to end.

**8 to 12 October. Certainty and sign-off rules.**
- The final sign-off (Gate D) can be signed by Ray alone for now, and the report says "Signed off by
  one person" with no independent second reviewer.
- GRADE rules: results in opposite directions, or large unexplained differences, lower the rating;
  publication bias lowers it one level only if the review says it is likely or the funnel plot is
  lopsided; one-study outcomes are not marked down for inconsistency; a harm is reported only when
  the result clearly rules out "no effect".
- Guidelines and expert consensus statements are background, not evidence. Case-control and
  cross-sectional studies are left out of treatment questions, and the report says why.

**12 October. Keep the 90% bar, and record both answers.** A test showed one checklist (RoB 2)
rarely reached the 90% bar, likely because the lowest sureness along a whole chain of questions was
being counted. Ray chose "Both": keep the 90% bar on real runs, build the fix behind a switch, and
record both results. *Why:* lowering the bar to get more answers through would let unsure answers
in.

**15 October. First full run signed.** Melatonin for sleep in autistic children: 520 papers, 0 lost,
4 outcomes all rated low certainty, "promising but contested", $5.16. Signed by Ray alone, with
that stated on the front page.

**15 October. Add an "In depth" section, written by Claude Opus 5.5.** ("Let's use Opus 5.5.") It
covers the numbers, the effects, what the authors concluded and how strongly, and where they say
research should go. Every phrase must be traceable to a paper and footnoted; code drops anything it
cannot verify. The audit document must be very detailed but readable by a 5th grader. *Why:*
families and experts asked "what did the studies actually find?", not just a rating.

**15 October. Simplify the operator console.** Show only the latest run, add a "Needs you now" list,
and keep the Lab view. *Why:* the console felt overwhelming to a new reviewer.

**16 to 17 October. Test Jev for reading reviews, three times.** "There must be a way to use Jev for
this." Three tests on the same 18 reviews, each improved on the last. Result: 0 of 18 each time,
about 37 cents in total. Claude keeps the step. *Why:* a cheaper step is worthless if its answers are
wrong.

**17 October. Watch the cost of building, too.** One night of building cost about $26 in AI
coding time, separate from the engine's own spend. New rules for build sessions: a fresh session for
each change, compact after each merge, search for lines instead of reading whole files, and check
spend after each change. *Why:* low cost has to be true of the whole company, not only of the
product.

**18 October. The operator console is reachable only over a private network.** *Why:* the console
controls signed reports; it should never be open to the public internet.

**25 October. A public page for reviewers.** This repository was made to explain the engine to
reviewers and funders. The engine's own code stays private.
