# Decision log

A dated record of the main decisions behind Spectrum Connect, made by the founder, Ray Kawai, during
the build. Each entry says what was decided and why. Smaller engineering choices are not listed.

For the reasoning behind the biggest choices in more depth, see
[design-decisions.md](design-decisions.md).

---

### September 2026: how screening is judged

**12 September. Screening follows the standard for human reviews.** A blind sample of 60 papers is
rated separately by the AI and a person, and every disagreement is settled by a named person.
*Why:* this is what PRISMA and Cochrane ask of human reviewers. The old pass mark in the code had no
written justification.

**14 September. The agreement score never blocks the work.** It is reported with its margin of
error. *Why:* the common cut-off (0.60) is a convention its own authors called arbitrary, and with 60
papers the margin of error crosses it.

**15 September. A weak review is weak evidence, not missing evidence.** A review rated low quality
stays in, and its low rating travels with it. *Why:* throwing it away hides it; keeping it with its
rating shows the reader how much to trust it.

**19 September. Rules for everything a reader sees.**
- Facts only, never advice. *Why:* that choice belongs to the family and their care team.
- Every fact about the evidence has a footnote and the exact number from the paper. *Why:* a rounded
  or uncited number cannot be checked.
- One named person settles disagreements, and "include" and "exclude" buttons look exactly the
  same. *Why:* the page should never quietly nudge the person toward one answer.
- The agreement figure published is the one from *before* discussion. *Why:* after discussion people
  agree almost by definition, so that number means nothing.

**23 September. Every AI step becomes a structured agent.** One fixed question per agent, a quote
for every answer, and code makes the decision. *Why:* structure makes the AI's work checkable and
repeatable.

**27 September. The company will own the engine.** *Why:* the engine is the company's core asset.

### October 2026: models, cost, and the first full run

**6 October. No Claude in the quality checklists.** AMSTAR 2, RoB 2, ROBINS-I and GRADE run on Jev.
Papers Jev is unsure about are held for a person, never guessed. *Why:* Claude was "way too
expensive" for the number of small questions involved.

**7 October. Jev must be 95% sure to screen a paper.** *Why:* set from a measured test.

**11 October. Run real topics now, and say plainly what is missing.**
- Until a second human reviewer joins, the checks that need one are marked "skipped" everywhere,
  never "passed". *Why:* running real topics teaches more than waiting, and honesty about the gap
  keeps the reports trustworthy.
- New search order: search everything, fetch free full text first, then one Jev request per paper,
  then code decides.
- Only free, open-access papers are read in full. Paywalled papers are counted, listed and stated in
  every document. *Why:* reading only the abstract was judged unreliable.
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
- The 90% confidence bar stays on real runs. *Why:* lowering it would let unsure answers through.

**15 October. First full run signed.** Melatonin for sleep in autistic children: 520 papers, 0 lost,
4 outcomes all rated low certainty, "promising but contested", $5.16.

**15 October. Add an "In depth" section, written by Claude Opus 5.5.** It covers the numbers, the
effects, what the authors concluded and how strongly, and where they say research should go. Every
phrase must be traceable to a paper and footnoted; code drops anything it cannot verify. The audit
document must be very detailed but readable by a 5th grader. *Why:* families and experts asked "what
did the studies actually find?", not just a rating.

**15 October. Simplify the operator console.** Show only the latest run, add a "Needs you now" list,
and keep the Lab view. *Why:* the console felt overwhelming to a new reviewer.

**16 to 17 October. Test Jev for reading reviews.** Built Jev agents for the step Claude does, then
tested three times on 18 reviews. Result: 0 of 18 each time, about 37 cents total. Claude keeps the
step. *Why:* a cheaper step is worthless if its answers are wrong.

**18 October. The operator console is reachable only over a private network.** *Why:* the console
controls signed reports; it should never be open to the public internet.
