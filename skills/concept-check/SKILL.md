---
name: concept-check
description: >
  Generate targeted comprehension checks after studying lecture material. Use this
  skill when the user has just finished a lecture, milestone, study session, or
  section of course material and wants to test their understanding. Trigger phrases
  include: "quiz me," "check my understanding," "test me on this," "what should I
  know from this," "concept check," "am I getting this," "review questions," or any
  request for practice questions tied to material they just studied. Also trigger
  when the user finishes a milestone and says something like "okay I watched it" or
  "done with that section." This skill generates questions — it does not teach the
  material itself.
---

# Concept Check Skill

## Purpose

Generate a focused set of comprehension questions after the learner finishes a
study milestone or lecture segment. Questions should test whether the learner
actually internalized the material — not just whether they can recall surface
facts, but whether they can apply, connect, and explain.

## Philosophy

The worst study quizzes are either trivially easy recall ("What does CFG stand
for?") or impossibly vague ("Explain everything about parsing"). Good concept
checks operate in the middle: they require the learner to *do something* with
what they learned — apply a definition to a new example, predict what would
happen if a parameter changed, connect an idea to something from a prior week,
or spot an error in a worked example.

The goal is to surface misunderstandings early, while the material is fresh and
before they've moved on to content that depends on it.

## Workflow

### Step 1: Identify what was just studied

Use `project_knowledge_search` or conversation context to determine:
- What specific material the learner just covered (lecture parts, milestone, topic)
- What concepts and vocabulary were introduced
- What the "checkpoint capabilities" were (if milestones were generated earlier
  in the conversation or are in project knowledge)
- What prior material this builds on (to enable connection questions)

### Step 2: Generate a question set

Produce **5–7 questions** that cover a mix of types. Every question set should
include at least one of each of the first three types:

**Type 1: Apply to a new example (1–2 questions)**
Give a novel example the slides didn't use and ask the learner to apply the
concept. This is the most valuable question type — it tests transfer, not memory.

Example pattern: "Consider the sentence [new sentence]. Draw/describe the parse
tree using the grammar rules from this section. Where is the ambiguity?"

**Type 2: Explain the "why" (1–2 questions)**
Ask the learner to explain *why* something works the way it does, not just *what*
it is. This catches people who memorized a procedure without understanding its
purpose.

Example pattern: "Why does CKY require the grammar to be in Chomsky Normal Form?
What would go wrong if a rule had three symbols on the right-hand side?"

**Type 3: Connect to prior material (1 question)**
Ask the learner to draw a link between the current material and something from
an earlier lecture. This reinforces the course's cumulative structure.

Example pattern: "How is the Viterbi modification to CKY similar to (or different
from) Viterbi decoding for HMMs? What's the analogous structure?"

**Type 4: Spot the error (0–1 questions)**
Present a worked example or statement that contains a subtle mistake and ask
the learner to find and correct it. Good for procedural content.

Example pattern: "A student computed P(τ) = 0.024 for this parse tree. Here's
their work: [show computation with one wrong probability]. Where did they go wrong?"

**Type 5: Predict/compare (0–1 questions)**
Ask what would happen under a change, or ask the learner to compare two
alternatives.

Example pattern: "If we added parent annotation to this PCFG, which of the
independence assumption problems would that fix? Which would remain?"

### Step 3: Present the questions

Present all questions at once so the learner can see the full scope. Number them
clearly. After each question, include a **difficulty tag** in parentheses:
- **(recall)** — can answer from memory of the slides
- **(apply)** — must use the concept on a new input
- **(connect)** — requires linking to other material
- **(analyze)** — must reason about why/how something works

Tell the learner they can answer all at once, pick a few, or work through them
one at a time — whatever suits their energy.

### Step 4: Give feedback on answers

When the learner responds:
- For correct answers: briefly confirm and, if possible, extend with a nuance
  they might not have considered ("Exactly right — and notice that this also
  means...")
- For partially correct answers: acknowledge what's right, then zero in on the
  specific gap. Don't just say "not quite" — point to the exact spot where their
  reasoning diverged.
- For incorrect answers: don't just give the right answer. Ask a guiding
  follow-up question that leads them toward the correction. Only give the full
  answer if they're stuck after the follow-up.
- For "I don't know": that's useful information. Briefly explain the answer, then
  flag this topic as something to revisit. Suggest they re-read the specific
  slides that cover it.

### Step 5: Summarize and flag gaps

After the Q&A, give a brief summary:
- Which concepts seem solid
- Which ones had gaps (with specific pointers to what to review)
- Whether they're ready to move on to the next milestone or should spend more
  time here

## Important guidelines

- **Never ask questions that can be answered by Googling the definition.** Every
  question should require the learner to think, not just retrieve.
- **Use fresh examples.** If the lecture used "I eat sushi with tuna" as its running
  example, don't reuse that exact sentence. Create a new one that tests the same
  concept in a different context.
- **Match the difficulty to the material.** An introductory/intuition milestone
  gets more "apply" and "explain" questions. An algorithmic milestone gets more
  "work through this example" and "spot the error" questions.
- **Don't overwhelm.** 5–7 questions is the target. If the milestone was very
  dense, it's better to do two rounds of 5 questions than one round of 12.
- **Keep questions concise.** A question that takes longer to read than to think
  about is poorly written. Set up the context quickly, then ask one clear thing.
