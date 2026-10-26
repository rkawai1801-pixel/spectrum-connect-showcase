# Design decisions, and why

This page lists the big choices behind the engine. Each one says what we decided, why, and what
it costs us. Where we measured something, the number is here.

---

## 1. The AI states facts. Code makes the decisions.

**What we decided.** The AI is never asked "is this study good?" It is asked small, fixed
questions, such as "Did the authors say how children were put into groups? Quote the sentence."
Code then turns those answers into a score using the published checklist's own rules.

**Why.**
- **Same input, same answer.** An AI can give two different answers to the same question on two
  different days. A rule in code cannot. Scores have to be repeatable, or nobody can check them.
- **Every answer can be checked.** Each answer comes with a quote from the paper. A reviewer can
  look at the quote and see whether the AI read it right.
- **The rules are not ours to invent.** Checklists like AMSTAR 2, RoB 2 and GRADE already say
  how to combine answers into a rating. Code follows those rules exactly.

**What it costs.** More AI calls, because one big question becomes many small ones. Building and
testing every rule by hand.

---

## 2. Every fact in a report needs a source, with the exact number

**What we decided.** Every fact about the evidence carries a footnote that points to the paper it
came from, with the exact number from that paper. Code checks each number against the paper's
text. If a number cannot be found there, it is dropped.

**Why.**
- **AI can make things up.** The safest guard is to refuse anything that cannot be traced to a
  stored source. No source, no claim.
- **A rounded or uncited number cannot be checked.** The point of the engine is that a doubter can
  check it.

**What it costs.** Some true facts get dropped because the code could not match them to the text.
We accept losing a true fact over printing a false one.

---

## 3. Facts only, never advice

**What we decided.** Reports say what the research found and how sure we can be. They never tell a
family what to do. The "what next" section is a reminder that this is not medical advice and that
changes should go through the child's own care team.

**Why.** We are not the child's doctor. We do not know the child. Our job is to make the evidence
clear, not to make the choice.

---

## 4. A person signs before anything goes out

**What we decided.** Nothing can be published without a named person signing it. Before publishing,
the engine also checks that the page about to go out is exactly the page the person signed.

**Why.** Systematic reviews in medicine are signed by people who take responsibility for them. The
second check stops a quiet change from slipping in between the signature and the page.

**What it costs.** Speed. A topic waits until a person has time.

---

## 5. Say "one reviewer" out loud, until there are two

**What we decided.** The standard calls for a second, independent reviewer. We do not have one yet.
So today one person signs, and every report says on its front page that only one person signed it.

**Why.** We could have waited. Instead we chose to run real topics now and be honest about the one
missing piece, rather than hide it or fake it. The checks that need a second person are marked
"skipped" everywhere, never "passed".

---

## 6. Search everything, read only what is free, and count the rest

**What we decided.** The search covers everything that exists, paid or free. The engine reads in
full only papers that are free to read. Paywalled papers are counted, listed and named in the
report.

**Why.**
- **A paper we cannot read does not stop existing.** If we searched only free papers, the missing
  ones would be invisible. Counting them keeps the gap visible.
- **The checklist we use on others would mark us down.** AMSTAR 2 treats a full search as a
  critical item. A free-only search would fail our own test.

**What it costs.** Some results rest on fewer papers than exist. The report says how many were left
unread.

---

## 7. Lock the plan before looking at results

**What we decided.** The question, the outcomes that count, and the rules for which papers get in
are written down and registered (PROSPERO / OSF style) before any results are seen.

**Why.** If you can change the question after seeing the answers, you can make the answers say
anything. Locking the plan first is standard practice in medical research for that reason.

---

## 8. Report how often readers agree. Do not use it as a pass mark.

**What we decided.** When the AI and a person screen the same papers, we measure how often they
agree (a score called kappa) and publish it, with its margin of error. We do not block the work if
the score is low. Instead, every disagreement is settled by a named person.

**Why.**
- **The usual cut-off is a convention, not a rule.** The common 0.60 line comes from a 1977 paper
  whose authors called their own bands arbitrary.
- **The margin of error is too wide to judge.** With 60 papers, a score of 0.59 has a 95% range of
  0.38 to 0.80. That range crosses the line, so the line cannot decide anything.
- **This matches how human reviews work.** The standard practice is two readers, with every
  disagreement settled. Agreement is reported, not used as a gate.
- **We publish the number from before discussion.** After two readers talk, they agree almost by
  definition, so that number tells you nothing.

**What we measured.** Giving the screeners more to read made agreement go down, not up:

| What the screeners saw | Agreement (kappa) |
|---|---|
| Title only | 0.57 |
| Title and abstract | 0.51 |
| Title, abstract and the written rules | 0.46 |

The lesson was that agreement is mostly about the rules and the sample, not about feeding the AI
more text.

---

## 9. Nothing is thrown away

**What we decided.** A paper that is left out is never deleted. It keeps its reason and stays in
the record.

**Why.** "Where did that paper go?" should always have an answer. In the first melatonin run,
520 papers were tracked from search to report and 0 were lost.

---

## 10. Keep a written list of our own flaws

**What we decided.** Every known defect goes into a dated register, even ones we have not fixed.

**Why.** A system that claims to be honest about evidence has to be honest about itself. The list
is part of what makes the reports worth trusting.

---

## 11. Measure before you trust, and stop when the answer is no

**What we decided.** Before handing a step to a cheaper AI model, we test it on real papers and
compare it with the current one.

**What we measured.** We tried a cheaper model for reading review papers three times on the same
18 reviews. It matched the current reader on **0 of 18** each time. The three tests cost about
**37 cents** in total. We kept the stronger model for that step, even though it costs more.

**Why.** A cheaper run is worthless if its answers are wrong. A short, cheap test is the right way
to find that out.

---

## 12. Keep the cost low enough to cover many questions

**Target:** $1 to $3 of AI cost per question.

**What we measured.** The first full melatonin run cost **$5.16**. Most of the extra was waste we
found and fixed afterwards: about **$2.14** came from one step reading all 115 papers when it only
needed a few, and about **$0.69** came from paying twice for work after a restart. We have not yet
proven the lower cost on a finished run.

**What we use to save money.** Re-using repeated instructions between AI calls (prompt caching),
and, for the quality-check step, slower half-price batch calls where waiting is fine. Searching is nearly free; reading full
papers is where the cost is.
