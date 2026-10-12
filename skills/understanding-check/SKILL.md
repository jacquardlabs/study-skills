---
name: understanding-check
description: >
  Checks and builds understanding of something the user just studied — a paper,
  topic, chapter, method, or derivation — through targeted questions in chat,
  or through a worked example followed by a practice problem. Use this skill
  when the user says "quiz me," "check my understanding," "test me on this,"
  "am I getting this," "what should I know from this," "walk me through an
  example," "show me how to do this," "give me a problem to try," "walk me
  through the derivation," or "I get the idea but can't do it." This skill runs
  a conversation — it does not produce reading guides (use paper-reader),
  critiques (use paper-critique), or learning plans (use topic-map).
---

# Understanding Check Skill

## Purpose

Find out whether the learner actually understands what they just read, and
close the gaps while the material is fresh. Two modes:

- **Conceptual** (default): 4–6 questions that make the learner do something
  with the idea — apply it, explain it, connect it, predict with it.
- **Procedural**: for an algorithm, derivation, computation, or implementation
  — a fully worked example, then a practice problem the learner solves.

Pick procedural when the learner asks to work through something or says they
understand the idea but can't do it. Otherwise start conceptual, and switch
when a conceptual answer shows the learner can't carry out a procedure.

## Philosophy

Recognition feels like understanding. A learner who nods along to a paper's
method section often can't apply the method to a new case or explain why a
design choice was made. Good checks require production: the learner states,
derives, applies, or predicts, and the gaps show up in what they produce.

The learner does the thinking. When an answer is wrong, a guiding question
teaches more than the correct answer does.

## Workflow

### Step 1: Identify what was studied

Use the conversation first: a paper-reader guide's "3 Things You Should Be
Able to Explain," a topic-map milestone's checkpoints, or the material the
learner just discussed. Otherwise read the material the learner points to, or
search project knowledge if the environment has it. If the scope is unclear,
ask what they just studied.

### Step 2a: Conceptual mode — ask 4–6 questions

Mix these types, with at least one of each of the first three:

1. **Apply to a new case.** A scenario the source didn't use. This tests
   transfer, which is the point.
   *"A team adds a cache in front of a service whose data changes every few
   seconds. Using the consistency trade-off from the paper, what goes wrong
   and what would you change?"*
2. **Explain the why.** Why a design choice was made or a step is needed.
   *"Why does the bootstrap resample with replacement? What would sampling
   without replacement give you?"*
3. **Connect.** Link to something the learner already knows.
   *"How is this paper's attention mechanism related to the alignment model
   in earlier encoder-decoder work?"*
4. **Spot the error.** A short worked argument or result with one subtle
   mistake.
5. **Predict or compare.** What changes under a modified assumption, or how
   two approaches differ.

Present all questions at once, numbered, and let the learner answer in any
order or pick a subset.

### Step 2b: Procedural mode — worked example, then practice

**Worked example.** Design a small but non-trivial instance: small enough to
finish, large enough to hit at least one decision point. Use the source's
notation. Number every step, give the reasoning at each decision point, flag
where people commonly go wrong, and end by checking the answer.

**Practice problem.** Same procedure, different input, slightly harder, with
one twist the worked example didn't have (an edge case, a tie, a step where
the obvious move fails). For implementation topics, the problem can be
"write this in ~20 lines" with a test case to check against.

Compute every numeric answer with code, when a code tool is available, before
presenting it. A worked example with an arithmetic error teaches the error.

### Step 3: Give feedback

- **Correct**: confirm briefly, and add one nuance worth knowing if there is
  one.
- **Partly correct**: name what's right, then point to the exact step or
  claim where the reasoning diverged.
- **Wrong**: ask a guiding question that leads toward the correction. Give the
  full answer only if the learner is still stuck after that.
- **"I don't know"**: explain briefly and mark the topic for review.
- **Stuck on a procedure**: give the smallest hint that unblocks the next
  step, not the step itself.

### Step 4: Summarize

End with what's solid, what has gaps (and where in the source to revisit),
and the next step: another problem, a harder one, or moving on.

## Important guidelines

- **Every question requires thinking.** If a question can be answered by
  looking up a definition, replace it with an application.
- **Use fresh examples.** Change the domain or the numbers from the source's
  own examples, so the learner can't answer from memory of the page.
- **Keep questions short.** Set up the context quickly and ask one clear
  thing.
- **Scale problems to the procedure.** Large enough to exercise the hard step,
  small enough that bookkeeping doesn't swamp the learning.
- **Point to retention tools at the end.** Gaps worth keeping belong in a
  spaced-repetition tool; suggest the learner add them there.
