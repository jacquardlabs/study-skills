---
name: lecture-milestones
description: >
  Break down lecture slides into study milestones — thematic checkpoints that group
  slide decks by conceptual dependency rather than by file. Use this skill whenever the
  user uploads or references lecture slides/PDFs and asks to "break them down,"
  "create milestones," "plan a study session," "give me a roadmap," "what should I
  focus on," "how should I pace these lectures," or any request to organize lecture
  material into a learning plan. Also trigger when the user says things like "this
  week's lectures" or "prepare me for these slides." Works for any subject — not
  limited to a specific course. This skill is about creating the milestone plan, not
  about teaching the content itself.
---

# Lecture Milestone Breakdown Skill

## Purpose

Turn a set of lecture slide decks into a sequenced list of **study milestones** —
thematic stopping points where the learner should pause, check understanding, and
consolidate before moving on. Milestones group slide parts by conceptual coherence,
not by file boundary.

## Why this matters

Lecture slides are often split into parts for logistical reasons (file size, class
session breaks) that don't match the natural conceptual structure. Some parts are
tiny overviews (2–3 slides), others are algorithmically dense. Treating every file
as an equal-sized unit leads to either rushed or wasteful study sessions. Good
milestones group tightly coupled ideas together and isolate dense topics that need
solo attention.

## Workflow

### Step 1: Inventory the slides

Use `project_knowledge_search` (or read uploaded files directly) to identify:
- The **title of each slide deck part** (use the actual title from the slides, e.g.
  "Introduction to Syntax," not "Lecture 15 Part 1")
- The **key topics** covered in each part
- The **approximate density**: is it a short overview/recap (2–5 slides of setup),
  a conceptual introduction, an algorithmic deep-dive, a catalog of examples, or
  an evaluation/limitations discussion?
- Any **explicit dependencies**: does Part X say "recall from Part Y" or "this
  builds on..."?

Search broadly — use multiple queries to cover all parts. Don't guess content from
titles alone; actually inspect what's in each deck.

### Step 2: Map conceptual dependencies

Before grouping, sketch the dependency structure:
- Which parts introduce **new vocabulary or formalism** that later parts assume?
- Which parts are **motivating** (building intuition) vs. **formalizing** (giving
  the math/algorithm)?
- Which parts are **lightweight connective tissue** (overviews, recaps, transitions)
  that shouldn't stand alone as milestones?
- Which parts are **dense enough to deserve solo focus** (algorithms, proofs,
  complex derivations)?

### Step 3: Form milestones

Group parts into milestones using these principles:

**Merge when:**
- A motivation/intuition part is immediately followed by its formalization
  (splitting them leaves the learner with either vague intuitions or unmotivated
  formalism)
- A part is very short (overview, recap, transition) and doesn't introduce new
  concepts worth stopping for
- Two parts answer the same natural question (e.g., "what is X and how do we
  represent it?")
- A definition part and its immediate application are in consecutive decks

**Split / keep separate when:**
- A part contains a complex algorithm or derivation that needs focused attention
  and hands-on practice (e.g., chart-filling, dynamic programming, training
  procedures)
- The conceptual question shifts meaningfully (e.g., from "how does it work?" to
  "what goes wrong?")
- A part is a catalog of examples/rules that benefits from its own lighter
  checkpoint before moving to harder material

**Typical milestone count:** For a week of lectures (6–10 slide parts), expect
3–6 milestones. Fewer than 3 usually means you're cramming too much into each stop.
More than 7 usually means you're fragmenting unnecessarily.

### Step 4: Write up each milestone

For each milestone, provide:

1. **Milestone title**: A short thematic label (not just "Milestone 3")
2. **Slide deck titles included**: List the actual titles from the slides that
   belong to this milestone
3. **What it covers**: A plain-language summary of the concepts, written for
   someone who hasn't watched/read the material yet. Define key terms on first
   mention. Connect to things the learner likely already knows (prior lectures,
   everyday analogies).
4. **Why these are grouped together**: One or two sentences explaining the
   conceptual dependency that makes these a unit.
5. **Checkpoint — what to verify before moving on**: 2–4 concrete things the
   learner should be able to do or explain after this milestone. Frame these as
   capabilities ("you should be able to...") not abstract knowledge ("understand
   the concept of...").

### Step 5: Add connections

After listing all milestones, briefly note:
- **Prerequisites from earlier lectures** that the learner should have solid
  before starting (e.g., "Viterbi for HMMs will come back in Milestone 4")
- **Key vocabulary list**: A compact glossary of the most important new terms
  across all milestones, so the learner can preview them

## Output format

Use this structure:

```
## Milestone 1: [Thematic Title]
**Covers:** [Slide Deck Title A] + [Slide Deck Title B]

[Plain-language summary of what these cover and why they're grouped]

**Checkpoint — before moving on, you should be able to:**
- [Concrete capability 1]
- [Concrete capability 2]
- [Concrete capability 3]

---

## Milestone 2: [Thematic Title]
...

---

[After all milestones]

## Connections to Prior Material
[Brief notes on what earlier concepts come back]

## Key Vocabulary Preview
[Compact term → short definition list]
```

## Important guidelines

- **Use slide deck titles, not file names or part numbers.** Say "The CKY Parsing
  Algorithm" not "L15 Part 4" or "Lecture15newPart4.pdf." The learner needs to
  match milestones to what they see on screen.
- **Write for someone who hasn't seen the material yet.** The milestone plan is a
  preview/roadmap, not a review. Avoid assuming they already know the content.
- **Don't just echo the slide structure.** The whole point is to reorganize. If
  every milestone maps 1:1 to a slide part, you haven't added value.
- **Be honest about density.** If one milestone is much harder than the others,
  say so and suggest allocating more time to it.
- **Keep it concise.** Each milestone summary should be a focused paragraph or two,
  not a full lecture recap. Save the deep teaching for when the learner actually
  works through the material.
