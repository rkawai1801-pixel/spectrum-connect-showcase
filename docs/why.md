# Why Spectrum Connect exists

## The short version

Parents of autistic children have to make big choices about treatments, often with no science
training and very little time. The research that could help them exists, but it is hard to get, hard
to read, and often not trustworthy. Spectrum Connect does the careful checking first, then explains
the answer in plain words, with a source behind every fact.

---

## How it started

Spectrum Connect was started in May 2026 by two people who met through an autistic child.

- **Doug** is a parent. His son is autistic. He has 17 years of experience building software.
- **Ray Kawai** worked as a behavior therapist with Doug's son, and as an autism researcher at UC
  Davis.

Doug kept asking Ray questions. What worked for other families? What should he try next? Ray could
not share other families' stories, because those families' privacy had to be protected. Those
questions showed both of them two gaps: parents feel alone after a diagnosis, and they have nowhere
trustworthy to check what they hear. Spectrum Connect was started to fill both gaps. The Alta
California Regional Center told this story on its own site:
[Douglas, Keynan and Ray](https://www.altaregional.org/post/douglas-keynan-and-ray).

The **research engine** described in this repository is the part that fills the second gap. It is
what writes the plain-language research pages.

## What Ray learned while building it

In Ray's own words (from a post he wrote on 30 September 2026):

> When a parent told me about a supplement, a diet, or a therapy that helps autistic kids, my first
> thought used to be, "Why don't they just look it up in the research?"
>
> That question came from my own habit. Back in my UC Davis days as an autism researcher, I'd open
> PubMed, search, and read. I believed that if a study was on PubMed, it was evidence I could trust.
>
> Then I started building a platform that checks the research for parents and caregivers of autistic
> individuals, and I had to grade studies for real. That's when my belief fell apart. PubMed is an
> index, not a stamp of quality. Strong studies and weak ones sit side by side.
>
> If I came in as a researcher and still had to learn this, how could I expect a parent to do it on
> their own?

---

## The problem: three walls between a parent and the evidence

Telling a parent to "do your research" does not work. There are three walls in the way, and none of
them is the parent's fault.

### Wall 1. Getting in

Most research costs money to read. A study of 67 million articles estimated that at least **28%** of
research papers are free to read. The rest sit behind a paywall.
(Piwowar and others, *PeerJ*, 2018, [doi.org/10.7717/peerj.4375](https://doi.org/10.7717/peerj.4375).)

### Wall 2. Reading it

Even the summaries written for the public are hard. A study of **4,360** plain-language summaries from
Cochrane, the best-known review group, found that the typical one needed **14.9 years** of education
to read, and **80%** gave no clear conclusion.
(Banić and others, *BMC Medical Research Methodology*, 2022, 22:240,
[doi.org/10.1186/s12874-022-01721-7](https://doi.org/10.1186/s12874-022-01721-7).)

### Wall 3. Being ready for the answer

Checking a claim means being willing to learn that the thing you hope for does not work. Ray
describes a parent at 11pm who has just found something that might help their child. That is a lot to
risk. It is not a flaw in the parent. It is what hope does to anyone.

### And behind the walls: much of the research is not careful

Getting past the three walls is still not enough, because many published reviews skip the careful
steps. There are well-known rules for doing a review properly (PRISMA, AMSTAR 2, GRADE). One study of
357 recent treatment reviews found only **38% (135 of 357)** wrote down their plan before they
started (*Systematic Reviews*, 2022, 11:191). A review that does not lock its plan first can,
without meaning to, end up finding what it hoped to find.

### Why one child's story is dangerous

When the research is locked, hard and unclear, parents turn to what is free, readable and kind:
other parents' stories. Ray's worry is the thought **"if it worked for this one child, it would work
for mine."** A story about one child cannot show that the treatment caused the change. Children grow
on their own, hard stretches often get better on their own, and many things change at once. That is
why the engine does not use case reports as evidence. The full reasons are in
[evidence-we-use.md](evidence-we-use.md).

---

## What exists today, and what is missing

| | Careful (full methods) | Written for parents | Kept up to date |
|---|---|---|---|
| Cochrane and other expert reviews | Yes | Hard to read (see Wall 2) | Often not |
| Parent websites, groups and social media | Rarely | Yes | Varies |
| New AI research tools | Partly | No, built for researchers | Some |
| **Spectrum Connect** | **Yes, with every step recorded** | **Yes, plus a full audit for experts** | **Planned (Gate E, not built yet)** |

## What Spectrum Connect does differently

1. **The checking happens before the answer reaches the parent.** The engine does the full careful
   method that many published reviews skip: a locked plan, quality checklists for every study, and a
   certainty rating for every answer.
2. **It writes for parents, and proves it to experts.** Every topic produces two documents: a short
   report a parent can read, and a full audit document a scientist can check line by line.
3. **The AI does the reading. Rules and people make the calls.** The AI must quote the paper for
   every answer. Code checks the quote and makes each decision. A named person signs. Nobody is asked
   to "just trust the AI".
4. **It is honest about what is weak.** Low certainty is reported as low. A missing reviewer is
   named on the front page. Harm is checked first (see below).
5. **It is cheap enough to cover many questions.** The target is $1 to $3 of AI time per question,
   so it can cover the hundreds of questions families actually ask.

### Harm is checked first

The engine's final plain-language label is picked by a fixed table in code, not by the AI. That table
checks for harm **before** it checks for benefit. A benefit must reach "moderate" certainty or better
before the engine will call it settled. A harm signal does not have to clear that bar to be shown.
*Why:* telling a family a treatment is safe when it is not costs much more than being cautious.

---

## Why it matters that this technology exists

- **Families can make better choices with clear facts.** A parent who knows a treatment has "low
  certainty" evidence can ask better questions of their child's doctor.
- **It can protect children.** Some treatments sold to families do nothing, and some cause harm.
  Showing the evidence clearly, with sources, helps families avoid both.
- **It saves time for the people who help families.** Doctors, therapists and family support
  workers get a checked summary instead of starting from zero.
- **It shows AI can be used carefully in health.** Every AI answer can be checked, every decision is
  made by a fixed rule or a named person, and nothing is hidden. That is a model for using AI where
  mistakes matter.
- **It can grow.** Big research groups can only review a few questions a year. A careful, low-cost
  engine can cover far more.

## Where this is going

These are plans and ideas, not built products. They are listed so readers can see the direction.

1. **The website first.** The first goal is a public website where families read the finished
   topics. Today, finished reports stay on the team's own computer until that site exists.
2. **People beyond the founders using the engine.** The console is being designed so a new expert
   reviewer can join cold and handle hundreds of papers. A second independent reviewer is the most
   important missing piece (see [design-decisions.md](design-decisions.md)).
3. **Letting other teams run it.** Ray sees licensing the engine as a possible path: first a
   website, then instructions so another team can set the engine up on its own computers, once it
   has fewer bugs.
4. **A library of papers you can ask.** An idea from September 2026: each checked paper becomes its
   own small "expert" that only knows that paper, so a parent can ask a question and get an answer
   tied to real papers. This would be a separate project.
5. **A fact-checking tool.** Another idea: a browser tool that compares a claim on a website or video
   with the checked research.
6. **Clear, accurate short videos.** Ray's view is that social media is full of misinformation but is
   also where parents discover new things. Filling it with checked facts is part of the plan.

## Who is building it

- **Ray Kawai**, co-founder. Background in cell biology at UC Davis, autism research, and work as a
  behavior therapist with autistic children. Leads the research method and the engine.
- **Doug**, co-founder. Parent of an autistic son, with 17 years of experience building software.
  Engineering lead and code reviewer.

Based in Sacramento, California. The company is not yet incorporated.
