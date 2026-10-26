# Methods, step by step

This page says what each step does and who does it. It names the published standards the engine
follows. The exact rules inside the code stay private.

**Who does what**

- **AI reads.** The AI reads papers and answers fixed questions about them, quoting the paper.
- **Code decides.** Plain rules in code turn those answers into scores and yes/no decisions.
  The same answers always give the same result.
- **Person.** A named person confirms the question, settles disagreements, and signs at the end.

| Step | What happens | Who | Gate after it |
|---|---|---|---|
| 1. Ask the question | Write down the treatment, who it is for, what it is compared with, and what should change (PICO) | AI drafts, person confirms | Person confirms the question |
| 2. Lock the plan | Register the plan before seeing any results (PROSPERO / OSF) | Person | Person confirms the plan |
| 3. Search | Search PubMed and other medical sources, plus ClinicalTrials.gov | Code | |
| 4. Screen | Decide if each paper fits the question | AI and a person; code applies the result | **A1**: did any paper pass? |
| 5. Check quality | Score reviews with AMSTAR 2, and trials with RoB 2 or ROBINS-I | AI answers each checklist question; code scores | **A2**: is there at least one real systematic review? |
| 6. Find overlap | Check whether reviews re-use the same studies, so nothing is counted twice | Code | **B**: how much do the reviews overlap? |
| 7. Combine | Line up what the reviews found, and where they disagree | AI reads; code compares | **C**: do the strongest reviews agree? |
| 8. Rate certainty | Rate each outcome high, moderate, low or very low (GRADE) | AI answers; code rates | |
| 9. Sign off | A person checks every claim against its source | Person | **D**: does it pass sign-off? |
| 10. Plain status | Turn the rating into words a parent can use | Code | |
| 11. Write reports | A short report for parents, and a full audit document for experts. Every fact has a footnote | AI writes; code drops anything it cannot trace | |
| 12. Publish | Publish only the exact page the person signed | Code | Is this the signed page? |
| Later | **Gate E**: reopen a topic when a new review, a big trial, a retraction, or 12 months passing changes the picture | Planned, not built | |

## Rules we hold to

- **Nothing is thrown away.** A paper that is left out keeps its reason, so anyone can see why.
- **Agreement is reported, not used to hide disagreement.** When the AI and a person disagree,
  a named person settles it, and how often they agreed is reported.
- **Flaws are written down.** Every known defect goes into a written register, with a date.
- **One reviewer is said out loud.** Until a second independent reviewer joins, every report
  says on its front page that only one person signed it.
